---
title: "React로 경량 Typeahead 컴포넌트 설계하는 방법 - 프론트엔드 시스템 디자인"
description: ""
coverImage: "/assets/img/2024-07-09-DesignaLightweightTypeheadComponentinReactFrontendSystemDesign_0.png"
date: 2024-07-09 13:43
ogImage:
  url: /assets/img/2024-07-09-DesignaLightweightTypeheadComponentinReactFrontendSystemDesign_0.png
tag: Tech
originalTitle: "Design a Lightweight Typehead Component in React — Frontend System Design"
link: "https://medium.com/@shubham.sinha2512/design-a-lightweight-typehead-component-in-react-frontend-system-design-56565c86f5c9"
---

<img src="/assets/img/2024-07-09-DesignaLightweightTypeheadComponentinReactFrontendSystemDesign_0.png" />

타입헤드(Typehead)는 간단한 기능입니다. 검색 쿼리를 입력하기 시작하면 관련 제안을 표시해줍니다. 웹이나 모바일 애플리케이션에서 사용자 경험을 향상시키기 위한 표준 기능입니다. 이 간단한 기능은 최종 사용자의 삶을 훨씬 쉽게 만들지만, 올바르게 구현하는 것은 약간 까다로울 수 있습니다. 다행히도 현대적인 프레임워크와 라이브러리들은 개발자들의 삶을 훨씬 쉽게 만들어 주고 있습니다.

기능 및 비기능 요구 사항에 대해 이야기해봅시다. 여기서는 기능에만 초점을 맞출 것입니다.

디자인 문서는 다음 링크에서 확인할 수 있습니다: [디자인 문서 보기](https://drive.google.com/file/d/1P1Guarwh7Vry32az2wFmLkJBDMkoTmi7/view?usp=sharing)

<div class="content-ad"></div>

기능 요구 사항:

- 사용자가 입력할 때 제안을 표시해야 합니다.
- 제안의 드롭다운 목록을 표시해야 합니다.

비기능 요구 사항:

- 성능 최적화되어야 합니다.

<div class="content-ad"></div>

자, 이제 우리가 가장 간단한 형태의 Typehead 컴포넌트를 시작해볼게요. 이 컴포넌트는 세 부분으로 구성될 거에요 - 입력 필드, 검색 결과 항목을 표시할 결과 섹션, 그리고 검색 결과 항목 자체입니다.

처음에는 입력 필드만 보입니다.

![이미지](/assets/img/2024-07-09-DesignaLightweightTypeheadComponentinReactFrontendSystemDesign_1.png)

사용자가 입력을 시작하면 결과 컨테이너가 표시되고 검색 결과로 채워질 거에요.

<div class="content-ad"></div>

먼저, 우리의 입력 컴포넌트를 구축해 봅시다. 사용자가 타이핑을 멈출 때 최종 값만 필요하기 때문에 ref를 사용할 것입니다. TypeInput 컴포넌트는 부모 컴포넌트 Typehead로부터 전달된 입력용 ref를 받고, 사용자가 타이핑을 멈출 때 검색어를 포함한 onType 함수가 호출되며 포커스 및 블러에 대한 몇 가지 표준 핸들러도 포함되어 있습니다.

```js
const TypeInput = forwardRef(({ onType, onFocus, onBlur, ...props }, ref) => {
  return (
    <div className="w-full p-4 rounded-xl bg-stone-100">
      <input
        ref={ref}
        onChange={onType}
        onFocus={onFocus}
        onBlur={onBlur}
        type="text"
        placeholder="Type to search.."
        className="w-full bg-stone-100 outline-none text-stone-600"
      />
    </div>
  );
});
```

또한, TypeItem 및 ResultList 컴포넌트를 빠르게 구현해 보겠습니다.

<div class="content-ad"></div>

```js
const ResultList = ({ items }) => {
  return (
    <div className="bg-white shadow-lg">
      {items.map((item, index) => (
        <TypeItem item={item} key={index} />
      ))}
    </div>
  );
};

const TypeItem = ({ item }) => {
  return (
    <div className="px-2 py-4 border-b border-stone-200 text-stone-600">
      <h3 className="font-semibold">{item.name}</h3>
      <p className="text-xs mt-2">{item.registrationDate}</p>
    </div>
  );
};
```

이제 우리 컴포넌트의 핵심 로직을 포함할 Typehead 부모 컴포넌트로 들어가 봅시다.

```js
function Typehead({ onSearch, ...props }) {
  const searchTermRef = useRef();
  let [results, setResults] = useState([]);

  const handleType = (e) => {
    const searchTerm = searchTermRef?.current?.value || "";
    setResults(onSearch(searchTerm));
  };

  const handleFocus = (e) => {};

  // blur 이벤트가 발생하면 검색 결과 숨김
  const handleBlur = (e) => {
    setResults([]);
  };

  return (
    <div className="w-96">
      <TypeInput ref={searchTermRef} onType={handleType} onFocus={handleFocus} onBlur={handleBlur} />
      <ResultList items={results} />
    </div>
  );
}
```

Typehead 컴포넌트는 onSearch 함수라는 하나의 외부 프롭만 필요로 합니다. 이 함수는 검색 및 필터링 비즈니스 로직을 구현하며 searchRef는 검색어를 보유하는 ref이며, handleType 함수는 사용자가 문자를 입력할 때마다 실행되고 결과가 결과에 저장됩니다.

<div class="content-ad"></div>

우리의 Typehead 구성 요소는 현재 기능을 하고 있어요. 하지만 한 가지 주의할 점이 있습니다. onSearch 함수가 모든 키 입력에 대해 실행됩니다. 이는 실제 서버에서 데이터를 가져오는 구현에서 많은 API 호출을 유발할 수 있습니다. 이를 개선하기 위해 onSearch 함수를 디바운싱하는 커스텀 훅을 구현해봐요.

디바운싱을 위한 커스텀 훅을 구현해 봅시다.

```js
import { useRef, useCallback } from "react";

export function useDebounce(fn, delay) {
  const timeoutRef = useRef(null);

  const debouncedFn = useCallback(
    (...args) => {
      if (timeoutRef.current) {
        clearTimeout(timeoutRef.current);
      }
      timeoutRef.current = setTimeout(() => {
        fn(...args);
      }, delay);
    },
    [fn, delay]
  );

  return debouncedFn;
}
```

- timeoutRef: 이는 렌더 간 타임아웃 ID를 저장하는 데 사용됩니다. 값이 변경되어도 리렌더를 유발하지 않고 렌더 간 타임아웃이 유지됩니다.
- useCallback: fn 또는 delay 중 하나가 변경되었을 때만 변경되는 콜백 함수의 메모이즈된 버전을 반환하는 React 훅.
- fn: 디바운싱하고자 하는 함수.
- delay: 밀리초 단위의 디바운스 지연 시간.
- 반환 값: 제공된 함수의 디바운스된 버전.

<div class="content-ad"></div>

이제 이 Typehead 함수를 다음과 같이 업데이트하겠습니다.

```js
function Typehead({ onSearch, ...props }) {
  const searchTermRef = useRef();
  const [results, setResults] = useState([]);

  const handleType = (e) => {
    const searchTerm = searchTermRef?.current?.value || "";
    setResults(onSearch(searchTerm));
  };

  const debouncedOnType = useDebounce(handleType, 500);

  const handleFocus = (e) => {};
  const handleBlur = (e) => {
    setResults([]);
  };

  return (
    <div className="w-96">
      <TypeInput ref={searchTermRef} onType={debouncedOnType} onFocus={handleFocus} onBlur={handleBlur} />
      <ResultList items={results} />
    </div>
  );
}
```

위 코드는 Typehead 컴포넌트의 최적화된 버전을 보여줍니다. 이제 사용자가 입력을 멈춘 후 500ms 후에만 검색이 트리거되어 불필요한 처리를 제거했습니다.

이 컴포넌트를 더 최적화하고 사용자 경험을 향상시키는 기능을 더 추가할 수 있습니다. 결과를 캐시하는 것, 상위 결과 중 일부로 사전 준비하는 것, 관련성에 따라 정렬하는 것, 키보드 탐색을 추가하는 것 등 여러 가지 방법이 있습니다. 이 코드를 확장하여 직접 작업하고 작동 방식을 경험해 보세요. 즐거운 코딩이 되시길 바랍니다!
