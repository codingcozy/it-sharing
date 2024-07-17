---
title: "샤딩으로 대용량 파일 업로드 마스터하는 방법"
description: ""
coverImage: "/assets/img/2024-07-13-HowtoMasterLargeFileUploadwithSharding_0.png"
date: 2024-07-13 18:32
ogImage:
  url: /assets/img/2024-07-13-HowtoMasterLargeFileUploadwithSharding_0.png
tag: Tech
originalTitle: "How to Master Large File Upload with Sharding"
link: "https://medium.com/@awwwesssooooome/how-to-master-large-file-upload-with-sharding-9e74de2123ec"
---

![Large File Upload](/assets/img/2024-07-13-HowtoMasterLargeFileUploadwithSharding_0.png)

최신 애플리케이션에 대한 수요가 증가함에 따라 대용량 파일을 업로드하는 것이 더 흔해졌습니다. 대용량 파일의 단일 업로드는 네트워크 불안정성 및 브라우저 제한과 같은 문제가 발생할 수 있어, 파일을 덩어리로 나누어 업로드하는 것이 효과적인 해결책이 됩니다. 이 글에서는 대용량 파일 덩어리 업로드의 구현 과정 및 이를 처리하는 서버로서 Koa.js를 사용하는 방법에 대해 자세히 소개하겠습니다.

# 1. 소개

대용량 파일을 업로드하는 동안 다음과 같은 문제가 발생할 수 있습니다:

<div class="content-ad"></div>

- 업로드 중단을 일으키는 네트워크 불안정성
- 긴 업로드 시간에 이르는 과도한 파일 크기
- 브라우저 파일 크기 제한

파일을 청크(작은 조각)으로 분할하면 각 청크를 독립적으로 업로드할 수 있습니다. 업로드가 중단되더라도 마지막으로 작업한 곳부터 다시 시작할 수 있어서 업로드의 신뢰성과 효율성을 향상시킬 수 있습니다.

# 2. 클라이언트 측 구현

## 파일 청크화

<div class="content-ad"></div>

먼저 파일을 특정 크기로 나누어 여러 작은 조각으로 분할해야 합니다. 여기서는 조각 크기를 10MB로 선택했습니다.

```js
function fileToChunks(file, chunkSize) {
  const chunks = [];
  for (let i = 0; i < file.size; i += chunkSize) {
    chunks.push(file.slice(i, i + chunkSize));
  }
  return chunks;
}

// fileToChunks 함수는 파일을 각각 10MB 크기의 여러 작은 조각으로 나눕니다.
// 각 부분을 추출하고 chunks 배열에 저장하기 위해 slice 메서드를 사용합니다.
```

## 해시 계산

각 조각의 무결성을 보장하기 위해 각 조각에 대한 해시를 계산해야 합니다. 여기서는 SparkMD5 라이브러리를 사용하여 해시 값을 일괄로 계산하는 방법을 최적화했습니다.

<div class="content-ad"></div>

```js
async function getChunksHashes(chunks) {
  const spark = new SparkMD5.ArrayBuffer();
  const hashes = [];

  for (const chunk of chunks) {
    const hash = await new Promise((resolve) => {
      const reader = new FileReader();
      reader.onload = (event) => {
        spark.append(event.target.result);
        resolve(spark.end());
      };
      reader.readAsArrayBuffer(chunk);
    });
    hashes.push(hash);
  }

  return hashes;
}
```

## 이력서 업로드

로컬 스토리지를 사용하여 업로드된 청크 정보를 기록하여 이어받을 수 있는 업로드를 지원합니다.

```js
async function uploadFile(file, url) {
  const chunkSize = 10 * 1024 * 1024; // 10 MB
  const chunks = fileToChunks(file, chunkSize); // 파일 청킹
  const chunkHashes = await getChunksHashes(chunks); // 해시 계산
  const uploadedChunks = new Set(JSON.parse(localStorage.getItem(file.name)) || []); // 이어받기

  for (let i = 0; i < chunks.length; i++) {
    if (uploadedChunks.has(i)) {
      console.log(`청크 ${i}는 이미 업로드되었습니다`);
      continue;
    }

    const formData = new FormData();
    formData.append("chunk", chunks[i]);
    formData.append("hash", chunkHashes[i]);
    formData.append("index", i);
    formData.append("filename", file.name);
    let retryCount = 0;

    while (retryCount < 5) {
      // 에러 처리 및 재시도 메커니즘
      try {
        const response = await fetch(url, {
          method: "POST",
          body: formData,
        });
        if (response.ok) {
          uploadedChunks.add(i);
          localStorage.setItem(file.name, JSON.stringify([...uploadedChunks]));
          break;
        } else {
          retryCount++;
        }
      } catch (error) {
        retryCount++;
        if (retryCount === 5) {
          throw new Error(`5번 시도 후 청크 ${i} 업로드에 실패했습니다`);
        }
      }
    }
  }

  console.log("모든 청크가 성공적으로 업로드되었습니다");
}
```

<div class="content-ad"></div>

# 3. 서버 측 구현

서버 측에서는 Koa.js를 사용하여 청크 업로드와 병합을 처리할 것입니다.

## 청크 수신

```js
const Koa = require("koa");
const Router = require("koa-router");
const koaBody = require("koa-body");
const fs = require("fs-extra");
const path = require("path");

const app = new Koa();
const router = new Router();

router.post("/upload", koaBody({ multipart: true }), async (ctx) => {
  const { index, filename } = ctx.request.body;
  const file = ctx.request.files.chunk;
  const chunkPath = file.path;
  const uploadDir = path.join(__dirname, "uploads", filename);

  try {
    await fs.ensureDir(uploadDir); // 업로드 디렉터리가 있는지 확인
    await fs.move(chunkPath, path.join(uploadDir, `${index}`), { overwrite: true }); // 청크를 지정된 디렉터리로 이동
    ctx.status = 200;
  } catch (error) {
    ctx.status = 500;
  }
});
```

<div class="content-ad"></div>

## 청크 병합

```js
router.post("/merge", koaBody(), async (ctx) => {
  const { filename, totalChunks } = ctx.request.body;
  const uploadDir = path.join(__dirname, "uploads", filename);
  const filePath = path.join(__dirname, "uploads", `${filename}.merged`);

  try {
    const fileStream = fs.createWriteStream(filePath);
    for (let i = 0; i < totalChunks; i++) {
      const chunkPath = path.join(uploadDir, `${i}`);
      const chunkStream = fs.createReadStream(chunkPath);
      chunkStream.pipe(fileStream, { end: false }); // 청크 병합
      await new Promise((resolve) => chunkStream.on("end", resolve));
    }
    fileStream.end();
    await fs.remove(uploadDir); // 청크 파일 삭제
    ctx.status = 200;
  } catch (error) {
    ctx.status = 500;
  }
});

app.use(router.routes());
app.use(router.allowedMethods());

app.listen(3000, () => {
  console.log("서버가 3000번 포트에서 실행 중입니다");
});
```

이 문서는 대용량 파일 청크 업로드를 구현하는 과정에 대한 자세한 설명을 제공합니다. 파일 청킹, 해시 계산, 이어받기 가능한 업로드, 서버측 청크 수신 및 병합을 다루고 있습니다. 이 접근 방식을 통해 대용량 파일 업로드 시 발생할 수 있는 잠재적 문제를 효과적으로 해결하여 프로세스의 신뢰성과 효율성을 향상시킵니다. 이 문서가 대용량 파일 청크 업로드를 보다 효과적으로 이해하고 구현하는 데 도움이 되기를 바랍니다.

대용량 파일 업로드 프로세스의 신뢰성과 효율성을 보장함으로써, 대용량 파일 업로드 중 발생할 수 있는 다양한 문제를 해결하고, 이러한 접근 방식을 현대적인 애플리케이션 요구에 실용적으로 적용할 수 있습니다.
