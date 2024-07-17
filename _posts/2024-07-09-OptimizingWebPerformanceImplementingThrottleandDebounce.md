---
title: "웹 성능 최적화 Throttle과 Debounce 구현 방법"
description: ""
coverImage: "/assets/img/2024-07-09-OptimizingWebPerformanceImplementingThrottleandDebounce_0.png"
date: 2024-07-09 13:18
ogImage:
  url: /assets/img/2024-07-09-OptimizingWebPerformanceImplementingThrottleandDebounce_0.png
tag: Tech
originalTitle: "Optimizing Web Performance: Implementing Throttle and Debounce"
link: "https://medium.com/@gaudrishabh/optimizing-web-performance-implementing-throttle-and-debounce-314ad9ed314b"
---

<img src="/assets/img/2024-07-09-OptimizingWebPerformanceImplementingThrottleandDebounce_0.png" />

안녕하세요,

빠른 웹 개발의 세계에서 성능은 사용자 경험을 좌우할 수 있습니다. 버벅거리는 웹사이트를 스크롤하거나 검색 창에 글을 입력하고 제안이 나타날 때까지 기다리는 것을 상상해보세요. 짜증나죠? 여기서 스로틀링(throttling)과 디바운싱(debouncing)의 힘이 발휘됩니다. 이 두 가지 간단하면서도 효과적인 기술은 웹 애플리케이션을 둔갑성이며 빠른 속도로 변환시킬 수 있습니다. 이 블로그에서는 스로틀링과 디바운싱이 무엇인지, 왜 중요한지, 그리고 JavaScript 프로젝트에서 어떻게 구현하여 최적의 성능과 원활한 사용자 경험을 보장할 수 있는지 살펴보겠습니다. 그럼 함께 자세히 알아보겠습니다!

## 스로틀링과 디바운싱 이해하기

<div class="content-ad"></div>

뚫고와 추월하는 방법은 시간이 지남에 따라 함수가 실행되는 빈도를 제어하는 기술입니다.

- 쓰로틀(Throttle): 지정된 기간 동안 함수가 한 번만 호출되도록 보장합니다.
- 디바운스(Debounce): 지난 호출 이후 지정된 기간이 경과한 후에만 함수가 호출되도록 보장합니다.

## 쓰로틀과 디바운스를 사용하는 이유

- 성능 최적화: 함수 호출 빈도를 줄여 애플리케이션 성능을 향상시킵니다.
- 사용자 경험: 반복된 클릭이나 과도한 API 호출과 같은 문제를 방지합니다.

<div class="content-ad"></div>

## 쓰로틀

정의: 쓰로틀은 특정 시간 동안 특정 활동이 없을 때에만 함수가 호출되도록 하는 프로그래밍 기술로, 지정된 시간 동안 한 번 이상 함수가 호출되지 않도록 하는 것을 말합니다. 빠르게 발생할 수 있는 이벤트(스크롤링, 크기 조정, 키 입력 등)을 처리할 때 성능을 최적화하는 데 특히 유용합니다.

예시 사용 사례:

- 스크롤 이벤트
- 크기 조정 이벤트

<div class="content-ad"></div>

자바스크립트로 구현하기:

```js
const getData = () => {
  console.log("데이터 가져오는 중....");
};
const throttle = function (func, limit) {
  let flag = true;
  let context = this,
    args = arguments;
  return function () {
    if (flag) {
      func.apply(context, args);
      flag = false;
    }
    setTimeout(() => {
      flag = true;
    }, limit);
  };
};
const betterGetData = throttle(getData, limit);
window.addEventListener("resize", betterGetData);
```

## 디바운스

정의: 디바운스는 시간이 많이 걸리는 작업이 너무 자주 발생하지 않도록 보장하는 프로그래밍 연습입니다. 성능과 응답성을 향상시킵니다. 창 크기 조정, 스크롤, 키 이벤트와 같이 빠르게 연속으로 발생하는 이벤트를 처리하는 데 특히 유용합니다. 디바운스 함수는 특정 작업이 최후 이벤트 이후 지정된 지연 기간에 한 번만 실행되거나 이벤트 버스트의 시작에서 실행되도록 보장합니다.

<div class="content-ad"></div>

예시 사용 사례:

- 검색 입력 이벤트
- 창 크기 조정 이벤트

JavaScript로의 구현:

```js
// index.js
let counter = 0;
const getData = () => {
  console.log("데이터를 가져왔습니다....", counter++);
};

const debounce = (cb, delay) => {
  let timer;
  return function () {
    let context = this,
      args = arguments;
    clearTimeout(timer);
    timer = setTimeout(() => {
      cb.apply(context, args);
    }, delay);
  };
};
const betterFunction = debounce(getData, 300); // 300밀리초
```

<div class="content-ad"></div>

```js
<!-- index.html -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Debounce</title>
</head>
<body>
    <input type="text" onkeypress={betterFunction()} />
    <script src="./index.js"></script>
</body>
</html>
```

## Throttle과 Debounce의 주요 차이점

- Throttle: 일정 간격으로 함수 실행을 보장합니다.
- Debounce: 트리거 없이 일정 기간 후에 함수 실행을 보장합니다.

## 결론

<div class="content-ad"></div>

웹 애플리케이션의 성능과 사용자 경험을 크게 향상시키기 위해 쓰로틀링(throttling)과 디바운싱(debouncing)을 이해하고 구현하는 것이 중요합니다. 일정 간격으로 실행되어야 하는 함수에는 쓰로틀링을 사용하고, 최후의 트리거로부터 지연된 후에 실행되어야 하는 함수에는 디바운싱을 사용하세요.

이러한 기술을 현명하게 적용하여 보다 원활한 상호작용과 더 빠른 대응성을 보장할 수 있습니다.

저는 리샤브 고드라고 합니다. 소프트웨어 개발자로 일하며 분산 시스템을 구축하는 것을 즐깁니다. 기술 관련 문의 사항이 있으시면 망설이지 말고 LinkedIn이나 포트폴리오를 통해 연락해 주세요.

즐거운 학습되세요! 😃

<div class="content-ad"></div>

읽어 주셔서 감사합니다! 피드백이나 코멘트가 있으시면 언제든지 알려주세요.
