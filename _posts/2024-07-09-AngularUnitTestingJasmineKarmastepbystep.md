---
title: "Angular 유닛 테스트 Jasmine과 Karma를 단계별로 배우는 방법"
description: ""
coverImage: "/assets/img/2024-07-09-AngularUnitTestingJasmineKarmastepbystep_0.png"
date: 2024-07-09 17:28
ogImage:
  url: /assets/img/2024-07-09-AngularUnitTestingJasmineKarmastepbystep_0.png
tag: Tech
originalTitle: "Angular: Unit Testing Jasmine, Karma (step by step)"
link: "https://medium.com/swlh/angular-unit-testing-jasmine-karma-step-by-step-e3376d110ab4"
---

<img src="/assets/img/2024-07-09-AngularUnitTestingJasmineKarmastepbystep_0.png" />

어떤 프로젝트에서도 단위 테스팅을 수행하는 것은 중요합니다. TDD(test-driven development) 방법을 선택하든 하지 않든, 단위 테스팅을 사용하면 많은 이점을 얻을 수 있습니다.

이 글에서는 먼저 단위 테스팅의 이점을 간단히 언급한 후에, jasmine와 karma를 사용하여 Angular 단위 테스트의 완전한 예시를 작성하고, 과정 각 단계를 설명하겠습니다.

## 단위 테스트의 이점

<div class="content-ad"></div>

제가 생각하기에 솔루션에서 단위 테스트를 사용하는 주요 이유를 먼저 살펴봅시다...

- 구현 디자인 향상:
  디자인에 대해 많은 생각을 하지 않고 기능 코딩을 시작하는 것은 개발자들 사이에서 매우 흔한 실수입니다. 단위 테스트를 사용하면 디자인에 대해 생각하도록 강조되며, TDD를 사용한다면 영향이 더 커집니다.
- 리팩토링 가능:
  이미 모든 것이 예상대로 작동하는 것을 보장하는 테스트가 있기 때문에 해당 코드에 변경을 쉽게 추가할 수 있으며 버그가 추가되지 않음을 확신할 수 있습니다.
- 다른 부분을 손상시키지 않고 새로운 기능 추가:
  새로운 기능을 추가할 때 테스트를 실행하여 응용 프로그램의 다른 부분을 손상시키지 않았는지 확인할 수 있습니다.

더 많은 이유가 있지만, 이 세 가지만으로도 어떤 프로젝트에서도 큰 이득을 얻을 수 있어서 저에게는 결정 요소입니다만, 아직 납득되지 않았다면 좀 더 언급해볼까요?

- 테스트는 좋은 문서입니다.
- 테스트로 개발자들은 자신의 작업에 대해 더 확신을 갖습니다.

<div class="content-ad"></div>

재미있는 시간을 갖도록 해봅시다... 우리는 Angular, Jasmine 및 Karma를 사용하여 작은지만 꽤 완전한 애플리케이션 예제를 만들어볼 것입니다.

다음은 우리가 다루게 될 내용 몇 가지입니다:

- 도구 karma와 jasmine에 대해 간단히 설명합니다.
- karma 구성에 대해 설명합니다.
- 테스트 진입 파일을 설명합니다.
- 첫 번째 간단한 테스트를 만듭니다. Jasmine 및 Angular 테스팅 기능을 소개합니다.
- Angular 양식을 테스트합니다. Jasmine 및 Angular 테스팅 기능을 소개합니다.
- 서비스를 이용한 구성요소를 테스트합니다. Angular 테스팅 기능을 소개합니다.

## Jasmine와 Karma로 Angular 프로젝트 만들기

<div class="content-ad"></div>

앵귤러 팀의 권장에 따라 우리는 앵귤러 CLI를 사용하여 앱을 생성할 것입니다. 이렇게 함으로써 jasmine과 karma의 구성이 자동으로 해결됩니다.

앵귤러 CLI를 설치하고 새 프로젝트를 생성하세요:

- npm install -g @angular/cli
- ng new angular-unit-testing

프로젝트를 생성하면 모든 종속 항목이 설치되며 테스트를 작성하는 데 필요한 모든 것이 함께 설치됩니다.

<div class="content-ad"></div>

![AngularUnitTestingJasmineKarmastepbystep_1.png](/assets/img/2024-07-09-AngularUnitTestingJasmineKarmastepbystep_1.png)

위의 이미지에서는 테스트 목적으로 설치된 모든 종속성을 볼 수 있습니다. 더 중요한 것들을 살펴보겠습니다:

- jasmine-core. Jasmine은 테스트를 만들기 위해 사용할 프레임워크입니다. 다양한 종류의 테스트를 작성할 수 있도록 다양한 기능을 제공합니다.
- karma. Karma는 테스트를 위한 작업 실행기입니다. 시작 파일, 리포터, 테스트 프레임워크, 브라우저 등을 설정하는 구성 파일을 사용합니다.
- 나머지 종속성은 주로 테스트용 리포터, karma 및 jasmine 사용 도구, 브라우저 런처입니다.

테스트를 실행하려면 "ng test" 명령어를 실행하기만 하면 됩니다. 이 명령은 테스트를 실행하고 브라우저를 열고 콘솔 및 브라우저 리포트를 표시하며, 더 중요한 것은 테스트 실행을 watch 모드로 남겨놓습니다.

<div class="content-ad"></div>

## 카르마 구성

앵귤러 CLI에 의해 생성된 카르마 구성 파일을 살펴봅시다.

![이미지](/assets/img/2024-07-09-AngularUnitTestingJasmineKarmastepbystep_2.png)

아마 대부분의 구성 속성이 어떤 용도인지 추측할 수 있을 것입니다. 하지만 몇 가지를 함께 살펴보겠습니다.

<div class="content-ad"></div>

- frameworks: 여기서 jasmine을 테스팅 프레임워크로 설정합니다. 다른 프레임워크를 사용하려면 여기서 설정하세요.
- reporters: 여기서 리포터를 설정합니다. 변경하거나 새로운 것을 추가할 수 있습니다.
- autoWatch: 이 값을 true로 설정하면 테스트는 감시 모드에서 실행됩니다. 테스트를 변경하고 파일을 저장하면 테스트가 다시 빌드되고 다시 실행됩니다.
- browsers: 여기서 테스트가 실행될 브라우저를 설정합니다. 기본적으로 chrome이지만 다른 브라우저 런처를 설치하고 사용할 수 있습니다.

## 테스트 진입 파일

Angular-cli의 카르마 구성은 애플리케이션의 테스트의 시작점으로 "test.ts" 파일을 사용합니다. 해당 파일을 살펴보겠습니다;

![Test Entry File](/assets/img/2024-07-09-AngularUnitTestingJasmineKarmastepbystep_3.png)

<div class="content-ad"></div>

여기에는 많은 것이 진행 중입니다. 아마도 이 파일을 변경할 일은 거의 없겠지만 함께 논의해보겠습니다.

- 파일의 시작부분에 있는 모든 가져오기를 사용하여 Angular 테스트를 실행하는 환경이 생성중입니다.
- TestBed는 Angular에서 제공하는 강력한 단위 테스트 도구이며 이 파일에서 초기화됩니다.
- 마지막으로 karma는 애플리케이션의 모든 테스트 파일을 로드하며 파일 이름을 정규식과 일치시킵니다. app 폴더 내에 있는 모든 "spec.ts"이라는 이름의 파일은 테스트로 간주됩니다.

## 우리의 첫 번째 테스트

첫 번째 테스트를 만들어 봅시다. app.component.ts에 대한 테스트를 작성해보겠습니다. 이 컴포넌트에는 "text"라는 값을 가진 속성만 있으며 해당 값은 "h1" 태그 내부에 렌더링되며 라우팅 루트 엘리먼트와 몇 가지 라우팅 링크도 포함되어 있습니다. 컴포넌트에 실제로 해당 속성이 있는지 그리고 실제로 HTML에 렌더링되는지 확인하는 테스트 파일을 만들어 보겠습니다.

<div class="content-ad"></div>

![Angular Unit Testing](/assets/img/2024-07-09-AngularUnitTestingJasmineKarmastepbystep_4.png)

여기에서 무슨 일이 일어나고 있는지 알아보겠습니다:

- 사용할 모든 Angular 테스트 도구를 가져옵니다.
- 이 컴포넌트가 가지고 있는 모든 종속성을 가져옵니다.
- "describe"를 사용하여 테스트 블록을 시작하며 제목은 테스트 대상 컴포넌트의 이름과 일치합니다.
- 비동기 beforeEach를 사용합니다. 비동기의 목적은 가능한 모든 비동기 코드가 완료될 때까지 기다리는 것입니다.

Angular에서 어떤 테스트를 실행하기 전에는 Angular testbed를 구성해야 합니다. 이를 통해 테스트 대상 컴포넌트를 위한 Angular 환경을 만들 수 있습니다. 테스트 대상 컴포넌트에 필요한 모든 모듈, 컴포넌트 또는 서비스는 testbed에 포함되어야 합니다. 마지막으로 구성을 설정한 후에는 컴포넌트를 컴파일하는 함수를 호출합니다.

<div class="content-ad"></div>

app.component에 대한 더미 라우트 모듈을 구성하고 베이스 href를 설정하기 위한 프로바이더를 사용해야 합니다. 이 작업을 거치지 않으면 라우팅 모듈이 설정되고 베이스 href가 필요하기 때문에 테스트가 컴파일되지 않을 것입니다.

마지막으로, 두 개의 테스트가 있습니다. 첫 번째 테스트를 진행해 봅시다.

- 첫 번째 테스트에서, 컴포넌트가 실제로 "title" 속성에 예상한 텍스트를 포함하는지 확인합니다.
- 먼저, app.component의 인스턴스를 가져야 합니다. 이를 위해 앵귤러 testbed의 createComponent 함수를 사용하여 컴포넌트의 인스턴스를 생성할 수 있는 fixture 객체를 얻습니다.
- 이제 app.component의 인스턴스를 갖고 있으므로 텍스트 속성의 값을 확인하고 jasmine expect를 사용하여 예상 값과 동일한지 확인할 수 있습니다.

두 번째 테스트도 비슷한 작업을 수행하지만 돔이 "text" 속성을 렌더링하는지 확인합니다.

<div class="content-ad"></div>

- 먼저, 다른 테스트와 같이, app.component fixture를 가져온 후 detect changes 함수를 실행합니다. 이 함수는 컴포넌트 변경을 HTML에 적용합니다 (이 경우 "text" 컴포넌트 속성에 보간을 DOM에 적용합니다).
- 다음으로, 컴파일된 HTML의 네이티브 요소(컴포넌트에 의해 렌더링된 HTML)를 가져옵니다.
- 마지막으로, "text" 값을 포함하는 "h1"을 선택하고 선택된 HTML에 예상 값이 포함되어 있는지 확인합니다.

## 폼 테스트

이제 앵귤러 폼을 테스트하는 방법을 살펴보겠습니다. 먼저 contact.component HTML을 확인해 봅시다;

![이미지](/assets/img/2024-07-09-AngularUnitTestingJasmineKarmastepbystep_5.png)

<div class="content-ad"></div>

이것은 매우 간단한 작업이므로 설명이 필요하지 않습니다. 이것은 폼 컨트롤을 사용한 일반적인 앵귤러 폼입니다. 폼이 유효하지 않은 경우 제출 버튼이 비활성화됩니다.

이제 contact.component 파일을 살펴보겠습니다.

![image](/assets/img/2024-07-09-AngularUnitTestingJasmineKarmastepbystep_6.png)

이 컴포넌트도 매우 쉽게 이해할 수 있습니다. 제출 함수는 제출된 속성을 true로 변경합니다. 연락처 폼에는 세 가지 컨트롤과 해당 유효성이 있습니다.

<div class="content-ad"></div>

자, 이 컴포넌트에 대한 테스트를 살펴보겠습니다;

![Test Image](/assets/img/2024-07-09-AngularUnitTestingJasmineKarmastepbystep_7.png)

이 테스트는 이전에 본 것과 많은 차이점이 있습니다. 걱정 마세요, 각각에 대해 이야기할 거예요.

- 먼저, "By"를 도입하여 DOM에서 요소를 선택할 수 있게 해주는 임포트 섹션이 있습니다. 이외에는 이상한 점이 없어요.
- 테스트 블록은 테스트할 컴포넌트의 이름으로 선언합니다.
- 각 테스트에서 사용할 일부 테스트 범위 개체를 생성할 거에요. 이는 "beforeEach"에서 초기화할 것입니다.
- "beforeEach"의 첫 부분에서 시작하기 위해 필요한 모든 종속 항목을 설정합니다. 이미 "비동기"에 대한 이유를 언급한 바 있습니다.
- 이 예에서는 "compileComponents" 함수가 반환하는 프로미스를 사용합니다. 프로미스가 해결되면, 처음에 선언한 각 변수에 값을 주게 됩니다.
- 첫 번째 테스트에서는 컴포넌트 인스턴스가 "text" 속성의 예상 값이 있는지 확인합니다.
- 두 번째 테스트에서는 "onSubmit" 함수가 호출될 때 컴포넌트의 "submitted" 속성을 true로 기대합니다.
- 세 번째 테스트에서는 "fixture" 개체의 "detectChanges" 함수를 사용하여 컴포넌트 상태를 HTML에 적용하고, DOM에서 제출 버튼을 가져와 클릭 이벤트를 트리거합니다. 이 모든 작업 이전에 컴포넌트의 "onSubmit" 함수에 대한 jasmine "spy"를 생성합니다. 마지막으로, 버튼이 비활성화되어야 하므로 폼이 유효하지 않기 때문에 스파이드 함수가 실행되지 않는 것을 기대합니다.
- 네 번째 테스트에서는 컴포넌트 폼에 유효하지 않은 값이 설정되고, 폼 유효 속성이 false가 되기를 기대합니다.
- 마지막으로, 다섯 번째 테스트에서는 폼에 유효한 값이 설정되고, 폼 유효 속성이 true가 되기를 기대합니다.

<div class="content-ad"></div>

이 기사를 마무리하기 전에 한 가지 더 살펴보겠습니다. 테스트 중인 컴포넌트가 서비스를 사용할 때 어떻게 처리하는지 볼 것입니다.

## 서비스를 사용하는 컴포넌트 테스트

이미 본 것처럼 서비스를 사용하는 컴포넌트를 테스트할 때는 "beforeEach"에서 생성한 테스트 모듈에 제공자(Providers)를 추가해야 합니다. 중요한 것은 실제 서비스를 사용하는 것이 아니라 모의 버전을 사용하고 싶을 것이라는 점인데, 그런 방법을 살펴봅시다...

먼저, 컴포넌트의 구현을 살펴보겠습니다;

<div class="content-ad"></div>

이것은 서비스에서 사용자 목록을 가져오는 간단한 컴포넌트입니다.

실제 서비스 구현은 중요하지 않습니다. 사용자를 어디서 가져오든 상관없지만, 서비스 구현을 모킹하여 컴포넌트에 대한 테스트를 만드는 방법을 살펴봅시다.

<div class="content-ad"></div>

이 테스트는 이전에 본 예제들과 유사하지만 한 가지 주요 차이가 있습니다. 테스트 모듈을 선언하는 공급자에서는 "UserService" 서비스를 주입할 때 대신 "UserServiceMock"를 사용하도록 모듈에 알려줍니다. "UserServiceMock"는 테스트 컴포넌트의 테스트를 실행하기 위해 더미 데이터를 반환하는 더미 서비스입니다. 그게 전부입니다. 컴포넌트를 테스트할 때 서비스를 모킹하는 방법입니다.

## 결론

Angular 컴포넌트를 테스트하는 방법을 설명하기 위해 다양한 기능과 예제를 살펴보았습니다. 보시다시피 매우 간단한 것을 확인하실 수 있고 다른 유형의 테스트를 수행할 수 있습니다.
이 글이 Angular 테스트 도구를 좀 더 잘 활용하는 데 도움이 되기를 바라겠습니다.

GitHub 링크
