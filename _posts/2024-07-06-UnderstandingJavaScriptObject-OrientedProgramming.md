---
title: "자바스크립트 객체지향 프로그래밍 이해하기 기본 개념 및 예제"
description: ""
coverImage: "/assets/img/2024-07-06-UnderstandingJavaScriptObject-OrientedProgramming_0.png"
date: 2024-07-06 10:02
ogImage:
  url: /assets/img/2024-07-06-UnderstandingJavaScriptObject-OrientedProgramming_0.png
tag: Tech
originalTitle: "Understanding JavaScript Object-Oriented Programming"
link: "https://medium.com/the-javascript/understanding-javascript-object-oriented-programming-7e46a42c0743"
---

/assets/img/2024-07-06-UnderstandingJavaScriptObject-OrientedProgramming_0.png

# 소개

자바스크립트의 객체 지향 프로그래밍(OOP) 접근 방식은 고유하며, Java나 C++과 같은 전통적인 OOP 언어와는 큰 차이가 있습니다. 이 기사는 JavaScript가 OOP를 다루는 방식을 자세히 살펴보며, new 키워드, 프로토타입, 생성자, 프로토타입 상속과 같은 개념에 초점을 맞출 것입니다.

# 목차

<div class="content-ad"></div>

- 소개
- 새로운 키워드

- '새로운'의 작동 방식
- 프로토타입에 링크 만들기

3. 프로토타입 이해

- prototype 속성
- 프로토타입에 접근하기
- 사용되지 않는 **proto**

<div class="content-ad"></div>

4. 프로토타입 메소드에 대한 내용

- 프로토타입 메소드의 효율성
- ES5 예제
- ES2015 (ES6) 예제

5. 자바스크립트에서의 프로토타입 역할

- 프로토타입 체인

<div class="content-ad"></div>

6. 프로토타입 상속

- JavaScript에서 상속 구현하기
- ES5 예제
- ES5 객체지향 프로그래밍에 대한 노트

7. 결론

# 새로운 키워드

<div class="content-ad"></div>

# 새로운 키워드 동작 방식

JavaScript에서 새 키워드는 생성자 함수를 가진 객체의 인스턴스를 만드는 데 사용됩니다. new를 사용할 때 발생하는 일련의 과정은 다음과 같습니다:

- 빈 객체 생성: 새로운 빈 객체가 생성됩니다.
- this 키워드 설정: 새로 생성된 객체가 this에 할당됩니다.
- 객체 반환: 생성자 함수에 의해 새로 생성된 객체가 반환됩니다.
- 객체의 프로토타입에 대한 링크 설정: 새로운 객체는 생성자 함수의 프로토타입 객체에 링크합니다.

# 프로토타입과의 연결 생성하기

<div class="content-ad"></div>

프로토타입으로의 링크가 어떻게 설정되는지 이해하기 위해 JavaScript에서 객체와 함수를 검토해봅시다:

- 프로토타입 속성: 모든 함수는 prototype이라는 속성을 가지고 있습니다.
- 생성자 속성: 프로토타입 객체에는 생성자라는 속성이 있으며, 해당 속성은 함수를 가리킵니다.
- 내부 프로토타입: 새 키워드를 사용하여 함수를 호출할 때, 생성된 객체와 프로토타입 객체 간의 링크가 설정됩니다. 이 내부 프로토타입은 Object.getPrototypeOf()를 사용하여 액세스할 수 있습니다.

## 예시:

```js
function Device(brand, model, year) {
  this.brand = brand;
  this.model = model;
  this.year = year;
}
Device.prototype; // 객체
Device.prototype.constructor === Device; // true
let myFirstDevice = new Device("Samsung", "Galaxy S21", 2021);
Object.getPrototypeOf(myFirstDevice) === Device.prototype; // true
```

<div class="content-ad"></div>

# Deprecated \_\_proto\_\_

과거에는 \_\_proto\_\_ 속성을 이용해 객체의 프로토타입에 접근했습니다. 그러나 이 방법은 이제 폐기되었습니다:

```js
console.log(myFirstDevice.__proto__ === Device.prototype); // true (하지만 폐기됨)
```

대신 Object.getPrototypeOf()을 사용하세요:

<div class="content-ad"></div>

```js
console.log(Object.getPrototypeOf(myFirstDevice) === Device.prototype); // true
```

# 프로토타입에 있는 메서드

# 프로토타입 메서드의 효율성

생성자 내부가 아닌 프로토타입에 인스턴스 메서드를 정의하는 것이 더 효율적입니다. 이렇게 하면 객체의 모든 인스턴스가 동일한 메서드를 공유하므로 각 객체마다 새로운 메서드 인스턴스를 생성하지 않아도 됩니다.

<div class="content-ad"></div>

# ES5 예시

ES5에서 프로토타입에 메서드를 정의하는 방법은 다음과 같습니다:

```js
function Device(brand, model, year) {
  this.brand = brand;
  this.model = model;
  this.year = year;
  this.powerOn = function () {
    return "Powering on!";
  };
}
Device.prototype.beep = function () {
  return "Beep beep!";
};
```

# ES2015(ES6) 예시

<div class="content-ad"></div>

ES6에서는 클래스 구문을 사용하여 프로세스를 간소화할 수 있습니다:

```js
class Device {
  constructor(brand, model, year) {
    this.brand = brand;
    this.model = model;
    this.year = year;
    this.powerOn = function () {
      return "Powering on!";
    };
  }
  beep() {
    return "Beep beep!";
  }
}
```

# JavaScript에서 Prototype의 역할

JavaScript는 모든 객체에서 메소드와 속성을 찾을 때 프로토 타입 객체를 사용합니다. 객체에서 속성이나 메소드를 찾을 수 없으면 JavaScript는 "프로토 타입 체인"을 따라가며 각 객체의 프로토 타입을 확인하고, 속성 또는 메소드를 찾거나 undefined를 반환할 때까지 계속합니다.

<div class="content-ad"></div>

# 프로토타입 상속

프로토타입 상속을 통해 다른 객체의 프로토타입으로 동작하는 객체를 만들 수 있습니다.

# JavaScript에서 상속 구현하기

다음은 ES5를 사용하여 JavaScript에서 상속을 구현하는 방법입니다:

<div class="content-ad"></div>

```js
function Device(brand, model, year) {
  this.brand = brand;
  this.model = model;
  this.year = year;
}
```

# ES5 OOP에 대한 메모

- ES2015 (ES6)는 class 문법을 통해 이를 간단하게 만들었습니다.
- prototype이 무엇인지 설명할 수 있어야 합니다.
- JavaScript에서 상속이 어떻게 구현되는지 이해하는 것이 중요합니다.

# 결론

<div class="content-ad"></div>

테이블 태그를 마크다운 형식으로 변경해주세요.
