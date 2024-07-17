---
title: "React useRef 사용 예시 및 사례"
description: ""
coverImage: "/it-shar.ing/assets/no-image.jpg"
date: 2024-07-12 19:07
ogImage:
  url: /it-shar.ing/assets/no-image.jpg
tag: Tech
originalTitle: "React useRef Use Cases and Examples"
link: "https://medium.com/@zahidbashirkhan/react-useref-use-cases-with-examples-d7680d48a6e1"
---

React.js의 useRef Hook을 사용하는 이유와 방법에 대한 설명

React.js에서 useRef는 요소나 값에 대한 가변 참조를 만들 수 있게 해주는 Hook입니다. useState Hook과 달리 useRef를 업데이트하더라도 컴포넌트를 다시 렌더링하지 않습니다. useRef는 주로 DOM 요소에 직접 접근하거나 영구적인 값 저장, 또는 다시 렌더링을 일으키지 않아야 하는 값들과 작업하는 데 사용됩니다.

useRef의 기본 구문은 다음과 같습니다:

```js
const refContainer = useRef(initialValue);
```

<div class="content-ad"></div>

# useRef의 주요 사용 사례

## 1. DOM 요소에 액세스하기

useRef의 가장 흔한 사용 사례 중 하나는 DOM 요소에 액세스하는 것입니다. document.getElementById나 다른 DOM 쿼리를 사용하는 대신 useRef를 사용하여 컴포넌트 내에서 요소를 직접 참조할 수 있습니다.

```js
import React, { useRef } from "react";

const ExampleComponent = () => {
  const inputRef = useRef(null);

  const handleClick = () => {
    // 버튼 클릭 시 입력 요소에 포커스
    inputRef.current.focus();
  };

  return (
    <div>
      <input ref={inputRef} type="text" />
      <button onClick={handleClick}>입력에 포커스</button>
    </div>
  );
};
```

<div class="content-ad"></div>

## 2. 이전 값 저장하기

이전 렌더링 시에 유지되는 값을 저장하려면 useRef를 사용할 수 있습니다. 이 값을 업데이트할 때 다시 렌더링을 트리거하지 않습니다. 불필요한 렌더링을 트리거하지 않고 이전 값들을 추적하는 데 유용합니다.

```js
import React, { useEffect, useRef } from "react";

const PreviousValueComponent = ({ value }) => {
  const prevValueRef = useRef();

  useEffect(() => {
    prevValueRef.current = value;
  }, [value]);

  return (
    <div>
      <p>현재 값: {value}</p>
      <p>이전 값: {prevValueRef.current}</p>
    </div>
  );
};
```

## 3. 외부 라이브러리 사용하기

<div class="content-ad"></div>

가끔은 데이터를 가변 참조로 기대하는 외부 라이브러리를 통합할 때 useRef를 사용하면 편리합니다.

```js
import React, { useEffect, useRef } from "react";
import externalLibrary from "some-external-library";

const ExternalLibraryComponent = () => {
  const dataRef = useRef([]);

  useEffect(() => {
    // 최신 데이터로 외부 라이브러리를 업데이트합니다.
    externalLibrary.updateData(dataRef.current);
  }, []);

  // ... 컴포넌트의 나머지 부분
};
```

useRef는 다시 렌더링을 트리거하지 않으므로 컴포넌트를 업데이트하고 변경 사항을 UI에 반영해야 하는 경우 useState를 대신 사용해야 합니다. useRef는 컴포넌트를 렌더링하는 데 영향을 미치지 않아야 하는 가변 값을 또는 참조를 관리해야 할 때에만 사용하세요.

# useImperativeHandle

<div class="content-ad"></div>

useImperativeHandle은 React 훅 중 하나로, 자식 컴포넌트에 ref를 사용할 때 부모 컴포넌트에 노출되는 인스턴스 값을 사용자 정의할 수 있게 해줍니다. 이 훅은 주로 forwardRef와 useRef와 함께 사용되어 부모로부터 자식 컴포넌트의 메서드나 속성에 명시적이고 더 많은 제어를 제공하는 방식으로 상호 작용하는 데 사용됩니다.

아래는 useRef, useImperativeHandle, 그리고 forwardRef를 사용하여 프로그래밍 방식으로 입력란에 초점을 맞추는 메서드를 노출하는 간단한 커스텀 입력 컴포넌트를 만드는 예제입니다:

```js
// Child Component (CustomInput.js)

import React, { useRef, useImperativeHandle, forwardRef } from "react";

const CustomInput = forwardRef((props, ref) => {
  const inputRef = useRef(null);

  // 부모 컴포넌트에 'focusInput' 메서드 노출
  useImperativeHandle(ref, () => ({
    focusInput: () => {
      inputRef.current.focus();
    },
  }));

  return <input ref={inputRef} type="text" placeholder={props.placeholder} />;
});

export default CustomInput;
```

위의 자식 컴포넌트에서는 입력 요소에 대한 참조를 만들기 위해 useRef를 사용합니다. 그런 다음 useImperativeHandle을 사용하여 focusInput이라는 메서드를 ref를 통해 부모 컴포넌트에 노출합니다. 이를 통해 부모 컴포넌트는 자식 컴포넌트의 ref에서 focusInput을 호출하여 입력 요소에 프로그래밍 방식으로 초점을 맞출 수 있습니다.

<div class="content-ad"></div>

```js
//Parent Component

import React, { useRef } from "react";
import CustomInput from "./CustomInput";

const ParentComponent = () => {
  const inputRef = useRef(null);

  const handleFocusButtonClick = () => {
    // Call the 'focusInput' method of the child component
    if (inputRef.current) {
      inputRef.current.focusInput();
    }
  };

  return (
    <div>
      {/* Child component with the ref */}
      <CustomInput ref={inputRef} placeholder="Enter your name" />
      <button onClick={handleFocusButtonClick}>Focus Input</button>
    </div>
  );
};

export default ParentComponent;
```

부모 컴포넌트에서는 useRef를 사용하여 참조 변수(inputRef)를 생성하고, 이를 통해 자식 컴포넌트의 focusInput 메서드와 상호 작용할 수 있습니다. "Focus Input" 버튼을 클릭하면 handleFocusButtonClick 함수가 실행됩니다. 이 함수 내에서 자식 컴포넌트의 ref를 통해 focusInput 메서드를 호출하여 입력 요소에 초점을 맞출 수 있습니다.

이 예시는 useRef와 useImperativeHandle을 함께 사용하여 부모 컴포넌트에서 자식 컴포넌트와 상호 작용하는 더욱 제어된 인터페이스를 생성하는 방법을 보여줍니다.

# More Examples

<div class="content-ad"></div>

## useRef를 사용하여 요소 스타일 변경

```js
import React, { useRef } from "react";

const StyleChangeComponent = () => {
  const divRef = useRef(null);

  const handleColorChange = () => {
    divRef.current.style.backgroundColor = "lightblue";
  };

  return (
    <div ref={divRef}>
      <p>This is a div element whose style can be changed.</p>
      <button onClick={handleColorChange}>Change Color</button>
    </div>
  );
};
```

이 예시에서는 useRef를 사용하여 참조 변수 divRef를 생성하고 `div` 요소에 연결했습니다. "색상 변경" 버튼을 클릭하면 handleColorChange 함수가 divRef의 current 속성을 사용하여 `div` 요소의 배경색을 직접 변경합니다.

## useRef를 사용하여 입력 값 변경

<div class="content-ad"></div>

```js
import React, { useRef } from "react";

const InputChangeComponent = () => {
  const inputRef = useRef(null);

  const handleChangeValue = () => {
    inputRef.current.value = "새 값";
  };

  return (
    <div>
      <input ref={inputRef} type="text" />
      <button onClick={handleChangeValue}>입력 값 변경</button>
    </div>
  );
};
```

이 예시에서 useRef를 사용하여 inputRef라는 참조를 만들고 `input` 요소에 연결합니다. "입력 값 변경" 버튼을 클릭하면 handleChangeValue 함수가 inputRef의 current 속성을 사용하여 `input` 요소의 값을 직접 변경합니다.

## useRef와 useImperativeHandle을 사용하여 자식 컴포넌트의 `h1` 요소 텍스트 변경하기

```js
// 자식 컴포넌트

import React, { useRef, useImperativeHandle, forwardRef } from "react";

const CustomHeader = forwardRef((props, ref) => {
  const headerRef = useRef(null);

  // 'changeHeaderText' 메서드를 부모 컴포넌트에 노출합니다
  useImperativeHandle(ref, () => ({
    changeHeaderText: (text) => {
      headerRef.current.textContent = text;
    },
  }));

  return <h1 ref={headerRef}>{props.text}</h1>;
});

export default CustomHeader;
```

<div class="content-ad"></div>

이 예제에서는 useRef 훅을 사용하여 자식 컴포넌트(CustomHeader.js)에서 참조 headerRef를 생성하고 `h1` 요소에 연결합니다. useImperativeHandle 훅을 사용하여 ref를 통해 부모 컴포넌트에 changeHeaderText라는 메서드를 노출할 수 있습니다. 부모 컴포넌트(ParentComponent)는 이 메서드를 사용하여 자식 컴포넌트의 `h1` 요소의 텍스트를 변경할 수 있습니다. 다시 렌더링을 유발하지 않고요.

# 마무리

<div class="content-ad"></div>

useRef은 React.js에서 개발자가 DOM 요소에 효율적으로 액세스하고 수정하며 값을 보존하고 상태를 관리할 수 있도록 하는 강력한 후크입니다. 불필요한 다시 렌더링을 유발시키지 않고 기능적 지속성을 제공합니다. 이 다재다능성으로 인해 개발자 툴킷에서 꼭 필요한 도구이며 React 애플리케이션에서 제어와 성능 최적화를 가능하게 합니다. 본문에서 따라하며 사용 사례와 실용적 구현을 숙지하면 useRef의 모든 잠재력을 활용하여 보다 견고하고 효율적인 React 컴포넌트를 구축할 수 있습니다.
