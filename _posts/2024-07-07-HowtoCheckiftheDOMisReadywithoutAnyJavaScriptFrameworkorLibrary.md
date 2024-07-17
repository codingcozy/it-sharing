---
title: "JavaScript 프레임워크나 라이브러리 없이 DOM이 준비되었는지 확인하는 방법"
description: ""
coverImage: "/assets/img/2024-07-07-HowtoCheckiftheDOMisReadywithoutAnyJavaScriptFrameworkorLibrary_0.png"
date: 2024-07-07 12:24
ogImage:
  url: /assets/img/2024-07-07-HowtoCheckiftheDOMisReadywithoutAnyJavaScriptFrameworkorLibrary_0.png
tag: Tech
originalTitle: "How to Check if the DOM is Ready without Any JavaScript Framework or Library?"
link: "https://medium.com/@hohanga/how-to-check-if-the-dom-is-ready-without-any-javascript-framework-or-library-c02d95535756"
---

가끔은 JavaScript 프레임워크나 라이브러리 없이 DOM이 준비되었는지 확인하고 싶을 때가 있습니다.

이 글에서는 JavaScript 프레임워크나 라이브러리 없이 DOM이 준비되었는지 확인하는 방법을 살펴보겠습니다.

# JavaScript 프레임워크나 라이브러리 없이 DOM이 준비되었는지 확인하기

<div class="content-ad"></div>

자바스크립트 프레임워크나 라이브러리 없이 DOM이 준비되었는지 확인하려면 DOMContentLoaded 또는 load 이벤트를 청취할 수 있습니다.

각 이벤트에 대한 이벤트 핸들러 내부에서 document.readyState 속성의 값을 확인하여 DOM이 준비되었는지 여부를 결정할 수 있습니다.

예를 들어 다음과 같이 작성할 수 있습니다:

```js
window.addEventListener("DOMContentLoaded", () => {
  if (document.readyState === "complete") {
    console.log("로드되었습니다");
  } else if (document.readyState === "interactive") {
    // DOM 준비 상태! 이미지, 프레임 및 기타 하위 리소스는 아직 다운로드 중입니다.
  }
});

window.addEventListener("load", () => {
  if (document.readyState === "complete") {
    console.log("로드되었습니다");
  } else if (document.readyState === "interactive") {
    // DOM 준비 상태! 이미지, 프레임 및 기타 하위 리소스는 아직 다운로드 중입니다.
  }
});
```
