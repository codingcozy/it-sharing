---
title: "Svelte 시리즈-5 Props 사용 방법 자세히 알아보기"
description: ""
coverImage: "/assets/img/2024-07-24-SvelteSeries-5Props_0.png"
date: 2024-07-24 12:01
ogImage: 
  url: /assets/img/2024-07-24-SvelteSeries-5Props_0.png
tag: Tech
originalTitle: "Svelte Series-5 Props"
link: "https://dev.to/frost_gary_90f3cf1699bd02/svelte-series-5-props-d22"
---


## 구성 요소 기반 개발

프로젝트 개발에서는 보통 구성 요소 기반 개발을 촉진합니다. 이는 일반적인 로직을 추출하여 구성 요소로 조합하는 것을 말합니다. 페이지에서 필요한 기능을 수행하기 위해 구성 요소를 직접 호출할 수 있습니다. 이 구성 요소는 자체 내부 로직을 유지하는 것 외에도 외부 데이터를 전달받아 구성 요소의 동적이고 유연한 기능을 구현해야 합니다.

## 속성(Props)

간단한 할 일 목록(`todolist`)의 예시를 살펴보겠습니다. 할 일 목록을 구성 요소로 캡슐화해 표시할 수 있으며, 구성 요소의 내부 로직은 데이터 목록을 표시하는 것이며 외부에서 데이터 목록을 수신합니다. 페이지에서 구성 요소를 참조한 후 할 일 목록이나 완료된 목록을 전달함으로써 다른 데이터를 표시할 수 있습니다. 여기서 값을 전달하는 것이 중요한 연결 고리입니다.

<div class="content-ad"></div>

부모 페이지에서 자식 페이지나 컴포넌트로 값을 전달할 때 일반적으로 부모 페이지의 상태나 메소드를 전달합니다. Svelte에서 이 두 가지를 어떻게 전달할까요?

### prop이 데이터인 경우

이전 장의 끝에서 다음 코드를 만났습니다:

```js
<script>
  export let value;
</script>
```

<div class="content-ad"></div>

일반적으로 페이지에서 변수를 선언할 때는 let value;와 같이 선언합니다. 그 변수 앞에 export 키워드를 추가하면 외부 익스포트 속성으로 표시된 변수 선언과 동일합니다. 이렇게 하면 파일의 외부 참조에서 prop이라는 이름으로 변수에 저장된 값을 전달할 수 있습니다.

```js
<!-- Father .svelte -->
<script>
  import Child from './Child.svelte';
  let value = ''
</script>

<Child value={value} />
```

#### 기본 프롭

컴포넌트 내에서 export let value; 형식으로 외부로 내보낼 때:

<div class="content-ad"></div>

```js
<script>
  export let value;
</script>

<div>child: {value || ''}</div>
```

위 컴포넌트를 값 전달 없이 참조할 때 발생하는 문제를 설명한 이미지입니다.

![image](/assets/img/2024-07-24-SvelteSeries-5Props_0.png)

![image](/assets/img/2024-07-24-SvelteSeries-5Props_1.png)


<div class="content-ad"></div>

VsCode에서 값 전달이 필요하다는 메시지를 받을 뿐만 아니라 Chrome 콘솔에서도 적절한 경고 메시지를 받게 됩니다.

개발 중에는 몇몇 전달 매개변수에 대한 기본값을 제공하는 경우가 많습니다. 대부분의 경우 일정한 상태를 사용하며 특별한 상황에서만 상태를 변경하는 구성 요소가 있기 때문입니다. 예를 들어 UI 컴포넌트 라이브러리에서 Alert 컴포넌트와 유사한 구성 요소가 있다고 가정해보겠습니다. 이 구성 요소에는 네 가지 상태(success, info, warning, error)가 있으며, 우리가 사용하는 대부분의 알림은 일반 메시지 알림, 즉 info 상태이므로 구성 요소 내에서 기본 info 상태를 설정할 수 있습니다.

prop에 기본값을 설정한 후에는 위 예제의 경고 메시지를 제거할 수 있습니다. Svelte에서 prop에 기본값을 설정하는 것은 초기 선언 시 값 할당하는 간단한 작업입니다.

```js
<script>
  export let value = '안녕하세요, Svelte';
</script>
```

<div class="content-ad"></div>

#### 키워드

자바스크립트에서는 특정 의미를 갖는 프로그래밍 언어의 구문 구조에서 사용되는 특별한 단어인 키워드가 있습니다. 이러한 단어들은 변수로 사용될 수 없습니다.

Svelte에서는 prop를 설정할 때 일부 키워드를 prop으로 설정할 수 있습니다.

매우 일반적인 예로, 컴포넌트 내에 자체적인 클래스 스타일을 가진 컴포넌트를 정의할 수 있습니다. 그러나 사용자가 외부에서 클래스 스타일 속성을 사용자가 조작할 수 있는 형식으로 컴포넌트를 사용자가 조작할 수 있는 형식으로 지정할 수 있도록 하려면 (예: `Component class='' /`), 그리고 class는 클래스를 정의하는 데 사용되는 js의 키워드입니다. 따라서 아래와 같이 작성할 수 있습니다:

<div class="content-ad"></div>

```js
<script> 
  let className;
  export { className as class };
</script>
```

#### const

여기 질문 하나 있어요: export로 선언된 변수는 메소드가 될 수 있을까요?

```js
<!-- Child.svelte -->
<script>
  export let onChange = () => {return;}
</script>

<button on:click={onChange}>change</button>
```

<div class="content-ad"></div>

```js
<script>
  import Child from './Child.svelte';
  let count = 0;
  const onChange = () => {
    count = Math.random();
  }
</script>

<Child onChange={onChange} />
<span>{count}</span>
```

답은 YES입니다!

<img src="https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fvx1l4rmyynslof8sz38n.gif" />

주의할 점: 함수 방법, 클래스 클래스 및 상수 상수를 내보낼 때 내보낸 속성은 변경할 수 없습니다.

<div class="content-ad"></div>

위에 나온 내보내기 방법을 예시로 사용하여, Child.svelte의 내용을 다음과 같이 변경해보세요:

```js
<script>
  export function onChange() {
    console.log('no change');
  }
</script>

<button on:click={onChange}>change</button>
```

<img src="https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fr94vn571zlvgdn43qoid.gif" />

변수나 메서드를 속성(prop)으로 설정하려면 let 또는 var 선언을 사용해보세요.

<div class="content-ad"></div>

## 함수 호출됨

### createEventDispatcher

이전 장에서 HTML 태그의 이벤트 바인딩은 on:[이벤트 이름]을 사용한다는 것을 배웠습니다. 마찬가지로, 컴포넌트에 이벤트 바인딩을 하려면 이 형식을 사용합니다.

```js
<Component on:[이벤트 이름]={이벤트 처리기} />
```

<div class="content-ad"></div>

컴포넌트에서 이벤트를 바인딩할 때는 eventName를 컴포넌트 내부에 구현해야 합니다.

내부 구현은 다음과 같습니다:

```js
import { createEventDispatcher } from 'svelte';

const dispatch = createEventDispatcher();

dispatch(eventName, data)
```

dispatch의 첫 번째 매개변수는 이벤트 이름이며, 두 번째 매개변수는 외부로 데이터를 전달하는 데 사용됩니다. 둘 이상의 매개변수를 전달해야 하는 경우, 두 번째 매개변수를 객체로 설정하여 해당 객체에 외부로 전달할 여러 데이터를 저장합니다.

<div class="content-ad"></div>

여기 예시가 있어요:

```js
<script>
  import { createEventDispatcher } from 'svelte';

  const dispatch = createEventDispatcher();

  const onClick = (e) => {
    dispatch("click", e);
  }
</script>

<button on:click={onClick}>버튼</button>
```

```js
<script>
  import Component from './Component.svelte';

  const onClick = (e) => {
    console.log('component click', e);
  }
</script>

<Component on:click={onClick} />
```

클릭하면 콘솔에서 CustomEvent 형식의 데이터가 출력됩니다:

<div class="content-ad"></div>


![image](/assets/img/2024-07-24-SvelteSeries-5Props_2.png)

보통 우리는 페이지에 출력된 이벤트 매개변수가 자식 컴포넌트에서 전달한 것이라고 생각할 것입니다. 그러나 Svelte에서 createEventDispatcher를 사용하여 데이터를 전달할 때 데이터는 event.detail에 넣어진다는 점에 주목해 주고 싶습니다. 위 이미지를 자세히 살펴보면, 서브컴포넌트에서 전달된 데이터를 보관하는 detail 키가 출력된 데이터에 있다는 것을 알 수 있습니다.

위 예시가 직관적으로 충분하지 않을 수도 있으니, 우리의 서브컴포넌트 전달을 수정해 봅시다:

```js
<script>
  import { createEventDispatcher } from 'svelte';

  const dispatch = createEventDispatcher();

  const onClick = (e) => {
    dispatch("click", 123);
  }

</script>

<button on:click={onClick}>버튼</button>
```


<div class="content-ad"></div>


![image](/assets/img/2024-07-24-SvelteSeries-5Props_3.png)

컴포넌트가 초기화될 때 createEventDispatcher를 만들어야 함을 참고해주세요. 즉, 최상위 스크립트 스코프에서 만들어야하며, 그렇지 않으면 오류가 발생합니다.

```js
<script>
  import { createEventDispatcher } from 'svelte';

  const onClick = (e) => {
    const dispatch = createEventDispatcher();
    dispatch("click", 123);
  }

</script>

<button on:click={onClick}>버튼</button>
```

![image](/assets/img/2024-07-24-SvelteSeries-5Props_4.png)


<div class="content-ad"></div>

createEventDispatcher의 구현을 간단히 살펴봅시다:

```js
export function createEventDispatcher() {
  const component = get_current_component();
  return (type, detail, { cancelable = false } = {}) => {
    const callbacks = component.$$.callbacks[type];
    if (callbacks) {
      const event = custom_event(/** @type {string} */ (type), detail, {
        cancelable,
      });
      callbacks.slice().forEach((fn) => {
        fn.call(component, event);
      });
      return !event.defaultPrevented;
    }
    return true;
  };
}

export function custom_event(
  type,
  detail,
  { bubbles = false, cancelable = false } = {}
) {
  return new CustomEvent(type, { detail, bubbles, cancelable });
}
```

사실 createEventDispatcher는 CustomEvent의 새 인스턴스를 만들고, 그런 다음 두 번째 전달로 전달할 detail에 모든 데이터를 넣는 것입니다.

### 이벤트 발송

<div class="content-ad"></div>

DOM 이벤트와 달리, Svelte에서는 컴포넌트 이벤트가 버블링되지 않습니다.

```js
<script>
  // GrandSon.svelte
  import { createEventDispatcher, onMount } from 'svelte';

  const dispatch = createEventDispatcher();

  const func = () => {
    dispatch('func', '안녕');
  }

  onMount(() => {
    func();
  })
</script>

<div>孙组件</div>
```

```js
<script>
  // Child.svelte
  import { createEventDispatcher } from 'svelte';
  import GrandSon from './GrandSon.svelte';

  const dispatch = createEventDispatcher();

  const func = (e) => {
    dispatch('func', e.detail);
  }

</script>

<GrandSon on:func={func}/>
```

```js
<script>
  // Father.svelte
  import Child from './Child.svelte';

  const func = (e) => {
    console.log('孙子传值：', e.detail);
  }
</script>

<Child on:func={func} />
```

<div class="content-ad"></div>


![이미지](/assets/img/2024-07-24-SvelteSeries-5Props_5.png)

저는 이 예시를 주로 여기서 사용하여 내부 컴포넌트가 가장 바깥쪽 레이어에 이벤트를 전파하려고 할 때, 통과하는 각 컴포넌트 레이어가 createEventDispatch()를 사용하고 이벤트.detail을 전달해야 한다는 것을 설명하고 싶었습니다. 이것은 클릭과 같은 상호 작용을 포함하는 컴포넌트에 일반적인 일입니다. 우리는 컴포넌트에 클릭 이벤트 지원을 추가할 수 있지만 클릭 이벤트의 로직은 외부 사용자가 정의하는 것으로 남겨집니다. 이를 위해 이벤트 전파를 사용해야 합니다.

이벤트 전파는 다음과 같이 작성됩니다:

```js
on:이벤트명
```   


<div class="content-ad"></div>

이벤트 이름만 적혀 있으며 구성 요소 자체는 이벤트 구현을 제공하지 않습니다. 이벤트의 구현은 외부 레이어가 구현하고자 하는 페이지에 맡겨집니다.

```js
<!-- GrandSon.svelte -->
<button on:click>孙组件</button>
```

```js
<script>
  // Child.svelte
  import GrandSon from './GrandSon.svelte';
</script>

<GrandSon on:click />
```

```js
<script>
  // Father.svelte
  import Child from './Child.svelte';

  const onClick = () => {
    console.log('点击');
  }
</script>

<Child on:click={onClick} />
```

<div class="content-ad"></div>


![Image](/assets/img/2024-07-24-SvelteSeries-5Props_6.png)

We can dispatch custom events for components as well as native events.

When it occurs that the attribute name is the same as the value, it can be abbreviated. For example, value='value' can be abbreviated to 'value'.

Here's an example:


<div class="content-ad"></div>

위의 내용을 번역해 드리겠습니다.

```js
<script>
  let checked = false;

  const onChange = () => {
    checked = true;
  }
</script>

<input type="checkbox" {checked} />
<button on:click={onChange}>checked로 변경</button>
```

원래는 `input type="checkbox" checked='checked' /` 형식으로 작성해야 했습니다.

<img src="https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fatavfbwjecf24qt4rn2t.gif" />

## $$props

<div class="content-ad"></div>

$$props는 부모로부터 전달된 모든 속성을 가져옵니다. 프롭스 속성과 비-프롭스 속성 모두 포함됩니다.

```js
<script>
  // Child.svelte
  export let value;

  console.log('$$props', $$props)
</script>

<div>자식 컴포넌트</div>
```

```js
<script>
  // Father.svelte
  import Child from './Child.svelte';
</script>

<Child value={'hello'} a={'a'} b={'b'} />
```

![이미지](/assets/img/2024-07-24-SvelteSeries-5Props_7.png)

<div class="content-ad"></div>

## $$restProps

$$props는 props와 비-props 속성을 모두 포함하는 반면, $$restProps에는 비-props 속성만 포함됩니다.
이전 예제의 콘솔을 다음과 같이 다시 작성해보세요:

```js
<script>
  export let value;

  console.log('$$props', $$props);
  console.log('$$restProps', $$restProps);
</script>

<div>자식 컴포넌트</div>
```

![이미지](/assets/img/2024-07-24-SvelteSeries-5Props_8.png)

<div class="content-ad"></div>

우리는 $$props 및 $$restProps를 직접 전달하기 위해 해체할 수 있지만, 이것은 값들을 전달하는 것처럼 권장되지 않는 방법입니다. 혼동을 줄 수 있기 때문에 권장되지 않습니다.

```js
<script>
  import GrandSon from './GrandSon.svelte';
  export let value;
</script>

<GrandSon {...$$props} />
```

## 간단한 요약

이 장에서 우리는 다음을 배웠습니다:

<div class="content-ad"></div>

- 변수를 컴포넌트의 외부 속성으로 선언하려면 export를 사용하고, 외부 속성의 기본값을 설정하는 방법
- 키워드를 속성으로 선언하는 방법
- createEventDispatcher 메서드를 사용하여 컴포넌트 내에서 외부로 이벤트를 보내어 외부 당사자가 컴포넌트를 참조할 때 이벤트를 수신할 수 있게 하는 방법
- 컴포넌트 이벤트 디스패치
- 컴포넌트 속성 할당 약식 표기
- Svelte는 $$props 및 $$restProps 속성을 제공하여 해당 속성 값들을 빠르게 얻을 수 있습니다