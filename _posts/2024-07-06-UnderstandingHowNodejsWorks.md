---
title: "Nodejs 작동 원리 이해하기"
description: ""
coverImage: "/assets/img/2024-07-06-UnderstandingHowNodejsWorks_0.png"
date: 2024-07-06 09:55
ogImage:
  url: /assets/img/2024-07-06-UnderstandingHowNodejsWorks_0.png
tag: Tech
originalTitle: "Understanding How Node.js Works"
link: "https://medium.com/@victorpianwi/understanding-how-node-js-works-ea2d1a11ae6b"
---

/assets/img/2024-07-06-UnderstandingHowNodejsWorks_0.png

Node.js는 강력한 서버 측 JavaScript 런타임 환경으로, 웹 브라우저 외부에서 JavaScript 코드를 실행합니다.

Node.js는 이벤트 기반, 비차단 I/O 모델을 기반으로 구축되어 있습니다. I/O 작업이 완료될 때까지 기다리는 대기 없이 많은 동시 연결을 처리할 수 있습니다.

Node.js는 Google이 Chrome 브라우저용으로 개발한 V8 JavaScript 엔진을 사용합니다. 이 엔진은 효율적인 실행을 위해 JavaScript 코드를 직접 기계 코드로 컴파일합니다.

<div class="content-ad"></div>

Node.js는 이벤트를 처리하고 파일 I/O, 네트워킹, 타이머와 같은 비동기 작업을 관리하기 위해 Libuv라는 크로스 플랫폼 비동기 I/O 라이브러리를 사용합니다. Libuv는 각 운영 체제가 I/O 작업을 처리하는 방식의 차이를 추상화합니다.

Node.js는 단일 스레드 이벤트 루프에서 작동합니다. 이는 모든 I/O 작업이 논블로킹이며 비동기적으로 처리된다는 것을 의미합니다. I/O 작업이 시작되면 Node.js는 콜백 함수를 등록하고 다른 코드를 계속 실행합니다. 작업이 완료되면 콜백이 이벤트 루프의 대기열로 푸시되어 실행됩니다.

Node.js에는 코드를 재사용 가능한 구성 요소로 모듈화할 수 있게 해주는 내장 모듈 시스템이 있습니다. 코어 모듈은 파일 시스템 I/O, 네트워킹, HTTP 처리와 같은 기본 기능을 제공하며, 세 번째 자 모듈은 Node.js 패키지 관리자인 npm을 통해 설치할 수 있습니다.

Node.js는 HTTP 요청 및 응답을 처리하는 웹 서버를 만들 수 있습니다. 내장 http 모듈을 사용하면 개발자가 빠르고 쉽게 가벼운 고성능 웹 서버를 만들 수 있습니다.

<div class="content-ad"></div>

결론

Node.js의 이벤트 기반, 논블로킹 아키텍처는 V8 엔진과 Libuv와 결합되어 확장 가능하고 실시간 웹 애플리케이션 및 API를 구축하기에 적합합니다.

Node.js를 사용하기 시작한 이후 이벤트 기반, 비동기 I/O 모델 덕분에 웹 애플리케이션이 더 빨라졌습니다.
