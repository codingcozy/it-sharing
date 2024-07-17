---
title: "Inside React 서버 컴포넌트 이해하기"
description: ""
coverImage: "/assets/img/2024-07-09-InsideReactServercomponents_0.png"
date: 2024-07-09 13:40
ogImage:
  url: /assets/img/2024-07-09-InsideReactServercomponents_0.png
tag: Tech
originalTitle: "Inside React: Server components"
link: "https://medium.com/gitconnected/inside-react-server-components-670b63b755ad"
---

![이미지](/assets/img/2024-07-09-InsideReactServercomponents_0.png)

Inside React 시리즈의 마지막 블로그입니다. 이전 글은 여기에서 확인하실 수 있습니다. React Server Components (RSC)는 개발자가 최신 웹 애플리케이션을 구축하면서 성능을 향상하고 사용자 경험을 개선할 수 있는 강력한 기능입니다. 이 안내서는 React Server Components의 기본 사항을 안내하며, 그 작동 방법을 설명하는 예제를 포함하고 있습니다.

# 서버 컴포넌트가 필요한 이유

React Server Components가 필요한 이유를 이해하려면 웹 개발의 진화를 되돌아보고 다양한 도전 과제를 어떻게 해결해 왔는지 살펴보는 것이 도움이 됩니다.

<div class="content-ad"></div>

## React 이전의 세계

React 이전에는 PHP와 같은 언어를 사용하여 웹 개발을 진행했습니다. 이러한 언어들은 클라이언트와 서버를 강하게 결합했습니다. 모노리식 아키텍처에서는 개발자가 데이터를 가져오고 페이지를 직접 서버에서 렌더링할 수 있었습니다. 이 방법은 몇 가지 이점이 있었지만, 단순성과 데이터 직접 가져오기 등이 포함되어 있었지만, 다음과 같은 주요 단점도 있었습니다:

- 확장의 어려움: 모노리식 응용 프로그램은 팀 간 의존성과 높은 트래픽 요구로 인해 확장하기 어려웠습니다.
- 유지 보수 도전: 응용 프로그램이 커질수록 모노리식 코드베이스를 유지하는 것이 점점 어려워졌습니다.

## React의 등장

<div class="content-ad"></div>

이러한 도전에 대응하기 위해 React가 컴포넌트 기반 아키텍처를 도입하여 만들어졌습니다. 이를 통해 개발자는 재사용 가능한 UI 컴포넌트를 구축할 수 있었고 기존 코드베이스에 점진적으로 적용할 수 있도록 했습니다. 이는 팀들에게 중요한 역할을 했으며 협업과 조립이 용이해졌습니다.

React는 클라이언트와 서버의 문제를 분리하여 프론트엔드를 더 유연하게 만들었습니다. 이 변화는 웹 애플리케이션에서 성장하는 풍부한 상호작용 수요에 대응하는 데 중요했습니다.

## 렌더링 기술의 진화

지난 10년 동안 웹 개발은 렌더링 기술에서 상당한 발전을 이루었습니다. 다중 페이지 애플리케이션(MPAs)에서 단일 페이지 애플리케이션(SPAs)으로, 그리고 클라이언트 사이드 렌더링(CSR)에서 서버 사이드 렌더링(SSR)으로 발전해 왔습니다. 항상 빠른 데이터 제공, 풍부한 상호작용 제공, 우수한 개발자 경험 유지를 목표로 하였습니다.

<div class="content-ad"></div>

# 서버 사이드 렌더링과 리액트 서스펜스가 해결한 문제는 무엇인가요?

React 서버 컴포넌트의 필요성을 이해하기 위해서는 먼저 서버 사이드 렌더링(SSR)과 리액트 서스펜스가 해결하려고 했던 문제를 이해하는 것이 중요합니다.

## 서버 사이드 렌더링 (SSR)

SSR은 사전 렌더링된 HTML을 클라이언트로 전송하여 초기 페이지로드에 중점을 둡니다. 그런 다음 해당 HTML은 일반적인 React 앱처럼 동작하기 위해 JavaScript로 활성화되어야 합니다. SSR은 초기로드 시간을 개선하지만 제한 사항도 있습니다:

<div class="content-ad"></div>

- 모든 것 또는 아무것도 없는 워터폴: 모든 데이터는 표시되기 전에 서버에서 가져와야 합니다. 모든 JavaScript는 클라이언트가 수분을 받기 전에 다운로드되어야 하며, 모든 수분화가 완료되어야 상호 작용이 가능합니다.
- 일회성 렌더링: SSR은 페이지로 직접 이동할 때에만 한 번 발생합니다.

## React Suspense

React Suspense을 사용하면 서버 측 HTML 스트리밍과 클라이언트 측 선택적 수분화를 가능하게 합니다. `Suspense`로 컴포넌트를 감싸면 해당 컴포넌트의 렌더링 및 수분화를 우선순위가 더 낮도록 설정하여 다른 컴포넌트가 더 무거운 것으로 인해 차단되지 않고 로드되도록 할 수 있습니다.

Suspense은 상황을 크게 개선했지만 여전히 몇 가지 문제가 남아 있습니다:

<div class="content-ad"></div>

- 데이터 가져오기: 페이지 전체의 데이터는 모든 구성 요소가 표시되기 전에 서버에서 가져와야 합니다. useEffect() 훅에서 클라이언트 측에서 데이터를 가져오면 더 긴 왕복 시간이 소요되며 구성 요소가 렌더링되고 제대로 작동한 후에만 실행됩니다.
- JavaScript 다운로드: 모든 페이지 JavaScript는 비동기적으로 스트리밍되더라도 결국 다운로드됩니다.
- 클라이언트 측 수행: 클라이언트 측 JavaScript가 다운로드되고 구현되기 전까지 사용자는 구성 요소와 상호 작용할 수 없습니다.
- 계산 가중치: 대부분의 JavaScript 계산 가중치는 여전히 클라이언트에서 처리되며 이는 다양한 기기에서 실행될 수 있습니다. 이를 서버로 옮기면 성능을 향상시킬 수 있습니다.

# React 서버 컴포넌트란 무엇인가요?

RSCs는 서버에서 "실행"되는 새로운 유형의 컴포넌트를 소개하며 클라이언트 측 JavaScript 번들에서 제외됩니다. 이러한 컴포넌트는 빌드 시간에 실행될 수 있어 파일 시스템에서 읽거나 정적 콘텐츠를 가져오거나 데이터 레이어에 액세스할 수 있습니다. 서버 컴포넌트에서 데이터를 props로 전달하여 브라우저의 대화형 클라이언트 컴포넌트로 유지하면 RSC는 매우 효율적이고 성능이 우수한 애플리케이션을 유지합니다.

# 왜 React 서버 컴포넌트를 사용해야 하나요?

<div class="content-ad"></div>

- 향상된 성능: 서버에 렌더링을 offloading하여 클라이언트가 콘텐츠를 더 빨리 받아들이고 표시할 수 있습니다.
- 더 나은 사용자 경험: 더 빠른로드 타임과 줄어든 JavaScript 페이로드로 원활한 상호작용 및 전반적으로 더 나은 사용자 경험을 제공합니다.
- SEO 이점: 서버사이드 렌더링은 검색 엔진이 완전히 렌더링된 콘텐츠를 크롤링하고 인덱싱할 수 있기 때문에 SEO가 향상됩니다.

# React 서버 구성요소는 어떻게 작동하나요?

React 서버 구성요소는 서버에서 React 컴포넌트를 렌더링한 다음 HTML을 클라이언트로 스트리밍하는 방식으로 작동합니다. 이는 클라이언트가 이미 렌더링된 HTML을받아들이므로 클라이언트 측 JavaScript가 초기 뷰를 빌드하는 필요성이 줄어듭니다.

## React 서버 구성요소 설정하기

<div class="content-ad"></div>

react 컴포넌트를 express 서버에서 렌더링해보려고 합니다.

```js
//React 컴포넌트
import React from "react";

function App() {
  return (
    <div>
      <h1>Hello, React Server Components!</h1>
      <p>이것은 서버에서 렌더링됩니다.</p>
      <p>당신은 멋져요!</p>
    </div>
  );
}

export default App;
```

```js
const express = require("express");
const { renderToPipeableStream } = require("react-dom/server");
const React = require("react");
const App = require("./src/App");

const app = express();

app.get("/", (req, res) => {
  const stream = renderToPipeableStream(React.createElement(App), {
    onShellReady() {
      res.statusCode = 200;
      res.setHeader("Content-type", "text/html");
      stream.pipe(res);
    },
    onError(error) {
      console.error(error);
      res.statusCode = 500;
      res.send("내부 서버 오류");
    },
  });
});

app.listen(3000, () => {
  console.log("서버가 3000포트에서 들어오는 요청을 수신 중입니다.");
});
```

React의 renderToPipeableStream 함수는 React 18+ Server Components 기능 세트의 일부입니다. 이 함수를 사용하면 서버 측에서 React 컴포넌트를 더 효율적이고 반응적으로 렌더링할 수 있습니다. 이 함수를 사용하면 HTML을 생성하여 클라이언트로 스트리밍하므로 완전히 렌더링된 HTML 문서를 한 번에 보내는 것보다 더 효과적입니다.

<div class="content-ad"></div>

# renderToPipeableStream 작동 방식

renderToPipeableStream 함수는 React 컴포넌트 트리를 HTML 문자열로 렌더링하는 스트림을 생성합니다. 이 HTML 문자열은 그 후 HTTP 응답과 같은 쓰기 가능한 스트림으로 파이핑됩니다. 이 함수는 두 가지 주요 인수를 사용합니다: 렌더링할 React 컴포넌트 트리 및 라이프사이클 훅이 있는 옵션 객체입니다.

다음은 해당 매개변수와 사용법에 대한 자세한 설명입니다:

## 매개변수

<div class="content-ad"></div>

- 리액트 컴포넌트 트리: 첫 번째 인자는 렌더링하고자 하는 리액트 컴포넌트 트리입니다. 일반적으로 React.createElement를 사용하여 생성됩니다.
- 옵션 객체: 두 번째 인자는 여러 라이프사이클 훅과 구성 옵션을 포함하는 객체입니다:

  - onShellReady: 초기 셸의 HTML(비동기 콘텐츠 없이)이 클라이언트로 전송 준비를 마치면 호출되는 콜백 함수입니다.
  - onShellError: 셸을 렌더링하는 동안 오류가 발생하면 호출되는 콜백 함수입니다.
  - onAllReady: 모든 콘텐츠(비동기 콘텐츠 포함)가 준비되었을 때 호출되는 콜백 함수입니다.
  - onError: 렌더링 중 발생하는 각각의 오류에 대해 호출되는 콜백 함수입니다.

# 서버에서 데이터 가져오기

리액트 서버 컴포넌트의 주요 이점 중 하나는 클라이언트로 렌더링된 HTML을 전송하기 전에 서버에서 데이터를 가져올 수 있다는 것입니다. 예제에 API에서 데이터를 가져와서 표시하는 방법을 확장해 보겠습니다.

<div class="content-ad"></div>

```js
const express = require("express");
const { renderToPipeableStream } = require("react-dom/server");
const React = require("react");
const axios = require("axios");
const App = require("./src/App");

const app = express();

app.get("/", async (req, res) => {
  try {
    const response = await axios.get("https://jsonplaceholder.typicode.com/posts/1");
    const data = response.data;

    const stream = renderToPipeableStream(React.createElement(App, { data }), {
      onShellReady() {
        res.statusCode = 200;
        res.setHeader("Content-type", "text/html");
        stream.pipe(res);
      },
      onError(error) {
        console.error("Shell error:", error);
        res.statusCode = 500;
        res.send("Internal Server Error");
      },
    });
  } catch (error) {
    console.error(error);
    res.statusCode = 500;
    res.send("Internal Server Error");
  }
});

app.listen(3000, () => {
  console.log("서버가 포트 3000에서 실행 중입니다.");
});
```

## 결론

React Server Components는 현대적인 웹 응용 프로그램을 구축하는 데 혁신을 가져옵니다. 서버에서 컴포넌트를 렌더링하고 결과를 클라이언트에게 전송함으로써 더 나은 성능, 향상된 사용자 경험 및 향상된 SEO를 달성할 수 있습니다. 이 안내서는 React Server Components 설정 기본 사항을 다루고 서버에서 데이터 가져오는 간단한 예제를 제공했습니다. 이 기반을 통해 직접 프로젝트에서 React Server Components의 강점을 탐구하고 활용할 준비가 되었습니다. 즐거운 코딩하세요!

낮은 수준의 기술 세부 정보를 읽고 쓰며 기술 탐구에 대해 피드백이나 문의 사항이 있으면 언제든지 저에게 연락해주세요!

<div class="content-ad"></div>

[https://www.linkedin.com/in/itherohit/](https://www.linkedin.com/in/itherohit/)

[https://itherohit.dev/](https://itherohit.dev/)
