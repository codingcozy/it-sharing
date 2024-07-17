---
title: "모든 자바스크립트 개발자가 알아야 할 33가지 개념"
description: ""
coverImage: "/assets/img/2024-07-09-33ConceptsEveryJavaScriptDeveloperShouldKnow_0.png"
date: 2024-07-09 13:07
ogImage:
  url: /assets/img/2024-07-09-33ConceptsEveryJavaScriptDeveloperShouldKnow_0.png
tag: Tech
originalTitle: "33 Concepts Every JavaScript Developer Should Know"
link: "https://medium.com/@codingwinner/33-concepts-every-javascript-developer-should-know-ef225a72ed7f"
---

## 이 주제에 대해 50개 이상의 글을 모았어요. 이 글들이 당신이 더 잘 배울 수 있도록 도와줄 것을 희망합니다.

![33 Concepts Every JavaScript Developer Should Know](/assets/img/2024-07-09-33ConceptsEveryJavaScriptDeveloperShouldKnow_0.png)

자바스크립트에 대해 얼마나 알고 있다고 생각하시나요? 아마도 함수를 작성하는 방법을 알고, 간단한 알고리즘을 이해하고 클래스를 작성할 수 있다고 생각할 것입니다. 그렇지만 타입된 배열이 무엇인지 알고 계신가요?

지금 당장 모든 이 개념을 알 필요는 없지만, 나중에 직업 생활 중에 필요할 때가 올 겁니다. 그래서 이 목록을 북마크해 두는 것을 추천합니다. 왜냐하면 어디서 하나의 주제에 부딪히게 될지 모르기 때문에, 그럴 때 해당 내용을 완전히 이해할 수 있는 자습서가 필요할 수도 있기 때문입니다.

<div class="content-ad"></div>

다음 저장소에서 영감을 받은이 목록이 중요하다는 점을 알아두세요:

### 모든 JavaScript 개발자가 알아야 할 33가지 개념

![이미지](/assets/img/2024-07-09-33ConceptsEveryJavaScriptDeveloperShouldKnow_1.png)

## 목차

<div class="content-ad"></div>

- 호출 스택
- 원시 자료형
- 값 타입과 참조 타입
- 암시적, 명시적, 명명형, 구조화 및 덕 타이핑
- == vs === vs typeof
- 함수 스코프, 블록 스코프 및 렉시컬 스코프
- 표현식 vs 문장
- IIFE, 모듈 및 네임스페이스
- 메시지 대기열 및 이벤트 루프
- setTimeout, setInterval 및 requestAnimationFrame
- JavaScript 엔진
- 비트 연산자, 타입 배열 및 배열 버퍼
- DOM 및 레이아웃 트리
- 팩토리 및 클래스
- this, call, apply 및 bind
- new, 생성자, instanceof 및 인스턴스
- 프로토타입 상속 및 프로토타입 체인
- Object.create 및 Object.assign
- map, reduce, filter
- 순수 함수, 부작용, 상태 변이 및 이벤트 전파
- 클로저
- 고차 함수
- 재귀
- 컬렉션과 제너레이터
- Promises
- async/await
- 자료 구조
- 비싼 작업과 빅 오 표기법
- 알고리즘
- 상속, 다형성 및 코드 재사용
- 디자인 패턴
- 부분 적용, 커링, 합성 및 파이프
- 깨끗한 코드

# 1. 호출 스택

![이미지](/assets/img/2024-07-09-33ConceptsEveryJavaScriptDeveloperShouldKnow_2.png)

자바스크립트에서 콜백은 단순히 다른 함수에 매개변수로 전달되어 그 함수 안에서 호출되거나 실행되는 함수입니다. 여기서 함수는 다른 함수가 실행되거나 값을 반환할 때 기다려야 하며, 이로 인해 기능 체인이 형성됩니다(X가 완료되면 Y가 실행되고 그러면 계속됩니다.). 이러한 이유로 콜백은 일반적으로 자바스크립트의 비동기 작업에서 동기 기능을 제공하기 위해 사용됩니다.

<div class="content-ad"></div>

예시: 아래는 콜백(callbacks)의 예시입니다.

자바스크립트

```js
const greeting = (name) => {
  console.log("Hello " + name);
};
const processUserName = (callback) => {
  name = "GeeksforGeeks";
  callback(name);
};
processUserName(greeting);
```

결과:

<div class="content-ad"></div>

```js
안녕 GeeksforGeeks
```

위 예제에서는 'processUserName' 함수에 전달된 인사말(callback)을 주목해보세요. 'greeting' 함수가 실행되기 전에 'processUserName' 이벤트가 먼저 실행될 때까지 기다립니다.

# 튜토리얼

- 📜 자바스크립트 콜 스택, 이벤트 루프 이해하기 — Gaurav Pandvia
- 📜 자바스크립트 콜 스택 이해하기 — Charles Freeborn
- 📜 자바스크립트: 실행 컨텍스트란 무엇인가? 콜 스택이란 무엇인가? — Valentino Gagliardi

<div class="content-ad"></div>

맨 위로

# 2. 기본 유형

![이미지](/assets/img/2024-07-09-33ConceptsEveryJavaScriptDeveloperShouldKnow_3.png)

객체를 제외한 모든 유형은 변경할 수 없는 값(즉, 변경할 수 없는 값)을 정의합니다. 예를 들어, C와 달리 문자열은 변경할 수 없습니다. 이러한 유형의 값을 "기본 값"이라고 합니다.

<div class="content-ad"></div>

자바스크립트에는 6가지의 기본 데이터 유형이 있습니다: 숫자, 문자열, 부울(Boolean), 널(Null), 정의되지 않음(Undefined), 그리고 심볼(Symbol)이 있습니다. 이러한 유형들은 간단한 값들을 나타내며 변경할 수 없으므로 값이 변하지 않습니다. 이러한 유형들이 어떻게 작동하는지 이해하는 것은 효율적이고 버그가 없는 코드를 작성하는 데 매우 중요합니다.

```js
const age = 25; // 숫자(Number)
const name = "John"; // 문자열(String)
const isDeveloper = true; // 부울(Boolean)
let emptyValue = null; // 널(Null)
let notDefined; // 정의되지 않음(Undefined)
```

# 튜토리얼

- 📜 자바스크립트에서 숫자가 인코딩되는 방법 - Dr. Axel Rauschmayer
- 📜 자바스크립트 숫자 유형을 알아야 할 사항 - Max Wizard K
- 📜 모든 자바스크립트 개발자가 부동 소수점 숫자에 대해 알아야 할 사항 - Chewxy

<div class="content-ad"></div>

맨 위로 이동

# 3. 값 유형과 참조 유형

![image](/assets/img/2024-07-09-33ConceptsEveryJavaScriptDeveloperShouldKnow_4.png)

자바스크립트에서 변수는 값 유형 또는 참조 유형을 보관할 수 있습니다. 값 유형에는 숫자, 문자열, 부울 같은 것들이 포함되며, 이들은 데이터를 직접 저장합니다. 참조 유형(객체 및 배열과 같은)은 데이터를 가리키는 참조를 저장합니다.

<div class="content-ad"></div>

```js
// 값 타입
let num1 = 42;
let num2 = num1; // num1의 사본이 num2에 할당됩니다
```

```js
// 참조 타입
let arr1 = [1, 2, 3];
let arr2 = arr1; // arr1과 arr2는 같은 배열을 참조합니다
```

num1을 변경해도 num2에는 영향을 주지 않습니다. 그러나 arr1을 수정하면 arr2도 변경됩니다. 왜냐하면 두 변수가 같은 배열을 가리키기 때문입니다.

# 튜토리얼

<div class="content-ad"></div>

- 📜 자바스크립트에서 값과 참조 설명하기 — Arnav Aggarwal
- 📜 자바스크립트의 기본 유형 및 참조 유형 — Bran van der Meer
- 📜 자바스크립트에서 값 유형, 참조 유형 및 스코프 — Ben Aston

맨 위로

# 4. 암시적, 명시적, 명목, 구조화 및 덕 타이핑

![이미지](/assets/img/2024-07-09-33ConceptsEveryJavaScriptDeveloperShouldKnow_5.png)

<div class="content-ad"></div>

이러한 용어는 프로그래밍 언어의 다른 유형 시스템을 설명하며, 변수와 데이터 유형이 다루어지는 방식에 영향을 줍니다.

```js
// 구조적 유형
interface Shape {
  sides: number;
}
function getArea(shape: Shape) {
  // 구조에 기반한 면적 계산
}
```

이러한 개념은 개발자가 코드에서 변수 할당 및 유형 선언에 접근하는 방식에 영향을 미치며, 프로그래밍 언어의 유형 시스템의 성격을 정의합니다.

# 자습서

<div class="content-ad"></div>

- 📜 자바스크립트의 암시적 강제 변환에 대해 알아야 할 사항 - Promise Tochi
- 📜 자바스크립트 유형 강제 변환 설명 - Alexey Samoshkin
- 📜 자바스크립트 강제 변환 설명 - Ben Garrison

맨 위로

### 5. == vs === vs typeof

![이미지](/assets/img/2024-07-09-33ConceptsEveryJavaScriptDeveloperShouldKnow_6.png)

<div class="content-ad"></div>

자바스크립트에는 두 개의 동등 연산자가 있습니다: == (느슨한 동등)와 === (엄격한 동등). 둘 사이의 차이를 파악하는 것이 중요합니다. 또한 typeof 연산자는 값의 데이터 유형을 결정하는 데 도움이 됩니다. 동적 데이터를 처리할 때 이는 귀중한 도움이 됩니다.

```js
console.log(5 == "5"); // true (느슨한 동등)
console.log(5 === "5"); // false (엄격한 동등)
console.log(typeof "Hello"); // "string'
```

# 튜토리얼

- 📜 자바스크립트 더블 동등 vs. 트리플 동등 — Brandon Morelli
- 📜 자바스크립트에서 === 또는 == 동등 비교 연산자를 사용해야 할까요? — Panu Pitkamaki
- 📜 == vs === 자바스크립트: 더블 동등 및 강제형 변환 — AJ Meyghani

<div class="content-ad"></div>

맨 위로 이동

# 6. 함수 범위, 블록 범위 및 렉시컬 범위

![image](/assets/img/2024-07-09-33ConceptsEveryJavaScriptDeveloperShouldKnow_7.png)

JavaScript는 함수 범위와 블록 범위를 사용합니다. 함수 내에서 선언된 변수는 해당 함수 내에서만 접근할 수 있습니다 (함수 범위). let 및 const로 도입된 블록 범위는 변수를 해당 블록 (중괄호 내)으로 제한합니다.

<div class="content-ad"></div>

```js
// Function Scope
function exampleFunction() {
  let localVar = "I’m local!";
  console.log(localVar);
}
// Block Scope
function blockExample() {
  if (true) {
    let blockVar = "I'm in a block!";
    console.log(blockVar);
  }
  // console.log(blockVar); // Error: blockVar is not defined outside the block
}
```

렉시컬 스코프는 함수가 외부(폐쇄) 스코프에서 변수에 액세스할 수 있다는 것을 의미합니다.

# 튜토리얼

- 📜 JavaScript Functions — 기초 이해 — Brandon Morelli
- 📜 함수 스코프와 블록 스코프 사이의 전투 — Marius Herring
- 📜 JavaScript에서 블록 스코프 모방하기 — Josh Clanton

<div class="content-ad"></div>

맨 위로 가기

# 7. 표현식 대 문장

![Image](/assets/img/2024-07-09-33ConceptsEveryJavaScriptDeveloperShouldKnow_8.png)

표현식은 값이 생성되는 것이며, 반면 문장은 작업을 수행합니다.

<div class="content-ad"></div>

```js
// 표현식
let sum = 3 + 4;
// 문
if (sum === 7) {
  console.log("합이 7입니다.");
}
```

표현식은 if 문 안의 3 + 4와 같이 문의 일부가 될 수 있습니다. 이를 구별하는 것은 코드 구조를 이해하는 데 도움이 됩니다.

# 자습서

- 📜 자바스크립트의 표현식, 문 및 표현식 문에 대해 알아야 할 모든 것 — Promise Tochi
- 📜 함수 표현식 vs 함수 선언 — Paul Wilkins
- 📜 JavaScript 함수 — 선언 vs 표현식 — Ravi Roshan

<div class="content-ad"></div>

맨 위로

# 8. IIFE, 모듈 및 네임스페이스

![이미지](/assets/img/2024-07-09-33ConceptsEveryJavaScriptDeveloperShouldKnow_9.png)

IIFE(즉시 호출 함수 표현식), 모듈 및 네임스페이스는 코드 구성 및 캡슐화에 기여합니다.

<div class="content-ad"></div>

```js
// IIFE (즉시 호출 함수 표현식)
(function () {
  const privateVar = "나는 비공개입니다!";
  console.log(privateVar);
})();
// 모듈
// module.js
export function add(a, b) {
  return a + b;
}
// main.js
import { add } from "./module.js";
console.log(add(2, 3));
// 네임스페이스
const myNamespace = {
  variable: "저는 네임스페이스 안에 있어요",
  log() {
    console.log(this.variable);
  },
};
myNamespace.log();
```

IIFE는 비공개 범위를 보장하고, 모듈은 모듈식 코드 구성을 용이하게 하며, 네임스페이스는 전역 변수 충돌을 방지합니다.

# 튜토리얼

- 📜 즉시 호출 함수 표현식 마스터하기 ― Chandra Gundamaraju
- 📜 ES6 모듈이 IIFE의 필요성을 없애는가?
- 📜 10분 만에 JavaScript 모듈, 모듈 포맷, 모듈 로더 및 모듈 번들러 익히기 ― Jurgen Van de Moere

<div class="content-ad"></div>

맨 위로 가기

9. 메시지 큐와 이벤트 루프

![image](/assets/img/2024-07-09-33ConceptsEveryJavaScriptDeveloperShouldKnow_10.png)

JavaScript는 이벤트 루프를 사용하여 이벤트 기반으로 코드를 실행합니다. 메시지 큐에는 처리할 이벤트가 보관됩니다. 간단한 비동기 예제를 살펴보겠습니다:

<div class="content-ad"></div>

```js
console.log("시작");
setTimeout(() => {
  console.log("타임아웃 내부");
}, 1000);
console.log("끝");
```

setTimeout이 1초로 설정되었지만, 타임아웃 콜백은 메시지 큐로 이동하며, 스택이 비워진 후 이벤트 루프가 처리하기 때문에 끝이 먼저 기록됩니다.

# 튜토리얼

- 📜 자바스크립트 이벤트 루프 설명 — Anoop Raveendran
- 📜 자바스크립트 이벤트 루프: 설명 — Erin Sweson-Healey
- 📜 JS 이해: 이벤트 루프 — Alexander Kondov

<div class="content-ad"></div>

맨 위로 이동

# 10. setTimeout, setInterval 및 requestAnimationFrame

![image](/assets/img/2024-07-09-33ConceptsEveryJavaScriptDeveloperShouldKnow_11.png)

이러한 함수 - setTimeout, setInterval 및 requestAnimationFrame - 는 JavaScript에서 비동기 작업 및 애니메이션을 관리하는 데 필수적입니다.

<div class="content-ad"></div>

```js
// setTimeout
setTimeout(() => {
  console.log("Delayed log");
}, 1000);
// setInterval
const intervalId = setInterval(() => {
  console.log("Repeated log");
}, 2000);
// requestAnimationFrame
function animate() {
  console.log("Animating…");
  requestAnimationFrame(animate);
}
animate();
```

setTimeout은 지정된 지연 시간 후에 함수를 실행하고, setInterval은 고정된 시간 간격으로 함수를 반복 호출하며, requestAnimationFrame은 부드럽고 효율적인 애니메이션에 자주 사용됩니다.

# 자습서

- 📜 setTimeout와 setInterval — JavaScript.Info
- 📜 setInterval을 사용하지 말아야 하는 이유 — Akanksha Sharma
- 📜 setTimeout 대신 setInterval을 사용하는 이유 — Develoger

<div class="content-ad"></div>

맨 위로

# 11. JavaScript 엔진

![JavaScript 엔진](/assets/img/2024-07-09-33ConceptsEveryJavaScriptDeveloperShouldKnow_12.png)

웹을 위한 코드를 작성하는 것은 때때로 조금 마법적인 느낌이 듭니다. 개발자는 일련의 문자를 작성하고 마법처럼 그 문자들이 브라우저 내에서 구체적인 이미지, 단어 및 작업으로 변하는 것을 볼 수 있습니다. 기술을 이해하는 것은 개발자들이 프로그래머로서 자신의 기술을 더 잘 연마할 수 있도록 도와줍니다.
— 출처

<div class="content-ad"></div>

# 튜토리얼

- 📜 JavaScript 엔진 — Jen Looper
- 📜 Chrome V8 엔진이 JavaScript를 기계 코드로 번역하는 방법 이해하기 — DroidHead
- 📜 V8의 바이트 코드 이해하기 — Franziska Hinkelmann

맨 위로 이동

# 12. 비트 연산자, 타입 배열 및 배열 버퍼

<div class="content-ad"></div>

![이미지](/assets/img/2024-07-09-33ConceptsEveryJavaScriptDeveloperShouldKnow_13.png)

컴퓨터에게 있어서 기술적으로 모든 것은 1과 0으로 나누어집니다. 그것은 숫자나 문자 또는 문자열로 작동하지 않습니다. 이진 숫자(비트)만 사용합니다. 이 설명의 간단한 버전은 모든 것이 이진 형태로 저장된다는 것입니다. 그런 다음 컴퓨터는 UTF-8과 같은 인코딩을 사용하여 저장된 비트 조합을 문자, 숫자 또는 다른 기호에 매핑합니다(ELI5 버전).
— 출처

# 튜토리얼

- 📜 JS로 프로그래밍: 비트 연산 — Alexander Kondov
- 📜 실제 생활에서 JavaScript의 비트 연산자 사용 — ian m
- 📜 JavaScript 비트 연산자 — w3resource

<div class="content-ad"></div>

맨 위로 이동

# 13. DOM 및 레이아웃 트리

![image](/assets/img/2024-07-09-33ConceptsEveryJavaScriptDeveloperShouldKnow_14.png)

문서 객체 모델(Document Object Model), 일반적으로 DOM이라고 불리는 것은 웹사이트를 상호작용 가능하게 만드는 중요한 부분입니다. 이는 프로그래밍 언어가 웹사이트의 내용, 구조 및 스타일을 조작할 수 있게 하는 인터페이스입니다. JavaScript는 인터넷 브라우저에서 DOM에 연결되는 클라이언트 측 스크립팅 언어입니다.

<div class="content-ad"></div>

```js
// 필요한 DOM 요소에 대한 참조 가져오기
const commentsListElement = document.getElementById("commentsList");
const postCommentButton = document.getElementById("postComment");

// "댓글 작성" 버튼에 이벤트 리스너 추가
postCommentButton.addEventListener("click", function () {
  // 새로운 댓글 항목 생성
  const comment = document.createElement("li");
  comment.textContent = "새 댓글";

  // 새 댓글을 댓글 목록에 추가
  commentsListElement.appendChild(comment);
});
```

# 튜토리얼

- 📜 JavaScript에서 DOM 이해 및 수정하는 방법 — Tania Rascia
- 📜 문서 객체 모델(Document Object Model)이란 무엇이며 왜 사용하는지 알아야하는 이유 — Leonardo Maldonado
- 📜 예제와 함께 알아보는 JavaScript DOM 튜토리얼 — Guru99

맨 위로 이동

<div class="content-ad"></div>

# 14. 팩토리 및 클래스

![이미지](/assets/img/2024-07-09-33ConceptsEveryJavaScriptDeveloperShouldKnow_15.png)

자바스크립트에는 객체를 생성하는 두 가지 주요 방법이 있습니다: 팩토리와 클래스. 팩토리는 객체를 생성하고 반환하는 함수이며, 클래스는 더 구조화되고 객체 지향적인 청사진을 제공합니다.

```js
// 팩토리
function createPerson(name, age) {
  return {
    name,
    age,
    greet() {
      console.log(`안녕하세요, 제 이름은 ${this.name}입니다.`);
    },
  };
}
// 클래스
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
  greet() {
    console.log(`안녕하세요, 제 이름은 ${this.name}입니다.`);
  }
}
const person1 = createPerson("John", 25);
const person2 = new Person("Alice", 30);
person1.greet();
person2.greet();
```

<div class="content-ad"></div>

두 가지 접근 방식은 동일한 결과를 얻을 수 있지만, 클래스는 특히 상속이 포함된 시나리오에서 객체를 보다 구조화되고 전통적인 방식으로 정리하는 데 더 좋습니다.

# 튜토리얼

- 📜 JavaScript에서 클래스 사용 방법 — Tania Rascia
- 📜 JavaScript 클래스 — Under The Hood — Majid
- 📜 ES6 클래스 — Nathaniel Foster

맨 위로 돌아가기

<div class="content-ad"></div>

# 15. this, call, apply 및 bind

![이미지](/assets/img/2024-07-09-33ConceptsEveryJavaScriptDeveloperShouldKnow_16.png)

자바스크립트에서, call, apply 및 bind는 함수 내의 this 키워드 값을 제어하는 데 사용되는 메서드입니다. 이를 통해 함수가 실행되어야 하는 문맥을 지정하는 데 유연성을 제공합니다.

```js
const person = {
  name: "John",
  greet(message) {
    console.log(`${message}, ${this.name}!`);
  },
};
const anotherPerson = {
  name: "Alice",
};
// call
person.greet.call(anotherPerson, "Hello");
// apply
person.greet.apply(anotherPerson, ["Hi"]);
// bind
const greetAlice = person.greet.bind(anotherPerson, "Hey");
greetAlice();
```

<div class="content-ad"></div>

이러한 방법들은 함수를 호출할 때 this의 값을 명시적으로 설정하고 싶은 경우에 특히 유용합니다.

# 자습서

- 📜 자바스크립트의 call(), apply(), bind() 메서드에 대해 알아보기 - Aniket Kudale
- 📜 자바스크립트에서 call(), apply(), bind() 활용하기 - Niladri Sekhar Dutta
- 📜 자바스크립트의 Apply, Call, Bind 메서드는 자바스크립트 전문가에게 꼭 필요합니다 - Richard Bovell

맨 위로 이동하기

<div class="content-ad"></div>

# 16. 새로운, 생성자, instanceof 및 인스턴스

![이미지](/assets/img/2024-07-09-33ConceptsEveryJavaScriptDeveloperShouldKnow_17.png)

모든 JavaScript 객체는 프로토타입을 가지고 있습니다. JavaScript의 모든 객체는 메서드와 속성을 자신의 프로토타입으로부터 상속받습니다.
— 출처

# 튜토리얼

<div class="content-ad"></div>

- 📜 자바스크립트 초보자를 위한: 'new' 연산자 — Brandon Morelli
- 📜 자바스크립트의 'new' 키워드를 해체해보자 — Cynthia Lee
- 📜 생성자, 연산자 "new" — JavaScript.Info

맨 위로

# 17. 프로토타입 상속 및 프로토타입 체인

![이미지](/assets/img/2024-07-09-33ConceptsEveryJavaScriptDeveloperShouldKnow_18.png)

<div class="content-ad"></div>

JavaScript은 프로토타입 기반 상속을 사용하며, 객체들은 프로토타입 체인을 통해 다른 객체로부터 속성과 메서드를 상속 받을 수 있어요.

```js
// 프로토타입
function Animal(name) {
  this.name = name;
}
Animal.prototype.sound = function () {
  console.log("어떤 일반적인 소리");
};
// 상속
function Dog(name, breed) {
  Animal.call(this, name);
  this.breed = breed;
}
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;
Dog.prototype.sound = function () {
  console.log("왈왈!");
};
const myDog = new Dog("버디", "래브라도르");
myDog.sound(); // 왈왈!
```

효율적이고 재사용 가능한 코드 구조를 JavaScript에서 만들기 위해 프로토타입 상속을 이해하는 것이 중요해요.

# 자습서

<div class="content-ad"></div>

- 📜 자바스크립트 : 프로토타입 vs 클래스 — Valentin PARSY
- 📜 자바스크립트 엔진 기본: 프로토타입 최적화 — Mathias Bynens
- 📜 자바스크립트 프로토타입 — NC Patro

맨 위로 이동

# 18. Object.create와 Object.assign

![그림](/assets/img/2024-07-09-33ConceptsEveryJavaScriptDeveloperShouldKnow_19.png)

<div class="content-ad"></div>

The Object.create method is one of the methods to create a new object in JavaScript.
— Source

# Tutorials

- 📜 Object.create in JavaScript — Rupesh Mishra
- 📜 Object.create(): the New Way to Create Objects in JavaScript — Rob Gravelle
- 📜 Basic Inheritance with Object.create — Joshua Clanton

Back To Top

<div class="content-ad"></div>

# 19. map, reduce, filter

![Image](/assets/img/2024-07-09-33ConceptsEveryJavaScriptDeveloperShouldKnow_20.png)

함수형 프로그래밍이 무엇인지 몰라도 아마 map, filter 및 reduce를 사용해 보았을 것입니다. 이들은 매우 유용하며 더 깔끔한 로직을 작성할 수 있게 해 주어 코드를 덜 구릴게 만듭니다.
— 출처

# 튜토리얼

<div class="content-ad"></div>

- 📜 자바스크립트 함수형 프로그래밍 — map, filter 및 reduce — 보잔 고보자락
- 📜 자바스크립트에서 map, filter 및 reduce 배우기 — 조앙 미겔 쿤하
- 📜 자바스크립트의 Map, Reduce 및 Filter — 댄 마텐센

맨 위로

# 20. 순수 함수, 사이드 이펙트, 상태 변경 및 이벤트 전파

<img src="/assets/img/2024-07-09-33ConceptsEveryJavaScriptDeveloperShouldKnow_21.png" />

<div class="content-ad"></div>

우리의 많은 버그는 IO 관련, 데이터 변이, 부작용을 내포한 코드에서 발생합니다. 이러한 문제들은 사용자 입력을 받는 것, http 호출을 통해 예기치 않은 응답을 받는 것, 또는 파일 시스템에 쓰는 것과 같은 곳에서 발생합니다. 불행하게도, 우리는 이 문제에 익숙해져야 할 현실에 직면하고 있습니다. 그렇거나 아닐까요?
— 출처

# 튜토리얼

- 📜 자바스크립트와 함수형 프로그래밍 — 순수 함수 — Omer Goldberg
- 📜 자바스크립트 인터뷰에서 마스터해라: 순수 함수란 무엇인가? — Eric Elliott
- 📜 자바스크립트: 순수 함수란 무엇이며 왜 사용해야 하는가? — James Jeffery

맨 위로 가기

<div class="content-ad"></div>

# 21. 클로저

![이미지](/assets/img/2024-07-09-33ConceptsEveryJavaScriptDeveloperShouldKnow_22.png)

클로저는 외부 범위에서 변수에 대한 액세스를 유지하는 함수가 외부 함수의 실행이 끝난 후에도 발생합니다. 이 컨셉은 프라이빗 변수를 생성하고 함수 호출 간에 상태를 유지하는 데 중요합니다.

```js
function outerFunction() {
  let outerVar = "바깥에서 왔어요!";
  function innerFunction() {
    console.log(outerVar);
  }
  return innerFunction;
}
const closureExample = outerFunction();
closureExample(); // 출력: 바깥에서 왔어요!
```

<div class="content-ad"></div>

이 예제에서 innerFunction은 outerVar을 닫아서 outerFunction의 실행이 완료된 후에도 outerVar에 액세스할 수 있게 합니다.

# 자습서

- 📜 나는 JavaScript 클로저를 이해하지 못했다 — Olivier De Meulder
- 📜 쉽게 이해하는 JavaScript 클로저 — Richard Bovell
- 📜 JavaScript 클로저 이해하기 — Codesmith

위로 이동

<div class="content-ad"></div>

# 22. 고차 함수

![이미지](/assets/img/2024-07-09-33ConceptsEveryJavaScriptDeveloperShouldKnow_23.png)

고차 함수는 다른 함수를 인수로 사용하거나 함수를 반환하는 함수입니다. 이러한 함수들은 함수형 프로그래밍 스타일을 촉진하여 코드의 가독성과 유지 관리성을 향상시킵니다.

```js
const numbers = [1, 2, 3, 4, 5];
// Map - 배열의 각 요소에 함수를 적용합니다
const squared = numbers.map((num) => num * num);
// Filter - 조건을 만족하는 요소로 이루어진 새 배열을 만듭니다
const evenNumbers = numbers.filter((num) => num % 2 === 0);
// Reduce - 배열을 단일 값으로 줄입니다
const sum = numbers.reduce((acc, num) => acc + num, 0);
console.log(squared); // [1, 4, 9, 16, 25]
console.log(evenNumbers); // [2, 4]
console.log(sum); // 15
```

<div class="content-ad"></div>

고차 함수를 사용하면 배열 및 데이터 변환 작업에 간결하고 표현력 있는 코드를 구현할 수 있습니다.

# 튜토리얼

- 📜 자바스크립트에서의 고차 함수 - M. David Green
- 📜 고차 함수: Filter, Map 및 Reduce를 사용하여 유지보수가 쉬운 코드 작성하기 - Guido Schmitz
- 📜 1급 및 고차 함수: 효과적인 함수형 자바스크립트 - Hugo Di Francesco

맨 위로 이동

<div class="content-ad"></div>

# 23. 재귀

![이미지](/assets/img/2024-07-09-33ConceptsEveryJavaScriptDeveloperShouldKnow_24.png)

자바스크립트는 배열과 세트와 같은 다양한 컬렉션을 제공하여 여러 값을 저장하고 관리할 수 있습니다. 게다가, 제너레이터는 일시 중지되고 재개될 수 있는 특별한 함수로, 보다 유연하고 효율적인 반복이 가능합니다.

```js
// 배열
const colors = ["red", "green", "blue"];
// 세트
const uniqueColors = new Set(colors);
// 제너레이터
function* generateNumbers() {
  yield 1;
  yield 2;
  yield 3;
}
const numbersGenerator = generateNumbers();
console.log([...uniqueColors]); // ["red", "green", "blue"]
console.log([...numbersGenerator]); // [1, 2, 3]
```

<div class="content-ad"></div>

Collections은 데이터의 조직화와 조작을 용이하게 하고, generators는 순회 가능한 시퀀스를 생성하는 데 도움이 되어, 특히 대량의 데이터 세트를 나태로 로딩하거나 처리하는 데 유용합니다.

# 튜토리얼

- 📜 ES6 In Depth: Collections — Jason Orendorff
- 📜 ES6 Collections: Using Map, Set, WeakMap, WeakSet — Kyle Pennell
- 📜 ES6 WeakMaps, Sets, and WeakSets in Depth — Nicolás Bevacqua

맨 위로 이동

<div class="content-ad"></div>

# 24. 콜렉션 및 제너레이터

![이미지](/assets/img/2024-07-09-33ConceptsEveryJavaScriptDeveloperShouldKnow_25.png)

자바스크립트는 여러 값을 저장하고 관리하는 데 사용되는 배열 및 세트와 같은 다양한 콜렉션을 제공합니다. 또한 제너레이터는 일시 중지 및 재개할 수 있는 특별한 함수로, 더 유연하고 효율적인 반복이 가능합니다.

```js
// 배열
const colors = ["빨강", "초록", "파랑"];
// 세트
const uniqueColors = new Set(colors);
// 제너레이터
function* generateNumbers() {
  yield 1;
  yield 2;
  yield 3;
}
const numbersGenerator = generateNumbers();
console.log([...uniqueColors]); // ["빨강", "초록", "파랑"]
console.log([...numbersGenerator]); // [1, 2, 3]
```

<div class="content-ad"></div>

컬렉션은 데이터의 구성 및 조작을 용이하게 하며, 제너레이터는 반복 가능한 시퀀스를 생성하는 데 도움을 줍니다. 특히, 대량 데이터셋의 지연 로딩이나 처리에 유용합니다.

튜토리얼

- 📜 ES6 In Depth: Collections — Jason Orendorff
- 📜 ES6 Collections: Using Map, Set, WeakMap, WeakSet — Kyle Pennell
- 📜 ES6 WeakMaps, Sets, and WeakSets in Depth — Nicolás Bevacqua

맨 위로 이동

<div class="content-ad"></div>

# 25. Promises

![ConceptsEveryJavaScriptDeveloperShouldKnow](/assets/img/2024-07-09-33ConceptsEveryJavaScriptDeveloperShouldKnow_26.png)

콜백의 개념을 이해했지만, 콜백 안에 콜백이 들어가고 또 그 안에 콜백이 들어가고 계속 이어지면 어떻게 될까요? 이러한 재귀적인 콜백 구조를 '콜백 지옥'이라고 하며, 이런 문제를 해결하기 위해 Promise가 도와줍니다. Promise는 두 가지 이상의 연속적인 작업을 수행해야 할 때 유용하며, 각 후속 함수는 이전 함수가 완료된 후에 시작됩니다. Promise는 나중에 어떤 시점에 단일 값을 생성할 수 있는 객체로, 해결된 값 또는 해결되지 않은 이유(거부된 값)가 될 수 있습니다. 개발자.모질라에 따르면 "Promise는 비동기 작업의 최종 완료 또는 실패을 나타내는 객체입니다. 본질적으로 Promise는 콜백을 함수로 전달하는 대신 콜백을 첨부하는 반환된 객체입니다.". Promise는 '콜백 지옥'이라 불리는 문제를 해결하며, 이는 결국 콜백의 재귀적인 구조(콜백 안에서 콜백이 계속 중첩되는 형태)를 의미합니다.

Promise는 세 가지 상태일 수 있습니다…

- 완료됨(Fulfilled): 작업이 성공적으로 완료될 때.
- 거부됨(Rejected): 작업이 실패했을 때.
- 대기 중(Pending): 초기 상태로, 아직 완료되지 않았거나 거부되지 않은 상태.

<div class="content-ad"></div>

예시: 자바스크립트에서 프로미스를 생성하는 방법에 대해 예제로 알아봅시다.

```js
const promise = new Promise((resolve, reject) => {
  isNameExist = true;
  if (isNameExist) {
    resolve("사용자 이름이 이미 존재합니다");
  } else {
    reject("에러");
  }
});
promise
  .then((result) => console.log(result))
  .catch(() => {
    console.log("에러 발생!");
  });
```

출력:

```js
사용자 이름이 이미 존재합니다
Promise {<resolved>: undefined}
```

<div class="content-ad"></div>

위의 코드는 'isNameExist' 작업을 비동기적으로 수행하는 샘플 promise로 가정합니다. 이 promise 객체에는 resolve와 reject 두 함수가 인수로 전달됩니다. 작업이 성공하면 'isNameExist'가 'true'인 것이므로 해당 내용이 해결됩니다. 이 경우 출력은 "사용자 이름이 존재합니다"가 됩니다. 그렇지 않으면 작업이 실패하거나 거부되어 'error !' 결과를 표시합니다. Promises에서 체이닝 작업을 쉽게 수행할 수 있습니다. 체이닝 작업에서는 첫 번째 작업이 실행되고 첫 번째 작업의 결과가 두 번째 작업으로 전달되며 계속 진행됩니다.

# 자습서

- 📜 JavaScript Promises for Dummies ― Jecelyn Yeen
- 📜 Understanding promises in JavaScript — Gokul N K
- 📜 Master the JavaScript Interview: What is a Promise? — Eric Elliott

맨 위로 이동

<div class="content-ad"></div>

# 26. async/await

![async/await](/assets/img/2024-07-09-33ConceptsEveryJavaScriptDeveloperShouldKnow_27.png)

뭔가 해결될 때까지 기다렸다가 멈춥니다. Async & await은 Promise 위에 달린 문법적 설탕일 뿐이며, Promise처럼 비동기 작업을 더 동기적으로 유지하는 방법을 제공합니다. 그래서 JavaScript에서 비동기 작업을 처리할 수 있는 다양한 버전이 있습니다...

- ES5 - Callback
- ES6 - Promise
- ES7 - async & await

<div class="content-ad"></div>

테이블 태그를 마크다운 형식으로 바꿀 수 있습니다.

<div class="content-ad"></div>

![ConceptsEveryJavaScriptDeveloperShouldKnow](/assets/img/2024-07-09-33ConceptsEveryJavaScriptDeveloperShouldKnow_28.png)

약속(Promises)를 다루고 있다는 것을 JS에 알려주기 위해서는 ‘await’를 ‘async’ 함수 안에 감싸야 합니다. 위 예시에서, 우리는 두 가지를 기다립니다: 응답(response)과 게시물(posts). 응답을 JSON 형식으로 변환하기 전에, 먼저 응답을 가져온 것을 확인해야 합니다. 그렇지 않으면 아직 가져오지 못한 응답을 변환하게 되어 에러가 발생할 확률이 높습니다.

# 튜토리얼

- 📜 자바스크립트에서 async/await 이해하기 — Gokul N K
- 📜 자바스크립트에서 Async/Await 함수 탐색하기 — Alligator.io
- 📜 async/await을 사용한 비동기 자바스크립트 — Joy Warugu

<div class="content-ad"></div>

맨 위로 돌아가기

# 27. 자료 구조

![이미지](/assets/img/2024-07-09-33ConceptsEveryJavaScriptDeveloperShouldKnow_29.png)

자바스크립트는 매일 발전하고 있습니다. React, Angular, Vue, NodeJS, Electron, React Native과 같은 프레임워크 및 플랫폼의 급속한 성장으로 대규모 애플리케이션에 자바스크립트를 사용하는 것이 일반적해졌습니다.
— 출처

<div class="content-ad"></div>

# 튜토리얼

- 📜 자바스크립트에서의 데이터 구조 - Thon Ly
- 📜 자바스크립트에서의 알고리즘 및 데이터 구조 - Oleksii Trekhleb
- 📜 데이터 구조: 객체와 배열 - Chris Nwamba

맨 위로 가기

# 28. 비용이 많이 드는 작업과 빅 O 표기법

<div class="content-ad"></div>

![이미지](/assets/img/2024-07-09-33ConceptsEveryJavaScriptDeveloperShouldKnow_30.png)

“빅 오 표기법이란 무엇인가?”는 개발자들에게 매우 흔한 면접 질문입니다. 간단히 말하면, 입력값의 길이에 따라 알고리즘이 실행되는 데 얼마나 시간이 걸리는지를 수학적 표현한 것으로, 대개 최악의 경우 시나리오에 대해 이야기합니다.
— 출처

# 튜토리얼

- 📜 자바스크립트에서의 빅 오 표기법 — 세사르 안톤 도란테스
- 📜 시간 복잡도/빅 오 표기법 — 팀 로버츠
- 📜 자바스크립트의 빅 오 — 가브리엘라 메디나

<div class="content-ad"></div>

맨 위로 이동

# 29. 알고리즘

![이미지](/assets/img/2024-07-09-33ConceptsEveryJavaScriptDeveloperShouldKnow_31.png)

수학과 컴퓨터 과학에서 알고리즘은 주로 특정 문제 집합을 해결하거나 계산을 수행하는 데 사용되는 유한한 일련의 명확하게 정의된 지시문입니다.

<div class="content-ad"></div>

# 튜토리얼

- 📜 ES6를 활용한 데이터 구조 및 알고리즘
- 📜 설명 및 추가 독해 링크와 함께 자바스크립트로 구현된 알고리즘 및 데이터 구조
- 📜 JS: 인터뷰 알고리즘

위로 이동

# 30. 상속, 다형성 및 코드 재사용

<div class="content-ad"></div>

![Image](/assets/img/2024-07-09-33ConceptsEveryJavaScriptDeveloperShouldKnow_32.png)

클래스 상속은 한 클래스가 다른 클래스를 확장할 수 있는 방법으로, 기존의 기능 위에 새로운 기능을 만들 수 있습니다.
— 출처

JavaScript는 프로토타입 기반 상속을 사용하며, 객체가 프로토타입 체인을 통해 다른 객체로부터 속성 및 메소드를 상속할 수 있습니다.

```js
// 프로토타입
function Animal(name) {
  this.name = name;
}
Animal.prototype.sound = function () {
  console.log("일반적인 소리");
};
// 상속
function Dog(name, breed) {
  Animal.call(this, name);
  this.breed = breed;
}
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;
Dog.prototype.sound = function () {
  console.log("왈왈!");
};
const myDog = new Dog("버디", "라브라도");
myDog.sound(); // 왈왈!
```

<div class="content-ad"></div>

자바스크립트에서 프로토타입 상속을 이해하는 것은 효율적이고 재사용 가능한 코드 구조를 만드는 데 중요합니다.

# 튜토리얼

- 📜 자바스크립트 상속 — Rupesh Mishra
- 📜 자바스크립트로 간단한 상속 — David Catuhe
- 📜 자바스크립트 — 상속, 위임 패턴 및 객체 연결 — NC Patro

맨 위로 가기

<div class="content-ad"></div>

# 31. 디자인 패턴

![image](/assets/img/2024-07-09-33ConceptsEveryJavaScriptDeveloperShouldKnow_33.png)

모든 개발자는 유지보수가 용이하고 가독성이 좋으며 재사용 가능한 코드를 작성하기 위해 노력합니다. 애플리케이션이 커짐에 따라 코드 구조화가 더욱 중요해집니다. 디자인 패턴은 이러한 도전을 해결하는 데 중요한 역할을 합니다 — 특정 상황에서 공통 문제에 대한 조직 구조를 제공합니다.
— 출처

# 튜토리얼

<div class="content-ad"></div>

- 📜 4 JavaScript Design Patterns You Should Know — Devan Patel
- 📜 JavaScript Design Patterns — Beginner’s Guide to Mobile Web Development — Soumyajit Pathak
- 📜 JavaScript Design Patterns — Akash Pal

Back To Top

# 32. Partial Applications, Currying, Compose and Pipe

![ConceptsEveryJavaScriptDeveloperShouldKnow](/assets/img/2024-07-09-33ConceptsEveryJavaScriptDeveloperShouldKnow_34.png)

<div class="content-ad"></div>

함수 합성은 여러 간단한 함수를 결합하여 더 복잡한 함수를 만드는 메커니즘입니다. — 출처

# 튜토리얼

- 📜 JavaScript에서 함수 합성 사용하기 — Rémi
- 📜 JavaScript ES6에서 커링하기 — Adam Bene
- 📜 JavaScript에서 합성과 커링의 우아함 — Pragyan Das

맨 위로 이동

<div class="content-ad"></div>

# 33. Clean Code

![Concept Image](/assets/img/2024-07-09-33ConceptsEveryJavaScriptDeveloperShouldKnow_35.png)

깨끗하고 이해하기 쉽고 유지보수가 용이한 코드를 작성하는 것은 모든 개발자가 습득해야 하는 중요한 기술입니다.
— 출처

# 튜토리얼

<div class="content-ad"></div>

- 📜 Clean Code Explained — A Practical Introduction to Clean Coding for Beginners — freeCodeCamp
- 📜 Clean Code concepts adapted for JavaScript — Ryan McDermott
- 📜 Clean Code Practice: How to write clean code — Tirth Bodawala

Back To Top

If you liked the article, show your support with a clap 👏 and follow me! Feel free to highlight your favorite parts too. Your engagement keeps me inspired!
