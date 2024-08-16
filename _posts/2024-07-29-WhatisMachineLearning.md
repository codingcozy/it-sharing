---
title: "머신 러닝에 대해서 정리하기"
description: ""
coverImage: "/assets/img/2024-07-29-WhatisMachineLearning_0.png"
date: 2024-07-29 13:51
ogImage: 
  url: /assets/img/2024-07-29-WhatisMachineLearning_0.png
tag: Tech
originalTitle: "What is Machine Learning"
link: "https://dev.to/dakota_day/what-is-machine-learning-4g77"
isUpdated: true
updatedAt: 1723816444446
---



처음으로 '기계 학습'이라는 말을 듣게 되면, "시간이 지남에 따라 더 정확해지는 AI"라고 생각해요.

내 첫 인상이 꽤 정확하다는 느낌을 받지만, IBM은 더 직접적인 정의를 제공합니다: "AI 및 컴퓨터 과학의 한 분야로, 데이터 및 알고리즘을 사용하여 AI가 사람이 학습하는 방식을 모방하고, 점차 정확도를 향상시키도록 하는 데 중점을 둡니다." 
이 정보를 고려할 때, 어떻게 작동하나요?

### 알고리즘

모델 및 알고리즘을 살펴볼 때 기계 학습은 꽤 광범위한 주제입니다. 그들이 학습하는 방식을 보고 세분화해서 살펴볼 수 있습니다:

<div class="content-ad"></div>

- 지도 학습

![image0](/assets/img/2024-07-29-WhatisMachineLearning_0.png)

- 비지도 학습

![image1](/assets/img/2024-07-29-WhatisMachineLearning_1.png)

<div class="content-ad"></div>

- 강화 학습 알고리즘은 여기서 보상을 기반으로 합니다. 이것은 프로그램이 보상을 획들거나 잃는지에 따라 결정을 내립니다. 긍정적인 강화는 특정 행동으로 인해 이벤트가 발생할 때 발생하며, 그 행동은 강도나 빈도를 증가시킵니다. 부정적 강화는 부정적 조건을 피할 때 행동을 강화시킵니다. 이 기술의 학습은 반복적이므로 실수로부터 학습하고 그 지식을 다음 세대에 전달합니다. 비디오 게임을 고속으로 클리어하는 AI를 상상해보세요!

### 신경망

신경망을 살펴보겠습니다. 신경망은 우리 뇌의 사고 패턴을 모방하기 위해 설계되었습니다. 음성 패턴 인식, 번역 등과 같은 작업에 사용될 수 있습니다! 입력된 데이터에서 학습하고 해당 데이터를 기반으로 예측을 수행하여 더 많은 데이터가 있는 경우 더 정확해집니다. LearnCode.academy YouTube 채널의 간단한 프로그램을 사용하여 신경망이 어떻게 작동하는지 보여드릴 것입니다.

예제 프로그램으로는 배경색을 설정하는 색 선택기 입력을 포함하고 기본적으로 흰색인 몇 가지 예제 텍스트가 포함된 html을 사용할 것입니다.

<div class="content-ad"></div>

#### Brain.js

Brain.js는 뉴럴 네트워크를 만드는 데 사용되는 JavaScript 라이브러리입니다. 이 라이브러리를 우리 프로그램에서 사용할 예정입니다.

뉴럴 네트워크를 사용하면 배경이 너무 어두운지 또는 너무 밝은지 판단하고 텍스트 색상을 변경하여 더 가독성 있게 만들 수 있습니다.

자, 이제 우리 뇌를 만들어 봅시다:

<div class="content-ad"></div>

```js
// 이 코드는 새 신경망을 초기화합니다.
const network = new brain.NeuralNetwork();
```

쉽죠?

인간 뇌는 받은 정보를 통해 학습하죠, 신경망도 마찬가지입니다. 우리는 네트워크에서 .train 메소드를 사용할 수 있어요. 이것이 바로 네트워크가 결과를 예측하는 데 사용할 것입니다. 우리의 경우, 이것은 텍스트 색상을 변경할지 여부를 결정할 것입니다. 이제 네트워크에 처음 데이터를 제공해 봅시다.

```js
// .train은 테스트 데이터가 담긴 배열을 매개변수로 사용합니다
network.train([
  
// 색상의 값을 255로 나누어서 RGB 값을 가져올 수 있습니다
// 이 경우에는 출력은 배경이 밝은지 어두운지에 대한 것이어야 합니다
{input: {r: 0, g: 1, b: 0}, output: {light: 1},
{input: {r: 0.33, g: 0.24, b: 0.29}, output: {dark: 1},
]);
```

<div class="content-ad"></div>

자, 이제 색상 입력에 대한 이벤트 리스너를 만들어 보겠습니다! 여기서는 배경이 밝은지 어두운지를 결정할 것입니다.

```js
input.addEventListener('input', (color) => {
  // 이것은 hex 색상 데이터를 rgb 값으로 변환해주는 보조 함수입니다.
  const rgb = getRgb(color.target.value);

  // 여기서 배경색을 설정합니다.
  text.style.background = color.target.value;

  const result = brain.likely(rgb, network);

  text.style.color = result === 'dark' ? 'white' : 'black';
})
```

그러면 우리의 결과를 밝은 값이나 어두운 값으로 만들고 싶다고 하죠. Brain.js에 우리가 여기에서 사용할 수 있는 메소드가 있습니다. .likely 메소드는 우리의 rgb 값과 신경망을 인수로 취합니다. .likely는 그럼 우리가 훈련시킨 데이터를 사용하여 색깔이 밝은지 어두운지에 대한 가능성을 비교해줄 겁니다!

이것을 페이지에 표시하려고 하면 약간 이상한 일이 발생할 수 있습니다. 우리는 훈련 데이터에 두 가지 입력만 제공했기 때문에 네트워크가 일부 rgb 조합이 밝은지 어두운지 판단하는 데 문제가 있을 수 있습니다.

<div class="content-ad"></div>

수정하는 건 쉬워요! 그냥 뇌를 훈련시키기 위해 더 많은 데이터를 추가하면 돼요!

```js
{input: {r: 0, g: 1, b: 0}, output: {light: 1},
{input: {r: 0.33, g: 0.24, b: 0.29}, output: {dark: 1},
{input: {r: 0.62, g: 0.72, b: 0.88}, output: {light: 1},
{input: {r: 0.1, g: 0.84, b: 0.72}, output: {light: 1},
{input: {r: 0.74, g: 0.78, b: 0.86}, output: {light: 1},
{input: {r: 0.31, g: 0.35, b: 0.41}, output: {dark: 1},
{input: {r: 0, g: 1, b: 0.65}, output: {light: 1},
```

이 훈련 데이터로 우리 뇌는 배경이 밝은지 어둡지를 보다 정확하게 예측할 수 있게 될 거예요.

생각해 보면 기계 학습은 우리 삶의 여러 영역에 사용돼: 추천 시스템, 소셜 미디어 연결, 이미지 인식 등 다양한 분야에서. 기계 학습이 우리 삶의 여러 측면을 크게 향상시켰다고 말해도 될 것 같아요. 그리고 가장 좋은 점? 앞으로만 더 나아질 수 있다는 거에요!

<div class="content-ad"></div>

더 알고 싶다면 사용된 소스를 확인해보세요:
- 머신 러닝(ML)이란 무엇인가
- 초보자를 위한 최고의 머신 러닝 알고리즘 10가지: 지도 및 그 이상
- 지도 학습이란 무엇인가
- 비지도 학습이란 무엇인가
- 강화 학습이란 무엇인가
- Brain.js 문서
- 초보자를 위한 머신 러닝 튜토리얼 - 자바스크립트 활용!

사용된 이미지:
- 비지도 학습
- 지도 학습