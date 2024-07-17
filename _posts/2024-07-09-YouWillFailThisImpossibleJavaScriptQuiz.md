---
title: "여러분을 좌절시키는 불가능한 자바스크립트 퀴즈에 도전해보세요"
description: ""
coverImage: "/assets/img/2024-07-09-YouWillFailThisImpossibleJavaScriptQuiz_0.png"
date: 2024-07-09 17:13
ogImage:
  url: /assets/img/2024-07-09-YouWillFailThisImpossibleJavaScriptQuiz_0.png
tag: Tech
originalTitle: "You Will Fail This Impossible JavaScript Quiz"
link: "https://medium.com/gitconnected/you-will-fail-this-impossible-javascript-quiz-a62f6418ae1f"
---

<img src="/assets/img/2024-07-09-YouWillFailThisImpossibleJavaScriptQuiz_0.png" />

자바스크립트에는 이상한 특징들이 가득합니다.

하지만 불가능한 자바스크립트 퀴즈를 풀어보고 나니, 제 한계를 느꼈습니다...

이 퀴즈를 풀면서 자바스크립트는 로켓 같은 중요한 시스템에 사용해서는 안 된다는 걸 다시 한 번 깨닫게 되었어요. 자바스크립트는 문제보다는 해결할 문제를 더 많이 만들어내기 때문이죠!

<div class="content-ad"></div>

한 Ex FAANG 개발자도 이 퀴즈에 도전해 보았는데 10문제 중 9문제를 맞추었어요.

그도 깜짝 놀랐던 것은 진정으로 언어가 얼마나 이상하다는 것이었죠.

이 기사에서는 이 퀴즈에서 가장 기이한 3가지 질문을 나누어 드리고, 왜 최고의 JavaScript 개발자조차도 이 질문에 대해 틀릴 수 있는지 알려드릴게요.

그러니 더 이상 말이 필요 없죠... 바로 시작해 봐요!

<div class="content-ad"></div>

# 질문 1

이 코드의 출력물을 알고 계십니까?

```js
console.log(018 - 015);
```

## 제 설명

<div class="content-ad"></div>

첫 번째 이해한 것은 이것들이 십진수가 아니라 8진수인 것이라는 것입니다.

0으로 시작하는 숫자는 8진수를 나타내는 것이고, 이것은 8진수인 기수 8을 의미합니다.

그러나 018은 유효하지 않은 8진수입니다. 왜냐하면 8진수 자릿수는 0부터 7까지만 사용하기 때문입니다.

즉, JavaScript에서는 018을 십진수인 18로 처리합니다.

<div class="content-ad"></div>

자, 이제 015는 올바른 8진수이며 10진수로는 13과 같습니다.

그러므로 18 - 13 = 5. 정말 멋져요 JavaScript.

# 질문 2

이 코드의 출력물이 무엇인지 알고 계신가요?

<div class="content-ad"></div>

```js
const isTrue = true == [];
const isFalse = true == ![];

console.log(isTrue + isFalse);
```

## 나의 설명

우선 isTrue부터 시작해 보겠습니다:

자바스크립트는 true와 [] 값을 각각 숫자로 강제 변환하므로 다음과 같은 표현과 같습니다:

<div class="content-ad"></div>

```js
const isTrue = Number(true) == Number([]);
// Number(true) = 1
// Number([]) = 0
```

하지만 1은 0과 같지 않으므로 isTrue는 false로 평가됩니다.

그리고 isFalse에 대해:

여기서는 강제 변환(coercion)이 일어나지 않습니다. 표현식 양쪽 모두 이미 부울(boolean)입니다.

<div class="content-ad"></div>

그리고 기본적으로 빈 배열 []은 참(truthy)이기 때문에 ![]는 거짓(false)으로 평가됩니다. 따라서 다음 표현식이 생성됩니다:

```js
const isFalse = true == ![];
// true == false --> false.
```

마지막으로 console.log 문에서 isTrue와 isFalse가 모두 숫자로 강제 변환되어 더해집니다: 0 + 0 = 0.

이상하죠?

<div class="content-ad"></div>

# 질문 3

이 코드의 출력이 무엇인지 알고 계시나요?

```js
const numbers = [33, 2, 8];
numbers.sort();
console.log(numbers[1]);
```

## 제 설명

<div class="content-ad"></div>

내 첫 번째 답변은 실제로 8이었어요.

sort() 메소드가 정수를 오름차순으로 정렬할 거라고 생각했어요.

하지만 JavaScript에는 몇 가지 방해요소가 있어요. 😡

알아야 할 첫 번째 사항은 JavaScript가 동적으로 타입 지정된 언어임을 의미합니다. 이는 sort()와 같은 표준 메소드가 모든 타입에 대해 작동해야 한다는 것을 의미합니다.

<div class="content-ad"></div>

다시 말해, JavaScript는 배열 내의 모든 숫자를 문자열로 강제 변환합니다. 왜냐하면 JavaScript에서는 모든 것을 문자열로 변환할 수 있기 때문이죠!

결과적으로, 각 문자열의 첫 번째 문자를 비교하는 사전 순서대로 정렬이 수행됩니다.

```js
const numbers = [33, 2, 8];
// JavaScript가 보는 대로의 배열: ["33", "2", "8"];

// 각 문자열의 첫 번째 문자를 비교합니다.
numbers.sort();

// 최종 출력
console.log(numbers[1]); // => 33, "2" < "3" < "8" 때문에
```

# 마지막으로 생각해보기

<div class="content-ad"></div>

이 기사를 읽고 정신이 아직 굳지 않았으면 좋을텐데요. 때로는 자바스크립트는 정말 복잡하게 느껴질 수 있어요.

이 퀴즈는 자바스크립트와 같은 언어에 여전히 많은 문제가 있는 것을 증명합니다. 수십 년이 지난 후에도요.

큰 규모의 프로젝트에 착수하기 전에 이러한 문제를 이해하고, 최상의 앱을 만들기 위한 올바른 결정을 내리는 것이 중요하다고 생각해요.

# 참고문헌

<div class="content-ad"></div>

- ExFAANG가 불가능한 JS 퀴즈에 도전합니다.
- JavaScript 퀴즈
