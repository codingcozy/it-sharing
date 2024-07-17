---
title: "5분 만에 Nodejs 서버 구축하기 빠르고 쉽게 서버 설정하는 방법"
description: ""
coverImage: "/assets/img/2024-07-09-BuildaNodejsServerin5MinutesQuickandEasyServerSetup_0.png"
date: 2024-07-09 16:55
ogImage:
  url: /assets/img/2024-07-09-BuildaNodejsServerin5MinutesQuickandEasyServerSetup_0.png
tag: Tech
originalTitle: "Build a Node.js Server in 5 Minutes: Quick and Easy Server Setup"
link: "https://medium.com/@dhwajgupta27/build-a-node-js-server-in-5-minutes-quick-and-easy-server-setup-6eb594e8b26"
---

서버를 만드는 초보자 가이드.

![서버 이미지](/assets/img/2024-07-09-BuildaNodejsServerin5MinutesQuickandEasyServerSetup_0.png)

# 단계 1: Node.js 환경 설정

가장 먼저, 컴퓨터에 Node.js가 설치되어 있는지 확인하세요. 공식 Node.js 웹사이트(https://nodejs.org)에서 최신 버전을 다운로드할 수 있습니다. 설치가 완료되면 터미널이나 명령 프롬프트를 열고 다음을 입력하여 설치를 확인하세요:

<div class="content-ad"></div>

```js
node - v;
```

버전 번호가 표시되면, 잘 진행 중이십니다!

# 단계 2: 프로젝트 초기화

새 프로젝트 디렉토리를 만들고 터미널을 사용하여 해당 디렉토리로 이동하세요. 그런 다음, 다음 명령을 실행하여 새로운 Node.js 프로젝트를 초기화하세요:

<div class="content-ad"></div>

```js
npm init -y
```

이 명령어는 기본 설정으로 새로운 package.json 파일을 생성합니다.

# 단계 3: Express 모듈 설치

이제 Express 모듈을 설치해 보겠습니다. Express 모듈을 설치하면 서버 생성이 매우 간편해집니다. 터미널에서 다음 명령어를 실행해주세요:

<div class="content-ad"></div>

```js
npm install express
```

이 명령어를 사용하면 Express 모듈이 프로젝트에 다운로드되고 설치됩니다.

# 단계 4: 서버 파일 생성

프로젝트 디렉토리에 server.mjs라는 새 파일을 만듭니다. 텍스트 편집기에서 열고 코딩을 시작해보세요!

<div class="content-ad"></div>

server.mjs 파일에 다음 코드 줄을 추가해주세요:

```js
import express from "express";

const app = express();
const port = 3000;

app.get("/", (req, res) => {
  res.send("Welcome to my server!");
});

app.listen(port, () => {
  console.log(`Server is running on port ${port}`);
});
```

위 코드에서는 Express 모듈을 import하고, Express 애플리케이션의 인스턴스를 생성하며, 포트를 3000으로 설정하고, 루트 URL인 ("/")의 라우트를 정의하여 환영 메시지를 보내며, 마지막으로 서버를 시작합니다.

# 단계 5: 서버 시작하기

<div class="content-ad"></div>

터미널에서 프로젝트 디렉토리로 이동한 후 다음 명령어를 실행해주세요:

```js
node server.mjs
```

축하해요! 서버가 실행되었어요. 웹 브라우저를 열고 http://localhost:3000를 방문해보세요.

다음 블로그에서 만나요! 더 많은 라우트를 추가하고 다양한 HTTP 메서드를 처리하며 데이터베이스나 외부 API와 통합하여 서버를 확장해봐요. 건배해요!
