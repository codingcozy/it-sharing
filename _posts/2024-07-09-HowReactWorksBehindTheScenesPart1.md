---
title: "리액트의 내부 동작 원리 파트 1"
description: ""
coverImage: "/assets/img/2024-07-09-HowReactWorksBehindTheScenesPart1_0.png"
date: 2024-07-09 13:33
ogImage:
  url: /assets/img/2024-07-09-HowReactWorksBehindTheScenesPart1_0.png
tag: Tech
originalTitle: "How React Works Behind The Scenes: Part 1"
link: "https://medium.com/@mastermusili/how-react-works-behind-the-scenes-part-1-450d9e9ff9f2"
---

![How React Works Behind The Scenes Part 1](/assets/img/2024-07-09-HowReactWorksBehindTheScenesPart1_0.png)

Libraries vs Framework

Framework

Is a complete structure that includes everything you need to build a complete large scale application. Example: Angular, Vue

<div class="content-ad"></div>

다음은 프로젝트를 구성하는 데 필요한 모든 요소를 포함하고 있습니다.

HTTP 요청, 스타일 지정, 라우팅 등을 포함한 모든 것이 제공됩니다.

에러를 줄일 수 있는 통합된 시스템과 함께 안심하세요.

<div class="content-ad"></div>

프레임워크 도구와 규칙에 갇혀 계신 것 같네요.

라이브러리

대규모 애플리케이션을 개발할 때 다른 개발자들이 공유하는 코드 조각들입니다.

예시: React

<div class="content-ad"></div>

대규모 애플리케이션을 개발할 때는 라우팅, 스타일링, 폼 관리 등 외부 라이브러리를 포함해야 합니다.

자유로움:

제3자 라이브러리의 선택을 원하는 대로 자유롭게 할 수 있습니다.

결정 피로:

<div class="content-ad"></div>

여러 외부 라이브러리에 대해 조사하고 다운로드하여 배우고 업데이트된 상태를 유지해야 해요.

일부 React 3rd party 생태계

라우팅 - React Router, React Location

HTTP 요청 - fetch(), axios

<div class="content-ad"></div>

글로벌 상태 관리 — context api, redux

스타일링 — css 모듈, tailwind css

폼 관리 — formik, react hook form

React 컴포넌트, 인스턴스 및 엘리먼트

<div class="content-ad"></div>

**구성 요소**

사용자 인터페이스 조각을 서술하는 데 사용되는 요소입니다. 리액트 요소를 반환하며 자바스크립트 함수입니다.

**구성 요소 인스턴스**

구성 요소를 '사용'할 때 생성되며, 구성 요소의 물리적 표현입니다.

<div class="content-ad"></div>

테이블 태그를 마크다운 형식으로 변경하세요.

<div class="content-ad"></div>

브라우저에서 구성 요소 인스턴스의 실제 시각적 표현

참고

React 구성 요소를 직접 호출하는 대신 JSX로 렌더링하는 이유는 무엇인가요?

React가 그것을 구성 요소로 인식하지 않기 때문에 훅 규칙을 위반하게 됩니다

<div class="content-ad"></div>

React 렌더링 작동 방식

![React Rendering](/assets/img/2024-07-09-HowReactWorksBehindTheScenesPart1_1.png)

`render()`은 컴포넌트의 상태가 업데이트될 때 트리거됩니다.

화면에 컴포넌트가 표시되는 방식

<div class="content-ad"></div>

아래와 같이 Markdown 형식으로 테이블 태그를 변경해주세요.

<img src="/assets/img/2024-07-09-HowReactWorksBehindTheScenesPart1_2.png" />

Render is Triggered

State changes trigger render, state is updated

Triggered in two ways:

<div class="content-ad"></div>

- 리액트 애플리케이션이 로드될 때 - 초기 렌더
- 상태가 업데이트되거나 변경될 때

렌더 프로세스는 전체 트리를 살펴 애플리케이션 전체에 대해 트리거됩니다

렌더가 즉시 트리거되지는 않지만 자바스크립트 엔진에 여유 시간이 있을 때 예약됩니다

렌더 단계

<div class="content-ad"></div>

리액트는 컴포넌트 함수를 호출하고 DOM을 업데이트하는 방법을 파악합니다.

커밋 단계

리액트는 실제로 DOM을 작성하고 업데이트를 삽입하며 요소를 삭제한 다음, 브라우저가 화면을 다시 그립니다.

다음 글에서는 렌더 단계에 대해 알려드릴 예정입니다.

<div class="content-ad"></div>

행복한 학습되세요!
