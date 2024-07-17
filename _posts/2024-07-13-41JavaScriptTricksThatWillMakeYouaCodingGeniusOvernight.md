---
title: "하룻밤 만에 코딩 천재가 되는 41가지 자바스크립트 트릭"
description: ""
coverImage: "/assets/img/2024-07-13-41JavaScriptTricksThatWillMakeYouaCodingGeniusOvernight_0.png"
date: 2024-07-13 18:31
ogImage:
  url: /assets/img/2024-07-13-41JavaScriptTricksThatWillMakeYouaCodingGeniusOvernight_0.png
tag: Tech
originalTitle: "41 JavaScript Tricks That Will Make You a Coding Genius Overnight!"
link: "https://medium.com/@learntocodetoday/41-javascript-tricks-that-will-make-you-a-coding-genius-overnight-8039aaf6630b"
---

![JavaScript](/assets/img/2024-07-13-41JavaScriptTricksThatWillMakeYouaCodingGeniusOvernight_0.png)

자바스크립트는 다재다능하고 강력한 언어로 웹 개발에서 널리 사용됩니다. 초보자든 경험 많은 코더든, 항상 새로운 것을 배울 수 있습니다. 여기에는 코딩 기술을 향상시키고 더 효율적이고 효과적인 개발자로 만들어줄 41가지의 자바스크립트 트릭이 있습니다.

# 1. 해체 할당 (Destructuring Assignment)

해체 할당은 배열에서의 값이나 객체의 속성을 개별적인 변수에 풀어 헤칠 수 있는 기능을 제공합니다.

<div class="content-ad"></div>

# 예시:

```js
const user = { name: "Alice", age: 25 };
const { name, age } = user;
console.log(name, age); // Alice 25
```

# 2. 기본 매개변수

기본 매개변수를 사용하면 함수 매개변수에 기본값을 설정할 수 있습니다.

<div class="content-ad"></div>

# 예시:

```js
function greet(name = "손님") {
  console.log(`안녕하세요, ${name}님!`);
}
greet(); // 안녕하세요, 손님!
```

# 3. 템플릿 리터럴

템플릿 리터럴은 여러 줄의 문자열을 만들거나 문자열 내에 표현식을 포함시킬 수 있는 쉬운 방법을 제공합니다.

<div class="content-ad"></div>

# 예시:

```js
const name = "Alice";
const greeting = `안녕, ${name}!`;
console.log(greeting); // 안녕, Alice!
```

# 4. 화살표 함수

화살표 함수는 함수를 간결하게 작성할 수 있는 구문을 제공하며, this 값을 렉시컬하게 바인딩합니다.

<div class="content-ad"></div>

# 예시:

```js
const add = (a, b) => a + b;
console.log(add(2, 3)); // 5
```

# 5. 전개 연산자

전개 연산자는 반복 가능한(iterable) 항목(예: 배열)을 개별 요소로 확장할 수 있게 해줍니다.

<div class="content-ad"></div>

# 예제:

```js
const arr1 = [1, 2, 3];
const arr2 = [...arr1, 4, 5];
console.log(arr2); // [1, 2, 3, 4, 5]
```

## 6. 나머지 매개변수

나머지 매개변수를 사용하면 인수의 무한한 수를 배열로 나타낼 수 있습니다.

<div class="content-ad"></div>

# 예시:

```js
function sum(...args) {
  return args.reduce((a, b) => a + b, 0);
}
console.log(sum(1, 2, 3)); // 6
```

# 7. 단축 평가

논리 연산자를 사용하여 기본값을 제공하거나 함수를 조건부로 실행하세요.

<div class="content-ad"></div>

# 예시:

```js
const name = (user && user.name) || "Guest";
console.log(name); // Guest
```

# 8. 객체 속성 축약

속성 이름과 변수 이름이 동일한 경우 축약 구문을 사용할 수 있습니다.

<div class="content-ad"></div>

# 예제:

```js
const name = "Alice";
const user = { name };
console.log(user); // { name: 'Alice' }
```

# 9. 동적 속성 이름

객체 리터럴에서 속성 이름으로 표현식을 사용할 수 있습니다.

<div class="content-ad"></div>

# 예시:

```js
const propName = "age";
const user = { name: "Alice", [propName]: 25 };
console.log(user); // { name: 'Alice', age: 25 }
```

# 10. Async/Await

Async/await을 사용하면 비동기 코드를 동기적으로 작성하여 가독성을 향상시킬 수 있습니다.

<div class="content-ad"></div>

# 예시:

```js
async function fetchData() {
  const response = await fetch("https://api.example.com/data");
  const data = await response.json();
  console.log(data);
}
fetchData();
```

# 11. Promises

프로미스는 비동기 작업을 다루고 콜백을 처리하는 더 깔끔한 방법을 제공합니다.

<div class="content-ad"></div>

# 예시:

```js
function fetchData() {
  return new Promise((resolve, reject) => {
    setTimeout(() => resolve("데이터 수신 완료"), 1000);
  });
}
fetchData().then((data) => console.log(data));
```

# 12. Promises 연결하기

여러 개의 프로미스를 연결하여 순차적인 비동기 작업을 수행할 수 있습니다.

<div class="content-ad"></div>

# 예시:

```js
fetchData()
  .then((data) => {
    console.log(data);
    return fetchData();
  })
  .then((data) => console.log(data));
```

# 13. Optional Chaining

Optional chaining은 중첩된 속성에 안전하게 접근할 수 있게 해줍니다.

<div class="content-ad"></div>

# 예시:

```js
const user = { profile: { name: "Alice" } };
const name = user?.profile?.name;
console.log(name); // Alice
```

# 14. 널리시 콜레싱 연산자

널리시 콜레싱 연산자 (??)는 왼쪽 피연산자가 null 또는 undefined일 때만 기본값을 제공합니다.

<div class="content-ad"></div>

# 예시:

```js
const name = null ?? "Guest";
console.log(name); // Guest
```

# 15. Map과 Set

Map과 Set은 고유한 값 및 키-값 쌍을 저장하기 위한 내장 데이터 구조입니다.

<div class="content-ad"></div>

# 예시:

```js
const map = new Map();
map.set("name", "Alice");
console.log(map.get("name")); // Alice

const set = new Set([1, 2, 2, 3]);
console.log(set); // Set { 1, 2, 3 }
```

# 16. 반복자와 제너레이터

반복자와 제너레이터는 데이터 시퀀스를 처리하는 강력한 방법을 제공합니다.

<div class="content-ad"></div>

# 예시:

```js
function* idGenerator() {
  let id = 1;
  while (true) {
    yield id++;
  }
}
const gen = idGenerator();
console.log(gen.next().value); // 1
console.log(gen.next().value); // 2
```

# 17. 배열 메소드

자바스크립트는 배열을 다루는 것을 쉽게 만들어주는 다양한 배열 메소드를 제공합니다.

<div class="content-ad"></div>

# 예시:

```js
const arr = [1, 2, 3, 4, 5];
const even = arr.filter((num) => num % 2 === 0);
console.log(even); // [2, 4]
```

# 18. 배열 해체

배열 해체를 사용하면 배열에서 값을 추출하여 개별 변수로 언팩할 수 있습니다.

<div class="content-ad"></div>

# 예시:

```js
const [a, b] = [1, 2];
console.log(a, b); // 1 2
```

# 19. 객체 구조 분해

객체 구조 분해는 객체에서 속성들을 분리하여 서로 다른 변수로 언팩하는 것을 허용합니다.

<div class="content-ad"></div>

# 예시:

```js
const user = { name: "Alice", age: 25 };
const { name, age } = user;
console.log(name, age); // Alice 25
```

# 20. Object.assign

Object.assign은 하나 이상의 소스 객체에서 값을 대상 객체로 복사하는 데 사용됩니다.

<div class="content-ad"></div>

# 예시:

```js
const target = { a: 1 };
const source = { b: 2 };
const result = Object.assign(target, source);
console.log(result); // { a: 1, b: 2 }
```

# 21. Object.freeze

Object.freeze은 객체 내 기존 속성 및 값의 수정을 방지합니다.

<div class="content-ad"></div>

# 예시:

```js
const obj = Object.freeze({ name: "Alice" });
obj.name = "Bob"; // 이 코드는 영향을 미치지 않습니다.
console.log(obj.name); // Alice
```

# 22. Object.entries와 Object.values

Object.entries와 Object.values는 주어진 객체의 고유한 열거 가능한 속성 [키, 값] 쌍 및 값의 배열을 각각 반환합니다.

<div class="content-ad"></div>

# 예시:

```js
const user = { name: "Alice", age: 25 };
const entries = Object.entries(user);
console.log(entries); // [['name', 'Alice'], ['age', 25]]

const values = Object.values(user);
console.log(values); // ['Alice', 25]
```

# 23. 함수 호이스팅

함수 선언문은 자신이 포함된 스코프의 맨 위로 호이스팅됩니다.

<div class="content-ad"></div>

# 예시:

```js
호이스팅된함수();

function 호이스팅된함수() {
  console.log("이 함수는 호이스팅되었습니다!");
}
```

# 24. let과 const를 사용한 블록 범위 변수

var와는 달리, let과 const는 블록 범위를 갖기 때문에 변수 호이스팅으로 인한 버그를 줄여줍니다.

<div class="content-ad"></div>

# 예시:

```js
if (true) {
  let blockScoped = "I am block-scoped!";
  console.log(blockScoped);
}
// console.log(blockScoped); // 오류: blockScoped가 정의되지 않았습니다
```

# 25. HTML을 위한 템플릿 리터럴

템플릿 리터럴은 내장 표현식이 포함된 HTML 문자열을 생성하는 데 사용할 수 있습니다.

<div class="content-ad"></div>

# 예시:

```js
const name = "Alice";
const html = `\`<div>
    <p>Hello, ${name}!</p>
</div>\``;
console.log(html);
```

# 26. 태그된 템플릿 리터럴

태그된 템플릿을 사용하면 함수로 템플릿 리터럴을 구문 분석할 수 있습니다.

<div class="content-ad"></div>

# 예시:

```js
function tag(strings, ...values) {
  console.log(strings);
  console.log(values);
}
tag`안녕, ${name}!`;
// ["안녕, ", "!"]
// ["Alice"]
```

# 27. 기본 함수 설정

함수 매개변수에 기본값을 설정할 수 있습니다.

<div class="content-ad"></div>

# 예시:

```js
function greet(name = "손님") {
  console.log(`안녕하세요, ${name}님!`);
}
greet(); // 안녕하세요, 손님!
```

# 28. 함수 표현식과 익명 함수

함수는 변수에 할당되어 인수로 전달될 수 있습니다.

<div class="content-ad"></div>

# 예시:

```js
const greet = function (name) {
  console.log(`안녕, ${name}님!`);
};
greet("Alice");
```

# 29. 즉시 호출 함수 표현식 (IIFE)

IIFE는 정의된 즉시 실행되는 함수입니다.

<div class="content-ad"></div>

# 예시:

```js
(function () {
  console.log("IIFE 실행됨!");
})();
```

# 30. 클로저

클로저는 해당 범위 외부에서 함수가 실행되더라도 렉시컬 스코프에 대한 액세스 권한을 유지하는 함수입니다.

<div class="content-ad"></div>

# 예시:

```js
function createCounter() {
  let count = 0;
  return function () {
    count++;
    return count;
  };
}
```

# 31. 고차 함수

고차 함수는 다른 함수를 처리하는 함수로, 해당 함수를 인수로 사용하거나 반환하는 방식으로 작동합니다.

<div class="content-ad"></div>

# 예시:

```js
function higherOrderFunction(callback) {
  callback();
}
higherOrderFunction(() => console.log("콜백이 실행되었습니다!"));
```

# 32. 커링

커링은 여러 개의 인수를 가지고 있는 함수를 각각 하나의 인수를 가진 함수들의 연속으로 변환하는 과정입니다.

<div class="content-ad"></div>

# 예제:

```js
function curry(f) {
  return function (a) {
    return function (b) {
      return f(a, b);
    };
  };
}

const sum = (a, b) => a + b;
const curriedSum = curry(sum);
console.log(curriedSum(1)(2)); // 3
```

# 33. 메모이제이션

메모이제이션이란 결과를 캐싱함으로써 함수 호출을 가속화하는 기술입니다.

<div class="content-ad"></div>

# 예시:

```js
function memoize(fn) {
  const cache = {};
  return function (...args) {
    const key = JSON.stringify(args);
    if (!cache[key]) {
      cache[key] = fn(...args);
    }
    return cache[key];
  };
}

const slowFunction = (n) => {
  // 느린 함수를 시뮬레이션합니다
  for (let i = 0; i < 1e6; i++) {}
  return n;
};
const fastFunction = memoize(slowFunction);
console.log(fastFunction(10)); // 10
console.log(fastFunction(10)); // 10 (캐시에서 가져옴)
```

# 34. 디바운싱과 스로틀링

디바운싱과 스로틀링은 함수가 실행되는 속도를 제어하는 기술입니다.

<div class="content-ad"></div>

# 예시 (디바운싱):

```js
function debounce(fn, delay) {
  let timer;
  return function (...args) {
    clearTimeout(timer);
    timer = setTimeout(() => fn(...args), delay);
  };
}

const handleResize = debounce(() => {
  console.log("Resized!");
}, 500);

window.addEventListener("resize", handleResize);
```

# 예시 (스로틀링):

```js
function throttle(fn, interval) {
  let lastCall = 0;
  return function (...args) {
    const now = Date.now();
    if (now - lastCall >= interval) {
      lastCall = now;
      fn(...args);
    }
  };
}

const handleScroll = throttle(() => {
  console.log("Scrolled!");
}, 1000);

window.addEventListener("scroll", handleScroll);
```

<div class="content-ad"></div>

# 35. 이벤트 위임

이벤트 위임은 여러 자식 요소의 이벤트를 관리하기 위해 부모 요소에 단일 이벤트 리스너를 추가할 수 있게 해줍니다.

# 예시:

```js
document.getElementById("parent").addEventListener("click", function (event) {
  if (event.target.matches(".child")) {
    console.log("자식 요소가 클릭되었습니다!", event.target);
  }
});
```

<div class="content-ad"></div>

# 36. 객체 지향 프로그래밍을 위한 클래스 사용

JavaScript의 클래스 구문을 사용하면 더 깔끔하고 간결한 객체 지향 코드를 작성할 수 있습니다.

# 예제:

```js
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  greet() {
    console.log(`안녕, 내 이름은 ${this.name}이고, ${this.age}살이야.`);
  }
}

const alice = new Person("Alice", 25);
alice.greet(); // 안녕, 내 이름은 Alice이고, 25살이야.
```

<div class="content-ad"></div>

# 37. this 키워드

다양한 컨텍스트에서 this가 어떻게 작동하는지 이해하는 것은 JavaScript를 정복하는 데 중요합니다.

# 예시:

```js
const obj = {
  name: "Alice",
  greet() {
    console.log(this.name);
  },
};

const greet = obj.greet;
greet(); // undefined (또는 strict mode가 아닐 때 전역 객체 출력)

const boundGreet = obj.greet.bind(obj);
boundGreet(); // Alice
```

<div class="content-ad"></div>

# 38. for await...of를 사용한 비동기 반복

for await...of를 사용하면 async 이터러블 객체를 반복할 수 있습니다.

# 예시:

```js
async function* asyncGenerator() {
  yield "Hello";
  yield "World";
}

(async () => {
  for await (const value of asyncGenerator()) {
    console.log(value);
  }
})();
```

<div class="content-ad"></div>

# 39. 모듈 사용하기

모듈을 사용하면 코드를 분리하여 파일로 관리하고 기능을 가져오고 내보낼 수 있습니다.

# 예시:

```js
// math.js
export function add(a, b) {
  return a + b;
}

// main.js
import { add } from "./math.js";
console.log(add(2, 3)); // 5
```

<div class="content-ad"></div>

# 40. 프록시 객체

프록시는 객체의 기본 작업의 동작을 사용자 정의하는 방법을 제공합니다.

# 예시:

```js
const handler = {
  get: (target, prop) => {
    return prop in target ? target[prop] : "속성이 존재하지 않습니다";
  },
};

const user = new Proxy({}, handler);
user.name = "Alice";
console.log(user.name); // Alice
console.log(user.age); // 속성이 존재하지 않습니다
```

<div class="content-ad"></div>

# 41. 심볼 데이터 유형

심볼은 다른 코드에 노출되지 않는 고유한 식별자를 생성하는 방법을 제공합니다.

# 예시:

```js
const sym1 = Symbol("description");
const sym2 = Symbol("description");
console.log(sym1 === sym2); // false

const obj = {
  [sym1]: "value1",
  [sym2]: "value2",
};
console.log(obj[sym1]); // value1
```

<div class="content-ad"></div>

# 결론

이 41가지의 JavaScript 요령은 여러 기술과 최良의 방법을 다루며 능숙하고 효율적인 개발자가 되는 데 도움이 될 것입니다. 이러한 요령을 숙달함으로써 코딩 천재로 가는 길에 올라설 수 있을 것입니다. 이러한 팁들을 프로젝트에 적용하여 JavaScript 기술을 크게 향상시킬 수 있을 것입니다!
