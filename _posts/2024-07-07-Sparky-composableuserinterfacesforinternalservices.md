---
title: "내부 서비스용 컴포저블 사용자 인터페이스 Sparky 사용 방법"
description: ""
coverImage: "/assets/img/2024-07-07-Sparky-composableuserinterfacesforinternalservices_0.png"
date: 2024-07-07 20:44
ogImage:
  url: /assets/img/2024-07-07-Sparky-composableuserinterfacesforinternalservices_0.png
tag: Tech
originalTitle: "Sparky - composable user interfaces for internal services"
link: "https://dev.to/melezhik/sparky-composable-user-interfaces-for-internal-services-3i7k"
---

최근 Sparky 릴리스에서는 내부 웹 애플리케이션용 UI를 만들 수 있는 다양한 기능이 추가되었습니다.

간단히 말하면, Sparky는 자동화 작업을 실행하기 위한 웹 플랫폼이며, Sparky는 여러분이 작업을 매개변수와 함께 실행할 수 있는 풍부한 기능 세트를 제공합니다.

# HTML 컨트롤

커피를 우려봅시다. 최근에는 카페인을 줄이려고 노력하고 있지만요.

<div class="content-ad"></div>

```js
allow_manual_run: true
vars:
  -
    name: Flavor
    default: "latte"
    type: select
    values: [ espresso, americano, latte ]

  -
    name: Topic
    default: "milk"
    type: select
    values: [ milk, cream, cinnamon ]
    multiple: true

  -
    name: Step3
    default: "boiled water"
    type: input
```

이 간단한 Sparky 작업 정의는 3개의 컨트롤을 갖는 간단한 UI를 만듭니다:

<img src="/assets/img/2024-07-07-Sparky-composableuserinterfacesforinternalservices_0.png" />

라쿠 시나리오에서 이러한 입력 매개변수들은 다음과 같이 처리됩니다:

<div class="content-ad"></div>

```js
my $flavor = tags()<맛>;
my $water = tags()<단계3>;
my @topics = [];

# tags() 함수엔 :array 수정자가 필요합니다
# 여러 선택지를 전달할 때

for tags(:array)<주제><> -> $t {
   @topics.push: $t;
};
```

간단하죠?

한 잔 더 커피 주세요!

# 아니면 차를 좋아하세요?

<div class="content-ad"></div>

스파키의 또 다른 멋진 기능은 다중 시나리오 플로우 또는 변수 그룹입니다. 만약 커피 또는 차 중에 선택할 수 있는 선택지를 제공하고 싶다면, 서로 다른 변수 세트를 갖는 작업 정의가 2개가 아니라 하나만 만들어보죠!

```js
vars:

  # 차 변수

  -
    name: 맛
    default: "black"
    type: select
    values: [ black, green ]
    group: [ tea ]

  -
    name: 주제
    default: "milk"
    type: select
    values: [ milk, cream ]
    group: [ tea ]

  # 커피 변수

  -
    name: 맛
    default: "latte"
    type: select
    values: [ espresso, amerikano, latte ]
    group: [ coffee ]

  -
    name: 주제
    default: "milk"
    type: select
    values: [ milk, cream, cinnamon ]
    group: [ coffee ]
    multiple: true

  # 공통 변수

  -

    name: Step3
    default: "boiled water"
    type: input
    group: [ tea, coffee ]

# 그룹 변수
# 그룹의 모든 변수
#는 별도의
# 시나리오입니다

group_vars:
  - tea
  - coffee
```

이제 작업을 실행할 때 선택할 수 있습니다:

<img src="/assets/img/2024-07-07-Sparky-composableuserinterfacesforinternalservices_1.png" />

<div class="content-ad"></div>

그리고 (올바른 링크를 클릭하면), 차 한 잔 하면서 즐겨 봅시다:

![Tea](/assets/img/2024-07-07-Sparky-composableuserinterfacesforinternalservices_2.png)

# 결론

Sparky는 HTML을 사용하여 내부 자동화 서비스를 구축하고 Raku로 자동화 시나리오를 작성하는 멋진 웹 콘솔입니다. 읽어 주셔서 감사합니다.
