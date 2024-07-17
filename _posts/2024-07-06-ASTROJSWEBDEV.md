---
title: "2024년 웹 개발을 위한 ASTRO JS의 모든 것"
description: ""
coverImage: "/it-shar.ing/assets/no-image.jpg"
date: 2024-07-06 01:52
ogImage:
  url: /it-shar.ing/assets/no-image.jpg
tag: Tech
originalTitle: "ASTRO JS | WEB DEV"
link: "https://dev.to/shubhamtiwari909/astro-js-web-dev-15fp"
---

안녕하세요, 웹 개발자 여러분. 오늘은 NEXT JS와 같은 새로운 프레임워크인 Astro JS에 대한 소규모 시리즈를 시작하려고 합니다. NEXT JS에 대해 이미 알고 계시다면, 학습 경험이 매우 쉽고 간단할 것입니다.

자, 시작해봅시다...

## 목차

- Astro JS란 무엇인가
- 주요 기능들 중 일부
- 프런트 매터 아키텍처
- 아일랜드 아키텍처
- 통합
- 뷰 전환
- 자산 - CSS 및 이미지
- 장점
- 단점

<div class="content-ad"></div>

## Astro JS란 무엇인가요?

Astro.js는 정적 웹사이트와 동적 웹 애플리케이션을 구축하는 성능과 개발자 경험을 최적화하기 위해 설계된 현대적인 웹 프레임워크입니다. Astro.js는 부분 수화(partial hydration)라는 독특한 방식을 사용하여 인터랙티브 컴포넌트에 필요한 JavaScript만 클라이언트 측에 로드되어 로딩 시간과 전체 성능을 크게 향상시킵니다. 또한 SSG/SSR에서 비롯된 파일 기반 라우팅, Vue.js의 템플릿/레이아웃 시스템과 같은 기능을 제공합니다.

## 일부 주요 기능

- 정적 사이트 생성(Static Site Generation, SSG): Astro는 기본적으로 페이지를 정적 HTML로 렌더링하여 빠른 로딩 시간을 제공하고 SEO를 향상시킵니다.
- 컴포넌트 중립적(Component Agnostic): 개발자들은 React, Vue, Svelte 등 인기 있는 프레임워크에서 컴포넌트를 동일한 프로젝트 내에서 사용할 수 있습니다.
- 내장 최적화: Astro는 CSS, JavaScript 및 이미지를 자동으로 최적화하여 효율적이고 가벼운 웹사이트를 제공합니다.
- 유연성: 단일 페이지 응용 프로그램(SPA) 및 다중 페이지 응용 프로그램(MPA)을 모두 지원하여 다양한 프로젝트 요구 사항을 충족시킵니다.
- 파일 기반 라우팅: next.js와 같은 직관적인 파일 및 폴더 구조로 라우팅 프로세스를 간단하게 만들어줍니다.
- 다양한 파일 형식 지원: .astro, .md, .mdx, .js, .ts, .html과 같은 다양한 파일 형식을 지원합니다.

<div class="content-ad"></div>

## 프론트 매터 아키텍처

Astro에서 프론트매터는 .astro 파일 상단에 메타데이터와 로직을 포함하는 방법입니다. 세 개의 대시 (---)를 사용하여 정의되며 HTML 렌더링 전에 실행되는 JavaScript(또는 TypeScript) 코드를 작성할 수 있습니다. 이는 변수 설정, 컴포넌트 가져오기, 데이터 가져오기 및 기타 사전 작업에 일반적으로 사용됩니다.

```js
---
import Header from '../components/Header.astro';
import Footer from '../components/Footer.astro';
const title = "내 블로그 포스트";
const author = "Jane Doe";
---
<Header />
 <article>
  <h1>{title}</h1>
  <p>{author}가 작성함</p>
 </article>
<Footer />
```

```js
---
// markdoc 파일을 사용한 프론트 매터
layout: ../../layouts/BlogPostLayout.astro
title: 간단한 Astro
author: Himanshu
description: Astro가 멋지게 만드는 것을 확인하세요!
---
이것은 Markdown으로 작성된 포스트입니다.
```

<div class="content-ad"></div>

```js
---
// markdoc 파일 변수에 액세스하여 동적 경로로 더 나아지게 할 수 있습니다.
const {frontmatter} = Astro.props; // 리액트 프롭스와 동일한 개념
---
<html>
  <!-- ... -->
  <h1>{frontmatter.title}</h1>
  <h2>글쓴이: {frontmatter.author}</h2>
  <p>{frontmatter.description}</p>
  <slot /> <!-- Markdown 콘텐츠가 여기에 삽입됩니다 -->
</html>
```

## 아일랜드 아키텍처

- 이는 페이지의 상호작용 컴포넌트에 필요한 최소한의 자바스크립트만 전달함으로써 웹 성능을 최적화하는 것을 목표로 합니다. 이 아키텍처는 대부분의 페이지에 대해 정적 HTML을 렌더링하면서 클라이언트 사이드 자바스크립트가 필요한 상호작용 컴포넌트를 격리하는 데 초점을 맞춥니다. 이 모든 것은 react나 vue와 같은 즐겨 사용하는 라이브러리와 함께 작동합니다.
- 부분 수화: 기존의 Single Page Applications에서처럼 전체 페이지를 자바스크립트로 수화하는 대신, Astro는 일부 컴포넌트만 수화합니다. 이는 필요한 특정 컴포넌트에 대해서만 자바스크립트가 실행되며, 클라이언트 측에 로드되고 실행되는 자바스크립트의 양을 크게 줄입니다.
- 더 나은 SEO: 정적 HTML 콘텐츠는 즉시 검색 엔진에서 이용 가능해져서 크롤링 및 인덱싱이 개선됩니다.
- 아일랜드 수화를 위한 지시문:

- client:load: 페이지 로드 후 즉시 컴포넌트를 수화합니다.
- client:idle: 브라우저가 유휴 상태일 때 컴포넌트를 수화합니다.
- client:visible: 뷰포트에 표시될 때 컴포넌트를 수화합니다.
- client:media: 미디어 쿼리에 따라 컴포넌트를 수화합니다.
- client:only: 클라이언트 측에서만 컴포넌트를 수화합니다 (클라이언트 전용 컴포넌트에 유용함).

<div class="content-ad"></div>

- 예시

```js
import Header from '../components/Header.astro';
import Footer from '../components/Footer.astro';
const title = "내 블로그 포스트";
const author = "제인 도";
---
<Header />
 <article>
  <h1>{title}</h1>
  <p>{author}가 작성함</p>
 </article>
 <InteractiveComponent slug={post.slug} client:load />
<Footer />
```

## 통합

UI 통합 - React JS, Vue JS, Svelte JS와 SSR 어댑터 - Vercel, netlify, cloudfare, node js와 같이 여러 통합을 사용할 수 있습니다. 또한 tailwind, markdoc, mdx, sitemap 등 다른 통합도 지원됩니다.

<div class="content-ad"></div>

### 통합 기능 추가하는 방법

- React와 Tailwind 통합 추가하기

```js
npx astro add react tailwind
```

- 이 명령은 React와 Tailwind의 구성을 완전히 설정합니다. 만약 이들에 대해 수동으로 구성해야 한다면 astro.config.mjs 파일에서 할 수 있습니다.

<div class="content-ad"></div>

```js
import { defineConfig } from "astro/config";
import tailwind from "@astrojs/tailwind";
export default defineConfig({
  integrations: [
    tailwind({
      // 모든 페이지에 기본 'base.css'를 주입하는 것을 비활성화합니다.
      // 사용자 정의 'base.css'를 정의하거나 가져와야 하는 경우 유용합니다.
      applyBaseStyles: false,
      // 중첩된 CSS 선언을 작성할 수 있게 합니다.
      // Tailwind의 구문과 함께 사용할 수 있습니다.
      nesting: true,
    }),
  ],
});
```

## 화면 전환

- 화면 전환은 웹 애플리케이션의 서로 다른 뷰나 페이지 사이에서 부드러운 전환을 만들기 위해 사용되는 기술과 도구를 의미합니다. 이러한 전환은 사용자 경험을 향상시켜 시각적 연속성을 제공하고 사이트를 탐색하는 동안 느껴지는 로드 시간을 줄입니다. Astro 자체는 내장된 뷰 전환 기능을 제공하지는 않지만, 기존의 JavaScript 라이브러리와 프레임워크를 활용하여 이러한 효과를 구현할 수 있습니다.
- 페이지 전환에는 3가지 효과가 있습니다 - slide, fade, none.

```js
---
import { ViewTransitions } from 'astro:transitions';
---
<html lang="en">
  <head>
    <title>내 홈페이지</title>
    <ViewTransitions />
  </head>
  <body>
    <h1>내 웹사이트에 오신 것을 환영합니다!</h1>
  </body>
</html>
```

<div class="content-ad"></div>

## 자산

### CSS

우리는 2가지 모드로 스타일 태그를 통해 CSS를 추가할 수 있습니다. 하나는 scoped 모드(해당 컴포넌트에만 적용되는 CSS)이고, 다른 하나는 global 모드(전체 페이지에 적용되는 CSS)입니다. 또한 " :global "을 사용하여 일부 스타일을 scoped로 지정하고 일부를 global로 지정할 수도 있습니다.

```js
// Scoped
<style>
  .text {
    color: blue;
  }
</style>
```

<div class="content-ad"></div>

```js
// 전역
<style is:global>
  h1 { color: red; }
</style>
```

```js
// 혼합된 스타일
<style>
  /* 이 컴포넌트 내에서만 적용됩니다. */
  h1 { color: red; }
  /* 혼합: 자식 `h1` 요소에만 적용됩니다. */
  article :global(h1) {
    color: blue;
  }
</style>
```

- 클래스를 class:list와 조합하기 - class:list를 사용하여 조건에 따라 클래스를 결합할 수 있습니다.

```js
---
const { isRed } = Astro.props;
---
<!-- `isRed`가 참이면 클래스는 "box red"가 됩니다. -->
<!-- `isRed`가 거짓이면 클래스는 "box"가 됩니다. -->
<div class:list={['box', { red: isRed }]}><slot /></div>
<style>
  .box { border: 1px solid blue; }
  .red { border-color: red; }
</style>
```

<div class="content-ad"></div>

- CSS 변수 정의 - 스타일 태그 내에서 변수를 정의하여 해당 스타일 태그 내의 모든 요소에서 접근할 수 있습니다.

```js
---
const foregroundColor = "rgb(221 243 228)";
const backgroundColor = "rgb(24 121 78)";
---
<style define:vars={ foregroundColor, backgroundColor }>
  h1 {
    background-color: var(--backgroundColor);
    color: var(--foregroundColor);
  }
</style>
```

### 이미지

- Astro는 내장된 컴포넌트를 제공하여 웹 어플리케이션의 이미지 로딩과 성능을 최적화하는 데 사용할 수 있습니다. 이 컴포넌트는 반응형 이미지, 지연 로딩, 자동 형식 선택과 관련된 일반적인 작업을 처리하는 데 도움을 줍니다.
- 컴포넌트의 주요 기능

<div class="content-ad"></div>

- 자동 최적화
- 반응 형 이미지
- Lazy Loading
- 자동 형식 선택 (.webp)
- 내장된 SrcSet 생성
- 저품질 이미지 자리 표시자

## 장점

- 빠른 로드 시간: 빠른 로드 시간을 위해 SSG 사용
- 부분 적용: 대화식 구성 요소에 필요한 JavaScript만 로드
- 서버 측 렌더링 (SSR): 사전 렌더링된 페이지가 SEO를 향상시킴
- 컴포넌트에 중립적: React, Vue, Svelte 등 여러 컴포넌트를 동일한 프로젝트 내에서 지원
- 학습 용이성: 직관적인 구문과 잘 작성된 문서
- 파일 기반 라우팅: 간단하고 직관적인 라우팅 시스템
- 자동 이미지 최적화: 성능과 사용자 경험을 향상시킴
- CSS 및 JavaScript 최적화: 자동으로 자산을 최적화하고 최소화
- 다양한 플러그인 생태계: 기능을 확장할 수 있는 다양한 플러그인 제공
- 다중 페이지 애플리케이션: SPA 및 MPA 모두 지원
- 중소형 프로젝트에 적합한 확장 가능성이 좋음

## 단점

<div class="content-ad"></div>

- 빌드 성능: 수천 개의 페이지를 가진 대규모 사이트에 대해 긴 빌드 시간이 소요될 수 있습니다.
- 부분 수분 개념: 일부 개발자들에게 새로운 개념을 익혀야 합니다.
- 제한된 자원: 더 확립된 프레임워크에 비해 적은 튜토리얼 및 제3자 도구가 있습니다.
- 제한된 플러그인: 생태계가 덜 다양하며 성숙한 프레임워크와 비교할 때 다양하지 않습니다.
- 신생 프레임워크: 성숙도나 확장된 생태계 면에서 기존 프레임워크만큼 발달되지 않았을 수 있습니다.
- 제한된 자원: 더 적은 튜토리얼 및 커뮤니티 기여 자료가 있을 수 있습니다.
- 복잡한 통합: 특정 제3자 서비스나 복잡한 설정과의 통합이 도전적일 수 있습니다.

여기까지가 지금까지의 내용입니다. 다음에는 코드베이스 쪽에서 더 다룰 예정입니다.

아래 연락처로 연락하실 수 있습니다.

- Instagram: [supremacism\_\_shubh](https://www.instagram.com/supremacism__shubh/)
- LinkedIn: [shubham-tiwari-b7544b193](https://www.linkedin.com/in/shubham-tiwari-b7544b193/)
- 이메일: shubhmtiwri00@gmail.com

<div class="content-ad"></div>

아래 링크에서 기부를 도와주실 수 있어요. 감사합니다👇👇
https://www.buymeacoffee.com/waaduheck

이 게시물들도 확인해보세요 😊
