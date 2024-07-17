---
title: "React에서 setInterval을 사용하는 방법"
description: ""
coverImage: "/assets/img/2024-07-09-How-tousesetIntervalinReact_0.png"
date: 2024-07-09 17:02
ogImage:
  url: /assets/img/2024-07-09-How-tousesetIntervalinReact_0.png
tag: Tech
originalTitle: "How-to use setInterval in React"
link: "https://medium.com/@jonjamesdesigns/how-to-use-setinterval-in-react-10612f122027"
---

<img src="/assets/img/2024-07-09-How-tousesetIntervalinReact_0.png" />

# setInterval에 대한 참조 저장

React 구성 요소 내에서 setInterval을 사용할 때 고려해야 할 첫 번째 사항은 나중에 지울 수 있도록 interval에 대한 참조를 어디에 저장할지입니다.

일반적인 JavaScript에서는 일반적으로 다음과 같이 보입니다:

<div class="content-ad"></div>

```js
var intervalId = setInterval(() => {
  // 매 초마다 작업 수행..
}, 1000);

clearInterval(intervalId);
```

MDN 문서에 따르면 setInterval()은 숫자형 intervalId를 반환합니다.

setInterval이 고유한 숫자 id를 반환한다는 것을 알게 되었으므로, 이 id에 대한 참조를 React 컴포넌트 내에서 저장해야 합니다. 이 작업은 React의 내장 useRef 훅을 사용하여 수행할 수 있습니다:

```js
import { useRef } from "react";

const IntervalComponent = () => {
  const intervalId = useRef(null);
};
```

<div class="content-ad"></div>

# setInterval을 트리거하는 방법

이제 React ref를 설정하여 interval id를 저장할 수 있게 되었으므로, 실제 setInterval 함수를 정의할 차례입니다. 어떤 방식으로 interval을 시작할지에 따라서 작업 방식이 달라집니다.

이벤트를 통해 setInterval 시작하기:

```js
import { useCallback, useRef, useState } from "react";

const IntervalComponent = () => {
  const intervalId = useRef(null);
  const [count, setCount] = useState(0);

  const startInterval = useCallback(() => {
    intervalId.current = setInterval(() => {
      if (count < 5) {
        setCount((count) => count + 1);
      } else {
        clearInterval(intervalId.current);
      }
    }, 1000);
  }, [count]);

  return (
    <>
      <p>{count}</p>
      <button onClick={startInterval}>Start interval</button>
    </>
  );
};
```

<div class="content-ad"></div>

이벤트 트리거로 간격을 시작할 때는, setInterval이 아직 실행 중인 상태에서 컴포넌트가 언마운트되는 경우를 대비하여 useEffect를 설정하는 것이 중요합니다:

```js
useEffect(() => {
  return () => {
    clearInterval(intervalId.current);
  };
}, []);
```

컴포넌트가 마운트될 때 setInterval을 시작하세요:

```js
import { useEffect, useRef, useState } from "react";

const IntervalComponent = () => {
  const intervalId = useRef(null);
  const [count, setCount] = useState(0);

  useEffect(() => {
    intervalId.current = setInterval(() => {
      if (count < 5) {
        setCount((count) => count + 1);
      } else {
        clearInterval(intervalId.current);
      }
    }, 1000);
    return () => {
      clearInterval(intervalId.current);
    };
  }, [count]);

  return <p>{count}</p>;
};
```

<div class="content-ad"></div>

# React 카운트다운 타이머를 만들기 위해 setInterval을 사용해봤습니다

React에서 setInterval을 어떻게 사용하는지 알게 되었으니, 그 중 하나인 인기 있는 사용 사례 중 하나는 카운트다운 타이머를 만드는 것입니다.

아래는 10초 카운트다운 타이머를 만드는 예시입니다:

```js
import { useCallback, useEffect, useRef, useState } from "react";

const CountdownTimer: FC = () => {
  const intervalId = useRef(null);

  const [timerEnd, setTimerEnd] = (useState < Date) | (undefined > undefined);

  const [timeRemaining, setTimeRemaining] = (useState < number) | (undefined > 0);

  const startTimer = useCallback(() => {
    setTimerEnd(new Date(Date.now() + 10 * 1000));
  }, []);

  useEffect(() => {
    if (timerEnd !== undefined) {
      intervalId.current = setInterval(() => {
        const delta = timerEnd.getTime() - Date.now();
        if (delta < 0) {
          clearInterval(intervalId.current);
          setTimerEnd(undefined);
        }
        setTimeRemaining(Math.round(delta / 1000));
      }, 1000);
    }
  }, [timerEnd]);

  useEffect(() => {
    return () => {
      clearInterval(intervalId.current);
    };
  }, []);

  return (
    <>
      {!timerEnd && <button onClick={startTimer}>타이머 시작</button>}
      {timerEnd && <p>{timeRemaining}</p>}
    </>
  );
};
```

<div class="content-ad"></div>

안녕하세요!

시니어 프론트엔드 개발자를 찾고 계시다면, 앱과 웹사이트를 디자인하고 개발하는 데 경험이 풍부한 몇 년 경력의 저에게 언제든 연락해 주세요.
