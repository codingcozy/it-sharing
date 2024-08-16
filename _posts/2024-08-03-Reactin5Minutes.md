---
title: "5분 안에 배워보는 React 팁"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-03 20:46
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "React in 5 Minutes"
link: "https://dev.to/lokesh_singh/react-in-5-minutes-2bk"
isUpdated: true
updatedAt: 1723816545985
---



리액트는 사용자 인터페이스를 구축하기 위한 강력한 JavaScript 라이브러리입니다. 프론트엔드 개발을 보다 빠르고 쉽게 만들기 위해 설계되었습니다. 함께 간단하고 친근하게 리액트의 컨셉을 알아보도록 하죠!

### 1. 컴포넌트

컴포넌트는 리액트 애플리케이션의 구성 요소입니다. 재사용 가능한 코드 조각으로, 사용자 인터페이스의 일부를 반환하는 역할을 합니다.

예시:

<div class="content-ad"></div>

```js
기능 Welcome(props) {
  return <h1>안녕, {props.name}!</h1>;
}
```

참고:

- 구성 요소는 기능적일 수도 있고 클래스 기반일 수도 있습니다.
- 가능한 경우 함수형 구성 요소를 사용하십시오. 그들은 더 간단하고 관리하기 쉽습니다.

### 2. JSX


<div class="content-ad"></div>

JSX는 React에서 사용되는 HTML과 비슷한 구문 확장 기능입니다. React 컴포넌트를 쉽고 직관적으로 작성할 수 있게 도와줍니다.

예시:

```js
const element = <h1>Hello, world!</h1>;
```

참고:

<div class="content-ad"></div>

- JSX를 사용하면 JavaScript 내에서 HTML과 비슷한 코드를 작성할 수 있어요.
- React에 의해 JavaScript로 변환되기 때문에 UI 요소를 생성하는 데 사용할 수 있어요.

### 3. 속성

속성은 한 컴포넌트에서 다른 컴포넌트로 데이터를 전달하는 방식이에요. 그것들을 함수에 전달하는 인수처럼 생각해보세요.

예시:

<div class="content-ad"></div>

```js
function Greeting(props) {
  return <p>{props.message}</p>;
}
```

참고 사항:

- Props는 읽기 전용이며 받는 컴포넌트에서 수정할 수 없습니다.
- 이들은 컴포넌트를 재사용 가능하고 동적으로 만드는 데 도움이 됩니다.

### 4. State


<div class="content-ad"></div>

상태(State)는 시간이 지남에 따라 변하는 데이터를 관리하는 데 사용됩니다. 프롭스(props)와 달리 상태는 컴포넌트 내부에서 관리됩니다.

예시:

```js
function Counter() {
  const [count, setCount] = useState(0);

  return (
    <>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increase</button>
    </>
  );
}
```

참고:

<div class="content-ad"></div>

- State은 함수형 컴포넌트에서 useState 훅과 함께 사용됩니다.
- State를 사용하면 컴포넌트가 사용자 입력을 기억하고 반응할 수 있습니다.

### 5. 라이프사이클 메소드

라이프사이클 메소드는 컴포넌트의 생명주기의 여러 단계에서 호출되는 함수들입니다. 주로 클래스 컴포넌트에서 사용되지만 훅으로 함수형 컴포넌트에서 시뮬레이션할 수 있습니다.

단계:

<div class="content-ad"></div>

- 마운트: 컴포넌트가 DOM에 추가될 때 발생합니다.
- 업데이트: 컴포넌트의 상태 또는 프롭이 변경될 때 발생합니다.
- 언마운트: 컴포넌트가 DOM에서 제거될 때 발생합니다.

### 6. 훅

훅은 함수형 컴포넌트에서 상태 및 다른 React 기능을 사용할 수 있게 해주는 특별한 함수입니다.

예시:

<div class="content-ad"></div>

```js
import { useState, useEffect } from 'react';

function Timer() {
  const [time, setTime] = useState(0);

  useEffect(() => {
    const interval = setInterval(() => {
      setTime(time + 1);
    }, 1000);
    return () => clearInterval(interval);
  }, [time]);

  return <p>Time: {time}s</p>;
}
```

노트:

- 일반적인 후크: useState, useEffect, useContext 등
- 후크는 함수 컴포넌트 내에서만 사용할 수 있습니다.

### 7. Context API


<div class="content-ad"></div>

컨텍스트 API는 컴포넌트 트리를 통해 데이터를 전달하는 방법으로, 각 레벨마다 수동으로 props를 전달하지 않아도 됩니다.

예시:

```js
const ThemeContext = createContext('light');

function App() {
  return (
    <ThemeContext.Provider value="dark">
      <Toolbar />
    </ThemeContext.Provider>
  );
}

function Toolbar() {
  return (
    <ThemeContext.Consumer>
      {value => <p>Current theme: {value}</p>}
    </ThemeContext.Consumer>
  );
}
```

참고:

<div class="content-ad"></div>

- 컨텍스트는 테마, 사용자 정보 등과 같은 전역 데이터에 유용합니다.
- useContext 훅을 사용하면 간편한 Consumer 대신 사용할 수 있습니다.

### 8. Fragments

프래그먼트는 추가적인 노드 없이 여러 요소를 그룹화할 수 있습니다.

예시:

<div class="content-ad"></div>

```js
function App() {
  return (
    <>
      <h1>Title</h1>
      <p>Paragraph</p>
    </>
  );
}
```

참고사항:

- Fragment를 사용하면 DOM이 더 깔끔해지고 코드가 더 읽기 쉬워집니다.
- 빈 태그 `` `/` 는 React.Fragment의 축약형으로 사용됩니다.

### 9. Keys

<div class="content-ad"></div>

React에서 키는 항목이 변경되었는지, 추가되었는지 또는 제거되었는지 식별하는 데 도움이 됩니다. 리스트를 효율적으로 관리하는 데 있어서 매우 중요합니다.

예시:

```js
const items = ['Apple', 'Banana', 'Cherry'];
const listItems = items.map((item, index) => <li key={index}>{item}</li>);
```

참고:

<div class="content-ad"></div>

- 키는 고유하고 안정적이어야 합니다.
- 리스트 항목이 순서가 바뀔 수 있다면 인덱스를 키로 사용하는 것을 피하십시오.

### 10. 제어 컴포넌트 vs. 비제어 컴포넌트

제어 컴포넌트는 React가 폼 데이터를 제어하는 경우이며, 비제어 컴포넌트는 DOM에 의존합니다.

제어 컴포넌트의 예시:

<div class="content-ad"></div>

```js
function Form() {
  const [name, setName] = useState('');

  return (
    <>
      <input value={name} onChange={(e) => setName(e.target.value)} />
      <p>Your name: {name}</p>
    </>
  );
}
```

Uncontrolled Component Example:

```js
function Form() {
  const nameRef = useRef();

  return (
    <>
      <input ref={nameRef} />
      <button onClick={() => alert(nameRef.current.value)}>Alert Name</button>
    </>
  );
}
```

참고:

<div class="content-ad"></div>

- 제어 컴포넌트는 폼 데이터에 대해 더 나은 제어를 제공합니다.
- 비제어 컴포넌트는 기존의 비-React 코드와 쉽게 통합할 수 있습니다.

### 결론

React는 UI를 관리 가능한 조각으로 분해하고 코드를 더 재사용 가능하게 만드는 데에 관한 것입니다. 이러한 개념을 연습하고 프로젝트를 만들면 곧 React 전문가가 될 것입니다!

이 게시물을 즐겼다면 좋아요를 눌러주시고 공유하며 계속 학습하시기 바랍니다. 계속 연락을 유지해주세요.

<div class="content-ad"></div>

주석: 필수로 알아야 할 중요 내용이나 개념이 빠졌다면
알려주세요.