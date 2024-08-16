---
title: "직관적인 인터페이스를 위한 디자인 Jetpack Compose에서 모디파이어 기반 툴팁 구현 하는 방법"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-07-19 13:35
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Designing Intuitive Interfaces A Take on Modifier-based Tooltips in Jetpack Compose"
link: "https://medium.com/@michellbak/designing-intuitive-interfaces-a-guide-to-tooltips-in-jetpack-compose-ac37b355f43d"
isUpdated: true
updatedAt: 1723812218488
---



앱 개발 분야에서 계속 변화하는 환경 속에서 직관적이고 사용자 친화적인 인터페이스를 만드는 것은 어떤 프로젝트의 성공에 중요합니다.

사용자 경험을 향상시키는 데 개발자의 강력한 도구 중 하나는 툴팁입니다. 이러한 주석은 불가해하지만 정보 제공이 필요한 요소로, 사용자에게 보물 같은 도움을 제공하여 애플리케이션의 복잡성을 안내하고 그 기능을 최대한 활용할 수 있도록 돕습니다.

저희 Public에서는 최근 앱과 사용자 경험의 중요한 요소인 포트폴리오 화면을 재설계하는 데 상당한 시간과 노력을 투자했습니다.

이러한 변경 사항을 구현할 때, 회원들이 업데이트에 순조롭게 적응하면서 개선 사항을 소개받을 수 있도록 익숙함을 유지하는 것이 중요합니다. 이를 위한 툴팁 UI 구성요소는 이 목표를 달성하는 데 중요한 역할을 합니다.

<div class="content-ad"></div>

# Jetpack Compose에서 툴팁

저희 Public의 안드로이드 팀은 UI 코드 작성을 위해 Jetpack Compose를 전적으로 활용하고 있습니다. 따라서 저희의 툴팁 UI 구성 요소 또한 Compose를 기반으로 해야 합니다.

Compose 프레임워크 자체에는 내장된 툴팁 구성 요소가 없지만, 안드로이드 개발자 커뮤니티에서는 여러 대안을 제공하고 있습니다. 게다가 Compose를 위한 Material Design 3는 최근 v1.1 릴리스에서 새로운 옵션을 도입했습니다.

다양한 선택지가 있지만, 우리는 일관되게 공통된 문제에 직면해왔습니다: 모든 선택지는 컴포넌트를 부모 요소로 래핑해야 한다는 것입니다. 이 접근 방식은 선언적 UI API에서 흔한 것이지만, 유연성이 떨어지며, 특히 여러 툴팁이 있는 워크스루 화면을 개발할 때 번거로울 수 있습니다.

<div class="content-ad"></div>

이상적으로는 우리는 Modifier 기반 솔루션이 필요했습니다. 이를 통해 어떤 컴포넌트에 Modifier를 쉽게 적용하여 주변에 사용자 정의 툴팁을 표시할 수 있게 되었습니다.

# Modifiers로 도와주기

우리는 우리의 요구 사항과 완벽하게 일치하는 API를 개발하는 데 성공했습니다:

```js
@Composable
fun

<div class="content-ad"></div>

API를 사용하면 선택적 제목을 포함한 툴팁을 표시할 수 있고, 툴팁 위치 지정, 구성 요소 강조 및 기타 측면을 단순화하는 구성 옵션 배열을 제공합니다.

어떤 구성 요소에든 툴팁을 쉽게 추가할 수 있습니다:

MyComponent(
    modifier = Modifier.tooltip(
        text = "This is a tooltip",
        enabled = true
    )
)

그리고 이것이 동작 모습입니다:

<div class="content-ad"></div>

효율적으로 측정하면서 UI 구성 요소를 렌더링하기 위해 Compose 코드를 실행하는 Modifier 함수에는 @Composable 주석이 표시되어 있습니다. 이는 툴팁 자체, 반투명 오버레이 및 다양한 애니메이션을 처리하기 위해 Compose 코드를 실행하기 때문에 필요합니다.

그러나 중요한 점은 Popup 구성 요소를 활용한다는 것입니다. Popup은 현재 활동 위에 나타나는 부유 컨테이너입니다. 여러 개발자들이 익숙하지 않은 덜 알려진 구성 요소입니다. 저는 개인적으로 이에 대해 알지 못했지만 다행히도 동료 중 한 명이 지금 나의 구현을 작성하려고 할 때 (알지 못하고) 내 주의를 집중시켜 주었습니다.

코드의 다양한 세부 사항에 대해 깊이 파고들 수 있지만 지루하지 않도록 유지하기 위해 그것들을 모두 건너뛰고, 코드 자체가 말을 대신하도록 하여 우리가 생각한 구현의 간소화 버전을 제공하겠습니다.

<div class="content-ad"></div>

위의 코드 조각에서는 ArrowTooltip의 구현을 직접 해야하지만, 다행히 온라인에서 신뢰할 수 있는 많은 옵션이 있습니다. 또한, 오버레이와 툴팁이 나타날 때 각자의 애니메이션 세트를 추가하여 더욱 흥미롭게 만들 수 있습니다.

우리의 테스트에서 tooltip Modifier가 잘 작동했지만, 여전히 초기 단계에 있다는 점을 주의해야 합니다. 최적화할 부분이 있을 수 있고, 해결해야 할 몇 가지 남아 있는 버그가 있을 수도 있습니다.

사용 경험이 달라질 수 있지만, 여러분의 유용성이나 피드백 및 개선 제안에 대한 소식을 듣고 싶습니다.

시도해보고 싶다면, Google Play에서 Public 앱을 다운로드하시기를 권장합니다.
```
