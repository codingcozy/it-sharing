---
title: "인기 프레임워크 React의 Fiber 심층 분석 작동 원리와 장점"
description: ""
coverImage: "/assets/img/2024-07-13-UnderstandingReactFiberinDepth_0.png"
date: 2024-07-13 18:41
ogImage:
  url: /assets/img/2024-07-13-UnderstandingReactFiberinDepth_0.png
tag: Tech
originalTitle: "Understanding React Fiber in Depth"
link: "https://medium.com/javascript-in-plain-english/understanding-react-fiber-in-depth-abe1be7a1797"
---

![React Fiber](/assets/img/2024-07-13-UnderstandingReactFiberinDepth_0.png)

React Fiber는 React 16에서 소개된 새로운 조정 엔진으로, React의 핵심 알고리즘을 다시 구현하여 더 효율적이고 유연하게 만든 것입니다. Fiber는 React의 기초 데이터 구조로, 컴포넌트 트리의 상태를 설명하는 데 사용됩니다. Fiber의 주요 목표는 React의 성능을 향상시키는 것인데, 특히 복잡한 사용자 인터페이스 업데이트를 다룰 때 효율성을 개선합니다.

이 글은 React Fiber의 구성, 상태 관리, 후크 메커니즘, 그리고 더블 버퍼링 전략에 대해 상세히 알려주어 독자들이 그 원리를 깊게 이해할 수 있도록 도와줍니다.

# 1. FiberNode 데이터 구조

<div class="content-ad"></div>

```js
function FiberNode(this: $FlowFixMe, tag: WorkTag, pendingProps: mixed, key: null | string, mode: TypeOfMode) {
  // 인스턴스 속성
  this.tag = tag; // 함수 컴포넌트, 클래스 컴포넌트 등과 같은 노드 유형
  this.key = key; // 동일 레벨의 자식 노드를 구별하기 위한 고유 식별자
  this.elementType = null; // JSX 유형
  this.type = null; // 컴포넌트 유형
  this.stateNode = null; // 컴포넌트 인스턴스 또는 DOM 노드

  // Fiber 속성
  this.return = null; // 상위 노드
  this.child = null; // 첫 번째 자식 노드
  this.sibling = null; // 다음 형제 노드
  this.index = 0; // 자식 노드의 인덱스

  this.ref = null; // 참조
  this.refCleanup = null; // 참조 정리

  this.pendingProps = pendingProps; // 현재 수신 중인 props
  this.memoizedProps = null; // 메모이제된 props
  this.updateQueue = null; // 업데이트 대기열
  this.memoizedState = null; // 메모이제된 상태
  this.dependencies = null; // 종속성

  this.mode = mode; // Concurrent 모드와 같은 모드

  // 효과
  this.flags = NoFlags; // 효과 플래그
  this.subtreeFlags = NoFlags; // 하위 트리 효과 플래그
  this.deletions = null; // 삭제할 자식 노드

  this.lanes = NoLanes; // 현재 우선 순위
  this.childLanes = NoLanes; // 하위 트리의 우선 순위

  this.alternate = null; // 이중 버퍼링을 위한 대체 Fiber

  // 기타
}
```

## 인스턴스 부분:

- tag: 함수 컴포넌트, 클래스 컴포넌트, 호스트 컴포넌트 등과 같은 Fiber 노드의 유형을 나타냅니다.
- key: 동일한 레벨에서 자식 노드를 구별하는 데 사용되는 고유 식별자입니다.
- elementType 및 type: 각각 JSX 유형 및 실제 컴포넌트 유형을 나타냅니다.
- stateNode: 클래스 컴포넌트의 컴포넌트 인스턴스 또는 호스트 컴포넌트의 DOM 노드입니다.

## Fiber 부분:

<div class="content-ad"></div>

- return: 부모 노드를 가리킵니다.
- child 및 sibling: 각각 첫 번째 자식 노드와 다음 형제 노드를 가리킵니다.
- index: 자식 노드가 부모 노드 내에서의 위치를 나타냅니다.
- ref 및 refCleanup: 컴포넌트 인스턴스나 DOM 노드에 대한 참조 및 정리를 처리합니다.
- pendingProps 및 memoizedProps: 각각 현재 수신 중인 props와 마지막 렌더링에서의 메모이즈된 props를 나타냅니다.
- updateQueue: 처리할 업데이트 큐를 저장합니다.
- memoizedState: 마지막 렌더링에서의 메모이즈된 상태입니다.
- dependencies: 컴포넌트의 컨텍스트 의존성을 나타냅니다.
- mode: Fiber의 모드를 나타냅니다 (예: 동시 모드).

## 효과 부분:

- flags 및 subtreeFlags: 해당 노드와 하위 트리에 대해 처리해야 하는 부수 효과 유형을 표시하는 데 사용됩니다.
- deletions: 삭제할 자식 노드 목록을 저장합니다.
- lanes 및 childLanes: 스케줄링 우선순위를 관리하고 어떤 업데이트를 먼저 처리해야 하는지 결정합니다.
- alternate: 동일한 노드의 대안 버전을 가리키며, 더블 버퍼링에 사용됩니다.

# 2. Fiber 노드 생성하기

<div class="content-ad"></div>

React는 createFiber 함수를 사용하여 새로운 Fiber 노드를 생성합니다. 이 함수는 제공된 매개변수를 기반으로 새로운 FiberNode 인스턴스를 초기화합니다.

```js
function createFiber(tag, pendingProps, key, mode) {
  return new FiberNode(tag, pendingProps, key, mode);
}
```

만약 함수 컴포넌트를 위한 노드를 생성한다고 가정해 봅시다:

```js
const tag = FunctionComponent; // 또는 다른 태그 유형
const pendingProps = { someProp: "someValue" };
const key = "uniqueKey";
const mode = NoMode;

const fiber = createFiber(tag, pendingProps, key, mode);
```

<div class="content-ad"></div>

이 예시에서는 createFiber 함수를 사용하여 tag, pendingProps, key 및 mode와 같은 필수 매개변수를 제공하여 함수 컴포넌트의 새 Fiber 노드를 초기화합니다.

# 3. Fiber Node 상태 관리 및 Hook 정보

React Fiber 아키텍처에서 Fiber 노드는 컴포넌트의 기본 정보뿐만 아니라 컴포넌트의 상태와 Hook 정보도 유지합니다. 여기에는 소스 코드의 관련 부분을 포함한 상세한 설명이 제공됩니다:

## 3.1 Fiber Node에서 상태 저장

<div class="content-ad"></div>

Fi베어 노드의 상태 저장은 주로 다음 속성을 통해 달성됩니다:

```js
this.pendingProps = pendingProps;
this.memoizedProps = null;
this.updateQueue = null;
this.memoizedState = null;
this.dependencies = null;
```

- pendingProps: 이 업데이트에 사용되는 현재 수신된 프롭스입니다.
- memoizedProps: 이전 렌더링에서의 프롭스로, 업데이트가 필요한지 확인하기 위한 비교에 사용됩니다.
- updateQueue: 컴포넌트가 처리해야 하는 상태 및 이펙트 업데이트를 포함하는 업데이트 큐를 저장합니다.
- memoizedState: 마지막 렌더링에서의 상태로, 현재 컴포넌트의 상태 연결 목록을 포함합니다.
- dependencies: 컴포넌트가 다시 렌더링해야 하는지를 결정하기 위한 컴포넌트의 컨텍스트 의존성을 저장합니다.

## 3.2 훅의 저장 및 관리

<div class="content-ad"></div>

리액트에서, Hooks는 연결 리스트 구조를 사용하여 Fiber 노드에 저장됩니다. 각 Fiber 노드는 memoizedState 속성을 가지고 있는데, 이 속성은 각 Hook의 상태를 나타내는 노드를 가리키는 단일 연결 리스트입니다. useState와 useReducer를 예시로 삼아 Hooks를 생성하고 관리하는 소스 코드에 대한 설명이 있습니다.

useState와 useReducer의 경우, 구현은 Fiber 노드의 memoizedState에 연결된 연결 리스트에 Hook 상태를 생성하고 관리하는 것을 포함합니다. 아래에 간단한 설명이 있습니다:

```js
// useState의 예시 구현
function useState(initialState) {
  const hook = updateWorkInProgressHook(); // 연결 리스트에서 현재 Hook을 가져옵니다

  if (!hook.memoizedState) {
    hook.memoizedState = initialState; // 초기 상태를 설정합니다
  }

  const setState = (newState) => {
    hook.memoizedState = newState; // 상태를 업데이트합니다
    scheduleUpdateOnFiber(); // 업데이트를 예약합니다
  };

  return [hook.memoizedState, setState];
}

// useReducer의 예시 구현
function useReducer(reducer, initialArg, init) {
  const hook = updateWorkInProgressHook(); // 연결 리스트에서 현재 Hook을 가져옵니다

  if (!hook.memoizedState) {
    hook.memoizedState = init ? init(initialArg) : initialArg; // 초기 상태를 설정합니다
  }

  const dispatch = (action) => {
    hook.memoizedState = reducer(hook.memoizedState, action); // 리듀서로 상태를 업데이트합니다
    scheduleUpdateOnFiber(); // 업데이트를 예약합니다
  };

  return [hook.memoizedState, dispatch];
}

// 현재 Hook을 가져오는 도우미 함수
function updateWorkInProgressHook() {
  // Hook 연결 리스트를 탐색하고 업데이트하는 로직이 있습니다
}
```

이 예시에서, updateWorkInProgressHook은 Hook의 연결 리스트를 탐색하고 업데이트하는 도우미 함수입니다. 이를 통해 각 Hook의 상태가 올바르게 유지되고 업데이트가 효율적으로 예약되도록 합니다.

<div class="content-ad"></div>

위의 분석을 통해 React Fiber 노드는 복잡한 상태와 Hook 관리 메커니즘을 통해 효율적인 업데이트와 렌더링을 달성한다는 것이 명백합니다. 이 디자인은 React가 복잡한 애플리케이션을 처리할 때에도 뛰어난 성능과 유연성을 유지할 수 있도록 합니다.

# 4. 업데이트 유형

React Fiber 아키텍처에서는 이.flags를 사용하여 현재 Fiber 노드에 대한 부수효과 (effects)를 나타냅니다. 이 효과 플래그는 조정 및 적용 단계에서 Fiber 노드에 수행해야 할 작업을 결정합니다. 여기에 일반적인 플래그와 그 의미가 있습니다:

```js
export const NoFlags = /*                      */ 0b00000000000000000000000000;
export const Placement = /*                    */ 0b00000000000000000000000010;
export const Update = /*                       */ 0b00000000000000000000000100;
export const PlacementAndUpdate = /*           */ 0b00000000000000000000000110;
export const Deletion = /*                     */ 0b00000000000000000000001000;
// 다른 플래그들...
```

<div class="content-ad"></div>

- NoFlags (0b00000000000000000000000000)는 부작용이 없음을 나타냅니다. 이것은 기본 상태이며, 처리할 부작용이 없는 경우 flags가 NoFlags로 설정됩니다.
- Placement (0b00000000000000000000000010)는 새로운 Fiber 노드가 DOM 트리에 삽입되어야 함을 나타냅니다. 이 플래그는 일반적으로 새로운 노드가 생성될 때 설정됩니다.
- Update (0b00000000000000000000000100)는 기존 Fiber 노드가 업데이트되어야 함을 나타냅니다. 예를 들어 컴포넌트의 props나 state가 변경되는 경우 이 플래그가 설정됩니다.
- PlacementAndUpdate (0b00000000000000000000000110)는 노드가 삽입 및 업데이트되어야 함을 나타냅니다. 일반적으로 일부 노드가 DOM 트리에 삽입된 후 즉시 업데이트되어야 할 때 발생합니다.
- Deletion (0b00000000000000000000001000)는 Fiber 노드가 DOM 트리에서 제거되어야 함을 나타냅니다. 이 플래그는 노드가 제거되는 경우에 설정됩니다.

Fiber의 조정 과정에서 다양한 조건에 따라 다른 플래그가 설정됩니다. 아래는 조정 과정 중 this.flags가 어떻게 업데이트되는지 보여주는 간단한 예제입니다:

```js
function reconcileChildren(current, workInProgress, nextChildren) {
  if (current === null) {
    // 삽입 단계
    workInProgress.child = mountChildFibers(workInProgress, null, nextChildren);
    workInProgress.flags |= Placement; // Placement 플래그 설정
  } else {
    // 업데이트 단계
    workInProgress.child = reconcileChildFibers(workInProgress, current.child, nextChildren);
    workInProgress.flags |= Update; // Update 플래그 설정
  }
}
```

이 예제에서 삽입 단계에서는 Placement 플래그가 새 노드를 DOM에 삽입해야 함을 나타내도록 설정됩니다. 업데이트 단계에서는 Update 플래그가 기존 노드가 업데이트되어야 함을 나타내도록 설정됩니다.

<div class="content-ad"></div>

# 5. 대안

Fiber 아키텍처는 UI 업데이트를 더 효율적이고 부드럽게 만들기 위해 이중 버퍼링 전략을 도입했습니다. 이중 버퍼링 전략의 핵심은 두 개의 Fiber 트리를 사용하여 컴포넌트 상태를 관리하는 데 있습니다: 현재 트리와 진행 중인 작업 트리입니다. 이 두 트리는 `alternate` 속성을 통해 연결됩니다.

## 5.1 이중 버퍼링 전략 원리

React에서 UI 업데이트 프로세스는 Reconciliation Phase와 Commit Phase 두 단계로 나뉩니다. 이중 버퍼링 전략의 도입으로 이 두 단계를 더 효율적으로 실행할 수 있게 되었습니다.

<div class="content-ad"></div>

화해 단계:

- React는 현재 트리를 기반으로 작업 중인 트리를 생성합니다.
- 작업 중인 트리는 현재 트리의 복사본이며, 모든 업데이트 작업은 현재 표시된 UI에 영향을주지 않고이 트리에서 수행됩니다.
- 이 단계에서 React는 작업 중인 트리를 통해 탐색하여 각 노드의 업데이트를 계산하고 해당 부작용 플래그(flag)를 설정합니다.

커밋 단계:

- 화해 단계가 완료되면 React는 작업 중인 트리를 새로운 현재 트리로 만들고 이전 현재 트리를 이전 작업 중인 트리로 표시합니다.
- 그런 다음 React는 부작용 플래그에 따라 DOM 트리를 업데이트하여 UI를 새로운 현재 트리와 동기화합니다.

<div class="content-ad"></div>

각 Fiber 노드에는 동일한 노드의 다른 버전을 가리키는 대체 속성이 있습니다. 구체적으로는:

- 현재 트리의 각 노드에는 작업 진행 중인 트리에서 해당 노드를 가리키는 alternate가 있습니다.
- 작업 진행 중인 트리의 각 노드에는 현재 트리에서 해당 노드를 가리키는 alternate가 있습니다.

이 설계는 React가 조정 및 커밋 단계 중에 두 Fiber 트리를 효율적으로 전환하고 비교할 수 있도록 합니다.

## 5.2 alternate 사용하기

<div class="content-ad"></div>

재결합 단계에서 React는 현재 노드에 이미 대체 노드가 있는지 확인합니다. 대체 노드가 있는 경우 React는 이 대체 노드를 재사용하여 새로운 작업 진행 중인 트리를 구축하고, 그렇지 않으면 새로운 Fiber 노드를 생성합니다.

```js
function beginWork(current, workInProgress, renderLanes) {
  if (current !== null) {
    // 현재 트리에서 `대체(Alternate)` 노드를 재사용합니다.
    const existingAlternate = workInProgress.alternate;
    if (existingAlternate !== null) {
      // 기존 `대체(Alternate)` 노드를 재사용합니다.
      workInProgress = existingAlternate;
    } else {
      // 새로운 `대체(Alternate)` 노드를 생성합니다.
      workInProgress.alternate = createWorkInProgress(current, workInProgress.pendingProps);
      workInProgress = workInProgress.alternate;
    }
  } else {
    // 새로운 Fiber 노드를 생성합니다.
    workInProgress = createFiberFromTypeAndProps(workInProgress.tag, workInProgress.pendingProps);
  }
  // 재결합 작업을 계속합니다.
}
```

위 분석을 통해 React Fiber에서 대체 속성과 더블 버퍼링 전략의 중요성을 알 수 있습니다. 이 설계는 React의 성능을 크게 향상시키고 복잡한 UI 업데이트를 처리하는 유연성을 제공하여 React가 더 효율적으로 스케줄링하고 렌더링할 수 있게 합니다.

Fiber 객체 내 각 필드를 깊이있게 이해함으로써 React 조정자가 작동하는 방식을 더 잘 이해할 수 있습니다. Fiber는 조정자의 핵심 데이터 구조이며, 컴포넌트 상태, 업데이트 및 부수 효과에 대한 정보를 저장함으로써 React가 효율적으로 렌더링하고 업데이트할 수 있게 합니다. 이러한 메커니즘을 깊이 이해함으로써 React가 어떻게 작동하는지에 대한 깊은 통찰을 얻을 수 있어 실제 개발에서 효율성과 합리성을 향상시킬 수 있습니다.

<div class="content-ad"></div>

# 친근한 말투로 번역

인플레인잉글리쉬 커뮤니티에 함께해 주셔서 감사합니다! 마지막으로 가시기 전에:

- 작가를 칭찬하고 팔로우해주세요 👏️️
- 팔로우하기: X | LinkedIn | YouTube | Discord | 뉴스레터
- 다른 플랫폼 방문하기: CoFeed | Differ
- PlainEnglish.io에서 다양한 콘텐츠 확인하기
