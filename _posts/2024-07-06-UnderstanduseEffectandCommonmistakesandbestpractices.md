---
title: "useEffect 이해하기 흔히하는 실수와 최고의 팁 "
description: ""
coverImage: "/it-shar.ing/assets/no-image.jpg"
date: 2024-07-06 10:03
ogImage:
  url: /it-shar.ing/assets/no-image.jpg
tag: Tech
originalTitle: "Understand useEffect and Common mistakes and best practices"
link: "https://medium.com/@dushyanthak/mastering-useeffect-common-mistakes-and-best-practices-d0c9c199e73a"
---

# useEffect이란 무엇인가요?

실수와 해결책에 대해 들어가기 전에, useEffect가 무엇을 하는지 간단히 살펴보겠습니다. useEffect를 사용하면 함수 컴포넌트에서 API 호출, 구독, 또는 수동으로 DOM을 변경하는 등의 사이드 이펙트를 수행할 수 있습니다. 이는 두 개의 인수를 취합니다:

- 이펙트 콜백: 사이드 이펙트를 포함하는 함수입니다.
- 의존성 배열: 변경되면 효과를 다시 트리거할 의존성의 배열입니다.

# useEffect의 일반적인 사용법

<div class="content-ad"></div>

- 데이터 가져오기: 컴포넌트가 마운트될 때 데이터를 가져와서 상태에 설정합니다.
- 구독 설정: 데이터 스트림을 구독하고 나중에 정리(clean up)합니다.
- 변화에 대응하기: 특정 상태나 프롭이 변경될 때 부작용(side effects)을 다시 실행합니다.

# 코드 예시

다음은 컴포넌트가 마운트될 때 데이터를 가져오는 데 useEffect를 사용하는 방법입니다:

```js
import { useEffect, useState } from "react";

function UserDataComponent() {
  const [userData, setUserData] = useState(null);

  useEffect(() => {
    fetch("https://api.example.com/user")
      .then((response) => response.json())
      .then((data) => setUserData(data));
  }, []); // 빈 배열은 이 효과가 마운트되고 언마운트될 때만 실행됨을 의미합니다.

  return <div>{userData ? `안녕하세요, ${userData.name}님!` : "로딩 중..."}</div>;
}
```

<div class="content-ad"></div>

# 흔한 실수와 그 해결 방법

1. 데이터를 잘못 변환하기

실수: 매 렌더링마다 데이터를 계속해서 변환하여 성능 문제를 야기할 수 있습니다.

해결 방법:

<div class="content-ad"></div>

```js
import { useEffect, useState } from "react";

function DataTransformComponent({ inputData }) {
  const [transformedData, setTransformedData] = useState(null);

  useEffect(() => {
    const data = transformData(inputData); // Assuming transformData is a costly operation
    setTransformedData(data);
  }, [inputData]); // Transform only when inputData changes

  return <div>{transformedData}</div>;
}
```

이렇게 하면 데이터가 변할 때만 변환되므로 불필요한 계산을 방지할 수 있습니다.

2. 이벤트 처리를 잘못하는 경우

잘못된 점: 필요 없어진 이벤트 리스너를 제거하지 않아 메모리 누수가 발생할 수 있습니다.

<div class="content-ad"></div>

해결책:

```js
import { useEffect } from "react";

function EventHandlingComponent() {
  useEffect(() => {
    const handleResize = () => console.log("Window resized");
    window.addEventListener("resize", handleResize);

    return () => window.removeEventListener("resize", handleResize);
  }, []); // mount 시 한 번만 설정

  return <div>창 크기를 조정하고 콘솔을 확인해보세요.</div>;
}
```

이 패턴을 사용하면 컴포넌트가 언마운트될 때 이벤트 리스너가 정리됩니다.

3. 데이터를 무심코 가져오기

<div class="content-ad"></div>

잘하셨어요! 비동기 작업 후 상태 값을 설정할 때 컴포넌트의 마운트 상태를 무시한 실수를 하지 않도록 수정했네요.
아래는 수정된 코드입니다.

```js
import { useEffect, useState } from "react";

function FetchDataComponent() {
  const [data, setData] = useState(null);

  useEffect(() => {
    let isMounted = true;

    fetch("https://api.example.com/data")
      .then((response) => response.json())
      .then((result) => {
        if (isMounted) setData(result);
      })
      .catch((error) => console.error("데이터를 가져오는 중 오류 발생:", error));

    return () => {
      isMounted = false;
    };
  }, []);

  return <div>{data ? `데이터: ${data.name}` : "로딩 중..."}</div>;
}
```

이렇게 수정된 패턴은 상태를 설정하기 전에 컴포넌트가 여전히 마운트된 상태인지 확인하여 오류를 방지합니다.

<div class="content-ad"></div>

4. 큰 구성 요소를 너무 복잡하게 만드는 실수

잘못된 방법: 큰 구성 요소에서 많은 종속성을 가진 useEffect를 사용하여 다시 렌더링 문제 발생.

해결책: 큰 구성 요소를 작은 구성 요소로 나누고 해당 작은 구성 요소 내에서 useEffect를 로컬로 관리하십시오. 이 방법을 사용하면 구성 요소를 관리할 수 있고 효과를 필요한 곳에 특정할 수 있습니다.

5. 신중하지 않고 고급 논리 구현하기

<div class="content-ad"></div>

오류: useEffect 내부에 복잡한 로직이 포함되어 있으며 여러 상태와 프롭에 크게 의존합니다.

해결책:

```js
import { useEffect, useState } from "react";

function AdvancedLogicComponent({ propValue }) {
  const [stateValue, setStateValue] = useState(0);

  useEffect(() => {
    const result = complexCalculation(propValue, stateValue);
    setStateValue(result);
  }, [propValue, stateValue]);

  return <div>결과: {stateValue}</div>;
}
```

의존성을 신중하게 관리하고 useEffect에서 참조된 값들의 안정성을 고려해 주세요.

<div class="content-ad"></div>

# 일반적인 개선 팁

- 의존성 최소화: 효과를 발생시키는 데 절대 필요한 것만 포함합니다.
- 효과 격리: 효과를 더 작고 관리하기 쉬운 조각들로 분해합니다.
- 효과 정리: 구독, 이벤트 리스너, 타이머 등이 포함된 효과 이후에는 항상 정리를 해야 합니다.
- 오류 처리: 항상 비동기 작업 내에서 오류를 처리합니다.
- 복잡한 의존성 피하기: 의존성인 함수나 객체를 안정화하기 위해 useCallback과 useMemo를 사용합니다.
