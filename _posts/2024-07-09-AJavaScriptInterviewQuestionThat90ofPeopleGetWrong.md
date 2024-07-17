---
title: "90의 사람들이 틀리는 자바스크립트 인터뷰 질문"
description: ""
coverImage: "/assets/img/2024-07-09-AJavaScriptInterviewQuestionThat90ofPeopleGetWrong_0.png"
date: 2024-07-09 13:13
ogImage:
  url: /assets/img/2024-07-09-AJavaScriptInterviewQuestionThat90ofPeopleGetWrong_0.png
tag: Tech
originalTitle: "A JavaScript Interview Question That 90% of People Get Wrong"
link: "https://medium.com/javascript-in-plain-english/a-javascript-interview-question-that-90-of-people-get-wrong-0f2d59be3d3c"
---

![image](/assets/img/2024-07-09-AJavaScriptInterviewQuestionThat90ofPeopleGetWrong_0.png)

질문을 먼저 살펴보겠습니다:

바로 답을 생각해 보셨나요? 반응하기 전에 잠깐 기다려주세요. 혹시 이미 형 변환에 익숙하신 건가요? 하지만 toString 함수의 길이를 어떻게 계산할까요? 그리고 일반 함수의 길이는 어떻게 계산되는 걸까요?

JavaScript에서 함수의 length 속성은 함수가 선언될 때 함수가 기대하는 인수의 개수를 나타냅니다. rest 파라미터 연산자(...) 뒤의 예상 인수 개수와 기본 매개변수는 포함되지 않습니다. 이 속성은 반영(reflection)과 일부 함수형 프로그래밍 시나리오에서 자주 사용되어 함수가 받을 것으로 예상하는 인수의 개수를 이해하는 데 도움이 됩니다.

<div class="content-ad"></div>

## 예시 설명

1. 매개변수가 없는 함수

```js
function noParams() {}
console.log(noParams.length); // 결과: 0
```

이 함수는 매개변수를 정의하지 않았으므로 length 속성은 0입니다.

<div class="content-ad"></div>

2. 하나의 매개변수를 사용하는 함수

```js
function oneParam(a) {}
console.log(oneParam.length); // 결과: 1
```

이 함수는 하나의 매개변수를 정의했으므로 length 속성은 1입니다.
