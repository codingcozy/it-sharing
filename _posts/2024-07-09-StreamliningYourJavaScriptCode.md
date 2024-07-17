---
title: "자바스크립트 코드를 간소화하는 방법 5가지"
description: ""
coverImage: "/assets/img/2024-07-09-StreamliningYourJavaScriptCode_0.png"
date: 2024-07-09 13:16
ogImage:
  url: /assets/img/2024-07-09-StreamliningYourJavaScriptCode_0.png
tag: Tech
originalTitle: "Streamlining Your JavaScript Code"
link: "https://medium.com/@armstrongjustin034/streamlining-your-javascript-code-472f5e5c289a"
---

# 화살표 함수, DRY (Don’t Repeat Yourself), 및 SRP (단일 책임 원칙).

![Streamlining Your Java Script Code](/assets/img/2024-07-09-StreamliningYourJavaScriptCode_0.png)

ES6에서 소개된 화살표 함수는 함수 표현식을 작성하는 간결한 구문을 제공합니다. 이들은 코드를 더 읽기 쉽게 만들 뿐만 아니라 특정 시나리오에서 특히 유용한 렉시컬 this 바인딩도 제공합니다. 다음 예를 고려해보세요:

```js
// 전통적인 함수
const multiply = function (a, b) {
  return a * b;
};

// 화살표 함수
const multiplyArrow = (a, b) => a * b;
```

<div class="content-ad"></div>

이렇게 "this" 바인딩을 적용할 수 있어요:

```js
const calculator = {
  multiplier: 2,

  // 전통적인 함수
  multiplyTraditional: function (x) {
    return function (y) {
      // 여기서의 'this'는 calculator 객체를 가리키지 않아요
      return x * y * this.multiplier;
    };
  },

  // 화살표 함수
  multiplyArrow: function (x) {
    return (y) => {
      // 여기서의 'this'는 올바르게 calculator 객체를 가리킵니다
      return x * y * this.multiplier;
    };
  },
};

// 사용:
const traditionalMultiply = calculator.multiplyTraditional(3);
console.log(traditionalMultiply(4)); // NaN (왜냐하면 this.multiplier가 정의되지 않았기 때문이에요)

const arrowMultiply = calculator.multiplyArrow(3);
console.log(arrowMultiply(4)); // 24 (3 * 4 * 2)
```

DRY 원칙은 개발자들이 공통 기능을 재사용 가능한 구성 요소로 추상화하여 코드의 중복을 피하도록 권장합니다. 이렇게 하면 전체 코드베이스 크기를 줄일 뿐만 아니라 유지 관리를 쉽게 할 수 있어요. 여기 DRY를 적용한 예시가 있어요:

```js
// DRY 전
const calculateAreaSquare = (side) => side * side;
const calculateAreaRectangle = (length, width) => length * width;
```

<div class="content-ad"></div>

```js
// DRY 원칙 준수 후
const calculateArea = (shape, ...dimensions) => {
  switch (shape) {
    case "square":
      return dimensions[0] * dimensions[0];
    case "rectangle":
      return dimensions[0] * dimensions[1];
    default:
      throw new Error("지원하지 않는 도형입니다.");
  }
};
```

이 접근 방식은 영역 계산 논리의 반복을 제거하고 더 유연하고 재사용 가능한 함수를 만들어내어 DRY 원칙을 준수합니다. 이제 이 함수를 다음처럼 정사각형과 직사각형 모두에 사용할 수 있습니다:

```js
// 정사각형의 면적 계산
const squareArea = calculateArea("square", 5);
console.log(squareArea); // 25

// 직사각형의 면적 계산
const rectangleArea = calculateArea("rectangle", 4, 6);
console.log(rectangleArea); // 24
```

SRP는 각 함수 또는 클래스가 단일하고 명확한 책임을 갖도록 하는 원칙입니다. 이 원칙은 모듈화를 촉진하며 코드를 이해, 테스트 및 유지 보수하기 쉽게 만듭니다. 예제를 살펴보겠습니다:

<div class="content-ad"></div>

```js
// SRP 이전
const processUser = (user) => {
  validateUser(user);
  saveUserToDatabase(user);
  sendWelcomeEmail(user);
};
```

```js
// SRP 이후
const processUser = (user) => {
  if (isValidUser(user)) {
    const savedUser = saveUserToDatabase(user);
    sendWelcomeEmail(savedUser);
  }
};
```

“SRP 이후” 버전은 “SRP 이전” 버전보다 덜 하는 것은 아니지만, 오히려 작업을 더 잘 정리함으로써 단일 책임 원칙을 더 잘 준수합니다. 이제 각 함수는 하나의 명확하고 정의된 목적을 갖게 되며, 이를 통해 코드를 유지보수하고 확장하기 쉽게 만듭니다.

화살표 함수, DRY, SRP 이 세 가지 개념을 결합하여 더 효율적이고 가독성이 좋으며 유지보수가 쉬운 코드를 만들 수 있습니다. 화살표 함수는 간결한 구문을 제공하고, DRY 원칙은 중복을 제거하는 데 도움을 주며, SRP는 각 함수가 명확하고 단일한 목적을 갖도록 보장합니다. 이러한 원칙을 적용하면 더 견고하고 확장 가능한 코드베이스가 만들어지며, 최종적으로 개발 프로세스에서 시간을 절약하고 오류를 줄일 수 있습니다.
