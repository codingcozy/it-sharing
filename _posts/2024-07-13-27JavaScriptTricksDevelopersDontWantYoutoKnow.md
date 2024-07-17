---
title: "개발자들이 절대 알려주지 않는 27가지 JavaScript 비법"
description: ""
coverImage: "/assets/img/2024-07-13-27JavaScriptTricksDevelopersDontWantYoutoKnow_0.png"
date: 2024-07-13 18:27
ogImage:
  url: /assets/img/2024-07-13-27JavaScriptTricksDevelopersDontWantYoutoKnow_0.png
tag: Tech
originalTitle: "27 JavaScript Tricks Developers Don’t Want You to Know"
link: "https://medium.com/@learntocodetoday/27-javascript-tricks-developers-dont-want-you-to-know-1d407dd3ee4b"
---

![이미지](/assets/img/2024-07-13-27JavaScriptTricksDevelopersDontWantYoutoKnow_0.png)

자바스크립트는 다양한 기능과 요령을 가진 다재다능한 언어로, 코딩 효율성과 능력을 크게 향상시킬 수 있는 숨겨진 기능과 요령이 많이 있습니다. 이 글에서는 숙련된 개발자들이 더 깨끗하고 효율적이며 표현력이 있는 코드를 작성하는 데 사용하는 27가지 자바스크립트 요령을 살펴보겠습니다.

## 1. 해체 할당 (Destructuring Assignment)

해체 할당은 배열이나 객체의 속성에서 값을 추출하여 개별 변수로 분리하는 것을 가능하게 합니다.

<div class="content-ad"></div>

# 예시:

```js
// 배열 구조 분해
const numbers = [1, 2, 3];
const [a, b, c] = numbers;
console.log(a, b, c); // 1 2 3

// 객체 구조 분해
const person = { name: "Alice", age: 30 };
const { name, age } = person;
console.log(name, age); // Alice 30
```

# 2. 전개 구문

전개 구문 (...)은 반복 가능한 항목(예: 배열)을 함수 호출의 인수나 배열 리터럴의 요소가 기대되는 위치에서 확장할 수 있습니다.

<div class="content-ad"></div>

# 예시:

```js
// 배열 확장
const arr1 = [1, 2, 3];
const arr2 = [...arr1, 4, 5];
console.log(arr2); // [1, 2, 3, 4, 5]

// 객체 확장
const obj1 = { foo: "bar", x: 42 };
const obj2 = { ...obj1, y: 17 };
console.log(obj2); // { foo: 'bar', x: 42, y: 17 }
```

# 3. 널 병합 연산자

널 병합 연산자 (??)는 좌측 피연산자가 null 또는 undefined일 때 우측 피연산자를 반환하지만, `` 또는 0은 해당되지 않습니다.

<div class="content-ad"></div>

# 예시:

```js
const defaultValue = "기본 값";
const userInput = null;
const value = userInput ?? defaultValue;
console.log(value); // 기본 값
```

# 4. 옵셔널 체이닝

옵셔널 체이닝(?.)을 사용하면 객체의 깊게 중첩된 속성에 액세스할 때 해당 속성 중 어느 것이 null 또는 undefined인지 걱정할 필요가 없습니다.

<div class="content-ad"></div>

# 예시:

```js
const user = {
  name: "Alice",
  address: {
    city: "Wonderland",
  },
};

console.log(user.address?.city); // Wonderland
console.log(user.address?.street?.name); // undefined (no error)
```

# 5. 템플릿 리터럴

템플릿 리터럴(\`)을 사용하면 간단한 문자열 보간과 여러 줄의 문자열을 손쉽게 처리할 수 있으며, 복잡한 문자열을 읽기 쉽게 만들어줍니다.

<div class="content-ad"></div>

# 예시:

```js
const name = "Alice";
const greeting = `안녕하세요, ${name}님!
저희 웹사이트에 오신 것을 환영합니다.`;
console.log(greeting);
```

# 6. 화살표 함수

화살표 함수(`=>`)는 함수 표현식을 간결하게 작성할 수 있는 구문으로, 암시적 반환과 렉시컬 this 바인딩을 제공합니다.

<div class="content-ad"></div>

# 예제:

```js
const numbers = [1, 2, 3];
const doubled = numbers.map((num) => num * 2);
console.log(doubled); // [2, 4, 6]
```

# 7. 즉시 호출 함수 표현식 (IIFE)

IIFE는 함수를 정의한 직후 즉시 실행하여 전역 스코프 오염을 방지할 수 있게 합니다.

<div class="content-ad"></div>

# 예시:

```js
(function () {
  const message = "안녕하세요!";
  console.log(message);
})();
```

# 8. Object.entries()

Object.entries()은 주어진 객체의 열거 가능한 속성 키-값 쌍 [키, 값]의 배열을 반환합니다.

<div class="content-ad"></div>

```js
# 예시:

const obj = { a: 1, b: 2, c: 3 };
console.log(Object.entries(obj)); // [['a', 1], ['b', 2], ['c', 3]]
```

# 9. Array.from()

Array.from()은 배열과 유사한(배열처럼 동작하는) 객체나 이터러블 객체로부터 새롭고 얕게 복사된 배열 인스턴스를 생성합니다.

<div class="content-ad"></div>

# 예시:

```js
const set = new Set([1, 2, 3]);
const arr = Array.from(set);
console.log(arr); // [1, 2, 3]
```

# 10. Array.prototype.find()

Array.prototype.find()은 제공된 테스트 함수를 만족하는 배열의 첫 번째 요소를 반환합니다.

<div class="content-ad"></div>

# 예시:

```js
const numbers = [10, 20, 30, 40];
const found = numbers.find((num) => num > 25);
console.log(found); // 30
```

# 11. Array.prototype.every()

Array.prototype.every()은 배열의 모든 요소가 제공된 함수에 의해 구현된 테스트를 통화하는지를 확인합니다.

<div class="content-ad"></div>

# 예제:

```js
const numbers = [1, 2, 3, 4];
const allPositive = numbers.every((num) => num > 0);
console.log(allPositive); // true
```

# 12. Array.prototype.some()

Array.prototype.some()은 배열에서 제공된 함수에 의해 구현된 테스트를 통과하는 요소가 하나 이상 있는지를 확인합니다.

<div class="content-ad"></div>

# 예시:

```js
const numbers = [-1, 2, -3, 4];
const hasPositive = numbers.some((num) => num > 0);
console.log(hasPositive); // true
```

# 13. String.prototype.includes()

String.prototype.includes() 메서드는 한 문자열이 다른 문자열 내에 존재하는지 여부를 결정하고 필요에 따라 true 또는 false를 반환합니다.

<div class="content-ad"></div>

# 예제:

```js
const sentence = "Hello, world!";
console.log(sentence.includes("world")); // true
```

# 14. String.prototype.startsWith()와 String.prototype.endsWith()

String.prototype.startsWith()와 String.prototype.endsWith()는 문자열이 다른 문자열로 시작하는지 또는 끝나는지를 결정하여 true 또는 false를 반환합니다.

<div class="content-ad"></div>

# 예시:

```js
const str = "Hello, world!";
console.log(str.startsWith("Hello")); // true
console.log(str.endsWith("!")); // true
```

# 15. Array.prototype.reduce()

Array.prototype.reduce()은 배열의 각 요소에 대해 제공한 리듀서 함수를 실행하여 단일 출력 값을 생성합니다.

<div class="content-ad"></div>

# 예제:

```js
const numbers = [1, 2, 3, 4];
const sum = numbers.reduce((acc, curr) => acc + curr, 0);
console.log(sum); // 10
```

# 16. 기본 매개변수 값

기본 매개변수 값은 지정된 매개변수가 값이나 undefined가 전달되지 않은 경우 기본 값으로 초기화되도록 허용합니다.

<div class="content-ad"></div>

# 예시:

```js
function greet(name = "손님") {
  console.log(`안녕하세요, ${name} 님!`);
}
greet(); // 안녕하세요, 손님 님!
greet("Alice"); // 안녕하세요, Alice 님!
```

# 17. Object.freeze()

Object.freeze()은 객체를 동결시킵니다. 즉, 새로운 속성이 추가되는 것을 막고, 기존 속성이 제거되는 것을 방지하며, 기존 속성이나 열거 가능성, 설정 가능성 또는 쓰기 가능성이 변경되는 것을 방지합니다.

<div class="content-ad"></div>

# 예시:

```js
const obj = { prop: 42 };
Object.freeze(obj);
obj.prop = 33; // 엄격 모드에서 오류 발생
console.log(obj.prop); // 42
```

# 18. Function.prototype.bind()

Function.prototype.bind()은 호출될 때 this 키워드가 제공된 값으로 설정되고, 새 함수가 호출될 때 제공된 인수 시퀀스 앞에 주어진 인수를 가지고 새로운 함수를 생성합니다.

<div class="content-ad"></div>

# Example:

```js
const module = {
  x: 42,
  getX: function () {
    return this.x;
  },
};

const unboundGetX = module.getX;
const boundGetX = unboundGetX.bind(module);
console.log(boundGetX()); // 42
```

# 19. JSON.stringify() and JSON.parse()

JSON.stringify()은 JavaScript 객체나 값을 JSON 문자열로 변환하고, JSON.parse()는 JSON 문자열을 구문 분석하여 문자열에 설명된 JavaScript 값을 빌드합니다.

<div class="content-ad"></div>

# 예시:

```js
const obj = { name: "Alice", age: 30 };
const jsonString = JSON.stringify(obj);
console.log(jsonString); // '{"name":"Alice","age":30}'

const parsedObject = JSON.parse(jsonString);
console.log(parsedObject); // { name: 'Alice', age: 30 }
```

# 20. BigInt

BigInt은 253-1보다 큰 정수를 표현하는 방법을 제공하는 내장 객체입니다. 이는 JavaScript가 Number 원시형으로 신뢰할 수 있는 제일 큰 숫자를 나타내는 방법입니다.

<div class="content-ad"></div>

# 예시:

```js
const bigNumber = 123456789012345678901234567890n;
console.log(bigNumber); // 123456789012345678901234567890n
```

# 21. Private Class Fields

개인 클래스 필드는 클래스 내에서 개인 필드를 선언하기 위해 해시(#)를 사용하여 외부에서 액세스를 제한합니다.

<div class="content-ad"></div>

# 예제:

```js
class Counter {
  #count = 0;

  increment() {
    this.#count++;
  }

  getCount() {
    return this.#count;
  }
}

const counter = new Counter();
counter.increment();
console.log(counter.getCount()); // 1
console.log(counter.#count); // Error: Private field '#count' is not defined
```

# 22. 동적 Imports

동적 Imports를 사용하면 JavaScript 모듈을 조건부로 또는 필요할 때만 가져와 성능을 향상시킬 수 있습니다.

<div class="content-ad"></div>

# 예시:

```js
const moduleSpecifier = "./module.js";
import(moduleSpecifier)
  .then((module) => {
    console.log(module.default);
  })
  .catch((err) => {
    console.error("모듈을 불러오는 데 실패했습니다", err);
  });
```

# 23. Named Exports and Imports

Named exports 및 imports를 사용하면 JavaScript 파일 간에 함수, 객체 또는 기본 형식을 선택적으로 내보낼 수 있고 가져올 수 있습니다.

<div class="content-ad"></div>

# 예시:

```js
// module.js
export function greet(name) {
  return `안녕, ${name}!`;
}

// main.js
import { greet } from "./module.js";
console.log(greet("Alice")); // 안녕, Alice!
```

# 24. Proxy

프록시 객체는 객체의 기본 작업(예: 속성 조회, 할당, 열거, 함수 호출)을 가로채고 사용자 정의할 수 있게 해줍니다.

<div class="content-ad"></div>

# Example:

```js
const handler = {
  get: function (target, prop, receiver) {
    console.log(`Getting property ${prop}`);
    return target[prop];
  },
};

const obj = { a: 1 };
const proxy = new Proxy(obj, handler);
console.log(proxy.a); // Getting property a, 1
```

# 25. WeakMap and WeakSet

WeakMap과 WeakSet 객체는 가비지 수집을 방해하지 않고 약하게 보유된 객체를 컬렉션에 저장할 수 있게 합니다.

<div class="content-ad"></div>

# 예시:

```js
let key = { id: 1 };
const weakMap = new WeakMap();
weakMap.set(key, "Value");
console.log(weakMap.get(key)); // Value

key = null; // 참조를 제거합니다.
console.log(weakMap.get(key)); // undefined
```

# 26. 비동기 함수와 대기

비동기 함수와 await 키워드를 사용하면 동기적인 방식으로 비동기, 논블로킹 함수를 작성할 수 있어서 코드 가독성이 향상됩니다.

<div class="content-ad"></div>

# 예시:

```js
function fetchData() {
  return new Promise((resolve) => {
    setTimeout(() => resolve("Data"), 1000);
  });
}

async function getData() {
  const data = await fetchData();
  console.log(data); // Data
}

getData();
```

# 27. Intersection Observer API

Intersection Observer API는 대상 요소와 상위 요소 또는 최상위 문서 뷰포트와의 교차점 변경을 비동기적으로 관찰하는 방법을 제공합니다.

<div class="content-ad"></div>

# 예시:

```js
const observer = new IntersectionObserver((entries) => {
  entries.forEach((entry) => {
    if (entry.isIntersecting) {
      console.log("요소가 완전히 보입니다");
    } else {
      console.log("요소가 보이지 않습니다");
    }
  });
});

const targetElement = document.querySelector(".target");
observer.observe(targetElement);
```

# 결론

이 JavaScript 트릭들은 언어의 다양성과 기능의 강력함을 보여줍니다. 이러한 트릭들을 코딩 Repertoire에 편입함으로써, 더 효율적이고 표현력 있는 그리고 유지보수하기 쉬운 JavaScript 코드를 작성할 수 있습니다. 프로젝트에서 이러한 기술들을 실험해보고 최대한 활용하여 더 능숙한 JavaScript 개발자가 되어보세요.
