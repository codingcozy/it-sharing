---
title: "Nextjs에서의 Hydration 및 Hydration 오류를 완벽히 이해하는 방법 완벽 가이드"
description: ""
coverImage: "/assets/img/2024-07-09-TheUltimateGuidetoHydrationandHydrationErrorsinNextjs_0.png"
date: 2024-07-09 13:34
ogImage:
  url: /assets/img/2024-07-09-TheUltimateGuidetoHydrationandHydrationErrorsinNextjs_0.png
tag: Tech
originalTitle: "The Ultimate Guide to Hydration and Hydration Errors in Next.js"
link: "https://medium.com/@skarka90/the-ultimate-guide-to-hydration-and-hydration-errors-in-next-js-ae9b4bc74ee2"
---

![Next.js](/assets/img/2024-07-09-TheUltimateGuidetoHydrationandHydrationErrorsinNextjs_0.png)

Next.js는 서버사이드 렌더링(SSR), 정적 사이트 생성(SSG), 자동 코드 분할과 같은 강력한 기능을 제공하여 React 애플리케이션을 구축하는 방식을 혁신했습니다. Next.js의 렌더링 전략의 핵심에는 중요한 개념인 수분 보충(hydration)이 있습니다. Next.js를 사용하는 모든 개발자에게 수분 보충과 관련된 오류 처리에 대한 이해는 필수적입니다. 이 포괄적인 안내서는 수분 보충의 세계에 심취하여 강력하고 성능이 우수한 Next.js 애플리케이션을 구축하는 데 필요한 모든 지식을 제공할 것입니다.

수분 보충은 서버에서 생성된 정적 HTML을 가져와 이벤트 리스너 및 상태를 연결하여 클라이언트 측에서 상호 작용 가능하게 만드는 과정입니다.

마트에 있는 가게를 상상해보세요. 먹을 준비가 된 라면 포장을 볼 수 있습니다 – 모든 재료는 갖추어져 있지만 아직은 먹을 수 없는 상태입니다. 라면을 먹을 수 있도록 하려면 뜨거운 물을 추가해야 합니다. 이 과정은 Next.js에서의 수분 보충이 하는 역할과 유사합니다.

<div class="content-ad"></div>

기술 용어로 말씀드리면:

- 서버는 정적 HTML을 생성합니다.
- 이 HTML은 클라이언트(브라우저)로 전송되어 초기 렌더링이 빠르게 이루어집니다.
- 그런 다음 브라우저에 JavaScript 번들이 로드됩니다.
- Next.js는 해당 정적 HTML에 이벤트 핸들러를 첨부하고 서버 렌더링된 HTML을 클라이언트 측 React 컴포넌트 트리와 조화시키는 작업을 합니다.

- 서버 측 렌더링: 사용자가 페이지를 요청하면 Next.js가 서버에서 초기 HTML을 생성합니다. 이는 React 컴포넌트의 기본 구조와 내용을 포함합니다.
- 초기 HTML 전달: 이 사전 렌더링된 HTML이 브라우저로 전송됩니다. 이 시점에서 사용자는 내용을 볼 수 있지만 상호 작용은 불가능합니다.
- JavaScript 로딩: 그런 다음 Next.js가 브라우저로 필요한 JavaScript 파일을 전송합니다. 이에는 React 컴포넌트와 Next.js 실행 시스템이 포함됩니다.
- React 부트스트래핑: JavaScript 파일이 로드되면 React가 브라우저에서 시작됩니다.
- 조화작업: React는 서버 HTML에서 생성된 DOM과 클라이언트에서 컴포넌트를 렌더링하여 생성한 가상 DOM을 비교합니다. 이 단계는 클라이언트 측 React 트리가 서버 렌더링된 HTML과 일치하도록 보장합니다.
- 이벤트 핸들러 첨부: React는 DOM 요소에 필요한 모든 이벤트 핸들러를 첨부합니다. 이것이 페이지를 상호 작용할 수 있게 하는 요소입니다.

간단한 코드 예제를 통해 설명해 드리겠습니다:

<div class="content-ad"></div>

```js
// 이 컴포넌트는 서버와 클라이언트 양쪽에서 렌더링됩니다
function Button() {
  const [count, setCount] = React.useState(0);

  return <button onClick={() => setCount(count + 1)}>클릭 횟수: {count}번</button>;
}

// 서버에서는 이를 정적 HTML로 렌더링합니다
// <button>클릭 횟수: 0번</button>

// 클라이언트에서 수행된 후, 상호작용이 가능해집니다
// onClick 핸들러가 추가되고, 상태 업데이트가 작동합니다
```

수분화는 몇 가지 이유로 중요합니다:

- 성능: 서버 측 렌더링 덕분에 빠른 초기 페이지 로드를 제공하는 동시에 React 앱의 모든 상호작용 기능을 제공합니다.
- SEO: 검색 엔진이 초기 HTML을 쉽게 크롤링할 수 있어 사이트의 검색 엔진 최적화가 향상됩니다.
- 사용자 경험: 사용자가 콘텐츠를 빠르게 보고 상호작용을 얻을 수 있어 양쪽 모두의 장점을 제공합니다.
- 점진적 향상: JavaScript 로드나 실행에 실패해도 기본 콘텐츠를 사용할 수 있는 점진적 향상을 지원합니다.

수분화 오류는 서버에서 생성된 HTML과 조정 과정 중 클라이언트에서 렌더링할 것을 React가 예상하는 것 간에 불일치가 발생할 때 발생합니다.

<div class="content-ad"></div>

리액트가 서버에서 렌더링된 콘텐츠를 효화할 때, 클라이언트 측 렌더링이 정확히 동일한 HTML 구조를 생성할 것을 예상합니다. 어떤 차이가 있다면, 리액트는 효화 오류를 발생시킵니다.

이러한 오류는 일반적으로 다음과 같은 메시지가 포함된 콘솔에서 나타납니다:

```js
경고: 텍스트 콘텐츠가 일치하지 않습니다. 서버: "안녕하세요," 클라이언트: "안녕하세요, 제임스"
```

<div class="content-ad"></div>

```js
경고: 서버 HTML에 <body> 안에 일치하는 <div>를 포함시키기를 기대했습니다.

- 일관되지 않은 렌더링: 컴포넌트가 서버와 클라이언트에서 다르게 렌더링될 때 발생합니다.

function InconsistentComponent() {
  // 이 함수는 서버와 클라이언트에서 다른 콘텐츠를 생성할 것입니다
  const randomNumber = Math.random();
  return <div>{randomNumber}</div>;
}

2. 브라우저 전용 API: 서버 사이드 렌더링 중에만 브라우저에서만 존재하는 API를 사용합니다.
```

<div class="content-ad"></div>

```js
function BrowserOnlyComponent() {
  // 서버에서 오류를 발생시킵니다.
  const windowWidth = window.innerWidth;
  return <div>창 너비: {windowWidth}</div>;
}
```

3. 시간에 따른 렌더링: 현재 시간에 따라 렌더링되는 컴포넌트는 문제를 일으킬 수 있습니다.

```js
function TimeComponent() {
  // 서버와 클라이언트 간에 다를 수 있습니다.
  const currentHour = new Date().getHours();
  return <div>현재 시간: {currentHour}</div>;
}
```

4. 환경별 코드: 환경에 따라 다르게 동작하는 코드입니다.

<div class="content-ad"></div>

```javascript
function EnvironmentSpecificComponent() {
  const isServer = typeof window === "undefined";
  return <div>{isServer ? "서버" : "클라이언트"}</div>;
}
```

- 클라이언트 측 코드에 useEffect 사용:

```javascript
import { useState, useEffect } from "react";

function SafeComponent() {
  const [windowWidth, setWindowWidth] = useState(0);

  useEffect(() => {
    setWindowWidth(window.innerWidth);
  }, []);

  return <div>창 너비: {windowWidth}</div>;
}
```

2. 브라우저 환경 확인하기:

<div class="content-ad"></div>

```js
function SafeComponent() {
  const [isBrowser, setIsBrowser] = useState(false);

  useEffect(() => {
    setIsBrowser(true);
  }, []);

  return <div>{isBrowser ? "브라우저에서" : "서버에서"}</div>;
}
```

3. 초기 렌더링에 안정적인 값 사용하기:

```js
function TimeComponent() {
  const [currentHour, setCurrentHour] = useState(0);

  useEffect(() => {
    setCurrentHour(new Date().getHours());
  }, []);

  return <div>현재 시간: {currentHour}</div>;
}
```

- Components를 순수하게 유지하기: 환경에 관계없이 주어진 입력에 대해 동일한 출력을 생성하도록 컴포넌트를 보장합니다.
- 직접 DOM 조작을 피하기: React에 DOM 업데이트를 처리하게하여 서버와 클라이언트 렌더 사이의 불일치를 방지합니다.
- 써드파티 라이브러리 사용에 주의: 사용하는 라이브러리가 SSR을 지원하는지 확인하거나 클라이언트 측에서만 사용합니다.
- Next.js의 Image Component 사용하기: 이미지에는 많은 예외 사례 및 최적화를 자동으로 처리하는 next/image 컴포넌트를 사용합니다.
- 적절한 로딩 상태 구현: 즉시 사용할 수 없는 데이터에 대해 렌더 불일치를 방지하려면 로딩 상태를 사용합니다.

<div class="content-ad"></div>

Next.js에서 수분은 정적 HTML을 브라우저에서 활성화하는 중요한 프로세스입니다. 이를 기억해야 할 사항은 다음과 같습니다:

- 수분화는 서버 렌더링된 페이지를 인터랙티브하게 만듭니다.
- 서버 및 클라이언트 렌더링이 일치하지 않을 때 오류를 발생시킬 수 있습니다.
- 일반적인 원인으로는 브라우저 전용 API를 너무 일찍 사용하거나 데이터 불일치가 있습니다.
- 오류를 피하려면 렌더를 일관되게 유지하십시오.

수분화를 이해함으로써 더 빠르고 신뢰할 수 있는 Next.js 애플리케이션을 구축할 수 있습니다. 서버 및 클라이언트 렌더를 일관되게 유지하면 대부분의 수분화 관련 문제를 피할 수 있습니다. 즐거운 코딩하세요!
