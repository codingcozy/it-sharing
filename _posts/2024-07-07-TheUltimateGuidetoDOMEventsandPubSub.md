---
title: "DOM 이벤트와 Pub Sub에 대한 궁극적인 가이드"
description: ""
coverImage: "/assets/img/2024-07-07-TheUltimateGuidetoDOMEventsandPubSub_0.png"
date: 2024-07-07 20:54
ogImage:
  url: /assets/img/2024-07-07-TheUltimateGuidetoDOMEventsandPubSub_0.png
tag: Tech
originalTitle: "The Ultimate Guide to DOM Events and Pub Sub"
link: "https://medium.com/@pinjarirehan/the-ultimate-guide-to-dom-events-and-pub-sub-fe1c36e4eacb"
---

![이미지](/assets/img/2024-07-07-TheUltimateGuidetoDOMEventsandPubSub_0.png)

안녕, 개발자 여러분! 이쁜 버튼을 클릭했을 때 화면 내용이 갑자기 바뀌는 걸 어떻게 할 수 있는지 궁금했던 적이 있나요? 이 모든 것은 이벤트와 조용한 메신저들 덕분에 웹 애플리케이션이 원할하게 동작합니다.

하지만 이런 이벤트들은 어떻게 처리될까요? 준비되셨나요? JavaScript의 이벤트 처리의 두 가지 주요 접근 방식인 DOM Events와 Pub/Sub의 세계로 깊게 들어가보겠습니다.

![이미지](https://miro.medium.com/v2/resize:fit:1400/1*qV4VBwAHcpo4JQ3c2Siypw.gif)

<div class="content-ad"></div>

## 이벤트 101

당신의 웹 애플리케이션을 화려한 이벤트로 상상해보세요.

이벤트는 사람들을 통해 전해지는 메아리와 같습니다. 애플리케이션의 다양한 이벤트(파티 참가자)가 중요한 일이 발생했다는 것을 알리는 것(예를 들어 누군가가 코드로 펀치볼을 떨어뜨린 경우!)처럼요.

이러한 이벤트는 특정 동작을 유발하여 사람들을 흥미롭고 다이나믹하게 유지시킵니다.

<div class="content-ad"></div>

이벤트를 처리하는 두 가지 주요 접근 방식은 DOM 이벤트와 Pub/Sub입니다. 각각 살펴보겠습니다.

## DOM 이벤트

![이미지](/assets/img/2024-07-07-TheUltimateGuidetoDOMEventsandPubSub_1.png)

DOM 이벤트는 특정 이벤트에 대한 소문과 유사합니다.

<div class="content-ad"></div>

특정 구성 요소와 Document Object Model (DOM) 내에 특정한 연결이 있습니다. 이것들은 여러분의 웹 페이지의 청사진과 관련이 있어요.

클릭, 마우스 오버, 키 입력 등이 일반적인 이벤트 유형입니다.

이러한 이벤트들을 주시하기 위해 addEventListener 함수를 사용합니다. 이벤트에 참가하는 사람들 (이벤트 리스너들)에게 대화기를 나눠주는 것처럼 생각해보세요.

관련된 이벤트가 발생했을 때 (예: 누군가가 "화재!"라고 소리치는 것), 대화기가 팝하고 리스너(함수)가 조치를 취합니다.

<div class="content-ad"></div>

## 예시

여기 일반적인 DOM 이벤트 몇 가지가 있어요:

- click - 사용자가 요소를 클릭했을 때 발생.
- mouseover - 사용자가 요소 위에 마우스를 올렸을 때 발생.
- keydown - 사용자가 키보드에서 키를 눌렀을 때 발생.

## 이벤트 핸들링

<div class="content-ad"></div>

DOM 요소에 이벤트 리스너를 추가하는 것은 간단해요.

이벤트를 정의하고 발생할 때 실행될 코드를 addEventListener를 사용해 설정해요. 예를 들면:

```js
document.getElementById("myButton").addEventListener("click", function () {
  alert("Button clicked!");
});
```

## 장점:

<div class="content-ad"></div>

- 사용하기 쉽고 간단한 UI 상호 작용에 완벽합니다.
- 내장 브라우저 지원이 있어 추가 라이브러리가 필요하지 않습니다.

## 단점:

- 강한 결합: 과도한 사용은 코드가 엉망이 될 수 있습니다.
- 한정된 확장성: 복잡한 구성 요소 상호 작용에 적합하지 않습니다.

# Pub/Sub

<div class="content-ad"></div>

![이미지](/assets/img/2024-07-07-TheUltimateGuidetoDOMEventsandPubSub_2.png)

Pub/Sub은 Publish-Subscribe의 약어로, 더 구조화된 접근법을 사용합니다. DOM 컴포넌트에 의존하는 대신, 이는 이벤트 메시지 게시판과 유사한 중앙 메시지 버스를 사용합니다. 아래는 요약입니다:

- 발행자(Publishers): 이벤트 알리미로, 메시지(이벤트)를 메시징 버스를 통해 방송하는 역할입니다.
- 구독자(Subscribers): 특정 공지(이벤트)를 수신하기 위해 등록한 방문자로, 관련 상호작용이 발생하면 구독자는 알림을 받고 조치를 취할 수 있습니다.

## 실제 사용 사례

<div class="content-ad"></div>

퍼브/섭은 자바스크립트 이벤트에만 해당하는 것이 아닙니다.

높은 이메일 전달량의 메시지 브로커 및 실시간 알림(화면에 계속 뜨는 세일 팝업)과 같은 일상적인 사용 사례에서도 사용됩니다.

## 장점:

- 디커플링: 컴포넌트의 분리를 유지하여 코드를 더 명확하고 유지하기 쉽게 만듭니다(이벤트 루머 루프 없음!).
- 확장성: 다양한 부분 간의 복잡한 상호 작용을 쉽게 처리합니다.

<div class="content-ad"></div>

## 단점:

- 오버헤드: 메시지 버스 설정은 DOM 이벤트 설정보다 약간 복잡합니다.
- 혼란의 가능성: 너무 많은 메시지는 데이터 과부하를 일으킬 수 있습니다 (이벤트 과부하도 가능?).

# DOM 이벤트 대 Pub/Sub

백만 달러 질문은: 어느 것을 사용해야 할까요? 여기가 바로 치트 시트입니다:

<div class="content-ad"></div>

![DOM Events Image](/assets/img/2024-07-07-TheUltimateGuidetoDOMEventsandPubSub_3.png)

전반적으로, DOM 이벤트는 간단한 상호작용에 대해 다소 더 잘 작동합니다.

그러나, 결합 해제 및 확장성의 장점은 종종 더 큰 시스템에서 이를 능가합니다.

# 코드 타임!

<div class="content-ad"></div>

## 간단한 DOM 이벤트 처리

```js
// 이벤트 리스너를 추가하기 전에 DOM이 완전히 로드되었는지 확인
document.addEventListener("DOMContentLoaded", function () {
  // ID로 버튼 요소 가져오기
  const button = document.getElementById("myButton");

  // 이벤트 리스너를 추가하기 전에 버튼 요소가 있는지 확인
  if (button) {
    button.addEventListener("click", function () {
      console.log("버튼이 클릭되었습니다!");
    });
  } else {
    console.error('ID가 "myButton"인 버튼 요소를 찾을 수 없습니다.');
  }
});
```

## 간단한 Pub/Sub 구현

```js
// Pub/Sub 모듈
const PubSub = (function () {
  const events = {};

  function subscribe(event, callback) {
    if (typeof callback !== "function") {
      throw new Error("콜백은 함수여야 합니다");
    }
    if (!events[event]) {
      events[event] = [];
    }
    // 중복 구독 방지
    if (!events[event].includes(callback)) {
      events[event].push(callback);
    }
  }

  function publish(event, data) {
    if (events[event]) {
      events[event].forEach((callback) => callback(data));
    }
  }

  function unsubscribe(event, callback) {
    if (events[event]) {
      events[event] = events[event].filter((cb) => cb !== callback);
    }
  }

  return {
    subscribe,
    publish,
    unsubscribe,
  };
})();

// 사용 예시
PubSub.subscribe("buttonClicked", function (data) {
  console.log("데이터와 함께 버튼이 클릭되었습니다:", data);
});

// 버튼 클릭 이벤트 시뮬레이션
PubSub.publish("buttonClicked", { buttonId: "myButton" });

// 구독 취소 예제
const callback = function (data) {
  console.log("다른 버튼 클릭 핸들러:", data);
};
PubSub.subscribe("buttonClicked", callback);
PubSub.unsubscribe("buttonClicked", callback);
```

<div class="content-ad"></div>

# Best Practices

## DOM Events

- 더 읽기 쉬운 코드를 위해 의미있는 이벤트 리스너 이름을 사용하세요 (예: button.addEventListener('click', handleButtonClick)).
- 더 이상 필요하지 않은 경우 이벤트 리스너를 제거하여 메모리 누수를 줄이세요 (이벤트 후 정리하는 것 같은!).

## Pub/Sub

<div class="content-ad"></div>

- 혼란을 피하기 위해 이벤트에 대한 자세한 명명 표준을 설정하세요.
- 이벤트 수명주기를 위한 더 견고하고 기능이 풍부한 pub/sub 구현을 만들기 위해 EventEmitter와 같은 라이브러리를 사용해보세요.

# 언제 무엇을 선택해야 할까요?

웹사이트를 만든다고 가정해보겠습니다. 다음은 이벤트 처리의 우승자를 선택하는 데 도움이 되는 개요입니다:

![이미지](/assets/img/2024-07-07-TheUltimateGuidetoDOMEventsandPubSub_4.png)

<div class="content-ad"></div>

# 마무리 말씀

그래서 여기까지입니다. DOM 이벤트와 Pub/Sub는 둘 다 강력한 JavaScript 이벤트 처리 방식입니다.

각각을 실험해서 자신의 필요에 가장 잘 맞는 방법을 찾아보세요.

창의적으로 시도해 보는 것을 두려워하지 마세요! Socket.IO와 같은 고급 Pub/Sub 라이브러리는 실시간 통신 및 다양한 이벤트 처리에 대한 추가 기능을 제공할 수도 있습니다.

<div class="content-ad"></div>

![Image 1](/assets/img/2024-07-07-TheUltimateGuidetoDOMEventsandPubSub_5.png)

![Image 2](/assets/img/2024-07-07-TheUltimateGuidetoDOMEventsandPubSub_6.png)
