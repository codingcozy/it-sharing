---
title: "NgRx  상태를 강력히 타입 지정하고 셀렉터 만드는 방법"
description: ""
coverImage: "/assets/img/2024-07-13-NgRxStronglyTypingtheStateandBuildingSelectors_0.png"
date: 2024-07-13 18:29
ogImage:
  url: /assets/img/2024-07-13-NgRxStronglyTypingtheStateandBuildingSelectors_0.png
tag: Tech
originalTitle: "NgRx — Strongly Typing the State and Building Selectors"
link: "https://medium.com/@jawadhasan80/ngrx-strongly-typing-the-state-and-building-selectors-d230da95c409"
---

![Screenshot](/assets/img/2024-07-13-NgRxStronglyTypingtheStateandBuildingSelectors_0.png)

# 소개

입문 포스트에서는 NgRx의 기본 사항부터 시작했습니다.

Angular에 맞춘 Redux 패턴인 NgRx를 사용하여 애플리케이션을 위한 중앙 상태 관리로 나아가는 첫걸음을 다뤘습니다.

<div class="content-ad"></div>

우리는 NgRx 스토어를 설치했고 애플리케이션에 연결했어요. Feature Module State Composition을 사용하여 애플리케이션 상태를 작은 조각으로 구조화하여 유지보수를 쉽게할 수 있는 방법을 보았어요.

오늘은 다음 단계로 나아가서 애플리케이션 상태에 강한 타입을 지정할 거에요. 또한 selector를 만들고 사용하여 우리의 상태 조각에서 어떤 정보의 비트를 선택하는 방법도 알아볼 거에요.

# 상태에 강한 타입 할당하기

이전 게시물에서 다음과 같은 코드가 있었어요:

<div class="content-ad"></div>

![이미지](/assets/img/2024-07-13-NgRxStronglyTypingtheStateandBuildingSelectors_1.png)

제네릭 스토어 타입으로 any를 사용하고 있다는 것에 주목해주세요. 우리는 TypeScript 인터페이스를 사용하여 우리의 상태에 강력한 타입을 적용할 수 있습니다.

기본적으로, 각 상태 조각을 위해 여러 인터페이스를 정의하고 이러한 상태를 모두 포함하는 인터페이스를 정의할 수 있습니다.

그러나 먼저 제품 상태 조각에 집중해 봅시다. 먼저, 상태에 대한 이러한 인터페이스를 어디에 정의해야 할까요?

<div class="content-ad"></div>

# 리듀서에서 상태 인터페이스 정의하기

이전 글에서 우리는 Reducer가 애플리케이션 상태를 나타내기 때문에 해당 리듀서와 연결된 특성 상태 인터페이스를 정의하는 것이 합리적이라고 말했습니다.

![이미지](/assets/img/2024-07-13-NgRxStronglyTypingtheStateandBuildingSelectors_2.png)

여기에서는 상태의 제품 슬라이스에 대한 인터페이스를 정의했습니다.

<div class="content-ad"></div>

다른 애플리케이션 기능에 대한 인터페이스를 정의하는 데 이를 따를 수 있어요.

# 애플리케이션 상태 인터페이스 정의

기능 상태와 유사하게, 우리 angular 애플리케이션에 대한 인터페이스를 아래와 같이 정의할 수 있어요

![이미지](/assets/img/2024-07-13-NgRxStronglyTypingtheStateandBuildingSelectors_3.png)

<div class="content-ad"></div>

이제 상태의 완전한 구조가 준비되었습니다. 현재 두 개의 기능 슬라이스가 있습니다. 다른 기능을 추가하면 여기에 추가할 것입니다.

그러나 이 방식에 문제가 있습니다. 어플리케이션 상태로 ProductState를 직접 가져와서 게으른 로딩 경계를 깨버렸습니다. Angular 모듈의 게으른 로딩에 관한 정보는 이 게시물을 확인할 수 있습니다.

그래서 우리는 무엇을 할 수 있을까요?

앱 상태 인터페이스에서 게으르게 로드되지 않는 상태만 유지하겠습니다. 아래에서 표시된 대로요:

<div class="content-ad"></div>

![image 1](/assets/img/2024-07-13-NgRxStronglyTypingtheStateandBuildingSelectors_4.png)

then in the code of the products feature, we will expand the definition of our global application state to include the product slice of state as shown below:

![image 2](/assets/img/2024-07-13-NgRxStronglyTypingtheStateandBuildingSelectors_5.png)

As this code belongs to the product feature, we will confine it within our lazy loading boundaries.

<div class="content-ad"></div>

지금은 전체 상태 트리를 나타내는 단일 상태 인터페이스가 있습니다.

다음과 같이 인터페이스를 사용하여 상태를 강력하게 유형화해봅시다:

![state](/assets/img/2024-07-13-NgRxStronglyTypingtheStateandBuildingSelectors_6.png)

이제 구성 요소를 위해 스토어를 강력하게 유형화할 수 있습니다:

<div class="content-ad"></div>

<img src="/assets/img/2024-07-13-NgRxStronglyTypingtheStateandBuildingSelectors_7.png" />

알림: 리듀서에서 확장된 상태를 가져오기 때문에 우리의 컴포넌트는 전체 애플리케이션 상태에 접근할 수 있습니다.

이제 우리의 상태는 강력하게 유형이 지정되었습니다. 정적 타입 확인이 가능하고 편집기에서 코드 인텔리센스를 제공받게 됩니다. 그러면 버그가 줄어들죠.

우리의 상태를 강력하게 유형으로 지정하는 데 필요한 것은 해당 상태 구조를 설명하는 인터페이스 집합을 정의하는 것뿐입니다. 그런 다음 전역 애플리케이션 상태나 특정 부분 상태를 참조할 때마다 이러한 인터페이스를 타입으로 사용했습니다.

<div class="content-ad"></div>

# 초기 상태 값 설정하기

강력한 유형 설정과 초기 상태 값 지정은 강력한 유형의 이점을 가져다주며 코드를 더 예측 가능하게 만듭니다. 초기 상태를 강력하게 유형화하고 다음과 같이 초기화할 수 있습니다.

![이미지](/assets/img/2024-07-13-NgRxStronglyTypingtheStateandBuildingSelectors_8.png)

애플리케이션을 테스트하고 디버거를 확인하면 우리가 코드에서 정의한 대로 초기 상태가 설정된 것을 볼 수 있습니다.

<div class="content-ad"></div>

<img src="/assets/img/2024-07-13-NgRxStronglyTypingtheStateandBuildingSelectors_9.png" />

다음으로는 selector에 대해 좀 더 자세히 이야기해 봅시다.

# Selector 빌드

다음은 우리 컴포넌트에 있는 저장소 구독 코드입니다:

<div class="content-ad"></div>

<img src="/assets/img/2024-07-13-NgRxStronglyTypingtheStateandBuildingSelectors_10.png" />

현재 다음과 같은 문제가 있습니다.

- 컴포넌트에서 타이핑 오류를 기다리는 문자열 선택기가 있습니다.
- store에서 명시적으로 속성(products.showProductCode)을 검색하며, store 구조에 대한 가정을 하고 있습니다. (즉, store 구조를 변경해야 하는 경우; store를 하위 슬라이스로 구성하는 경우; 모든 선택기를 찾아 코드를 업데이트해야 합니다.)
- 마지막으로, 이 코드는 상태의 products 슬라이스에서 모든 속성의 변경 사항을 감시하므로, showProductCode 속성이 변경되지 않아도 이 코드가 알림을 받습니다.

어떻게하면 더 나아질 수 있을까요?

<div class="content-ad"></div>

답변: 셀렉터

셀렉터는 저장소의 재사용 가능한 쿼리입니다 (인-메모리 상태 정보에 액세스하기 위한 저장 프로시저와 같은 역할).

셀렉터를 사용하면 저장소에 상태의 단일 사본을 유지할 수 있지만 다양한 형태로 투영하여 컴포넌트 및 서비스가 쉽게 액세스할 수 있도록 만들 수 있습니다.

저희 컴포넌트는 저장소에서 상태를 선택하기 위해 셀렉터를 사용하며, 저장소 구조와 컴포넌트 사이에 추상화 수준을 추가합니다.

<div class="content-ad"></div>

# 선택자 사용의 장점

- 강력한 타입화된 API를 제공합니다.
- 스토어와 컴포넌트를 분리합니다.
- 복잡한 데이터 변환을 캡슐화할 수 있습니다.
- 재사용 가능합니다.
- 메모이제이션(캐싱)됩니다.

선택자는 스토어로 들어가 특정 상태를 반환하는 함수입니다.

NgRx 라이브러리에서 제공하는 두 가지 기본 함수 유형은 다음과 같습니다:

<div class="content-ad"></div>

- CreateFeatureSelector: 이 함수는 상태의 feature 조각을 가져올 수 있게 해줍니다. (보통 이 함수는 내보내지 않습니다)
- CreateSelector: 이 함수는 selectors를 조합하여 상태 트리를 탐색하는 것으로 어떤 상태를 얻을 수 있도록 해줍니다. 우리는 이를 사용하여 매우 구체적인 상태를 선택할 수 있습니다. (이것은 내보내기됩니다)

Selector는 순수한 함수여야합니다.

Selector는 reducer가 정의된 곳과 함께 정의할 수 있어서 관련된 것을 유지하는 것이 좋지만, 애플리케이션 내에서 적절한 위치에 두실 수 있습니다. (나중에 이에 대한 일반적인 장소를 살펴보겠습니다).

다음은 내부적으로 이 클래스에는 내보내지 않는 getProductFeatureState feature selector를 정의한 방법입니다. 그런 다음 프로젝터 함수를 사용하는 일반적인 selector인 getProductCode를 정의하고 내보냈습니다.

<div class="content-ad"></div>

![2024-07-13-NgRxStronglyTypingtheStateandBuildingSelectors_11](/assets/img/2024-07-13-NgRxStronglyTypingtheStateandBuildingSelectors_11.png)

Similarly, we can define other selectors for the product as follows

![2024-07-13-NgRxStronglyTypingtheStateandBuildingSelectors_12](/assets/img/2024-07-13-NgRxStronglyTypingtheStateandBuildingSelectors_12.png)

Now that we have defined selectors, let's see how to use them next.

<div class="content-ad"></div>

# 셀렉터 사용하기

컴포넌트 코드를 업데이트하여 다음과 같이 셀렉터를 사용합니다.

![Component Image](/assets/img/2024-07-13-NgRxStronglyTypingtheStateandBuildingSelectors_13.png)

이렇게 하면 코드가 간소화되고 스토어 구조에서 필요에 따라 변경될 수 있어도 여기 컴포넌트에서 변경이 필요하지 않습니다.

<div class="content-ad"></div>

이제 문제없이 애플리케이션이 이전과 동일하게 작동하는지 테스트할 수 있습니다.

# 셀렉터 조합

이전 섹션에서 getShowProductCode 셀렉터를 만들 때 어떻게 셀렉터를 조합하는지 이미 보았습니다. 그러나 추가로 셀렉터를 결합할 수도 있습니다. 아래 예시를 살펴보겠습니다.

![image](/assets/img/2024-07-13-NgRxStronglyTypingtheStateandBuildingSelectors_14.png)

<div class="content-ad"></div>

이 선택기는 제품 ID를 기반으로 현재 제품을 선택하기 위해 배열의 find 메서드를 사용합니다.

우리는 이번 글을 여기서 마치고, 다음 글에서 계속 학습할 예정입니다.

이 애플리케이션의 소스 코드는 해당 Git 저장소에서 사용할 수 있으며, 온라인으로 배포된 애플리케이션은 이 URL에서 접속할 수 있습니다.

# 체크 리스트

<div class="content-ad"></div>

체크리스트 — 강한 타이핑 상태

- 각 상태 조각에 대한 인터페이스를 정의합니다.
- 이러한 인터페이스를 조합하여 전역 애플리케이션 상태를 정의합니다.
- 응용 프로그램 전반에 걸쳐 상태를 강하게 유형화하기 위해 이러한 인터페이스를 사용합니다.
- 예측 가능성을 돕기 위해 각 조각의 초기 상태를 제공합니다.

체크리스트 — 셀렉터

- 재사용 가능한 상태 쿼리를 정의하기 위해 셀렉터를 작성합니다.
- 상태의 피처 조각을 위한 피처 셀렉터를 정의합니다.
- 셀렉터 구성을 사용하여 저장소에서 임의의 상태 조각을 반환하기 위해 일반 셀렉터를 사용합니다.

<div class="content-ad"></div>

# 요약

이 게시물에서는 정적 타입 확인 및 코드 인텔리전스의 이점을 얻는 TypeScript 인터페이스를 사용하여 상태를 강력하게 타입화했습니다. 이러한 인터페이스를 조합하여 전체 애플리케이션 상태를 정의할 수 있습니다.

우리는 selectors를 구축하고 활용하는 방법에 대해 다루었는데, 이는 스토어 구조와 컴포넌트 사이에 좋은 추상화를 제공하며, selectors를 조합하여 상태 정보의 모든 부분에 세심한 제어를 할 수 있습니다.

의견이나 질문이 있으시면 언제든지 알려주세요. 다음에 또 만날 때까지, 즐거운 코딩되세요!

<div class="content-ad"></div>

https://hexquote.com에서 2024년 7월 13일에 처음 게시되었습니다.
