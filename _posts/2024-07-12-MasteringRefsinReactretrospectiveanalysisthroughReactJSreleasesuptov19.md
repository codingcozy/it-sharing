---
title: "React의 Ref 완벽 정복 ReactJS v 19까지의 버전별 회고 분석"
description: ""
coverImage: "/assets/img/2024-07-12-MasteringRefsinReactretrospectiveanalysisthroughReactJSreleasesuptov19_0.png"
date: 2024-07-12 19:04
ogImage:
  url: /assets/img/2024-07-12-MasteringRefsinReactretrospectiveanalysisthroughReactJSreleasesuptov19_0.png
tag: Tech
originalTitle: "Mastering Refs in React: retrospective analysis through ReactJS releases up to v. 19"
link: "https://medium.com/javascript-in-plain-english/mastering-refs-in-react-retrospective-analysis-through-reactjs-releases-up-to-v-19-a76b5eab8e37"
---

<img src="/assets/img/2024-07-12-MasteringRefsinReactretrospectiveanalysisthroughReactJSreleasesuptov19_0.png" />

내가 개발하면서 겪은 경험과 처음에 혼란스러울 수 있는 도전에 관한 것을 기반으로, ReactJS에서 값에 대한 참조와 관련된 사용 사례들을 종합적으로 정리해 보았습니다. 또한, 우리는 React 컴포넌트를 함수로 만드는 것이 아니라 클래스 컴포넌트로 만드는 구식 방법에 대해 몇 가지 특정 사용 사례를 다룰 것입니다. 마지막으로, 최근 React 19의 릴리스 이후에 이 주제에서의 최신 변경 사항을 반드시 언급할 것입니다.

이 글을 개인적으로 쓴 주요 가치와 동기는 React 릴리스 역사의 관점에서 이 기능을 분석하고, 철학이 시간이 지남에 따라 어떻게 변화하는지 살펴보려는 것입니다.

## ReactJS에서 참조 사용 목적

새로운 것을 배울 때, 가장 먼저 확인해야 할 단일 진리의 원천은 공식 문서여야 합니다. 여기 참조로 첫 번째 응용 프로그램이 있습니다:

<div class="content-ad"></div>

그리고 여기가 두 번째 애플리케이션입니다 (공식 문서):

## refs 기능의 역사적 고찰

리액트에서 refs를 사용했던 초기 시절을 아직 기억합니다. 그때는 refs가 DOM 노드나 리액트 엘리먼트에 직접 접근하는 유일한 방법이었습니다.

그런데 왜 최근에는 리액트에서 참조의 첫 번째 적용에 대해 언급이 없을까요? 그것에는 이유가 있죠. 최근에는 React.Component를 확장하는 클래스 컴포넌트를 통해 React 컴포넌트를 생성하는 유일한 방법이 있었습니다. 새로운 렌더를 유발하지 않고 일부 정보를 "기억"하기를 원한다면 refs를 사용해서는 안 되었습니다. 클래스 컴포넌트 내의 this.state 바깥에 어떤 필드든 선언하는 것으로 충분했죠:

<div class="content-ad"></div>

```js
class Counter extends React.Component {
  counter = 0;

  handleClick = () => {
    this.counter = this.counter + 1;
    alert("You clicked " + this.counter + " times!");
  };

  render() {
    return <button onClick={this.handleClick}>Click</button>;
  }
}
```

여기서는 전혀 refs가 필요하지 않음을 알 수 있습니다. 그러나 함수로 표시된 컴포넌트에서 동일한 로직을 수행하려면 다음과 같이 보입니다:

```js
function Counter() {
  const ref = useRef(0);

  function handleClick() {
    ref.current = ref.current + 1;
    alert("You clicked " + ref.current + " times!");
  }

  return <button onClick={handleClick}>Click!</button>;
}
```

클래스 컴포넌트를 기반으로 개발된 애플리케이션에서 refs를 사용하는 또 다른 흥미로운 사례는 부모 컴포넌트에서 클래스 컴포넌트의 인스턴스에 액세스하여 해당 메서드를 호출할 수 있다는 것입니다.

<div class="content-ad"></div>

클래스 컴포넌트로 구현된 Timer 컴포넌트가 있다고 가정해봅시다:

```js
import React from "react";

export class Timer extends React.Component {
  state = {
    counter: 0,
  };

  interval: NodeJS.Timer | null = null;

  reset = () => {
    this.setState({ counter: 0 });
  };

  stop = () => {
    if (this.interval) {
      clearInterval(this.interval);
    }
  };

  start = () => {
    this.stop();
    this.interval = setInterval(() => {
      this.setState((state) => ({ counter: state.counter + 1 }));
    }, 1000);
  };

  componentDidMount() {
    this.start();
  }

  componentWillUnmount() {
    this.stop();
  }

  render() {
    return <div>{this.state.counter}</div>;
  }
}
```

이 클래스는 reset, stop, start와 같은 추가 메서드를 노출합니다. 이 컴포넌트를 다른 곳에서 사용할 때는 Timer를 관리하고자 하는 메서드에 액세스할 수 있습니다:

```js
function App() {
  const timerRef = useRef < Timer > null;

  return (
    <>
      <button onClick={() => timerRef.current?.stop()}>정지</button>
      <button onClick={() => timerRef.current?.reset()}>리셋</button>
      <button onClick={() => timerRef.current?.start()}>시작</button>
      <Timer ref={timerRef} />
    </>
  );
}
```

<div class="content-ad"></div>

여기는 이 코드에 대한 샌드박스입니다 (링크). 멋지지 않나요?

함수형 React 컴포넌트에 동일한 방법을 적용할 수 있는 방법이 있나요? 네, 있습니다. 특별한 훅인 useImperativeHandle을 사용하면 됩니다 (공식 문서는 여기에 있습니다). 그리고 동일한 Timer 컴포넌트는 다음과 같이 보일 것입니다:

```js
export type TimerHandlers = {
  reset: () => void,
  stop: () => void,
  start: () => void,
};

export const TimerFunctionalComponent =
  forwardRef <
  TimerHandlers >
  ((_, ref) => {
    const [counter, setCounter] = useState(0);
    const intervalRef = (useRef < NodeJS.Timer) | (null > null);

    const stop = useCallback(() => {
      if (intervalRef.current) {
        clearInterval(intervalRef.current);
      }
    }, []);

    const start = useCallback(() => {
      stop();
      intervalRef.current = setInterval(() => {
        setCounter((counter) => counter + 1);
      }, 1000);
    }, []);

    const reset = useCallback(() => {
      setCounter(0);
    }, []);

    useImperativeHandle(ref, () => ({ stop, start, reset }), [stop, start, reset]);

    useEffect(() => {
      start();
    }, [start]);

    return <div>{counter}</div>;
  });
```

부모 컴포넌트는 거의 동일할 것입니다 — 유일한 차이점은 TypeScript 유형에만 있습니다:

<div class="content-ad"></div>

```js
import { Timer, TimerHandlers } from "./components/Timer";

function App() {
  const timerRef = useRef < TimerHandlers > null;

  return (
    <>
      <button onClick={() => timerRef.current?.stop()}>Stop</button>
      <button onClick={() => timerRef.current?.reset()}>Reset</button>
      <button onClick={() => timerRef.current?.start()}>Start</button>
      <Timer ref={timerRef} />
    </>
  );
}
```

## forwardRef — 무엇인가요?

우리가 컴포넌트를 개발하고 부모 컴포넌트에서 일부 DOM 노드에 액세스할 수 있도록 하려면 어떻게 해야 할까요? 다음과 같이 보일 수 있습니다:

```js
const Child = ({
  containerRef,
  contentRef,
}: {
  containerRef: MutableRefObject<HTMLDivElement | null>,
  contentRef: MutableRefObject<HTMLDivElement | null>,
}) => (
  <div ref={containerRef} className="container">
    <div ref={contentRef} className="content" />
  </div>
);

function App() {
  const containerRef = (useRef < HTMLDivElement) | (null > null);
  const contentRef = (useRef < HTMLDivElement) | (null > null);

  useEffect(() => {
    console.log(containerRef.current, contentRef.current);
  }, []);

  return (
    <>
      <Child containerRef={containerRef} contentRef={contentRef} />
    </>
  );
}
```

<div class="content-ad"></div>

콘솔에서는 다음과 같이 볼 수 있습니다:

<img src="/assets/img/2024-07-12-MasteringRefsinReactretrospectiveanalysisthroughReactJSreleasesuptov19_1.png" />

하지만 Child 컴포넌트의 프로퍼티 containerRef를 확인해보세요. 아마도 이렇게 ref로 이름을 변경하는 것이 더 나아보일 것입니다:

```js
// containerRef 대신 ref로 프로퍼티를 선언합니다
const Child = ({
  ref,
  contentRef,
}: {
  ref: MutableRefObject<HTMLDivElement | null>,
  contentRef: MutableRefObject<HTMLDivElement | null>,
}) => (
  <div ref={ref} className="container">
    <div ref={contentRef} className="content" />
  </div>
);

function App() {
  const containerRef = (useRef < HTMLDivElement) | (null > null);
  const contentRef = (useRef < HTMLDivElement) | (null > null);

  useEffect(() => {
    console.log(containerRef.current, contentRef.current);
  }, []);

  return (
    <>
      <Child ref={containerRef} contentRef={contentRef} />
    </>
  );
}
```

<div class="content-ad"></div>

React 버전 ≤18로 이 코드를 실행하면 놀랄 거에요:

![image](/assets/img/2024-07-12-MasteringRefsinReactretrospectiveanalysisthroughReactJSreleasesuptov19_2.png)

하지만 forwardRef를 사용하여 이 문제를 해결할 수 있어요:

```js
type ChildProps = {
  contentRef: MutableRefObject<HTMLDivElement | null>;
};

const Child = forwardRef<HTMLDivElement | null, ChildProps>(
  ({ contentRef }: ChildProps, ref) => (
    <div ref={ref} className="container">
      <div ref={contentRef} className="content" />
    </div>
  )
);
```

<div class="content-ad"></div>

## React 19 버전 이후의 변경 사항

이전 코드가 꽤 이상하고 난잡해 보인다는 사실에 동의해야 합니다 — 부모 컴포넌트에서 자식 컴포넌트로 참조를 전달하기 위해 컴포넌트를 forwardRef로 감싸야 했습니다.

좋은 소식이 있어요: React 19 버전 이상에서는 더 이상 이렇게 할 필요가 없습니다.

## React에서 참조 사용의 일반적인 실수

<div class="content-ad"></div>

새 기능으로 소개된 References 변수에 대해 알게 되자마자 이러한 코드 일부를 자주 만나게 되었어요 (이 코드의 링크를 증명으로 제공하지만 이미 없는 경우, 아래 제공된 코드를 참조하세요):

```js
export const App: FC = () => {
    const inputGroupRef = useRef<HTMLDivElement>(null);
    const inputRef = useRef<HTMLInputElement>(null);

    useEffect(() => {
        const handleFocus = () =>
           inputGroupRef.current!.classList.add(styles.active);
        const handleBlur = () =>
           inputGroupRef.current!.classList.remove(styles.active);

        inputRef.current!.addEventListener('focus', handleFocus);
        inputRef.current!.addEventListener('blur', handleBlur);

        return () => {
            inputRef.current!.removeEventListener('focus', handleFocus);
            inputRef.current!.removeEventListener('blur', handleBlur);
        }
    }, []);

    return (
        <div className={styles.container}>
            <div ref={inputGroupRef} className={styles.inputGroup}>
                <label className={styles.label}>
                    Your name
                </label>
                <input
                    ref={inputRef}
                    className={styles.input}
                    type="text"
                />
                <div className={styles.border}/>
            </div>

        </div>
    )
};
```

React 문서를 주의 깊게 살펴보면 다음과 같은 경고가 있어요:

이전 코드 일부는 이 규칙을 어기고 있어요. React는 명령형(imperative) 방식이 아닌 선언적(declarative) 접근 방식을 지지합니다. 그리고 이 코드는 매우 지저분한 구현과 많이 닮았습니다. jQuery가 명령형 접근 방식으로 살아있었던 때를 떠올립니다. 여기서 개발자는 컴포넌트의 상태를 명령적으로 변경하고, React의 강력한 점인 DOM 업데이트를 담당하고 효율적으로 처리하는 방법을 사용하지 않았어요. 여기서 개발자의 의도는 각 입력 상태에 대해 컴포넌트의 CSS 클래스를 변경하는 것이고, useRef 사용을 제거할 수 있어요. 그래서, 이를 선언적 방식으로 다시 작성해볼까요:

<div class="content-ad"></div>

```js
export const App: FC = () => {
  const [isActive, changeState] = useState(false);

  return (
    <div className={styles.container}>
      <div className={isActive ? "${styles.inputGroup} ${styles.active}" : styles.inputGroup}>
        <label className={styles.label}>Your name</label>
        <input
          onFocus={() => changeState(() => true)}
          onBlur={() => changeState(() => false)}
          className={styles.input}
          type="text"
        />
        <div className={styles.border} />
      </div>
    </div>
  );
};
```

The comparison of the number of code lines still maintains its neat appearance.

## How to keep DOM nodes for a list of components

There is no problem accessing a single DOM node in a React Component as the documentation provides a solution for this:

<div class="content-ad"></div>

```js
const Component = () => {
  const myRef = useRef(null);

  return <div ref={myRef} />;
};
```

하지만 다음 작업에 대해 시간을 투자해 보세요 — React 컴포넌트에서 div 엘리먼트 목록을 렌더링하고, 이들의 DOM 엘리먼트에 액세스해야 하는 경우 — 이를 어떻게 처리할 것인가요?

```js
return (
  <>
    {list.map((item) => (
      <div key={item.id}>{item.title}</div>
    ))}
  </>
);
```

React 컴포넌트의 ref 속성에 대한 Typescript 타입을 확인해 보면, 다음과 같은 내용을 볼 수 있을 거예요:

<div class="content-ad"></div>

```js
타입 Ref<T> = RefCallback<T> | RefObject<T> | null;
```

여기서 말하는 것은 useRef 훅을 사용하여 이전에 생성된 값에 참조를 첨부하거나 특정 DOM 요소에 액세스를 제공하는 콜백을 사용하는 두 가지 옵션이 있다는 것입니다. 따라서 이렇게 보일 수 있습니다:

```js
const elementsRef = useRef<{[k: string]: HTMLElement | null }>({});
return (
  <>
     {list.map(({id, title}) =>
        <div
           key={id}
           ref={(node) => {
              elementsRef.current[id] = node
           }}
        >
        {item.title}
        </div>
     )}
  </>
);
```

실제 적용 사례는 무엇일까요? 반응형 헤더를 만들어야 하는 경우를 가정해 봅시다. 이 헤더에는 링크가 있는 네비게이션 메뉴가 포함되어 있어야 합니다. 화면 크기가 충분히 축소되어 모든 링크를 표시할 공간이 없는 경우 네비게이션 메뉴가 버거 메뉴로 전환되어야 합니다. 이를 위해 네비게이션 링크의 DOM 노드에 액세스하여 getBoundingClientRect 메서드를 사용하여 모든 링크의 크기를 측정해야 합니다.

<div class="content-ad"></div>

# 주요 포인트

React의 Refs는 다음과 같은 용도로 사용됩니다:

- DOM 요소에 액세스하기
- 다시 렌더링을 유발하지 않는 값 저장하기

Refs를 과도하게 사용하는 것은 코드를 읽고 유지 관리하기 어렵게 만드는 일반적인 실수입니다. 따라서 여기에는 좋은 사용 사례 목록이 있습니다:

- 포커스, 텍스트 선택 또는 미디어 재생 관리하기
- DOM 요소의 위치와 크기 측정하기
- 특정 DOM 요소로 스크롤하기
- React에서 노출하지 않는 브라우저 API 호출하기
- 명령형 애니메이션 트리거하기
- 서드파티 DOM 라이브러리와 통합하기

부모 구성 요소에서 자식 구성 요소로 Ref를 전달하려면 React의 버전이 18 이하인 경우 forwardRef로 자식을 래핑해야 합니다. React 19부터는 이 작업을 더 이상 수행할 필요가 없습니다.

<div class="content-ad"></div>

만약 React 클래스 컴포넌트에 참조(reference)를 할당하면, 해당 클래스를 기반으로 React에 의해 생성된 인스턴스의 모든 메소드에 액세스할 수 있습니다. 함수형 컴포넌트에서도 hook useImperativeHandle을 사용하여 동일한 효과를 얻을 수 있습니다.

레퍼런스를 할당하는 주요 두 가지 방법이 있습니다:

- 변수로 할당하는 경우 — React.createRef (클래스 컴포넌트)를 통해 ref를 생성하거나 hook useRef (함수형 컴포넌트)를 통해 ref를 생성하고 JSX 태그의 ref 속성에 할당합니다.
- 콜백 함수로 할당하는 경우 — JSX 태그의 ref 속성에 전달하고, 컴포넌트가 마운트 또는 언마운트 될 때 React가 이 함수를 호출하여 DOM 요소나 클래스 컴포넌트의 인스턴스 (또는 마운트 해제 시 null)를 전달합니다.

# 간단한 용어로 🚀

In Plain English 커뮤니티의 일원이 되어 주셔서 감사합니다! 계속 참여해주세요:

<div class="content-ad"></div>

- 작가를 칭찬하고 팔로우해주세요! 👏
- 팔로우하기: X | 링크드인 | 유튜브 | 디스코드 | 뉴스레터
- 다른 플랫폼 방문하기: CoFeed | Differ
- 더 많은 콘텐츠: PlainEnglish.io
