---
title: "자바스크립트 객체를 생성하는 7가지 방법"
description: ""
coverImage: "/assets/img/2024-07-09-2JavaScriptSevenwaystocreateobjects_0.png"
date: 2024-07-09 13:19
ogImage:
  url: /assets/img/2024-07-09-2JavaScriptSevenwaystocreateobjects_0.png
tag: Tech
originalTitle: "2. JavaScript: Seven ways to create objects"
link: "https://medium.com/@opensrc0/7-ways-to-create-objects-in-javascript-19f0fa9d4ce3"
---

## JavaScript에서 객체 생성의 심층 탐색

![JavaScript에서 객체를 생성하는 7가지 방법](/assets/img/2024-07-09-2JavaScriptSevenwaystocreateobjects_0.png)

JavaScript에서 객체를 생성하는 7가지 방법:

- 객체 생성자
- 리터럴 표기법
- 팩토리 함수
- 함수 생성자
- 프로토타입
- 함수/프로토타입 조합
- 싱글톤

<div class="content-ad"></div>

## 1. 객체 생성자:

```js
var person = new Object();
person.name = "Diego";
person.getName = function () {
  return this.name;
};
```

## 2. 리터럴 표기법

```js
var person = {
  name: "Diego",
  getName: function () {
    return this.name;
  },
};
```

<div class="content-ad"></div>

3. 팩토리 함수

팩토리 함수는 유사한 객체를 생성하는 논리를 캡슐화하고 재사용할 수 있게 합니다. 이를 위해 이전에 생성한 구성 요소 중 하나를 활용합니다. 아래 코드를 참조해주세요:

```js
var newPerson = function (name) {
  var result = new Object();
  result.name = name;
  result.getName = function () {
    return this.name;
  };
  return result;
};

var personOne = newPerson("Diego");
var personTwo = newPerson("Gangelo");

console.log(personOne.getName()); // 결과: Diego
console.log(personTwo.getName()); // 결과: Gangelo
```

<div class="content-ad"></div>

```js
var newPerson = function(name) {
  return {
    person.name : name,
    person.getName : function() {
      return this.name;
    };
};

var personOne = newPerson("Diego");
var personTwo = newPerson("Gangelo");

console.log(personOne.getName()); // prints Diego
console.log(personTwo.getName()); // prints Gangelo
```

4. Function Constructor

자바스크립트에서는 어떤 함수 앞에 new 연산자를 붙여서 호출할 수 있습니다. 함수 F가 주어졌을 때, new F()를 실행하면 새로운 빈 객체 X가 생성됩니다. X가 F의 컨텍스트로 설정되므로 F 내에서 this는 X를 가리킵니다. 마지막으로 X가 F의 결과로 반환됩니다.

```js
function Person(name) {
  this.name = name;
  this.getName = function() {
    return this.name;
  };
};

var personOne = new Person("Diego");

console.log(personOne.getName()); // Diego 출력
console.log(personOne instanceOf Person); // true 출력
console.log(personOne.constructor === Person); // true 출력
console.log(personOne instanceOf Object); // true 출력
```

<div class="content-ad"></div>

### 5. 프로토타입

자바스크립트에서 함수는 매우 특별합니다. 함수는 객체이며, 다른 객체를 생성할 수 있습니다. 그리고 자동으로 prototype이라는 필드를 가집니다. 프로토타입은 생성자(constructor)라는 하나의 필드를 가진 단순한 객체입니다. 이 특별한 점은 함수를 통해 생성된 모든 객체가 해당 함수의 프로토타입을 상속한다는 것입니다.

```js
function Person() {}

Person.prototype.name = "Diego";

var personOne = new Person();
var personTwo = new Person();

console.log(personOne.constructor == Person); // true
console.log(personOne.name); // Diego
console.log(personTwo.constructor == Person); // true
console.log(personTwo.name); // Diego
```

### 6. 함수/프로토타입 조합

<div class="content-ad"></div>

함수/프로토타입 조합은 두 가지 접근 방식을 모두 활용합니다 :) view plainprint?

```js
function Person(name) {
  this.name = name;
}

Person.prototype.getName = function () {
  return this.name;
};

var personOne = new Person("Diego");
var personTwo = new Person("Filippo");

console.log(personOne.getName()); // Diego 출력
console.log(personTwo.getName()); // Filippo 출력
console.log(personOne.getName === personTwo.getName); // true 출력
```

7. 싱글톤

가끔 특정 클래스의 단 하나의 인스턴스만 존재하는 것을 보장하고 싶을 때가 있습니다. JavaScript의 싱글톤을 만드는 것은 간단합니다. 생성자를 정의하고 동시에 호출하기만 하면 됩니다. view plainprint?

<div class="content-ad"></div>

```js
var singleton = new (function () {
  this.name = "ApplicationName";
})();
```

http://github.com에서 처음으로 발행했습니다.
