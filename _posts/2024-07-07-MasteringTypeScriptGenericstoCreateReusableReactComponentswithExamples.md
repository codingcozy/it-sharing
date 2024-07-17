---
title: "예제와 함께 하는 TypeScript 제네릭을 활용한 재사용 가능한 React 컴포넌트 마스터하기"
description: ""
coverImage: "/assets/img/2024-07-07-MasteringTypeScriptGenericstoCreateReusableReactComponentswithExamples_0.png"
date: 2024-07-07 12:33
ogImage:
  url: /assets/img/2024-07-07-MasteringTypeScriptGenericstoCreateReusableReactComponentswithExamples_0.png
tag: Tech
originalTitle: "Mastering TypeScript Generics to Create Reusable React Components with Examples"
link: "https://medium.com/javascript-in-plain-english/mastering-typescript-generics-to-create-reusable-react-components-with-examples-a4f221ed4e47"
---

<img src="/assets/img/2024-07-07-MasteringTypeScriptGenericstoCreateReusableReactComponentswithExamples_0.png" />

프론트엔드 개발에서 React 컴포넌트는 우리의 일상적인 작업의 기반을 형성합니다. TypeScript 제네릭을 사용하면 이러한 컴포넌트를 더 유연하고 재사용 가능하게 만들 수 있습니다. 오늘은 TypeScript 제네릭을 활용하여 재사용 가능한 React 컴포넌트를 만드는 방법을 살펴보겠습니다.

예제 코드를 통해 목표를 성취하는 방법을 단계별로 시연하여 실제 프로젝트에 쉽게 적용할 수 있도록 도와드리겠습니다. 초보자든 숙련자든 가치를 찾을 수 있을 것입니다. 시작해 봅시다!

# 1. TypeScript 제네릭을 사용하여 간단한 재사용 가능한 React 컴포넌트 만들기

<div class="content-ad"></div>

## 1.1 간단한 일반적인 리액트 컴포넌트 생성하기

우선, 어떤 유형의 데이터도 허용하고 render 함수를 사용하여 데이터를 표시할 수 있는 일반적인 React 컴포넌트를 만들어보겠습니다.

```js
type GenericProps<T> = {
  data: T,
  render: (data: T) => React.ReactNode,
};

function GenericComponent<T>({ data, render }: GenericProps<T>): JSX.Element {
  return <div>{render(data)}</div>;
}
```

이 예제에서 GenericComponent는 데이터 형식 T를 취하고 React.ReactNode을 반환하는 함수를 사용하는 render 속성을 허용합니다.

<div class="content-ad"></div>

## 1.2 일반적인... 사용하기
