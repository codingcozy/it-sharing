---
title: "JavaScript에서 배열 나누기 4가지 방법 비교"
description: ""
coverImage: "/assets/img/2024-07-07-ChunkinganArrayinJavaScriptFourWaysCompared_0.png"
date: 2024-07-07 02:06
ogImage:
  url: /assets/img/2024-07-07-ChunkinganArrayinJavaScriptFourWaysCompared_0.png
tag: Tech
originalTitle: "Chunking an Array in JavaScript: Four Ways Compared"
link: "https://medium.com/@readwanmd/chunking-an-array-in-javascript-four-ways-compared-fe13efa84c8a"
---

```js
<img src="/assets/img/2024-07-07-ChunkinganArrayinJavaScriptFourWaysCompared_0.png" />

배열을 청킹한다는 것은 지정된 크기로 작은 배열로 나누는 것을 의미합니다. 이는 데이터 처리, 페이지네이션 등에 유용합니다. 배열을 청킹하는 네 가지 방법을 살펴보고 성능을 비교해보겠습니다.

먼저, 1부터 10까지의 숫자 배열을 만들어 봅시다:

const arr = Array.from({ length: 10 }, (_, i) => i + 1);

<div class="content-ad"></div>

Array.from()은 1에서 10까지의 요소가 있는 배열을 생성하는 데 사용됩니다.
이제이 배열을 4가지 방법으로 쪼개는 방법을 살펴 보겠습니다.

## 방법 1 : For 루프 사용

function chunkArr(arr, size) {
 let res = [];
 for (let i = 0; i < arr.length; i += size) {
 res.push(arr.slice(i, size + i));
 }
 return res;
}

console.time("for");
console.log(chunkArr(arr, 2));
console.timeEnd("for");

출력:

<div class="content-ad"></div>

[ [ 1, 2 ], [ 3, 4 ], [ 5, 6 ], [ 7, 8 ], [ 9, 10 ] ]
실행 시간: 4.363ms

설명: 이 함수는 지정된 청크 크기의 단계별로 배열을 반복합니다. 각 단계에서 배열을 잘라내고 결과 하위 배열을 결과 배열(res)에 추가합니다. 성능 측정 결과 약 4.363 밀리초가 소요되었습니다.

## 메서드 2: `Array.reduce()` 사용

function chunkArr2(arr, size) {
 if (size <= 0) throw new Error('Chunk size must be a positive integer');
 return arr.reduce((acc, _, i) => {
 if (i % size === 0) acc.push(arr.slice(i, i + size));
 return acc;
 }, []);
}
console.time("reduce");
console.log(chunkArr2(arr, 2));
console.timeEnd("reduce");
```

<div class="content-ad"></div>

결과:

```js
[ [ 1, 2 ], [ 3, 4 ], [ 5, 6 ], [ 7, 8 ], [ 9, 10 ] ]
reduce: 0.069ms
```

설명: 이곳에서는 Array.reduce()를 사용하여 청크 배열을 만듭니다. 현재 인덱스가 청크 크기의 배수인지 확인하고 배열을 이에 따라 슬라이스합니다. 이 방법은 약 0.069밀리초가 걸려 매우 빠릅니다.

## 방법 3: `Array.splice()` 사용하기

<div class="content-ad"></div>

```js
let [list, chunkSize] = [arr, 2];
console.time("slice");
list = [...Array(Math.ceil(list.length / chunkSize))].map((_) => list.slice(0, chunkSize));
console.timeEnd("slice");
console.log(list);
```

Output:

```js
[ [ 1, 2 ], [ 3, 4 ], [ 5, 6 ], [ 7, 8 ], [ 9, 10 ] ]
slice: 0.048ms
```

Explanation: 이 접근 방식은 Array.splice()를 Array.map()과 함께 사용하여 배열을 청크 단위로 나눕니다. 필요한 개수의 청크로 새로운 배열을 만들고 splice()를 사용하여 원래 배열에서 청크를 제거하고 수집합니다. 이 방법은 매우 빠르며 약 0.048 밀리초가 소요됩니다.

<div class="content-ad"></div>

## 방법 4: 재귀적 접근

```js
const chunk = function(array, size) {
 if (!array.length) {
 return [];
 }
 const head = array.slice(0, size);
 const tail = array.slice(size);
return [head, …chunk(tail, size)];
};

console.time('recursive');
console.log(chunk(arr, 2));
console.timeEnd('recursive');
```

결과:

```js
[ [ 1, 2 ], [ 3, 4 ], [ 5, 6 ], [ 7, 8 ], [ 9, 10 ] ]
recursive: 4.372ms
```

<div class="content-ad"></div>

설명: 이 재귀 메서드는 배열을 첫 번째 청크(헤드)와 나머지 요소(테일)로 분할합니다. 그런 다음 테일을 재귀적으로 처리하여 결과를 연결합니다.보다 우아하지만, 이 방법은 reduce 및 splice 방법보다 느리며 약 4.372 밀리초가 걸립니다.

# 결론

네 가지 메서드 모두 지정된 크기의 하위 배열로 배열을 나누는 데 성공했습니다. 그러나 성능은 다음과 같이 상당히 다릅니다:

1. For Loop: 4.363ms
2. Reduce: 0.069ms
3. Splice: 0.048ms
4. Recursive: 4.372ms

<div class="content-ad"></div>

`splice`과 `reduce` 메서드가 가장 빠르기 때문에 성능에 중점을 두는 애플리케이션에 적합합니다. `for` 루프와 재귀적인 방법은 느리기 때문에 대량 데이터에 적합하지 않을 수 있습니다.

자바스크립트 프로젝트에서 배열을 나누는 적합한 방법을 선택할 수 있도록 니즈와 가독성에 따라 선택할 수 있습니다.

# 마지막으로,
