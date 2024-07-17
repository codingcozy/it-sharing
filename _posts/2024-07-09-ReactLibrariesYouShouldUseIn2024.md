---
title: " 2024년에 꼭 사용해야 할 React 라이브러리 목록"
description: ""
coverImage: "/assets/img/2024-07-09-ReactLibrariesYouShouldUseIn2024_0.png"
date: 2024-07-09 13:38
ogImage:
  url: /assets/img/2024-07-09-ReactLibrariesYouShouldUseIn2024_0.png
tag: Tech
originalTitle: "📚 React Libraries You Should Use In 2024"
link: "https://medium.com/@reedbarger/react-libraries-you-should-use-in-2024-4a9fd422a1bd"
---

![React Libraries You Should Use in 2024](/assets/img/2024-07-09-ReactLibrariesYouShouldUseIn2024_0.png)

To build applications with React, you need to be familiar with the right libraries to add the features you require.

For instance, you will need a good third-party library to handle features like authentication or styling.

In this detailed guide, I will introduce all the libraries that I recommend for building fast and reliable React apps with ease in 2024.

<div class="content-ad"></div>

# 🛠️ React Framework

우선, 2024년에 React 앱을 어떻게 만들 수 있는지부터 알아보겠어요.

클라이언트에서 렌더링되는 React 프로젝트를 원한다면, 폐기된 Create React App 도구를 대체한 Vite가 최선의 선택입니다.

서버에서 렌더링하거나 풀스택 React 프로젝트를 만들고 싶다면, Next.js가 가장 완성도 높고 인기 있는 풀스택 React 프레임워크입니다. Next.js 버전 13에서는 앱 라우터를 도입하여 서버 컴포넌트 및 서버 액션과 같은 기능을 제공했습니다. 이를 통해 데이터를 가져오고 React 컴포넌트를 서버에서 렌더링할 수 있습니다.

<div class="content-ad"></div>

만약 Next.js의 몇 가지 기능들이 이해하기 조금 어렵다면, 동적 및 정적 사이트를 만들기 위한 훌륭한 풀 스택 대안은 Remix입니다.

빠르게 로드되고 주로 정적 콘텐츠를 제공하고 싶은 애플리케이션을 만들고 싶다면, 또 다른 좋은 선택지는 Astro입니다. Astro를 사용하면 React를 다른 프레임워크인 Vue 및 Svelte와 함께 사용할 수 있으며(즉, "프레임워크 중립적"입니다), 클라이언트에 필요한 가장 적은 양의 JavaScript만 전송되도록 설계되어 매우 빠른 로드 시간을 제공합니다.

# 📦 패키지 매니저

이 가이드에 나열된 모든 라이브러리를 설치하려면, 패키지 매니저라는 것이 필요합니다.

<div class="content-ad"></div>

Node.js가 설치되어 있다면, 컴퓨터에서 React 프로젝트를 로컬로 실행하는 데 필요한 것입니다. 2024년에도 여전히 좋은 선택인 NPM을 간단히 사용할 수 있습니다. NPM 이외에도 다른 대안으로 Yarn과 PNPM이 있었습니다.

자바스크립트 세계에서 매우 인기 있는 새로운 대안은 Bun입니다. Bun은 Node와 마찬가지로 JavaScript 런타임이자 패키지 관리자로, Node와 NPM에 대한 빠른 대안으로 마케팅되고 있습니다.

# 🎨 CSS & UI 라이브러리

프로젝트를 설정하고 라이브러리를 설치했다면, React 프로젝트를 어떻게 스타일링할지 고민해보세요.

<div class="content-ad"></div>

위에서 언급한 모든 프레임워크는 기본적인 CSS 지원이 포함되어 있어요. 2024년에 React 프로젝트를 평범한 CSS로 스타일링하고 싶다면 괜찮아요.

CSS 스타일 시트나 CSS 모듈을 사용할 수 있지만, 순수한 스타일링 측면에서 현재 가장 인기 있는 선택은 Tailwind CSS를 사용하는 것입니다. Tailwind CSS를 사용하면 몇 가지 설정 단계가 필요하지만, 사전 제작된 클래스를 연결하여 컴포넌트 파일 내에서 빠르게 스타일을 추가할 수 있습니다.

ShadCN은 Tailwind CSS를 Radix UI라는 컴포넌트 라이브러리와 결합한 2024년에 매우 인기 있는 UI 라이브러리입니다. Radix와 같은 컴포넌트 라이브러리는 직접 코딩하지 않고도 쉽게 컴포넌트를 추가할 수 있도록 해줍니다. Tailwind CSS의 도움으로 구성 요소가 어떻게 보여야 하는지에 대한 더 큰 제어를 제공하는 ShadCN이 인기를 얻었습니다.

다른 매우 인기 있는 기능 컴포넌트 라이브러리도 많이 있어요. 여전히 인기 있는 Material UI뿐만 아니라 Mantine, Chakra UI 등 다른 라이브러리도 있어요. 어떤 것을 선택할지는 최종 앱이 어떻게 보이길 원하는지에 달려 있어요. 여러 UI 라이브러리를 시도해보고 어떤 것이 가장 좋아하는지 확인해보는 것을 추천합니다.

<div class="content-ad"></div>

# 💽 상태 관리

React에는 기본 애플리케이션에서 앱 상태를 관리할 수 있는 useState와 useReducer와 같은 내장 도구가 있습니다. Next.js를 사용하는 경우, 서버 컴포넌트와 작업할 때 상태 관리가 URL로 이동했습니다. 이러한 옵션들을 통해 더 정확한 방법으로 상태를 관리할 필요가 있을 수 있습니다.

여러 조각의 상태가 있고 상태 업데이트가 컴포넌트 렌더링에 어떻게 반영되는지에 대한 더 큰 제어를 원할 수 있습니다. 그렇다면 전용 상태 관리 라이브러리를 활용할 수 있습니다.

Zustand, Recoil, Jotai와 같은 라이브러리들은 모두 매우 유사하며, 객체에 속성과 메서드를 선언함으로써 상태를 간단히 관리할 수 있게 합니다. 결국 이것이 앱 컴포넌트 전체에서 상태 관리를 처리하는 가장 간단한 방법입니다.

<div class="content-ad"></div>

만약 2024년에 내 모든 React 프로젝트를 위한 상태 관리 라이브러리를 하나 선택해야 한다면, Zustand를 선택할 것입니다. Zustand를 사용하는 방법을 배우는 데 거의 시간이 걸리지 않습니다. 또한 응용 프로그램에 공급자 구성 요소를 추가할 필요가 없어서 원하는 모든 구성 요소에서 매우 편리하게 사용할 수 있습니다.

# 🐕 데이터 가져오기

상태 관리와 데이터 가져오기는 종종 함께 진행됩니다. 클라이언트 렌더링된 React 앱을 개발하고 있다면, 데이터 가져오기 라이브러리가 필요할 것입니다.

2024년에 React 앱에서 서버로부터 데이터를 가져오는 가장 좋은 방법은 여전히 React Query 또는 이제는 Tanstack Query로 불리는 것입니다. TanStack Query는 데이터를 가져오는 것뿐만 아니라 데이터를 언제 가져오고 다시 가져오며 캐싱하는 것까지 세밀하게 제어할 수 있도록 편리한 사용자 지정 후크를 통해, 데이터를 쉽게 변경하거나 변이시키는 기능을 제공합니다.

<div class="content-ad"></div>

또 다른 견고한 대안은 SWR입니다. SWR은 질의와 뮤테이션을 처리하는 사용자 정의 후크를 제공하지만 제공하는 기능 면에서는 훨씬 간단합니다. 두 가지 중 하나를 선택하여 Fetch API를 사용하여 데이터를 가져오고 HTTP 요청을 수행할 때 문제가 발생하지 않습니다.

# 🧭 라우팅

Next.js, Remix 또는 Astro와 같은 프레임워크를 사용하는 경우, 이미 라우팅이 처리되어 있습니다. 이들은 모두 프로젝트 내에서 라우트 또는 페이지를 만드는 내장 방식을 제공합니다.

Vite 또는 Create React App과 같은 클라이언트 렌더링 React 앱을 사용하는 경우 라우팅 라이브러리를 선택해야 합니다. React Router는 여전히 가장 인기 있는 선택지입니다. React Router는 모든 라우팅 요구 사항을 처리합니다. loader prop을 통해 데이터로드 기능이 매우 발전적입니다. loader prop을 사용하면 외부 라이브러리인 React Query를 사용하지 않고 주어진 경로에 대한 데이터를 가져올 수 있습니다.

<div class="content-ad"></div>

이제 인기가 높아지고 있는 라이브러리 중 하나는 Tanstack Router인 것 같아요. Tanstack Router은 완전히 타입 안전하게 설계되었고 React Router 버전 6과 같이 데이터 가져오기에 좋은 기본값을 제공한다고 해요.

Tanstack Router가 조금 더 최근에 나왔지만, 두 선택 중 어느 것을 선택해도 잘못될 일은 없어요. 이미 Tanstack Query나 SWR을 사용하고 있다면, 이것은 완벽한 조합이 될 거예요.

# 🔒 인증

인증은 프로젝트의 서버 쪽에서 처리되는 것이지만, React 프로젝트와 가장 잘 통합되는 라이브러리가 무엇인지 알고 있는 것이 좋아요. 클라이언트 및 서버 측 모두에 대한 정보가 필요하죠.

<div class="content-ad"></div>

2024년, Supabase가 매우 강력한 인증 솔루션으로 등장했고 React 앱과 쉽게 통합됩니다. 서버 측인 Next.js 프로젝트와 클라이언트 측에서 모두 쉽게 사용할 수 있습니다. 지난 몇 년 동안 Firebase는 비슷한 이유로 선택되었지만 서버 측 통합이 어려웠습니다.

Next.js를 사용하는 경우 NextAuth, Clerk, 그리고 새로 온 Lucia와 같은 다양한 옵션이 있습니다. 현재는 Next.js에 내장된 인증 라이브러리가 없는 것이 안타깝지만, 미래에는 그런 라이브러리가 생길 수도 있습니다.

그동안 저는 개인적으로 Supabase를 사용할 예정이며 권유드리고 싶습니다. 꼭 그들의 문서를 확인해보시기 바랍니다.

# 🥞 데이터베이스 & ORM

<div class="content-ad"></div>

인증과 마찬가지로 데이터베이스는 주로 서버 측 코드와 밀접하게 관련된 부분입니다.

Supabase나 Firebase와 같은 데이터베이스는 서버가 필요하지 않습니다. 클라이언트에서 직접 데이터를 가져오고 변경할 수 있으며, 대시보드에서 보안 규칙을 추가하여 사용자의 인증 상태와 역할에 따라 특정 유형의 요청을 허용하거나 거부할 수 있습니다.

이 두 가지 옵션 이외에도, 전통적인 서버나 풀스택 프레임워크를 사용하는 경우 무수히 많은 옵션이 있습니다. 2024년에는 MySQL이나 PostgreSQL과 같은 SQL 데이터베이스가 MongoDB나 Firebase와 같은 NoSQL 데이터베이스보다 뚜렷하게 선호되는 경향입니다.

데이터베이스와 대화하기 위해 직접 SQL을 사용하거나 사용자 정의 쿼리 언어를 사용할 수 있는 ORM을 사용할 것입니다. 인기 있는 ORM 옵션에는 Prisma나 Drizzle가 포함되며, 둘 다 데이터가 반환될 내용을 알 수 있는 타입 안전한 코드를 생성할 수 있으며, 선택한 SQL이나 NoSQL 데이터베이스와 잘 통합됩니다.

<div class="content-ad"></div>

# 🕰️ 날짜 및 시간

자바스크립트를 다룰 때는 항상 날짜 라이브러리를 사용하는 것이 좋습니다. 자바스크립트의 Date 생성자는 예측할 수 없으며 시간대와 같은 것들과 신뢰성 있게 작업하기 거의 불가능합니다.

다양한 옵션이 있지만, date-fns 나 day.js를 선호하는 경향이 있습니다. 둘 다 매우 작지만 확장성 있는 라이브러리로, 브라우저나 백엔드에서 자바스크립트 날짜를 조작할 수 있습니다. 둘 중 어느 것을 선택해도 잘못된 선택은 없습니다.

# 📋 양식

<div class="content-ad"></div>

대부분의 경우 간단한 가입 양식을 사용하여 응용 프로그램을 만드는 경우에는 양식 라이브러리가 필요하지 않을 수 있습니다. 입력 요소에 required 속성을 사용하는 것이 필요한 경우가 많습니다.

더 복잡한 것을 구축하고 많은 양식을 가지고 있다면, React Hook Form은 양식 입력 유효성을 검사하고 매우 적은 코드로 오류를 표시할 수 있는 완전한 기능을 갖춘 양식 라이브러리입니다.

Formik 및 React Final Form과 같은 다른 양식 라이브러리는 동일한 기능을 제공하지만 React Hook Form은 보통 더 적은 코드를 필요로 하는 더 현대적인 API를 가지고 있기 때문에 조금 더 나은 선택입니다.

# ☔️ 드래그 & 드롭

<div class="content-ad"></div>

애플리케이션에 드래그 앤 드롭을 추가할 때 거의 항상 서드파티 라이브러리가 필요합니다. 과거에는 React Beautiful DnD가 가장 인기 있는 선택이었습니다. 그러나 2024년 기준으로 정기 업데이트를 더 이상 받지 않고 있습니다.

앞으로 가장 좋은 대안은 DnD Kit을 사용하는 것입니다. 이 라이브러리는 가벼우며 매우 유연하며, 문서에는 드래그 앤 드롭을 구현할 때 필요한 모든 사용 사례를 다루는 매우 유용한 예제가 포함되어 있습니다.

# 📱 모바일 앱

모바일 앱을 개발하려면 React 개발자들을 위한 라이브러리로 React Native가 항상 있었습니다. React Native의 기능을 웹으로 확장하는 흥미로운 라이브러리들이 있습니다.

<div class="content-ad"></div>

엑스포(Expo)는 모바일 리액트 앱을 만들기 위한 Vite와 유사한 도구입니다. 빠른 새로 고침(fast refresh)과 엑스포 고(Expo Go)를 통해 프로젝트를 개발 중에 손쉽게 자신의 기기에서 실행할 수 있는 훌륭한 기능을 갖추고 있습니다. 엑스포는 모바일 코드 기반을 가져와 웹으로 배포하기 쉽도록 도와줍니다.

이와 같은 목표를 달성하는 다른 프로젝트로는 Tamagui가 있는데, 안드로이드, iOS, 웹에서 실행되는 실제 네이티브 앱을 만들고 싶다면 꼭 확인해보세요.

브라우저에서 실행되도록 작성한 리액트 앱이 있다면, 네이티브 앱으로 빠르게 전환하고 애플 앱 스토어 또는 구글 플레이 스토어에 배포하는 가장 빠른 방법은 Capacitor.js를 사용하는 것입니다.

# 🚀 배포

<div class="content-ad"></div>

React 앱을 배포하는 방법이 더 다양해졌어요. Vercel은 클라이언트 렌더링이든 서버 렌더링이든 React 앱을 배포하기 가장 쉬운 플랫폼 중 하나라고 할 수 있어요. 거의 모든 프레임워크를 지원하며 자바스크립트 이외의 언어도 지원해요. Vercel은 넉넉한 프리미엄 요금제를 제공하고 있어요. Netlify와 Cloudflare Pages는 같은 공간에서 Vercel과 경쟁하고 있어요.

Cloudflare Pages는 풀스택 React 프로젝트가 있는 경우 설정하기 약간 까다로울 수 있지만, 다른 옵션들과 비교했을 때 가장 저렴한 가격대를 제공하고 있어요. 서버에 요금을 내고 싶다면, Railway나 Render와 같은 것을 사용할 수도 있어요. 서로 다른 서버를 사용 중인 경우 이런 옵션들을 활용해 React 앱을 배포할 수 있어요.

# 🏆 가장 중요한 라이브러리

마지막으로, 2024년에 React 개발자라면 꼭 알아야 할 가장 중요한 라이브러리는 TypeScript입니다. 언급한 모든 프레임워크들은 TypeScript를 사용해 React 앱을 빌드할 수 있는 옵션을 제공하고 있어요.

<div class="content-ad"></div>

TypeScript은 JavaScript 코드 내의 유형 오류를 감지하여 런타임 오류를 방지하는 데 도움이 되는 도구입니다. 모든 React 프로젝트를 JavaScript만으로 구축할 수 있지만, 언젠가는 TypeScript를 사용하는 이점을 직접 경험하거나 TypeScript가 포함된 코드베이스를 살펴볼 것입니다.

나는 TypeScript를 배우는 데 시간을 할애하는 것을 강력히 추천합니다. React 개발자로서 알아야 할 가장 필수적이고 가장 수요가 높은 도구이며 전체적으로 코드 품질을 크게 향상시킬 수 있습니다.

# 🏆 프로페셔널 React 개발자가 되어 보세요

React를 처음부터 끝까지 배울 수 있는 궁극의 자원을 찾고 계신가요?

<div class="content-ad"></div>

✨ 소개합니다: 리액트 부트캠프

부트캠프에는 리액트를 성공적으로 습득하는 데 도움이 되는 모든 자원이 포함되어 있어요:

- 🎬 200개의 심층 비디오
- 🕹️ 100가지의 실전 리액트 도전과제
- 🛠️ 5가지 인상적인 포트폴리오 프로젝트
- 📄 10가지 필수 리액트 치트시트
- 🥾 완전한 Next.js 부트캠프
- 🖼️ 완전한 시리즈의 애니메이션 비디오

리액트 부트캠프를 직접 체험해보려면 아래를 클릭해주세요.

<div class="content-ad"></div>

![React Libraries You Should Use In 2024](/assets/img/2024-07-09-ReactLibrariesYouShouldUseIn2024_1.png)

시작하려면 클릭하세요!
