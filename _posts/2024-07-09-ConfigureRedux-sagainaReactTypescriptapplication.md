---
title: "React Typescript 애플리케이션에서 Redux-saga 설정 방법"
description: ""
coverImage: "/assets/img/2024-07-09-ConfigureRedux-sagainaReactTypescriptapplication_0.png"
date: 2024-07-09 13:31
ogImage:
  url: /assets/img/2024-07-09-ConfigureRedux-sagainaReactTypescriptapplication_0.png
tag: Tech
originalTitle: "Configure Redux-saga in a React Typescript application"
link: "https://medium.com/@pramudithame/configure-redux-saga-in-a-react-typescript-application-4663f972b327"
---

<img src="/assets/img/2024-07-09-ConfigureRedux-sagainaReactTypescriptapplication_0.png" />

# 프로젝트 설정하기

이전 게시물에서 상태 관리의 중요성, Redux의 소개, Redux-Saga가 무엇이고 언제 사용해야 하는지, 그리고 Redux-Saga 애플리케이션의 아키텍처에 대해 설명했습니다. 이제 자세히 알아보고 React TypeScript 애플리케이션에서 Vite를 사용하여 Redux-Saga를 구성해 보겠습니다.

터미널을 열고 다음 명령을 사용하여 새 프로젝트를 만들어보세요:

<div class="content-ad"></div>

```js
npm create vite@latest
```

![Screenshot 1](/assets/img/2024-07-09-ConfigureRedux-sagainaReactTypescriptapplication_1.png)

터미널에서는 프로젝트 이름을 입력하고 프레임워크를 선택하라는 안내가 나올 겁니다. React를 선택하고 나면, 새로 생성된 React TypeScript 애플리케이션에서 Redux-Saga를 구성하는 것에 대해 알아볼 준비가 돼 있습니다.

![Screenshot 2](/assets/img/2024-07-09-ConfigureRedux-sagainaReactTypescriptapplication_2.png)

<div class="content-ad"></div>

이제 필요한 종속 항목을 설치하고 개발 환경에서 프로젝트를 실행하여 모든 것이 올바르게 설정되었는지 확인해 봅시다.

터미널을 열고 프로젝트 디렉토리로 이동하세요. 다음 명령어를 실행해주세요:

```js
cd your-project-name
npm install
npm run dev
```

# 폴더 구조 및 종속 항목

<div class="content-ad"></div>

다음으로, Redux-Saga를 프로젝트에 통합하기 위해 필요한 종속성을 설치해야 합니다. 터미널을 열고 다음 명령어를 실행해주세요:

```js
npm install redux react-redux redux-saga @types/react-redux @types/redux-saga @reduxjs/toolkit axios
```

아래는 우리가 설치한 종속성에 대한 간단한 설명입니다:

- redux: 핵심 Redux 라이브러리입니다.
- react-redux: Redux의 공식 React 바인딩입니다.
- redux-saga: Redux에서 사이드 이펙트를 처리하기 위한 미들웨어입니다.
- @types/react-redux 및 @types/redux-saga: react-redux와 redux-saga의 TypeScript 유형 정의입니다.
- @reduxjs/toolkit: 효율적인 Redux 개발을 위한 공식적이고 의견이 포함된 툴셋입니다.
- axios: API 요청을 위한 프로미스 기반 HTTP 클라이언트입니다.

<div class="content-ad"></div>

src 폴더 내부에 redux라는 새 폴더를 만들어주세요.

# 스토어 생성하기

React TypeScript 애플리케이션에서 Redux 스토어를 활성화하려면 스토어를 만들고 그 스토어를 프로바이더를 사용하여 `App` 컴포넌트로 감싸야 합니다.

먼저 redux 폴더 내에 Store.tsx 파일을 만들고 아래 코드 블록을 추가해주세요.

<div class="content-ad"></div>

```js
import { configureStore } from "@reduxjs/toolkit";
import createSagaMiddleware from "redux-saga";
import rootReducer from "./reducers/rootReducer";
import rootSaga from "./sagas/rootSaga";

// Redux Toolkit의 configureStore를 사용하여 Redux store를 생성합니다.
// Redux Saga 미들웨어를 생성하기 위해 createSagaMiddleware를 사용합니다.
// 모든 리듀서를 결합한 rootReducer입니다.
// 모든 사가를 결합한 rootSaga입니다.

// rootSaga를 실행하기 위해 saga 미들웨어를 생성합니다.
const sagaMiddleware = createSagaMiddleware();

// 루트 리듀서와 saga 미들웨어를 사용하여 스토어를 구성합니다.
const store = configureStore({
  reducer: rootReducer,
  middleware: (getDefaultMiddleware) => getDefaultMiddleware().concat(sagaMiddleware),
});

// rootSaga를 실행합니다.
sagaMiddleware.run(rootSaga);
export default store;
```

코드 설명:

- Redux Toolkit의 configureStore를 사용하여 Redux store를 생성합니다.
- Redux Saga 미들웨어를 생성하기 위해 createSagaMiddleware를 사용합니다.
- 모든 리듀서를 결합한 rootReducer입니다.
- 모든 사가를 결합한 rootSaga입니다.

스토어 구성: 우리는 rootReducer를 전달하고 기본 미들웨어에 saga 미들웨어를 추가하여 스토어를 구성합니다.

<div class="content-ad"></div>

루트 사가를 실행하고 스토어를 내보내세요.

# 애플리케이션을 Provider로 감싸고 스토어를 전달하세요.

다음으로, main.tsx 파일(또는 App 컴포넌트를 감싸는 경우 App.tsx)을 열어서 react-redux의 Provider로 App 컴포넌트를 감싸세요:

```js
import React from 'react'
import ReactDOM from 'react-dom/client'
import App from './App.tsx'
import './index.css'
import { Provider } from 'react-redux'
import store from './redux/store.tsx'

ReactDOM.createRoot(document.getElementById('root')!).render(
  <Provider store={store}>
    <React.StrictMode>
      <App />
    </React.StrictMode>,
  </Provider>
)
```

<div class="content-ad"></div>

# 상수 정의

Redux 애플리케이션의 액션을 정의하기 위해 TypeScript 열거형(enum)과 인터페이스를 사용하여 타입 안전성과 더 나은 코드 유지 보수성을 확보할 것입니다.

먼저, redux 폴더 안에 actionTypes.ts라는 새 파일을 만들고 다음 코드를 추가해주세요:

```js
export enum ReduxActionTypes {
    FETCH_PRODUCTS = "FETCH_PRODUCTS",
    FETCH_PRODUCTS_SUCCESS = "FETCH_PRODUCTS_SUCCESS",
    FETCH_PRODUCTS_ERROR = "FETCH_PRODUCTS_ERROR"
}

export interface FetchProductDataAction {
    type: ReduxActionTypes.FETCH_PRODUCTS;
    payload: any;
}

export interface FetchProductDataSuccessAction {
    type: ReduxActionTypes.FETCH_PRODUCTS_SUCCESS;
    payload: any;
}

export interface FetchProductDataErrorAction {
    type: ReduxActionTypes.FETCH_PRODUCTS_ERROR;
    error: string;
}

export type ActionTypes =
    FetchProductDataAction
    | FetchProductDataSuccessAction
    | FetchProductDataErrorAction;
```

<div class="content-ad"></div>

아래는 코드 설명입니다:

1. ReduxActionTypes라는 Enum을 정의합니다. 여기에는 Redux 설정에서 사용할 수 있는 다양한 작업 유형이 포함됩니다. 이를 통해 작업 유형에서 오타를 방지하고 일관성을 유지할 수 있습니다.

2. 각 작업 유형에 대한 작업 인터페이스를 정의하여 작업 객체의 형태를 구체화합니다.

3. 가능한 작업 유형을 이해하는 데 도움을 주는 조합 작업 유형을 정의합니다.

<div class="content-ad"></div>

# 리듀서 구현

이제 애플리케이션 상태를 관리하기 위한 리듀서를 설정해보겠습니다. 리듀서는 현재 상태와 액션을 가져와 액션 유형에 따라 새로운 상태를 반환하는 함수입니다.

먼저 redux 폴더 내에 reducers라는 새 폴더를 만드세요.

그리고 productReducer.tsx 파일을 생성하세요.

<div class="content-ad"></div>

productReducer는 Redux 스토어에서 제품 데이터를 업데이트하고 검색하는 역할을 맡게 될 것입니다. reducers 폴더 안에 productReducer.tsx 파일을 만들고 다음 코드를 추가해주세요.

```js
import { ActionTypes, ReduxActionTypes } from "../ActionTypes";

export interface ApiDataFetchInitialState {
  data: any;
  loading: boolean;
  error: any;
}

const InitialValues: ApiDataFetchInitialState = {
  data: {},
  loading: false,
  error: null,
};

const productData = (
  state: ApiDataFetchInitialState = InitialValues,
  action: ActionTypes
): ApiDataFetchInitialState => {
  switch (action.type) {
    case ReduxActionTypes.FETCH_PRODUCTS:
      return { ...state, loading: true };
    case ReduxActionTypes.FETCH_PRODUCTS_SUCCESS:
      return { ...state, loading: false, data: action.payload };
    case ReduxActionTypes.FETCH_PRODUCTS_ERROR:
      return { ...state, loading: false, error: action.error };
    default:
      return state;
  }
};

export default productData;
```

rootReducer.tsx 파일을 생성해주세요.

rootReducer는 모든 개별 reducer를 하나의 루트 reducer로 결합하여 Redux 스토어에서 사용될 것입니다. reducers 폴더 안에 rootReducer.tsx 파일을 만들고 다음 코드를 추가해주세요.

<div class="content-ad"></div>

```typescript
import { combineReducers } from "redux";
import productData from "./productReducer";

const rootReducer = combineReducers({
  productData,
});

export type RootState = ReturnType<typeof rootReducer>;

export default rootReducer;
```

# 사가 구현

이제 사가를 구현하여 API에서 데이터를 가져오는 것과 같은 부수 효과를 처리해봅시다. productSaga.tsx와 rootSaga.tsx 두 가지 핵심 파일을 만들겠습니다.

productSaga.tsx 파일을 생성하세요.

<div class="content-ad"></div>

제품Saga.tsx 파일에는 watcher saga 및 worker saga 생성 함수가 포함될 것입니다. 우리는 takeEvery, put 및 call과 같은 유용한 redux-saga 부수 효과를 사용할 것입니다.

redux/sagas 폴더 안에 productSaga.tsx라는 파일을 만들고 다음 코드를 추가하세요:

```js
import {
  FetchProductDataAction,
  FetchProductDataErrorAction,
  FetchProductDataSuccessAction,
  ReduxActionTypes,
} from "../ActionTypes";
import { takeEvery, put, call } from "redux-saga/effects";
import axios from "axios";

function* fetchProductData(action: FetchProductDataAction): Generator<any, void, any> {
  try {
    const id = action.payload;
    const response = yield call(axios.get, `https://fakestoreapi.com/products`);
    yield put <
      FetchProductDataSuccessAction >
      { type: ReduxActionTypes.FETCH_PRODUCTS_SUCCESS, payload: response.data };
  } catch (error) {
    if (error instanceof Error) {
      yield put <
        FetchProductDataErrorAction >
        {
          type: ReduxActionTypes.FETCH_PRODUCTS_ERROR,
          error: error.message,
        };
    } else {
      yield put <
        FetchProductDataErrorAction >
        {
          type: ReduxActionTypes.FETCH_PRODUCTS_ERROR,
          error: "An unknown error occurred",
        };
    }
  }
}
export function* productSaga() {
  yield takeEvery(ReduxActionTypes.FETCH_PRODUCTS, fetchProductData);
}
```

코드 설명:

<div class="content-ad"></div>

fetchProductData 함수는 API에서 제품 데이터를 가져오는 과정을 처리하는 제너레이터 함수를 반환합니다. 이 함수는 특정 지점에서 실행을 일시 중지하여 비동기 작업을 수행하고 결과를 처리할 수 있으며, API 호출의 결과에 따라 Redux 스토어를 업데이트하는 작업을 디스패치합니다.

productSaga 함수는 애플리케이션에서 디스패치된 특정 액션을 감시하고 해당 액션을 처리하기 위해 해당 워커 사가를 트리거합니다.

rootSaga.tsx 생성

redux/sagas 폴더 내에 rootSaga.tsx라는 파일을 만들고 아래 코드를 추가하세요:

<div class="content-ad"></div>

```js
import { all } from "redux-saga/effects";
import { productSaga } from "./productSaga";

export default function* rootSaga() {
  yield all([productSaga()]);
}
```

코드 설명:

- all: all 이펙트는 여러 사가를 동시에 실행합니다. 모든 위쳐 사가를 하나의 루트 사가로 결합하는데 사용됩니다.
- Root Saga (rootSaga): 이 함수는 watchFetchProductData 사가와 다른 Watcher 사가들을 결합합니다.

# 액션 생성자 구현하기

<div class="content-ad"></div>

좋아요! 계속해서 UI로부터 액션을 전달하는 역할을 담당하는 액션 생성자를 만들어 봅시다. 이러한 액션 생성자는 Redux-Saga를 트리거하여 필요한 부수효과를 수행하는 데 도움을 줄 것입니다.

redux 폴더 내에 actions라는 새 폴더를 만들고 index.tsx라는 새 파일을 생성하세요.

```js
import { ReduxActionTypes } from "../ActionTypes";

export const fetchProductData = (data?: any) => {
  return {
    type: ReduxActionTypes.FETCH_PRODUCTS,
    payload: data,
  };
};
```

코드 설명:

<div class="content-ad"></div>

fetchProductData 함수는 FETCH_PRODUCTS 타입의 액션을 생성하고 선택적 데이터 매개변수를 사용하여 액션 페이로드에 포함합니다.

이 액션이 디스패치되면 watcher 사가에서 FETCH_PRODUCTS 타입을 트리거하고 제품 데이터를 가져오는 프로세스를 시작합니다.

# 프론트엔드 컴포넌트에서 데이터를 디스패치하고 표시하기

이제 우리가 스토어, 리듀서, 사가 및 액션 생성자를 갖췄으니 React 컴포넌트에서 액션을 디스패치하여 제품 데이터를 가져오는 방법을 살펴봅시다.

<div class="content-ad"></div>

아래 코드로 App.js 코드를 교체해주세요.

```js
import { useEffect } from "react";
import "./App.css";
import { useDispatch } from "react-redux";
import { fetchProductData } from "./redux/actions";
import { useSelector } from "react-redux";
import { RootState } from "./redux/reducers/rootReducer";

function App() {
  const dispatch = useDispatch();

  useEffect(() => {
    dispatch(fetchProductData());
  }, [dispatch]);
  const { data, loading, error } = useSelector((state: RootState) => state.productData);
  return (
    <div style={{ padding: "10px" }}>
      {loading && <div>Loading...</div>}
      {error && <div>{error}</div>}

      <div className="container">
        {data.length > 0 &&
          data.map((item) => {
            return (
              <div className="card">
                <h2>{item.title}</h2>
                <p>{item.description}</p>
                <img src={item.image} alt={item.title} />
              </div>
            );
          })}
      </div>
    </div>
  );
}

export default App;
```

코드 설명:

이 코드는 초기 페이지 로드 중에 useEffect 훅을 사용하여 fetchProductData 액션을 이용하여 제품 데이터를 가져오는 React 컴포넌트를 설정합니다.

<div class="content-ad"></div>

컴포넌트가 마운트될 때 액션을 디스패치하는 데 useDispatch를 사용하고, Redux 스토어에서 가져온 데이터에 액세스하여 표시하는 데 useSelector를 사용합니다.

UI에는 로딩 및 오류 상태를 처리하는 조건부 렌더링이 포함되어 있으며 제품 데이터를 구조화된 형식으로 표시합니다.

# 어플리케이션 실행하기

좋아요. 이제 우리의 개발을 마쳤습니다. 어플리케이션을 실행할 시간입니다. 터미널로 이동하여 다음 명령어를 사용하여 어플리케이션을 실행하세요.

<div class="content-ad"></div>

```js
npm run dev
```

브라우저를 열고 http://localhost:5173/로 이동하면 다음 UI를 볼 수 있습니다.

<img src="/assets/img/2024-07-09-ConfigureRedux-sagainaReactTypescriptapplication_3.png" />

그 외에도 브라우저 확장 프로그램 `redux-devtools`를 설치하면 Chrome에서 다음과 같이 store를 검사할 수 있습니다.

<div class="content-ad"></div>

![이미지](/assets/img/2024-07-09-ConfigureRedux-sagainaReactTypescriptapplication_4.png)

# 마무리

만약 핵심 개념을 놓치거나 이 설정을 이해하는 데 어려움을 겪었다면, Redux-Saga를 사용한 상태 관리의 기본 개념을 자세히 다루는 이 기사의 첫 부분을 다시 살펴보기를 권장합니다.

# 추가 자료 및 참고문헌

<div class="content-ad"></div>

가짜 상점 API 엔드포인트: [링크](https://fakestoreapi.com/)

제너레이터 함수: [링크](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function*)

리덕스 사가 문서: [링크](https://redux-saga.js.org/docs/introduction/GettingStarted)

Vite 개발자 가이드: [링크](https://vitejs.dev/guide/)
