---
title: "TypeScript에서 배열의 인덱스를 추출하는 방법"
description: ""
coverImage: "/assets/img/2024-07-06-HowtoExtractTheIndexesofAnyArrayinTypeScript_0.png"
date: 2024-07-06 09:57
ogImage:
  url: /assets/img/2024-07-06-HowtoExtractTheIndexesofAnyArrayinTypeScript_0.png
tag: Tech
originalTitle: "How to Extract The Indexes of Any Array in TypeScript"
link: "https://medium.com/gitconnected/i-discovered-this-cool-typescript-trick-dbe075bdbb3c"
---

/assets/img/2024-07-06-HowtoExtractTheIndexesofAnyArrayinTypeScript_0.png

매트 포콕의 트윗을 발견했어요. 정말 흥미로운 내용이에요!

문제는 매트가 이 속임수를 자세히 설명해주지 않는다는 거에요.

또한, 이 속임수를 사용할 수 있는 잠재적인 사용 사례를 제시하지 않는다는 게 조금 아쉬운 부분이에요. 😔

<div class="content-ad"></div>

맷에게는 놓친 기회로 보입니다. 오늘은 여러분에게 이 트릭을 설명하고 TypeScript 능력을 향상시키는 데 도움이 될 것입니다!

그러니 더 이상 말설이 없이... 바로 시작해 봅시다! 👇

### ToNumber 타입

ToNumber 타입의 목적은 배열 인덱스와 유사한 키를 받아 숫자로 변환하는 것입니다. 예를 들어 "42"는 숫자 42로 파싱됩니다.

<div class="content-ad"></div>

아래는 ToNumber 함수의 코드와 설명입니다:

```js
type ToNumber<Key extends PropertyKey> = Key extends `${infer Index extends number}` ? Index : never;
```
