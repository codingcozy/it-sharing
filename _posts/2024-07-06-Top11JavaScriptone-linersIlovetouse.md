---
title: "제가 자주 사용하는 최고의 11개 자바스크립트 한 줄 코드"
description: ""
coverImage: "/assets/img/2024-07-06-Top11JavaScriptone-linersIlovetouse_0.png"
date: 2024-07-06 09:57
ogImage:
  url: /assets/img/2024-07-06-Top11JavaScriptone-linersIlovetouse_0.png
tag: Tech
originalTitle: "Top 11 JavaScript one-liners I love to use"
link: "https://medium.com/gitconnected/top-11-javascript-one-liners-i-love-to-use-405519e073e8"
---

## 당신도 해봐요

![JavaScript One Liners](/assets/img/2024-07-06-Top11JavaScriptone-linersIlovetouse_0.png)

오늘은 제 프로젝트를 만드는 데 많은 도움이 된 몇 가지 유용하고 간단한 JavaScript 원 라이너를 여러분과 공유하고 싶습니다.

제가 매일 사용하여 작업 프로세스를 간소화하는 데 도움이 되는 상위 10개의 JavaScript 원 라이너를 소개합니다. 이러한 코드 조각들은 간단하면서도 강력하며, 최소한의 노력으로 앱의 성능과 기능을 향상시킵니다.

<div class="content-ad"></div>

더 이상 말하지 않고 시작해 봅시다!

# 1. 배열이 비어 있는지 확인하기

```js
const isEmpty = (arr) => arr.length === 0;
```

배열이 요소를 가지고 있는지 확인합니다.

<div class="content-ad"></div>

# 2. 현재 타임스탬프 가져오기

```js
const timestamp = () => Date.now();
```

1970년 1월 1일부터 경과된 밀리초 수를 반환합니다.

사용 가능한 날짜로 변환하려면 다음 코드를 적용하십시오:

<div class="content-ad"></div>

```js
const myDate = new Date(timestamp).toLocaleString();
```

# 3. 배열 복제하기

```js
const cloneArray = (arr) => [...arr];
```

배열의 얕은 복사본을 생성합니다.

<div class="content-ad"></div>

# 4. 배열에서 falsy 값 제거

```js
const removeFalsy = (arr) => arr.filter(Boolean);
```

`false`, `null`, `0`, `""`, `undefined`, `NaN`과 같은 값을 배열에서 제거합니다.

# 5. 문자열 배열을 대문자로 변환

<div class="content-ad"></div>

```js
const toUpperCaseArray = (arr) => arr.map((s) => s.toUpperCase());
```

배열의 모든 문자열 요소를 대문자로 변환합니다.

# 6. 배열의 모든 숫자 더하기

```js
const sum = (arr) => arr.reduce((a, b) => a + b, 0);
```

<div class="content-ad"></div>

배열에 있는 숫자들의 총 합을 계산하세요.

# 7. 배열에서 무작위 요소 가져오기

```js
const randomElement = (arr) => arr[Math.floor(Math.random() * arr.length)];
```

배열에서 무작위 요소를 반환합니다.

<div class="content-ad"></div>

# 8. 랜덤 16진수 색상 생성하기

```js
const randomHexColor = () =>
  `#${Math.floor(Math.random() * 0xffffff)
    .toString(16)
    .padStart(6, "0")}`;
```

랜덤한 16진수 색상 코드를 생성합니다.

# 9. 함수를 디바운스하기(기본 구현)

<div class="content-ad"></div>

```js
const debounce = (fn, delay) => {
  let timeout;
  return (...args) => {
    clearTimeout(timeout);
    timeout = setTimeout(() => fn(...args), delay);
  };
};
```

함수 실행을 지연시켜 함수가 호출되는 속도를 제한합니다.

# 10. 함수 쓰로틀링 (기본 구현)

```js
const throttle = (fn, limit) => {
  let lastFunc;
  let lastRan;
  return (...args) => {
    if (!lastRan) {
      fn(...args);
      lastRan = Date.now();
    } else {
      clearTimeout(lastFunc);
      lastFunc = setTimeout(() => {
        if (Date.now() - lastRan >= limit) {
          fn(...args);
          lastRan = Date.now();
        }
      }, limit - (Date.now() - lastRan));
    }
  };
};
```

<div class="content-ad"></div>

함수가 특정 시간 간격마다 한 번만 실행되도록 속도를 제한합니다.

# 11. 문자열 내의 HTML 이스케이프

```js
const escapeHTML = (str) =>
  str.replace(/[&<>"']/g, (match) => ({ "&": "&amp;", "<": "&lt;", ">": "&gt;", '"': "&quot;", "'": "&#39;" }[match]));
```

특수 HTML 문자를 이스케이프하여 XSS 공격을 방지하세요.

<div class="content-ad"></div>

이 코드 조각들을 매일의 업무 흐름에 통합함으로써 프로젝트의 보안성과 기능성을 효과적으로 향상시킬 수 있어요.

좋은 코딩 되세요!
