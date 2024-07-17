---
title: "JavaScript에서 추상 클래스 사용법"
description: ""
coverImage: "/assets/img/2024-07-09-AbstractClassesinJavaScript_0.png"
date: 2024-07-09 17:19
ogImage:
  url: /assets/img/2024-07-09-AbstractClassesinJavaScript_0.png
tag: Tech
originalTitle: "Abstract Classes in JavaScript"
link: "https://medium.com/@rheedhar/abstract-classes-in-javascript-d6510afac958"
---

![이미지](/assets/img/2024-07-09-AbstractClassesinJavaScript_0.png)

자바스크립트에서 추상 클래스 개념은 Java, TypeScript 및 Python과 같은 다른 언어에서처럼 네이티브로 지원되지 않습니다. 그러나 우리는 자바스크립트에서 추상 클래스의 행동을 모방하기 위해 사용자 정의 코드를 작성할 수 있습니다. 이 글에서는 추상 클래스가 무엇인지, 왜 추상 클래스가 필요한지, 그리고 어떻게 JavaScript에서 구현할 수 있는지 설명하겠습니다. TypeScript는 JavaScript를 기반으로 한 프로그래밍 언어이므로, 이해를 돕기 위해 추상 클래스 개념을 설명할 때 TypeScript를 사용할 것입니다.

추상 클래스란 무엇인가요?

이는 추상 클래스를 직접적으로 객체를 생성하는 데 사용할 수 없다는 것을 의미합니다. 예를 들어 클래스를 집 건축용 청사진으로 생각해보면, 추상 클래스는 바로 집을 짓는 데 사용할 수 없는 종류의 청사진입니다. 대신, 추상 클래스는 다른 클래스가 상속할 수 있는 클래스입니다. 다른 청사진(클래스)이 기본으로 사용할 수 있는 청사진 역할을 합니다. 청사진을 비유하면, 다른 청사진이 기반으로 사용할 수 있는 청사진입니다.

<div class="content-ad"></div>

아래 예시에서 Shape 클래스는 추상 클래스입니다. 이는 abstract 키워드를 사용하여 정의되었기 때문입니다. 만약 Shape 클래스의 인스턴스를 생성하려고하면 오류가 발생합니다. Circle 클래스는 추상 Shape 클래스를 상속받은 클래스입니다. 따라서 일반 클래스처럼 해당 클래스의 속성 및 메서드에 액세스할 수 있습니다.

```js
abstract class Shape {
  name: string;
  constructor(name: string) {
     this.name = name;
  }
}

class Circle extends Shape {
  radius: number;
  constructor(name: string, radius:number){
     super(name);
     this.radius = radius;
  }

}

const myShape = new Shape('My shape'); // 이 코드는 오류가 발생합니다.
const shortCircle = new Circle("Short Circle", 0.5); // 이 코드는 정상적으로 작동합니다.
```

추상 클래스의 주요 기능 중 하나는 추상 클래스가 추상 메서드를 가질 수 있다는 것입니다.

추상 클래스 내에서 추상 메서드가 생성된 경우, 해당 추상 클래스를 상속받는 모든 클래스(하위 클래스)는 추상 메서드의 구현을 가져야 합니다. 하위 클래스 내에서 구현은 서로 다를 수 있지만(사실 추상 메서드의 목적), 모든 하위 클래스는 메서드가 무엇을 할지 정의해야 합니다.

<div class="content-ad"></div>

추상 클래스의 정상 메서드는 하위 클래스에서 강제되지 않고 하위 클래스가 사용하거나 확장할 수 있는 기능입니다. 아래 예시에서 getArea() 메서드는 추상 Shape 클래스 내에 정의된 추상 메서드입니다. Circle 및 Rectangle 클래스는 모두 Shape 클래스를 상속받지만 Circle 클래스만 getArea()에 대한 구현이 있습니다. 따라서 Rectangle에서 새 인스턴스를 만들려고하면 getArea() 메서드의 구현이 필요하다는 오류가 발생합니다.

```js
abstract class Shape {
  name: string;
  constructor(name: string) {
     this.name = name;
  }

  abstract getArea(): number;
}

class Circle extends Shape {
  radius: number;
  constructor(name: string, radius:number){
     super(name);
     this.radius = radius;
  }

  getArea(): number {
    return Math.PI * Math.pow(this.radius, 2);
  }

}

class Rectangle extends Shape {
  length: number;
   width: number;

   constructor(name:string, length:number, width:number){
     super(name);
     this.length = length;
     this.width = width;
   }

}

const smallRectangle = new Rectangle("Small Rectangle", 3, 5) // 이 부분은 오류를 발생시킵니다.
```

추상 클래스가 필요한 이유는 무엇일까요?

추상 클래스는 코드 내에서 행동을 구조화하고 강제하는 데 유용합니다. 추상 클래스를 사용하면 개발자가 각 하위 클래스가 어떻게 구현되어야 하는지 설정하고 강제할 수 있습니다. 이는 코드의 의도된 구조를 문서화하고 전달하는 데 도움이 됩니다. 추상 클래스는 또한 기본 클래스의 직접적인 인스턴스화를 방지하여 코드베이스에서 원치 않는 버그를 방지하는 안전장치 역할을 합니다. 또한 하위 클래스 내의 메서드 구현의 간과 가능성을 제거합니다. 이는 프로그램 내의 버그를 방지하고 코드가 의도한대로 작동하도록 보장하는 데 도움이 됩니다.

<div class="content-ad"></div>

자바스크립트에서의 추상 클래스

자바스크립트에서 추상 클래스를 만드는 것은 창의력이 필요해요. 여기 이전에 작성한 코드를 자바스크립트로 구현한 예제가 있어요:

```js
class Shape {
  constructor(name) {
    if (this.constructor == Shape) {
      throw new Error("Class is of abstract type and can't be instantiated");
    }

    if (this.getArea == undefined) {
      throw new Error("getArea method must be implemented");
    }
    this.name = name;
  }
}

class Rectangle extends Shape {
  constructor(name, length, width) {
    super(name);
    this.length = length;
    this.width = width;
  }
}

const myShape = new Shape("My shape"); // 이 부분에서 에러가 발생할 거에요
const smallRectangle = new Rectangle("Small Rectangle", 3, 5); // 이 부분에서도 에러가 발생해요.
```

위 코드에서는 새로 생성된 객체가 Shape 클래스의 생성자 함수에 의해 생성되었는지 확인해요. 그렇다면 Shape 클래스가 직접적으로 인스턴스화되고 있다는 것이기 때문에 에러를 던져요. 또한 모양 클래스의 모든 하위 클래스에서 추상 메서드인 getArea()가 구현되고 있는지 확인해요. 하위 클래스에 getArea()의 구현이 없다면 에러를 던져요.

<div class="content-ad"></div>

우리가 코드를 이렇게 작성함으로써, JavaScript에서도 적용할 수 있는 추상 클래스의 동작을 모방하고 있습니다.

마지막으로 한 가지 생각.

추상 클래스는 코드를 구조화하고 정리하는 데 매우 유용합니다. JavaScript에서 추상 클래스를 만드는 방법을 찾고 있다면, TypeScript를 사용하는 것을 권장드립니다. TypeScript는 다른 최상의 방법을 강요하고, 타입 안전성을 제공하며, 추상 클래스 개념을 네이티브로 지원하기 때문에 좋은 선택입니다.

그렇지만 JavaScript를 사용해야 하는 경우에는, 이제 어떻게 해야 하는지 알게 되었으니 걱정 마세요 :)

<div class="content-ad"></div>

읽어 주셔서 감사합니다!
