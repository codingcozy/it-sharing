---
title: "React로 스크롤-투-탑 기능 구현하는 방법"
description: ""
coverImage: "/it-shar.ing/assets/no-image.jpg"
date: 2024-07-09 08:32
ogImage:
  url: /it-shar.ing/assets/no-image.jpg
tag: Tech
originalTitle: "Implementing a Scroll-to-Top Feature with React"
link: "https://medium.com/@aptia.rahgozar/implementing-a-scroll-to-top-feature-with-react-d6994913c597"
---

React 애플리케이션에서 다른 페이지나 구성 요소 간에 이동할 때 사용자는 부드러운 전환을 기대합니다. 그러나 한 가지 흔한 문제가 발생합니다: 새로운 구성 요소가 렌더링될 때 화면이 자동으로 맨 위로 스크롤되지 않습니다. 이 기사에서는 이 UX 도전 과제를 해결하기 위해 전용 ScrollToTop 컴포넌트를 생성하는 방법을 탐색하겠습니다. 이 단계를 따라가면 사용자 경험을 향상시키고 앱이 경로 간에 매끄럽게 작동하도록 보장할 수 있습니다. 함께 알아보죠!

src 폴더 안에 utils라는 새 폴더를 만들어 ScrollToTop.js 파일을 그 안에 생성하는 것을 선호합니다. ScrollToTop 파일 내에서 새로운 React 화살표 함수 컴포넌트를 생성하여 시작합니다. 그 다음으로는 useEffect 훅을 React의 컴포넌트 맨 위에서 가져옵니다. useEffect 훅을 사용하여 화면을 맨 위로 스크롤하도록 설정하겠습니다. 컴포넌트가 렌더링될 때나 경로 이름이 업데이트될 때마다 화면을 맨 위로 스크롤합니다. 또한 react-router-dom에서 useLocation 훅을 가져옵니다. 현재 경로 이름에 액세스할 수 있도록 해줍니다. 경로 이름은 도메인 이름(또는 호스트) 뒤에 오는 URL의 일부를 나타냅니다. 선택적 포트 번호도 포함될 수 있습니다. 이는 웹사이트나 애플리케이션 내의 특정 위치나 경로를 나타냅니다. 예를 들어 다음 URL에서: https://example.com/products/electronics/laptops, 도메인은 example.com이고 경로 이름은 /products/electronics/laptops입니다. React 애플리케이션에서는 경로 이름을 다루는 데 중요합니다. 사용자가 앱 내에서 다른 페이지나 경로를 방문할 때마다 경로 이름이 그에 따라 변경됩니다.

지금까지 우리의 ScrollToTop 컴포넌트는 이렇게 보일 것입니다:

```js
import { useEffect } from "react";
import { useLocation } from "react-router-dom";

export const ScrollToTop = () => {
  return <div>ScrollToTop</div>;
};
```

<div class="content-ad"></div>

다음 단계는 useEffect를 설정하는 것입니다. 컴포넌트 내에서 useEffect 훅을 사용하여 pathname을 종속성으로 지정합니다. 이렇게 하면 경로명이 변경될 때마다 효과가 실행되도록 보장됩니다. 다시 말하면 페이지 간 이동할 때 이동합니다. 또한 pathname에 액세스하려면 useLocation 훅을 사용합니다.

다음은 최종 ScrollToTop 컴포넌트의 모습입니다:

```js
import { useEffect } from "react";
import { useLocation } from "react-router-dom";

export const ScrollToTop = () => {
  const { pathname } = useLocation();

  useEffect(() => {
    window.scrollTo(0, 0);
  }, [pathname]);

  return null;
};
```

위 코드에서 보듯이, window는 브라우저 창을 나타내는 전역 객체를 가리킵니다. 이 코드 라인인 window.scrollTo(0, 0);은 창이나 뷰포트를 맨 위로 스크롤하는 데 책임이 있습니다. scrollTo 메서드는 두 인수를 취하는데, 수평 위치(픽셀)와 수직 위치(픽셀)입니다. (0, 0)을 전달하여 브라우저에 페이지의 좌측 상단으로 스크롤하도록 지시합니다. 새 컴포넌트가 렌더링될 때(경로 변화로 인해), 사용자가 페이지 상단을 볼 수 있도록 보장합니다.

<div class="content-ad"></div>

React에서 의문이 드실 수 있는 또 다른 부분은 컴포넌트 끝에 null을 반환하는 것입니다. React에서는 모든 컴포넌트가 무언가를 반환해야 합니다(일반적으로 JSX). ScrollToTop 컴포넌트는 실제로 보이는 콘텐츠를 렌더링하지 않고(스크롤 동작만 다룸) 간단히 null을 반환할 수 있습니다. null을 반환함으로써 이 컴포넌트가 시각적 UI에 기여하지 않지만 의도한 기능을 숨겨서 수행한다는 것을 나타낼 수 있습니다.

마지막으로 ScrollToTop 컴포넌트를 index.js 파일( src 폴더 안에 있음)에서 import 합니다. App 컴포넌트를 렌더링하기 전에 꼭 호출하는 것을 잊지 않습니다. 이렇게 함으로써 모든 경로에서 스크롤 최상단 동작이 원활하게 작동하도록 보장합니다:

```js
import React from "react";
import ReactDOM from "react-dom";
import { BrowserRouter as Router } from "react-router-dom";

import "./index.css";

import { ScrollToTop } from "./components";
import App from "./App";

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(
  <React.StrictMode>
    <Router>
      <ScrollToTop />
      <App />
    </Router>
  </React.StrictMode>
);
```

본 문서에서는 React 애플리케이션에서 스크롤 최상단 기능을 만드는 방법을 탐구해보았습니다. ScrollToTop 컴포넌트를 구현함으로써 사용자들이 페이지 간에 맥락을 유지하면서 원활히 이동할 수 있도록 만들었습니다. 이와 같은 작은 개선은 사용자 만족도와 참여도에 상당한 영향을 미칠 수 있습니다. React 프로젝트를 계속해서 개발하는 동안 우리는 대상 청중을 위한 즐거운 경험을 만들기 위해 비슷한 UX 개선을 고려해야 합니다. 유용하게 활용하셨길 바랍니다. 읽어 주셔서 감사합니다.
