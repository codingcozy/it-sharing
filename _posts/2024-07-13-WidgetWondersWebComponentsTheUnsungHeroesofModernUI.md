---
title: "위젯의 놀라움 현대 UI의 숨은 영웅, 웹 컴포넌트 사용의 장점"
description: ""
coverImage: "/assets/img/2024-07-13-WidgetWondersWebComponentsTheUnsungHeroesofModernUI_0.png"
date: 2024-07-13 18:33
ogImage:
  url: /assets/img/2024-07-13-WidgetWondersWebComponentsTheUnsungHeroesofModernUI_0.png
tag: Tech
originalTitle: "Widget Wonders: Web Components: The Unsung Heroes of Modern UI"
link: "https://medium.com/@guhaprasaanth/widget-wonders-web-components-the-unsung-heroes-of-modern-ui-052131d692be"
---

![이미지](/assets/img/2024-07-13-WidgetWondersWebComponentsTheUnsungHeroesofModernUI_0.png)

웹 구성 요소는 개발자들이 기능을 캡슐화하고 코드베이스의 나머지 부분에 영향을 미치지 않고 재사용 가능한 사용자 지정 요소를 생성할 수 있는 강력한 기술 모음을 나타냅니다. 이 기술은 JavaScript 프레임워크로 가득 찬 UI 환경에서 눈에 띄며 구성 요소 빌드에 표준 기반 접근 방식을 제공합니다. 여기에는 웹 구성 요소, 그들의 진화 및 현대 JavaScript 프레임워크와 상호 작용하는 방법이 코드 예제를 포함하여 탐색됩니다.

# 웹 구성 요소 소개

웹 구성 요소는 네 가지 주요 명세를 기반으로 합니다:

<div class="content-ad"></div>

- 사용자 정의 요소: 이는 사용자가 정의한 템플릿, 동작 및 태그 이름을 가진 완전한 유효한 HTML 요소입니다.
- 그림자 DOM: 구성 요소 내의 범위가 지정된 하위 트리로, 기능을 비공개로 유지하여 주 문서의 CSS와 JavaScript가 침범하지 않습니다.
- HTML 템플릿: `template` 및 `slot` 요소를 사용하여 문서에 추가되기 전에 렌더링되지 않는 마크업 템플릿을 작성할 수 있습니다.
- ES 모듈: 자바스크립트 모듈로, 스크립트를 더 효율적으로 관리하고 캡슐화할 수 있습니다.

# 웹 컴포넌트의 진화

웹 컴포넌트는 개발자가 "바퀴를 재발명"하는 것을 피하도록 도와줄 목적으로 2011년쯤 처음 소개되었습니다. 이후 여러 해 동안 크게 발전하였습니다:

- 초기 단계 (2011–2014): 기본 기능과 함께 소개됨.
- V0 API (2014–2016): 초기 구현이 V0 API로 이끌어졌으며, 몇몇 프로젝트에서 사용되었지만 결국 폐기되었습니다.
- V1 API (2016년부터 현재): 사용자 정의 요소와 그림자 DOM V1과 같은 API의 표준화로, 더 나은 호환성과 견고성을 제공합니다.

<div class="content-ad"></div>

# 웹 구성요소와 JavaScript 프레임워크

React, Angular, Vue와 같은 프레임워크는 각자의 구성요소 모델을 제공하지만, 웹 구성요소는 플랫폼에 독립적인 성격을 가지고 있어서 이러한 프레임워크와 상호 운용이 가능합니다. 예를 들어, React 애플리케이션 내에서 일반적인 React 구성요소처럼 웹 구성요소를 사용할 수 있습니다.

# 코드 예제

## 사용자 정의 엘리먼트 정의

<div class="content-ad"></div>

다음은 쉐도우 DOM을 포함하는 사용자 정의 요소의 간단한 예제입니다:

```js
class UserCard extends HTMLElement {
  constructor() {
    super();
    this.attachShadow({ mode: "open" });
    this.shadowRoot.innerHTML = `
            <style>
                :host {
                    display: block;
                    border: 1px solid #ccc;
                    padding: 10px;
                    width: 200px;
                }
            </style>
            <img src="" alt="사용자 이미지">
            <div>
                <h3></h3>
                <p></p>
            </div>
        `;
  }
  connectedCallback() {
    this.shadowRoot.querySelector("h3").innerText = this.getAttribute("name");
    this.shadowRoot.querySelector("p").innerText = this.getAttribute("info");
    this.shadowRoot.querySelector("img").src = this.getAttribute("image");
  }
}
customElements.define("user-card", UserCard);
```

## 사용자 정의 요소 사용

이 user-card 요소를 HTML에서 직접 사용할 수 있습니다.

<div class="content-ad"></div>

```js
<user-card name="John Doe" info="Software Engineer" image="path/to/image.jpg"></user-card>
```

# Frameworks와의 통합

React 예시:

```js
import React from "react";
import "./UserCard";

function App() {
  return <user-card name="Jane Doe" info="Web Developer" image="path/to/image.jpg"></user-card>;
}
export default App;
```

<div class="content-ad"></div>

이 코드는 React에 웹 컴포넌트를 원활하게 통합할 수 있음을 보여줍니다. React 생태계와 독립성을 유지합니다.

# 웹 컴포넌트의 독특한 제안

- 캡슐화: 웹 컴포넌트는 Shadow DOM을 통해 스타일 및 동작 캡슐화를 제공합니다. 이는 컴포넌트의 내부 작업(CSS 및 JavaScript)이 웹 앱의 나머지 부분이나 다른 컴포넌트와 충돌하지 않음을 의미합니다.
- 상호 운용성: 프레임워크에 독립적이며 다른 JavaScript 프레임워크 또는 순수 JavaScript와 함께 사용할 수 있습니다. 이는 프로젝트의 여러 부분 또는 다른 프로젝트 간에 사용성을 향상시키는데 도움이 됩니다.
- 표준화: 브라우저 표준인 웹 컴포넌트는 추가적인 라이브러리나 도구가 필요하지 않아 실행 시간을 줄이고 성능을 향상시킬 수 있습니다.

# 도전과 제한

<div class="content-ad"></div>

웹 컴포넌트는 여러 장점을 갖고 있지만 몇 가지 도전 과제가 있어요:

- 브라우저 지원: 현대 브라우저는 웹 컴포넌트를 지원하지만 예전 버전에서는 일관성이 없거나 버그가 있을 수 있어요.
- 학습 곡선: 개발자들은 그림자 DOM과 사용자 정의 요소의 개념을 기존 프레임워크 컴포넌트 사용과 비교해 덜 직관적으로 느낄 수 있어요.
- 통합 복잡성: 때로는 웹 컴포넌트를 현대적 프레임워크와 통합하는 것이 추가 구성이나 보일러플레이트 코드가 필요할 수 있어요.

# 코드 샘플:

## 웹 컴포넌트를 사용하여 간단한 툴팁 만들기:

<div class="content-ad"></div>

이 예제는 웹 컴포넌트를 사용하여 기본 툴팁을 만드는 방법을 보여줍니다.

```js
class Tooltip extends HTMLElement {
  constructor() {
    super();
    this.attachShadow({ mode: "open" });
    this.shadowRoot.innerHTML = `
            <style>
                :host {
                    position: relative;
                    display: inline-block;
                }
                .tooltip-content {
                    visibility: hidden;
                    background-color: black;
                    color: white;
                    text-align: center;
                    border-radius: 6px;
                    padding: 5px 10px;
                    position: absolute;
                    z-index: 1;
                }
                :host(:hover) .tooltip-content {
                    visibility: visible;
                }
            </style>
            <slot>툴팁 텍스트</slot>
            <span class="tooltip-content">
                <slot name="content">툴팁 내용을 입력하세요!</slot>
            </span>
        `;
  }
}

customElements.define("my-tooltip", Tooltip);
```

## HTML에서 커스텀 툴팁 사용하기

```js
<my-tooltip>
  위에 마우스를 올려보세요!
  <span slot="content">이것은 툴팁 내용입니다!</span>
</my-tooltip>
```

<div class="content-ad"></div>

# 모던 UI 개발에서의 중요성

- 컴포넌트 재사용성: 마이크로서비스와 마이크로 프론트엔드의 부상으로, 웹 컴포넌트는 다른 시스템과 프레임워크 사이의 다리 역할을 하여 팀이 UI 요소를 쉽게 공유할 수 있습니다.
- 성능: 가벼운 상호작용과 작은 번들 크기가 필요한 프로젝트에서는 웹 컴포넌트가 더 무겁고 느린 JavaScript 프레임워크보다 효율적일 수 있습니다.
- 신흥 패턴: 애플과 구글과 같은 기업들은 일부 웹 애플리케이션에 웹 컴포넌트를 사용하며, 대규모 환경에서의 점점 더 받아들임이 늘어나고 있습니다.

# React와의 통합 예시

```js
import React from "react";

function App() {
  return (
    <div>
      <my-tooltip>
        Hover over me!
        <span slot="content">React is fun!</span>
      </my-tooltip>
    </div>
  );
}
export default App;
```

<div class="content-ad"></div>

웹 구성 요소는 여러 가지 주요 기술로 구성되어 있어요:

- Custom Elements: 개발자가 새로운 HTML 태그를 정의할 수 있게 해줘요.
- Shadow DOM: 스타일과 기능을 캡슐화해 주는 기능을 제공해 주요.
- HTML Templates: `template` 및 `slot` 태그를 사용하여 마크업을 한 번 작성하고 여러 번 인스턴스화할 수 있게 해줘요.

# 자바스크립트 프레임워크 통합 전략

## React

<div class="content-ad"></div>

전략: Props 및 이벤트를 사용한 직접 통합.

- Props 전달: React는 웹 컴포넌트로 데이터를 속성을 통해 전달할 수 있지만 기본적으로 문자열 값이나 함수만 지원됩니다. 객체나 배열 데이터의 경우 JSON 문자열 또는 사용자 지정 이벤트 메커니즘을 사용해야 합니다.
- 이벤트 처리: React의 componentDidMount 라이프사이클 메서드에서 이벤트 리스너를 직접 추가하거나 함수형 컴포넌트에서 hooks를 사용할 수 있습니다.

코드 예시:

```js
import React, { useEffect, useRef } from "react";

function MyComponent() {
  const webComponentRef = useRef(null);
  useEffect(() => {
    function handleCustomEvent(e) {
      console.log("받은 이벤트:", e.detail);
    }
    const webComponent = webComponentRef.current;
    webComponent.addEventListener("customEvent", handleCustomEvent);
    return () => {
      webComponent.removeEventListener("customEvent", handleCustomEvent);
    };
  }, []);
  return <my-web-component ref={webComponentRef}></my-web-component>;
}
export default MyComponent;
```

<div class="content-ad"></div>

## Angular

전략: Angular의 사용자 정의 요소 지원을 활용하여 Web Components와 Angular의 컴포넌트 시스템을 연결합니다.

- 사용자 정의 요소 스키마: Angular 모듈에 CUSTOM_ELEMENTS_SCHEMA를 포함하여 템플릿 구문 분석 오류를 방지합니다.
- 양방향 데이터 바인딩: Angular의 @Input() 및 @Output() 데코레이터를 활용하여 Web Components와 상호 작용합니다.

코드 예시:

<div class="content-ad"></div>

```js
import { Component, NgModule, CUSTOM_ELEMENTS_SCHEMA } from "@angular/core";

@Component({
  selector: "app-root",
  template: `<my-web-component [data]="data" (customEvent)="onCustomEvent($event)"></my-web-component>`,
})
export class AppComponent {
  data = { key: "value" };
  onCustomEvent(event: CustomEvent) {
    console.log("Event data:", event.detail);
  }
}
@NgModule({
  declarations: [AppComponent],
  schemas: [CUSTOM_ELEMENTS_SCHEMA],
})
export class AppModule {}
```

## Vue

전략: 템플릿에서 이벤트와 속성 바인딩을 직접 사용합니다.

- Props 및 Events: Vue는 사용자 지정 요소를 원시 HTML 요소와 유사하게 처리하므로 템플릿에서 속성을 바인딩하고 이벤트를 직접 수신할 수 있습니다.

<div class="content-ad"></div>

# 코드 예시

```js
<template>
  <my-web-component :prop="info" @custom-event="handleEvent"></my-web-component>
</template>

<script>
export default {
  data() {
    return {
      info: { message: 'Vue에서 안부!' }
    };
  },
  methods: {
    handleEvent(e) {
      console.log('받은 내용:', e.detail);
    }
  }
}
</script>
```

# 최상의 실천 방법

- 제한 사항 이해: 각 프레임워크는 데이터와 이벤트 처리하는 특정 방식을 갖고 있습니다. 이를 인식하면 Web Components를 효과적으로 통합할 수 있습니다.
- 호환성을 위한 폴리필 사용: 폴리필을 사용하여 Web Components가 모든 브라우저에서 작동하도록 보장하세요. 특히 이 표준을 기본적으로 지원하지 않는 이전 버전 브라우저에 대해 중요합니다.
- 컴포넌트를 분리 유지: Web Components 내에서 프레임워크에 대한 의존성을 최소화하여 이식성과 재사용성을 유지하세요.
- 성능 고려 사항: Web Components를 통합할 때 성능 영향을 모니터링하세요. 특히 Shadow DOM의 다시 렌더링 및 데이터 업데이트와 관련된 성능 영향에 주의하세요.

<div class="content-ad"></div>

# 보너스 팁: 'slot'이 웹 컴포넌트와 셸벳(Svelte)에서 얼마나 다른가요?

웹 컴포넌트에서의 'slot' 요소와 셸벳(Svelte)의 '#each' 블록 또는 슬롯은 각각의 프레임워크와 기술 내에서 서로 다른 목적을 수행하지만 둘 다 컴포넌트 내의 콘텐츠 배치와 관리와 관련이 있습니다. 여기 'slot'이 웹 컴포넌트에서 셀벳(Svelte)에서 유사한 개념과 어떻게 다른지 살펴보겠습니다:

# 웹 컴포넌트에서의 'slot'

'slot' 요소는 그림자 DOM 명세의 일부로, 웹 컴포넌트 영역 내에서 사용됩니다. 이 요소는 콘텐츠 프로젝션을 가능하게 하며, 개발자가 사용자 정의 요소 내에 자신이 작성한 마크업으로 채워질 수 있는 플레이스홀더를 정의할 수 있습니다. 이는 재사용 가능하고 사용자 정의 가능한 컴포넌트를 생성하는 데 특히 유용합니다.

<div class="content-ad"></div>

주요 특징:

- 콘텐츠 프로젝션: `slot`을 사용하여 외부 콘텐츠를 투영할 수 있는 셰도우 DOM 구조 내에 자리 표시자를 정의합니다.
- 대체 콘텐츠: `slot`은 대체 콘텐츠를 가질 수 있으며, 슬롯에 콘텐츠가 할당되지 않은 경우에 표시됩니다.
- 이름있는 슬롯(Named Slots): 단일 컴포넌트 내에서 여러 이름이 지정된 슬롯을 생성할 수 있어서 컴포넌트 템플릿의 다른 부분을 상세하게 사용자 정의할 수 있습니다.

사용 예시:

```js
<!-- 컴포넌트 내에서 정의 -->
<div>
  <slot name="header">기본 헤더 콘텐츠</slot>
  <slot name="body">기본 본문 콘텐츠</slot>
</div>

<!-- HTML에서 사용 -->
<my-component>
  <span slot="header">사용자 지정 헤더</span>
  <span slot="body">사용자 지정 본문</span>
</my-component>
```

<div class="content-ad"></div>

# Svelte의 Slot

Svelte의 슬롯은 Web Components의 슬롯과 개념적으로 유사하며 구성 요소의 마크업에 미리 정의된 위치에 내용을 삽입할 수 있도록 합니다. 그러나 Svelte 슬롯은 섀도 DOM을 사용하지 않고 빌드 시간에 다시 최적화된 바닐라 JavaScript로 재컴파일되는 컴포넌트 기반 프레임워크의 일부입니다.

주요 특징:

- 기본 슬롯: Svelte는 명시적으로 명명된 슬롯에 대상이 지정되지 않은 모든 콘텐츠가 배치되는 기본 슬롯을 허용합니다.
- 명명된 슬롯: Web Components와 유사하게, Svelte는 명명된 슬롯을 지원합니다.
- 슬롯 프로퍼티: Svelte 슬롯은 슬롯된 콘텐츠로 속성을 전파할 수 있으며, 표준 Web Components에서는 추가 JavaScript 없이 이 기능을 사용할 수 없습니다.

<div class="content-ad"></div>

예시 사용법:

```js
<!-- 컴포넌트 정의 -->
<div>
  <slot name="header">기본 헤더 내용</slot>
  <slot>이름이 지정되지 않은 기본 슬롯 내용</slot>
</div>

<!-- Svelte 앱에서 사용 -->
<MyComponent>
  <div slot="header">사용자 정의 헤더</div>
  <div>기본 슬롯을 위한 다른 내용</div>
</MyComponent>
```

# Svelte에서의 '#each'

Svelte의 '#each' 블록은 배열을 반복하고 해당 데이터를 기반으로 반복 요소를 생성하는 데 사용됩니다. 이는 콘텐츠 프로젝션 메커니즘이 아닌 템플릿 구조입니다.

<div class="content-ad"></div>

키 특징:

- 데이터 루핑: 개발자가 배열이나 순회 가능한 요소의 요소를 기반으로 동적으로 구성 요소나 요소를 생성할 수 있습니다.
- 인덱스 및 키 지정: 각 항목에 대해 인덱스나 키를 사용하여 더 복잡한 상호 작용이나 성능 최적화를 할 수 있습니다.

사용 예시:

```js
<script>
  let items = ['Item 1', 'Item 2', 'Item 3'];
</script>

<ul>
  {#each items as item, index (item)}
    <li>{index}: {item}</li>
  {/each}
</ul>
```

<div class="content-ad"></div>

# 결론:

현재 UI 개발 환경에서 Web Components의 중요성이 상당히 높습니다. 특히 기존 JavaScript 프레임워크를 보완하는 역할로 중요한 역할을 합니다. Web Components는 높은 수준의 구성 요소 재사용, 캡슐화 및 사용자 정의가 필요한 프로젝트에 독특한 기능 세트를 제공합니다. JavaScript 프레임워크를 대체할 순 없지만, Web Components는 현대 개발자들에게 귀중한 도구로 작용하여 웹 애플리케이션이 견고하고 유지보수 가능하며 확장 가능하도록 합니다.
