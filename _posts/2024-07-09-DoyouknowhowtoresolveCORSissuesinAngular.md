---
title: "Angular에서 CORS 문제 해결하는 방법 알아보기"
description: ""
coverImage: "/assets/img/2024-07-09-DoyouknowhowtoresolveCORSissuesinAngular_0.png"
date: 2024-07-09 16:46
ogImage:
  url: /assets/img/2024-07-09-DoyouknowhowtoresolveCORSissuesinAngular_0.png
tag: Tech
originalTitle: "Do you know how to resolve CORS issues in Angular?"
link: "https://medium.com/weekly-webtips/do-you-know-how-to-resolve-cors-issues-in-angular-9d818474825f"
---

<img src="/assets/img/2024-07-09-DoyouknowhowtoresolveCORSissuesinAngular_0.png" />

안녕하세요! 오늘은 CORS 문제 및 클라이언트 측 앵귤러 앱에서 이를 처리하는 방법에 대한 제 경험을 공유하려고 합니다. 이 주제를 시작하기 전에 CORS에 대해 간단히 알려드리고, CORS가 외부 리소스를 관리하는 데 어떻게 사용되는지 설명해 드리겠습니다.

## CORS가 무엇을 의미하나요?

CORS는 Cross-Origin Resource Sharing의 약자입니다. 이는 같은 출천 정책을 우회하여 한 출처이 리소스가 다른 출처에서 리소스에 안전하게 액세스할 수 있도록 하는 메커니즘입니다.

<div class="content-ad"></div>

## CORS는 외부 도메인에서의 요청을 어떻게 관리하나요?

CORS는 외부 도메인에서의 요청을 아래와 같은 새로 생성된 HTTP 헤더 세트를 사용하여 관리합니다:

- Access-Control-Allow-Origin
- Access-Control-Allow-Credentials
- Access-Control-Allow-Headers
- Access-Control-Allow-Methods
- Access-Control-Expose-Headers
- Access-Control-Max-Age
- Access-Control-Request-Headers
- Access-Control-Request-Method
- Origin

## CORS 문제가 발생하는 이유는 무엇인가요?

<div class="content-ad"></div>

웹 응용 프로그램에서 CORS 문제가 발생할 수 있습니다. 이는 백엔드 서버(귀하의 서비스)가 다른 도메인에서 실행되고 제대로 구성되지 않은 경우에 발생합니다.

따라서 CORS 문제는 Angular 애플리케이션을 개발하는 동안 발생할 수 있습니다.

## CORS 문제를 해결하는 방법은?

우리는 두 가지 방법으로 CORS 문제를 해결할 수 있습니다:

<div class="content-ad"></div>

- 문제를 해결하는 한 가지 방법은 서버 측에서 적절한 CORS 헤더 요청을 활성화하는 것입니다.
- 또 다른 방법은 Angular CLI 프록시를 구성하는 것입니다.

## Angular CLI란 무엇인가요?

Angular CLI는 개발 워크플로우를 자동화하기 위한 명령줄 인터페이스 도구입니다. Angular 애플리케이션을 초기화, 개발, 스캐폴딩 및 유지보수하는 데 사용됩니다.

## Angular CLI 프록시란 무엇인가요?

<div class="content-ad"></div>

Angular CLI 프록시는 브라우저와 백엔드 서버 간의 중간 역할을 합니다. 한번 설정하면 교차 출처 제한을 비활성화합니다.

Angular CLI 프록시를 사용하여 CORS 문제를 해결하는 방법을 살펴보겠습니다.

다음과 같이 표시된 코드로 실행 중인 포트 4200의 스프링 부트 애플리케이션에 서버 측 요청을 보내는 Angular 애플리케이션을 만들어 봅시다.

![image](/assets/img/2024-07-09-DoyouknowhowtoresolveCORSissuesinAngular_1.png)

<div class="content-ad"></div>

다음과 같이 포트 8090에서 실행 중인 다른 스프링 부트 애플리케이션을 사용하여 프록시를 구성해 봅시다.

![이미지](/assets/img/2024-07-09-DoyouknowhowtoresolveCORSissuesinAngular_2.png)

그리고 아래에 나타난 대로 src 폴더 아래에 프록시 구성 파일을 만들어주세요.

아래는 저의 샘플 Angular proxy.conf.json 파일 화면입니다.

<div class="content-ad"></div>

아래 Markdown 포맷으로 표 태그를 변경해주세요.

| `<img src="/assets/img/2024-07-09-DoyouknowhowtoresolveCORSissuesinAngular_3.png" />` |
| ------------------------------------------------------------------------------------- |
| Next, add proxyConfig key to the angular.json file as shown below.                    |

| Add it under “serve” key “options” key as a new key value pair entry. Below is the screen of my sample angular angular.json file. |

| `<img src="/assets/img/2024-07-09-DoyouknowhowtoresolveCORSissuesinAngular_4.png" />` |

<div class="content-ad"></div>

마침내, 아래와 같이 Angular CLI 프록시를 활성화하여 serve 명령을 사용하여 Angular 앱을 실행할 수 있습니다.

```js
ng serve
```

이 경우에는 "환경 변수" "시스템 변수" 경로 아래에 %AppData%\npm을 추가해야 합니다.

다른 대안은 다음 명령을 실행하는 것입니다.

<div class="content-ad"></div>

```js
npm run ng serve
```

성공적으로 명령어를 실행한 후에는 브라우저에서 localhost:4200/API를 입력하여 서버 API를 테스트할 수 있습니다. 이상이 없다면 테스트 API 값을 반환할 것입니다. 아래는 제 샘플 Angular 애플리케이션 화면입니다.

![Sample Angular Application](/assets/img/2024-07-09-DoyouknowhowtoresolveCORSissuesinAngular_5.png)

아래에 서버 호스트에서 동일한 API를 실행한 내용이 표시되어 있습니다.

<div class="content-ad"></div>

<img src="/assets/img/2024-07-09-DoyouknowhowtoresolveCORSissuesinAngular_6.png" />

같은 문제에 직면한 동료 개발자들에게 도움이 되길 바랍니다. Angular 앱을 개발하는 동안 CORS 문제를 해결해야 하지만 백엔드 서버에 액세스할 수 없는 경우에도 개발 작업을 진행해야 하는 사람들을 위해서입니다.

이 콘텐츠를 읽는 누구든지 피드백을 공유해 주시면 제게 큰 힘이 될 것입니다. 읽어주셔서 감사합니다.

참고 문서: https://angular.io, https://en.wikipedia.org/wiki/Proxy_server

<div class="content-ad"></div>

또, Angular에 대한 멋진 내용이 포함된 이 기사를 확인해보세요 - https://materialize-thoughts.com/how-to-use-ai-name-generator-api/
