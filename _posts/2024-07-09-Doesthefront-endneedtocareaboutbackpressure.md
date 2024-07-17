---
title: "프론트엔드에서 백프레셔를 신경 써야 할까요"
description: ""
coverImage: "/assets/img/2024-07-09-Doesthefront-endneedtocareaboutbackpressure_0.png"
date: 2024-07-09 17:12
ogImage:
  url: /assets/img/2024-07-09-Doesthefront-endneedtocareaboutbackpressure_0.png
tag: Tech
originalTitle: "Does the front-end need to care about back pressure?"
link: "https://medium.com/@beckmoulton/does-the-front-end-need-to-care-about-back-pressure-f06dc68dd274"
---

# 백프레셔(back pressure)란?

백프레셔는 생산자와 소비자 간의 속도가 일치하지 않을 때의 해결책을 가리킵니다. 생산자의 속도가 소비자보다 빠른 경우, 소비자는 생산자로 인해 압도당할 수 있습니다. 이 때, 생산자의 속도를 제한하기 위한 메커니즘이 필요하며, 이를 백프레셔라고 합니다.

아직 백프레셔가 무엇인지 이해하지 못하는 친구들도 있을 것입니다. 두 가지 예시를 통해 설명해보겠습니다. 하나는 파일 읽기 및 쓰기입니다.

예를 들어, 읽기 속도가 150M/s이고 쓰기 속도가 100M/s인 경우, 읽기 속도가 쓰기 속도보다 50M/s 더 빠름을 알 수 있습니다. 이 경우, 쓰기를 위해 매 초 50M의 데이터를 캐시해야 합니다. 그러나 캐시된 데이터가 메모리 크기를 초과하면 OOM(Out Of Memory)이 발생합니다. 이 때, 읽기 속도를 제한하기 위한 메커니즘이 필요한데, 바로 백프레셔입니다.

<div class="content-ad"></div>

다른 상황에서, 프론트엔드 관련 예시로 UI 렌더링을 살펴보겠습니다. 초당 20,000개의 이벤트를 발생시키는 WebSocket의 출력이 있는 경우, 이러한 이벤트를 페이지에 렌더링해야 합니다. 그러나 우리의 페이지는 초당 10,000개의 이벤트만 렌더링할 수 있습니다. 이때, 다음 그림에서 보여지는 문제가 발생할 수 있습니다.

![Figure](/assets/img/2024-07-09-Doesthefront-endneedtocareaboutbackpressure_0.png)

결국, 페이지가 멈출 것이 분명합니다. 이때 WebSocket의 출력 속도를 제한하는 메커니즘이 필요한데, 이것이 바로 백프레셔(back pressure)입니다.

# 프론트엔드는 백프레셔에 신경 써야 할까요?

<div class="content-ad"></div>

위의 예에서 우리는 백프레셔(back pressure)가 생산자와 소비자 속도의 불일치를 해결하기 위한 해결책이라는 것을 볼 수 있습니다. 그렇다면 프론트 엔드에서 백프레셔에 신경을 써야 할까요? 답은 네입니다.

그렇다면 프론트엔드 개발에서 백프레셔를 해결하기 위해 어떤 오픈 소스 라이브러리를 사용해야 할까요? 여기서 한 가지 라이브러리를 추천합니다. 그것은 RxJS입니다.

RxJS는 옵저버블 시퀀스를 사용하여 비동기적 및 이벤트 중심 프로그램을 작성하는 데 사용되는 라이브러리입니다. Observable이라는 코어 타입을 제공하며, 이는 이벤트 시퀀스를 다루는 데 사용할 수 있는 메소드가 있는 클래스입니다. RxJS는 다른 몇 가지 타입도 제공하지만 Observable이 가장 기본적인 타입입니다.

RxJS에서는 throttleTime, debounceTime, sampleTime 및 auditTime과 같은 오퍼레이터를 통해 이벤트의 출력 속도를 제한할 수 있으며, 이를 통해 백프레셔 문제를 해결할 수 있습니다.

<div class="content-ad"></div>

실제 예시를 살펴보겠습니다. 클릭한 버튼으로 요청을 보내는 경우를 가정해보면, 사용자가 1초 동안 버튼을 계속 클릭하지 못하도록 RxJS를 사용하여 이 문제를 해결할 수 있습니다.

```js
import { fromEvent } from "rxjs";
import { throttleTime } from "rxjs/operators";

const button = document.querySelector("button");
fromEvent(button, "click")
  .pipe(throttleTime(1000))
  .subscribe(() => console.log("Clicked!"));
```

위 코드에서는 throttleTime(1000)를 사용하여 이벤트의 출력 속도를 제한하므로, 사용자가 1초 내에 연속해서 버튼을 클릭하는 문제를 해결할 수 있습니다.

그래서 RxJS 이외에도 다른 해결책이 있을까요? 네, 답은 있습니다.

<div class="content-ad"></div>

# 스로틀 기능 사용하기: lodash의 스로틀 함수를 사용해보세요

```js
import throttle from "lodash/throttle";
const button = document.querySelector("button");
button.addEventListener(
  "click",
  throttle(() => {
    console.log("Clicked!");
  }, 1000)
);
```

# 버퍼링: JavaScript의 배열을 버퍼로 활용할 수 있어요

```js
let buffer = [];
const producer = (data) => {
  buffer.push(data);
};
const consumer = () => {
  while (buffer.length) {
    const data = buffer.shift();
    // 데이터 처리
    console.log(`데이터 처리됨: ${data}`);
  }
};
```

<div class="content-ad"></div>

# requestAnimationFrame을 사용하면, 브라우저가 다음 그림을 그리기 전에 애니메이션 업데이트를 수행할 수 있는 API입니다. 이 API를 사용하면 이벤트 처리 속도를 제한하여 브라우저의 새로 고침 속도에 맞출 수 있습니다. 일반적으로 1초에 60회입니다. 이를 통해 짧은 시간 내에 많은 이벤트를 처리하는 것을 피함으로써 백프레셔 이슈를 효과적으로 줄일 수 있습니다.

```js
let tasks = []; // 이는 큰 작업 배열로 가정합니다.
const processTask = (task) => {
  // 작업 처리
  console.log(`작업 처리됨: ${task}`);
};
const scheduleTaskProcessing = () => {
  if (tasks.length > 0) {
    processTask(tasks.shift());
    requestAnimationFrame(scheduleTaskProcessing);
  }
};
scheduleTaskProcessing();
```

# requestIdleCallback을 사용하여 이벤트의 출력 속도를 제한할 수 있습니다: requestIdleCallback은 브라우저가 유휴 상태일 때 함수를 실행할 수 있어 작업 실행 속도를 조절하는 데 사용할 수 있습니다.

```js
let tasks = []; // 이는 큰 작업 배열로 가정합니다.
const processTask = (task) => {
  // 작업 처리
  console.log(`작업 처리됨: ${task}`);
};
const scheduleTaskProcessing = () => {
  requestIdleCallback((deadline) => {
    while (deadline.timeRemaining() > 0 && tasks.length > 0) {
      processTask(tasks.shift());
    }
    if (tasks.length > 0) {
      scheduleTaskProcessing();
    }
  });
};
scheduleTaskProcessing();
```

<div class="content-ad"></div>

# DOM 요소의 가시성 문제를 해결하기 위해 IntersectionObserver를 사용하세요 : IntersectionObserver는 DOM 요소의 가시성을 모니터링하는 데 사용될 수 있으며, 보이지 않는 요소의 처리를 지연시켜 작업 수를 줄일 수 있습니다.

```js
const observer = new IntersectionObserver((entries, observer) => {
  entries.forEach((entry) => {
    if (entry.isIntersecting) {
      // entry 처리
      console.log(`Entry processed: ${entry.target.id}`);
      observer.unobserve(entry.target);
    }
  });
});

// elements가 DOM 요소로 이루어진 배열이라고 가정
elements.forEach((element) => {
  observer.observe(element);
});
```
