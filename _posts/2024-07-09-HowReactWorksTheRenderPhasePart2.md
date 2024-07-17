---
title: "리액트 작동 원리 렌더 단계 - 파트 2"
description: ""
coverImage: "/assets/img/2024-07-09-HowReactWorksTheRenderPhasePart2_0.png"
date: 2024-07-09 13:27
ogImage:
  url: /assets/img/2024-07-09-HowReactWorksTheRenderPhasePart2_0.png
tag: Tech
originalTitle: "How React Works: The Render Phase — Part 2"
link: "https://medium.com/@mastermusili/how-react-works-the-render-phase-part-2-318360b23cdb"
---

![How React Works - The Render Phase Part 2 - Image 0](/assets/img/2024-07-09-HowReactWorksTheRenderPhasePart2_0.png)

지난 글에서 렌더가 어떻게 트리거되는지를 공유했습니다. 이번 글에서는 렌더 단계가 어떻게 작동하는지를 공유하려고 합니다.

컴포넌트 인스턴스가 다시 생성된 후에는 React 엘리먼트를 업데이트하여 Virtual DOM을 만듭니다.

![How React Works - The Render Phase Part 2 - Image 1](/assets/img/2024-07-09-HowReactWorksTheRenderPhasePart2_1.png)

<div class="content-ad"></div>

가상 DOM

첫 번째 렌더링 시 React는 전체 컴포넌트 트리를 가져와서 큰 React 요소 트리로 전환합니다.

따라서 가상 DOM은 컴포넌트 트리의 모든 인스턴스에서 생성된 모든 React 요소로 구성된 트리입니다. (그냥 자바스크립트 객체일 뿐입니다)

![이미지](/assets/img/2024-07-09-HowReactWorksTheRenderPhasePart2_2.png)

<div class="content-ad"></div>

상태 업데이트나 다시 렌더링이 발생하면 해당 요소의 모든 하위 컴포넌트가 렌더링되어 가상 DOM이 다시 만들어집니다. (프롭이 변경되었는지 여부와 관계없이)

이 작업은 React가 하위 요소가 영향을 받을지 여부를 알 수 없기 때문에 필요하며 실제 DOM이 아닌 가상 DOM만 다시 생성됩니다.

![이미지](/assets/img/2024-07-09-HowReactWorksTheRenderPhasePart2_3.png)

이제 가상 DOM이 다시 생성된 후 새로운 가상 DOM이 현재 Fiber 트리와 조화 및 차이 분석 단계로 이동합니다.

<div class="content-ad"></div>

이 작업은 피버(Fiber)이라고 불리는 React 조정자(Reconciler)에서 수행됩니다.

이 조정 과정의 결과물은 결국 DOM에 쓰여질 업데이트된 피버(Fiber) 트리가 될 것입니다.

![React 작동 방식: 렌더 단계 파트 2](/assets/img/2024-07-09-HowReactWorksTheRenderPhasePart2_4.png)

조정이란 무엇이며 왜 필요한가요?

<div class="content-ad"></div>

가상 DOM을 실제 DOM으로 변환하는 일은 재렌더링이 발생할 때 비용이 많이 들고 빠르지 않습니다. 왜냐하면 앞서 언급했듯이 모든 하위 요소가 다시 렌더링되기 때문입니다.

참고사항

렌더링이 트리거될 때 React는 가능한 한 기존 DOM을 최대한 활용합니다.

조정(reconciliation)

<div class="content-ad"></div>

최신 변경 사항을 반영하기 위해 실제로 삽입, 삭제 또는 업데이트해야 하는 DOM 요소를 결정하는 것은 Reconciler가 수행합니다.

Reconciler: Fiber — 작동 방식

React 애플리케이션을 초기 렌더링하는 동안 Fiber는 전체 리액트 엘리먼트 트리(가상 DOM)를 가져와 Fiber 트리로 변환합니다.

![이미지](/assets/img/2024-07-09-HowReactWorksTheRenderPhasePart2_5.png)

<div class="content-ad"></div>

Fiber Tree

각 구성 요소 인스턴스와 DOM 요소에 대한 'fiber'를 가지고 있는 내부 트리입니다. 가상 DOM과 달리, 매 렌더링마다 다시 생성되지 않습니다.

Fibers는 주로 작업의 단위로 언급됩니다.

Fibers의 독특한 특성 중 하나는 작업을 비동기적으로 수행할 수 있다는 것입니다. 이는 렌더링 프로세스를 청크로 분할할 수 있음을 의미하며, 작업을 우선 순위별로 나눠 실행하거나 일시 중지, 재사용 또는 폐기할 수 있습니다.

<div class="content-ad"></div>

동시에 suspense 및 transition과 같은 기능을 활성화합니다.

실제 조정 작업

조정이 진행되는 경우 fiber는 변경된 부분과 변경되지 않은 부분을 확인하기 위해 전체 트리를 통과합니다.

![image](/assets/img/2024-07-09-HowReactWorksTheRenderPhasePart2_6.png)

<div class="content-ad"></div>

전체 피버 트리를 다시 만들지 않고 조정 프로세스는 영향을 받는 리액트 요소만 다시 만들고 나머지는 그대로 유지됩니다.

트리 내 위치에 따라 요소를 단계별로 비교하는 프로세스를 Diffing이라고 합니다.

다음 글에서는 Diffing 및 커밋 단계에 대해 알아볼 예정입니다.

즐거운 학습 되세요!

<div class="content-ad"></div>

렌더링 단계에 대해 더 알아보세요.
