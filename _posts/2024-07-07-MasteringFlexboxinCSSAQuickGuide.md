---
title: "CSS Flexbox 완벽 가이드 빠르게 배우기"
description: ""
coverImage: "/assets/img/2024-07-07-MasteringFlexboxinCSSAQuickGuide_0.png"
date: 2024-07-07 12:24
ogImage:
  url: /assets/img/2024-07-07-MasteringFlexboxinCSSAQuickGuide_0.png
tag: Tech
originalTitle: "Mastering Flexbox in CSS: A Quick Guide"
link: "https://medium.com/@jeanpierrecarvalho/mastering-flexbox-in-css-a-quick-guide-6ee4c4942187"
---

![Flexbox Guide](/assets/img/2024-07-07-MasteringFlexboxinCSSAQuickGuide_0.png)

웹 개발자로서 Flexbox를 정복하는 것은 마치 초능력을 가진 것과 같습니다. 이는 레이아웃 디자인하는 방법을 변화시키며, 작업을 효율적으로 만들어주고 디자인을 더 유연하게 만들어줍니다. 하지만 가끔은 우리가 유틸리티-퍼스트 클래스를 사용할 때 이 도구들 뒤에 있는 기본 원칙을 잊고 있는 경우가 있습니다. 한 발 물러서서 Flexbox에 대한 이해를 되짚어보겠습니다. 그 과정에서 Flexbox를 배우기 쉽게 만들어줄 재미있는 게임도 소개해드릴게요.

# 왜 유틸리티-퍼스트 클래스는 시간을 절약할까요

Tailwind CSS와 같은 유틸리티-퍼스트 클래스는 굉장히 유용합니다. 커스텀 CSS를 작성하지 않고 빠르게 기능을 구축할 수 있게 해줍니다. 예를 들어, 새로운 CSS 규칙을 작성하는 대신, flex, justify-center, items-center와 같은 클래스를 HTML 엘리먼트에 직접 추가할 수 있습니다. 이 접근 방식은 개발 시간을 단축시키고 코드베이스를 관리하기 쉽게 만들어줍니다.

<div class="content-ad"></div>

# Flexbox의 기본

게임에 도입되기 전에, Flexbox의 기본 개념을 간단히 살펴보겠습니다. Flexbox 또는 유연한 박스 레이아웃은 복잡한 레이아웃을 손쉽게 디자인할 수 있는 레이아웃 모델입니다. 다음은 몇 가지 주요 개념입니다:

- Flex 컨테이너: Flex 속성이 적용된 부모 요소입니다.
- Flex 항목: Flex 컨테이너의 자식 요소들입니다.
- 주축과 교차축: 주축은 Flex 항목이 배치되는 주요 축입니다 (기본적으로 수평 방향), 교차축은 주축과 수직인 축입니다.
- Justify Content: 항목들을 주축을 따라 정렬합니다 (예: justify-content: center).
- Align Items: 항목들을 교차축을 따라 정렬합니다 (예: align-items: center).
- Flex Direction: 주축의 방향을 정의합니다 (예: flex-direction: row 또는 flex-direction: column).

# 어떻게 사용하나요?

<div class="content-ad"></div>

```js
/* 플렉스 컨테이너 클래스 */

/* 모든 직계 자식 요소에 대해 플렉스 컨텍스트를 정의하고 활성화합니다 */
.flex {
    display: flex;
}

/* 플렉스와 유사하지만 인라인 수준의 플렉스 컨테이너를 생성합니다 */
.inline-flex {
    display: inline-flex;
}

/* 플렉스 방향 */

/* 플렉스 항목을 행으로 정렬합니다 (왼쪽에서 오른쪽으로) */
.flex-row {
    flex-direction: row;
}

/* 플렉스 항목을 역순으로 행으로 정렬합니다 (오른쪽에서 왼쪽으로) */
.flex-row-reverse {
    flex-direction: row-reverse;
}

/* 플렉스 항목을 열로 정렬합니다 (위에서 아래로) */
.flex-col {
    flex-direction: column;
}

/* 플렉스 항목을 반대로 열로 정렬합니다 (아래에서 위로) */
.flex-col-reverse {
    flex-direction: column-reverse;
}

/* 주 축 정렬 */

/* 플렉스 항목을 주 축의 시작 부분에 정렬합니다 */
.justify-start {
    justify-content: flex-start;
}

/* 플렉스 항목을 주 축의 끝 부분에 정렬합니다 */
.justify-end {
    justify-content: flex-end;
}

/* 플렉스 항목을 주 축의 중앙에 정렬합니다 */
.justify-center {
    justify-content: center;
}

/* 플렉스 항목을 균등하게 분배하고 그 사이에 공간을 두어 정렬합니다 */
.justify-between {
    justify-content: space-between;
}

/* 플렉스 항목을 균등하게 분배하고 주위에 공간을 두어 정렬합니다 */
.justify-around {
    justify-content: space-around;
}

/* 플렉스 항목을 균등하게 분배하고 주위에 동일한 공간을 두어 정렬합니다 */
.justify-evenly {
    justify-content: space-evenly;
}

/* 항목 정렬 */

/* 플렉스 항목을 교차 축의 시작 부분에 정렬합니다 */
.items-start {
    align-items: flex-start;
}

/* 플렉스 항목을 교차 축의 끝 부분에 정렬합니다 */
.items-end {
    align-items: flex-end;
}

/* 플렉스 항목을 교차 축의 중앙에 정렬합니다 */
.items-center {
    align-items: center;
}

/* 플렉스 항목을 교차 축의 베이스라인을 따라 정렬합니다 */
.items-baseline {
    align-items: baseline;
}

/* 플렉스 항목을 교차 축을 채우도록 늘립니다 */
.items-stretch {
    align-items: stretch;
}

/* 자기 정렬 */

/* 단일 플렉스 항목을 교차 축의 시작 부분에 정렬할 수 있도록 합니다 */
.self-start {
    align-self: flex-start;
}

/* 단일 플렉스 항목을 교차 축의 끝 부분에 정렬할 수 있도록 합니다 */
.self-end {
    align-self: flex-end;
}

/* 단일 플렉스 항목을 교차 축의 중앙에 정렬할 수 있도록 합니다 */
.self-center {
    align-self: center;
}

/* 단일 플렉스 항목을 교차 축에 따라 베이스라인을 사용하여 정렬할 수 있도록 합니다 */
.self-baseline {
    align-self: baseline;
}

/* 단일 플렉스 항목을 교차 축을 채우도록 늘릴 수 있도록 합니다 */
.self-stretch {
    align-self: stretch;
}

/* 순서 */

/* 플렉스 항목의 순서를 설정합니다 */
.order-1 {
    order: 1;
}

.order-2 {
    order: 2;
}

order-3 {
    order: 3;
}
```

# Flexbox Defense: 재미있게 Flexbox 배우기

Flexbox를 정말로 이해하려면 Flexbox Defense를 시도해 보는 것을 매우 권장합니다. Flexbox 속성을 사용하여 방어 시스템을 배치하고 들어오는 적을 멈추는 재미있고 대화식 게임입니다. 배운 내용을 연습하고 강화하는 좋은 방법입니다.

<img src="/assets/img/2024-07-07-MasteringFlexboxinCSSAQuickGuide_1.png" />

<div class="content-ad"></div>

위 문제 해결에 도움을 주셔서 감사합니다!

## 해결책 및 추가 학습 자료

만약 막히거나 어떻게 문제를 해결했는지 보고 싶다면, 이 GitHub 레포지토리를 확인해보세요. Flexbox를 사용하는 다양한 접근 방식을 배우고 비교할 수 있는 유용한 자료입니다. 레포지토리에 별을 추가해주시고 저를 지원해주시는 것을 잊지 마세요 ❤️. 그리고 더 많은 해결책을 받기 위해 Github에서 제 프로필을 팔로우해주세요.

## 결론

<div class="content-ad"></div>

유틸리티 퍼스트 클래스를 사용하면 개발 프로세스가 크게 가속화될 수 있지만, 기본 사항을 기억하는 것이 언제나 좋습니다. 이러한 클래스가 어떻게 구축되는지와 Flexbox가 실제로 작동하는 방식을 이해하면 더 다재다능하고 효과적인 개발자가 될 수 있습니다. 그래서 Flexbox Defense를 해보고 GitHub의 해결책을 탐색하며 Flexbox 스킬을 계속 연마해 보세요. 코딩 즐기세요!

아래 댓글에서 당신이 좋아하는 Flexbox 자료나 생각을 자유롭게 공유해 주세요! 즐거운 코딩 하세요!
