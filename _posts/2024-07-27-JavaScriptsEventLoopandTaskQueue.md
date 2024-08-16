---
title: "자바스크립트 이벤트 루프와 태스크 큐 정리"
description: ""
coverImage: "/assets/img/2024-07-27-JavaScriptsEventLoopandTaskQueue_0.png"
date: 2024-07-27 13:55
ogImage: 
  url: /assets/img/2024-07-27-JavaScriptsEventLoopandTaskQueue_0.png
tag: Tech
originalTitle: "JavaScripts Event Loop and Task Queue"
link: "https://medium.com/@louistrinh/javascripts-event-loop-and-task-queue-a020dcf7f131"
isUpdated: true
updatedAt: 1723816384116
---



![이미지](/assets/img/2024-07-27-JavaScriptsEventLoopandTaskQueue_0.png)

## 이벤트 루프 및 작업 대기열: JavaScript 실행의 핵심

자바스크립트는 다른 많은 프로그래밍 언어와 달리 단일 스레드 실행 모델을 사용합니다. 이는 한 번에 한 가지 작업만 처리할 수 있다는 것을 의미합니다. 그러나 동시성 및 반응성의 환상을 제공하기 위해 자바스크립트는 이벤트 루프와 작업 대기열을 함께 활용하는 정교한 메커니즘을 사용합니다.

## 이벤트 루프: 지휘자

<div class="content-ad"></div>

이벤트 루프는 다음을 계속 모니터하는 무한 루프입니다:

- Call Stack: 현재 실행 중인 함수와 해당 지역 변수를 보관하는 스택

- Task Queue (Message Queue): 실행 대기 중인 함수(보통 콜백이라고 불림)를 보관하는 큐

이벤트 루프는 계속해서 호출 스택이 비어있는지 확인합니다.

<div class="content-ad"></div>

만약 호출 스택이 비어 있다면, 이벤트 루프는 다음 작업(콜백 함수)을 작업 큐에서 가져와요.
가져온 작업을 호출 스택에 푸시하고, 호출 스택에서 함수를 실행해요.

이 과정이 무한히 반복되며, 새로운 작업이 발생할 때 JavaScript 코드가 항상 실행되거나 실행 준비가 되어 있도록 합니다.