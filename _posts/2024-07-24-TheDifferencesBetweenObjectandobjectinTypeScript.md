---
title: "TypeScript에서 Object, , object의 차이점 이해하기"
description: ""
coverImage: "/assets/img/2024-07-24-TheDifferencesBetweenObjectandobjectinTypeScript_0.png"
date: 2024-07-24 11:59
ogImage: 
  url: /assets/img/2024-07-24-TheDifferencesBetweenObjectandobjectinTypeScript_0.png
tag: Tech
originalTitle: "The Differences Between Object, , and object in TypeScript"
link: "https://dev.to/zacharylee/the-differences-between-object-and-object-in-typescript-f6f"
---


TypeScript에서 객체 유형을 정의하려면 `Object`, `''`, `object`과 같이 간결한 옵션이 여러 가지 있습니다. 이들 간의 차이는 무엇인가요?

## Object (대문자)

대문자로 된 Object는 모든 JavaScript 객체에 공통된 속성을 설명합니다. TypeScript 라이브러리와 함께 제공되는 lib.es5.d.ts 파일에서 정의됩니다.

![차이](/assets/img/2024-07-24-TheDifferencesBetweenObjectandobjectinTypeScript_0.png)

<div class="content-ad"></div>

알 수 있듯이, 그것은 toString(), valueOf(), 등과 같은 일반적인 속성들을 포함하고 있습니다.

이것은 JavaScript 객체에 일반적인 속성만 강조하기 때문에, 문자열, 부울, 숫자, bigint, 심볼과 같은 객체들을 할당할 수 있지만, 반대로는 할 수 없습니다.

<img src="/assets/img/2024-07-24-TheDifferencesBetweenObjectandobjectinTypeScript_1.png" />

## ''

<div class="content-ad"></div>

''라는 것은 자신의 멤버가 없는 객체를 설명하며, 이는 TypeScript가 속성 멤버에 액세스하려고 시도하면 오류가 발생한다는 것을 의미합니다:

![image](/assets/img/2024-07-24-TheDifferencesBetweenObjectandobjectinTypeScript_2.png)

위의 코드 예제에서 확인할 수 있듯이, ''와 Object(대문자로 표시된)는 동일한 기능을 가지고 있습니다. 다시 말해, JavaScript 코드 논리가 맞더라도 공통 속성에만 액세스할 수 있고, 모든 박싱 가능한 객체를 할당할 수 있습니다.

왜냐하면 '' 타입은 프로토 타입 체인을 통해 해당 공통 속성에 액세스할 수 있을 뿐만 아니라 자체 속성이 없기 때문입니다. 따라서 Object(대문자로 표시된) 타입과 동일하게 작동합니다. 그러나 다른 개념을 나타냅니다.

<div class="content-ad"></div>

## object (소문자로 작성)

object (소문자로 작성)은 코드에서 다음과 같이 표현된 모든 원시 자료형이 아닌 타입을 의미합니다:

```js
type PrimitiveType =
  | undefined
  | null
  | string
  | number
  | boolean
  | bigint
  | symbol;

type NonPrimitiveType = object;
```

이는 모든 비 원시 자료형이 다를 수 없으며 그 반대도 마찬가지입니다.

<div class="content-ad"></div>


![이미지](/assets/img/2024-07-24-TheDifferencesBetweenObjectandobjectinTypeScript_3.png)

## 간식: 레코드

많은 일반 라이브러리의 소스 코드에서 우리는 비 기본 타입을 나타내기 위해 `Record<string, any>`을 볼 수 있습니다. 이는 object (소문자로 표기된)와 동일한 효과를 내지만, 보다 의미론적인 면이 더해졌습니다.

내 컨텐츠가 도움이 되었다면 구독을 고려해 주세요. 매주 일요일에 최신 웹 개발 업데이트가 포함된 뉴스레터를 발송합니다. 지금까지 지지해 주셔서 감사합니다!
