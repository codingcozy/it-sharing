---
title: "가장 인기 있는 두 가지 JS 프레임워크 비교"
description: ""
coverImage: "/assets/img/2024-07-09-Comparisonof2MostPopularJSFrameworks_0.png"
date: 2024-07-09 13:22
ogImage:
  url: /assets/img/2024-07-09-Comparisonof2MostPopularJSFrameworks_0.png
tag: Tech
originalTitle: "Comparison of 2 Most Popular JS Frameworks"
link: "https://medium.com/@ianakozlova/comparison-of-2-most-popular-js-frameworks-6ba6ce566058"
---

## 두 가지 프레임워크의 유사점 및 차이점에 대해 알아봅시다. 초보자가 Vue와 React 중 어떤 것을 사용하는 것이 더 좋을까요?

지금까지 프론트엔드 세계에서 3대 주요 영역이 있습니다:

1. 구글이 주도하는 Angular;

2. 페이스북이 주도하는 React;

<div class="content-ad"></div>

3. 독립 개발자들이 추천하는 Vue.

대형 기업에서 심각하고 오랜 개발 시기에는 Angular가 가장 적합하다고 합니다. React는 더 다재다능하며, 소규모 및 대규모 프로젝트에 적합합니다. Vue는 스타트업 및 실험에 더 적합합니다.

초보자, 경험 부족한 개발자로써 전문적인 검토를 제공하는 것은 불가능하지만, 이 주제를 공부한 후 가장 주목한 주요 사항을 이 기사에서 공유하고자 합니다. 그렇다면 프론트엔드를 하고자 하는 초보자에게 가장 좋은 선택은 무엇일까요?

전문가들의 조언에서 이해한 바로는 Angular는 경험 있는 개발자를 위한 프레임워크입니다. JavaScript와 TypeScript에 대한 충분한 경험이 없으면 매우 어렵고, 학습 의욕을 잃을 수도 있다고 합니다. 따라서, 저는 이 프레임워크를 즉시 고려에서 제외합니다.

<div class="content-ad"></div>

REACT과 VUE를 유지해보세요. VUE에 대한 반대 의견은 매우 쉽게 접근할 수 있다는 것입니다. 그렇다면 왜 현재 React가 더 인기 있는 것일까요? 이 문제를 공부하는 초심자로서 내가 주목한 장단점을 이 기사에서 고려하고 싶습니다.

![이미지](/assets/img/2024-07-09-Comparisonof2MostPopularJSFrameworks_0.png)

차이점

React.js와 Vue.js 사이에는 각각이 어떤 용도에 더 적합한지를 결정하는 몇 가지 차이가 있습니다. Vue는 HTML 템플릿과 JSX를 사용하는 반면 React는 JSX만 사용합니다. JSX는 JS 코드에 직접 HTML을 삽입할 수 있게 해주는 확장 기능으로, 많은 복잡한 작업을 단순화할 수 있지만 동시에 간단한 작업을 복잡하게 만들 수도 있습니다.

<div class="content-ad"></div>

HTML 템플릿은 디자이너와 경험이 적은 개발자들이 현재 코드 기반을 이해하고 조정하기 쉬워지도록 도와줍니다. 또한 Vue에서 Pug(이전 이름은 Jade)와 같은 전처리기를 사용하여 템플릿을 작성하는 것도 가능합니다.

![Comparison of 2 Most Popular JS Frameworks](/assets/img/2024-07-09-Comparisonof2MostPopularJSFrameworks_1.png)

성능

일부 상황에서 Vue는 성능 면에서 명백하게 React를 앞서갑니다. 예를 들어 React 컴포넌트의 상태를 변경할 때, 해당 서브트리의 나머지 컴포넌트도 다시 렌더링됩니다. shouldComponentUpdate 또는 PureComponent 불변 데이터 구조를 사용하여 자식 컴포넌트의 다시 렌더링을 방지할 수 있지만, 이는 복잡성이 증가하고 DOM 상태 불일치로 이어질 수 있습니다. Vue에서는 의존성이 추적되며 다시 렌더링이 필요하지 않습니다.

<div class="content-ad"></div>

문서

React 문서는 Vue 수준만큼 완벽하지 않으므로 문제를 해결하기 위해 커뮤니티를 더 자주 찾아야 합니다. 그러나 React에는 방대한 학습 자료가 있는 활발한 커뮤니티가 있습니다.

보안

Vue와 React 둘 다 보안 특이성이 있지만, Vue 앱이 React 앱보다 보안을 강화하기가 어느 정도 쉽습니다. React 앱의 보안은 심층적인 지식과 경험이 필요합니다.

<div class="content-ad"></div>

Vue 경험

제가 공부하는 동안 React만 다뤄 봤기 때문에 Vue에 대해 알고 있는 프로그래머에게 문의하고 싶었습니다. 여기서 그가 강조한 주요 장점은 다음과 같습니다:

- 개발이 훨씬 빠릅니다;

- 필요한 경우 JSX를 포함할 수 있지만, .vue 파일의 표준 구조가 더 편리하고 논리적으로 보인다고 합니다;

<div class="content-ad"></div>

- 그는 실제로 Vue로 구축된 Quasar 프레임워크를 정말 좋아합니다. (리액트를 위한 유사한 것을 찾지 못했다고, 아마도 충분히 탐색하지 않았을지도 몰라요);

- 표준 프로젝트 구조 (그래도 프레임워크입니다)

또한 Redux, Vuex 및 Pinea와 같은 도구에 대해 논의했습니다. 리액트 커뮤니티는 애플리케이션을 위한 매우 혁신적인 상태 관리 솔루션인 Redux를 만들었습니다. Redux는 보통 JavaScript 프레임워크인 React, TypeScript, Vue, Angular 등과 함께 사용됩니다. Vue는 Vuex를 만들어 더 나은 삶을 위해 Vue와 긴밀하게 통합했습니다.

또한 Vue.js를 위한 가벼운 상태 관리 라이브러리인 Pinia도 있습니다. Vue 3의 새로운 반응성 시스템을 사용하여 직관적이고 완전히 유형화된 상태 관리 라이브러리를 구축했습니다. 사용자 피드백에 따르면 Vuex보다 빠르고 코드 양도 적으며 더 작은 라이브러리라고 합니다. 새로운 애플리케이션에는 Pinia를 사용하는 것이 좋습니다.

<div class="content-ad"></div>

크로스 플랫폼 애플리케이션

이 글에서는 Vue와 React를 기반으로 한 크로스 플랫폼 애플리케이션 개발에 대해 언급하고 싶습니다. 데스크톱 크로스 플랫폼 애플리케이션의 경우, Vue 및 React 프로젝트 모두 Electronjs(https://www.electronjs.org/) 또는 다른 유사한 도구를 사용할 수 있습니다.

PWA(Progressive Web App)도 React와 Vue를 기반으로 만들 수 있으며, 데스크톱 및 모바일 브라우저를 통해 설치할 수 있지만 Apple Store와 Google Play에는 출시할 수 없습니다.

Cordova 플랫폼을 사용하면 하이브리드 모바일 애플리케이션을 빠르게 개발할 수 있어 개발 시간을 단축할 수 있습니다. Cordova를 사용하면 React 또는 Vue로 만든 웹사이트를 애플리케이션으로 빠르게 감쌀 수 있으며, React Native를 사용하면 애플리케이션을 처음부터 작성해야 합니다.

<div class="content-ad"></div>

React Native은 React 컴포넌트 모델을 활용하여 iOS 및 Android용 네이티브 앱을 개발할 수 있는 기회를 제공합니다. 이것은 개발자들이 동일한 프레임워크의 지식을 여러 플랫폼에 적용할 수 있기 때문에 매우 유용합니다. Vue는 Weex 프로젝트를 공식적으로 지원하고 있습니다. 현재 개발 단계에 있어 React Native만큼 성숙하고 경험이 풍부하지는 않지만, 기업가와 프로그래머들은 종종 Cordova와 React Native 사이에 심각한 차이를 보지 못해 종종 Cordova를 선택하는 경향이 있습니다.

요약하면, Vue 개발은 초보자에게 더 빠르고 쉽습니다. React는 더 어렵게 느껴질 수 있지만, 한 번 마스터한 후에는 Vue나 Angular로 전환하기가 훨씬 쉬워집니다.

복잡한 기업용 애플리케이션이 필요하다면 React를 선택하는 것이 좋습니다. 소규모 애플리케이션, 스타트업 또는 아이디어를 빠르게 테스트하려는 경우 Vue를 활용하는 것이 더 좋습니다.
