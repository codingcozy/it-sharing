---
title: "인터랙티브 모바일 웹 앱을 위한 흔들림 감지 구현하기"
description: ""
coverImage: "/assets/img/2024-07-30-ImplementingShakeDetectionforInteractiveMobileWebAppsJavaScript_0.png"
date: 2024-07-30 17:02
ogImage: 
  url: /assets/img/2024-07-30-ImplementingShakeDetectionforInteractiveMobileWebAppsJavaScript_0.png
tag: Tech
originalTitle: "Implementing Shake Detection for Interactive Mobile Web Apps JavaScript"
link: "https://medium.com/@louistrinh/implementing-shake-detection-for-interactive-mobile-web-apps-javascript-4eb8c2393055"
isUpdated: true
updatedAt: 1723817112333
---



이미지 태그를 Markdown 형식으로 변경하십시오.


![이미지](/assets/img/2024-07-30-ImplementingShakeDetectionforInteractiveMobileWebAppsJavaScript_0.png)


## 흔들림 이벤트를 감지하는 이유

모바일 웹 앱에서 흔들림 이벤트를 감지하려는 이유는 다음과 같습니다:

- 상호 작용 기능: 컨텐츠 새로 고침, 게임에서 주사위 굴리기 또는 사용자 정의 작업 트리거와 같은 재미있고 매료되는 상호 작용을 구현합니다.
- 접근성: 터치 스크린을 사용하는 데 어려움을 겪는 사용자를 위한 대체 제어 방법을 제공합니다.

<div class="content-ad"></div>

## DeviceMotionEvent API 사용 방법(라이브러리 사용 안 함):

- window.addEventListener(`devicemotion`, handleMotion)을 사용하여 기기의 가속도계 데이터에 액세스합니다.
- event.acceleration.x, event.accelerationIncludingGravity.y 및 event.accelerationIncludingGravity.z를 기반으로 각 축(X, Y, Z)의 가속도를 계산합니다.
- 가속도 크기가 일정한 한계를 초과하는 가속도.magnitude를 기반으로 시간대 내에서 흔들림을 판단하는 임계값 기반 탐지 메커니즘을 구현합니다.
- 저역통과 필터링과 같은 기법을 사용하여 잡음과 우연한 움직임을 걸러내는 것을 고려합니다.

```js
function handleMotion(event) {
    const x = event.accelerationIncludingGravity.x;
    const y = event.accelerationIncludingGravity.y;
    const z = event.accelerationIncludingGravity.z;

    const acceleration = Math.sqrt(x * x + y * y + z * z);

    // 여기에 흔들림 감지 논리를 구현합니다
    // 예: 일정 시간 내에 가속도 한계를 초과하는지 확인
    if (acceleration > 15 && /* 시간 창에 대한 추가 조건 */) {
        console.log('흔들림 감지!');
    }
}

window.addEventListener('devicemotion', handleMotion);
```

## JavaScript 라이브러리 사용 방법(Shake.js):

<div class="content-ad"></div>

- CDN 또는 패키지 관리자를 사용하여 프로젝트에 Shake.js 라이브러리를 포함시킵니다.
- threshold, timeout 및 onshake 콜백과 같은 사용자 정의 옵션을 사용하여 Shake 인스턴스를 만듭니다.
- start()를 호출하여 흔들림 이벤트를 청취합니다.

```js
var myShakeEvent = new Shake({
    threshold: 15, // 선택적인 흔들림 강도 임계값
    timeout: 1000 // 선택적으로 이벤트 생성 빈도 결정
});

myShakeEvent.start();

window.addEventListener('shake', shakeEventDidOccur, false);

function shakeEventDidOccur() {
    console.log('흔들림 감지됨!');
}
```

## 추가 고려 사항:

- 사용자 개인정보: DeviceMotionEvent.requestPermission()을 사용하여 iOS 기기의 장치 모션 센서 접근 권한 요청.
- 세부 조정: 앱의 요구 사항에 따라 임계값 및 timeout 값을 조정하여 우연한 움직임으로 인한 잘못된 양성 결과를 피합니다.
- 현대적 브라우저: devicemotion이 널리 지원되지만 대상 브라우저와의 호환성을 확인하세요.