---
title: "- 최신 JavaScript this 키워드 완벽 가이드- 2024년 최신 this 키워드 사용법- JavaScript this 키워드 이해와 활용법- JavaScript this 키워드 심층 분석- this 키워드와 다른 컨텍스트 비교- 알아두면 좋은 this 키워드 팁 5가지"
description: ""
coverImage: "/assets/img/2024-07-07-Letsrevisethisthiskeyword_0.png"
date: 2024-07-07 02:04
ogImage:
  url: /assets/img/2024-07-07-Letsrevisethisthiskeyword_0.png
tag: Tech
originalTitle: "Let’s revise this — this keyword"
link: "https://medium.com/@zest.vivek/lets-revise-this-this-keyword-18da0e99e2e2"
---

저희는 코딩 여정에서 모두 이것을 어딘가에서 만난 적이 있을 것이고, 때로는 this 키워드의 개념을 이해하기가 조금 까다로울 수 있습니다. 이를 쉽게 이해하고 기억할 수 있도록 쪼개 보겠습니다.

![이미지](/assets/img/2024-07-07-Letsrevisethisthiskeyword_0.png)

JS에서 this가 함수를 어떻게 호출하는지에 따라 달라진다는 마지막 줄에 주목해 주세요.

# 1. 전역 컨텍스트

<div class="content-ad"></div>

글로벌 실행 컨텍스트(어떤 함수 내에서도 아닌 것)에서 this는 전역 객체를 가리킵니다. 웹 브라우저에서, 전역 객체는 window입니다.

```js
console.log(this); // 브라우저에서는 window 객체를 로깅합니다.
```

# 2. 함수 컨텍스트

일반 함수 내에서 this를 사용할 때, 그 값은 함수가 호출되는 방식에 따라 달라집니다:

<div class="content-ad"></div>

- 간단한 함수 호출: 객체 참조 없이 함수를 호출할 때, 이것은 전역 객체를 가리킵니다 (비 엄격 모드에서) 또는 정의되지 않음 (엄격 모드에서).

```js
function simpleFunction() {
  console.log(this);
}
simpleFunction(); // 비 엄격 모드에서 window를 기록하고, 엄격 모드에서는 정의되지 않음을 기록합니다
```

2. 메서드 호출: 함수가 객체의 메서드로 호출될 때, 이것은 호출된 메서드가 속한 객체를 가리킵니다.

```js
const obj = {
  method: function () {
    console.log(this);
  },
};

obj.method(); // obj 객체를 기록합니다
```

<div class="content-ad"></div>

퀴즈 시간입니다! 다음 코드를 실행했을 때 출력되는 결과는 무엇일까요?

```js
const obj = {
  method: function () {
    console.log(this);
  },
};

const myFunc = obj.method;

myFunc();

// 힌트: 함수 컨텍스트에서의 간단한 함수 호출을 다시 읽어보세요
```

만약 답이 `obj.method() // obj 객체를 로깅한다`라면, 죄송합니다. 정답은: 전역 객체인 window를 로깅합니다. 왜냐하면 함수 호출은 전역 컨텍스트에서 이루어지며, 브라우저에서 코드가 실행될 경우 window 객체를 로깅합니다.

이제 한번 해보세요!

<div class="content-ad"></div>

```js
const employee = {
  name: "vivek",
  method: function () {
    console.log(this.name);
  },
};

const myFunc = employee.method;

// try to solve obj.method() vs myFunc();

employee.method(); // vivek
myFunc(); // vivek
```

# 3. Constructor Functions

When a function is used as a constructor with the new keyword, this refers to the new instance of the object created by the constructor.

```js
function Person(name) {
  this.name = name;
}

const person1 = new Person("Alice");
console.log(person1.name); // Logs 'Alice'
```

<div class="content-ad"></div>

# 4. 화살표 함수

화살표 함수는 자체적인 this 컨텍스트가 없습니다. 대신에 주변 렉시컬 컨텍스트로부터 this를 상속받습니다. 이는 화살표 함수 내부에서 this의 값이 화살표 함수 바깥의 값과 동일하다는 것을 의미합니다.

```js
const obj = {
  regularFunction: function () {
    console.log(this); // obj 객체를 기록합니다

    const arrowFunction = () => {
      console.log(this); // regularFunction에서 상속된 obj 객체를 기록합니다
    };

    arrowFunction();
  },
};

obj.regularFunction();
```

# 5. 명시적인 바인딩

<div class="content-ad"></div>

JavaScript은 `this` 값을 명시적으로 설정하는 방법을 제공합니다:

- call: 주어진 this 값 및 개별적으로 제공된 인수로 함수를 호출합니다.
- apply: 주어진 this 값 및 배열로 제공된 인수로 함수를 호출합니다.
- bind: 호출될 때 제공된 값으로 this 값을 설정한 새로운 함수를 반환합니다.

```js
function greet() {
  console.log(this.name);
}

const person = { name: "Alice" };
greet.call(person); // 'Alice'를 출력합니다
greet.apply(person, []); // 'Alice'를 출력합니다
const greetPerson = greet.bind(person);
greetPerson(); // 'Alice'를 출력합니다
```

# 6. 이벤트 핸들러

<div class="content-ad"></div>

이벤트 핸들러에서 "this"는 이벤트를 받은 요소를 가리킵니다.

```js
const button = document.querySelector("button");
button.addEventListener("click", function () {
  console.log(this); // 버튼 요소를 로그합니다
});
```

# 개요

- 전역 컨텍스트: "this"는 전역 객체를 가리킵니다 (브라우저에서는 window).
- 함수 컨텍스트: 호출된 방식에 따라 "this"가 정해집니다 (비엄격 모드에서 전역 객체, 엄격 모드에서는 undefined 또는 호출된 메소드에 대상이 됩니다).
- 생성자 함수: "this"는 생성자에 의해 생성된 새로운 인스턴스를 가리킵니다.
- 화살표 함수: "this"는 주변 렉시컬 컨텍스트를 상속합니다.
- 명시적 바인딩: "this"는 call, apply 및 bind를 사용하여 명시적으로 설정할 수 있습니다.
- 이벤트 핸들러: "this"는 이벤트를 받은 요소를 가리킵니다.

<div class="content-ad"></div>

```js
const employee = {
  name: 'vivek',
  methodConstructor: function() {
    console.log(this.name);
  },
  methodArrow: () => {
    console.log(this.name);
  }
};

const myFuncConstructor = employee.methodConstructor;
const myFuncArrow = employee.methodArrow;
employee.methodConstructor(); // ??
myFuncConstructor(); // ??
employee.methodArrow(); // ??
myFuncArrow(); ??
```

```js
var myself = {
  name: "Tony Stark",
  whoAmI: function (jobs) {
    jobs.forEach(function (person) {
      // this points to the global object
      console.log(`${this.name} is ${person}`);
    });
  },
};
myself.whoAmI(["Iron man", "Avenger", "the best avenger"]);

// Try to correct this
```
