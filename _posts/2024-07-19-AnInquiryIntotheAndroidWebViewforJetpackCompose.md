---
title: "Jetpack Compose에서 Android WebView 사용법 탐구"
description: ""
coverImage: "/assets/img/2024-07-19-AnInquiryIntotheAndroidWebViewforJetpackCompose_0.png"
date: 2024-07-19 13:34
ogImage: 
  url: /assets/img/2024-07-19-AnInquiryIntotheAndroidWebViewforJetpackCompose_0.png
tag: Tech
originalTitle: "An Inquiry Into the Android WebView for Jetpack Compose"
link: "https://medium.com/gitconnected/an-inquiry-into-the-android-webview-for-jetpack-compose-35cb52e29622"
---


현재 나의 직장에서는 구 가 사용하던 구 도로 마이그레이션을 마무리하려고 하고 있어. 여전히 예전 View 시스템을 사용하던 몇 개의 화면을 Compose로 변환하여 마이그레이션 중이야. 그리고 마이그레이션이 특별히 고통스럽지는 않았지만 한 가지 주제가 오랫동안 우리를 헷갈리게 만들고 있어: WebView 화면을 마이그레이션하는 것.

Compose에서 WebView를 사용하는 것에는 어떤 독특한 어려움이 따르고 있다. 동료 중 한 명은 "Compose는 WebView를 제대로 처리할 수 없어!" 라고 말했었지. 그럼에도 불구하고 이 마이그레이션을 완료하는 분위기에서 나는 이 주제를 깊이 파헤쳐서 가능한 최상의 해결책을 찾기로 결심했어!

# 탐색

내 탐색은 아마도 우리 모두가 Compose로 WebView 를 마이그레이션하는 여정을 시작했던 곳에서 시작되었을 것이다: 구글의 Accompanist 프로젝트에서. Accompanist 프로젝트 소개 페이지에서 설명했듯이, Accompanist 프로젝트는 Jetpack Compose를 필요로 하지만 아직 공식 지원을 받지 못한 기능을 보충하기 위한 라이브러리 그룹이야. 나는 구글 팀의 '테스트용 땅'으로 보고 있어, 그곳에서 '비공식' 해결책을 게시하고 있는 곳이라고 생각해.

<div class="content-ad"></div>

안타깝게도 Accompanist의 WebView 라이브러리는 몇 달 전에 사용 중단되었습니다. 그럼에도 불구하고 해당 API에서 우리가 필요한 경우 솔루션을 복제하고 자체 프로젝트로 계속 개발하도록 하는 노트를 저장소에 추가했습니다. 그래서 몇 시간의 추가 연구와 시행착오 끝에, 나는 이 라이브러리의 내 버전을 포크하고 사용하고자하는 WebView 구현이 내가 원하는 모든 기능을 갖추었으며 그에 상응하는 수준에 있음을 보장하기로 결정했습니다.

결심을 굳게 다지고, "composable-webview"라는 개인 저장소를 만들었습니다. 현재 여기에는 웹 뷰 문제에 대한 내 개인 솔루션이 호스팅되고 있습니다. 위에서 제안된 대로, 코드의 대부분은 Accompanist 프로젝트 자체에서 직접 가져왔지만, 제가 직접 추가한 추가 요소들도 있습니다.

다음 몇 단락에서는 저가 공개로 게시한 이 라이브러리, 그 사용 방법 및 Accompanist 이전 버전과 구별되는 몇 가지 고급 기능 등을 탐색해 볼 것입니다. Accompanist 설명서에서 직접 가져오는 부분이 많을 것이며, 기여한 곳에는 유익한 점수가 주어지기를 희망합니다!

# 시작하기

<div class="content-ad"></div>

안녕하세요! 안드로이드 프로젝트에 새로운 종속성을 추가할 때마다, 첫 번째 단계는 라이브러리를 프로젝트에 통합하는 것입니다. 해당 라이브러리는 현재 Jitpack 메이븐 저장소에 호스팅되어 있으므로 의존성을 해당 위치에서 가져오려고 시도하는지 확인하는 것이 첫 번째 단계입니다. 이를 위해 루트 build.gradle 또는 build.gradle.kts 파일을 열고 다음 줄을 repositories 블록의 맨 끝에 추가하세요:

```js
// build.gradle.kts
repositories {
  mavenCentral()
  maven { url ("https://jitpack.io") } // 이 줄 추가
}
```

이제 필요한 부분을 해결했으므로 실제 라이브러리를 프로젝트에 통합할 수 있습니다. 현대적인 안드로이드 앱은 최신 버전 카탈로그를 사용하여 의존성을 보유하고 관리하지만, 가능한 한 포괄적이 되도록 전통적인 방법으로 의존성을 가져오는 방법도 다룰 것입니다!

## 전통적인 방법을 사용한 의존성 관리

<div class="content-ad"></div>

만약 앱을 버전 카탈로그를 활용하도록 마이그레이션하지 않았다면, 해당 라이브러리를 통합하려는 모듈의 Gradle 빌드 파일을 열고 아래 줄을 종속성 블록에 추가하세요:

```js
// build.gradle.kts
dependencies {
  ...
  implementation("com.ivangarzab:composable-webview:0.6") // 이 줄을 추가하세요
}
```

## 버전 카탈로그 방법을 사용한 종속성 관리

그대신, Gradle 종속성을 관리하기 위해 버전 카탈로그 기능을 사용 중이라면 몇 가지 다른 단계가 필요합니다. 먼저 libs.versions.toml 파일을 열고 라이브러리를 Gradle 프로젝트에 통합하세요:

<div class="content-ad"></div>

```kotlin
[versions]
...
igbWebView = "0.6"

[libraries]
...
igb-webview = { group = "com.ivangarzab", name = "composable-webview", version.ref = "igbWebView" }
```

의존성을 Gradle 수준에서 선언했으므로 이제 이 WebView를 필요로 하는 모듈에 해당 의존성을 통합할 수 있습니다:

```kotlin
// build.gradle.kts
dependencies {
  ...
  implementation(libs.igb.webview) // 이 줄 추가
}
```

# 사용법

<div class="content-ad"></div>

이제 라이브러리가 프로젝트에 통합되었으므로 WebView 구현을 사용하여 필요에 맞게 작업을 시작할 수 있습니다! Accompanist 문서에서 설명한 대로 이 API의 가장 간단한 사용법은 rememberWebViewState(url) 함수 필드가 필요하며 Composable WebView을 호출하는 것뿐입니다:

```kotlin
val state = rememberWebViewState("https://your-url.com")
WebView(state = state)
```

## JavaScript 활성화

위의 코드는 URL을 로드하고 선택한 Composable 레이아웃에 표시하는 간단한 WebView을 생성합니다. 그러나 대부분의 경우 이것만으로 충분하지 않을 것입니다. 대부분의 웹 사이트는 특정 정보를 표시하거나 실행하기 위해 JavaScript를 사용합니다. 그러나 기본 WebView 구현에는 JavaScript가 활성화되어 있지 않습니다. WebView 구현에서 JavaScript를 활성화하려면 그런 기능을 달성하기 위해 추가 설정이 필요합니다.

<div class="content-ad"></div>

```kotlin
val state = rememberWebViewState("https://your-url.com")
WebView(
  state = state,
  onCreated = { webView -> webView.settings.javaScriptEnabled = true }
)
```

이 업데이트된 구현은 웹 사이트가 JavaScript에 의존하는 모든 콘텐츠를로드 할 수 있도록합니다. 그러나 Android Studio에서 빠르게 지적하듯이 WebView 구현에서 JavaScript를 활성화하면 응용 프로그램에 취약점이 발생할 수 있습니다.이 기능이 정말 필요한지 여부를 결정하기 전에 여기와 여기의 문서를 검토해주십시오.

## 백프레스 캡처

Accompanist 문서를 따르면 맞춤 WebView의 미진행 구현에서 백프레스 또는 제스처를 캡처하는 것은 captureBackPresses 매개변수를 활용하면 됩니다. WebView 구현은 기본적으로 백프레스 캡처 플래그가 활성화되어 있지만, captureBackPresses 매개변수를 활용하여 동작을 변경할 수 있습니다:


<div class="content-ad"></div>

```kotlin
val state = rememberWebViewState("https://your-url.com")
WebView(
  state = state,
  onCreated = { webView -> webView.settings.javaScriptEnabled = true },
  captureBackPresses = false
)
```

## 커스텀 WebView 서브클래싱

이 섹션에서 볼 마지막 기능은 WebView을 서브클래싱하는 내용입니다. 즉, 필요에 맞는 사용자 정의 구현이 필요할 경우 팩토리 함수 매개변수를 통해 가능합니다:

```kotlin
// 컴포저 내부
val state = rememberWebViewState("https://your-url.com")
WebView(
  state = state,
  onCreated = { webView -> webView.settings.javaScriptEnabled = true },
  captureBackPresses = false,
  factory = { context -> CustomWebView(context) }
)

// 컴포즈 외부
class CustomWebView(context: Context) : WebView(context) {
  ...
}
```

<div class="content-ad"></div>

이 기능을 사용하려면 우리가 전통적인 View 시스템에서 자체 android.webkit.Webview를 만들고 팩토리 함수 내에서 인스턴스화해야 합니다. 앞서 언급한 대로요.

# 고급 사용법

다음 섹션에서는 composable-webview 라이브러리에서 사용할 수 있는 몇 가지 고급 기능을 탐색할 것입니다. 먼저 aWebViewClient나 a WebChromeClient 그리고 WebViewNavigator를 통합하는 방법에 대해 설명하겠습니다. 그런 다음 로딩 스피너 및 우리의 기본 WebView에 대한 풀-투-새로 고침 패턴의 구현에 중점을 둘 것입니다.

## 웹 클라이언트

<div class="content-ad"></div>

View 시스템의 WebView 컴포넌트와 유사하게, Composable WebView 구현은 aWebViewClient 또는 a WebChromeClient의 사용을 허용합니다. 이 두 객체 모두 우리가로드하려는 URL에서 일부 추가 데이터를 가로채거나 받을 수 있습니다. 예를 들어, WebViewClient는 오류 또는 로딩 상태와 관련된 콜백을 받고, WebChromeClient는 웹 사이트의 아이콘이나 제목을 가져오는 데 사용할 수 있습니다.

우리의 기본 WebView Composable에 이 중 하나를 구현하려면 해당 매개 변수 필드에 AccompanistWebViewClient 또는 AccompanistWebChromeClient를 제공하기만 하면 됩니다:

```js
val state = rememberWebViewState("https://your-url.com")
val webViewClient = AccompanistWebViewClient()
val webChromeClient = AccompanistWebChromeClient()

WebView(
  state = state,
  onCreated = { webView -> webView.settings.javaScriptEnabled = true },
  client = webViewClient,
  chromeClient = webChromeClient
)
```

어느 경우에도 사용자 정의 클라이언트를 정의하지 않으면 내부적으로 기본 클라이언트가 생성되어 이러한 객체 중 하나에서 받은 정보를 추적합니다.

<div class="content-ad"></div>

## 웹 네비게이터

비슷한 방식으로, WebViewNavigator가 구성 요소 외부에서 탐색 작업을 수행하려는 경우 WebView 구현에 제공될 수 있습니다. 예를 들어, 네비게이터를 사용하여 뒤로 제스처 액션에 응답하거나 원하는대로 URL을 다시로드하는 작업을 수행할 수 있습니다.

네비게이터를 제공하는 방법은 위와 같이 작동합니다. 간단히 말해서, 모든 작업을 수행하기 위해 CoroutineScope를 수신하는 지역 인스턴스를 만들고 구성 선언 단계에서 설정하는 것입니다:

```js
val state = rememberWebViewState("https://your-url.com")
val coroutineScope = rememberCoroutineScope()
val navigator = WebViewNavigator(coroutineScope)

WebView(
  state = state,
  navigator = navigator
)
```

<div class="content-ad"></div>

## 로딩 상태 받아내기

지금까지 우리의 WebView 구현은 조금 부풀어있어 보일 수 있습니다. 그 중 일부는 아마도 생각 중이실 것입니다: "정말 그 모든 것이 필요한 걸까요?" 간단히 대답하자면 물론 NO죠. 하지만 이렇게 나중에 추가된 기능들은 더 깔끔한 동작을 위해 결합될 수 있습니다.

우리가 본 것처럼, 두 웹 클라이언트 구현 방식은 WebView의 로딩 상태를 파생하는 데 활용될 수 있습니다. WebView의 상태를 이용하여 state.loadingState를 얻고 필요에 따라 로딩 인디케이터를 만들 수 있습니다:

```js
...
val loadingState = state.loadingState
if (loadingState is LoadingState.Loading) {
  LinearProgressIndicator(
    modifier = Modifier
     .fillMaxWidth(),
    progress = { loadingState.progress },
    color = MaterialTheme.colorScheme.onSurface,
    trackColor = MaterialTheme.colorScheme.surface
  )
}
```

<div class="content-ad"></div>

클라이언트가 로딩 여부를 알 수 있는 필요한 정보를 제공하는 곳입니다. 그러나 모든 것은 이미 내부적으로 발생합니다! AccompanistWebViewClient 또는 AccompanistWebChromeClient의 사용자 정의 구현을 통해 이러한 내부 상태 변경에 대해 추가적으로 반응하고 필요한 추가 작업을 수행할 수 있습니다.

## Pull-to-Refresh

비슷한 맥락으로, WebViewNavigator를 활용하여 pull-to-refresh 패턴을 구현할 수 있으며, 이를 사용하여 명령에 따라 페이지 새로고침을 트리거하기 위해 navigator.reload() 함수를 호출할 수 있습니다:

```js
val navigator = WebViewNavigator()
// 새로고침 시
navigator.realod()
```

<div class="content-ad"></div>

이제 Pull-to-refresh 패턴을 구현하는 것은 약간 더 많은 설정이 필요하며, 자세한 내용은 다른 문서에서 설명할 것입니다. 현재 작성 중인 이 글에서는, 다음 섹션에서 우리가 이야기한 모든 내용을 완전히 구현한 것을 보여줄 것입니다. 이것은 내가 준비한 보조 Composable에 패키징되어 있는 완전한 구현입니다!

# 모두 함께 넣기

지금까지 살펴본 것처럼, Accompanist의 WebView Composable은 이미 다양한 사용자 정의 기능을 제공하고 있습니다. 그러나 초기 유연성에서 파생될 수 있는 멋진 '추가 기능'들을 고려할 때, WebViewLayout이라는 보조 Composable을 만들었습니다. 이 Composable은 이미 이러한 기능들이 많이 포장되어 있습니다!

```kotlin
@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun WebViewLayout(
    state: WebViewState,
    modifier: Modifier = Modifier,
    captureBackPresses: Boolean = true,
    loadingIndicatorEnabled: Boolean = true,
    pullToRefreshState: PullToRefreshState = rememberPullToRefreshState(),
    navigator: WebViewNavigator = rememberWebViewNavigator(),
    onCreated: (WebView) -> Unit = {},
    onDispose: (WebView) -> Unit = {},
    client: AccompanistWebViewClient = remember { AccompanistWebViewClient() },
    chromeClient: AccompanistWebChromeClient = remember { AccompanistWebChromeClient() },
    factory: ((Context) -> WebView)? = null,
) { ... }
```

<div class="content-ad"></div>

이 Composable 레이아웃은 모든 장점을 갖추고 있어요: Accompanist의 WebView 구현을 활용하며 동일한 사용자 정의를 가능하게 합니다. 또한 기본 pull-to-refresh 패턴과 Composable 상단에 간단한 LinearProgressIndicator를 활성화하는 옵션이 함께 제공되어 대상 URL의 로딩 상태를 더 잘 시각화할 수 있습니다.

![이미지](https://miro.medium.com/v2/resize:fit:960/1*nOihp1geDnoIa3u3PzWx1A.gif)

이 Composable을 사용하는 것은 매우 간단하며 일반 WebView Composable보다 더 많은 설정을 필요로하지 않아요. WebViewLayout을 사용하면 loading indicator 및 pull-to-refresh 기능 추가 요구 사항을 간소화하려는 아이디어입니다. 그러나 여러분의 요구 사항이 제 것과 크게 다를 수 있다는 것을 알아야 해요.

만약 그렇다면, WebViewLayout이 원래 WebView의 사용자 정의 구현 시작점으로서 도움이 되기를 바랍니다. 또는 composable-webview 프로젝트로 Feature Request 또는 Pull Request를 제공하여 더 나은 솔루션을 함께 찾아갈 수 있도록 해 주세요!

<div class="content-ad"></div>

# 마음이 바뀌면서

이 글에서는 구글 Accompanist 라이브러리에서 사용되던 WebView의 사용에 대해 다루었는데, 이를 내가 만든 composable-webview 라이브러리로 포크 및 사용자 정의한 것을 살펴보았습니다. 그 유연한 API로 할 수 있는 멋진 트릭 몇 가지를 소개했고, 내 프로젝트 안에 있는 두 번째 Composable인 WebViewLayout의 존재를 강조했습니다. 이 레이아웃은 모든 것을 통합하여 사용자에게 위에서 언급한 기능을 단일 패키지로 제공하는 것을 목표로 합니다.

이 글이 충분히 유익했기를 바랍니다. 그러나 궁금한 점이나 우려 사항이 있으면 댓글을 남기거나 GitHub 저장소에 버그 보고서 또는 기능 요청을 직접 제출해 주시기 바랍니다. 그리고 다시 한 번 강조하지만, 저희가 기여할 수 있기를 바랍니다! 이를 더 나아지게 할 수 있다고 생각한다면 프로젝트를 포크하고(링크 아래) 고려 및 토론을 위해 풀 리퀘스트로 제출할 아이디어를 자유롭게 제안해 주세요!

👾 내 프로필 👾 | 🔗 모든 다른 링크 🔗 | 라이브러리의 저장소 👇

<div class="content-ad"></div>

![An Inquiry Into the Android WebView for Jetpack Compose](/assets/img/2024-07-19-AnInquiryIntotheAndroidWebViewforJetpackCompose_0.png)

# 자원

- [composable-webview](https://github.com/ivangarzab/composable-webview)
- [accompanist](https://google.github.io/accompanist/)
- [accompanist/web](https://google.github.io/accompanist/web/)
- [accompanist/tree/main/web](https://github.com/google/accompanist/tree/main/web)
- [안전하지 않은 URI로드에 대한 Android 개발자 문서](https://developer.android.com/privacy-and-security/risks/unsafe-uri-loading)
- [보안 팁](https://developer.android.com/privacy-and-security/security-tips)