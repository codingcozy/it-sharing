---
title: "Svelte Series-6 생명주기 관리 방법"
description: ""
coverImage: "/assets/img/2024-07-26-SvelteSeries-6Lifecycle_0.png"
date: 2024-07-26 11:55
ogImage: 
  url: /assets/img/2024-07-26-SvelteSeries-6Lifecycle_0.png
tag: Tech
originalTitle: "Svelte Series-6 Lifecycle"
link: "https://dev.to/frost_gary_90f3cf1699bd02/svelte-series-6-lifecycle-mgp"
---


각 구성 요소는 생성부터 파괴까지의 라이프 사이클을 가지고 있어요. 서로 다른 라이프 사이클 단계에서 구성 요소는 외부 세계에 메소드를 제공하면서, 구성 요소를 호출하는 개발자가 각 라이프 사이클 단계에서 구성 요소를 보다 유연하게 제어할 수 있도록 도와줘요. 이러한 메소드를 라이프 후크 함수라고 해요.

Svelte의 라이프사이클 후크는 다음과 같아요:

- onMount
- beforeUpdate
- afterUpdate
- onDestroy
- tick

## 호출 시점

<div class="content-ad"></div>

만약 서버 측 렌더링(SSR)을 사용한다면, onDestroy를 제외한 나머지 라이프사이클 함수가 SSR 실행 중에 실행되지 않습니다.

Svelte에서는 라이프사이클 함수를 컴포넌트를 초기화할 때만 작성할 수 있어서, 컴포넌트의 인스턴스에 콜백을 바인딩할 수 있습니다. setTimeout이나 setInterval과 같은 비동기 메서드 내부에 라이프사이클 함수를 배치하지 마세요.

컴포넌트를 초기화할 때 라이프사이클 함수가 호출되도록 해야 하지만, 어디서 호출되었는지는 중요하지 않습니다. 예를 들어, 라이프사이클 함수를 메서드 내부에 넣을 수 있습니다:

```js
// app.js
import { onMount, onDestroy } from 'svelte';

export const startInterval = () => {
  let timer = null;
  let count = 0;

  onMount(() => {
    console.log('onMount');
    timer = setInterval(() => {
      count++;
      console.log('count');
    }, 1000)
  });

  onDestroy(() => {
    console.log('onDestroy');
    if (timer) {
      clearInterval(timer);
    }
  })
}
```

<div class="content-ad"></div>

```js
<script>
  import { startInterval } from './app.js';

  startInterval();
</script>

<div>App</div>
```

이 경우에는 라이프사이클이 구성 요소가 초기화될 때 호출됨을 보장할 수 있습니다.

## onMount

구성 요소가 DOM에 마운트된 직후 바로 실행되는 콜백 함수입니다.

<div class="content-ad"></div>


onMount(callback: () => void)

onMount(callback: () => () => void)


onMount은 콜백 함수를 인수로 받으며, 콜백 함수 내에서 함수가 반환되면 컴포넌트가 언로드 될 때 호출됩니다.

Svelte 프로젝트의 소스 코드를 살펴볼 수 있습니다. 파일 경로는 packages/svelte/src/runtime/internal/Component.js:

```js
/** @returns {void} */
export function mount_component(component, target, anchor) {
  const { fragment, after_update } = component.$$;
  fragment && fragment.m(target, anchor);
  // onMount happens before the initial afterUpdate
  add_render_callback(() => {
    const new_on_destroy = component.$$.on_mount.map(run).filter(is_function);
    // if the component was destroyed immediately
    // it will update the `$$.on_destroy` reference to `null`.
    // the destructured on_destroy may still reference to the old array
    if (component.$$.on_destroy) {
      component.$$.on_destroy.push(...new_on_destroy);
    } else {
      // Edge case - component was destroyed immediately,
      // most likely as a result of a binding initializing
      run_all(new_on_destroy);
    }
    component.$$.on_mount = [];
  });
  after_update.forEach(add_render_callback);
}
```

<div class="content-ad"></div>

우리는 onMount 메서드를 실행하고 값을 가져오는 것을 볼 수 있습니다. 그런 다음 컴포넌트의 onDestroy 라이프사이클 후크에 선언이 있는지 확인하고, 선언이 있다면 onDestroy 배열에 추가한 다음, onDestroy 이후에 이 반환 값을 실행하고, 그렇지 않으면 반환 값을 직접 실행합니다. React에서 useEffect 후크도 동일한 기능을 가지고 있습니다.

```js
useEffect(() => {
  return () => {}
}, []);
```

onMount의 반환값에 대한 Svelte 예제를 따라해봅시다:

```js
<script>
  // Child.svelte
  import { onMount } from 'svelte';

  onMount(() => {
    console.log('child mount');
    return () => {
      console.log('child destroy 1');
    }
  })
</script>

<div>child</div>
```

<div class="content-ad"></div>

```js
<script>
  // Father.svelte
  import Child from './Child.svelte';
  let count = 0;

  const updateCount = () => {
    count++;
  }
</script>


<button on:click={updateCount}>add</button>
{#if count == 0}
<Child />
{/if}
```

이 예시에서는 부모 페이지에 변수 count가 있으며, count를 변경하여 자식 컴포넌트의 암시적 값을 제어합니다. 코드를 실행하면 콘솔에서 순서대로 출력됩니다: child mount - child destroy 1.

그런 다음 onDestroy 후크를 추가합니다:

```js
<script>
  import { onMount, onDestroy } from 'svelte';

  onMount(() => {
    console.log('child mount');
    return () => {
      console.log('child destroy 1');
    }
  })

  onDestroy(() => {
    console.log('child destroy 2');
  })
</script>

<div>child</div>
```

<div class="content-ad"></div>

실행 가능한 코드를 실행하면 콘솔에 child mount -` child destroy 2 -` child destroy 1이 출력됩니다.

형제 구성 요소의 경우 컴포넌트 호출 순서에 따라 위에서 아래로 onMount가 실행됩니다. 부모 및 자식 구성 요소의 경우 자식 구성 요소의 onMount가 부모 구성 요소의 onMount보다 먼저 실행되며, 즉 안쪽에서 바깥쪽으로 실행됩니다.

```js
<script>
  import { onMount } from "svelte";
  import Child from "./Child.svelte";
  import Child2 from "./Child2.svelte";

  onMount(() => {
    console.log("fahter mount");
  });
</script>

<Child />
<Child2 />
```

우리는 부모 구성 요소와 두 개의 자식 구성 요소를 정의합니다. 세 개의 구성 요소 내에서 onMount를 별도로 호출합니다. 실행 후에는 child mount 1 -` child mount 2 -` father mount를 볼 수 있습니다.

<div class="content-ad"></div>

## beforeUpdate

DOM 업데이트 전에 실행되며, 첫 번째 콜백은 초기화 전에 onMount에서 실행됩니다.

```js
beforeUpdate(callback: () => void)
```

여기에 예시가 있습니다:

<div class="content-ad"></div>

```js
<script>
  import { beforeUpdate } from "svelte";

  let count = 0;
  let str = "";

  const updateData = () => {
    count++;
    setTimeout(() => {
      str += "s";
    }, 1000);
  };

  beforeUpdate(() => {
    console.log("before update", count, str);
  });
</script>

<button on:click={updateData}>update</button>
<span>{count}</span><span>{str}</span>
```

페이지를 로드한 후에 콘솔에 먼저 before update 0이 출력됩니다. 버튼을 클릭하여 업데이트하고 before update 1이 출력되는 것을 확인하고, 1초 후에 before update 1s가 출력됩니다.

<img src="/assets/img/2024-07-26-SvelteSeries-6Lifecycle_0.png" />

동시에 beforeUpdate와 onMount가 처음부터 존재하는 경우에 주의해야 합니다. beforeUpdate 콜백이 onMount 보다 먼저 실행됩니다. 위 코드에서 onMount 훅을 호출하고 동일한 단계를 진행하면 다음과 같은 결과를 볼 수 있습니다.

<div class="content-ad"></div>

<img src="/assets/img/2024-07-26-SvelteSeries-6Lifecycle_1.png" />

## afterUpdate

컴포넌트가 렌더링된 후에 실행되는 콜백입니다.

```js
afterUpdate(callback: () => void)
```

<div class="content-ad"></div>

동료 컴포넌트의 경우에는 component 호출 순서에 따라 여전히 beforeUpdate 및 afterUpdate가 위에서 아래로 실행됩니다. 하지만 부모 및 자식 컴포넌트의 경우에는 부모 컴포넌트의 beforeUpdate가 먼저 실행되고, 그 다음 자식 컴포넌트의 beforeUpdate가 실행됩니다.

자식 컴포넌트의 beforeUpdate가 완료되면 부모 컴포넌트의 afterUpdate가 실행되고, 마지막으로 자식 컴포넌트의 afterUpdate가 실행됩니다.

```js
<script>
  import { onMount, onDestroy, beforeUpdate, afterUpdate } from "svelte";
  import Child from "./Child.svelte";
  import Child2 from "./Child2.svelte";
  let count = 0;

  const updateCount = () => {
    count++;
  };

  beforeUpdate(() => {
    console.log("부모 beforeupdate");
  });

  onMount(() => {
    console.log("부모 mount");
  });

  afterUpdate(() => {
    console.log("부모 afterupdate");
  });

  onDestroy(() => {
    console.log("부모 destroy");
  });
</script>

<button on:click={updateCount}>추가</button>
{#if count <= 1}
  <Child {count} />
  <Child2 {count} />
{/if}
```

자식 컴포넌트 내용은 다음과 같습니다. Child1.svelte 및 Child2.svelte는 출력 콘텐츠의 일련 번호만 다르고 다른 것이 없습니다.

<div class="content-ad"></div>

```js
<script>
  // Child.svelte
  import { onMount, onDestroy, beforeUpdate, afterUpdate } from 'svelte';

  export let count;

  beforeUpdate(() => {
    console.log('child beforeupdate 1')
  });

  onMount(() => {
    console.log('child mount 1');
  });

  afterUpdate(() => {
    console.log('child afterupdate 1');
  });

  onDestroy(() => {
    console.log('child destroy 1');
  });
</script>

<div>child</div>
{count}
```

처음 실행 시:
![Svelte Lifecycle Methods](/assets/img/2024-07-26-SvelteSeries-6Lifecycle_2.png)

버튼을 클릭하여 업데이트 후:


<div class="content-ad"></div>


![이미지](/assets/img/2024-07-26-SvelteSeries-6Lifecycle_3.png)

## onDestroy

컴포넌트가 파괴된 후에 실행할 콜백입니다:

```js
onDestroy(callback: () => void)
```

<div class="content-ad"></div>

형제 컴포넌트 간에는 호출 순서에 따라 여전히 onDestroy가 위에서 아래로 실행됩니다. 부모 및 자식 컴포넌트는 바깥쪽에서 안쪽으로 실행되며, 부모 컴포넌트가 먼저 onDestroy를 실행한 후에 자식 컴포넌트의 onDestroy가 실행됩니다.

onMount에서 반환된 함수는 컴포넌트가 소멸될 때 실행되며, onDestroy 실행 후에 실행됩니다.

## tick

tick 함수는 다른 라이프사이클 후크와 다르게 컴포넌트가 처음으로 초기화될 때를 기다리지 않고 언제든 호출할 수 있습니다. 상태가 변경될 때 즉시 해결되는 Promise를 반환합니다.

<div class="content-ad"></div>

```js
promise: Promise = tick()
```

Svelte에서 상태가 변경되면 DOM이 즉시 업데이트되지 않고, 다음 마이크로태스크를 기다립니다. 대기 기간 동안 다른 상태 변경을 계속 수신하고, 이후에 이 마이크로태스크에서 DOM을 일관되게 업데이트하므로 불필요한 작업을 줄이고 브라우저가 이러한 작업을 더 효율적으로 처리할 수 있게 합니다.

개발 중에 이러한 문제를 종종 마주할 수 있습니다: 컴포넌트의 상태가 업데이트되지만 DOM이 업데이트되지 않을 때, 단순히 DOM의 값을 얻고 싶다면 tick 함수를 사용하면 편리합니다! (네, 이는 Vue의 $nextTick()과 같이 사용할 수 있습니다)

```js
<script>
  let count = 0;

  const addCount = () => {
    count++;
    let countDom = document.querySelector('#count');
    if (countDom) {
      console.log('count text', countDom.innerHTML);
    }
  }
</script>

<button on:click={addCount}>추가</button>

<span id="count">{count}</span>
```

<div class="content-ad"></div>


![Image](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fidsbxktldadzh41m6dsc.gif)

페이지가 업데이트되어도 DOM 작업에 의해 검색된 값이 여전히 이전 데이터임을 확인할 수 있습니다.

위 문제를 해결하기 위해 다음과 같이 할 수 있습니다:

```js
<script>
  import { tick } from 'svelte';
  let count = 0;

  const addCount = async () => {
    count++;
    await tick();
    let countDom = document.querySelector('#count');
    if (countDom) {
      console.log('count text', countDom.innerHTML);
    }
  }
</script>

<button on:click={addCount}>추가</button>

<span id="count">{count}</span>
```

<div class="content-ad"></div>


![image](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fztd7njjyc3d7uqbw454v.gif)

그리고 틱을 사용하면 최신 값을 얻을 수 있습니다.

## 요약

이 장에서 우리는 다음을 배웠습니다:


<div class="content-ad"></div>

- Svelte 라이프사이클 함수의 역할
- 부모 및 자식 컴포넌트가 렌더링될 때 각 라이프사이클 함수의 실행 순서
- 틱의 역할