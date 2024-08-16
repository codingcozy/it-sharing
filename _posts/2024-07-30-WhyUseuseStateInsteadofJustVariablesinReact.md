---
title: "React에서 변수 대신 useState를 사용해야 하는 이유"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-07-30 16:52
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Why Use useState Instead of Just Variables in React"
link: "https://dev.to/homayunmmdy/why-use-usestate-instead-of-just-variables-in-react-2heo"
isUpdated: true
updatedAt: 1723817084545
---



# React에서 단순 변수 대신 useState를 사용하는 이유

React에서 왜 useState를 사용하는지 궁금한 적이 있나요? 간단한 카운터 예제를 통해 이 개념을 살펴보겠습니다.

## 간단한 카운터 예제

두 개의 버튼(1을 증가시키는 버튼, 1을 감소시키는 버튼)이 있는 기본 카운터가 있다고 상상해보세요. useState를 사용하여 이 카운터를 생성하면 완벽하게 작동합니다. 그러나 일반 변수를 사용하려고 시도하면 예상대로 작동하지 않습니다.

<div class="content-ad"></div>

```js
import React, { useState } from 'react';

function Counter() {
  // useState를 사용하여 상태 변수 생성
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increase</button>
      <button onClick={() => setCount(count - 1)}>Decrease</button>
    </div>
  );
}

export default Counter;
```

## useState란 무엇인가요?

useState는 React에서 함수형 컴포넌트에 상태를 추가할 수 있게 해주는 훅입니다. 상태는 컴포넌트가 사용하여 시간이 지남에 따라 기억하고 업데이트하는 메모리와 같습니다.

## 왜 변수가 제대로 작동하지 않을까요?

<div class="content-ad"></div>

변수를 사용하는 것이 작동하지 않는 이유는 React가 useState로 관리되는 상태와 같이 일반 변수의 변경을 추적하지 않기 때문입니다. 증가 또는 감소 버튼을 클릭할 때 useState는 React에게 상태가 변경되었음을 알려줍니다. 그 후 React는 컴포넌트를 다시 렌더링하고 카운트를 업데이트합니다.

그러나 일반 변수를 사용하는 경우 React는 변경 사항을 인식하지 못하므로 카운트를 업데이트하지 않습니다.

```js
import React from 'react';

function Counter() {
  // 일반 변수 사용
  let count = 0;

  const increase = () => {
    count += 1;
  };

  const decrease = () => {
    count -= 1;
  };

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increase}>Increase</button>
      <button onClick={decrease}>Decrease</button>
    </div>
  );
}

export default Counter;
```

## 결론

<div class="content-ad"></div>

제가 useState가 React에서 상태 관리에 얼마나 중요한지 이해했기를 바랍니다. 이를 통해 React가 변경 사항을 추적하고 컴포넌트를 업데이트할 수 있도록 합니다. 시간 내 주셔서 감사합니다. 곧 다시 만나요!