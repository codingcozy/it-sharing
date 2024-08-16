---
title: "자바스크립트 프로미스 이해하기"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-07-29 13:50
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Understanding JavaScript Promises"
link: "https://dev.to/rahulvijayvergiya/understanding-javascript-promises-29j4"
isUpdated: true
updatedAt: 1723816435881
---



자바스크립트 프로미스는 비동기 작업을 처리하는 강력한 방법입니다. 프로미스가 도입되기 전에는 콜백이 비동기 작업을 처리하는 주요 방법이었습니다. 그러나 콜백은 중첩된 콜백으로 인해 관리하고 읽기 어려운 "콜백 지옥"으로 이어질 수 있습니다. 프로미스는 비동기 작업을 더 깔끔하고 읽기 쉽게 처리하는 방법을 제공합니다.

## 프로미스란 무엇인가요?

자바스크립트의 프로미스는 비동기 작업의 최종 완료(또는 실패)와 그 결과값을 나타냅니다. 이는 다음 세 가지 상태 중 하나에 있을 수 있는 객체입니다:

- 대기 중 (Pending): 초기 상태로, 완료되거나 거부되지 않은 상태입니다.
- 이행됨 (Fulfilled): 작업이 성공적으로 완료됨.
- 거부됨 (Rejected): 작업이 실패함.

<div class="content-ad"></div>

## Promise 생성하기

Promise를 생성하려면 Promise 생성자를 사용하십시오. 이 생성자는 두 개의 매개변수(resolve 및 reject)를 가지는 함수를 받습니다. 이 함수들은 Promise의 상태를 제어합니다.

```js
const myPromise = new Promise((resolve, reject) => {
  let success = true; // 작업 시뮬레이션
  if (success) {
    resolve("작업 성공!");
  } else {
    reject("작업 실패.");
  }
});
```

## Promise 사용하기

<div class="content-ad"></div>

약속(Promise)은 결과를 처리하기 위해 then, catch, finally 메서드가 있습니다.

- then: 약속이 이행될 때 호출됩니다.
- catch: 약속이 거부될 때 호출됩니다.
- finally: 약속의 결과와 관계없이 호출됩니다.

```js
myPromise
  .then((message) => {
    console.log(message); // "작업 성공!"
  })
  .catch((error) => {
    console.error(error); // "작업 실패."
  })
  .finally(() => {
    console.log("작업 완료.");
  });
```

## 프라미스(Chaining Promises) 연결하기

<div class="content-ad"></div>

Promise의 주요 장점 중 하나는 연쇄적으로 사용할 수 있다는 것입니다. 여러 개의 `then` 호출을 연달아 사용하여 비동기 작업을 순차적으로 수행할 수 있습니다.

```js
const firstPromise = new Promise((resolve) => {
  setTimeout(() => resolve("첫 번째 Promise 해결됨"), 1000);
});

firstPromise
  .then((result) => {
    console.log(result); // "첫 번째 Promise 해결됨"
    return new Promise((resolve) => {
      setTimeout(() => resolve("두 번째 Promise 해결됨"), 1000);
    });
  })
  .then((result) => {
    console.log(result); // "두 번째 Promise 해결됨"
  })
  .catch((error) => {
    console.error(error);
  });
```

## 여러 개의 Promise 처리하기

JavaScript는 여러 개의 Promise를 동시에 처리하는 유틸리티 메서드를 제공합니다: `Promise.all`, `Promise.race`, `Promise.allSettled`, 그리고 `Promise.any`.

<div class="content-ad"></div>

- Promise.all: 모든 프로미스가 해결될 때까지 기다리거나 거부될 때까지 기다립니다.

```js
const promise1 = Promise.resolve(3);
const promise2 = 42;
const promise3 = new Promise((resolve) => setTimeout(resolve, 100, 'foo'));

Promise.all([promise1, promise2, promise3]).then((values) => {
  console.log(values); // [3, 42, 'foo']
});
```

- Promise.race: 프로미스 중 하나가 해결되거나 거부될 때 즉시 해결하거나 거부합니다.

```js
const promise1 = new Promise((resolve) => setTimeout(resolve, 500, 'one'));
const promise2 = new Promise((resolve) => setTimeout(resolve, 100, 'two'));

Promise.race([promise1, promise2]).then((value) => {
  console.log(value); // "two"
});
```

<div class="content-ad"></div>

- Promise.allSettled: 모든 프로미스가 처리될 때까지 기다립니다 (성공하거나 실패).

```js
const promise1 = Promise.resolve(42);
const promise2 = Promise.reject('error');
const promise3 = new Promise((resolve) => setTimeout(resolve, 100, 'foo'));

Promise.allSettled([promise1, promise2, promise3]).then((results) => {
  results.forEach((result) => console.log(result.status));
  // "fulfilled"
  // "rejected"
  // "fulfilled"
});
```

- Promise.any: 어떤 프로미스가 해결될 때까지 기다립니다.

```js
const promise1 = Promise.reject('error1');
const promise2 = new Promise((resolve) => setTimeout(resolve, 100, 'foo'));

Promise.any([promise1, promise2]).then((value) => {
  console.log(value); // "foo"
}).catch((error) => {
  console.error(error); // AggregateError: All promises were rejected
});
```

<div class="content-ad"></div>

## Promise 사용 시 흔히 범하는 실수들

Promise는 강력한 도구이지만, 혼란과 오류의 원인이 될 수도 있습니다. Promise를 사용할 때 개발자들이 종종 저지르는 일반적인 실수들은 다음과 같습니다:

### 1. then 핸들러에서 Promise를 반환하지 않는 경우

체이닝이 올바르게 작동하려면 then 핸들러 내에서 Promise를 반환해야 합니다. 이를 하지 않으면 체인이 깨질 수 있습니다.

<div class="content-ad"></div>

```js
firstPromise
  .then((result) => {
    console.log(result);
    new Promise((resolve) => setTimeout(resolve, 1000, '두 번째 프로미스가 해결됨')); // 반환(return) 부분이 빠져 있습니다
  })
  .then((result) => {
    console.log(result); // 이 부분은 undefined를 로깅할 것입니다
  });
```

### 2. 에러를 너무 일찍 잡기

체인 중간에 에러를 일찍 잡으면 하류의 then 핸들러들이 해당 에러를 인식하지 못할 수 있습니다.

```js
firstPromise
  .then((result) => {
    throw new Error('문제 발생');
  })
  .catch((error) => {
    console.error(error); // 너무 일찍 잡힘
  })
  .then(() => {
    console.log('여전히 실행됩니다');
  });
```

<div class="content-ad"></div>

위의 상황을 피하려면, 오류 처리를 체인의 끝이나 특정 작업 근처에 놓는 것이 좋습니다.

### 3. 불필요한 프라미스 생성

동기 작업을 불필요하게 프로미스로 감싸는 것은 복잡성을 추가하는 일반적인 실수입니다.

```js
const promise = new Promise((resolve) => {
  resolve('Hello, World!');
});
promise.then((message) => console.log(message));
```

<div class="content-ad"></div>

대신에 값 직접 사용하기:
```js
Promise.resolve('안녕하세요, 세계!').then((message) => console.log(message));
```

### 4. 거절 처리를 잊는 경우

애플리케이션에서 문제를 일으킬 수 있는 처리되지 않은 프로미스 거절을 방지하려면 항상 거절을 처리해야 합니다.

<div class="content-ad"></div>

```js
const promise = new Promise((_, reject) => {
  reject('에러가 발생했습니다');
});
promise.then((message) => console.log(message)); // catch 핸들러가 없습니다
```

항상 catch 블록을 추가하세요:

```js
promise
  .then((message) => console.log(message))
  .catch((error) => console.error(error));
```

### 5. 프로미스와 Async/Await 혼합하기

<div class="content-ad"></div>

약속과 async/await은 모두 비동기 코드를 처리하는 방법입니다. 그러나 두 가지를 혼합해서 사용하면 혼란스러울 수 있습니다.

```js
async function fetchData() {
  const data = await fetch('https://api.example.com/data');
  data.then((result) => console.log(result)); // 불필요한 약속
}
```

한 가지 방식을 골라 사용하세요:

```js
async function fetchData() {
  const data = await fetch('https://api.example.com/data');
  console.log(await data.json());
}
```

<div class="content-ad"></div>

## 결론

프로미스는 현대 JavaScript의 중요한 부분으로, 비동기 작업을 처리하는 강력한 방법을 제공합니다. 프로미스를 만들고 사용하며 관리하는 방법을 이해함으로써 더 깨끗하고 유지보수가 쉬운 코드를 작성할 수 있습니다. 일반적인 실수를 피하면 프로미스를 효과적으로 활용하고 잠재적인 문제를 방지할 수 있습니다. 하나의 비동기 작업을 처리하거나 여러 동시 작업을 조정할 때, 프로미스는 JavaScript 도구상의 강력한 도구입니다.

이 글은 초기에 내 블로그에 게시되었습니다. 아래 링크를 통해 원본 소스를 확인해보세요: