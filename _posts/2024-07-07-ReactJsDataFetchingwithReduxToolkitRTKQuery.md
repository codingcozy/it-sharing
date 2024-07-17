---
title: "Reactjs Redux ToolkitRTK Query로 데이터 가져오기 방법"
description: ""
coverImage: "/assets/img/2024-07-07-ReactJsDataFetchingwithReduxToolkitRTKQuery_0.png"
date: 2024-07-07 12:27
ogImage:
  url: /assets/img/2024-07-07-ReactJsDataFetchingwithReduxToolkitRTKQuery_0.png
tag: Tech
originalTitle: "React.Js: Data Fetching with Redux Toolkit (RTK) Query."
link: "https://medium.com/@olaotanabarowei/react-js-data-fetching-with-redux-toolkit-rtk-query-ad30553df38c"
---

<img src="/assets/img/2024-07-07-ReactJsDataFetchingwithReduxToolkitRTKQuery_0.png" />

웹 애플리케이션의 완성을 위해 클라이언트 측과 서버 측 사이의 효과적인 커뮤니케이션 채널은 필수적입니다. React 애플리케이션은 복잡해질 수 있습니다. 예를 들어, Twitter 클론 웹 앱을 구축한다고 가정해보면, 홈페이지는 서버의 다양한 구성 요소와 상호 작용하는 여러 구성 요소로 구성됩니다. 이에는 다음이 포함될 수 있습니다:

- 서버에서 모든 게시물 가져오기.
- 특정 게시물에 좋아요와 좋아요 취소하기.
- 사용자 팔로우 및 언팔로우.
- 단일 게시물 및 관련 댓글 가져오기.
- 특정 게시물을 좋아하는 모든 사용자 가져오기.

홈페이지 외에도 사용자 프로필, 게시물 생성, 프로필 편집 페이지 등 다양한 페이지가 있습니다. 각 페이지는 서버와 상호 작용하여 상당한 양의 코드가 필요합니다.

<div class="content-ad"></div>

React에서 데이터를 가져오는 기본적인 과정은 다음과 같이 설명할 수 있습니다:

- 여러 상태 변수를 선언합니다.
- JavaScript의 기본 fetch 메서드나 Axios와 같은 외부 API를 사용하여 비동기 fetch 함수를 정의합니다.
- useEffect 훅을 활용하여 정의된 fetch 함수를 호출합니다.
- fetch 함수의 결과에 따라 상태 변수를 업데이트합니다.
- 상태 변수의 값에 따라 조건부 사용자 인터페이스를 렌더링합니다.

```js
import React, { useState, useEffect } from "react";

const MyComponent = () => {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchData = async () => {
      try {
        const response = await fetch("https://api.example.com/data");
        if (!response.ok) {
          throw new Error("Network response was not ok");
        }
        const jsonData = await response.json();
        setData(jsonData);
        setLoading(false);
      } catch (error) {
        setError(error);
        setLoading(false);
      }
    };

    fetchData();
  }, []);

  if (loading) {
    return <p>Loading...</p>;
  }

  if (error) {
    return <p>Error: {error.message}</p>;
  }

  return (
    <div>
      <h1>My Data</h1>
      {data && (
        <ul>
          {data.map((item) => (
            <li key={item.id}>{item.name}</li>
          ))}
        </ul>
      )}
    </div>
  );
};

export default MyComponent;
```

한 페이지에서 최대 다섯 개의 다른 API 엔드포인트와 실제 애플리케이션에서 최대 열 개의 페이지와 상호 작용할 수 있습니다. 이 구성은 복잡한 웹 애플리케이션에는 최적이 아닌데요, 여기서 Redux Toolkit (RTK) Query가 유용하게 사용됩니다. RTK Query는 상태 관리와 데이터 가져오기 함수의 생성을 자동화하여 복잡한 웹 애플리케이션에서 간편화된 프로세스를 제공해줍니다.

<div class="content-ad"></div>

리덕스는 자바스크립트 애플리케이션의 전역 상태를 관리하기 위한 라이브러리입니다. 앱의 데이터를 추적하고 앱의 다른 컴포넌트간에 더 효율적인 통신을 도와줍니다.

리덕스 툴킷(RTK)은 리덕스 기반 애플리케이션에서 상태를 관리하는 프로세스를 단순화하는 라이브러리입니다.

예제 프로젝트 — 스타워즈 API에서 데이터 가져오기

RTK Query의 이해를 돕기 위해 RTK Query를 사용하여 데이터를 가져오는 미니 프로젝트를 만들어보겠습니다.

<div class="content-ad"></div>

Step 1: 프로젝트 설정

선호하는 디렉토리의 터미널에서 다음 명령을 실행하여 새로운 React 프로젝트를 생성하세요. "rtk-query"를 원하는 프로젝트 이름으로 대체하세요.

```js
npx create-react-app rtk-query
```

<div class="content-ad"></div>

리덕스 툴킷을 사용하려면 리액트 컴포넌트를 리덕스 스토어에 연결하기 위해 리액트-리덕스 라이브러리가 필요합니다. 아래 명령을 실행하여 설치할 수 있습니다:

```js
npm install react-redux @reduxjs/toolkit
```

3단계: RTK Query 설정

src 디렉터리에서 redux라는 새 폴더를 만듭니다. 해당 폴더 안에 api라는 또 다른 폴더를 추가하세요. 복잡한 애플리케이션에서는 이 디렉터리가 각기 다른 기능을 위해 설계된 여러 API를 호스팅할 것입니다. 프로젝트가 단일 API를 사용하므로 지정된 위치에 index.js 파일을 만들어 주세요.

<div class="content-ad"></div>

아래 표를 마크다운 형식으로 변경하세요.

<img src="/assets/img/2024-07-07-ReactJsDataFetchingwithReduxToolkitRTKQuery_1.png" />

다음 코드를 index.js 파일에 입력해 주세요:

```js
import { createApi, fetchBaseQuery } from "@reduxjs/toolkit/query/react";

export const api = createApi({
  reducerPath: "api",
  baseQuery: fetchBaseQuery({
    baseUrl: "https://swapi.dev/api",
  }),
  endpoints: (builder) => ({
    fetchPeople: builder.query({ query: () => "/people" }),
    fetchPlanets: builder.query({ query: () => "/planets" }),
  }),
});

export const { useFetchPeopleQuery, useFetchPlanetsQuery } = api;코드 분석
```

- RTK Query의 createApi 함수는 다중 엔드포인트를 가진 API를 정의하는 데 도움을 줍니다.
- fetchBaseQuery 함수는 API의 기본 URL을 호출합니다. 이를 통해 API를 재정의하지 않고 여러 엔드포인트를 호출할 수 있습니다.
- baseQuery는 기본 fetch 동작을 구성합니다. 여기에서 헤더 및 인증을 구성할 수 있습니다.
- endpoints는 개별 API 엔드포인트를 정의합니다.
- useFetchPeopleQuery 및 useFetchPlanetsQuery 훅은 RTK Query에 의해 자동으로 생성됩니다. 이 훅들은 컴포넌트가 데이터를 검색하고 있고, people과 planets 엔드포인트 호출에 대한 로딩 및 오류 상태를 처리합니다.

<div class="content-ad"></div>

Step 4: 스토어 설정

Redux 폴더 안에 store.js 파일을 생성하세요.

![이미지](/assets/img/2024-07-07-ReactJsDataFetchingwithReduxToolkitRTKQuery_2.png)

아래 코드를 store.js 파일에 입력하세요:

<div class="content-ad"></div>

```js
import { configureStore } from "@reduxjs/toolkit";
import { api } from "./api";

export default configureStore({
  reducer: {
    [api.reducerPath]: api.reducer,
  },
  middleware: (getDefaultMiddleWare) => getDefaultMiddleWare().concat(api.middleware),
});
```

여기서:

- api.reducer는 생성된 RTK Query reducer를 redux store에 통합합니다.
- api.middleware는 비동기 요청을 관리하기 위해 필요한 미들웨어를 통합합니다.

앱의 메인 index.js 파일에 위치하세요:

<div class="content-ad"></div>

- 설정된 상점을 가져옵니다.
- 그런 다음, `react-redux`에서 `Provider/` 컴포넌트를 가져와서 `App/` 컴포넌트를 `Provider/`로 감싸고 상점을 전역적으로 사용할 수 있도록 prop으로 전달합니다.

```js
import React from "react";
import ReactDOM from "react-dom";
import App from "./App";
import { Provider } from "react-redux";
import store from "./redux/store";

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>
);
```

5단계: 컴포넌트에서 훅 사용하기

이 튜토리얼은 useFetchPeopleQuery 훅을 사용하여 사용자를 가져올 것입니다. RTK Query의 컨셉을 연습하고 이해하기 위해 useFetchPlanetsQuery 훅을 활용할 것입니다.

<div class="content-ad"></div>

앱의 app.js 파일에서 useFetchPeopleQuery 훅을 가져와 실행해주세요.

일반적인 RTK Query 데이터 가져오는 훅은 다음과 같은 프로퍼티를 포함한 객체를 반환합니다:

- isLoading: 데이터를 가져오는 과정이 진행 중인지를 나타내는 부울 값입니다.
- isSuccess: 데이터를 가져오는 과정이 성공적으로 완료되었는지를 나타내는 부울 값입니다.
- data: API로부터 검색한 데이터입니다.
- isError: 데이터를 가져오는 과정 중에 오류가 발생했는지를 나타내는 부울 값입니다.
- error: 오류가 발생했을 경우 해당 오류에 대한 세부 정보입니다.

```js
import { useFetchPeopleQuery } from "./redux/api";

function App() {
  const { data, isLoading, isError, isSuccess, error } = useFetchPeopleQuery();

  return (
    <div className="App">
      <h2>RTK FETCHING TUTORIAL</h2>
    </div>
  );
}

export default App;
```

<div class="content-ad"></div>

반환된 객체를 추출한 후, useEffect 훅을 사용하여 콘솔에 가져온 결과를 기록하고 가져온 결과에 따라 조건부로 컴포넌트를 렌더링하십시오.

```js
import { useEffect } from "react";
import { useFetchPeopleQuery } from "./redux/api";

function App() {
  const { data, isLoading, isError, isSuccess, error } = useFetchPeopleQuery();

  useEffect(() => {
    if (isSuccess) {
      console.log(data);
    }
    if (isError) {
      console.log(error);
    }
  }, [error, isError, isSuccess, data]);

  return (
    <div className="App">
      <h1>TRK FETCHING</h1>
      {isLoading && <p>Loading...</p>}
      {isError && <p>Error</p>}
      {isSuccess && <p>Success :)</p>}
    </div>
  );
}

export default App;
```

이 코드는 성공적인 가져오기 시 콘솔에 다음과 같이 기록됩니다:

![ReactJS Data Fetching with Redux Toolkit RTK Query](/assets/img/2024-07-07-ReactJsDataFetchingwithReduxToolkitRTKQuery_3.png)

<div class="content-ad"></div>

RTK Query를 사용하여 특정 데이터를 가져오기

호출할 때 후크에 인수를 전달하여 RTK Query를 사용하여 특정 데이터를 가져올 수 있습니다. 이 데이터는 API를 구성하는 동안 쿼리 설정에 활용될 수 있습니다.

API에서 단일 사용자를 검색하려면 엔드포인트 빌더 함수 객체 내에 새로운 fetchPerson 메소드를 추가하고 personId 매개변수를 포함해야 합니다. 그런 다음 RTK Query가 자동으로 생성하는 useFetchPersonQuery 후크를 내보냅니다.

```js
import { createApi, fetchBaseQuery } from "@reduxjs/toolkit/query/react";

export const api = createApi({
  reducerPath: "api",
  baseQuery: fetchBaseQuery({
    baseUrl: "https://swapi.dev/api",
  }),
  endpoints: (builder) => ({
    fetchPeople: builder.query({ query: () => "/people" }),
    fetchPlanets: builder.query({ query: () => "/planets" }),
    fetchPerson: builder.query({
      query: (personId) => `/people/${personId}`,
    }),
  }),
});

export const { useFetchPeopleQuery, useFetchPlanetsQuery, useFetchPersonQuery } = api;
```

<div class="content-ad"></div>

앱.js 파일에서:

- useFetchPersonQuery 훅을 가져옵니다.
- 훅을 호출하고 인자로 1을 전달합니다.
- 훅에 의해 반환된 객체에서 결과를 추출합니다.
- useEffect 훅을 이용하여 콘솔에 fetch 결과를 로그하고 결과에 따라 컴포넌트를 조건부 렌더링합니다.

```js
import { useEffect } from "react";
import { useFetchPersonQuery } from "./redux/api";

function App() {
  const { data, isLoading, isError, isSuccess, error } = useFetchPersonQuery(1);

  useEffect(() => {
    if (isSuccess) {
      console.log(data);
    }
    if (isError) {
      console.log(error);
    }
  }, [error, isError, isSuccess, data]);
  return (
    <div className="App">
      <h1>RTK FETCHING</h1>
      {isLoading && <p>Loading...</p>} {isError && <p>Error</p>} {isSuccess && <p>Success :)</p>}
    </div>
  );
}

export default App;
```

성공적인 fetch 시 콘솔에 다음과 같이 기록됩니다:

<div class="content-ad"></div>

![image](/assets/img/2024-07-07-ReactJsDataFetchingwithReduxToolkitRTKQuery_4.png)

결론

RTK Query는 클라이언트와 서버 간 데이터 트랜잭션의 관리를 크게 단순화하여 여러 복잡한 작업을 능숙하게 처리합니다. 더 포괄적인 이해를 위해 공식 문서를 참조하는 것이 매우 권장됩니다.

본 기사에서는 RTK Query를 사용한 데이터 가져오기의 기본 사항을 소개했습니다. 그러나 RTK Query가 제공하는 기본 기능 이상의 다양한 고급 기능이 있습니다. 다음은 그 중 일부입니다:

<div class="content-ad"></div>

- 데이터 변경(서버에 POST 요청 보내기).
- 정규화 및 캐싱.
- 페이지네이션 및 커서 기반 페이지네이션.
- 백그라운드 데이터 업데이트.

이번 튜토리얼은 여기까지 입니다. 무언가를 배우고 즐거운 시간이 되었으면 좋겠습니다.

더 많은 재미있고 유익한 기사를 보시려면 팔로우해주세요.

즐거운 코딩하세요!

<div class="content-ad"></div>

추가 자료

- [RTK Query](https://redux-toolkit.js.org/rtk-query/): Redux Toolkit
- [Redux](https://redux.js.org/): 예측 가능하고 유지보수 가능한 전역 상태 관리를 위한 JS 라이브러리
- [React Redux](https://react-redux.js.org/): React Redux

<div class="content-ad"></div>

Redux Toolkit | Redux Toolkit (redux-toolkit.js.org)
