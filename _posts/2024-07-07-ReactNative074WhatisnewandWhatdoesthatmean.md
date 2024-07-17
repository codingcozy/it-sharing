---
title: "React Native 074의 새로운 기능과 그 의미는"
description: ""
coverImage: "/assets/img/2024-07-07-ReactNative074WhatisnewandWhatdoesthatmean_0.png"
date: 2024-07-07 19:04
ogImage:
  url: /assets/img/2024-07-07-ReactNative074WhatisnewandWhatdoesthatmean_0.png
tag: Tech
originalTitle: "React Native 0.74: What is new? and What does that mean?"
link: "https://medium.com/javascript-in-plain-english/react-native-0-74-what-is-new-and-what-does-that-mean-6705fbba4167"
---

![이미지](/assets/img/2024-07-07-ReactNative074WhatisnewandWhatdoesthatmean_0.png)

# 소개

React Native의 최신 버전 0.74가 4월 22일에 출시되었습니다. 더 많은 정보를 확인하려면 공식 문서에서 확인할 수 있습니다. 이에 대해 이야기하고 싶었지만, 직접 테스트하고 리액트 네이티브 팀과 커뮤니티가 한 놀라운 작업을 확인한 후에 이야기하려고 했습니다.

이 가이드에서는 이 버전의 새로운 내용에 대해 간략히 설명하겠지만, 먼저 이 가이드에서 사용되는 기술 용어에 대한 소개부터 시작하여 이해하고 엔지니어링 기술을 더 발전시킬 수 있도록 하겠습니다.

<div class="content-ad"></div>

시작해 봅시다:

# 용어 및 정의:

## React Native의 Yoga:

Yoga는 React Native에서 사용되는 C++로 작성된 레이아웃 엔진으로, 화면에 요소의 위치를 결정하는 데 사용됩니다. 레이아웃 지시(플렉스박스 등)를 사용하여 각 요소의 정확한 좌표를 계산합니다.

<div class="content-ad"></div>

용어 설명: React Native 앱에 버튼과 텍스트 요소가 있는 상황을 상상해보세요. Yoga는 당신의 flexbox 속성(예: flex: 1)을 사용하여 각 요소가 얼마나 많은 공간을 차지하고 서로 어떻게 위치해야 하는지 결정하는 것처럼 동작합니다. 이것은 여러 기기에서 응용 프로그램의 UI가 의도한 대로 보이도록 보장합니다.

## 브릿지 없는 아키텍처:

브릿지 없는 아키텍처는 React Native 0.74에서 새로운 접근 방식으로, 특정 시나리오에서 자바스크립트와 네이티브 코드 간의 통신 레이어(브릿지)를 제거합니다. 이 브릿지는 일반적으로 두 환경 간의 데이터를 전달하는 데 사용됩니다. 브릿지 없는 모드는 더 직접적인 통신을 가능하게 하여 성능을 향상시킵니다.

용어 설명: React Native 앱을 섬(JavaScript 코드)으로, 네이티브 플랫폼(Android 또는 iOS)을 본토로 생각해보세요. 보통 데이터는 상호작용을 위해 브릿지를 통해 전송됩니다. 브릿지 없는 아키텍처는 어떤 상황에서는 섬과 본토를 직접 연결하는 보트처럼 동작하여 더 빠른 통신을 제공합니다. 이는 전체 브릿지 기능이 필요하지 않은 구체적인 작업의 성능을 향상시킵니다.

<div class="content-ad"></div>

## Yarn:

Yarn은 JavaScript 프로젝트의 종속성(라이브러리)을 관리하기 위해 특별히 설계된 패키지 관리자입니다. 이를 통해 이러한 라이브러리를 효율적으로 설치, 업데이트 및 제거할 수 있습니다. React Native 프로젝트는 기능을 수행하는 데 다양한 라이브러리에 의존합니다. Yarn은 이를 추적하고 올바른 버전을 보장하는 데 도움을 줍니다.

## Android SDK:

Android SDK(Software Development Kit)는 Google이 Android 앱을 개발하기 위해 제공하는 도구 및 자원의 모음입니다. 컴파일러, 디버거와 같은 도구와 문서와 같은 필수 구성 요소를 포함하여 개발자가 앱을 작성하고 테스트할 수 있도록 도와줍니다.

<div class="content-ad"></div>

# React Native 0.74에서 새로운 내용:

1. 개선된 레이아웃 엔진 (Yoga 3.0):

- 새 버전의 Yoga 레이아웃 엔진은 스타일링의 예측 가능성을 향상시켜 레이아웃 동작이 다양한 기기와 화면 크기에서 더 일관되도록 보장합니다.
- 또한 웹 구성 요소를 더 잘 지원하여 React Native 및 웹 프로젝트 간에 스타일 지식을 공유하기 쉽게하며 코드 재사용성과 유지 보수성을 개선합니다.

2. 기본적으로 브릿지 없음 (새 아키텍처):

<div class="content-ad"></div>

- 새 아키텍처는 Bridgeless Mode를 우선시하여 JavaScript와 네이티브 코드 간의 전통적인 브리지를 제거합니다.
- 이 변경으로 인해 브리지와 관련된 오버헤드를 줄여 성능이 크게 향상되어 실행 속도가 빨라지고 애니메이션이 더 부드러워집니다.
- 또한 JavaScript와 네이티브 레이어 간의 통신이 간소화되어 버그 가능성이 줄고 전체 앱 안정성이 향상됩니다.

3- 일괄 처리된 onLayout 업데이트 (새 아키텍처):

- 일괄 처리된 onLayout 업데이트의 도입은 여러 onLayout 콜백을 단일 배치로 그룹화하여 성능을 최적화합니다.
- 이로 인해 필요한 레이아웃 계산과 다시 렌더링의 횟수가 줄어들어 좀 더 효율적인 렌더링과 부드러운 사용자 인터페이스가 가능해집니다.
- 복잡한 레이아웃에서 성능이 향상되고 레이아웃 변경 중 CPU 사용량이 줄어드는 것을 개발자들이 눈치챌 것입니다.

4- 기본 패키지 매니저로 Yarn 3:

<div class="content-ad"></div>

- 새로운 프로젝트에 대한 기본 패키지 관리 도구로 React Native 0.74는 이제 Yarn 3를 사용하며, Yarn Classic을 대체합니다.
- Yarn 3는 더 빠른 패키지 설치, 개선된 의존성 해결, 그리고 최신 JavaScript 기능에 대한 더 나은 지원을 제공합니다.
- 이 변경으로 개발 흐름이 간소화되고, 프로젝트가 패키지 관리의 최신 기술 발전을 활용할 수 있게 됩니다.

## 주요 변경 사항:

1- PropTypes 삭제:

- 번들 크기를 줄이고 성능을 개선하기 위해 PropTypes가 React Native 코어 패키지에서 삭제되었습니다.
- 개발자들은 TypeScript를 사용하거나 prop-types 패키지를 별도로 가져와서 자체 타입 체크 솔루션을 구현해야 합니다.

<div class="content-ad"></div>

2- PushNotificationIOS 변경 사항:

- PushNotificationIOS API가 큰 변화를 겪었으며, 모듈화되어 커뮤니티 중심적인 접근을 가능케 합니다.
- 개발자들은 새 API 구조에 맞게 코드를 업데이트해야 하며, 업데이트된 문서를 참고하여 안내를 받아야 합니다.

3- 최소 Android SDK 업데이트:

- React Native 프로젝트의 최소 필수 Android 버전은 이제 Android 6.0 (SDK 23)입니다.
- 이 변경은 성능을 향상시키고 새로운 Android 기능의 사용을 허용하며 오래된, 효율적이지 않은 API 지원을 폐기함으로써 앱 크기를 줄일 수 있습니다.
- 오래된 Android 버전을 대상으로 하는 개발자들은 프로젝트 구성을 업데이트하고 지원되는 기기에서 앱을 테스트해야 합니다.

<div class="content-ad"></div>

# 결론

이것들은 React Native 0.74의 최신 변화들입니다. 제 개인적인 견해로는 이 버전을 적극 추천합니다. 변경 사항에 대한 자세한 내용은 공식 문서에서 확인할 수 있습니다.

# 쉽게 이해하기 🚀

In Plain English 커뮤니티에 참여해 주셔서 감사합니다! 떠나시기 전에:

<div class="content-ad"></div>

- 작가에게 박수를 보내고 팔로우 해주세요 ️👏️️
- 팔로우하기: X | LinkedIn | YouTube | Discord | 뉴스레터
- 다른 플랫폼 방문하기: CoFeed | Differ
- 더 많은 콘텐츠: PlainEnglish.io
