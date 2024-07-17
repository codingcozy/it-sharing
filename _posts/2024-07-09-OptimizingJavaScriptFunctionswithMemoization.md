---
title: "메모이제이션으로 JavaScript 함수 최적화하는 방법"
description: ""
coverImage: "/assets/img/2024-07-09-OptimizingJavaScriptFunctionswithMemoization_0.png"
date: 2024-07-09 08:24
ogImage:
  url: /assets/img/2024-07-09-OptimizingJavaScriptFunctionswithMemoization_0.png
tag: Tech
originalTitle: "Optimizing JavaScript Functions with Memoization"
link: "https://medium.com/@mukulmedatwal/optimizing-javascript-functions-with-memoization-036bd035a636"
---

웹 개발에서는 성능이 매우 중요합니다. 복잡한 웹 애플리케이션을 개발하거나 간단한 인터랙티브 웹사이트를 만들더라도 코드를 최적화하면 상당한 성능 향상을 기대할 수 있습니다. 최적화를 위한 강력한 기술 중 하나가 메모이제이션입니다.

# 메모이제이션이란?

메모이제이션은 비싼 함수 호출의 결과를 저장하고, 동일한 입력이 발생했을 때 캐시된 결과를 반환하는 최적화 기술입니다. 이는 계산이 많이 필요한 함수나 재귀 연산과 같이 많은 자원을 필요로 하는 경우에 특히 시간과 계산 자원을 절약할 수 있습니다.

<div class="content-ad"></div>

# 메모이제이션의 장점

- 성능 향상: 중복된 계산을 피함으로써, 메모이제이션은 비싼 또는 재귀적인 작업이 있는 함수의 실행 속도를 크게 향상시킬 수 있습니다.
- 중복 제거: 메모이제이션은 동일한 계산이 여러 번 수행되는 것을 방지하여 CPU 사용량을 줄입니다.
- 효율성 향상: 계산량이 많은 애플리케이션에서, 메모이제이션은 코드 실행을 더 효율적으로 만들어 전체적인 성능을 향상시킵니다.
- 확장성: 메모이제이션은 동일한 매개변수로 자주 호출되는 함수의 성능을 최적화하여 확장 가능한 애플리케이션을 만드는 데 도움을 줍니다.
- 자원 최적화: 결과를 캐싱함으로써, 메모이제이션은 서버와 데이터베이스에 부담을 줄이고 자원 이용을 개선합니다.

# 메모이제이션의 단점

- 증가하는 메모리 사용량: 캐시된 결과를 저장하는 데 메모리가 필요합니다. 캐시가 너무 커지면 상당한 메모리 자원을 소모할 수 있습니다.
- 캐시 무효화: 캐시 무효화를 관리하는 것은 복잡할 수 있습니다. 입력이 변경되거나 유효하지 않아지면 캐시를 지워야 하거나 업데이트해야 합니다.
- 오버헤드: 캐시를 확인하고 결과를 저장하는 것은 오버헤드를 추가합니다. 계산 비용이 적게 드는 함수의 경우, 이 오버헤드가 메모이제이션의 장점을 상쇄시킬 수 있습니다.
- 제한된 사용 사례: 메모이제이션은 순수 함수에 대해서만 효과적입니다 - 동일한 입력에 대해 동일한 출력을 생성하고 부작용이 없는 함수에 대해만 적용 가능합니다.

<div class="content-ad"></div>

# 메모이제이션을 사용하는 경우

- 비용이 많이 드는 계산: 함수가 비용이 많이 드는 또는 복잡한 계산을 수행하고 입력이 동일한 경우.
- 재귀 함수: 피보나치 수열과 같은 재귀 함수에서 동일한 계산을 반복 수행하는 경우.
- 순수 함수: 동일한 입력에 대해 일관된 결과를 생성하고 부작용이 없는 함수의 경우.
- API 호출: 동일한 데이터를 반복해서 가져오는 API 호출이 있는 경우, 메모이제이션을 사용하면 네트워크 요청을 줄일 수 있습니다.

# 메모이제이션을 사용하지 않는 경우

- 비용이 적은 함수: 빠르고 비용이 적게 드는 함수의 경우, 메모이제이션의 부담이 이득을 상쇄할 수 있습니다.
- 비순수 함수: 부작용이 있는 함수나 외부 상태에 의존하는 함수의 경우, 메모이제이션은 적합하지 않습니다.
- 동적 입력: 입력이 자주 또는 예측할 수 없이 변경되는 경우, 메모이제이션은 효과적이지 않을 수 있습니다.
- 메모리 제약: 제한된 메모리 환경에서 결과를 캐싱함으로써 발생하는 증가된 메모리 사용량이 문제가 될 수 있습니다.

<div class="content-ad"></div>

# 자바스크립트에서 메모이제이션 구현하기

자바스크립트에서 메모이제이션을 구현하는 방법에 대해 알아봅시다.

# 메모이제이션 함수

우리는 다른 함수 fn을 인수로 받는 고차 함수 memoize를 생성할 것입니다. 이 새로운 함수는 주어진 인수의 결과가 이미 캐시되어 있는지 확인할 것입니다. 만약 그렇다면 캐시된 결과를 반환하고, 그렇지 않으면 결과를 계산한 후 캐시하고 반환할 것입니다.

<div class="content-ad"></div>

```js
function memoize(fn) {
  const cache = new Map();
  return function (...args) {
    const key = JSON.stringify(args);
    if (cache.has(key)) {
      return cache.get(key);
    }
    const result = fn(...args);
    cache.set(key, result);
    return result;
  };
}
```

# 예시 1: 간단한 함수 메모이제이션

간단한 예시부터 시작해봅시다. 두 숫자를 입력받아 그들의 합을 반환하는 add 함수가 있다고 가정해봅시다.

```js
function add(a, b) {
  return a + b;
}

const memoizedAdd = memoize(add);
console.log(memoizedAdd(2, 3)); // 5 (계산됨)
console.log(memoizedAdd(2, 3)); // 5 (캐시된 값)
```

<div class="content-ad"></div>

이 예제에서 memoizedAdd(2, 3)에 대한 첫 번째 호출은 결과를 계산하고 캐싱합니다. 두 번째 호출은 캐시에서 결과를 검색합니다.

# 예제 2: 재귀 함수의 메모이제이션

메모이제이션은 피보나치 수열과 같은 재귀 함수에 특히 유용합니다. 이러한 경우에는 동일한 계산이 반복적으로 수행되기 때문입니다.

```js
function fibonacci(n) {
  if (n <= 1) {
    return n;
  }
  return fibonacci(n - 1) + fibonacci(n - 2);
}

const memoizedFibonacci = memoize(fibonacci);
console.log(memoizedFibonacci(10)); // 55 (메모이제이션으로 계산됨)
console.log(memoizedFibonacci(10)); // 55 (캐시됨)
console.log(memoizedFibonacci(20)); // 6765 (메모이제이션으로 계산됨)
console.log(memoizedFibonacci(20)); // 6765 (캐시됨)
```

<div class="content-ad"></div>

메모이제이션을 사용하지 않으면, 피보나치 함수의 시간 복잡도는 O(2^n)일 것입니다. 하지만 메모이제이션을 사용하면 O(n)으로 줄어들어 성능이 크게 향상됩니다.

# 예제 3: API 콜 메모이제이션

메모이제이션은 API 콜을 하는 함수에도 적용할 수 있어서 중복되는 네트워크 요청을 줄일 수 있습니다.

```js
async function fetchData(id) {
  const response = await fetch(`https://api.example.com/data/${id}`);
  return response.json();
}

const memoizedFetchData = memoize(fetchData);
memoizedFetchData(1).then((data) => console.log(data)); // API에서 가져옴
memoizedFetchData(1).then((data) => console.log(data)); // 캐시된 결과를 반환
```

<div class="content-ad"></div>

이 예시에서 memoizedFetchData(1) 함수를 처음 호출하면 API에서 데이터를 가져오지만, 동일한 id로 이후에 호출된 함수는 캐시된 결과를 반환하여 중복된 네트워크 요청을 방지합니다.

# React에서의 메모이제이션

React에서 메모이제이션은 함수 컴포넌트와 후크를 최적화하는 데 유용할 수 있습니다.

# React.memo를 사용한 함수 컴포넌트의 메모이제이션

<div class="content-ad"></div>

리액트는 함수형 컴포넌트를 메모이제이션하는 데 React.memo 함수를 제공하여 불필요한 다시 렌더링을 방지합니다.

```js
import React from "react";

const MyComponent = React.memo(({ value }) => {
  console.log("Rendered with value:", value);
  return <div>{value}</div>;
});
export default MyComponent;
```

이 예시에서 MyComponent는 value prop이 변경될 때만 다시 렌더링됩니다.

# useMemo를 사용하여 비용이 많이 드는 계산 메모이제이션하기

<div class="content-ad"></div>

React의 useMemo 훅은 함수형 컴포넌트 내에서 비용이 많이 드는 계산 결과를 메모이징하는 데 사용할 수 있어요.

```js
import React, { useMemo } from "react";

function ExpensiveComponent({ num }) {
  const expensiveCalculation = useMemo(() => {
    console.log("비용이 많이 드는 계산 중...");
    return num * num;
  }, [num]);
  return <div>결과: {expensiveCalculation}</div>;
}
export default ExpensiveComponent;
```

여기서 비용이 많이 드는 계산은 num이 변경될 때만 다시 계산됩니다.

# Angular에서 메모이제이션

<div class="content-ad"></div>

앵귤러에서는 메모이제이션을 서비스 메소드와 템플릿 표현식에 적용할 수 있어요.

## 서비스 메소드에 메모이제이션 적용하기

로다시 같은 메모이제이션 라이브러리를 사용하거나 직접 메모이제이션 함수를 구현할 수 있어요.

```js
import { Injectable } from '@angular/core';
import * as _ from 'lodash';

@Injectable({
  providedIn: 'root'
})
export class DataService {
  private expensiveCalculation = _.memoize((num: number) => {
    console.log('비용이 많이 드는 계산 수행 중...');
    return num * num;
  });
  getCalculation(num: number): number {
    return this.expensiveCalculation(num);
  }
}
```

<div class="content-ad"></div>

## 순수 파이프를 사용하여 템플릿 표현식의 결과를 메모이제이션하기

앵귤러의 순수 파이프를 사용하여 템플릿 표현식을 메모이제이션할 수도 있습니다.

```js
import { Pipe, PipeTransform } from "@angular/core";

@Pipe({
  name: "expensiveCalculation",
  pure: true,
})
export class ExpensiveCalculationPipe implements PipeTransform {
  transform(value: number): number {
    console.log("비용 많이 드는 계산 수행 중...");
    return value * value;
  }
}
```

```js
<div>{num | expensiveCalculation}</div>
```

<div class="content-ad"></div>

# TypeScript을 사용한 메모이제이션

TypeScript을 사용하면 메모이제이션 함수에 타입 안전성을 추가할 수 있어요.

## 타입 안전한 메모이제이션 함수

```js
function memoize<T extends (...args: any[]) => any>(fn: T): T {
  const cache = new Map<string, ReturnType<T>>();
  return function(...args: Parameters<T>): ReturnType<T> {
    const key = JSON.stringify(args);
    if (cache.has(key)) {
      return cache.get(key)!;
    }
    const result = fn(...args);
    cache.set(key, result);
    return result;
  } as T;
}
```

<div class="content-ad"></div>

## 예제: TypeScript 함수 메모이제이션

```typescript
function add(a: number, b: number): number {
  return a + b;
}

const memoizedAdd = memoize(add);
console.log(memoizedAdd(2, 3)); // 5 (계산)
console.log(memoizedAdd(2, 3)); // 5 (캐시)
```

이 예제에서 TypeScript는 메모이제이션 함수 및 사용 방법이 타입 안전성을 유지하며 개발 중 추가적인 신뢰성을 제공합니다.

# 결론

<div class="content-ad"></div>

메모이제이션은 JavaScript 함수를 최적화하는 간단하면서도 강력한 기술입니다. 비싼 함수 호출의 결과를 캐싱함으로써, 재귀 함수 및 반복적인 계산이 포함된 작업에서 애플리케이션의 성능을 향상시킬 수 있습니다.

하지만 이점과 잠재적인 단점을 모두 고려하는 것이 중요합니다.
