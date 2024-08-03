---
title: "Power Pages 모달에서 ESC 비활성화하는 방법"
description: ""
coverImage: "/assets/img/2024-08-03-PowerPageshowtodisableESConmodals_0.png"
date: 2024-08-03 20:45
ogImage: 
  url: /assets/img/2024-08-03-PowerPageshowtodisableESConmodals_0.png
tag: Tech
originalTitle: "Power Pages how to disable ESC on modals"
link: "https://dev.to/andrewelans/power-pages-how-to-disable-esc-on-modals-d4a"
---


Power Pages의 이전 버전은 Bootstrap 3을 사용하고, 최신 버전은 Bootstrap 5를 사용하며, 모두 jQuery를 사용합니다.

## jQuery 및 Bootstrap 버전 확인

콘솔에 다음을 입력하세요:

- $.fn.jquery =` jQuery 버전 3.6.2
- $.fn.tooltip.Constructor.VERSION =` Bootstrap 버전은 포털이 작성된 시점에 따라 ~3.4.1 또는 ~5.2.2입니다

<div class="content-ad"></div>

저의 경우는 2년 전에 프로비저닝된 포털에 부트스트랩 3.4.1을 사용하고 있고, 최근에는 5.2.2를 사용하고 있습니다.

## ESC 키로 모달

사용자 입력을 받기 위해 모달을 사용하고 있습니다. 모달에는 사용자가 입력해야 할 많은 항목이 있기 때문에 ESC 키를 사용한 모달 닫기 기능을 비활성화해야 합니다.

escape 키가 눌렸을 때 모달을 닫는 것을 비활성화하기 위해 keyboard라는 내장 옵션이 있습니다. 이 속성을 false로 설정하면 ESC 키로 모달을 닫지 않을 수 있습니다.

<div class="content-ad"></div>

안타깝지만 모달에 data-keyboard="false"를 추가해도 모달의 동작에는 변화가 없고 ESC 키로 모달이 정상적으로 사라집니다.

다음 코드를 추가하면 모달이 포커스를 가지고 있을 때에만 ESC 키가 비활성화됩니다:

```js
document.querySelector("#fa-edit-modal").addEventListener("keydown", (e) => {
    if (e.key === "Escape") {
        e.stopPropagation()
    }
})
```

그러나 사용자로부터 작업을 확인하는 다이얼로그와 같이 모달이 포커스를 잃은 상태에서 열려 있을 때에는 ESC 키를 눌러도 모달이 정상적으로 닫힙니다.

<div class="content-ad"></div>

부트스트랩 모달에서 키보드를 false로 설정하는 이유를 파헤쳐 보기 시작했어요.

## 모달의 이벤트 리스너

이벤트 리스너를 확인하기 위해 개발자 도구를 사용하세요. =` Elements에서 모달을 선택하세요. =` 이벤트 리스너 탭 =` 키다운 이벤트:

![이미지](/assets/img/2024-08-03-PowerPageshowtodisableESConmodals_0.png)

<div class="content-ad"></div>

한 줄씩 확인하면서 keydown 스크립트를 살펴보니 총 코드 중 모달을 닫는 역할을 하는 다음 코드를 발견했어요:

![code image](/assets/img/2024-08-03-PowerPageshowtodisableESConmodals_1.png)

이 코드로부터 어떤 점들을 알 수 있을까요?

- 문서 수준에 이벤트 리스너가 설정되어 있고 keycode 27인 ESC 키가 눌렸을 때 실행됩니다.
- 만약 모달이 표시 중이라면 (c(".modal").is(":visible")) 모달이 닫힙니다: c(".modal").modal("hide") =` 여기가 우리가 찾는 부분입니다.
- 여기서 'c'는 jQuery 객체를 가리킵니다.
- 문서에 위치해 있기 때문에 이벤트는 타깃에서 문서 수준까지 버블링되어 실제로 포커스된 모든 요소에서 실행됩니다.
- 이 스크립트는 Power Pages 번들에 속해 있어 Bootstrap의 기능을 무시합니다.

<div class="content-ad"></div>

jQuery 특정 이벤트 리스너를 확인하려면 콘솔에 $._data(document, "events")를 입력하면 됩니다. 이 이벤트는 이미지에서 볼 수 있습니다:

![이미지](/assets/img/2024-08-03-PowerPageshowtodisableESConmodals_2.png)

## 해결 방법

jQuery 이벤트를 제거하려면 다음과 같이 .off() 명령어를 사용합니다: $(document).off("keydown").

<div class="content-ad"></div>

그 결과로, 이제 모달 종료 기능의 동작을 부트스트랩 `keyboard` 옵션으로 제어할 수 있습니다.