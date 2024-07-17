---
title: "마이크로 프런트엔드를 위한 호스트 애플리케이션 구축 방법"
description: ""
coverImage: "/assets/img/2024-07-06-BuildingaHostApplicationforYourMicroFrontends_0.png"
date: 2024-07-06 10:01
ogImage:
  url: /assets/img/2024-07-06-BuildingaHostApplicationforYourMicroFrontends_0.png
tag: Tech
originalTitle: "Building a Host Application for Your Micro Frontends"
link: "https://medium.com/bitsrc/building-a-host-application-for-your-micro-frontends-b80880648b3e"
---

## 모든 비기능 요구 사항을 충족하는 쉘 애플리케이션 개발

/assets/img/2024-07-06-BuildingaHostApplicationforYourMicroFrontends_0.png

# '쉘 애플리케이션'이란 무엇인가요?

쉘 애플리케이션은 종종 "컨테이너" 또는 "호스트"라고 불리며 다양한 마이크로 프론트엔드를 결합하는 중심 역할을 합니다. 각 마이크로 프론트엔드가 활용할 수 있는 주요 구조와 표준 기능을 제공합니다. 로 웹 응용 프로그램의 개별 요소를 지원하고 연결하는 뼈대로 생각해보세요.

<div class="content-ad"></div>

셸 애플리케이션의 기능 요구 사항은 라우팅 및 네비게이션, 공유 상태 관리, 미크로 프론트엔드의 로딩 및 조정, 그리고 로깅, 분석 등의 공유 서비스 및 유틸리티 제공에 포함됩니다.

## 셸 애플리케이션의 비기능 요구 사항

비기능 요구 사항(NFRs)은 미크로 프론트엔드 아키텍처에서 셸 애플리케이션의 성공과 사용성에 중요합니다. 이러한 요구 사항은 로드 시간, 응답성, 확장성, 보안 등을 포함하여 셸 앱이 충족해야 하는 품질 속성, 성능 및 제약을 정의합니다.

# 고립된 단일 몰리스에서 투명하고 구성 가능한 시스템으로의 전환

<div class="content-ad"></div>

"모든 NFR 상자를 확인하는 핵심은 독립적인 팀과 프로젝트 간에 잘 문서화된 구성 요소를 공유하는 것입니다.

실현될 때 독립된 모놀리스로 구현되는 경우 '쉘 앱'은 마이크로 프론트엔드가 필요한 서비스를 제공할 수 없습니다. 대신, 쉘 앱 팀은 MFE를 위한 구성 요소, 다양한 통합용 SDK 및 유형, 독립적인 MFE의 적절한 개발 및 테스트를 위한 호스트 애플리케이션 컨텍스트를 제공해야 합니다.

공유 가능하고 독립적인 구성 요소로 구성 가능한 시스템을 구축하기 위해 Bit를 사용할 것입니다.

여기서 데모로 사용된 전체 솔루션을 탐색하려면 배포된 솔루션, Bit 조직 및 GitHub 조직을 방문해 주세요."

<div class="content-ad"></div>

아래 블로그 포스트를 확인하여 완전한 해결책에 대해 알아보세요:

## 더 좋은 확장성과 사용성을 위한 공유 형태 구성요소

Bit은 종종 서비스 제공업체와 해당 클라이언트 간에 "엔티티 컴포넌트"를 공유하는 데 사용됩니다. 엔티티 컴포넌트는 전송되는 데이터(서비스에서 클라이언트로 그리고 그 반대로)를 설명하고 유효성을 검사하는 데 사용하는 유형을 제공하며 해당 데이터를 처리하고 조작하는 데 필요한 유틸리티를 제공합니다.

같은 방법론을 사용하여 마이크로 프론트엔드가 셸 응용 프로그램에 통합되는 데이터 엔티티 유형을 설명하는 데 사용할 수 있습니다.

<div class="content-ad"></div>

예를 들어, 우리의 호스트 애플리케이션은 마이크로 프론트엔드가 상단 내비게이션 바에 메뉴를 등록할 수 있도록 합니다.

/assets/img/2024-07-06-BuildingaHostApplicationforYourMicroFrontends_1.png

호스트 앱 팀은 다른 팀들이 이 유형의 통합을 사용하려는 경우 구현해야 하는 인터페이스를 만들었습니다.

예를 들면:

<div class="content-ad"></div>

```js
수출 인터페이스 NavItem {
  라벨: 문자열;
  경로: 문자열;
  아이콘?: 문자열;
  자식?: NavItem[];
}
```

그런 다음 해당 유형은 Bit 구성 요소로 공유되어 다양한 Micro 프론트 엔드(별도의 저장소에서 유지됨) 간에 사용할 수 있습니다:

/assets/img/2024-07-06-BuildingaHostApplicationforYourMicroFrontends_2.png

이 예에서는 인터페이스가 "Storefront" 및 "Blog" MFE(Micro Frontend)에서 구현됩니다:

<div class="content-ad"></div>

/assets/img/2024-07-06-BuildingaHostApplicationforYourMicroFrontends_3.png

예를 들면:

## 일관성, 성능 및 개발 속도를 위해 UI 컴포넌트 공유하기

UI 컴포넌트를 공유하는 것은 새로운 것이 아닙니다. 핵심은 개발과 사용자 경험 사이에 타협을 만들지 않는 방식으로 이루어져야 한다는 것입니다.

<div class="content-ad"></div>

UI 구성 요소는 독립적인 조각으로 공유되어야 합니다. 이를 통해 MFE 팀은 필요한 구성 요소를 선택하고 구성 요소 버전을 점진적으로 업그레이드할 수 있습니다 (프로젝트에 의미없는 업데이트를 피할 수 있습니다). 이는 UI 구성 요소가 "메가 라이브러리"로 공유될 때 보통 어떻게 되는지와는 반대되는 접근 방식입니다.

단일 버전을 포함하는 메가 라이브러리는 여러 구성 요소에 대해 하나의 버전을 강제로 제공하여 MFE 팀이 종종 "모두 또는 아무것도" 상황으로 밀어 넣는 결과로 이어지며, 이는 종종 숨겨진 해결책(주로 처음부터 새로 만드는 것)을 생성하고, 최적화되지 않은 번들 및 UI/UX의 일관성 부족을 야기할 수 있습니다.

/assets/img/2024-07-06-BuildingaHostApplicationforYourMicroFrontends_4.png

## 개선된 DevX 및 애플리케이션 견고성을 위한 컨텍스트 구성 요소 공유

<div class="content-ad"></div>

앱 셸 컨텍스트를 구성하는 컨텍스트 컴포넌트는 MFE(Micro Frontend)에 의해 개발되는 데 사용하고 공유할 수 있습니다.

공유 컨텍스트 컴포넌트

한 가지 방법은 컨텍스트 컴포넌트로 구성 요소 테스트 및 미리보기를 래핑하는 것입니다. 예를 들어:

```js
/**
 * @filename: app-bar.composition.tsx (the preview file for the app-bar)
 */

/**
 * MFE 내에서 컴포넌트를 테스트하고 미리보기하려면 공유 컨텍스트를 가져오세요
 */
import { ThemeProvider } from "@bit-bazaar/design.theme.theme-provider";
import { AuthProvider } from "@bit-bazaar/shell-app.auth.auth-provider";
import { AppBar } from "./app-bar";

export const AppBarPreview = () => (
  <ThemeProvider>
    <AuthProvider mock>
      <AppBar />
    </AuthProvider>
  </ThemeProvider>
);
```

<div class="content-ad"></div>

불변의 쉘 애플리케이션 인스턴스

개발을 향상시키는 또 다른 방법은 전체 앱 쉘을 불변 패키지로 공유하는 것입니다. 이를 통해 MFE 팀은 MFE를 실행하면서 쉘 애플리케이션에 실수로 변경이 가해지는 것을 방지할 수 있습니다(다른 팀에서 유지보수).

이를 위해 '플랫폼'이라는 특정 컴포넌트의 도움을 받습니다. 이러한 컴포넌트는 프론트엔드 및 백엔드 애플리케이션의 조정자입니다. 이들을 통해 여러 마이크로서비스와 마이크로 프론트엔드로 구성된 완전한 시스템을 빠르게 실행할 수 있습니다. 일부는 여러분이 유지보수하고, 다른 일부는 개발 목적으로만 소비됩니다.

예를 들어, '스토어프론트'팀은 불변의 쉘 애플리케이션 내에서 '스토어프론트' MFE를 실행하여(쉘 앱 및 기타 MFE 포함) 전체적인 컨텍스트에서 MFE를 실행할 수 있습니다. 이 팀은 MFE를 실행한 다음 이를 사용하는 호스트 앱을 실행해주는 플랫폼 컴포넌트를 사용합니다.

<div class="content-ad"></div>

/assets/img/2024-07-06-BuildingaHostApplicationforYourMicroFrontends_5.png

```js
bit run storefront-platform
```

/assets/img/2024-07-06-BuildingaHostApplicationforYourMicroFrontends_6.png

Module Federation with Bit에 관한 블로그 포스트를 읽어보세요. 자신만의 ModFed 플랫폼 컴포넌트 버전을 생성하는 방법을 배울 수 있습니다.
