---
title: "Biome 차세대 코드 포매팅 및 린팅 도구 "
description: ""
coverImage: "/assets/img/2024-07-09-BiomeTheNextGenerationofCodeFormattingandLinting_0.png"
date: 2024-07-09 17:31
ogImage:
  url: /assets/img/2024-07-09-BiomeTheNextGenerationofCodeFormattingandLinting_0.png
tag: Tech
originalTitle: "Biome: The Next Generation of Code Formatting and Linting"
link: "https://medium.com/navara/biome-the-faster-lint-and-formatting-alternative-to-prettier-12fcf8b122b9"
---

여러 해 동안 Prettier는 JavaScript, TypeScript, JSON 등의 코드를 서식 지정하는 데 개발자들 사이에서 널리 사용되어 왔습니다. 그러나 한 가지 단점이 있었습니다: 성능. 웹 개발을 위한 새로운 오픈 소스 도구인 Biome은 더 빠른 서식 지정 도구와 린터를 결합하여 이 문제에 대처합니다.

Rust로 개발된 Biome은 속도와 효율성으로 유명하며 Prettier와 비교하여 많은 성능 이점을 가지고 있습니다. 최근에 Biome은 Prettier의 창시자 중 한 명이 시작한 Prettier 챌린지에서 우승했습니다. 이 챌린지는 Rust에서 prettier 호환 프리티 프린터를 작성하는 것이었습니다.

Biome은 더욱 빠르며 일관적이고 예측 가능한 서식을 제공하여 매우 유용합니다. 또한, 오류 메시지가 더 정보가 풍부하고 도움이 되어 포맷팅 문제를 식별하고 수정하기 쉽습니다.

![이미지](/assets/img/2024-07-09-BiomeTheNextGenerationofCodeFormattingandLinting_0.png)

<div class="content-ad"></div>

## 린터 및 포맷터를 한 번에 처리하는 도구

바이오미(Biome)를 사용하면 코드를 분석, 포맷팅 및 린팅하는 등 다양한 작업을 수행할 수 있습니다. `check` 명령을 사용하면 린터와 포맷 옵션을 모두 실행할 수 있습니다.

```js
npx @biomejs/biome check --apply <파일들>
```

바이오미의 린터는 프레티어(Prettier)보다 더 적극적이라고 느꼈습니다. 잠재적인 버그와 오류를 조기에 발견하여 주요 문제로 발전하는 것을 막습니다. 린터는 즉시 사용되지 않는 변수나 잘못 배치된 괄호와 같은 여러 가지 잠재적인 문제를 찾아냅니다. 이러한 문제는 신속하고 쉽게 해결되어 코드를 더 깨끗하고 조직적으로 만듭니다.

<div class="content-ad"></div>

## 바이옴의 주요 기능

- 속도와 효율성: Rust 프로그래밍 언어를 기반으로 만들어진 바이옴은 우수한 성능을 자랑합니다.
- 간편함: 복잡한 구성 설정이 필요없어 처음부터 사용할 수 있게 해주는 바이옴은 세부 설정을 통해 사용자 선호에 맞게 동작을 조정할 수 있습니다.
- 확장성: 바이옴은 어떤 크기의 프로젝트에도 대응할 수 있도록 설계되어 있어 코드 베이스의 복잡성에 관계없이 일관된 성능을 제공합니다.
- IDE 통합: 바이옴은 VS Code와 IntelliJ IDEA와 같은 인기 있는 IDE 및 코드 편집기와 완벽하게 통합되며 플러그인과 후크를 통해 확장 가능합니다.
- 오류 보고 및 진단: 바이옴은 개발자가 빠르게 문제를 식별하고 해결할 수 있도록 자세하고 맥락 있는 오류 메시지를 제공합니다.

![](/assets/img/2024-07-09-BiomeTheNextGenerationofCodeFormattingandLinting_1.png)

## 접근성

<div class="content-ad"></div>

HTML에서 접근성을 확인하는 것은 웹 개발에서 점점 더 중요해지고 있어요. Biome은 접근성 문제를 식별하는 데 탁월한 성과를 거두고 있어요. 이는 이해하기 쉬운 오류 메시지를 제공하고 그에 대한 해결책을 제시해줘요. 두 가지 예시를 들고 싶어요.

![예시 1](/assets/img/2024-07-09-BiomeTheNextGenerationofCodeFormattingandLinting_2.png)

위의 예시에서 Biome은 role="slider"의 잘못된 사용에서 정확히 무엇을 수정해야 하는지 가리켜 줘요.

![예시 2](/assets/img/2024-07-09-BiomeTheNextGenerationofCodeFormattingandLinting_3.png)

<div class="content-ad"></div>

위 예제에서 Biome는 문제 해결을 위한 해결책을 제공합니다. 이를 통해 개발 중에 문제를 식별하고 해결하기가 더 쉬워집니다.

## Biome 대 Prettier의 성능 평가

Biome는 Prettier 및 parallel-prettier와 비교하는 벤치마크를 실행하는 전용 저장소를 만들었습니다. 이 벤치마크는 다양한 크기와 복잡성의 JavaScript 및 TypeScript 파일 모음을 서식 지정하는 것을 포함합니다.

![image](/assets/img/2024-07-09-BiomeTheNextGenerationofCodeFormattingandLinting_4.png)

<div class="content-ad"></div>

벤치마킹 결과에서는 Biome이 포맷팅 및 린팅 양쪽에서 Prettier보다 우수한 성능을 보여줍니다. Biome은 포맷팅에서 Prettier와 dprint보다 월등히 빠르며, 린팅에서는 ESLint보다 빠릅니다.

여기서 주목할 점은 Biome을 실행할 때 — max-diagnostics=0을 사용한다는 것인데, 이는 Biome이 진단 카운트만을 출력하고 ESLint는 각 린트 오류에 대한 라인을 출력합니다.

## 지금 바로 Biome으로 변경해야 할까요?

Biome은 유망한 기능을 제공하지만, 현재는 초기 개발 단계에 있고 아직 몇 가지 제한 사항이 있습니다.

<div class="content-ad"></div>

바이옴에는 타입 검사된 린트 규칙이 적습니다.

놀라운 속도는 대가가 따릅니다: 러스트 기반 린터는 현재 ESLint에서 제공하는 타입 검사된 린트 규칙의 포괄성이 부족합니다. 이는 문법 오류와 일반적인 스타일 문제를 식별할 수 있지만, 타입 정보에 의존하는 문제를 놓칠 수 있다는 것을 의미합니다.

타입 검사에 관해서는 러스트 기반 린터가 여전히 퍼즐 한 조각을 놓친 상태입니다. 이는 ESLint + typescript-eslint가 보유한 기능입니다. 한편, 2024년 1월 현재, 바이옴은 64개의 typescript-eslint 규칙을 포함하고 있지만, 이러한 규칙이 모두 cover되지는 않습니다. 예를 들어, typescript-eslint와는 달리:

- 바이옴에는 prefer-readonly 규칙이 없어서, 생성자 외부에서 수정되지 않는 경우 비공개 멤버를 읽기 전용으로 표시하는 것을 강제하지 않습니다.
- 또한 explicit-function-return-type 규칙이 없어서, 함수가 항상 동일한 유형을 반환해도 모든 함수에 대한 명시적 반환 유형 선언을 요구하지 않습니다.

<div class="content-ad"></div>

그리고 Biome이 갖지 않는 규칙 예제가 몇 가지 더 있습니다. 그러나 Biome은 여전히 개발 중이며 더 많은 규칙이 추가되고 있습니다.

그렇다면 어떤 접근 방식을 선택해야 할까요? 성능이 중요하고 typescript-eslint 규칙에서 약간의 타형을 감수할 수 있다면, Biome이 좋은 선택일 수 있습니다. 프로젝트에서 완전한 유형 검사가 필요하다면, Prettier와 ESlint를 사용하고 Biome이 더 발전할 때까지 기다리세요.

Nx 지원

Biome에 대한 nx 지원에 대한 공식 발표는 없지만, 웹 프로젝트를 위한 새로운 도구 연쇄인 Biome에 대한 이러한 통합이 미래에 개발될 수도 있습니다...

<div class="content-ad"></div>

Prettier가 더 빨라지고 있어요

바이옴이 좋은 선택지인 것은 사실이지만, 미래에는 Prettier도 속도 향상을 할 가능성이 있어요. 이를 위해 파싱 엔진, AST 표현, 공백 처리를 최적화하고 하드웨어 가속을 활용할 거예요. 그래서, 만족스러운 Prettier를 사용 중이라면 계속 사용해도 괜찮을 거예요.

## 결론

바이옴이 계속 발전하면서, 웹 애플리케이션을 개발하는 방법을 최적화할 수 있다고 믿어요. 성능, 호환성, 사용 편의성에 초점을 맞춘 바이옴은 개발자에게 매력적인 선택지가 되고 있어요. 미래에는 바이옴이 웹 개발의 표준 도구 체인이 될 수 있다고 생각해요.

<div class="content-ad"></div>

프로젝트에서 Biome을 설정하고 싶다면, 몇 초만에 완료할 수 있어요. Biome 설치에 대한 자세한 내용은 biomejs.dev를 방문해 주세요. 코드 포매터를 알아보기 위한 플레이그라운드도 있답니다.
