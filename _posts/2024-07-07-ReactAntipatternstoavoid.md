---
title: "React 사용 시 피해야 할 안티 패턴들"
description: ""
coverImage: "/assets/img/2024-07-07-ReactAntipatternstoavoid_0.png"
date: 2024-07-07 19:05
ogImage:
  url: /assets/img/2024-07-07-ReactAntipatternstoavoid_0.png
tag: Tech
originalTitle: "React: Anti patterns to avoid."
link: "https://medium.com/@shriharim006/react-anti-patterns-to-void-5dbc930a714a"
---

React에서의 안티-패턴은 비효율적이거나 문제가 있는 코드로 이어질 수 있는 공통적인 오류 또는 관행을 가리킵니다. 이러한 패턴을 피하는 것은 깨끗하고 확장 가능하며 유지보수 가능한 React 코드베이스를 유지하는 데 도움이 됩니다.

![React 안티-패턴](/assets/img/2024-07-07-ReactAntipatternstoavoid_0.png)

주의해야 할 몇 가지 React 안티-패턴:

## 인덱스를 Key로 사용하기:

<div class="content-ad"></div>

리스트에서 원소에 배열 인덱스를 키로 사용하면 문제가 발생할 수 있습니다. 리스트가 수정될 때 불필요한 재렌더링을 유발하고 React가 UI를 올바르게 업데이트하는 데 어렵게 만들 수 있습니다.

```js
// 안티 패턴
{items.map((item, index) => (
  <Item key={index} data={item} />
))}

대신 각 항목에 고유 식별자를 키로 사용하세요.
```

## PropTypes를 사용하지 않은 경우 (Typescript를 사용하지 않는 경우):

PropTypes를 빠뜨리면 컴포넌트의 예상 props를 이해하는 데 어려움이 있을 수 있습니다. PropTypes는 잠재적인 버그를 찾아내고 컴포넌트 사용에 대한 문서로 작용합니다.

<div class="content-ad"></div>

```js
// 안티-패턴
function MyComponent(props) {
  // ...
}
```

PropTypes를 사용하여 props의 예상 타입을 정의하세요:

```js
import PropTypes from "prop-types";

function MyComponent(props) {
  // ...
}

MyComponent.propTypes = {
  someProp: PropTypes.string.isRequired,
};
```

## 중첩된 콜백 함수:

<div class="content-ad"></div>

깊게 중첩된 콜백 함수는 코드를 읽고 유지하기 어렵게 만들 수 있으므로 피하는 것이 좋습니다. 복잡한 로직을 더 작고 관리하기 쉬운 함수로 분해하는 것을 고려해보세요.

```js
// 안티 패턴
const fetchDataApi = () => {
  fetchData().then((data) => {
    process(data).then((result) => {
      updateState(result);
    });
  });
};
```

대신에 보다 가독성 있게 따로 함수를 만들거나 async/await를 사용하세요:

```js
const fetchDataAndProcess = async () => {
  const data = await fetchData();
  const result = await process(data);
  updateState(result);
};
```

<div class="content-ad"></div>

## 가변 상태:

상태를 직접 수정하면 예기치 못한 동작이 발생할 수 있습니다. 특히 새 상태가 이전 상태에 의존하는 경우에는 항상 setState 함수를 사용하여 상태를 업데이트하십시오.

```js
// 안티 패턴
const [myObject, setMyObject] = useState({ property: "" });

const handleClick = () => {
  // 상태를 직접 변경
  myObject.property = "새 값";
  setMyObject(myObject);
};
```

정확한 상태 업데이트를 보장하기 위해 setState의 함수형 형식을 사용하세요:

<div class="content-ad"></div>

```js
const [myObjcect, setMyObject] = useState({ property: "" });

const handleClick = () => {
  setMyObject((prevObject) => ({
    ...prevObject,
    property: "new value",
  }));
};
```

## 과도한 컴포넌트 상태:

모든 데이터 조각에 대해 컴포넌트 상태를 무분별하게 사용하지 마세요. 상태를 절약하고 특정 데이터 조각이 컴포넌트 상태의 일부이어야 하는지 확인하거나 더 높은 수준에서 관리할 수 있는지(예: 컨텍스트 또는 Redux를 통해) 고려하세요.

## 컨텍스트 남용:

<div class="content-ad"></div>

Context API은 강력하지만 모든 것에 대해 과용하면 복잡하고 유지보수하기 어려운 코드베이스로 이어질 수 있습니다. 전역 상태를 공유해야 하는 경우에만 Context를 사용하세요.

## Uncontrolled Components:

제어 컴포넌트에서는 상태가 React에 의해 관리되지만, 비제어 컴포넌트는 DOM에 의존합니다. 비제어 컴포넌트는 예측할 수 없는 동작을 유발할 수 있고 일반적으로 관리하기 어렵습니다.

```js
// 안티 패턴
<input type="text" ref={inputRef} />
```

<div class="content-ad"></div>

리액트를 통해 상태를 관리하여 제어 구성 요소를 사용하세요:

```js
const [inputValue, setInputValue] = useState("");

<input type="text" value={inputValue} onChange={(e) => setInputValue(e.target.value)} />;
```

# 결론:

이 방법론은 엄격한 규칙이 아닌 가이드라인이므로 예외가 적절한 경우도 있을 수 있습니다. 목표는 이해, 유지보수 및 디버깅이 쉬운 코드를 작성하는 것입니다.
