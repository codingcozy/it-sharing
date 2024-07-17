---
title: "React Native에서 NSFWjs 이해하기 2024 최신 가이드"
description: ""
coverImage: "/assets/img/2024-07-12-UnderstandingNSFWjsforReactNative_0.png"
date: 2024-07-12 18:51
ogImage:
  url: /assets/img/2024-07-12-UnderstandingNSFWjsforReactNative_0.png
tag: Tech
originalTitle: "Understanding NSFW.js for React Native"
link: "https://medium.com/stackademic/understanding-nsfw-js-for-react-native-f6af41592c86"
---

<img src="/assets/img/2024-07-12-UnderstandingNSFWjsforReactNative_0.png" />

한편 사용자가 생성할 수 있는 콘텐츠는 카테고리별로 증가하고 애플리케이션이 이 콘텐츠가 안전하고 게시할 수 있는 적합성을 확인해야 합니다. NSFW(Not Safe For Work)는 성인 콘텐츠를 포함하거나 시청자를 역격하여 움직이는 무언가를 설명하는 온라인 용어입니다. 당신의 앱에게 키가 되는 것은 사용자와 애플리케이션의 무결성을 보호하고 그러한 콘텐츠를 탐지하고 필터링하는 것입니다. 이것이 NSFW.js가 등장하는 이유입니다.

NSFW.js란?

NSFW.js는 TensorFlow 위에 구축된 JavaScript 라이브러리로 사용자의 브라우저에서 음란 이미지를 스캔할 수 있습니다. 이 구현은 사전에 훈련된 합성곱 신경망(CNN) 모델을 사용하여 성인물, 헨타이, 중립, 섹시 및 드로잉의 5가지 분류 가능성을 출력합니다. 이를 통해 플랫폼에서 콘텐츠 모니터링에 강력한 무기가 됩니다.

<div class="content-ad"></div>

```js
npm install -g react-native-cli
```

새로운 React Native 프로젝트를 만들어보세요:

```js
react-native init NSFWDemo
cd NSFWDemo
```

## 단계 2: 종속성 설치하기

<div class="content-ad"></div>

TensorFlow.js와 NSFW.js 모델을 설치해야 합니다:

```js
npm install @tensorflow/tfjs @tensorflow-models/nsfwjs
```

## 단계 3: TensorFlow 및 NSFW.js 통합 추가

이제 즐겨 사용하는 코드 에디터에서 프로젝트를 열고 App.js 파일을 수정하여 TensorFlow 및 NSFW.js를 포함하십시오. 간단한 예시를 보여드리겠습니다:

<div class="content-ad"></div>

```js
import React, { useEffect, useState } from "react";
import { View, Text, Button, Image, ActivityIndicator } from "react-native";
import * as tf from "@tensorflow/tfjs";
import * as nsfwjs from "nsfwjs";

const App = () => {
  const [loading, setLoading] = useState(true);
  const [model, setModel] = useState(null);
  const [imageUri, setImageUri] = useState(""); // 여기에 이미지 URI를 넣어주세요
  const [prediction, setPrediction] = useState(null);

  useEffect(() => {
    const loadModel = async () => {
      await tf.ready();
      const loadedModel = await nsfwjs.load();
      setModel(loadedModel);
      setLoading(false);
    };

    loadModel();
  }, []);

  const handleImageUpload = async (uri) => {
    setLoading(true);
    const response = await fetch(uri);
    const blob = await response.blob();
    const image = new Image();
    image.src = URL.createObjectURL(blob);
    image.onload = async () => {
      const predictions = await model.classify(image);
      setPrediction(predictions);
      setLoading(false);
    };
  };

  return (
    <View style={{ flex: 1, justifyContent: "center", alignItems: "center" }}>
      <Text>NSFW.js with React Native</Text>
      {loading && <ActivityIndicator size="large" />}
      {prediction && (
        <View>
          <Text>Predictions:</Text>
          {prediction.map((p, index) => (
            <Text key={index}>
              {p.className}: {p.probability.toFixed(2)}
            </Text>
          ))}
        </View>
      )}
      <Button title="Upload Image" onPress={() => handleImageUpload(imageUri)} />
      {imageUri && <Image source={{ uri: imageUri }} style={{ width: 200, height: 200 }} />}
    </View>
  );
};

export default App;
```

# 설명

- 종속성: 우리는 리액트, TensorFlow.js, NSFW.js와 같은 필수 패키지를 가져옵니다.
- 상태 관리: useState와 useEffect 훅을 사용하여 상태와 효과를 관리합니다.
- 모델 로딩: loadModel 함수는 TensorFlow를 초기화하고 NSFW.js 모델을 로드합니다. 모델을 상태로 설정하고 로딩 상태를 토글합니다.
- 이미지 업로드 처리: handleImageUpload 함수는 지정된 URI에서 이미지를 가져와 blob로 변환하고 로드된 모델을 사용하여 분류합니다. 예측값은 상태로 설정됩니다.
- 렌더링: 로딩 인디케이터, 이미지 업로드 버튼 및 예측값을 표시하는 인터페이스를 렌더링합니다.

# 고려사항

<div class="content-ad"></div>

- 성능: 이미지 분류는 자원을 많이 소비할 수 있습니다. 효율적인 처리를 위해 서버로 계산을 오프로드하는 것을 고려해보세요.
- 사용자 경험: 특히 오류가 발생하거나 처리 시간이 긴 경우에는 사용자에게 명확한 피드백을 제공해주세요.
- 콘텐츠 모더레이션 정책: 자동화된 도구와 함께 강력한 콘텐츠 모더레이션 정책을 시행하여 전반적인 커버리지를 보장해주세요.

# 결론

NSFW.js를 React Native와 통합하면 모바일 애플리케이션에서 부적절한 콘텐츠를 감지하는 강력한 메커니즘을 제공할 수 있습니다. TensorFlow.js와 사전 훈련된 모델의 기능을 활용하여 앱의 콘텐츠 모더레이션 기능을 크게 향상시킬 수 있습니다. 언제나 사용자 데이터를 책임지고 사용자에게 원활한 경험을 제공해주시기 바랍니다.

이 통합을 통해 앱 사용자들을 위한 더 안전하고 매혹적인 환경을 조성하여 콘텐츠가 적절하며 커뮤니티 지침 내에 유지되도록 보장할 수 있습니다.

<div class="content-ad"></div>

🌟 읽어 주셔서 감사합니다!

- 🚀 이 링크를 클릭하여 제 작업을 지원해 주세요: Support Me 🌟
- 👏 기사에 박수를 보내면 감사히 받습니다.
- 📌 보다 유익한 콘텐츠를 위해 Avishek Kumar를 팔로우하세요.

📣 더 많은 소식을 받아보세요

- 🔔 트위터에서 팔로우하기(X)
- 🔔 GitHub에서 팔로우하기 🥷
- 🔗 LinkedIn에서 연결하기

<div class="content-ad"></div>

# 스택데미크 🎓

끝까지 읽어주셔서 감사합니다. 떠나시기 전에:

- 작가를 클랩하고 팔로우해 주시면 감사하겠습니다! 👏
- 저희를 팔로우하세요: X | LinkedIn | YouTube | Discord
- 다른 플랫폼도 방문해 보세요: In Plain English | CoFeed | Differ
- 스택데미크닷컴에서 더 많은 콘텐츠를 만나보세요
