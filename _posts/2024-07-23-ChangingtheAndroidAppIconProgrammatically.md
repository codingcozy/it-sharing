---
title: "안드로이드 앱 아이콘을 프로그래밍으로 변경하는 방법"
description: ""
coverImage: "/assets/img/2024-07-23-ChangingtheAndroidAppIconProgrammatically_0.png"
date: 2024-07-23 21:57
ogImage: 
  url: /assets/img/2024-07-23-ChangingtheAndroidAppIconProgrammatically_0.png
tag: Tech
originalTitle: "Changing the Android App Icon Programmatically"
link: "https://medium.com/@callmeryan/changing-the-android-app-icon-programmatically-c913550330d"
---


요즘, 𝕏 Blue 사용자들이 앱 아이콘을 변경할 수 있는 기능이 공지되었습니다. 이 기능은 Reddit 앱도 비슷한 기능을 제공하고 있습니다.

![이미지](/assets/img/2024-07-23-ChangingtheAndroidAppIconProgrammatically_0.png)

온라인에서 해결책을 찾는 것은 비교적 쉽습니다. 그러나 나에게 흥미로운 점은 이를 구현하는 방법을 묻는 사람들이 StackOverflow에 올리면서 받는 반응이었습니다. 대부분의 응답자들은 이게 불가능하다고 주장하지만, 우회 방법이 실제로 존재한다는 사실을 무시합니다.

# 우회 방법이 있다면, 그것은 불가능한 일이 아닙니다.

<div class="content-ad"></div>

![이미지](/assets/img/2024-07-23-ChangingtheAndroidAppIconProgrammatically_1.png)

이 해결책은 여러 해 동안 돌고 있습니다. 일부 사용자들은 특정 폰에서 문제가 발생했다고 보고했지만, 이 방법은 제게는 작동합니다. 그러므로 구현이 어렵지 않다는 점 때문에 여기에 공유하고 싶습니다.

## 작동 원리

AndroidManifest.xml에서 activity-alias 요소를 선언하여 여러 런처 아이콘을 생성할 수 있습니다. 앱 아이콘을 변경하는 효과는 현재 아이콘을 비활성화하고 새로운 아이콘을 활성화하여 달성됩니다.

<div class="content-ad"></div>

# AndroidManifest.xml에 앱 아이콘 정의하기

앱 아이콘을 프로그래밍적으로 변경하려고 노력하지만, 여전히 미리 정의된 일련의 아이콘을 하드코딩해야 합니다. 이를 통해 우리는 프로그래밍적으로 그들 사이를 전환할 수 있게 됩니다. 예를 들어, 다음 코드는 기본 아이콘 외에 세 개의 추가 런처 아이콘을 생성합니다.

주의할 점:

- 별칭(alias)으로 생성하는 activity-alias를 위해 구체적인 클래스를 구현할 필요는 없습니다. 기존 targetActivity를 가리키는 한 됩니다.
- 원래 MainActivity가 더 이상 android.intent.action.MAIN에 응답하지 않기 때문에, 기본 activity-alias를 활성화해야만 합니다.
- 아이콘 외에 라벨도 변경할 수 있습니다.
- 필요한 intent-filter를 반드시 포함해야 합니다. 그렇지 않으면 특정 activity-alias가 활성화될 때 앱을 실행할 수 없게 됩니다.

<div class="content-ad"></div>

# 아이콘 선택을 위한 UI 생성

구현 방식은 완전히 우리에게 달려 있습니다. ViewModel 및 Repository를 사용하여 사용 가능한 아이콘 목록 및 현재 선택 항목을 관리하는 복잡한 접근 방식을 선택할 수도 있습니다. 활동 별칭(activity-alias로 정의한 구성 요소 이름과 아이콘을 올바르게 연결할 수 있다면 작동할 것입니다.

![아이콘 선택을 위한 UI](/assets/img/2024-07-23-ChangingtheAndroidAppIconProgrammatically_2.png)

# 앱 아이콘 활성화/비활성화하기

<div class="content-ad"></div>

기술적으로는 구성 요소(활동 또는 활동 별칭)를 프로그래밍 방식으로 활성화 또는 비활성화하고 있습니다. 각 구성 요소에는 다른 아이콘이나 이름이 함께 제공됩니다.

## 구성 요소 활성화

AndroidManifest.xml에는 클래스 이름만 제공할 수 있지만, 구성 요소 이름을 지정할 때는 패키지 이름을 포함한 완전히 정규화된 클래스 이름을 제공해야 합니다. setComponentEnabledSetting()을 COMPONENT_ENABLED_STATE_ENABLED 상태로 실행한 후에는 런처가 새로운 앱 아이콘을 표시해야 합니다.

## 구성 요소 비활성화

<div class="content-ad"></div>

위의 과정과 동일하지만 이번에는 상태를 COMPONENT_ENABLED_STATE_DISABLED로 변경합니다. 일반적으로는 하나의 구성요소만 활성화하려고 하지만, 나머지 모두를 비활성화해야 할 수도 있습니다.

주의할 점:

- 현재 활성 상태인 구성요소를 비활성화하려고 하면, DONT_KILL_APP 플래그를 지정해도 앱이 종료됩니다.
- 위에서 언급한대로 MainActivity가 더 이상 인텐트 필터에 응답하지 않기 때문에 “Run App” 기능을 사용하여 앱을 실행할 수 없을 수 있습니다. 대신 gradle 명령줄을 사용하여 앱을 설치해야 합니다.
- 구성요소를 개별적으로 반복하지 않고 아이콘 목록이 있는 경우, packageManager.setComponentEnabledSettings()를 사용하여 설정 목록을 전달할 수 있습니다(API 레벨 33 이상 필요).

# 모두 함께 적용하기

<div class="content-ad"></div>

- AndroidManifest.xml 파일에서 가능한 앱 아이콘 옵션을 정의합니다.
- 사용자가 아이콘을 선택할 수 있는 화면을 제공합니다.
- 사용자의 현재 설정을 사용자 환경 설정에 저장하여 선택 화면에서 표시할 수 있습니다. 그러나 각 구성 요소의 상태를 확인하는 가장 정확한 방법은 packageManager.getComponentEnabledSetting()을 사용하는 것입니다.
- 사용자가 아이콘을 선택한 후, 선택한 구성 요소를 활성화하고 나머지는 비활성화하여 여러 개의 아이콘이 생성되지 않도록 합니다.
- 이후 앱은 자동으로 종료될 가능성이 있습니다. Reddit은 이러한 상황이 발생하기 전에 빠르게 확인 대화상자를 표시합니다.

![이미지](/assets/img/2024-07-23-ChangingtheAndroidAppIconProgrammatically_3.png)

# 샘플 앱

![이미지](/assets/img/2024-07-23-ChangingtheAndroidAppIconProgrammatically_4.png)

<div class="content-ad"></div>

만약 직접 코딩하기 귀찮다면, 깃허브에 예제 앱이 있어서 빠르게 테스트해 볼 수 있어요.

PS: 그렉의 팬이에요. 😛

# 마지막으로

어떤 사람은 이 방법이 딥링크와 같은 일부 기능을 망가뜨린다고 말하지만, 저는 이것이 레딧 앱에서 발견된 구현임을 말할 수밖에 없어요.

<div class="content-ad"></div>

네, 아이콘 또는 앱 이름이 변경되는 것 이상의 기능을 활용하여 컴포넌트의 활성화/비활성화 기술을 사용할 수 있습니다. 무엇이든 상상해보고 구현할 수 있는 다른 기능이 무엇인지 확인해보세요!