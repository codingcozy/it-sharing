---
title: "React-useEffect 내부에서 async await를 사용하지 말아야 하는 이유"
description: ""
coverImage: "/assets/img/2024-07-09-WhynotuseasyncawaitinsideReact-useEffect_0.png"
date: 2024-07-09 13:32
ogImage:
  url: /assets/img/2024-07-09-WhynotuseasyncawaitinsideReact-useEffect_0.png
tag: Tech
originalTitle: "Why not use async await inside React-useEffect?"
link: "https://medium.com/stackademic/why-not-use-async-await-inside-react-useeffect-e924642e6efe"
---

가끔 func.apply가 함수가 아닙니다 에러가 발생하고 "이번에는 뭘 잘못했지?" 하고 궁금했던 적이 있나요?

첫 번째 렌더링에서는 모든 것이 원할하게 작동하지만, 컴포넌트가 다시 렌더링될 때마다 문제가 발생하고 콘솔에 이 오류가 표시됩니다. 왜 그럴까요?

아래의 예시처럼 useEffect 훅 내에서 직접 async/await 또는 fetch 함수를 사용했기 때문일 수 있습니다:

<div class="content-ad"></div>

```js
useEffect(async () => {
    const data = await fetchDropdowns();
    console.log(data);
}, []);

직접적으로 useEffect 내에서 async/await를 사용하면, React가 초기 효과를 올바르게 처리하기 때문에 첫 렌더링에서는 "약간" 작동할 수 있습니다. 그러나 다시 렌더링될 때, 효과를 정리해야 하거나 변경된 종속성이 있는 경우, React는 useEffect의 정리 함수를 호출하려고 할 수 있습니다 - 이로 인해 의도하지 않은 동작(메모리 누수, 경쟁 조건 등)이 발생할 수 있습니다.

# React가 정리 함수를 처리하는 방법은?

React가 정리 함수를 호출할 때, JavaScript의 Function.prototype.apply 메서드를 사용하여 정리 함수를 호출합니다. 이 방법을 사용하면 지정된 this 값과 인수로 함수를 호출할 수 있습니다.
```

<div class="content-ad"></div>

요즘 많이 사용하는 리액트 코드 예제를 하나 드릴게요:

```js
const effect = async () => {
  await fetchDropdowns();
};

// 처음 렌더링 시
let cleanupFunction = effect();

// 다시 렌더링하거나 언마운트될 때
if (typeof cleanupFunction === "function") {
  cleanupFunction();
} else {
  // 만약 cleanupFunction이 프로미스라면 여기서 에러가 발생할 수 있어요
  cleanupFunction.apply(null);
}
```

cleanupFunction이 프로미스라는 상황에서 apply를 호출하면 TypeError: func.apply is not a function 에러가 발생하는군요.

# 문제 해결 방법

<div class="content-ad"></div>

IIFE(즉시 호출 함수 표현식)에서 async 함수를 호출해보세요!

```js
useEffect(() => {
  //IIFE
  (async () => {
    try {
      const response = await fetchDropdowns();
    } catch (error) {
      console.error("데이터를 불러오는 중 에러가 발생했습니다:", error);
    }
  })();
}, []); // 의존성 배열이 비어있는 상태에서는 이 효과가 한 번만 실행됩니다.
```

비동기 함수를 정의하고 즉시 호출하는 방식입니다. useEffect가 프라미스를 반환하지 않도록 하여 React의 정리 메커니즘과 혼동을 피할 수 있습니다. 여기서 async 함수 내에서 발생하는 에러는 쉽게 관리하고 디버깅할 수 있습니다.

다음에 또 만나요!

<div class="content-ad"></div>

# Stackademic 🎓

끝까지 읽어 주셔서 감사합니다. 떠나기 전에:

- 작가를 클랩하고 팔로우해주세요! 👏
- 팔로우하기: X | LinkedIn | YouTube | Discord
- 다른 플랫폼 방문하기: In Plain English | CoFeed | Differ
- Stackademic.com에서 더 많은 콘텐츠 보기
