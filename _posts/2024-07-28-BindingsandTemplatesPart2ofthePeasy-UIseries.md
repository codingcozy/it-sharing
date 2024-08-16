---
title: "Peasy-UI 시리즈 바인딩과 템플릿 사용법, 두 번째 이야기"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-07-28 13:53
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Bindings and Templates Part 2 of the Peasy-UI series"
link: "https://dev.to/jyoung4242/bindings-and-templates-part-2-of-the-peasy-ui-series-k44"
isUpdated: true
updatedAt: 1723817115372
---



# 목차

- 소개
- 바인딩과 템플릿
- 텍스트 바인딩
    - 기본 바인딩
    - 조건부 불리언 텍스트 바인딩
    - 일회성 텍스트 바인딩
- 기본 바인딩
- 조건부 불리언 텍스트 바인딩
- 일회성 텍스트 바인딩
- 속성 바인딩
    - 요소 속성을 위한 바인딩
    - 이벤트를 위한 바인딩
    - 데이터 모델에 전체 요소 바인딩
    - 랜더링을 위한 바인딩
    - 배열을 위한 바인딩
    - 컴포넌트를 위한 바인딩
- 요소 속성을 위한 바인딩
- 이벤트를 위한 바인딩
- 데이터 모델에 전체 요소 바인딩
- 랜더링을 위한 바인딩
- 배열을 위한 바인딩
- 컴포넌트를 위한 바인딩
- 게터
- 더 많은 정보
- 결론

## 소개

오늘은 Peasy-UI 라이브러리를 좀 더 자세히 살펴볼 것입니다. 이 라이브러리에서 사용할 수 있는 다양한 종류의 바인딩을 설명하고, 해당 바인딩을 DOM으로 렌더링되도록 라이브러리에 제공하는 문자열 리터럴 템플릿에 어떻게 통합할 수 있는지 설명하겠습니다. 이 글에서는 TypeScript를 사용하여 모든 예시를 제공하지만, 라이브러리는 순수 JavaScript와도 완벽하게 작동합니다.

<div class="content-ad"></div>

만약 Peasy-UI에 대한 소개 기사를 놓치셨다면, 여기에서 읽을 수 있습니다. 이 기사는 라이브러리에 대한 개요를 제공하며, 시리즈에서 다룰 주제도 소개합니다.

## 바인딩과 템플릿

바인딩이 무엇인지, 템플릿이 무엇인지, 그리고 이들이 DOM에 렌더링되는 내용을 제어하기 위해 라이브러리와 어떻게 작동하는지를 간단히 정리해보겠습니다.

Peasy-UI는 지정한 DOM 요소에 콘텐츠를 삽입하는 라이브러리입니다. 삽입되는 콘텐츠는 HTML 표시 방식을 제공하는 문자열 리터럴 템플릿에 의해 결정됩니다.

<div class="content-ad"></div>

```js
const template = `
  <div> Render Me </div>
  <button> Click </button>
`;
```

Peasy-UI에 전달하는 문자열 리터럴 템플릿의 예입니다. 이 템플릿은 대상 DOM 요소로 렌더링됩니다.

바인딩은 해당 템플릿을 동적으로 제어할 수 있는 수단을 제공합니다. 예를 들어, Hello World 예제를 해보겠습니다.

```js
const model = {
  name: "Bob",
};

const template = `
<div>
    Hello: \${name}!!!!
</div>
`;

await UI.create(document.body, model, template).attached;
```

<div class="content-ad"></div>

이 코드는 문자열 리터럴 템플릿에 바인딩된 데이터 저장소(모델)를 생성합니다. UI.create()는 모델, 템플릿 및 주입할 대상 DOM 요소를 매개변수로 사용합니다. JavaScript 코드에서 model.name을 마음껏 변경할 수 있으며, 이렇게 하면 새로운 텍스트가 동적으로 DOM에 다시 렌더링됩니다. 참고로 UI.create() 메서드의 await는 Peasy-UI가 콘텐츠를 DOM에 연결할 때까지 후속 로직의 실행을 일시 중지시킵니다. 이렇게 함으로써 아직 렌더링되지 않은 DOM 콘텐츠를 변경하거나 액세스하려고 하는 것을 방지할 수 있습니다.

## 텍스트 바인딩

Peasy-UI에는 텍스트 바인딩과 속성 바인딩 두 가지 유형의 바인딩이 있습니다. 문자열 리터럴을 Peasy-UI에 제공하기 때문에 이를 바인딩으로 지정하기 위해 백슬래시 이스케이프 문자를 사용한다는 것을 기억해야 합니다.

### 기본 바인딩

<div class="content-ad"></div>

가장 기본적인 바인딩은 데이터를 DOM으로 삽입하는겁니다. 위의 Hello World는 이것의 좋은 예시에요. 이 바인딩의 사용 사례는 DOM 요소 내부 텍스트 또는 HTML 콘텐츠를 조작할 수 있고, 예를 들어 이름을 변경할 수 있다는 점이에요. 또한 CSS 문자열 내부의 CSS 속성을 변경하는 데에도 사용해본 적이 있어요.

바인딩: $'propertyName'

위의 예시와 같이 이렇게 사용할 수 있어요:

```js
const model = {
  name: "Bob",
};

const template = `
<div>
    Hello: \${name}!!!!
</div>
`;

<div class="content-ad"></div>

테이블 태그를 마크다운 형식으로 변경해주세요.

<div class="content-ad"></div>

바인딩: $'`value` ! propertyName'

다음은 이에 대한 예시입니다:

const model = {
  darkMode: true,
};

const template = `
<div>
    I am using \${'Dark mode' = darkMode} \${'Light mode' ! darkMode} now!
</div>
`;

모델의 darkMode 속성의 값에 따라 다른 텍스트가 렌더링됩니다. 만약 true라면 div는 "나는 지금 다크 모드를 사용 중입니다!"라고 표시되고, false로 설정되어 있다면 "나는 지금 라이트 모드를 사용 중입니다!"라고 렌더링됩니다.

<div class="content-ad"></div>

### 일회성 텍스트 바인딩

이전 바인딩은 RequestAnimationFrame()에 대해 지속적으로 평가됩니다. 그러나 때로는 초기화 시와 같이 한 번만 평가하고 싶을 수도 있습니다. Peasy-UI는 일회성 바인딩을 제공합니다.

바인딩: $'|`값` = 속성'

바인딩: $'|`값` ! 속성'

<div class="content-ad"></div>

위의 예시를 재사용할 수 있습니다. 단, darkMode 속성의 평가는 첫 번째 렌더링 주기에서만 수행됩니다. 바인딩에 파이프(|) 문자가 있는 점을 유의해주시기 바랍니다. 이것이 한 번만 바인딩된 것을 나타냅니다.

## 속성 바인딩

속성 바인딩은 요소의 동작에 영향을 미치기 위해 \$' ' 또는 요소 태그 내에서 pui 속성을 사용합니다. 이 글에서는 계속해서 \$' ' 표기법을 사용하겠습니다. 다음 글에서는 바인딩 할당의 다양한 방법을 살펴보겠습니다.

### 요소 속성을 위한 바인딩

<div class="content-ad"></div>

Peasy-UI를 통해 요소 속성에 액세스할 수 있고 해당 값을 쉽게 모니터링하고 제어할 수 있습니다.

네 가지 기본 요소 속성 바인딩이 있습니다:

바인딩: `$'attribute `== propertyName'`은 모델에서 바인딩으로 일방향으로 바인딩합니다.

바인딩: `$'attribute `=| propertyName'`은 모델에서 바인딩으로 일회성으로 일방향으로 바인딩합니다.

<div class="content-ad"></div>

태그를 Markdown 형식으로 변경해 주세요.

BINDING: $'속성 ==` 속성명' 요소에서 모델로 단방향으로 바인딩됩니다

BINDING: $'속성 `=` 속성명' 요소와 모델 간에 양방향 또는 양쪽으로 연결됩니다

이것의 예시를 살펴보겠습니다:

const model = {
  color: "red",
};

const template = `
<div>
    <input type="radio" \${'red' ==> color}>Red</input>
    <input type="radio" \${'green' ==> color}>Green</input>
</div>

<div class="content-ad"></div>

이 예제에서는 모델의 color 속성에 각 요소 'red'와 'green'에 사용자 정의 속성을 바인딩하고 있습니다. 이 그룹의 라디오 버튼이 선택되면 모델 속성 'color'의 데이터가 변경됩니다.

또 다른 간단한 예제:

const model = {
  userName: "",
};

const template = `
<div>
    <label>사용자 이름 입력: </label>
    <input type="text" \${value ==> userName}></input>
</div>

<div class="content-ad"></div>

양방향 예제를 살펴봅시다:

const model = {
  userName: "Bob",
};

const template = `
<div>
    <input type="text" \${value <=> userName}>\${userName}</input>
</div>

여기에 존재하는 마법은 데이터 모델에서 username을 프로그래밍 방식으로 설정할 수 있으며, 자동으로 입력란에 렌더링된 텍스트를 변경할 수 있다는 것입니다. 또는 입력란에 타이핑하여 데이터 모델의 값이 동적으로 변경됩니다.

### 이벤트에 대한 바인딩

<div class="content-ad"></div>

만약 드롭다운 선택 입력란 또는 버튼을 렌더링하고 싶다면 어떻게 하면 될까요? Peasy-UI는 이벤트 리스너를 자동으로 매핑하고 데이터 모델에 연결할 수 있게 해줍니다.

바인딩: $'이벤트 @=`콜백이름'

버튼은 쉬운 예제를 제공하지만, 모든 DOM 이벤트를 사용할 수 있습니다.

const model = {
  clickHandler: (event: HTMLEvent, model: any, element: HTMLElement, eventType: string, object: any) => {
    window.alert("클릭됐어요!");
  },
};

const template = `
<div>
    <button \${click @=> clickHandler}> 클릭해주세요! </button>
</div>

<div class="content-ad"></div>

알겠어요. 여기서는 조금 깊게 파고들어보도록 하죠. 먼저 바인딩에 집중해 보겠습니다. HTML DOM 이벤트(예: change, input, click, mouseenter 등)를 이벤트 바인딩으로 바인딩할 수 있으며, 그런 다음 'handler' 콜백을 제공합니다. 이 'handler' 콜백은 데이터 모델 안에 존재합니다.

Peasy는 편의를 위해 핸들러에 5가지 표준 매개변수를 전달합니다. 첫 번째 매개변수는 실제로 발생한 HTMLEvent입니다. 두 번째 매개변수는 엘리먼트에 바인딩된 'localized' 데이터 모델입니다. 이 부분은 약간 tricky 할 수 있습니다. 엘리먼트의 중첩에 따라 다르지만, 그 엘리먼트에 대한 localized 데이터 모델이 될 수도 있고, 최상위 바인딩인 경우 표준 데이터 모델이 될 수 있습니다. 나중에 중첩에 대해 자세히 다룰 것이지만, 이를 돕기 위한 다른 옵션이 제공된다는 것만 알아두세요.

세 번째 매개변수는 이벤트를 발생시킨 대상 엘리먼트이므로 엘리먼트 중첩된 어떤 속성에든 접근할 수 있습니다. 네 번째 요소는 'click'이나 'change'와 같은 이벤트의 문자열 표현이며, 이는 동일한 콜백에 여러 이벤트를 바인딩했을 경우 어떤 이벤트가 이 콜백을 트리거했는지 구별하는 데 도움이 됩니다. 마지막 매개변수는 대규모 데이터 모델입니다. 이 객체 내에서, 바인딩이 얼마나 중첩되었든 상관없이 모델 속성을 포함하는 객체가 있으며, 해당 객체를 탐색하여 데이터 모델의 모든 속성에 액세스할 수 있습니다.

### 데이터 모델에 전체 엘리먼트 바인딩하기

<div class="content-ad"></div>

자바스크립트에서 document.getElementById(`myElement`)를 수행해야 하는 것이 얼마나 거슬리는지 알고 계시나요? 네, Peasy-UI가 그것을 좀 더 쉽게 만들어 줍니다.

바인딩: $' ==` propertyName'

이 시간을 절약하는 예제를 살펴봅시다:

const model = {
  myInputElement: undefined as HTMLElement | undefined,
};

const template = `
<div>
    <input type="text" \${==> myInputElement} />
</div>

<div class="content-ad"></div>

이 바인딩은 렌더링된 DOM 요소를 매핑하고 해당 참조를 모델 내에 바인딩합니다. 이 예제에서는 getElementById() 메서드를 사용하는 대신 모델 내에 참조가 들어 있습니다. 첫 번째 렌더링 사이클 후에 model.myInputElement로 참조할 수 있습니다. 요소는 `먼저` 렌더링되어야하며, 그 참조가 데이터 모델에 캡처됩니다. 따라서 기본적으로 undefined로 설정되어 있으며, HTML이 DOM에 삽입된 후 요소 참조로 설정됩니다. 따라서 요소 값에 액세스하려면 model.myInputElement.value 또는 해당 요소의 속성에 액세스할 수 있습니다.

### 렌더링을 위한 바인딩

만약 특정 조건하에 렌더링되는 부분을 선택적으로 변경하고 싶다면 어떨까요? Peasy-UI를 사용하면 전반적인 렌더링을 제어할 수 있습니다.

바인딩: $'=== 속성이름'

<div class="content-ad"></div>

BINDING: $'!== propertyName'

이 바인딩의 예제를 살펴 봅시다:

const model = {
  isDog: true,
};

const template = `
<div>
    <div \${=== isDog}> 나는 개입니다</div>
    <div \${!== isDog}> 나는 고양이입니다</div>
</div>

이 예제에서 하나의 바인딩된 div만 렌더링됩니다. 데이터 모델인 isDog에서 true와 false를 토글하여 렌더링되는 div를 제어할 수 있습니다.

<div class="content-ad"></div>

### 배열을 위한 바인딩

가끔 DOM에 항목을 나열해야 할 때가 있습니다. 포럼 글, 셀렉트 입력의 옵션 또는 내비게이션 바의 목록 항목 등이 그 예시일 수 있습니다. Peasy-UI는 순회 가능한 렌더러를 제공하여 데이터 모델의 목록이나 배열을 여러 요소 렌더링으로 변환할 수 있도록 도와줍니다.

바인딩: $' item `=* propertyArray'

간단한 예제로 설명해보겠습니다:

<div class="content-ad"></div>

const model = {
  myItems: [{text: 'one', value: '1'},{text: 'one', value: '1'},{text: 'one', value: '1'}],
};

const template = `
<div>
    <select>
      <option \${opt <=* myItems} value="\${opt.value}">\${opt.text}</option>
    </select>
</div>

이 사용 사례는 데이터 모델의 `myItems` 속성 배열을 반복하고 선택한 태그에 각기 다른 옵션 요소를 렌더링합니다.

각 렌더링된 옵션 태그에는 자체 모델 범위가 있습니다. opt.text 및 opt.value와 같이 볼 수 있습니다.

### 구성 요소를 위한 바인딩
```

<div class="content-ad"></div>

큰 프로젝트를 다룰 때는 코드 베이스를 조직화하고 프로젝트를 관리할 수 있는 코드 분할을 활용하는 것이 바람직합니다. Peasy-UI는 컴포넌트 기반 디자인을 가능하게 합니다. 이 기사에서는 이 주제를 조금 다루겠습니다. 전체적인 컴포넌트 기능에 대해 적절히 탐구하기 위해 별도의 기사가 작성될 것입니다.

Peasy-UI에서의 컴포넌트는 정적인 `템플릿` 속성과 `create` 메서드를 포함하는 JavaScript 객체입니다.

바인딩: $'ComponentName === state'

다음에는 더 많은 예제를 살펴보겠습니다.

<div class="content-ad"></div>

```js
class MyComponent{
  public static template = `
  <div>
      <div> Hello \${name}</div>
      <div> My count is: \${count}</div>
  </div>
  `;

  constructor(public name: string,public count:number){}

  static create( state: {name:string, count:number}){
    return new MyComponent(state.name, state.count)
  }
}

const model = {
  MyComponent,
  componentData:{
    name: 'bob',
    count: 10,
  }
};

const template = `
<div>
    <\${MyComponent === componentData}>
</div>
```

따라서 MyComponent 클래스는 들어오는 상태를 수락하는 정적 create 메서드와 public 정적 템플릿이 있어야합니다. Peasy-UI가 할 일은 각 구성 요소에 내부 상태로 로컬 클래스 속성을 설정하고 클래스 템플릿을 전체 프로젝트 템플릿에 주입하는 것입니다.

이를 통해 대규모 프로젝트를 작은 조각으로 나눌 수 있습니다. 컴포넌트로 수행할 수있는 작업은 훨씬 더 많으며 자세히 탐색할 별도의 기사가 필요합니다.

## 게터(Getters)


<div class="content-ad"></div>

최종 바인딩 아이디어 중 하나는 데이터 모델에서 getter를 사용하는 방법 및 그들이 제공하는 내용을 다루는 것입니다. 때로는 간단한 원시 값으로는 데이터 모델을 충분히 표현할 수 없을 때가 있습니다. 그럴 때 getter는 더 복잡한 로직을 제공하고 반환 값은 바인딩 값으로 제공하는 수단을 제공합니다. 여기서 다룰 특별한 고유한 바인딩 태그는 없지만, 예시를 살펴보겠습니다:

```js
const model = {
  val1: 1,
  val2: 3,
  get myGetter(){
      return this.val1 + this.val2;
  },
};

const template = `
<div>
    <span>My getter value is: \${myGetter}</span>
</div>
```

getter를 사용하는 좋은 점은 매 렌더링 루프마다 다시 계산된다는 것입니다. 따라서 해당 로직 내에서 어떤 값이 변경되더라도 업데이트될 것입니다.

## 더 많은 정보

<div class="content-ad"></div>

Peasy-Lib의 GitHub 저장소에서 더 많은 정보를 찾을 수 있어요. 거기서 다른 동반 패키지도 찾을 수 있어요. 또한, Peasy에는 우리가 만나서 Peasy에 대해 토론하고 서로 도와주는 디스코드 서버가 있어요.

저자의 트위터: Here

저자의 itch: Here

GitHub Repo: Here

<div class="content-ad"></div>

디스코드 서버: [여기](링크)

## 결론

이 글에서는 Peasy-UI 라이브러리의 데이터 바인딩 기본 사항을 모두 다루었습니다. 이 데이터 바인딩은 라이브러리가 DOM에 필요한 것을 렌더링하는 데 사용되는 마법입니다. 텍스트 바인딩과 속성 바인딩에 대해 논의했습니다. 각각에 대해 바인딩 구문과 예제에서 사용할 수 있는 방법을 다루었습니다.

다음 글에서는 이벤트에 대해 더 깊이 파고들 것입니다.