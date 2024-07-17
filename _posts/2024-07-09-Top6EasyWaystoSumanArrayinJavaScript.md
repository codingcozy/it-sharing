---
title: "자바스크립트에서 배열을 합산하는 6가지 쉬운 방법"
description: ""
coverImage: "/assets/img/2024-07-09-Top6EasyWaystoSumanArrayinJavaScript_0.png"
date: 2024-07-09 16:32
ogImage:
  url: /assets/img/2024-07-09-Top6EasyWaystoSumanArrayinJavaScript_0.png
tag: Tech
originalTitle: "Top 6 Easy Ways to Sum an Array in JavaScript"
link: "https://medium.com/@onlinemsr/top-6-easy-ways-to-sum-an-array-in-javascript-41491c305fd2"
---

<img src="/assets/img/2024-07-09-Top6EasyWaystoSumanArrayinJavaScript_0.png" />

자바스크립트에서 배열의 합을 계산하는 방법을 배우고 싶나요? 웹 개발자이신 경우, 배열은 자바스크립트에서 가장 흔하고 유용한 데이터 구조 중 하나이기 때문에 이 문제를 자주 마주치게 될 것입니다. 그러나 다른 몇몇 프로그래밍 언어와는 달리, 자바스크립트에는 숫자 배열을 합하는 내장 메서드가 없습니다. 그렇다면 자바스크립트에서 배열의 기본 산술 연산을 어떻게 수행할 수 있을까요?

이 블로그 포스트에서 다음을 안내해 드리겠습니다:

- reduce() 메서드를 사용하여 자바스크립트 배열의 합 계산
- for-loop를 이용해 자바스크립트에서 배열 합 구하기
- for…of 루프로 배열의 합 계산
- forEach() 메서드를 이용하여 배열 합 구하기
- map() 및 reduce() 메서드를 사용하여 배열 합 구하기
- 배열의 합을 찾기 위해 재귀 사용
- 배열 합을 구하기 위한 각 메서드의 장단점 평가
- 배열 합 구하는 방법의 성능 벤치마킹 평가

<div class="content-ad"></div>

이 접근 방식의 성능 및 가독성 비교를 보여드리겠습니다. 이 블로그 포스트 끝에는 JavaScript에서 배열을 전문가처럼 더하는 방법을 익힐 수 있을 거예요. 시작해봅시다!

## 1. reduce() 메서드를 사용한 JavaScript 배열 합계 구하기

JavaScript에서 배열을 더하는 간단한 방법은 reduce() 메서드를 사용하는 것입니다.

이 방법은 콜백 함수와 초기값을 인수로 받아 배열의 각 요소에 콜백 함수를 적용하여 결과를 단일 값으로 누적합니다.

<div class="content-ad"></div>

콜백 함수는 두 개의 매개변수를 사용합니다: 누적기(accumulator)와 현재 요소(current element)입니다. 누적기는 각 반복 후 반환된 값이며, 현재 요소는 처리 중인 값입니다. 초기 값은 누적기의 시작점이며, 선택적입니다.

![array sum](/assets/img/2024-07-09-Top6EasyWaystoSumanArrayinJavaScript_1.png)

초기 값을 생략하면 배열의 첫 번째 요소가 누적기로 사용되고, 두 번째 요소가 현재 요소로 사용됩니다.

## JavaScript 배열 합계 값

<div class="content-ad"></div>

JavaScript의 배열 메소드 reduce()를 사용하여 숫자 배열의 합을 구할 수 있습니다. 아래와 같이 작성할 수 있어요:

```js
// 숫자 배열
let numbers = [1, 2, 3, 4, 5];

// reduce()를 사용하여 배열의 합 구하기
let sum = numbers.reduce((accumulator, current) => accumulator + current);

// 결과 출력
console.log(sum);
// 결과: 15
```

위 코드에서 숫자 배열이 정의되고 있어요. reduce() 메소드를 사용하여 배열의 모든 요소를 합하는 방법을 보여주고 있습니다. 결과는 sum 변수에 저장돼요.

<div class="content-ad"></div>

## 자바스크립트 배열 속성의 합

예를 들어, 객체 배열에서 각 객체의 특정 속성에 접근하여 합을 구할 수 있습니다.

```js
// 책 객체 배열
let books = [
  { name: "JavaScript 가이드", price: 5 },
  { name: "JS 참고서", price: 10 },
  { name: "JS 최종 안내서", price: 20 },
];

// reduce() 함수를 사용하여 각 객체의 price 속성을 합산
let total = books.reduce((accumulator, current) => accumulator + current.price, 0);

// 결과 출력
console.log(total);
// 출력: 35
```

이 자바스크립트 코드는 이름과 가격을 가진 책 객체 배열을 정의합니다. 그런 다음 reduce() 함수를 사용하여 모든 책의 총 가격을 계산하고 결과를 콘솔에 출력합니다. 결과는 35입니다.

<div class="content-ad"></div>

이 경우에는 누산기에 0의 초기값을 제공해야 합니다. 그렇지 않으면 배열의 첫 번째 요소가 누산기로 사용되어 결과가 잘못될 수 있습니다.

reduce() 메서드는 JavaScript에서 배열을 합산하는 가장 인기 있는 방법 중 하나이며 간결하고 표현력이 풍부하며 기능적입니다. 그러나 초심자에게는 가장 직관적이거나 익숙한 방식이 아닐 수 있으며 다른 방법과 비교했을 때 성능 단점이 있을 수 있습니다.

# 2. for문을 사용한 JavaScript 배열 합산

JavaScript에서 배열을 합산하는 또 다른 일반적인 방법은 for문을 사용하는 것입니다. 이것은 배열을 반복하는 기본적이고 명령적인 방법으로, 카운터 변수, 조건 및 증가 표현식을 사용합니다. 루프 내에서는 각 배열 요소에 대해 색인을 사용하여 접근하고 합을 보유하는 변수에 추가할 수 있습니다. 예를 들어:

<div class="content-ad"></div>

```js
// 숫자 배열
let numbers = [1, 2, 3, 4, 5];

// 합계를 저장할 변수 초기화
let sum = 0;

// for 루프를 사용하여 배열 반복
for (let index = 0; index < numbers.length; index++) {
  // 현재 요소를 합계에 더하기
  sum += numbers[index];
}

// 결과 출력
console.log(sum);
// 출력: 15
```

for 루프는 배열을 처리하는 고전적이고 익숙한 방법으로, 반복 프로세스를 완전히 제어할 수 있습니다. 객체 배열의 총합을 계산하는 데도 특정 객체의 속성에 접근하여 for 루프를 사용할 수 있습니다.

```js
// 책 객체 배열
let books = [
  { name: "JavaScript 가이드", price: 5 },
  { name: "JS 참조서", price: 10 },
  { name: "JS 최종 가이드", price: 20 },
];

// 합계를 저장할 변수 초기화
let total = 0;

// for 루프를 사용하여 배열 반복
for (let i = 0; i < books.length; i++) {
  // 현재 객체의 price 속성을 총합에 더하기
  total += books[i].price;
}

// 결과 출력
console.log(total);
// 출력: 60
```

for 루프는 JavaScript에서 배열을 합하는 간단한 방법이며, 다른 방법보다 성능상의 이점을 가질 수 있습니다. 그러나 다른 방법보다 더 번잡하고 오류가 발생하기 쉽며 가독성이 떨어질 수 있습니다.

<div class="content-ad"></div>

자바스크립트에서 배열의 합을 구하는 다양한 방법이 있습니다. 다양한 방법과 기술을 사용할 수 있습니다. 그 중 일부는 아래 섹션에 나와 있습니다.

## 3. for...of 루프를 사용한 배열의 합

이것은 카운터 변수나 인덱스를 사용하지 않고 배열을 반복하는 현대적이고 간결한 방법입니다. 예를 들어:

```js
// 숫자 배열
let numbers = [1, 2, 3, 4, 5];

// 합계를 저장할 변수를 초기화
let sum = 0;

// for...of 루프를 사용하여 배열을 반복
for (let number of numbers) {
  // 현재 요소를 합계에 추가
  sum += number;
}

// 결과 출력
console.log(sum);
// 출력: 15
```

<div class="content-ad"></div>

# 4. forEach() 메서드를 사용하여 배열의 합을 구하기

forEach()는 배열의 각 요소에 대해 콜백 함수를 실행하는 내장 메서드입니다. 예를 들어:

```js
// 숫자 배열
let numbers = [1, 2, 3, 4, 5];

// 합을 저장할 변수 초기화
let sum = 0;

// forEach() 메서드를 사용하여 배열을 순회
numbers.forEach((number) => {
  // 현재 요소를 합에 추가
  sum += number;
});

// 결과 출력
console.log(sum);
// 출력: 15
```

# 5. map() 및 reduce() 메서드를 사용한 배열의 합

<div class="content-ad"></div>

map()과 reduce()는 두 가지 내장 메서드로, 배열을 변환하고 집계하는 데 함께 연결할 수 있어요.

map() 메서드는 원래 배열의 각 요소에 콜백 함수를 적용하여 새 배열을 생성해요.

reduce() 메서드는 다른 콜백 함수를 적용하여 새 배열을 단일 값으로 축소시켜요. 예를 들어:

```js
// 숫자 배열
let numbers = [1, 2, 3, 4, 5];

// map()과 reduce()를 사용하여 배열 합계
let sum = numbers.map((number) => number * 2).reduce((accumulator, current) => accumulator + current);

// 결과 출력
console.log(sum);
// 출력: 30
```

<div class="content-ad"></div>

# 6. 배열의 합을 재귀 방법을 사용하여 구하기

이 기술은 기본 사례에 도달할 때까지 함수를 자체적으로 호출합니다. 예를 들어:

```js
// 숫자 배열
let numbers = [1, 2, 3, 4, 5];

// 배열의 합을 계산하는 재귀 함수 정의
function sumArray(array) {
  // 기본 사례: 배열이 비어있으면 0을 반환
  if (array.length === 0) return 0;

  // 재귀 사례: 배열의 첫 번째 요소와 배열의 나머지 요소의 합을 반환
  return array[0] + sumArray(array.slice(1));
}

// 재귀 함수 호출 및 결과 출력
console.log(sumArray(numbers));
// 결과: 15
```

이 JavaScript 코드는 숫자 배열과 배열 요소의 합을 계산하는 sumArray()라는 재귀 함수를 정의합니다. JavaScript의 slice(1)은 인덱스 1부터 시작하는 모든 배열 요소를 반환합니다. 그런 다음 배열 [1, 2, 3, 4, 5]로 함수를 호출하고 결과인 15를 출력합니다.

<div class="content-ad"></div>

자바스크립트에서 배열을 합하는 여러 방법 중에는 다양한 방법이 있습니다. 각 방법마다 장단점이 있어요. 선호도, 사용 사례 및 성능 요구 사항에 따라 가장 적합한 방법을 선택할 수 있어요.

## 각 방법의 장단점

각 방법의 주요 장단점을 요약한 표:

![표](/assets/img/2024-07-09-Top6EasyWaystoSumanArrayinJavaScript_3.png)

<div class="content-ad"></div>

# 성능 벤치마크

나는 JsPerf 도구를 사용하여 이러한 모든 메소드에 대한 성능 벤치마크를 수행했습니다. 모든 메소드에 동일한 입력을 사용했습니다.

```js
let numbers = [1, 2, 3, 4, 5];
```

![이미지](/assets/img/2024-07-09-Top6EasyWaystoSumanArrayinJavaScript_4.png)

<div class="content-ad"></div>

위의 결과에서 for-loop가 다른 방법들과 비교했을 때 더 나은 결과를 보여준다는 것을 알 수 있습니다. 성능이 좋지 않기 때문에 재귀 방법은 다른 옵션이 없을 때에만 사용하십시오.

# 결론

이 블로그 포스트에서는 다양한 방법과 기술을 사용하여 JavaScript에서 배열의 합을 계산하는 방법을 배웠습니다. reduce() 메소드, for loop, for…of loop, forEach() 메소드, map() 및 reduce() 메소드, 그리고 재귀를 사용하는 방법에 대해 살펴보았습니다. 또한 각 접근 방식의 성능과 가독성을 비교하고 JavaScript에서 배열을 다룰 때 몇 가지 팁과 모범 사례를 제공했습니다.

이 블로그 포스트가 도움이 되고 유익했기를 바랍니다. 궁금한 사항, 댓글 또는 피드백이 있으시면 언제든지 아래에 남겨주시기 바랍니다. 😊

<div class="content-ad"></div>

웹사이트(rajamsr.com) | X (트위터) | LinkedIn
