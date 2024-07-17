---
title: "JavaScript로 요소가 div인지 확인하는 방법"
description: ""
coverImage: "/assets/img/2024-07-12-HowtoCheckifanElementisaDivwithJavaScript_0.png"
date: 2024-07-12 18:45
ogImage:
  url: /assets/img/2024-07-12-HowtoCheckifanElementisaDivwithJavaScript_0.png
tag: Tech
originalTitle: "How to Check if an Element is a Div with JavaScript?"
link: "https://medium.com/javascript-in-plain-english/how-to-check-if-an-element-is-a-div-with-javascript-821533d4c576"
---

가끔은 JavaScript를 사용하여 요소가 div인지 확인하고 싶을 때가 있습니다.

이 문서에서는 JavaScript를 사용하여 요소가 div인지 확인하는 방법을 살펴보겠습니다.

# JavaScript로 요소가 div인지 확인하기

<div class="content-ad"></div>

JavaScript로 요소가 div인지 확인하려면 요소의 tagName 속성을 가져올 수 있습니다.

예를 들어, 다음과 같은 HTML이 있다고 가정해봅시다:

```js
<div>
  foo
</div>
<p>
  bar
</p>

그럼 우리는 다음과 같이 작성할 수 있습니다:

<div class="content-ad"></div>

for (const el of document.querySelectorAll('*')) {
  if (el.tagName.toLowerCase() === "div") {
    // 'div' 태그입니다
    console.log(el)
  } else {
    // 'div' 태그가 아닙니다
  }
}

HTML의 모든 요소를 document.querySelectorAll로 선택하고 선택된 요소를 for-of 루프로 반복합니다.

그런 다음 루프 내에서 각 요소의 태그 이름을 tagName 속성으로 가져옵니다.

태그 이름을 소문자로 변환하기 위해 toLowerCase를 호출합니다.

<div class="content-ad"></div>

그럼 el이 'div'와 비교하여 div인지 확인할 수 있어요.

만약 div이면, 콘솔에 로그를 남겨요.

콘솔 로그에서 div를 볼 수 있을 거에요.

# 결론

<div class="content-ad"></div>

자바스크립트로 요소가 div인지 확인하려면 요소의 tagName 속성을 가져올 수 있습니다.

# 간단한 영어로 🚀

In Plain English 커뮤니티의 일원이 되어 주셔서 감사합니다! 떠나시기 전에:

- 작가를 클랩하고 팔로우해주세요 👏️️
- 팔로우하기: X | LinkedIn | YouTube | Discord | 뉴스레터
- 다른 플랫폼에서도 방문해주세요: CoFeed | Differ
- 추가 콘텐츠: PlainEnglish.io
```
