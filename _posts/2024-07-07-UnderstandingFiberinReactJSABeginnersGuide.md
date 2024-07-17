---
title: "초보자를 위한 ReactJS의 Fiber 이해하기 첫걸음 가이드"
description: ""
coverImage: "/assets/img/2024-07-07-UnderstandingFiberinReactJSABeginnersGuide_0.png"
date: 2024-07-07 20:50
ogImage:
  url: /assets/img/2024-07-07-UnderstandingFiberinReactJSABeginnersGuide_0.png
tag: Tech
originalTitle: "Understanding Fiber in ReactJS: A Beginner’s Guide"
link: "https://medium.com/@achu1997singh/understanding-fiber-in-reactjs-a-beginners-guide-f8382dd37585"
---

<img src="/assets/img/2024-07-07-UnderstandingFiberinReactJSABeginnersGuide_0.png" />

리액트JS 애호가로서 "Fiber"라는 용어를 들어본 적이 있을 텐데, 과연 무엇일까요? 리액트를 처음 사용하시거나 깊이있게 이해하고 싶다면, 이 안내서가 여러분이 Fiber의 본질을 이해하고 그것이 리액트 어플리케이션에 미치는 영향을 이해하는 데 도움이 될 것입니다.

## Fiber가 무엇인가요?

Fiber는 리액트의 핵심 알고리즘을 완전히 재작성한 것으로, 리액트가 렌더링과 업데이트를 처리하는 방식을 개선하도록 설계되었습니다. Fiber를 리액트를 움직이는 엔진으로 생각해보세요. 리액트가 작업을 더 효율적으로 처리하도록 가능하게 하는 것입니다. 목표는 리액트가 렌더링 작업을 청크로 분할하고 여러 프레임으로 퍼지도록 허용하여 어플리케이션을 더 반응적이고 부드럽게 만드는 것입니다.

<div class="content-ad"></div>

## 왜 Fiber가 도입되었나요?

Fiber 이전에 React는 스택 기반의 조정 알고리즘을 사용했는데, 이는 작은 애플리케이션에는 잘 동작했지만 복잡한 애플리케이션에서는 어려움을 겪었습니다. 스택 기반 접근 방식은 종종 성능 문제를 야기하여 화면이 끊기거나 응답이 없는 사용자 인터페이스를 초래했습니다. Fiber는 이러한 문제를 해결하기 위해 도입되었으며 다음과 같은 방법으로 문제를 해결합니다:

- 업데이트에 우선순위 부여: Fiber는 업데이트에 우선순위를 부여할 수 있어 사용자 상호작용과 같은 중요한 작업이 먼저 처리되도록 보장합니다. 이는 앱이 무거운 업데이트 중에도 반응성을 유지하도록 합니다.
- 작업 일시 중지 및 재개: Fiber를 사용하면 React가 컴포넌트에서 작업을 일시 중지하고 더 중요한 작업을 처리한 다음 작업을 재개할 수 있습니다. 이를 통해 사용자 인터페이스가 고강도 작업 중에도 멈추지 않도록 합니다.
- 작업 분해: Fiber는 렌더링 작업을 작은 청크로 분해하여 React가 작업을 여러 프레임에 걸쳐 분산시키고 시간을 더 잘 관리할 수 있게 합니다.

## Fiber는 어떻게 작동하나요?

<div class="content-ad"></div>

더 명확한 이해를 돕기 위해 Fiber가 어떻게 작동하는지 살펴보겠습니다:

- 조정(Reconciliation): 컴포넌트의 상태나 props가 변경될 때 React는 UI를 업데이트해야 합니다. Fiber는 이를 조정이라고 하는 프로세스로 최적화하며, 작은 작업 단위로 나누어 처리합니다. 이를 통해 React는 필요에 따라 렌더링을 일시 중지하고 다시 시작할 수 있습니다.
- 우선순위 수준: Fiber는 업데이트에 다른 우선순위 수준을 할당합니다. 예를 들어, 클릭이나 타이핑과 같은 사용자 상호작용은 네트워크 요청 업데이트보다 더 높은 우선순위를 갖습니다. 이를 통해 UI가 집중적인 업데이트 중에도 반응성을 유지할 수 있습니다.
- 작업 루프: Fiber는 렌더링을 관리하기 위해 작업 루프를 사용합니다. 각 반복에서 Fiber는 작은 작업 단위를 처리합니다. 높은 우선순위 작업이 발생하면 현재 작업을 일시 중지하고 높은 우선순위 작업을 처리한 후 일시 중지한 작업을 다시 시작합니다.

## Fiber의 장점

- 성능 향상: 렌더링 작업을 나누고 우선순위에 따라 업데이트를 처리함으로써, Fiber는 React 애플리케이션의 성능을 크게 향상시킵니다. 특히 구성 요소가 많고 업데이트가 빈번한 복잡한 앱에 매우 유용합니다.
- 더 나은 사용자 경험: Fiber는 사용자 상호작용을 신속하게 처리하여 UI 지연을 줄이고 전반적인 사용자 경험을 향상시킵니다.
- 부드러운 애니메이션: 렌더링 작업을 효율적으로 관리하여 애플리케이션에서 부드러운 애니메이션과 전환을 구현하는 데 도움이 됩니다.

<div class="content-ad"></div>

## Fiber가 동작하는 예시

Fiber가 동작하는 것을 확인하기 위해 간단한 예시를 살펴봅시다. 매 초마다 업데이트되는 카운터 애플리케이션을 만든다고 상상해보세요. 버튼 클릭으로 카운트를 재설정할 수 있는 기능이 포함되어 있습니다.

```js
import React, { useState, useEffect } from "react";

function App() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const interval = setInterval(() => {
      setCount((prevCount) => prevCount + 1);
    }, 1000);

    return () => clearInterval(interval);
  }, []);

  return (
    <div>
      <h1>카운트: {count}</h1>
      <button onClick={() => setCount(0)}>초기화</button>
    </div>
  );
}

export default App;
```

이 예시에서는 매 초마다 카운트 상태가 업데이트되며, 카운트를 재설정할 수 있는 버튼이 있습니다. Fiber를 사용하면 카운트 상태의 업데이트가 효율적으로 처리되어 UI가 다른 중요 작업이 진행 중일 때도 응답성을 유지합니다.

<div class="content-ad"></div>

## 개인적 인사이트

Fiber에 대해 처음 배우기 시작했을 때 React 애플리케이션의 성능이 근본적으로 향상되는 방법이 놀라웠어요. 앱에 터보 부스트를 주는 느낌이에요! Fiber를 이해하고 나니 더 반응이 빠르고 효율적인 애플리케이션을 구축할 수 있어 개발 경험이 훨씬 더 원만해졌어요.

가장 만족스러운 순간 중 하나는 애니메이션이 얼마나 부드러워졌고 사용자 상호 작용이 얼마나 빠르게 처리되는지에 주목했을 때였어요. Fiber의 힘이 실제로 발휘되는 것을 보는 것은 눈을 뜨게 해주는 경험이었어요. 고성능 React 앱을 만드는 데 열성적이라면, Fiber에 뛰어들어 시간을 투자하는 것은 분명히 가치가 있어요.

## 결론

<div class="content-ad"></div>

마크다운(Markdown) 형식으로 테이블 태그를 변경해 주세요.
