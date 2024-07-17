---
title: "고급 TypeScript 기능 열기 제네릭, 데코레이터 및 그 이상의 기술 알아보기"
description: ""
coverImage: "/assets/img/2024-07-12-UnlockingAdvancedTypeScriptFeaturesGenericsDecoratorsandBeyond_0.png"
date: 2024-07-12 18:49
ogImage:
  url: /assets/img/2024-07-12-UnlockingAdvancedTypeScriptFeaturesGenericsDecoratorsandBeyond_0.png
tag: Tech
originalTitle: "Unlocking Advanced TypeScript Features: Generics, Decorators, and Beyond"
link: "https://medium.com/@sviat-kuzhelev/unlocking-advanced-typescript-features-generics-decorators-and-beyond-1878e381e19a"
---

TypeScript는 JavaScript의 강력한 상위 집합으로, 코드 품질과 유지 보수성을 향상시킬 수 있는 능력으로 상당한 인기를 얻고 있어요.

우리 많은 사람들은 타입, 인터페이스, 클래스와 같은 기본 TypeScript 개념에 익숙할 것이지만, 고급 기능에 대한 탐구는 코드의 생산성과 견고성을 새로운 수준으로 끌어올릴 수 있어요.

이 기사에서는 실용적인 예제와 실세계 사용 사례를 통해 제네릭, 데코레이터, 조건부 타입과 같은 세 가지 고급 TypeScript 기능을 탐구해 보려고 해요. 💖

# 제네릭: 유연하고 재사용 가능한 코드

<div class="content-ad"></div>

일반적인 함수는 타입 안전성을 희생하지 않으면서 다양한 유형에서 작동할 수 있습니다. 다음은 간단한 예시입니다:

![이미지](/assets/img/2024-07-12-UnlockingAdvancedTypeScriptFeaturesGenericsDecoratorsandBeyond_0.png)

<div class="content-ad"></div>

이 예시에서 identity는 타입 매개변수 T와 T 타입의 인수를 취하고 동일한 타입의 값을 반환하는 일반 함수입니다. 이 함수는 다른 타입으로 호출할 수 있어도 타입 안정성을 유지할 수 있습니다.

## 제네릭 클래스

제네릭 클래스를 사용하면 다양한 타입에 대한 청사진을 만들 수 있습니다:

![이미지](/assets/img/2024-07-12-UnlockingAdvancedTypeScriptFeaturesGenericsDecoratorsandBeyond_1.png)

<div class="content-ad"></div>

여기서 GenericBox는 어떤 유형 T도 보관할 수 있는 클래스입니다. 이로 인해 클래스가 다른 유형을 재사용할 수 있어 로직을 다시 작성할 필요가 없게 됩니다.

대부분의 일반적인 경우에는 일반화 스케일링이 필요하지 않지만, 필요한 시기에는 이를 염두에 두고 있어야 합니다.

## 일반화 제약조건

가끔은 제네릭과 사용할 수 있는 유형을 제한하고 싶을 수 있습니다. 이때 제약조건이 사용됩니다:

<div class="content-ad"></div>

![이미지](/assets/img/2024-07-12-UnlockingAdvancedTypeScriptFeaturesGenericsDecoratorsandBeyond_2.png)

이 함수에서 T는 길이 속성이 있는 유형으로 제한되어 있으며, 여전히 일반적인 상태를 유지하면서 유형 안전성을 보장합니다.

# 데코레이터: TypeScript에서의 메타 프로그래밍

데코레이터는 클래스 선언 및 멤버에 대한 주석 및 메타 프로그래밍 구문을 추가할 수 있는 강력한 기능입니다. Angular 및 NestJS와 같은 프레임워크에서 널리 사용됩니다.

<div class="content-ad"></div>

## 클래스 데코레이터

클래스 데코레이터는 클래스 생성자를 가져와 수정하거나 주석을 추가하는 함수입니다:

![이미지](/assets/img/2024-07-12-UnlockingAdvancedTypeScriptFeaturesGenericsDecoratorsandBeyond_3.png)

여기서 sealed 데코레이터는 클래스를 봉인하여 새로운 속성이 추가되는 것을 방지합니다.

<div class="content-ad"></div>

## 메서드 데코레이터

메서드 데코레이터를 사용하면 클래스 내의 메서드를 수정하거나 주석 처리할 수 있습니다:

![image](/assets/img/2024-07-12-UnlockingAdvancedTypeScriptFeaturesGenericsDecoratorsandBeyond_4.png)

이 예시에서 로그 데코레이터는 add 메서드의 매개변수와 결과를 기록하여 메서드 호출에 대한 통찰력을 제공합니다.

<div class="content-ad"></div>

# 조건부 타입: TypeScript의 타입 논리

조건부 타입을 사용하면 조건에 따라 복잡한 타입 변환을 수행할 수 있습니다. 유연하고 강력한 타입 정의를 만드는 데 유용합니다.

## 기본 조건부 타입

기본 조건부 타입은 두 개의 타입에 적용되며, 조건에 따라 그 중 하나를 반환합니다:

<div class="content-ad"></div>

<img src="/assets/img/2024-07-12-UnlockingAdvancedTypeScriptFeaturesGenericsDecoratorsandBeyond_5.png" />

여기서 IsString은 타입 T가 문자열인지 확인하고 true이면 "yes"를 반환하고, 그렇지 않으면 "no"를 반환합니다.

## 조건부 타입 추론

조건부 타입은 조건 내에서도 타입을 추론할 수 있습니다.

<div class="content-ad"></div>

![image](/assets/img/2024-07-12-UnlockingAdvancedTypeScriptFeaturesGenericsDecoratorsandBeyond_6.png)

이 예에서 ElementType은 배열의 요소 유형을 추출하거나 배열이 아닌 경우 유형 자체를 반환합니다.

## 💡실제 사용 사례: Deep Readonly

조건부 유형의 실제 사용 사례 중 하나는 깊은 읽기 전용 타입을 만드는 것입니다. 객체의 모든 속성과 하위 속성을 읽기 전용으로 ​​만드는 것입니다.

<div class="content-ad"></div>

![image](/assets/img/2024-07-12-UnlockingAdvancedTypeScriptFeaturesGenericsDecoratorsandBeyond_7.png)

DeepReadonly recursively makes all properties of User readonly, ensuring immutability.

# 결론

제네릭, 데코레이터, 조건부 타입과 같은 고급 TypeScript 기능은 유연하고 재사용 가능하며 안전한 유형의 코드를 작성하는 데 강력한 도구를 제공합니다.

<div class="content-ad"></div>

이러한 기능들을 숙달하면 TypeScript 프로젝트의 품질과 유지보수성을 크게 향상시킬 수 있어요. 복잡한 애플리케이션을 개발하거나 간단한 유틸리티를 만들더라도, 이러한 고급 기술을 활용하면 더 나은, 더 견고한 코드를 작성할 수 있을 거예요.

다음 프로젝트에서 이러한 기능들을 탐험해보고, 개발 프로세스를 변화시킬 수 있는지 확인해보세요.

행복한 코딩하세요! 🍻
