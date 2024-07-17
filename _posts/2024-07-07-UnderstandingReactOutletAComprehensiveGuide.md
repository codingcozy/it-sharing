---
title: "리액트 아울렛React Outlet 완전 정복 가이드"
description: ""
coverImage: "/assets/img/2024-07-07-UnderstandingReactOutletAComprehensiveGuide_0.png"
date: 2024-07-07 21:01
ogImage:
  url: /assets/img/2024-07-07-UnderstandingReactOutletAComprehensiveGuide_0.png
tag: Tech
originalTitle: "Understanding React Outlet: A Comprehensive Guide"
link: "https://medium.com/@shruti.latthe/understanding-react-outlet-a-comprehensive-guide-b122b1e5e7ff"
---

![React Outlet](/assets/img/2024-07-07-UnderstandingReactOutletAComprehensiveGuide_0.png)

React는 사용자 인터페이스를 구축하기 위한 강력한 JavaScript 라이브러리입니다. 그 중요한 기능 중 하나인 React Router를 통해 개발자들은 여러 뷰를 가진 동적인 단일 페이지 애플리케이션을 만들 수 있습니다. 이 블로그 포스트에서는 React Router의 필수 구성 요소 인 React Outlet을 탐색하고 React 애플리케이션 내에서 중첩된 경로를 관리하는 역할을 이해해 보겠습니다.

React Outlet이란 무엇인가요?

React Outlet은 부모 경로 내에서 자식 경로를 위한 자리 표시자로 작동하는 React Router에서 제공하는 구성 요소입니다. 이를 통해 경로를 중첩하여 개발자는 응용 프로그램에서 복잡한 네비게이션 구조를 만들 수 있습니다.

<div class="content-ad"></div>

```js
import React from "react";

// 컴포넌트 가져오기
import Routes from "./routes";

// 컴포넌트
const App = () => {
  return (
    <>
      <Routes />
    </>
  );
};
export default App;
```

```js
// 패키지 가져오기

import { BrowserRouter, Outlet, Route, Routes } from "react-router-dom";
import React, { Suspense, lazy } from "react";
import { dashboard, homePath, loginPath, rootPath } from "./routePaths";

import Loader from "../components/loader";
import ProtectedRoute from "./protectedRoute";

// 라우트 경로 가져오기

const Login = lazy(() => import("../pages/login"));
const Home = lazy(() => import("../pages/home"));
const RouteNotFound = lazy(() => import("../pages/pageNotFound"));

const DashboardComponent = lazy(() => import("../pages/dashboard/"));

const AllRoutes = () => {
  return (
    <Suspense fallback={<Loader />}>
      <BrowserRouter>
        <Routes>
          <Route path={rootPath} element={<Outlet />}>
            <Route index element={<Login />} />
            <Route path={loginPath} element={<Login />} />
            <Route element={<ProtectedRoute />}>
              <Route path={homePath} element={<Home />} />
              <Route path={dashboard} element={<DashboardComponent />} />
            </Route>
            <Route path="*" element={<RouteNotFound />} />
          </Route>
        </Routes>
      </BrowserRouter>
    </Suspense>
  );
};

export default AllRoutes;
```

- App 컴포넌트:

- App 컴포넌트는 React 애플리케이션의 진입점입니다.
- 라우팅을 처리하는 Routes 컴포넌트를 렌더링합니다.

<div class="content-ad"></div>

2. AllRoutes 컴포넌트:

- AllRoutes 컴포넌트는 React Router를 사용하여 애플리케이션의 라우트를 설정합니다.
- 다른 경로에 대한 라우트를 정의하고 성능을 최적화하기 위해 Suspense 컴포넌트를 사용하여 컴포넌트를 지연로드합니다.
- 라우트 내에서 중첩된 라우트를 처리하기 위해 Outlet 컴포넌트를 활용합니다.

총 정리

- Imports: 우리는 react-router-dom에서 필요한 컴포넌트와 함수를 가져오고, 프로젝트에서 다른 컴포넌트와 경로를 가져옵니다.
- 레이지 로딩: 우리는 React의 lazy()를 사용하여 컴포넌트를 지연로드합니다. 이렇게 함으로써 번들을 작은 조각으로 분할하여 필요할 때 로드하여 초기 로드 시간을 개선합니다.
- Suspense: 우리는 'Suspense' 컴포넌트를 사용하여 지연로드된 라우트를 감쌉니다. 컴포넌트가 로드되는 동안 대체 UI를 지정할 수 있습니다.
- 라우팅: Routes 컴포넌트 내에서 Route 컴포넌트를 사용하여 다른 라우트를 정의합니다. 여기에 라우트에 대한 자세한 내용이 있습니다:

<div class="content-ad"></div>

- Root Route (/): 우리 애플리케이션의 루트 경로에 대한 Route입니다. Login 컴포넌트를 렌더링합니다.
- Login Route (/login): Login 컴포넌트를 렌더링합니다.

5. 보호된 라우트: 이 라우트들은 `ProtectedRoute` 컴포넌트 안에 포함되어 있습니다. 이는 아마도 인증 로직을 처리하여 특정 라우트로의 접근을 제한할 것입니다. 보호된 라우트 안에는 다음이 있습니다:

- Home Route (/home): Home 컴포넌트를 렌더링합니다.
- Dashboard Route (/dashboard): DashboardComponent 컴포넌트를 렌더링합니다.
- 와일드카드 라우트 (\*): 이 라우트는 이전 라우트에 일치하지 않는 모든 경로에 대응되며 RouteNotFound 컴포넌트를 렌더링합니다.

6. BrowserRouter: 이 컴포넌트는 애플리케이션에 라우팅 기능을 제공합니다. HTML5 history API를 사용하여 UI를 URL과 동기화시킵니다.

<div class="content-ad"></div>

```js
// 패키지 가져오기
import React, { useEffect } from "react";
import { useNavigate, Outlet } from "react-router-dom";

import BaseLayout from "../pages/layout";

// 경로 가져오기
import { rootPath } from "./routePaths";

// 컴포넌트
const ProtectedRoute = () => {
  const navigate = useNavigate();
  const isAuthenticated = false; // 동적 메소드 호출 추가
  const user = "shruti"; // 동적 메소드 호출 추가

  useEffect(() => {
    if (!isAuthenticated) {
      return navigate(rootPath, { replace: true });
    }
  }, [isAuthenticated, navigate]);

  return (
    <BaseLayout>
      <Outlet />
    </BaseLayout>
  );
};

export default ProtectedRoute;
```

ProtectedRoute 컴포넌트:

- ProtectedRoute 컴포넌트는 인증이 필요한 특정 경로를 보호하는 역할을 합니다.
- Outlet 컴포넌트를 활용하여 보호된 경로 내에서 중첩된 자식 경로를 렌더링합니다.

```js
import React, { lazy } from "react";

import ErrorBoundary from "../../components/error-boundary";

// const Header = lazy(() => import("@components/header"));
const Sidebar = lazy(() => import("../../components/sidebar"));
export default function BaseLayout(props) {
  const { children } = props;
  // 페이지마다 보여줘야 하는 일반적인 요소들을 포함한 컴포넌트인 헤더, 사이드바, 푸터
  return (
    <ErrorBoundary>
      <div className="wrapper">
        <div className="d-flex position-relative">
          <Sidebar />
          <main className="main-container">
            {/* <Header /> */}
            {children} {/* 모든 경로의 컴포넌트들 */}
          </main>
        </div>
      </div>
    </ErrorBoundary>
  );
}

BaseLayout.defaultProps = {
  children: [],
  user: {},
};
```

<div class="content-ad"></div>

BaseLayout 구성 요소:

- BaseLayout 구성 요소는 우리 애플리케이션의 모든 페이지에 대한 일반적인 레이아웃 구조로 작동합니다.
- 사이드바 및 본문 내용 영역과 같은 공통 요소를 포함합니다.
- 다른 경로의 하위 구성 요소는 메인 콘텐츠 영역 내에서 Outlet 구성 요소를 사용하여 렌더링됩니다.

React Outlet 이해:

- React Outlet은 AllRoutes 구성 요소 내에서 사용되어 부모 경로 내에서 하위 경로를 렌더링합니다.
- 이를 통해 경로를 모듈화하여 복잡한 네비게이션 구조를 관리하기 쉽게 만들 수 있습니다.
- 중첩된 경로는 더 큰 부모 경로의 일부일 수 있으면서 자체 레이아웃과 기능을 가질 수 있습니다.
- React Outlet을 사용함으로써 개발자는 React 애플리케이션에 대해 조직화되고 유지보수가 쉬운 코드베이스를 만들 수 있습니다.

<div class="content-ad"></div>

결론:
React Outlet은 React Router에서 제공하는 강력한 도구로 React 애플리케이션 내에서 중첩된 경로를 관리하는 데 사용됩니다. 그 사용법을 이해하고 라우팅 설정에 통합함으로써 더 다이내믹하고 유연한 사용자 인터페이스를 만들 수 있습니다. 라우트 보호, 레이아웃 생성 또는 복잡한 네비게이션 구조 처리와 같은 작업을 수행할 때 React Outlet은 견고한 React 애플리케이션을 구축하는 데 중요한 역할을 합니다.
