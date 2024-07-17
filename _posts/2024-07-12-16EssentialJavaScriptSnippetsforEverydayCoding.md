---
title: "일상 코딩을 위한 필수 JavaScript 스니펫 16가지 "
description: ""
coverImage: "/assets/img/2024-07-12-16EssentialJavaScriptSnippetsforEverydayCoding_0.png"
date: 2024-07-12 18:34
ogImage:
  url: /assets/img/2024-07-12-16EssentialJavaScriptSnippetsforEverydayCoding_0.png
tag: Tech
originalTitle: "16 Essential JavaScript Snippets for Everyday Coding 🚀"
link: "https://medium.com/javascript-in-plain-english/16-essential-javascript-snippets-for-everyday-coding-f0537e706d11"
---

![JavaScript Code Snippet](/assets/img/2024-07-12-16EssentialJavaScriptSnippetsforEverydayCoding_0.png)

웹 개발의 세계에서는 효율이 중요해요. 🧰 새 프로젝트를 시작할 때마다 똑같은 것을 다시 만들 필요가 있을까요? 이 기사는 Lodash나 day.js와 같은 보편적인 것 이상의 유용한 JavaScript 코드 조각을 모아놓았어요. 코딩 작업을 간소화하기 위한 깔끔한 방법을 제공해요. 바로 시작해 볼까요!

- 요소 외부에서 클릭 감지하기 🖱️
  타겟 요소 외부에 클릭이 발생했는지 확인하기 위한 복잡한 체크에 지쳤나요? 👋 모달 숨기기나 메뉴 축소와 같은 작업을 더 깔끔하게 처리할 contains 메서드를 사용하여 단순화해보세요:

```js
document.addEventListener("click", function (evt) {
  // 'ele' 외부를 클릭한 경우 isClickedOutside는 true입니다.
  const isClickedOutside = !ele.contains(evt.target);
});
```

<div class="content-ad"></div>

2. 빠르게 공식 웹사이트 열기 🚀
   제3자 라이브러리의 문서나 저장소를 확인해야 하는가요? 이 명령어를 사용하면 빠르게 해당 웹사이트로 이동할 수 있어요:

```js
# 홈페이지 열기
   npm home PACKAGE_NAME
   npm home react

# 저장소 열기
   npm repo PACKAGE_NAME
   npm repo react
```

3. 일회성 이벤트 리스너 👂
   한 번만 처리하고 싶은 이벤트의 경우, 리스너를 수동으로 제거하는 대신 한 번만 처리되도록 설정하세요:

```js
const handler = function (e) {};
ele.addEventListener("event-name", handler, { once: true });
```

<div class="content-ad"></div>

4. 초를 HH:mm:ss 형식으로 변환하기 ⏱️
   오디오나 비디오의 지속 시간을 표시할 때, 초를 사용자 친화적인 HH:mm:ss 형식으로 표현해보세요:

```js
const formatSeconds = (s) =>
  [parseInt(s / 60 / 60), parseInt((s / 60) % 60), parseInt(s % 60)].join(":").replace(/\b(\d)\b/g, "0$1");
```

"몇 초 전"이나 "5분 전"과 같은 상대적인 시간 디스플레이를 위해서는 timeago.js 라이브러리를 살펴보세요!

5. URL 매개변수를 객체로 변환하기 🗃️
   query-string은 이를 수행하기 위한 인기 있는 라이브러리이지만, URLSearchParams API를 사용하여 직접 동일한 작업을 수행할 수 있습니다:

<div class="content-ad"></div>

```js
const getUrlParams = (query) =>
  Array.from(new URLSearchParams(query)).reduce(
    (p, [k, v]) => Object.assign({}, p, { [k]: p[k] ? (Array.isArray(p[k]) ? p[k] : [p[k]]).concat(v) : v }),
    {}
  );

// 쿼리 매개변수 얻기
getUrlParams(location.query);
// { a: ['1', '4'], b: '2', c: '3' }
getUrlParams("?a=1&b=2&c=3&a=4");
// 해시 매개변수 얻기
getUrlParams(location.hash.split("?")[1]);
```

6. 새 탭을 안전하게 열기 🔐
   새 탭 열기는 사소한 것처럼 보이지만 보안에 주의를 기울여야 합니다. 외부 링크를 제공할 때에는 window.opener.location을 통해 악성 사이트가 여러분의 사이트로 리디렉션하는 것을 방지하기 위해 noopener noreferrer를 활용하세요. window.open에도 동일한 원칙이 적용됩니다.

```js
<a target="_blank" rel="noopener noreferrer">
  ...
</a>;

// window.open은 rel을 'opener'로 기본 설정하므로 명시적으로 설정해야 합니다
window.open("https://google.com", "google", "noopener,noreferrer");
```

주의: 아래 코드에는 보안 취약점이 있습니다!

<div class="content-ad"></div>

```js
<a target="_blank" rel="opener">
  ...
</a>;

window.opener.location = "http://fake.website.here";
```

7. 업로드된 이미지 미리보기 🖼️
   브라우저에서 FileReader API를 사용하여 사용자가 업로드한 이미지를 직접 미리볼 수 있습니다:

```js
function readImage() {
  const fileReader = new FileReader();
  const file = document.getElementById("uploaded-file").files[0];

  if (file) {
    fileReader.readAsDataURL(file);
  }

  fileReader.addEventListener(
    "load",
    () => {
      const result = fileReader.result;
      const resultContainer = document.getElementById("result");
      const img = document.createElement("img");
      img.src = result;
      resultContainer.append(img);
    },
    { once: true }
  );
}
```

8. 파일 다운로드 📥
   `a` 태그의 `download` 속성을 사용하여 다운로드를 트리거할 수 있습니다. 동일 출처 파일에 대해서 신뢰성 있게 작동하며 IE 및 모바일에서 제한 사항이 있을 수 있습니다:

<div class="content-ad"></div>

```js
<a href="/path/to/file" download>
  다운로드
</a>
```

또는 JavaScript로 임시 `a` 태그를 생성합니다:

```js
function download(url) {
  const link = document.createElement("a");
  link.download = "파일명";
  link.href = "url";

  document.body.appendChild(link);
  link.click();
  document.body.removeChild(link);
}
```

정적 파일의 경우, 서버에서 적절한 Content-Disposition 헤더를 설정하여 다운로드를 트리거할 수도 있습니다:

<div class="content-ad"></div>

```js
Content-Disposition: attachment; filename="filename.jpg"
```

파일 다운로드를 넘어서, Blob 객체와 createObjectURL 메서드를 사용하여 텍스트 또는 JSON 파일을 생성하고 다운로드할 수도 있어요:

```js
const data = JSON.stringify({ 'message': 'Hello Word' });

const blob = new Blob([data], { type: 'application/json' });
// Blob을 위한 URL 생성
const url = window.URL.createObjectURL(blob);
// 'download' 기능을 사용하여 URL 다운로드
...

// URL 객체 해제
window.URL.revokeObjectURL(url);
```

9. 함수 결과 메모이제 🧠
   계산 비용이 많이 드는 함수의 결과를 캐싱하여 저장하세요.

<div class="content-ad"></div>

```js
const memoize = (fn) =>
  (
    (cache = Object.create(null)) =>
    (arg) =>
      cache[arg] || (cache[arg] = fn(arg))
  )();
```

10. Multi-Line Ellipsis …
    CSS를 사용하여 텍스트 내용을 생략부호로 자를 때, 단일 라인 또는 다중 라인 모두 사용할 수 있습니다:

```css
.truncate {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.truncate-multi {
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 2;
  overflow: hidden;
}
```

11. CSS로 마지막 N개 요소 선택하기 🎯
    CSS 선택자를 사용하여 목록에서 마지막 몇 개 요소를 대상으로합니다.

<div class="content-ad"></div>

```js
// 첫 세 개 아이템
   li:nth-child(-n + 3) {
     text-decoration: underline;
   }

// 2부터 5번 아이템
   li:nth-child(n + 2):nth-child(-n + 5) {
     color: #2563eb;
   }
   // 마지막 두 개 아이템
   li:nth-last-child(-n + 2) {
     text-decoration-line: line-through;
   }
```

<img src="/assets/img/2024-07-12-16EssentialJavaScriptSnippetsforEverydayCoding_1.png" />

12. 스크롤바 스타일 맞춤 설정 🎨
    스크롤바의 모양과 느낌을 개선하세요:

```js
::-webkit-scrollbar {
   width: 8px;
   height: 8px;
}

::-webkit-scrollbar-track {
   border-radius: 10px;
   background-color: #fafafa;
}

::-webkit-scrollbar-thumb {
   border-radius: 10px;
   background: rgb(191, 191, 191);
}
/* 현대적인 스크롤바 API */
body {
   scrollbar-width: thin;
   scrollbar-color: #718096 #edf2f7;
}
```

<div class="content-ad"></div>

스크롤 막대 스타일링은 better-scroll과 같은 라이브러리를 사용하여 더 고급스러운 사용자정의가 가능합니다.

13. 최대 잔여량 방법을 사용한 정밀한 백분율 계산 📊
    백분율을 다룰 때 반올림은 때로 100%가 되지 않는 총합을 만들어낼 수 있습니다. 정확한 분배를 위해 최대 잔여량 방법을 활용하세요:

```js
// 결과:  ['32.56%', '6.97%', '27.91%', '32.56%']
getPercentWithPrecision([56, 12, 48, 56], 2);

function getPercentWithPrecision(valueList, precision) {
  const digits = Math.pow(10, precision);
  const sum = valueList.reduce((total, cur) => total + cur, 0);

  const votesPerQuota = valueList.map((val) => {
    return (val / sum) * 100 * digits;
  });
  const seats = votesPerQuota.map((val) => {
    return Math.floor(val);
  });
  const remainder = votesPerQuota.map((val) => {
    return val - Math.floor(val);
  });

  let totalSeats = 100 * digits;
  let currentSeats = votesPerQuota.reduce((total, cur) => total + Math.floor(cur), 0);

  while (totalSeats - currentSeats > 0) {
    let maxIdx = -1;
    let maxValue = Number.NEGATIVE_INFINITY;
    for (let i = 0; i < remainder.length; i++) {
      if (maxValue < remainder[i]) {
        maxValue = remainder[i];
        maxIdx = i;
      }
    }

    seats[maxIdx]++;
    remainder[maxIdx] = 0;
    currentSeats++;
  }

  return seats.map((val) => `${(val / totalSeats) * 100}%`);
}
```

14. 동시 요청 제한 🚦
    대량의 요청을 다룰 때 서버가 과부하가 걸리지 않도록 동시성을 관리하세요.

<div class="content-ad"></div>

```js
비동기풀 (poolLimit, iterable, iteratorFn) {
    const 결과 = [];
    const 실행중 = new Set();
    for (const 요소 of iterable) {
        const p = Promise.resolve().then(() => iteratorFn(요소, iterable));
        결과.push(p);
        실행중.add(p);
        const 정리 = () => 실행중.delete(p);
        p.then(정리).catch(정리);
        if (실행중.size >= poolLimit) {
            await Promise.race(실행중);
        }
    }
    return Promise.all(결과);
}

// 예시 사용법
const 타임아웃 = i => new Promise(해결 => setTimeout(() => 해결(i), i));
비동기풀(2, [1000, 5000, 3000, 2000], 타임아웃).then(결과 => {
    console.log(결과)
})
```

15. UUID 생성하기 🆔
    유니버설 유니크 식별자를 생성합니다:

```js
const uuid = (a) =>
  a
    ? (a ^ ((Math.random() * 16) >> (a / 4))).toString(16)
    : ([1e7] + -1e3 + -4e3 + -8e3 + -1e11).replace(/[018]/g, uuid);
```

16. 모달이 열릴 때 본문 스크롤 비활성화하기 🚫📜
    모달 대화 상자가 열릴 때 배경 콘텐츠 스크롤을 방지합니다:

<div class="content-ad"></div>

```js
// 본문 스크롤 비활성화
document.body.style.overflow = "hidden";

// 스크롤 재활성화
document.body.style.removeProperty("overflow");
```

제가 제공한 최적화된 코드 조각이 독자들에게 유용하면 좋겠네요. 다른 질문이 있거나 더 개선이 필요한 부분이 있다면 언제든 말씀해주세요! 😊

# 쉽게 설명하기 🚀

In Plain English 커뮤니티에 함께 해주셔서 감사합니다! 떠나시기 전에:

<div class="content-ad"></div>

- 작가를 칭찬하고 팔로우하기를 잊지 마세요! 👏️️
- 팔로우하기: X | LinkedIn | YouTube | Discord | 뉴스레터
- 다른 플랫폼 방문하기: CoFeed | Differ
- 더 많은 콘텐츠: PlainEnglish.io
