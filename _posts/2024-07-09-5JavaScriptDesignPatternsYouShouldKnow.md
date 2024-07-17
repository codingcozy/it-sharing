---
title: "알아두면 좋은 5가지 자바스크립트 디자인 패턴"
description: ""
coverImage: "/assets/img/2024-07-09-5JavaScriptDesignPatternsYouShouldKnow_0.png"
date: 2024-07-09 16:23
ogImage:
  url: /assets/img/2024-07-09-5JavaScriptDesignPatternsYouShouldKnow_0.png
tag: Tech
originalTitle: "5 JavaScript Design Patterns You Should Know"
link: "https://medium.com/@emma-delaney/5-javascript-design-patterns-you-should-know-25c31ad61191"
---

![JavaScript Design Patterns](/assets/img/2024-07-09-5JavaScriptDesignPatternsYouShouldKnow_0.png)

자바스크립트 개발자로서 디자인 패턴을 배우는 것은 흔한 프로그래밍 문제에 대한 입증된 솔루션을 제공하는 도구 세트를 갖게 됨을 의미합니다. 디자인 패턴은 다양한 문제를 해결하기 위한 구조화된 접근 방식을 제공하여 깨끗하고 효율적이며 유지보수가 용이한 코드를 작성하는 데 도움이 됩니다. 이 블로그 포스트에서는 모든 JavaScript 개발자가 인식해야 할 다섯 가지 주요 디자인 패턴을 살펴보고 각각에 대한 실용적인 예제를 제공할 것입니다.

## 싱글톤 패턴

싱글톤 패턴은 클래스의 단일 인스턴스만 존재하도록 보장하고 해당 인스턴스에 대한 전역 진입점을 제공합니다. 이것은 클래스의 존재를 단일 객체로 제한하고 싶은 경우 매우 유용합니다.

<div class="content-ad"></div>

문제: 응용 프로그램의 어디에서든 액세스할 수있는 로거를 만들어야하지만 하나의 로거 인스턴스만 필요합니다.

해결책:

```js
// 싱글톤 패턴 구현
class Logger {
  private static instance: Logger;

  private constructor() {}

  public static getInstance(): Logger {
    if (!Logger.instance) {
      Logger.instance = new Logger();
    }
    return Logger.instance;
  }

  public log(message: string): void {
    console.log(`[${new Date().toISOString()}] ${message}`);
  }
}

// 사용 예시
const logger1 = Logger.getInstance();
const logger2 = Logger.getInstance();

logger1.log("안녕, 세상!"); // [2023-02-20T14:30:00.000Z] 안녕, 세상!
logger2.log("다시 안녕!"); // [2023-02-20T14:30:00.000Z] 다시 안녕!

console.log(logger1 === logger2); // true
```

## 팩토리 패턴

<div class="content-ad"></div>

팩토리 패턴은 생성되는 객체의 정확한 클래스를 지정하지 않고 객체를 생성하는 인터페이스를 제공합니다. 특정 조건이나 매개변수에 기반하여 객체를 생성하고 싶을 때 유용합니다.

문제: 차량(예: 자동차, 트럭, 오토바이)을 다양하게 생성하되 내부 빌드 로직을 숨기고 싶습니다.

해결:

```js
// 팩토리 패턴 구현
interface Vehicle {
  drive(): void;
}

class Car implements Vehicle {
  drive(): void {
    console.log("자동차를 운전합니다...");
  }
}

class Truck implements Vehicle {
  drive(): void {
    console.log("트럭을 운전합니다...");
  }
}

class Motorcycle implements Vehicle {
  drive(): void {
    console.log("오토바이를 운전합니다...");
  }
}

class VehicleFactory {
  public static createVehicle(type: string): Vehicle {
    switch (type) {
      case "car":
        return new Car();
      case "truck":
        return new Truck();
      case "motorcycle":
        return new Motorcycle();
      default:
        throw new Error(`알 수 없는 차량 타입: ${type}`);
    }
  }
}

// 사용 방법
const car = VehicleFactory.createVehicle("car");
const truck = VehicleFactory.createVehicle("truck");
const motorcycle = VehicleFactory.createVehicle("motorcycle");

car.drive(); // 자동차를 운전합니다...
truck.drive(); // 트럭을 운전합니다...
motorcycle.drive(); // 오토바이를 운전합니다...
```

<div class="content-ad"></div>

## 3. Observer Pattern

옵저버 패턴은 객체들 간의 일대다 관계를 정의합니다. 객체(엔티티)의 상태가 변경될 때, 모든 종속 객체(옵저버)들이 알림을 받고 자동으로 업데이트됩니다.

문제: 한 객체가 변경될 때 여러 개의 객체가 업데이트되어야 합니다.

해결책:

<div class="content-ad"></div>

```js
// 옵서버 패턴 구현
interface Observer {
  update(data: any): void;
}

class Subject {
  private observers: Observer[] = [];
  private data: any;

  public addObserver(observer: Observer): void {
    this.observers.push(observer);
  }

  public removeObserver(observer: Observer): void {
    const index = this.observers.indexOf(observer);
    if (index !== -1) {
      this.observers.splice(index, 1);
    }
  }

  public notifyObservers(): void {
    this.observers.forEach((observer) => observer.update(this.data));
  }

  public setData(data: any): void {
    this.data = data;
    this.notifyObservers();
  }
}

class ConcreteObserver implements Observer {
  public update(data: any): void {
    console.log(`Received update: ${data}`);
  }
}

// 사용법
const subject = new Subject();
const observer1 = new ConcreteObserver();
const observer2 = new ConcreteObserver();

subject.addObserver(observer1);
subject.addObserver(observer2);

subject.setData("안녕, 세상!"); // Received update: 안녕, 세상!

```

## 데코레이터 패턴

데코레이터 패턴은 클래스 내의 다른 객체들의 행동에 영향을 미치지 않고 개별 객체에 행동을 추가할 수 있도록 해줍니다. 이는 하위 클래스 작성의 유연한 대안입니다.

문제: 기존 객체에 로깅 기능을 추가해야 하지만 해당 객체의 구현에는 영향을 주고 싶지 않을 때 사용합니다.

<div class="content-ad"></div>

해결책:

```js
class Coffee {
  cost() {
    return 5;
  }
}

class MilkDecorator {
  constructor(coffee) {
    this.coffee = coffee;
  }

  cost() {
    return this.coffee.cost() + 2;
  }
}

class SugarDecorator {
  constructor(coffee) {
    this.coffee = coffee;
  }

  cost() {
    return this.coffee.cost() + 1;
  }
}

let coffee = new Coffee();
coffee = new MilkDecorator(coffee);
coffee = new SugarDecorator(coffee);

console.log(coffee.cost()); // 결과: 8
```

## 5. Module Pattern

모듈은 JavaScript에서 포함되고 재사용 가능한 구성 요소를 생성하는 기능을 제공합니다. 이를 통해 깔끔한 코드를 유지하고 전역 이름 공간 오염을 피할 수 있습니다.

<div class="content-ad"></div>

```js
const CalculatorModule = (function () {
  let result = 0;

  function add(a, b) {
    result = a + b;
  }

  function subtract(a, b) {
    result = a - b;
  }

  function getResult() {
    return result;
  }

  return {
    add,
    subtract,
    getResult,
  };
})();

CalculatorModule.add(5, 3);
console.log(CalculatorModule.getResult()); // 결과: 8
```

다섯 가지 디자인 패턴인 싱글톤, 팩토리, 옵서버, 데코레이터 그리고 모듈을 이해하면 깔끔하고 유지보수 가능하며 효율적인 JavaScript 코드를 작성하는 능력이 크게 향상됩니다. 이러한 패턴을 개발자 도구에 통합하면 다양한 프로그래밍 도전에 우아하고 효율적으로 대응할 수 있습니다.
