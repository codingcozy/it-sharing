---
title: "Vuejs로 재미있는 SVG 애니메이션 만드는 방법"
description: ""
coverImage: "/assets/img/2024-07-09-CreatingaFunnySVGAnimationinVuejs_0.png"
date: 2024-07-09 08:35
ogImage:
  url: /assets/img/2024-07-09-CreatingaFunnySVGAnimationinVuejs_0.png
tag: Tech
originalTitle: "Creating a Funny SVG Animation in Vue.js"
link: "https://medium.com/@andras.tovishati/creating-a-funny-svg-animation-in-vue-js-3635ea8c1779"
---

SVG 애니메이션은 웹 애플리케이션에 많은 개성과 상호 작용을 불러올 수 있습니다. 이 블로그 포스트에서는 가벼운 JavaScript 라이브러리인 GrafikJS를 사용하여 Vue.js 프로젝트에서 재미있는 SVG 애니메이션을 만드는 방법을 살펴보겠습니다. 이 튜토리얼의 끝에는 버튼 클릭으로 트리거할 수 있는 튀는 스마일 페이스 애니메이션이 완성될 것입니다.

![이미지](/assets/img/2024-07-09-CreatingaFunnySVGAnimationinVuejs_0.png)

# 컴포넌트 구조

`template` 섹션에서는 GrafikJS 컴포넌트를 사용하여 SVG 구조를 정의합니다.

<div class="content-ad"></div>

```js
<template>
  <Canvas>
    <Circle :r="180" :left="200" :top="200" :strokeWidth="2" fill="none" />
    <Group :left="200" :top="200" :animation="animation">
      <Circle :r="10" :left="-80" :top="-50" :strokeWidth="2" fill="none" />
      <Circle :r="10" :left="80" :top="-50" :strokeWidth="2" fill="none" />
      <Path
        d="M 0 0 C 0 40 100 40 100 0"
        :left="0"
        :top="50"
        :strokeWidth="2"
        fill="none"
      />
    </Group>
  </Canvas>
  <button @click="play()">PLAY</button>
</template>
```

- `Canvas`: 우리의 SVG 요소들을 담을 루트 컨테이너입니다.
- `Circle`: 우리 웃는 얼굴의 바깥쪽 원을 만듭니다.
- `Group`: 얼굴 요소(눈과 입)를 그룹화하고 애니메이션을 적용합니다.
- `Path`: 입모양을 만들기 위해 cubic Bézier 곡선을 사용하는 웃는 얼굴의 입을 만듭니다.
- 애니메이션을 트리거하는 버튼이 있습니다.

# Script Setup

이제, 우리가 애니메이션 로직을 처리하고 GrafikJS와 상호 작용하는 스크립트 설정을 자세히 살펴보겠습니다:

<div class="content-ad"></div>

## 가져오기

먼저, SVG 요소를 그리고 애니메이션을 관리하기 위해 GrafikJS (Canvas, Circle, Group, Path)에서 필요한 구성 요소를 가져옵니다.

```js
import { Canvas, Circle, Group, Path, useCanvas } from "@grafikjs/vue";
```

## useCanvas() 훅.

<div class="content-ad"></div>

이 후크는 GrafikJS 캔버스 객체에 액세스할 수 있게 합니다. 이를 사용하여 재생 동작을 바인딩하여 애니메이션을 실행합니다.

```js
const {
  actions: { play },
} = useCanvas(
  null,
  (canvas) => ({
    play: canvas.getAnimation().play.bind(canvas.getAnimation()),
  }),
  ""
);
```

여기서 canvas.getAnimation()은 GrafikJS 캔버스 인스턴스와 관련된 애니메이션 객체를 검색합니다. 이 애니메이션 객체의 play 메서드는 우리의 Vue.js 컴포넌트 컨텍스트에서 작동하도록 바인딩됩니다.

useCanvas에 의해 반환된 actions 객체에는 정의된 동작(이 경우 재생만)이 포함되어 있습니다. 이러한 동작은 Vue.js 컴포넌트의 메서드와 이벤트 핸들러 내에서 사용할 수 있습니다.

<div class="content-ad"></div>

<button @click="play()">재생</button>

## 애니메이션 객체

애니메이션 객체는 웃는 얼굴 이모티콘의 움직임과 행동을 정의하는 데 중요합니다. 이 객체는 서로 다른 애니메이션 속성(이 경우 상단과 왼쪽)과 이에 해당하는 키프레임을 지정하는 트랙으로 구성됩니다.

```javascript
const animation = {
  tracks: [
    {
      property: "top",
      keyframes: [
        {
          to: 500,
          value: 250,
          easing: "cubicOut",
        },
      ],
    },
    {
      property: "left",
      keyframes: [
        {
          to: 500,
          value: 200,
        },
        {
          to: 1000,
          value: 250,
          easing: "cubicOut",
        },
        {
          to: 1500,
          value: 150,
          easing: "cubicOut",
        },
        {
          to: 2000,
          value: 200,
          easing: "cubicOut",
        },
      ],
    },
  ],
};
```

<div class="content-ad"></div>

애니메이션 객체 내 각 트랙은 SVG 요소의 특정 속성이 시간에 따라 어떻게 애니메이션화되어야 하는지를 정의합니다. 예를 들어, 상단 속성은 초기 위치에서 더 높은 위치로 애니메이션을 수행할 수 있으며, 왼쪽 속성은 화면을 가로질러 수평으로 이동할 수 있습니다.

각 트랙 내의 키프레임은 애니메이션화되는 속성의 특정 시간(딜레이)과 해당 속성의 대상값(값)을 자세히 설명합니다. 이 세분성을 통해 애니메이션의 타이밍과 궤적을 정밀하게 조절할 수 있습니다.

cubicOut와 같은 이징 함수는 일부 키프레임에 적용되어 부드러운 가속과 감속을 보장하여 애니메이션의 자연스러운 느낌과 모양을 높일 수 있습니다.

# 전체 코드

<div class="content-ad"></div>

위 코드는 완전한 컴포넌트를 포함하고 있어요. StackBlitz에서 SVG 애니메이션을 실제로 확인해보세요.

```js
<script setup>
import { Canvas, Circle, Group, Path, useCanvas } from '@grafikjs/vue';

const {
  actions: { play },
} = useCanvas(
  null,
  (canvas) => ({
    play: canvas.getAnimation().play.bind(canvas.getAnimation()),
  }),
  ''
);
const animation = {
  tracks: [
    {
      property: 'top',
      keyframes: [
        {
          to: 500,
          value: 250,
          easing: 'cubicOut',
        },
      ],
    },
    {
      property: 'left',
      keyframes: [
        {
          to: 500,
          value: 200,
        },
        {
          to: 1000,
          value: 250,
          easing: 'cubicOut',
        },
        {
          to: 1500,
          value: 150,
          easing: 'cubicOut',
        },
        {
          to: 2000,
          value: 200,
          easing: 'cubicOut',
        },
      ],
    },
  ],
};
</script>

<template>
  <Canvas>
    <Circle :r="180" :left="200" :top="200" :strokeWidth="2" fill="none" />
    <Group :left="200" :top="200" :animation="animation">
      <Circle :r="10" :left="-80" :top="-50" :strokeWidth="2" fill="none" />
      <Circle :r="10" :left="80" :top="-50" :strokeWidth="2" fill="none" />
      <Path
        d="M 0 0 C 0 40 100 40 100 0"
        :left="0"
        :top="50"
        :strokeWidth="2"
        fill="none"
      />
    </Group>
  </Canvas>
  <button @click="play()">재생</button>
</template>
```

# 결론

GrafikJS를 사용하면 Vue.js에서 SVG 애니메이션을 만들고 제어할 수 있어요. 몇 줄의 코드로 간단하지만 재미있는 튕기는 황금볼 애니메이션을 만들었어요. 여러분도 다양한 SVG 모양과 애니메이션 속성을 실험하여 고유한 애니메이션을 만들어보세요. 즐거운 코딩되세요!

<div class="content-ad"></div>

만약 이 기사가 마음에 드셨다면, 제 Medium 팔로우도 잊지마세요! :)
