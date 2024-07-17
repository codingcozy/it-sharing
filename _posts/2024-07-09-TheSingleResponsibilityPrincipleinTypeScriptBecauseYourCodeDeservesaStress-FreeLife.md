---
title: "TypeScript의 단일 책임 원칙 코드가 스트레스 없이 살아야 하는 이유"
description: ""
coverImage: "/assets/img/2024-07-09-TheSingleResponsibilityPrincipleinTypeScriptBecauseYourCodeDeservesaStress-FreeLife_0.png"
date: 2024-07-09 08:40
ogImage:
  url: /assets/img/2024-07-09-TheSingleResponsibilityPrincipleinTypeScriptBecauseYourCodeDeservesaStress-FreeLife_0.png
tag: Tech
originalTitle: "The Single Responsibility Principle in TypeScript: Because Your Code Deserves a Stress-Free Life"
link: "https://medium.com/@neon._.builds/the-single-responsibility-principle-in-typescript-because-your-code-deserves-a-stress-free-life-246821d65dea"
---

![image](/assets/img/2024-07-09-TheSingleResponsibilityPrincipleinTypeScriptBecauseYourCodeDeservesaStress-FreeLife_0.png)

# 소개

어, SRP(Single Responsibility Principle)군요. 이것에 대해 들어보지 못했다면 걱정 마세요. 이것은 자기계발 서적은 아니지만, 될 수도 있습니다. 이것은 코딩 원칙의 Marie Kondo라고 생각해보세요. SRP는 코드를 정리하여 한 번에 한 가지만 걱정하게끔 만드는 것입니다. 솔직히 말해서, 누가 자신의 삶에서 약간의 스트레스를 덜 받을 수 없겠습니까?

# SRP(Single Responsibility Principle)란 무엇인가요?

<div class="content-ad"></div>

SRP는 객체 지향적 디자인의 SOLID 원칙 중 하나입니다. 간단하게 말하면 클래스는 변경할 이유가 하나만 있어야 한다는 것을 의미합니다. 이는 클래스가 한 가지 작업 또는 책임만 가져야 한다는 것을 의미합니다. 만약 토스터가 커피를 내리려고 하거나, 설거지를 하려 하거나, 연애 조언을 해 준다면 어떤 일이 벌어질까요? 전혀 엉망진창이 될 것 같죠? 코드도 마찬가지입니다.

# SRP의 중요성은 무엇인가요?

- 유지보수성: 클래스가 단일 책임을 갖고 있으면 버그를 찾기가 더 쉽습니다. 다중 작업 몬스터의 어느 부분이 문제를 일으키는지 알아내는 탐정 할 일이 없을 것입니다.
- 테스트 가능성: 클래스가 집중되어 있을 때 테스트가 더 간단해집니다. 특정 기능을 다루는 보다 목표 지향적인 테스트를 작성하고 의도하지 않은 부작용의 위험 없이 특정 기능을 다룰 수 있습니다.
- 재사용성: 단일 책임 클래스는 응용 프로그램의 서로 다른 부분 또는 다른 프로젝트에서도 더 쉽게 재사용할 수 있습니다.

# 실세계 예시: 과로한 직원

<div class="content-ad"></div>

실제 사례를 들어보겠습니다. 밥을 만나보세요. 밥은 작은 스타트업 기업에서 일하는 직원입니다. 밥은 고객 이메일에 답변하고, 커피를 내리고, 코드를 작성하고, 사무실을 청소하고, 사무실 고양이를 먹이는 책임이 있습니다. 밥은 지치고 항상 스트레스를 받는 상태입니다. 어느 날, 밥은 감당할 수 없어지고 고양이 사료를 커피 머신에 섞어버립니다. 혼돈이 시작됩니다.

이제, 이 시나리오에 SRP를 적용해 봅시다. 밥이 모든 일을 하기보다 추가적인 직원을 고용합니다: 앨리스가 고객 이메일을 처리하고, 찰리가 커피를 내리고, 데이브가 코드를 작성하고, 이브가 사무실을 청소하고, 프랭크가 고양이를 먹입니다. 각자가 하나의 책임을 맡고, 밥은 자신의 가장 잘하는 일에 집중할 수 있게 됩니다 (아마 커피 내리기는 가장 잘하지 않는 일이겠죠).

# TypeScript에서 SRP 적용하기

이를 코드에 다시 적용해 봅시다. TypeScript에서 하면 안 되는 예시가 여기 있습니다:

<div class="content-ad"></div>

```js
class OverworkedEmployee {
  handleCustomerEmails() {
    console.log("Handling customer emails...");
  }

  makeCoffee() {
    console.log("Making coffee...");
  }

  writeCode() {
    console.log("Writing code...");
  }

  cleanOffice() {
    console.log("Cleaning the office...");
  }

  feedCat() {
    console.log("Feeding the cat...");
  }
}
```

이제 SRP를 사용하여 이를 리팩토링해 봅시다:

```js
class CustomerService {
  handleCustomerEmails() {
    console.log("Handling customer emails...");
  }
}

class Barista {
  makeCoffee() {
    console.log("Making coffee...");
  }
}

class Developer {
  writeCode() {
    console.log("Writing code...");
  }
}

class Janitor {
  cleanOffice() {
    console.log("Cleaning the office...");
  }
}

class CatCaretaker {
  feedCat() {
    console.log("Feeding the cat...");
  }
}
```

# SRP in Action

<div class="content-ad"></div>

저희가 리팩터링한 코드로 어떻게 모든 것이 더 원활해졌는지 확인해 봅시다. 각 클래스는 이제 단일 책임을 갖습니다:

```js
const alice = new CustomerService();
alice.handleCustomerEmails();

const charlie = new Barista();
charlie.makeCoffee();

const dave = new Developer();
dave.writeCode();

const eve = new Janitor();
eve.cleanOffice();

const bob = new CatCaretaker();
frank.feedCat();
```

SRP를 따르면 더 깨끗하고 관리하기 쉬운 코드베이스가 됩니다. 만약 커피 제조 방법을 변경해야 한다면, Barista 클래스만 업데이트하면 됩니다. 우리 코드도 더 행복하고, 우리도 행복할 거예요.

# 결론

<div class="content-ad"></div>

마지막으로, 단일 책임 원칙은 당신과 코드를 위해 삶을 더 쉽게 만드는 것에 관한 것입니다. 각 클래스에 단일 작업을 부여함으로써 복잡성을 줄이고 코드를 유지 보수 가능하고 테스트 가능하며 재사용 가능하게 만들 수 있습니다. 그러니 자신의 마리 콘도를 채널하여 코드 정리를 시작해보세요. 결국, 행복한 코드베이스는 행복한 개발자로 이어집니다. 그리고 어떨까요? 아마도 밥이 스트레스 없는 한 잔의 커피를 즐길 수도 있을 거예요.

재미있거나 공포스러운 단일 책임 원칙 이야기가 있으신가요? 아래 댓글에 공유해 주세요! 그리고 더 많은 코딩 정보를 얻으려면 팔로우를 잊지 마세요…!✌️
