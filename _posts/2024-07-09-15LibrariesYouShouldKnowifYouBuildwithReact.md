---
title: "React로 개발할 때 알아야 할 15가지 라이브러리"
description: ""
coverImage: "/assets/img/2024-07-09-15LibrariesYouShouldKnowifYouBuildwithReact_0.png"
date: 2024-07-09 13:36
ogImage:
  url: /assets/img/2024-07-09-15LibrariesYouShouldKnowifYouBuildwithReact_0.png
tag: Tech
originalTitle: "15 Libraries You Should Know if You Build with React"
link: "https://medium.com/@khushi1399gupta/15-libraries-you-should-know-if-you-build-with-react-088d7bb110e8"
---

# React 소개

현대 웹 개발의 세계에서 React는 강력하고 다재다능한 라이브러리로 사용자 인터페이스를 구축하는 데 빛을 발합니다. Meta(이전 Facebook)에서 개발된 React는 개발자들 사이에서 엄청난 인기를 얻었으며 다양한 애플리케이션에서 널리 사용되고 있습니다.

# React란 무엇인가요?

React는 동적이고 인터랙티브한 사용자 인터페이스를 구축하는 과정을 간소화하는 무료 오픈 소스 프런트엔드 JavaScript 라이브러리입니다. React는 컴포넌트 기반 아키텍처를 채택하여 개발자가 재사용 가능한 UI 컴포넌트를 생성하고 이를 조합하여 복잡한 애플리케이션을 구축할 수 있도록 합니다.

<div class="content-ad"></div>

# 리액트 활용 예시

리액트는 Facebook, Instagram, Netflix, Airbnb, Twitter, WhatsApp Web, Pinterest, Twitch 등 인기 있는 웹사이트 및 웹 애플리케이션 개발에 널리 사용됩니다.

# 리액트 라이브러리 탐구

<div class="content-ad"></div>

# 라이브러리란 무엇인가요?

코딩에서 라이브러리는 개발자가 프로그래밍 작업을 단순화하고 가속화하기 위해 활용할 수 있는 미리 작성된 코드 집합을 가리킵니다. 이러한 라이브러리는 재사용 가능한 기능을 제공하여 다양한 응용 프로그램에 통합할 수 있도록 해주어 개발 시간과 노력을 줄여줍니다.

![라이브러리 이미지](/assets/img/2024-07-09-15LibrariesYouShouldKnowifYouBuildwithReact_0.png)

# React 개발을 위한 필수 라이브러리

<div class="content-ad"></div>

# 1. Axios

```js
npm i axios
```

Axios는 브라우저와 node.js를 위한 간단한 프로미스 기반 HTTP 클라이언트입니다. Axios는 매우 확장 가능한 인터페이스를 갖춘 작은 패키지의 쉽게 사용할 수 있는 라이브러리를 제공합니다.

```js
async function getUser() {
  try {
    const response = await axios.get("/user?ID=12345");
    console.log(response);
  } catch (error) {
    console.error(error);
  }
}
```

<div class="content-ad"></div>

표 태그를 사용하는 대신 Axios를 사용하여 API를 호출하고 코드 라인을 줄일 수 있습니다.

## 2. Formik:

Formik은 React 애플리케이션에서 양식 데이터를 빌드하고 처리하는 데 도움이 되는 무료 오픈 소스 라이브러리입니다. 간단한 API 및 내장된 유효성 검사를 제공하여 입력 데이터를 수집하고 조작하기 쉽게 만듭니다. Formik은 Airbnb, Walmart, Lyft 및 Stripe와 같은 회사에서 사용됩니다.

```js
npm i formik
```

<div class="content-ad"></div>

```js
import React from "react";
import { Formik, Form, Field, ErrorMessage } from "formik";

const initialValues = {
  firstName: "",
  lastName: "",
  email: "",
};

const onSubmit = (values) => {
  console.log(values);
};

const validate = (values) => {
  const errors = {};

  if (!values.firstName) {
    errors.firstName = "필수 항목입니다";
  }

  if (!values.lastName) {
    errors.lastName = "필수 항목입니다";
  }

  if (!values.email) {
    errors.email = "필수 항목입니다";
  } else if (!/^\S+@\S+\.\S+$/.test(values.email)) {
    errors.email = "유효한 이메일 주소가 아닙니다";
  }

  return errors;
};

const SampleForm = () => {
  return (
    <div>
      <h1>샘플 폼</h1>
      <Formik initialValues={initialValues} onSubmit={onSubmit} validate={validate}>
        {({ isSubmitting }) => (
          <Form>
            <div>
              <label htmlFor="firstName">이름:</label>
              <Field type="text" id="firstName" name="firstName" />
              <ErrorMessage name="firstName" component="div" />
            </div>

            <div>
              <label htmlFor="lastName">성:</label>
              <Field type="text" id="lastName" name="lastName" />
              <ErrorMessage name="lastName" component="div" />
            </div>

            <div>
              <label htmlFor="email">이메일:</label>
              <Field type="email" id="email" name="email" />
              <ErrorMessage name="email" component="div" />
            </div>

            <button type="submit" disabled={isSubmitting}>
              제출
            </button>
          </Form>
        )}
      </Formik>
    </div>
  );
};

export default SampleForm;
```

Formik은 React 애플리케이션에서 폼 검증을 간편하게 해주는 API를 제공하여 폼 상태와 검증 로직을 관리하는 것을 도와줍니다.

## 3. React Helmet

예를 들어, React Helmet을 사용하면 문서의 제목, 설명 및 메타 태그를 동적으로 설정할 수 있습니다. SEO를 위해 메타 태그를 업데이트해야 할 때 이 기능이 매우 유용합니다. React Helmet은 title, style, base, meta, link, script, NoScript를 포함한 모든 유효한 head 태그를 지원합니다.

<div class="content-ad"></div>

```js
npm i react-helmet
```

```js
import React from "react";
import { Helmet } from "react-helmet";

class Application extends React.Component {
  render() {
    return (
      <div className="application">
        <Helmet>
          <meta charSet="utf-8" />
          <title>My Title</title>
          <link rel="canonical" href="http://mysite.com/example" />
        </Helmet>
        ...
      </div>
    );
  }
}
```

React Helmet를 사용하면 웹 사이트의 SEO를 개선할 수 있습니다. 이것은 검색 엔진이 페이지를 인덱싱하고 순위를 매기는 데 사용하는 메타 태그를 설정하고 업데이트하기 쉽게 만들어줍니다. 콘텐츠에 대한 정확하고 최신 정보를 제공함으로써 검색 엔진이 웹 사이트를 더 잘 이해하고 검색 결과의 순위를 향상시킬 수 있습니다.

또한 React Helmet를 사용하여 웹 사이트의 소셜 미디어 공유를 향상시킬 수 있습니다. 여기에는 소셜 미디어 플랫폼이 콘텐츠를 공유할 때 사용하는 메타 태그를 설정하고 업데이트하기가 더 쉬워지는 기능이 포함됩니다.

<div class="content-ad"></div>

## 4. 리액트 - 리덕스:

리덕스는 예측 가능하고 유지보수가 쉬운 전역 상태 관리를 위한 JS 라이브러리입니다.

```js
npm i react-redux
```

리액트-리덕스의 기본 예제

<div class="content-ad"></div>

```js
import React from "react";
import { createStore } from "redux";
import { Provider, connect } from "react-redux";

// 초기 상태 정의
const initialState = {
  count: 0,
};

// 리듀서 함수 정의
const reducer = (state = initialState, action) => {
  switch (action.type) {
    case "INCREMENT":
      return {
        ...state,
        count: state.count + 1,
      };
    case "DECREMENT":
      return {
        ...state,
        count: state.count - 1,
      };
    default:
      return state;
  }
};

// Redux 스토어 생성
const store = createStore(reducer);

// 액션 생성자 정의
const increment = () => ({ type: "INCREMENT" });
const decrement = () => ({ type: "DECREMENT" });

// 카운터 컴포넌트
const Counter = ({ count, increment, decrement }) => {
  return (
    <div>
      <h1>Counter: {count}</h1>
      <button onClick={increment}>증가</button>
      <button onClick={decrement}>감소</button>
    </div>
  );
};

// 카운터 컴포넌트를 Redux 스토어에 연결
const ConnectedCounter = connect((state) => ({ count: state.count }), { increment, decrement })(Counter);

// 앱 컴포넌트
const App = () => {
  return (
    <Provider store={store}>
      <ConnectedCounter />
    </Provider>
  );
};

export default App;
```

앱의 전역 상태는 단일 스토어 내부의 객체 트리에 저장됩니다. 상태 트리를 변경하는售벼시 action을 생성하고 store에 dispatch합니다. 상태가 action에 응답하여 어떻게 업데이트되는지 지정하려면 이전 상태와 action을 기반으로 새 상태를 계산하는 순수 리듀서 함수를 작성합니다.

## 5. React Router DOM

```js
npm i react-router-dom
```

<div class="content-ad"></div>

React Router Dom은 React를 사용하여 구축된 웹 애플리케이션에서 라우팅 및 네비게이션을 관리하기 위해 일반적으로 사용됩니다. 이는 API를 제공하여 경로를 정의하고 탐색하고 렌더링하는 과정을 간소화합니다.

```js
import * as React from "react";
import * as ReactDOM from "react-dom/client";
import { createBrowserRouter, RouterProvider } from "react-router-dom";
import "./index.css";

const router = createBrowserRouter([
  {
    path: "/",
    element: <div>Hello world!</div>,
  },
]);

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <RouterProvider router={router} />
  </React.StrictMode>
);
```

react-router의 주요 장점은 다른 페이지로 이동하는 링크를 클릭할 때 페이지를 새로 고치지 않아도 된다는 것입니다...

## 6. Dotenv:

<div class="content-ad"></div>

```js
npm install dotenv --save
```

Dotenv(도트엔브)는 .env 파일에서 환경 변수를 process.env로 불러오는 의존성이 없는 모듈입니다. 코드베이스와 별도로 환경에 대한 구성을 저장하여 비밀번호와 비밀 키와 같이 비밀로 유지해야 하는 정보를 안전하게 보호합니다.

```js
import React, { useEffect, useState } from "react";
require("dotenv").config();

const App = () => {
  const [data, setData] = useState(null);

  useEffect(() => {
    // 환경 변수를 사용하여 API에서 데이터 가져오기
    fetch(process.env.REACT_APP_API_URL)
      .then((response) => response.json())
      .then((data) => setData(data))
      .catch((error) => console.error("데이터 가져오는 중 오류 발생:", error));
  }, []);

  return (
    <div>
      <h1>API로부터 데이터</h1>
      {data ? <pre>{JSON.stringify(data, null, 2)}</pre> : <p>로딩 중...</p>}
    </div>
  );
};

export default App;
```

## 7. esLint

<div class="content-ad"></div>

```js
npm i eslint
```

ESLint은 인기 있는 오픈 소스 자바스크립트 린트 유틸리티입니다. 이는 잠재적인 오류를 분석하고 코딩 표준을 강제하며 코드 품질을 향상시킵니다. ESLint는 개발자로서 일반적인 실수를 식별하고 수정하고, 최고의 실행 관행을 사용하며 코드베이스 전체에서 일관성을 유지하는 데 도움을 줄 수도 있습니다.

기본 예시

```js
import js from "@eslint/js";

export default [
  js.configs.recommended,

  {
    rules: {
      "no-unused-vars": "warn",
      "no-undef": "warn",
    },
  },
];
```

<div class="content-ad"></div>

```js
import React from "react";
import Header from "./components/Header";

const App = () => {
  return (
    <div>
      <Header />
    </div>
  );
};

export default App;
```

위와 같이 하나의 컴포넌트를 렌더링하려고 합니다. 그러나 이 파일을 실행하면 몇 가지 오류가 발생합니다.

```js
1:8  warning  'React' is defined but never used   no-unused-vars
2:8  warning  'Header' is defined but never used  no-unused-vars
```

## 8. date-fns

<div class="content-ad"></div>

```js
npm i date-fns
```

date-fns는 브라우저 및 Node.js에서 JavaScript 날짜를 다루는 가장 포괄적이면서도 간단하고 일관된 도구 세트를 제공합니다.

```js
// 날짜 문자열 파싱
const date = dateFns.parse("2023-08-04");

// 날짜 형식 지정
const formattedDate = dateFns.format(date, "MM/dd/yyyy");

// 두 날짜 비교
const areDatesEqual = dateFns.isEqual(date1, date2);

// 두 날짜 간의 차이 계산
const differenceInDays = dateFns.differenceInDays(date1, date2);
```

Date-fns는 JavaScript에서 날짜와 시간을 다루는 강력하고 다재다능한 라이브러리입니다. 모든 크기의 프로젝트에 적합한 선택입니다.

<div class="content-ad"></div>

## 9. 리액트 에러 바운더리

```js
npm install react-error-boundary
```

에러 바운더리는 자바스크립트 오류를 캐치하여 해당 오류를 기록하고, 충돌이 발생한 컴포넌트 트리 대신 대체 UI를 표시하는 React 컴포넌트입니다.

이 컴포넌트는 여러 가지 방법으로 대체 UI를 렌더링하는 방법을 지원합니다 (아래에서 보여집니다).

<div class="content-ad"></div>

```js
"use client";

import { ErrorBoundary } from "react-error-boundary";

function fallbackRender({ error, resetErrorBoundary }) {
  // resetErrorBoundary()을 호출하여 에러 경계를 재설정하고 렌더링을 다시 시도하세요.

  return (
    <div role="alert">
      <p>문제가 발생했습니다:</p>
      <pre style={ color: "red" }>{error.message}</pre>
    </div>
  );
}

<ErrorBoundary
  fallbackRender={fallbackRender}
  onReset={(details) => {
    // 앱의 상태를 재설정하여 에러가 다시 발생하지 않도록 합니다.
  }
>
  <ExampleApplication />
</ErrorBoundary>;
```

기타 React 컴포넌트 주위에 ErrorBoundary 컴포넌트를 래핑하여 에러를 "잡고" 대체 UI를 렌더링할 수 있습니다 (대체 UI는 실제 UI 로딩이 완료되지 않았을 때 임시적으로 렌더링되는 UI입니다).

## 10. sweetalert

```js
npm install sweetalert --save
```

<div class="content-ad"></div>

SweetAlert은 웹 애플리케이션을 위한 대체 경고창 및 모달 대화 상자를 제공하는 JavaScript 라이브러리입니다. 내장된 경고 함수를 대체하고 기본 브라우저 대화 상자의 사용자 인터페이스를 개선할 수 있습니다.

```js
import React from "react";
import Swal from "sweetalert";

const SweetAlertExample = () => {
  const handleClick = () => {
    Swal.fire({
      title: "안녕하세요!",
      text: "이것은 SweetAlert 대화 상자입니다.",
      icon: "success",
      confirmButtonText: "확인",
    });
  };

  return (
    <div>
      <h1>SweetAlert 예제</h1>
      <button onClick={handleClick}>SweetAlert 보이기</button>
    </div>
  );
};

export default SweetAlertExample;
```

## 11. styled-components

```js
npm i styled-components
```

<div class="content-ad"></div>

styled-components는 React 및 React Native 개발자들이 UI 구성 요소와 스타일을 하나의 파일 위치에서 정의할 수 있게 해주는 오픈 소스 라이브러리입니다. 이 라이브러리는 JavaScript (JS) 내에서 CSS를 사용하여 개발자가 JavaScript 파일에서 직접 CSS 코드를 작성할 수 있도록 합니다.

```js
import React from "react";

import styled from "styled-components";

// <Title> 리액트 컴포넌트를 생성합니다. 이 컴포넌트는 <h1> 태그를 렌더링하며 가운데 정렬되어 palevioletred 색상으로 크기는 1.5em으로 설정합니다.
const Title = styled.h1`
  font-size: 1.5em;
  text-align: center;
  color: palevioletred;
`;

// <Wrapper> 리액트 컴포넌트를 생성합니다. 이 컴포넌트는 패딩이 적용되고 papayawhip 배경을 가진 <section>을 렌더링합니다.
const Wrapper = styled.section`
  padding: 4em;
  background: papayawhip;
`;

function MyUI() {
  return (
    // 다른 React 컴포넌트처럼 사용합니다. 단, 여기서는 스타일이 적용된 컴포넌트를 사용하게 됩니다!
    <Wrapper>
      <Title>Hello World, this is my first styled component!</Title>
    </Wrapper>
  );
}
```

## 12. react-tooltip

```js
npm install react-tooltip
```

<div class="content-ad"></div>

리액트 툴팁 패키지는 앵커 요소에 바인딩되어 특정 요소 정보를 표시하는 `Tooltip` 컴포넌트를 제공합니다.

```js
import React from "react";
import ReactTooltip from "react-tooltip";

const TooltipExample = () => {
  return (
    <div>
      <h1>리액트 툴팁 예시</h1>
      <button data-tip="안녕, 나는 툴팁이야!" data-for="tooltip">
        마우스를 올려보세요
      </button>
      <ReactTooltip id="tooltip" place="bottom" effect="solid" />
    </div>
  );
};

export default TooltipExample;
```

## 13. 리액트 스피너

```js
npm install --save react-spinners
```

<div class="content-ad"></div>

react-spinner-loader은 데이터가 뷰로 로드되기 전에 비동기-await 작업을 위해 구현할 수 있는 간단한 React SVG 스피너 컴포넌트를 제공합니다.

```js
import React from "react";
import { css } from "@emotion/react";
import { RingLoader } from "react-spinners";

const override = css`
  display: block;
  margin: 0 auto;
  border-color: red;
`;

const SpinnerExample = () => {
  return (
    <div className="sweet-loading">
      <RingLoader color={"#123abc"} css={override} size={150} loading={true} />
    </div>
  );
};

export default SpinnerExample;
```

## 14. Yup

```js
npm i yup
```

<div class="content-ad"></div>

Yup은 런타임 값 분석 및 유효성 검사를위한 스키마 빌더입니다. 스키마를 정의하고 값의 형태를 맞추거나 기존 값의 형태를 확인하거나 둘 다 할 수 있습니다. Yup 스키마는 매우 표현력이 뛰어나며 복잡한 상호 의존적 유효성 검사 또는 값 변환을 모델링할 수 있습니다.

```js
import * as yup from 'yup';

const personSchema = yup.object({
  firstName: yup.string().defined(),
  nickName: yup.string().default('').nullable(),
  sex: yup
    .mixed()
    .oneOf(['male', 'female', 'other'] as const)
    .defined(),
  email: yup.string().nullable().email(),
  birthDate: yup.date().nullable().min(new Date(1900, 0, 1)),
});
```

# 15. @testing-library/jest-dom

```js
npm install --save-dev @testing-library/jest-dom
```

<div class="content-ad"></div>

@testing-library/jest-dom는 DOM 상태를 테스트하기 위한 맞춤형 매처를 제공하는 라이브러리입니다. 이는 Jest 테스트 프레임워크의 확장으로, DOM 요소, 속성 및 내용에 대한 단언을 가능하게 합니다.

```js
<button data-testid="button" type="submit" disabled>submit</button>
<fieldset disabled><input type="text" data-testid="input" /></fieldset>
<a href="..." disabled>link</a>
```

```js
expect(getByTestId("button")).toBeDisabled();
expect(getByTestId("input")).toBeDisabled();
expect(getByText("link")).not.toBeDisabled();
```

## 결론
