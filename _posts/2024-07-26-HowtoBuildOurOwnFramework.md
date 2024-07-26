---
title: "우리만의 프레임워크를 만드는 방법"
description: ""
coverImage: "/assets/img/2024-07-26-HowtoBuildOurOwnFramework_0.png"
date: 2024-07-26 11:59
ogImage: 
  url: /assets/img/2024-07-26-HowtoBuildOurOwnFramework_0.png
tag: Tech
originalTitle: "How to Build Our Own Framework"
link: "https://dev.to/slydragonn/how-to-build-our-own-framework-2eoo"
---


안녕하세요 여러분!

이 글에서는 객체 지향 프로그래밍과 원자 디자인을 기반으로 프론트엔드를 개발하기 위한 프레임워크 또는 라이브러리를 만드는 것이 무엇을 의미하는지 알아보겠습니다.

## 주제

- 개념
- 아키텍처 및 기술
- 렌더링
- 상태
- 생명주기
- 전역 상태
- npm 게시
- 결론

<div class="content-ad"></div>

## Youtube Video

## Concept

프레임워크란 개발자들이 응용 프로그램을 더 효율적이고 일관되게 구축하는 데 도움을 주기 위해 설계된 도구, 라이브러리 및 관행들의 집합입니다.

생각은 요소 렌더링, 상태 제어 및 라이프사이클과 같은 기능을 제공하는 작은 패키지를 만들고, 당연히 모든 코드를 번들로 묶어 npm에 게시하는 것입니다.

<div class="content-ad"></div>

## 아키텍처 및 기술

소프트웨어 아키텍처는 소프트웨어 시스템의 기본 구조로, 구성 요소, 그들 사이의 관계 및 운영 환경을 포함합니다.

어떤 프로젝트를 시작하기 전에는 기술을 정의하고 어떤 아키텍처를 따를지 먼저 결정하는 것이 중요합니다.

이번에는 모든 기능을 개발할 때 JavaScript 언어와 패키징 도구로 Vitejs를 사용할 예정입니다.

<div class="content-ad"></div>

원시적인 설계와 객체 지향 프로그래밍 개념을 기반으로 삼겠다고 결심했어요. 즉, HTML 요소를 클래스처럼 만들어 사용할 생각이에요.

## 렌더링

먼저, 프론트엔드 프레임워크가 JavaScript 코드에서 HTML 요소를 보다 간편하게 렌더링할 수 있는 것이 가장 중요하다고 생각해요.

이 작업을 수행하기 위해 DOM API를 사용할 것이고, 프레임워크는 Classy JS로 이름 짓겠습니다. 개발 의존성으로 vitejs를 사용할 거에요.

<div class="content-ad"></div>

자바스크립트 내에서 HTML 요소를 생성하는 방법은 다음과 같습니다:

```js
// p, h1, button 또는 input과 같은 HTML 요소 만들기
let element = document.createElement("elementType");

// DOM에 추가하기
let parent = document.getElementById("parentId")
parent.appendChild(element)
```

새로운 vite(비테) 프로젝트를 시작하고 이번 기회에는 순수 자바스크립트만 사용할 것입니다:

```js
npm create vite@latest
```

<div class="content-ad"></div>

vite.config.js 파일을 사용하여 vite를 구성하고 라이브러리의 진입점과 이름을 추가합니다. 모든 로직은 src 폴더 안에 있을 것입니다.

```js
import { defineConfig } from "vite";

export default defineConfig({
  build: {
    lib: {
      entry: "src/index.js",
      name: "classyjs",
      fileName: (format) => `classyjs.${format}.js`,
    },
    rollupOptions: {
      external: [],
      output: {
        globals: {},
      },
    },
  },
});
```

원자 디자인을 따르면 각 HTML 요소는 정의상 원자입니다. 각 원자는 자신과 자식 요소를 렌더링할 수 있으며 고유의 상태를 가집니다.

createAtom 함수는 우리가 원하는 HTML 요소를 생성하고 고유한 키를 할당하며, 자체 상태로 시작하도록 책임을 지게 될 것입니다. 우리는 이 함수에 전달할 것인데, 이 때문에 우리가 생성하려는 HTML 요소와 함께 상태도 전달할 것입니다.

<div class="content-ad"></div>

```js
export default function createAtom(atomType, initialState) {
  let atom = document.createElement(atomType);
  let key = parseInt(Math.random() * 10000000000);
  let state = initialState;

  atom.setAttribute("data-key", key);

  const setAtomValues = (newState) => {
    for (let [stateName, stateValue] of Object.entries(newState)) {
      for (let [name, value] of Object.entries(stateValue)) {
        if (stateName === "children") {
          if (typeof value === "object") {
            value.render(atom);
          } else if (typeof value === "number" || typeof value === "string") {
            atom.innerText = value;
          } else {
            atom.appendChild(value);
          }
        } else if (stateName === "props") {
          atom[name] = value;
        } else if (stateName === "attrs") {
          atom.setAttribute(name, value);
        } else if (stateName === "events") {
          atom.addEventListener(name, value);
        }
      }
    }
  };

  setAtomValues(state);

  const updateState = (newState) => {
    setAtomValues(newState);
    return atom;
  };

  return {
    render: atom,
    key,
    updateState,
  };
}
```

이 함수는 요소, 고유 키 및 속성 값, 자식, 속성 및 이벤트의 할당을 담당하는 메소드를 리턴하며, 일반적으로 이를 사용하여 아톰의 상태를 업데이트합니다.

## 상태 및 라이프사이클

다음 단계로, createAtom 함수의 모든 기능을 Atom 클래스로 추상화하고, 이를 통해 아톰의 라이프사이클(마운트된 상태, 언마운트된 상태, 업데이트된 상태)을 듣는 기능을 제공하는 것을 계획하고 있습니다.


<div class="content-ad"></div>

```js
import createAtom from "./createAtom";

export default class Atom {
  constructor(atomType, initialState, lifeCycle = {}) {
    this.state = initialState;
    this.lifeCycle = lifeCycle;
    this.atom = createAtom(atomType, this.state);
    this.parent;
  }
  render(parent) {
    this.parent = parent;
    this.parent.appendChild(this.atom.render);
    if (this.lifeCycle?.mount) {
      return this.lifeCycle.mount();
    }
    return;
  }
  update(newState, callback) {
    let state = { ...this.state, ...newState };
    if (typeof newState.children !== "object") {
      state = {
        ...this.state,
        ...newState,
        children: {
          child: newState.children,
        },
      };
    }
    this.state = state;
    const newAtom = this.atom.updateState(this.state);
    const oldAtom = this.parent.querySelector(`[data-key="${this.atom.key}"]`);
    if (oldAtom && oldAtom.parentNode) {
      oldAtom.parentNode.replaceChild(newAtom, oldAtom);
      if (typeof callback === "function") {
        callback(this.state);
      }
      if (this.lifeCycle?.mount) {
        return this.lifeCycle.mount();
      }
      return;
    }
    this.parent.appendChild(newAtom);
    if (typeof callback === "function") {
      callback(this.state);
    }
    if (this.lifeCycle?.mount) {
      return this.lifeCycle.mount;
    }
  }
  remove() {
    const atom = this.parent.querySelector(`[data-key="${this.atom.key}"]`);
    atom.remove();
    if (this.lifeCycle?.unmount) {
      return this.lifeCycle.unmount();
    }
    return;
  }
}
```

이 클래스는 원자 유형, 초기 상태 및 수명 세 가지 매개변수를 받습니다.

원자 유형은 생성하려는 HTML 요소의 유형입니다. 예: 입력(Input), 디브(Div), 또는 버튼(Button).

초기 상태는 4가지 프로퍼티(children, attributes, properties, events)를 갖는 객체입니다. 각각의 프로퍼티는 원자의 상태에 속하며 언제든지 업데이트할 수 있습니다.


<div class="content-ad"></div>

```js
const initialState = {
    children: {
        text: "Hello"
    },
    props: {
        style: "color:red;"
    },
    events: {
        click: () => alert("Message")
    },
    attrs: {
        class: "my-class"
    }
}
```

라이프사이클은 mount와 unmount 두 가지 속성을 가진 객체입니다. 이것들은 요소가 마운트되거나 언마운트될 때 실행되는 콜백의 쌍입니다.

```js
const lifeCycle = {
    mount: () => console.log("atom did mount"),
    unmount: () => console.log("atom did unmount")
}
```

Atom 클래스에는 render, update 및 remove 3가지 메서드도 있습니다. 그 이름에서 알 수 있듯이, 요소 또는 atom을 DOM에서 렌더링, 업데이트 및 제거하는 역할을합니다.

<div class="content-ad"></div>

위의 코드에서 업데이트 메소드는 두 개의 매개변수를 받습니다. 새 상태와 해당 요소가 업데이트될 때마다 실행되는 콜백입니다.

```js
let count = 0

const atom = new Atom({
    children: {count}
})

count++

atom.update({
    children: {count}
}, () => console.log("atom updated"))
```

위의 내용을 토대로 필요한 모든 종류의 HTML 요소에 대한 원자를 생성할 수 있습니다. ul 요소를 예로 들면, 이는 목록을 나타내며 저희 프레임워크에서는 분자로 나타낼 수 있고, li 요소는 원자가 될 것입니다.

```js
import Atom from "../atoms/Atom";

// 분자
class List extends Atom {
  constructor(ListType, initialState, lifeCycle) {
    super(
      { ul: "ul", ol: "ol" }[ListType],
      {
        ...initialState,
        children: Object.assign({}, initialState.children),
      },
      lifeCycle,
    );
  }
}

// 원자
class ListItem extends Atom {
  constructor(initialState, lifeCycle) {
    super("li", initialState, lifeCycle);
  }
}
```

<div class="content-ad"></div>

우리의 아톰을 사용하려면 다음을 수행해야 합니다.

```js
const listItem = new ListItem({
    children: { text: "I am a li" }
})

const list = new List({
    children: [ listItem ]
}, {
    mount: () => console.log("List did mount")
})

list.render(document.getElementById("parentId"))
```

## 전역 상태

전역 상태를 관리하기 위해 createContext라는 함수를 만들겠습니다. 이 함수는 브라우저의 세션 저장소에 전달된 상태를 저장하여 응용 프로그램의 모든 페이지에서 액세스할 수 있도록 합니다. 이 함수는 두 개의 매개변수를 받습니다: 컨텍스트 이름과 상태입니다. 그리고 상태와 상태를 업데이트하는 메서드를 반환합니다.

<div class="content-ad"></div>

```js
export default function createContext(name, initialState) {
  const key = `context-${name}`;
  let state;

  if (sessionStorage.getItem(key)) {
    state = JSON.parse(sessionStorage.getItem(key));
    console.log("state", state);
  } else {
    state = initialState;
    sessionStorage.setItem(key, JSON.stringify(state));
  }

  const setState = (callback) => {
    const newState = callback(state);
    state = newState;
    sessionStorage.setItem(key, JSON.stringify(newState));
    return newState;
  };

  return {
    initialState: state,
    setState,
  };
}
```

## npm에 게시하기

먼저 npm run build 명령을 실행하여 라이브러리가 패키지되도록 해야하지만, 먼저 package.json 파일을 구성해야 합니다:

```json
{
  "name": "@slydragonn/classyjs",
  "version": "1.1.3",
  "publishConfig": {
    "access": "public"
  },
  "description": "Framework Frontend based on OOP and Atomic design",
  "main": "dist/classyjs.umd.js",
  "module": "dist/classyjs.es.js",
  "files": [
    "dist"
  ],
  "scripts": {
    "build": "vite build"
  },
  "keywords": [],
  "author": "slydragonn",
  "license": "MIT",
  "devDependencies": {
    "vite": "^5.3.3"
  },
  "repository": {
    "type": "git",
    "url": "git+https://github.com/slydragonn/classyjs.git"
  }
}
```

<div class="content-ad"></div>

npm은 dist 폴더를 React와 같은 패키지로 공유할 것입니다. 이를 게시하려면 npm 계정을 만들어야 합니다. 계정이 없다면 npm login 명령으로 로그인하고 npm publish 명령으로 프로젝트를 게시하면 npm 페이지에서 확인할 수 있습니다.

![이미지](/assets/img/2024-07-26-HowtoBuildOurOwnFramework_0.png)

이 프로젝트를 확인하고 싶다면, 모든 사용 방법에 대한 자세한 내용이 있는 저장소로 이동해보세요.

저장소: https://github.com/slydragonn/classyjs

<div class="content-ad"></div>

## 결론

결론적으로, 프레임워크는 개발 시간을 절약하는 데 도움이 되는 우수한 도구일 뿐만 아니라 좋은 실천 방법으로 프로그램을 작성하고 훨씬 더 적은 코드로 복잡한 것들을 만드는 데 도움이 됩니다.

프레임워크를 만드는 것은 쉬운 작업이 아닙니다. 이 기사에서도 프레임워크가 정말 무엇인지의 10%만 다룬 것이지만, 저는 이것이 흥미로운 프로젝트이며 많은 개념을 다루는 것을 좋아한다면 확실히 재미있는 작업이 될 것이라고 생각합니다.

좋아했으면 좋겠구요! 나중에 뵙겠습니다!