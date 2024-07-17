---
title: "React Native에서 CodePush로 무선 업데이트 구현하는 방법"
description: ""
coverImage: "/assets/img/2024-07-09-ImplementingCodePushforOver-the-AirUpdatesinReactNative_0.png"
date: 2024-07-09 17:00
ogImage:
  url: /assets/img/2024-07-09-ImplementingCodePushforOver-the-AirUpdatesinReactNative_0.png
tag: Tech
originalTitle: "Implementing CodePush for Over-the-Air Updates in React Native"
link: "https://medium.com/@ritika100898/implementing-codepush-for-over-the-air-updates-in-react-native-535e750ec308"
---

![image](https://miro.medium.com/v2/resize:fit:1400/1*uGMaGBkkErfcFp2SeiiBzQ.gif)

코드 푸시란 소프트웨어 애플리케이션이나 시스템에 새로운 코드를 업데이트하거나 배포하는 과정을 말합니다. 개발 환경에서 최신 버전의 코드를 제품 환경으로 전송하여 변경 사항을 사용자에게 라이브로 제공합니다.

CodePush에 대해 더 알고 싶다면 다음 링크를 참조해주세요.

# 단계 1: AppCenter에서 앱 등록하기

<div class="content-ad"></div>

- AppCenter에 로그인해주세요.
- Android 및 iOS용 새로운 앱을 추가해주세요.

![image](/assets/img/2024-07-09-ImplementingCodePushforOver-the-AirUpdatesinReactNative_0.png)

- 필수 필드를 작성하고 '새 앱 추가' 버튼을 클릭해주세요.

# 예시:

<div class="content-ad"></div>

## 안드로이드

![안드로이드 이미지](/assets/img/2024-07-09-ImplementingCodePushforOver-the-AirUpdatesinReactNative_1.png)

## iOS

![iOS 이미지](/assets/img/2024-07-09-ImplementingCodePushforOver-the-AirUpdatesinReactNative_2.png)

<div class="content-ad"></div>

- Distribute로 이동하여 CodePush를 선택한 다음 표준 배포 만들기를 클릭하세요.
- 코드 푸시를 수행할 때마다 사용될 릴리스 업데이트 명령어를 메모해 두세요.
- 원하는 환경을 편집, 삭제 또는 새로 만들 수 있습니다. Distribute -` CodePush로 이동한 다음 설정 아이콘을 클릭하고 세 개의 점을 클릭하세요.

## Android

```js
$ appcenter codepush release-react -a xxxxx/Awesome-Project -d Production

/* "Production"은 환경 이름입니다
위 단계를 따라 이름을 선택사항으로 변경할 수 있습니다*/
```

## iOS

<div class="content-ad"></div>

```js
$ appcenter codepush release-react -a xxxxx/Awesome-Project-IOS -d Production

/* "Production"은 환경 이름입니다.
이름을 원하는 대로 변경할 수도 있습니다. 위 단계를 따라하세요. */
```

# Step 2: React Native에 CodePush SDK 설치하기

- 아래 명령어를 사용하여 새 React Native 프로젝트를 만드세요.

```js
$ npx react-native init AwesomeProject
```

<div class="content-ad"></div>

- npm install을 사용하여 CodePush를 전역으로 설치하세요.

```bash
$ npm install -g appcenter-cli
```

- CodePush 라이브러리를 React Native 프로젝트에 통합하세요.

```bash
$ npm install react-native-code-push

                 또는

$ yarn add react-native-code-push
```

<div class="content-ad"></div>

- CLI를 통해 AppCenter에 로그인하고, 터미널에서 아래 명령을 실행하세요.

```js
$ appcenter login
```

- 새로운 웹페이지가 브라우저에 열립니다. 토큰을 복사하여 터미널에 붙여넣으세요.
- 브라우저에서 발급받은 토큰을 입력하세요: [여기에 붙여넣기]

![이미지](/assets/img/2024-07-09-ImplementingCodePushforOver-the-AirUpdatesinReactNative_3.png)

<div class="content-ad"></div>

# 단계 3: 플랫폼별 구성

## 안드로이드 설정

1.  android/settings.gradle 파일 수정

```js
…
include ':app', ':react-native-code-push'
project(':react-native-code-push').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-code-push/android/app')
```

<div class="content-ad"></div>

![Image](/assets/img/2024-07-09-ImplementingCodePushforOver-the-AirUpdatesinReactNative_4.png)

2. Add code to android/app/build.gradle

```js
apply from: "../../node_modules/react-native-code-push/android/codepush.gradle"
```

![Image](/assets/img/2024-07-09-ImplementingCodePushforOver-the-AirUpdatesinReactNative_5.png)

<div class="content-ad"></div>

3. MainApplication.Java을 업데이트 해주세요.

```js
…
// 1. 플러그인 클래스를 가져와주세요.
import com.microsoft.codepush.react.CodePush;
public class MainApplication extends Application implements ReactApplication {
private final ReactNativeHost mReactNativeHost = new ReactNativeHost(this) {
…
// 2. CodePush 실행 시 JS 번들 위치를 결정할 수 있도록
// getJSBundleFile 메서드를 재정의해주세요.
@Override
protected String getJSBundleFile() {
return CodePush.getJSBundleFile();
}
};
}
```

![ImplementingCodePushforOver-the-AirUpdatesinReactNative_6.png](/assets/img/2024-07-09-ImplementingCodePushforOver-the-AirUpdatesinReactNative_6.png)

4. 이제 AppCenter로 이동하여 특정 프로젝트를 선택하고
   Distribute-CodePush-설정 아이콘을 클릭하고 Production 키를 복사해주세요.

<div class="content-ad"></div>

![Instruction image](/assets/img/2024-07-09-ImplementingCodePushforOver-the-AirUpdatesinReactNative_7.png)

5. 안드로이드 앱의 buildTypes 하위에 이 프로덕션 키를 붙여넣으세요.

```js
resValue “string”, “CodePushDeploymentKey”, ‘“”’  // 디버그용
```

```js
resValue “string”, “CodePushDeploymentKey”, ‘PUT_YOUR_DEVELOPMENT_KEY_HERE’ // 릴리즈용

// 'PUT_YOUR_DEVELOPMENT_KEY_HERE'를 여러분의 CodePush 키로 교체하세요
```

<div class="content-ad"></div>

안녕하세요! 아래는 Markdown 형식으로 테이블 태그를 변경한 코드입니다.

<img src="/assets/img/2024-07-09-ImplementingCodePushforOver-the-AirUpdatesinReactNative_8.png" />

## iOS Setup

- First Run pod install to update CocoaPods dependencies.
- Edit AppDelegate.mto allow CodePush bundle selection.

```js
#import <CodePush/CodePush.h>
```

<div class="content-ad"></div>

다음 코드 라인을 AppDelegate.m에서 찾으세요. 해당 코드는 프로덕션 릴리스용 브릿지의 소스 URL을 설정하고 아래와 같이 해당 URL을 대체합니다.

```js
return [[NSBundle mainBundle] URLForResource:@"main" withExtension:@"jsbundle"];
```

아래와 같이 해당 코드를 대체하세요.

<div class="content-ad"></div>

```js
return [CodePush bundleURL];
```

![CodePush Configuration](/assets/img/2024-07-09-ImplementingCodePushforOver-the-AirUpdatesinReactNative_10.png)

4. Info.plist 파일에 배포 키를 추가하세요. AppCenter에 들어가서 프로젝트를 선택하고 Distribute - CodePush - 설정 아이콘을 클릭한 후 프로덕션 키를 복사하여 Info.plist 파일에 붙여넣으세요.

```js
…
<key>CodePushDeploymentKey</key>
<string>PUT_YOUR_DEVELOPMENT_KEY_HERE</string>
…

// PUT_YOUR_DEVELOPMENT_KEY_HERE에는 CodePush 키를 입력하세요
```

<div class="content-ad"></div>

![CodePush](/assets/img/2024-07-09-ImplementingCodePushforOver-the-AirUpdatesinReactNative_11.png)

# Step 4: Wrap Your App.js with CodePush

- 앱의 주요 컴포넌트를 CodePush로 래핑합니다.

```js
import React from "react";
import { StyleSheet, Text, View, Image } from "react-native";
import CodePush from "react-native-code-push";

const App = () => {
  return (
    <View style={styles.container}>
      <Text style={styles.heading}>CodePush: Seamless Over-the-Air로 React Native 개발자를 강화합니다.</Text>
      <Image style={styles.image} resizeMode={"contain"} source={{ uri: "https://loremflickr.com/g/520/540/paris" }} />
      <View style={styles.button}>
        <Text style={styles.buttonText}>BEFORE CODE PUSH</Text>
      </View>
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: "center",
    alignItems: "center",
    marginHorizontal: 16,
  },
  heading: {
    fontSize: 24,
    marginBottom: 20,
    fontWeight: "700",
    textAlign: "center",
  },
  image: {
    width: 200,
    height: 200,
    borderRadius: 10,
  },
  button: {
    padding: 15,
    marginTop: 20,
    borderRadius: 8,
    backgroundColor: "#0E4B91",
  },
  buttonText: {
    color: "#FFFFFF",
    fontWeight: "700",
  },
});

export default CodePush(App);
```

<div class="content-ad"></div>

프로젝트를 실행하는 방법입니다.

```js
$ react-native run-android // 안드로이드용

$ react-native run-iOS // iOS용
```

![ImplementingCodePushforOver-the-AirUpdatesinReactNative_12.png](/assets/img/2024-07-09-ImplementingCodePushforOver-the-AirUpdatesinReactNative_12.png)

## 참고: CodePush 업데이트 이전에 Android 및 iOS 기기에 릴리스 빌드를 생성하고 설치해야 합니다.

<div class="content-ad"></div>

# 단계 5: CodePush 실행

- 필요한 JS 변경 사항을 하고 저장하세요.

```js
import React from "react";
import { StyleSheet, Text, View, Image } from "react-native";
import CodePush from "react-native-code-push";

const App = () => {
  return (
    <View style={styles.container}>
      <Text style={styles.heading}>
        CodePush: Empowering React Native Developers with Seamless Over-the-Air Updates
      </Text>
      <Image style={styles.image} resizeMode={"contain"} source={{ uri: "https://loremflickr.com/520/540" }} />
      <View style={styles.button}>
        <Text style={styles.buttonText}>코드 푸시 후</Text>
      </View>
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: "center",
    alignItems: "center",
    marginHorizontal: 16,
  },
  heading: {
    fontSize: 24,
    marginBottom: 20,
    fontWeight: "700",
    textAlign: "center",
  },
  image: {
    width: 200,
    height: 200,
    borderRadius: 10,
  },
  button: {
    padding: 15,
    marginTop: 20,
    borderRadius: 8,
    backgroundColor: "#B7012F",
  },
  buttonText: {
    color: "#FFFFFF",
    fontWeight: "700",
  },
});

export default CodePush(App);
```

2. Android 또는 iOS용 CodePush 명령을 실행하세요.

<div class="content-ad"></div>

- Android

```js
appcenter codepush release-react -a xxxxx/AwesomeProject -d Production
```

- iOS

```js
appcenter codepush release-react -a xxxxx/AwesomeProject-IOS -d Production
```

<div class="content-ad"></div>

3. 위 명령을 실행하면 터미널에서 필요한 모든 정보를 확인할 수 있습니다. 터미널에서 지정된 버전이 프로젝트의 build.gradle 및 Info.plist 파일에 지정된 버전과 같은지 확인해주세요.

![image](/assets/img/2024-07-09-ImplementingCodePushforOver-the-AirUpdatesinReactNative_13.png)

4. AppCenter에서 버전 정보를 확인하세요.

AppCenter-`프로젝트명-`Distribute-`CodePush

<div class="content-ad"></div>

![이미지](/assets/img/2024-07-09-ImplementingCodePushforOver-the-AirUpdatesinReactNative_14.png)

5. 위의 버전을 클릭한 후 화면 오른쪽 상단의 설정 아이콘을 클릭합니다.

![이미지](/assets/img/2024-07-09-ImplementingCodePushforOver-the-AirUpdatesinReactNative_15.png)

6. 최신 버전을 사용자들이 보유하도록 "필수 업데이트" 버튼을 활성화합니다.

<div class="content-ad"></div>

아래와 같은 순서로 iOS에 대해 AppCenter에서 동일한 단계를 따르세요.

새로 고침을 한 후, 설치된 앱에 변경 사항이 반영됩니다.

<div class="content-ad"></div>

# 단계 6: 문제 해결 및 롤백:

- 활성화 버튼을 끄면 업데이트가 비활성화되고 이전 변경 사항으로 롤백됩니다.

![이미지](/assets/img/2024-07-09-ImplementingCodePushforOver-the-AirUpdatesinReactNative_18.png)

# 중요 사항:

<div class="content-ad"></div>

- 일치하는 버전:

- 초기 릴리스와 동일한 앱 버전에서 항상 CodePush 업데이트를 수행해야 합니다.
- 버전 간 일관성은 원활한 업데이트를 보장합니다.

2. 앱 버전 업그레이드:

- 앱을 업데이트할 때 Android와 iOS 구성 파일 모두에서 버전을 변경하세요.
- CodePush 명령 실행 전에 새 버전으로 릴리스 빌드를 생성하세요.
- 이전 버전은 변경되지 않습니다.

<div class="content-ad"></div>

## 모니터링 및 분석:

- 앱센터를 통해 업데이트 채택률에 대한 분석을 제공합니다.
- 업데이트의 성공 여부를 모니터링하고 사용자 피드백을 수집합니다.

## 최적의 방법:

- 프로덕션 환경으로 전개하기 전에 업데이트를 철저히 테스트합니다.
- 업데이트 시 혼동을 피하기 위해 명확한 버전 관리를 유지합니다.

<div class="content-ad"></div>

## 제한 사항:

- CodePush는 앱의 JavaScript 번들만 업데이트할 수 있습니다. 네이티브 코드를 업데이트할 수는 없습니다. 이는 CodePush만으로는 네이티브 모듈 수정이 필요한 변경을 할 수 없다는 것을 의미합니다.

# 결론:

즐거운 코딩하세요!!

<div class="content-ad"></div>

![CodePush implementation](/assets/img/2024-07-09-ImplementingCodePushforOver-the-AirUpdatesinReactNative_19.png)
