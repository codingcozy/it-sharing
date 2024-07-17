---
title: "각 Angular 애호가들이 놓치지 말아야 할 6가지 업데이트 NgRx 18, 신규 RFC DomRef API, Signals로 웹 스토리지 구현 방법 등"
description: ""
coverImage: "/assets/img/2024-07-09-AngularAddicts27NgRx18NewRFCDomRefAPIWebstoragewithSignalsmore_0.png"
date: 2024-07-09 16:27
ogImage:
  url: /assets/img/2024-07-09-AngularAddicts27NgRx18NewRFCDomRefAPIWebstoragewithSignalsmore_0.png
tag: Tech
originalTitle: "Angular Addicts #27: NgRx 18, New RFC: DomRef API, Web storage with Signals , more"
link: "https://medium.com/angularaddicts/angular-addicts-27-ngrx-18-new-rfc-domref-api-web-storage-with-signals-more-13d485f55b81"
---

## 2024년 6월 내가 가장 좋아하는 Angular 자료들

![이미지](/assets/img/2024-07-09-AngularAddicts27NgRx18NewRFCDomRefAPIWebstoragewithSignalsmore_0.png)

# 👋 안녕하세요 Angular 중독자 여러분

이것은 Angular 중독자 뉴스레터의 27번째 호입니다. 매월 주목한 Angular 자료들을 선별한 모음집입니다. (26번째, 25번째, 그리고 24번째 호는 여기에서 확인하세요)

<div class="content-ad"></div>

# 📢 릴리스 공지

## 📢 NgRx 18

지난 달에 최신 버전인 NgRX 18이 릴리스되었습니다. Tim Deschryver의 기사에서는 새로운 NgRx 버전에 대한 주요 사항을 다루고 있습니다:

- 안정적인 NgRx 신호 (거의 완성)
- ESLint v9를 위한 ESLint 플러그인 지원
- 새로운 NgRx Operators 패키지
- 새로운 로고 및 ngrx.io의 재디자인

<div class="content-ad"></div>

# 💬 새로운 RFCs

## 💬 Angular에서 DOM 상호작용

이 RFC는 ElementRef API를 폐기하고 새로운 API인 DomRef로 대체하는 것을 제안합니다.

# 💎 2024년 6월의 Angular 보석들

<div class="content-ad"></div>

## 📰 Angular Forms의 새로운 통합 컨트롤 상태 변경 이벤트

Angular 18에서 Reactive Forms 라이브러리에 새로운 기능이 도입되었습니다. 이 기능은 통합된 컨트롤 상태 변경 이벤트입니다. AbstractControl 클래스(FormControl, FormGroup 및 FormArray의 기본 클래스)에는 이제 업데이트된 Observable`ControlEvent`TValue`` 속성이 있습니다. 이 속성은 값, 상태, pristine 또는 touched 변경 사항에 대한 이벤트를 방출하는 옵저버블입니다. Davide Passafaro의 기사는 Reactive Forms의 기본 사항부터 시작하여 템플릿과의 바인딩, 유효성 검사 및 비활성 상태를 이해하는 내용을 다룹니다. 그런 다음, 새로운 이벤트 옵저버블을 사용하여 폼의 상태를 추적하는 방법에 대해 설명합니다.

## 📰 Angular의 신호와 Web Storage의 동기화

Pierre Bouillon은 이 기사에서 Angular의 신호와 Web Storage API의 Storage 이벤트를 사용하여 브라우저의 Web Storage를 반응적으로 만드는 방법을 소개합니다.

<div class="content-ad"></div>

## 📰 Angular Object Inputs

블로그 게시물에서 Chau Tran은 컴포넌트에 기본값이 있는 많은 입력이 있는 경우에 단일 입력을 제공하는 방법에 대해 설명합니다.

## 📰 hidden 속성을 사용하면 display: none을 정의할 필요가 없어집니다

Stas Melnikov은 CSS Isn't Magic 뉴스레터의 저자입니다. 이 호에서 그는 표시되거나 숨겨질 수 있는 요소에 대한 CSS를 간소화하는 팁을 공유합니다.

<div class="content-ad"></div>

## 📰 Angular로 웹 페이지를 요약하는 텍스트 요약 애플리케이션을 만들었어요

Connie Leung이 Angular에서 텍스트 요약 앱을 만들었습니다. 이 앱을 사용하면 사용자가 텍스트와 주제 힌트를 입력할 수 있고, 텍스트를 요약하여 글머리 기호 목록으로 정리할 수 있어요. 이 앱에는 Google의 Gemini LLM과 통신하는 NestJS 백엔드가 있어요. 전체 소스 코드는 여기에서 확인할 수 있어요

# 👨‍💻 저자 소개

저는 Gergely Szerovay이고, 많은 해 데이터 과학자 및 풀스택 개발자로 일했었고, 최근에는 Angular 기반 프론트엔드 개발에 초점을 맞춘 프론트엔드 기술 리드로 일하고 있어요. 제 역할의 일환으로 Angular와 프론트엔드 개발 시장이 어떻게 변화하는지 지속적으로 따르고 있어요. 제 지식을 공유하고자 2022년에 Angular Addicts 월간 뉴스레터와 출판물을 시작했어요. 매달 발견한 최상의 자료들을 여러분께 전해드릴 수 있도록 노력하고 있어요. 숙련된 Angular 중독자이든 초보자이든, 여러분을 지원하겠습니다. 저에게 작성자로 포함되고 싶은지 알려주세요. 함께 Angular를 배워봐요! 여기서 구독하세요 🔥

<div class="content-ad"></div>

지난 몇 년 동안 Angular가 매우 빠르게 발전해 왔고, 작년에는 생산적 AI의 등장으로 소프트웨어 개발 워크플로우도 빠르게 진화해 왔어요. AI 지원 소프트웨어 개발의 진화를 밀접히 따라가기 위해, 나는 공개적으로 AI 도구를 개발하고 AIBoosted.dev에 진행 상황을 공개하기로 결정했어요. 이러한 학습 여정에 함께하려면 여기를 구독해 주세요 🚀

Angular, Typescript, React 및 Angular로 AI 앱을 구축하는 방법에 대해 더 자세히 알아보려면 Substack(앵귤러 종속성), Substack(AIBoosted.dev), Medium, Dev.to, Twitter 또는 LinkedIn에서 제 팔로우를 해주세요!

# 🕹️이전 이슈

뉴스레터의 이전 이슈를 놓쳤다면 여기에서 읽을 수 있어요. 최신 3 개 이슈는 다음과 같아요:

<div class="content-ad"></div>

- Angular Addicts #26: Angular 18, best practices, recent conference recordings & more
- Angular Addicts #25: Angular과 Wiz가 통합될 예정, React와 Angular의 차이점 등
- Angular Addicts #24: Angular 17.3, Signals 및 유닛 테스팅 모범 사례, Storybook 8 등

# 📨 Angular 자료 제출

요즘 발견했거나 작성한 흥미로운 Angular 관련 기사, 트윗 또는 다른 자료가 있나요? 댓글로 알려주세요 또는 트위터 DM으로 보내주세요! 다음 Angular Addicts 이슈에서 소개할 수 있을지도 모르겠어요!
