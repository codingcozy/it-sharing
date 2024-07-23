---
title: "프로그래밍에서 상속의 문제점은 무엇인가"
description: ""
coverImage: "/assets/img/2024-07-23-WhatstheProblemwithInheritanceinProgramming_0.png"
date: 2024-07-23 11:32
ogImage: 
  url: /assets/img/2024-07-23-WhatstheProblemwithInheritanceinProgramming_0.png
tag: Tech
originalTitle: "Whats the Problem with Inheritance in Programming"
link: "https://medium.com/gitconnected/whats-the-problem-with-inheritance-in-programming-e1533f4b4b35"
---



![Image](/assets/img/2024-07-23-WhatstheProblemwithInheritanceinProgramming_0.png)

일부 사람들에게는 C++ 또는 Java와 같은 언어를 통해 객체 지향 프로그래밍을 배운 사람들에게는 상속이 OOP의 선행 조건이 아니라는 것이 놀라울 수 있습니다.

일부 프로그래밍 언어는 상속을 지원하지 않지만 상속이 있는 언어보다 여전히 더 객체 지향적으로 간주됩니다.

Erlang 프로그래밍 언어의 창시자인 Joe Armstrong은 Erlang이 유일한 진정한 객체 지향 언어일 수도 있다고 말한 적이 있습니다.


<div class="content-ad"></div>

알란 케이, 오래된 순수 OOP 언어 중 하나인 Smalltalk의 발명가도 동의했습니다.

Erlang은 주요 OOP 언어인 Java나 C++과는 근본적으로 다른 함수형, 액터 기반 언어입니다. 클래스와 상속을 갖고 있지 않지만 메시지 전달과 캡슐화를 지원합니다.

클래스 자체가 OOP에 꼭 필요한 것은 아니기 때문에 상속이 필수적이지 않다고 받아들이기 쉽습니다.

Self가 이 분야의 초기 개척자인 프로토타입 기반 언어로 알려진 클래스 없이 객체 지향 언어 패밀리도 있습니다.

<div class="content-ad"></div>

나의 경험에 따르면, 상속은 객체 지향 언어에서 가장 남용되는 기능 중 하나입니다.

이 글에서는 상속과 관련된 문제점을 강조하는 몇 가지 예시를 소개하겠습니다.

탱크 전투 게임을 작성한다고 가정해봅시다. 많은 사람들은 다음과 같이 상속을 사용하여 도메인을 모델링할 것입니다:

탱크는 AmoredVehicle을 확장하고, AmoredVehicle은 Vehicle을 확장합니다.

<div class="content-ad"></div>

이것은 순수하게 분류학적인 관점에서만 의미가 있지만, 우리의 목표는 분류체계를 만드는 것이 아닙니다.

더 중요한 것은 게임 엔진의 관점에서 의미가 있는지 여부이며, 우리 자신의 관점이 아닙니다.

게임 엔진은 탱크를 캔버스에 그리고, 탱크와 다른 장애물 간의 충돌을 확인하며, 탱크를 애니메이션화해야합니다.

탱크는 보이는 객체이고, 애니메이션이 적용되며, 물리적입니다.

<div class="content-ad"></div>

이러한 속성을 인터페이스로 표현할 수 있습니다. 여기서는 단일 상속 언어(예: Java) 관점에서 여러 인터페이스의 구현이 허용되지만 다중 상속은 허용되지 않습니다.

다중 상속에는 자체적인 문제가 있습니다. 이에 대해 자세히 설명하지는 않겠지만, 인터페이스를 사용하면 이러한 문제를 피할 수 있습니다.

```java
interface Visible { 
  void draw(Canvas c);
}

interface Animated {
  void tick();
}

interface Physical { 
  Shape hitBox();
  Shape hurtBox();
}

class Tank implements Visible, Animated, Physical { .. }

class Screen {
  [..]
  void draw() {
    for (Visible each : obj)
      each.draw(canvas);
  }
}

[..]

// 게임 루프
animations.tick();
screen.draw();
collisionDetector.check();
```

실제로 탱크가 실제로 차량과 같다고 해서 프로그램에서 반드시 차량 클래스를 가져야 하는 것은 아닙니다.

<div class="content-ad"></div>

게임에서는 "차량"을 고려하지 않을 수 있으며, 대신 렌더링, 애니메이션 및 서로 충돌할 수 있는 객체를 고려합니다.

이제 다음을 고려해 봅시다: 우리는 서로 다른 종류의 포탑, 엔진 및 라디오를 가진 탱크를 표현해야 합니다.

또한 게임 중에 이러한 요소를 동적으로 변경해야 할 수도 있습니다.

상속은 정적이며, 많은 언어에서는 단일 상속만 지원합니다.

<div class="content-ad"></div>

```js
class 탱크 { ... }

class 120mm포탑탱크 extends 탱크 { ... }
class V12트윈터보엔진탱크 extends 탱크 { ... }
class 둘다있는탱크 extends ??? { ... }
```

그래서 위의 방식은 작동하지 않을 것입니다.

서브클래스로 모든 조합을 표현하는 대신, 다양한 구성 요소를 사용하여 탱크를 함께 구성할 수 있습니다.

```js
탱크 tank = new 탱크(
  new 105mm포탑(),
  new V12트윈터보엔진(),
  new 단거리라디오()
);

tank.무기업그레이드(new 150mm포탑());
```

<div class="content-ad"></div>

이렇게 하면 서로 다른 탱크와 장비를 섞어서 사용할 수 있고, 동적으로 변경할 수도 있습니다.

상속의 또 다른 문제는 하위 클래스가 기본 클래스의 구현 세부 정보와 결합될 수 있다는 것입니다.

다음과 같은 기본 클래스가 있다고 상상해보세요. 여기서 addAll 메서드는 내부적으로 add 메서드를 사용하는 방식으로 구현되어 있습니다. 이것은 구현 세부 정보입니다.

```js
class MyList<T> {

  void add(T item) { .. }

  void addAll(T... items) {
    for (T each : items)
      add(each);
  }
}
```

<div class="content-ad"></div>

우리는 요소를 추가한 횟수에 대한 통계를 유지하는 서브클래스를 만들고 싶어해요.

```js
class MyListWithStatistics<T> extends MyList<T> {
  int timesAdded = 0;
  
  @Override
  void add(T item) {
    timesAdded++;
    super.add(item);
  }

  @Override
  void addAll(T... items) {
    timesAdded += items.length;
    super.addAll(items);
  }  
}
```

우리는 단순한 카운터를 증가시키고 슈퍼클래스의 메소드를 호출하는 방식으로 작업합니다.

그러나 리스트에 5개의 항목을 추가한 후에는 카운터가 8로 증가합니다. 왜냐하면 add 및 addAll 두 곳에서 카운터를 증가시켰고 후자는 전자를 사용했기 때문이에요.

<div class="content-ad"></div>

```js
var list = new MyListWithStatistics<Integer>();
list.add(1);
list.add(2);
list.addAll(3, 4, 5);
System.out.println(list.timesAdded);
```

이 문제는 addAll을 오버라이드하지 않고 수정할 수 있습니다. 그러나 핵심은 하위 클래스가 여전히 상위 클래스의 이 구현 세부 정보에 결합되어 있다는 것입니다.

누군가 상위 클래스의 구현을 변경할 때마다 하위 클래스도 영향을 받을 수 있습니다.

만약 데코레이터와 같은 합성을 사용했다면 이 문제를 피할 수 있었을 것입니다.

<div class="content-ad"></div>

사항이 완벽하지 않아도, 별도로 논의해야 할 아이덴티티와 평등에 관련된 문제가 발생할 수 있습니다.

구성을 사용할 때 객체의 외부 인터페이스에만 의존하므로 결합이 약해집니다.

![그림](/assets/img/2024-07-23-WhatstheProblemwithInheritanceinProgramming_1.png)

원래 질문으로 돌아가면, Dog가 Animal을 상속해야 할까요?

<div class="content-ad"></div>

그건 경우에 따라 다를 거예요. 프로그램 관점에서 유용한가요? 개가 실생활에서 동물이라고 해서 반드시 프로그램 안에서도 같은 방식으로 표현해야 한다는 뜻은 아니에요.

우리 프로그램 안에 개와 고양이와 같은 다른 종류의 객체가 혼합된 컬렉션이 있는가요?

만약 그것이 예라면, 다른 종류의 동물들 간에 공통점을 포착할 수 있는 추상화가 필요할 가능성이 높아요. 이를 인터페이스나 슈퍼클래스를 사용하여 구현할 수 있어요.

그렇다고 해서 추상화를 반드시 Animal이어야 하는 것은 아니에요. 예를 들어, 개와 고양이가 플레이어에 대항하는 적인 게임에서는 적을 나타내는 추상화를 사용하는 것이 더 나을 수도 있어요.

<div class="content-ad"></div>

프로그램이 이러한 객체들을 사용하는 방식에서 결정되어야지 생물학 수업에서만 알 수 있는 거 아니지 않을까요?

당신이 생각하기에 가장 과도하게 사용되는 프로그래밍 언어 기능은 무엇인가요?