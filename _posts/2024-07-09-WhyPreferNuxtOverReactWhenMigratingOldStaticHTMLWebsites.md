---
title: "기존의 정적 HTML 웹사이트를 리액트가 아닌 Nuxt로 마이그레이션 해야 하는 이유"
description: ""
coverImage: "/assets/img/2024-07-09-WhyPreferNuxtOverReactWhenMigratingOldStaticHTMLWebsites_0.png"
date: 2024-07-09 13:30
ogImage:
  url: /assets/img/2024-07-09-WhyPreferNuxtOverReactWhenMigratingOldStaticHTMLWebsites_0.png
tag: Tech
originalTitle: "Why Prefer Nuxt Over React When Migrating Old Static HTML Websites"
link: "https://medium.com/@arhamkhnz/why-prefer-nuxt-over-react-when-migrating-old-static-html-websites-01651f5ecc7b"
---

![이미지](/assets/img/2024-07-09-WhyPreferNuxtOverReactWhenMigratingOldStaticHTMLWebsites_0.png)

오래된 정적 웹사이트를 이주하는 것, 특히 jQuery에 의존하는 경우, 복잡하고 어려운 작업일 수 있습니다. 최근에, 25개 이상의 jQuery 스크립트에 의존하는 클라이언트 웹사이트를 이주하는 프로젝트를 진행했었습니다. 처음에는 React를 선택했지만, jQuery 호환성에 심각한 문제가 발생하여 결함이 발생하고 전반적으로 만족스럽지 못한 이주 프로세스를 겪었습니다. 이 경험을 통해 대안으로 Nuxt를 탐구하게 되었고, 결과가 놀라울 정도로 좋아졌습니다.

## React와 jQuery의 문제점

- DOM 조작 문제: React의 가상 DOM이 항상 jQuery의 직접적인 DOM 조작과 호환되지 않을 수 있어 일관성과 성능 문제를 야기할 수 있습니다.
- 복잡성 증가: React 프로젝트 내에서 jQuery를 사용하는 것은 종종 추가 구성과 해결책이 필요해 코드 기반에 불필요한 복잡성을 추가할 수 있습니다.
- 이벤트 처리 충돌: React와 jQuery 모두 이벤트 처리를 관리하므로 함께 사용할 때 충돌 및 예기치 않은 동작이 발생할 수 있습니다.
- 시간 소비적인 이주: jQuery 스크립트를 React로 이주하는 것은 시간이 많이 소요되며 호환성과 기능성을 보장하기 위해 상당한 재작성과 조정이 필요할 수 있습니다.

<div class="content-ad"></div>

이와 많은 다른 문제들이 함께하여 디버깅에 많은 시간을 소비하고 해결책을 찾는 데 많은 노력이 필요했기 때문에, 전체 이주의 목표를 방해했었습니다.

## 왜 Nuxt를 사용해야 할까요?

Nuxt로 전환한 후, 여러 이유로 이주 과정이 더 원활해졌습니다:

Nuxt에 의해 해결된 가장 큰 문제 중 하나는 스크립트 및 스타일시트의 효율적인 로딩이었습니다. nuxt.config.ts 파일에서 HTML 문서의 여러 부분, 예를 들어 head, body 시작부 또는 body 끝에 스크립트를 로드할 수 있습니다. 이 유연성은 jQuery 및 기타 종속성을 쉽게 관리하며, 중요한 시점에 로드하여 충돌과 성능 문제를 피할 수 있도록 보장합니다.

<div class="content-ad"></div>

예시 구성 파일인 nuxt.config.ts

```js
export default defineNuxtConfig({
  compatibilityDate: "2024-04-03",
  devtools: { enabled: true },

  app: {
    head: {
      link: [
        { rel: "stylesheet", href: "assets/css/bootstrap.css" },
        { rel: "stylesheet", href: "assets/css/animate.css" },
      ],
      script: [
        { src: "assets/js/vendor/jquery.js", tagPosition: "bodyClose" },
        { src: "assets/js/bootstrap-bundle.js", tagPosition: "bodyClose" },
      ],
    },
  },
});
```

가상 DOM이 없음: React와 달리 Nuxt( Vue 기반)는 가상 DOM을 사용하지 않아 jQuery의 직접적인 DOM 조작과의 충돌을 크게 줄입니다. DOM과 직접 상호작용하므로 jQuery가 간섭 없이 작동하여 통합을 더 부드럽고 신뢰할 수 있게 만듭니다.

## Nuxt로 시작하기

<div class="content-ad"></div>

Nuxt에 대해 더 자세한 문서 및 시작 방법에 대한 지침을 보려면 공식 Nuxt 문서를 확인해보세요.

## 결론

Nuxt는 오래된 정적 웹사이트를 더 동적인 프레임워크로 이전하기에 최적인 선택입니다. 효율적인 스크립트 및 스타일시트 관리와 가상 DOM이 없다는 장점으로 부드러운 전환을 제공하고 기존 jQuery 종속성과의 호환성을 보장합니다. 이로써 Nuxt는 기존 레거시 웹 애플리케이션을 현대화하는 강력하고 신뢰할 수 있는 솔루션으로 자리매김하고 있습니다.
