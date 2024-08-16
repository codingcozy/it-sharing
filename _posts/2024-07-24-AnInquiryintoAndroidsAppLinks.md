---
title: "Android 앱 링크 사용 방법 정리"
description: ""
coverImage: "/assets/img/2024-07-24-AnInquiryintoAndroidsAppLinks_0.png"
date: 2024-07-24 12:06
ogImage: 
  url: /assets/img/2024-07-24-AnInquiryintoAndroidsAppLinks_0.png
tag: Tech
originalTitle: "An Inquiry into Androids App Links"
link: "https://medium.com/gitconnected/an-inquiry-into-androids-app-links-2d6918666c9e"
isUpdated: true
updatedAt: 1723813129098
---



파이어베이스가 내년 2025년 8월에 다이나믹 링크 기능을 폐기한다는 점을 감안하면, 최근에 우리는 방향 전환을 했고 안드로이드 앱 링크를 우리 애플리케이션에 통합하기 시작했습니다. 이 과정은 특별히 고통스럽지는 않았지만, 공식 문서에 몇 가지 빈틈과 함정이 있음을 알아 차렸고, 같은 프로세스를 거치는 사람들을 위해 이 기사에서 이를 다루고자 합니다.

따라서, 이 기사에서는 저가 앱에 앱 링크를 완전히 통합할 수 있게 해준 프로세스에 대해 설명하겠습니다. 이는 로컬 디버그 및 릴리스 빌드와 플레이스토어의 프로덕션 빌드와 같은 모든 종류의 빌드와 작동합니다.

시작하기 전에 Android의 앱 링크에 대해 간단히 소개하겠습니다. 공식 문서에 설명된대로, Android의 앱 링크는 일반 딥 링크의 하위 범주로, 모바일 기기에서 URL이 클릭될 때 명령에 따라 애플리케이션을 엽니다. 예를 들어, https://yourapp.com 링크로 이동할 때 YouApp 애플리케이션이 열리도록 설정되어 있다면입니다.

# 시작하기

<div class="content-ad"></div>

이 프로세스를 시작하는 두 가지 주요 방법이 있습니다. Android Studio 내의 App Link Assistant 패널을 사용하거나 AndroidManifest.xml 파일을 직접 열고이 프로세스를 수동으로 진행할 수 있습니다. 이 글에서는 최대한 병렬로 두 경로를 설명하겠지만, AS 패널이 말하는 대로가 아닌 코드가 올바르게 수행되는 것을 우선시할 것입니다.

## Android Manifest 작성

Android Studio의 App Links Assistant 패널을 사용할 계획이 없다면 다음 섹션으로 건너뛰세요. 그렇지 않으면 맥에서 다음 뷰 "도구 창" "앱 링크 지원"을 열어 시작할 수 있습니다. 그런 다음 도움 패널 내의 Create Applink 버튼을 클릭하여 프로세스를 시작할 수 있습니다.

![](/assets/img/2024-07-24-AnInquiryintoAndroidsAppLinks_0.png)

<div class="content-ad"></div>

"Create Applink" 버튼을 클릭한 후 App Links Assistant 마법사 안으로 들어오게 됩니다. 단 4단계로 우리의 App Link를 만들 수 있다고 제안하고 있습니다.

![이미지](/assets/img/2024-07-24-AnInquiryintoAndroidsAppLinks_1.png)

첫 번째 단계의 "Open URL Mapping Editor" 버튼을 클릭하면 URL Mapping 호스트 URL 목록이 표시되는 새 창이 열립니다. 이 목록에 새 URL을 추가하려면 '+' 버튼을 클릭하여 이전에 언급한 예제 URL인 https://yourapp.com을 입력할 수 있습니다. 호스트 목록에 이 URL을 추가하면 AndroidManifest.xml 파일 내에 다음과 같은 코드가 생성됩니다:

```js
<intent-filter android:autoVerify="true">
  <action android:name="android.intent.action.VIEW" />
  
  <category android:name="android.intent.category.DEFAULT" />
  <category android:name="android.intent.category.BROWSABLE" />
  
  <data android:scheme="https" />
  <data android:host="yourapp.com" />
```

<div class="content-ad"></div>

## 안드로이드 매니페스트 선언 설명

만일 어시스턴스 패널을 사용하지 않는다면, 위의 코드 블록을 시작점으로 삼아보세요. AndroidManifest.xml 파일 안에 이 블록을 추가하시면 됩니다. 이때, 해당하는 앱 링크가 트리거될 때 열리길 원하는 특정 활동(activity)의 `activity` 태그 내부에 이 블록을 추가하시면 됩니다.

이 코드 조각으로부터 설명할 부분이 몇 가지 있습니다. 그러므로 다음으로 나아가기 전에 이 6줄의 코드에 대해 잠시 살펴보겠습니다.

우선적으로, `intent-filter` 태그는 일반적으로 어떤 종류의 인텐트 필터를 선언하는 데 사용됩니다. 예를 들어, 안드로이드 서비스나 브로드캐스트 리시버도 동일한 방식으로 선언될 것입니다. 더불어, 저희가 초기 태그에 적용하는 android:autoVerify="true" 속성은 앱 링크를 설정할 때 안드로이드가 포함하길 원하는 요소입니다. 공식 문서에 설명된 대로, 이 속성은 특정 유형의 링크의 기본 핸들러로 이 인텐트 필터를 지정할 수 있게 합니다. 이는 우리가 정의한 링크를 사용자가 클릭할 때 애플리케이션이 이미 설치되어 있다면 자동으로 열리게 됨을 뜻합니다.

<div class="content-ad"></div>

다음 줄에서는 Google 검색 사이트에서 활동이 접근 가능하다는 것을 선언하는 ACTION_VIEW Intent 작업을 선언합니다.

이어서, BROWSABLE 및 DEFAULT 두 가지 다른 Intent 범주를 선언합니다. BROWSABLE은 intent 필터가 웹 브라우저에서 접근할 수 있도록 허용하며, DEFAULT는 필터가 암시적 intent에 응답할 수 있도록 단순하게 허용합니다.

마지막으로, 위에서부터의 코드 블록의 마지막 두 줄은 이 웹 사이트가 도달해야 하는 호스트 URL과 스키마를 정의합니다. 우리의 경우, 우리는 https 프로토콜에 따라 도달되는 상기 yourapp.com을 사용하고 있습니다. 우리는 쉽게 http의 보조 스키마를 추가할 수 있지만, 이 글에 대해서는 그 부분을 선택사항으로 남겨둘 것입니다.

# App 링크 처리

<div class="content-ad"></div>

AndroidManifest.xml 파일 안에 intent-filter가 정의되어 있으면, 다음 단계는 앱 내에서 수신된 앱 링크 인텐트를 처리하는 코드를 구현하는 것입니다. Android Studio의 App Links Assistant 패널로 돌아가면, 이것이 가이드에서 STEP 2가 됩니다.

![image](/assets/img/2024-07-24-AnInquiryintoAndroidsAppLinks_2.png)

두 번째 단계에서 Select Activity 버튼을 클릭하면 Activity를 선택하라는 메시지가 나오며, 해당 Activity의 onCreate() 함수 안에 다음의 기본 코드가 추가될 것입니다:

```kotlin
// 주의: 이 코드는 앱 링크를 처리하기 위해 자동으로 생성되었습니다.
val appLinkIntent: Intent = intent
val appLinkAction: String? = appLinkIntent.action
val appLinkData: Uri? = appLinkIntent.data
```

<div class="content-ad"></div>

이 코드는 특별히 "잘못"되었다고 할 수는 없지만, 모든 관련 데이터를 수집하는 최소한만 하는 것입니다. 대신, 우리는 가져올 수 있고, 확인하고, 그런 다음에만 앱 링크 데이터를 처리해야 합니다. 우리가 찾는 것을 가지고 있는지 알았을 때에만! 다음 코드는 이 모든 것을 더 포괄적으로 처리하는 방법의 예제로 제공됩니다. 이에 대해 자세히 다루겠습니다:

```js
// Android view holder 클래스에서 null이 아닌 Intent 가져 오기; 
// 예: Activity 또는 Fragment.
fun handleAppLinkIntent(intent: Intent) {
  intent.data?.let { data ->
    data.host?.let { host ->
      if (host.contains("yourapp")) {
        Log.d(TAG, "Data=$data를 가진 App Link intent를 받았습니다")
        // 데이터를 사용하여 작업 수행
      }
    }
  } ?: Log.v(TAG, "사용할 앱 링크 intent가 없습니다")
}
```

위의 함수는 입력으로 null이 아닌 Intent를 사용합니다. 이는 일반적으로 onCreate() 또는 onResume() 함수 중 하나에서 Activity 또는 Fragment에서 가져올 수 있습니다. 이 함수는 원하는 호스트/웹 사이트의 App Link인지 확인하기 위한 필요한 확인 작업을 수행합니다. 이러한 모든 확인 작업을 확인할 수 있으면 추가적인 함수를 수행해야 합니다.

## URL 데이터 처리

<div class="content-ad"></div>

앱 링크라는 Android의 기능으로 딥 링크를 클릭한 후 앱을 열기만 하는 것은 아닙니다. 일반적으로 우리는 사용 중인 URL을 가져와서 더 많은 데이터를 추출하려고 할 것입니다. 이를 위해 이전에 식별한 intent.data 객체를 가져와서 더 많은 정보를 추출할 수 있도록 다른 함수를 실행할 것입니다.

다음 URL을 예시로 사용해 봅시다: https://myapp.com/?extra=69.

다음 섹션에서 할 일은 extra라는 매개변수를 가져와서 그 값을 69와 같이 우리 앱의 다른 곳에서 더 사용할 수 있도록 하는 것입니다.

알다시피 intent.data는 Uri 형식으로 포맷되어 있습니다. 그러나 특정 예시에서는 실제 데이터가 URL 형식입니다. 따라서 URLDecoder 클래스의 편의성을 이용하여 우리 URL을 마음대로 조작할 수 있습니다.

<div class="content-ad"></div>

```kotlin
@Throws(UnsupportedEncodingException::class)
fun Uri.getUrlParamExtra(): String {
  URLDecoder.decode(toString(), "UTF-8").let {
    return Uri.parse(it).getQueryParameter("extra").orEmpty()
  }
}
```

위의 Uri 확장 기능은 extra 매개변수를 추출하기 위해 URLDecoder 클래스를 사용합니다. 데이터에 문제가 있거나 매개변수가 없는 경우 빈 문자열을 반환합니다. URLDecoder.decode(..) 함수는 문자 인코딩이 지원되지 않을 때 UnsupportedEncodingException을 던질 수 있습니다. 이 경우 UTF-8을 사용하지만 다른 인코딩 스키마를 대체로 사용할 수도 있습니다.

이러한 함수를 사용하면 이전에 작성한 Intent 처리 함수에 통합하여 URL을 완전히 처리할 수 있습니다:

```kotlin
fun handleAppLinkIntent(intent: Intent) {
  intent.data?.let { data ->
    data.host?.let { host ->
      if (host.contains("yourapp")) {
        Log.d(TAG, "Got an App Link intent with data=$data")
        val extra: String = data.getUrlParamExtra()
        // 'extra' 매개변수를 사용하여 작업 수행
      }
    }
  } ?: Log.v(TAG, "No App Link intent to consume")
}
```

<div class="content-ad"></div>

# 디지털 자산 링크 파일 설정

저희 앱 링크 어시스턴트의 세 번째 단계에서는 위저드가 디지털 자산 링크 파일을 생성하는 방법을 안내합니다. 이 파일은 저희 웹 사이트 안에 호스팅해야 하는 파일로, 인증을 위해 필요합니다. 좀 더 구체적으로 말하면, 안드로이드 및 플레이 스토어는 앱 링크를 사용하는 애플리케이션과 해당 사이트의 소유권을 인증하는 데 사용할 .json 파일을 웹 사이트에 추가하도록 요구합니다.

![이미지](/assets/img/2024-07-24-AnInquiryintoAndroidsAppLinks_3.png)

이 프로세스의 이 부분에는 몇 가지 유의할 점이 있으므로 최대한 자세히 설명하고 천천히 진행하겠습니다.

<div class="content-ad"></div>

## JSON 파일 호스팅하기

우선, assetlinks.json이라는 새 파일을 만들어 웹 사이트 내 다음 버킷에 호스팅해야 합니다:

```js
https://yourapp.com/.well-known/assetlinks.json
```

물론, 기본 URL을 실제 URL 호스트로 대체해야 합니다. 더 중요한 것은, 앱이 여러 도메인 - 예를 들어 yourapp.com 및 yourapp.net과 같은 -을 등록하는 경우, 설명서에 설명된 대로 두 웹 도메인에서 이 단계를 수행해야 합니다.

<div class="content-ad"></div>

## JSON 파일 생성하기

호스팅 버킷이 채워질 준비가 되면, 이제 Android Studio의 App Links Assistant 패널로 다시 돌아갈 수 있습니다. 그 전에, 다음 단계에서 무엇을 할 것인지 간단히 설명하고 싶습니다. 제 경우에는, debug, stage, release, 그리고 Play Store의 프로덕션 빌드를 위한 자산 링크 파일이 필요하다고 결정했습니다. 그리고 debug 빌드가 애플리케이션 패키지에 .debug 접미사를 추가하므로, 자산 링크 파일에 여러 항목을 생성해야 합니다. 이 모든 것이 보다 명확해지기를 기대하며 계속 진행하겠습니다!

Android Studio로 돌아와서, "Open Digital Asset Links File Generator" 버튼을 클릭하면 자산 링크 파일 생성을 돕는 또 다른 마법사 페이지가 나타납니다.

우리는 debug 빌드부터 시작합니다. 사이트 도메인과 해당 애플리케이션 ID를 상단의 두 입력 상자에 입력한 후, 이 페이지 내에서 다음과 같은 상태로 끝나야 합니다:

<div class="content-ad"></div>


<img src="/assets/img/2024-07-24-AnInquiryintoAndroidsAppLinks_4.png" />

Generate Digital Asset Links 파일 버튼을 클릭하면 웹사이트에 호스팅할 자산 링크 파일의 첫 번째 항목이 생성됩니다. 저희는 디버그 빌드에 서명을 하지 않기 때문에, 위의 옵션들은 이 단계에 대해 충분합니다.

다음으로, 이 단계를 두 번 더 반복할 것입니다 — 릴리스와 스테이지 빌드 유형 각각에 대해 — 하지만 이번에는 각 빌드 유형에 필요한 서명 자격 증명을 적절히 선택하기 위해 케이스토어 파일 선택 버튼을 클릭할 것입니다.

<img src="/assets/img/2024-07-24-AnInquiryintoAndroidsAppLinks_5.png" />


<div class="content-ad"></div>

한 가지 상황에서 나는 내 로컬 키스토어를 사용하여 내가 서명하는 두 가지 빌드 유형을 모두 커버하기 위해이 단계를 두 번 반복했습니다. 그러나 당신의 특정한 요구 사항은 달라질 수 있습니다. 어쨌든, 이 프로세스의 두 번째 단계는 우리가 자산 링크 파일에 들어갈 두 번째 항목을 제공해야 합니다.

마지막으로, 우리는 플레이 스토어의 프로덕션 빌드 유형을 커버해야 합니다. 저는 전통적인 릴리즈 빌드와 플레이 스토어의 빌드를 구분하는 이유는 우리 프로젝트에서 플레이 콘솔이 제품 빌드 스스로 서명하도록 허용하기 때문입니다. 이것은 우리의 키스토어가 릴리스되고 나면 플레이 콘솔에 의해 (어느 정도) 덮어씌워지기 때문에 서로 다른 빌드 유형과 다른 서명 스키마가 생성된다는 것을 의미합니다.

문서에 따르면, 우리 중에서 플레이 앱 서명을 사용하는 사람들은 플레이 콘솔의 릴리즈 '설정' 앱 서명 페이지를 참조하여 필요한 JSON 파일의 전체 버전을 가져와야 한다고 합니다.

![이미지](/assets/img/2024-07-24-AnInquiryintoAndroidsAppLinks_6.png)

<div class="content-ad"></div>

모든 작업이 완료되어 새로 생성된 assetlinks.json 파일은 다음과 같이 보일 것입니다.

```js
[
  {
    "relation": ["delegate_permission/common.handle_all_urls"],
    "target": {
      "namespace": "android_app",
      "package_name": "com.ivangarzab.yourapp",
      "sha256_cert_fingerprints": [
        "redacted_fingerprint_store_production",
        "redacted_fingerprint_release",
        "redacted_fingerprint_stage"
      ]
    }
  },
  {
    "relation": ["delegate_permission/common.handle_all_urls"],
    "target": {
      "namespace": "android_app",
      "package_name": "com.ivangarzab.yourapp.debug",
      "sha256_cert_fingerprints":
      ["redacted_fingerprint_debug"]
    }
  }
]
```

제가 설명한 제약 사항에 따라 보시다시피, 각 다른 package_name에 대해 등록해야 하는 별도의 항목이 필요합니다. 아래 항목은 debug 빌드를 다루며, 이는 이번 단계 초반에 생성한 단일 SHA256 지문을 사용합니다. 상단 항목은 앱의 일반적 패키지 이름을 다루며, 다른 모든 빌드 유형(production, release, stage)을 커버합니다. 이 항목은 각 경우를 각각 보완하는 3개의 지문을 가지고 있습니다.

이 변경 사항을 웹사이트에 공개하고, 이를 테스트하는 가장 좋거나 쉬운 방법에 대해 고민을 시작해보아야 합니다!

<div class="content-ad"></div>

# 앱 링크 테스트

긴 여정이었지만 거의 끝나갑니다. 우리 애플리케이션에 앱 링크를 통합하는 다음 단계는 그것을 테스트하는 것입니다!

![이미지](/assets/img/2024-07-24-AnInquiryintoAndroidsAppLinks_7.png)

우리의 앱 링크 어시스턴트 마법사 안의 네 번째이자 마지막 단계인 '테스트 앱 링크' 버튼을 클릭하면 매우 간단한 페이지가 나타납니다. 이 페이지는 URL을 입력으로 사용하고 '테스트 실행'이라고 적힌 단 하나의 버튼이 있는 페이지입니다.

<div class="content-ad"></div>

![example_image](/assets/img/2024-07-24-AnInquiryintoAndroidsAppLinks_8.png)

위의 설정을 고려할 때, (적어도) 두 가지 다른 테스트를 실행해야 합니다:

- 위의 그림과 완전히 동일한 모습의 테스트를 하나, 우리의 기본 URL 이상은 없는 기본 기능을 테스트하는 것.
- 그리고 우리가 추가한 추가 매개변수를 포함하는 보조 테스트를 하나 더 해야 합니다. 즉, https://yourapp.com/?extra=69.

테스트 실행 버튼을 클릭한 후, 우리가 이전에 설정한 코드가 실행되고 새로운 App Link URL 및 매개변수의 처리 및 사용에 문제가 없는지 확인해야 합니다.

<div class="content-ad"></div>

# 앱 링크 확인 지원

만약 위의 모든 작업이 올바르게 완료되었고 assetlinks.json 파일이 성공적으로 웹사이트에 발행되었을 경우, 확인에 어떤 문제나 어려움도 발생하지 않아야 합니다.

더불어, 앱 링크가 앱과 웹사이트 간에 성공적으로 할당되었다는 것을 확인하는 주요 두 가지 방법이 있습니다: 안드로이드 스튜디오 내에서 또는/그리고 플레이 콘솔에서 직접 확인하는 것입니다. 우리는 두 방법에 대해 모두 설명하겠습니다. 선호도 뿐만 아니라 완성을 위해서도요.

## 안드로이드 스튜디오를 통한 확인

<div class="content-ad"></div>

앱 링크 어시스턴트 패널을 통해 열린 초기 페이지를 기억하시나요? 이를 통해 앱 ↔️ 웹사이트 간에 링크를 올바르게 연결했는지 확인할 수 있습니다. 그러나 이 과정은 안드로이드 스튜디오에 직접 플레이 콘솔 계정을 연결해야 합니다.

안드로이드 스튜디오를 통해 확인을 시작하려면, 앱 링크 어시스턴트 패널을 열어보고 초기 내용을 확인해보세요. 아직 이 과정을 수행하지 않았다면, 안드로이드 스튜디오가 플레이 콘솔 계정에 연결할 수 있도록 허용해야 한다는 경고가 먼저 표시될 것입니다.

<img src="/assets/img/2024-07-24-AnInquiryintoAndroidsAppLinks_9.png" />

설정... 버튼을 클릭하면 이를 수행하는 방법을 안내받을 수 있습니다. 성공적으로 연결한 후에 앱 링크 어시스턴트 패널로 돌아가면 AndroidManifest.xml 파일에 추가한 링크 목록과 해당 링크가 성공적으로 확인되었는지 여부를 보여줍니다.

<div class="content-ad"></div>

<img src="/assets/img/2024-07-24-AnInquiryintoAndroidsAppLinks_10.png" />

## 플레이 콘솔을 통한 확인

반면에, 두 번 확실하게 확인한 것뿐만 아니라, 플레이 콘솔 계정을 Android Studio에 연결할 의도가 없는 경우, 플레이 콘솔을 직접 열어서 깊은 링크를 찾을 수도 있습니다. 간단히 말해서, 플레이 콘솔 내의 "성장" 섹션에서 깊은 링크를 찾아 클릭하십시오.

<img src="/assets/img/2024-07-24-AnInquiryintoAndroidsAppLinks_11.png" />

<div class="content-ad"></div>

페이지를 스크롤해서 Domains 섹션으로 이동하면, 확인하려는 각 URL의 확인 상태를 찾을 수 있어요.

다음 이미지에서 성공적인 확인과 실패한 확인이 어떻게 보일지 보여주는 샘플만 있는 걸 주목해주세요. 여러분의 도메인 중 하나 이상이 성공적으로 확인되지 않은 경우, 클릭해서 Play Console의 피드백을 읽어 문제의 원인을 파악하도록 하세요.

<img src="/assets/img/2024-07-24-AnInquiryintoAndroidsAppLinks_12.png" />

# 사후생각

<div class="content-ad"></div>

그게 다입니다! 이 기사에서는 안드로이드 애플리케이션 내에서 App Link를 올바르게 설정하고 처리하는 방법에 대한 단계별 안내를 살펴보았습니다. Android Studio나 Play Console에서 지원 및 확인을 위해 활용할 수 있는 다양한 도구와 패널이 있다는 것을 확인했으며, 공식 문서에서 완전히 명확하지 않은 몇 가지 세부 사항과 함정에 대해 설명하기 위해 최선을 다해 노력했습니다.

이 기사가 도움이 되었기를 바랍니다. 그러나 이 안내서를 보다 포괄적으로 만들거나 누락된 부분이 있는지 알려주시면 감사하겠습니다. 그 외에는 즐거운 코딩하세요 ✌️

👾 내 프로필 👾 | 🔗 모든 다른 링크들 🔗

<img src="/assets/img/2024-07-24-AnInquiryintoAndroidsAppLinks_13.png" />

<div class="content-ad"></div>

# 자원

- [앱 링크 교육](https://developer.android.com/training/app-links)
- [딥 링킹 앱 링크 교육](https://developer.android.com/training/app-links/deep-linking)
- [안드로이드 앱 링크 확인 교육](https://developer.android.com/training/app-links/verify-android-applinks)
- [Firebase 동적 링크 FAQ](https://firebase.google.com/support/dynamic-links-faq)
- [인텐트 및 필터 예시](https://developer.android.com/guide/components/intents-filters#ExampleSend)
- [URLDecoder Java 참조](https://developer.android.com/reference/java/net/URLDecoder)
- [Google Play 개발자 센터 지원](https://support.google.com/googleplay/android-developer/answer/9842756)