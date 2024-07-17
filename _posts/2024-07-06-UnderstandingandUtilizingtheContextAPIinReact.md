---
title: "React에서 Context API 이해 및 활용하는 방법"
description: ""
coverImage: "/it-shar.ing/assets/no-image.jpg"
date: 2024-07-06 02:02
ogImage:
  url: /it-shar.ing/assets/no-image.jpg
tag: Tech
originalTitle: "Understanding and Utilizing the Context API in React"
link: "https://medium.com/@jai_bajpai/understanding-and-utilizing-the-context-api-in-react-20005cbdda44"
---

오늘은 React 개발자들에게 필수적인 주제인 Context API에 대해 알아보려고 해요. 만약에 여러분이 prop drilling에 어려움을 겪어 본 적이 있거나 응용 프로그램 전체에서 상태를 공유할 수 있는 더 깨끗한 방법이 필요했다면, Context API가 여러분의 새로운 친구가 될 겁니다. 이 글에서는 Context API가 무엇인지, 왜 유용한지, 그리고 프로젝트에서 효과적으로 활용하는 방법을 알아볼 거에요.

Context API란 무엇인가요?

Context API는 React에서 제공하는 기능으로, 모든 수준에서 수동으로 props를 전달하지 않고 응용 프로그램 전체에서 상태를 공유할 수 있게 해줍니다. 이를 통해 컴포넌트 트리를 통해 데이터를 전달할 수 있는 방법을 제공하여 코드를 더 깨끗하고 유지보수하기 쉽게 만들어 줍니다.

Context API를 사용하는 이유는 무엇인가요?

<div class="content-ad"></div>

- 프롭 드릴링을 피하세요: 부모 구성 요소에서 심층적으로 중첩된 자식으로 데이터를 전달해야 할 때, 프롭 드릴링은 코드를 혼란스럽고 관리하기 어렵게 만들 수 있습니다. Context API는 전역적으로 데이터를 공유할 수 있는 방법을 제공하여 이 문제를 해결합니다.
- 코드 가독성 향상: Context API를 사용하면 지나치게 많은 프롭이 필요한 것을 제거하여 구성 요소를 더 깔끔하게 유지할 수 있습니다. 이는 더 읽기 쉽고 유지하기 쉬운 코드로 이어집니다.
- 효과적인 상태 관리: Redux와 같은 라이브러리는 복잡한 상태 관리에 좋지만, Context API는 덜 복잡한 애플리케이션에 대한 간단한 대안을 제공합니다. React에 내장되어 있으므로 추가적인 종속성을 설치할 필요가 없습니다.

이제 Context API를 사용하는 방법을 설명하는 예제를 살펴보겠습니다. 로그인 또는 로그아웃된 사용자 인증 상태를 관리하는 간단한 애플리케이션을 만들어 보겠습니다.

먼저, 컨텍스트를 생성해야 합니다. React의 createContext 함수를 사용하여 이 작업을 수행할 수 있습니다.

```js
import React, { createContext, useState, useContext } from "react";

const AuthContext = createContext();
```

<div class="content-ad"></div>

프로바이더 컴포넌트는 상태를 보유하고 다른 컴포넌트에 제공하는 역할을 합니다. 상태를 전달하기 위해 값(prop)를 사용합니다.

인증 상태를 보유하고 로그인 및 로그아웃 기능을 제공하는 프로바이더 컴포넌트를 생성해보세요.

```js
//AuthProvider.js
const AuthProvider = ({ children }) => {
  const [isAuthenticated, setIsAuthenticated] = useState(false);

  const login = () => setIsAuthenticated(true);
  const logout = () => setIsAuthenticated(false);

  return <AuthContext.Provider value={(isAuthenticated, login, logout)}>{children}</AuthContext.Provider>;
};
```

애플리케이션(또는 일부분)을 프로바이더 컴포넌트로 감싸서 상태를 해당 컴포넌트의 자식들에게 제공합니다. 우리의 경우 모든 컴포넌트에 인증 상태를 제공하기 위해 애플리케이션을 AuthProvider로 감싸보세요.

<div class="content-ad"></div>

```js
//Home.js

const App = () => (
  <AuthProvider>
    <Navbar />
    <Home />
  </AuthProvider>
);
```

컴포넌트에서 컨텍스트에 접근하려면 useContext 훅을 사용하세요.

인증 컨텍스트를 사용할 컴포넌트를 생성하세요. Navbar 컴포넌트는 로그인/로그아웃 버튼을 표시하고 Main 컴포넌트는 인증 상태에 따라 다른 콘텐츠를 표시합니다.

```js
//Navbar.js

import React, { useContext } from "react";
import { AuthContext } from "./AuthProvider";

const Navbar = () => {
  const { isAuthenticated, login, logout } = useContext(AuthContext);

  return (
    <nav>{isAuthenticated ? <button onClick={logout}>로그아웃</button> : <button onClick={login}>로그인</button>}</nav>
  );
};
```

<div class="content-ad"></div>

여기는 홈 컴포넌트입니다.

```js
//Home.js

import React, { useContext } from "react";
import { AuthContext } from "./AuthProvider";

const Main = () => {
  const { isAuthenticated } = useContext(AuthContext);

  return (
    <main>{isAuthenticated ? <h1>다시 오신 걸 환영합니다, 사용자님!</h1> : <h1>계속하려면 로그인해주세요.</h1>}</main>
  );
};
```

Context API 사용 시 권장 사항

- Context 범위 제한: 전체 애플리케이션에 대해 하나의 컨텍스트를 생성하지 말고, 앱의 다른 부분에 대해 여러 개의 컨텍스트를 생성하여 상태 관리를 모듈화하고 효율적으로 유지하세요.
- 적절한 Context 사용: Context API는 강력하지만 너무 많이 사용하면 성능 문제가 발생할 수 있습니다. 많은 컴포넌트 간에 공유되어야 하는 전역 상태에만 사용하세요.
- 다른 상태 관리 도구와 결합: 복잡한 애플리케이션의 경우, Context API를 Redux나 Zustand와 같은 다른 상태 관리 도구와 결합하는 것을 고려해보세요. Context API는 특정하고 복잡하지 않은 상태 요구에 사용하세요.

<div class="content-ad"></div>

결론

Context API는 React 애플리케이션에서 상태를 관리하는 강력한 도구입니다. 효과적으로 사용하는 방법을 이해함으로써 prop drilling을 피하고 코드의 가독성을 향상시키며 애플리케이션의 상태를 더 효율적으로 관리할 수 있습니다. 이 가이드가 Context API에 대한 이해를 돕고 다음 프로젝트에 구현할 열망을 고취시켰으면 좋겠습니다.
