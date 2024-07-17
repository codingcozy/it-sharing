---
title: "HTML dialog 태그 게임 체인저급 혁신"
description: ""
coverImage: "/it-shar.ing/assets/no-image.jpg"
date: 2024-07-07 01:48
ogImage:
  url: /it-shar.ing/assets/no-image.jpg
tag: Tech
originalTitle: "New HTML <dialog> tag: An absolute game changer"
link: "https://dev.to/manojgohel/new-html-tag-an-absolute-game-changer-3j8j"
---

### 새로운 `dialog` 태그로 변화된 프론트엔드 개발

#### ❌ 이전:

다이얼로그를 만드는 것은 이전에는 매우 복잡했습니다. 얼마나 많은 작업이 필요했는지 알아봅시다:

```js
<!-- 다이얼로그를 위한 HTML -->
<div class="dialog-overlay">
  <div class="dialog">
    <p>다이얼로그 내용...</p>
    <button class="close-button">닫기</button>
  </div>
</div>
```

<div class="content-ad"></div>

```js
/* 대화 상자에 대한 CSS */
.dialog-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.dialog {
  background: white;
  padding: 20px;
  border-radius: 5px;
}

.close-button {
  background: #f44336;
  color: white;
  border: none;
  padding: 10px;
  cursor: pointer;
}
```

이게 대화 상자의 기본 기능에 대한 CSS입니다. 여전히 매우 평범해 보이고 대화 상자를 열고 닫는 추가 JavaScript가 필요합니다.

#### ✅ 지금:

새로운 `dialog` 태그를 사용하면 훨씬 더 적은 노력으로 동일한 기능을 구현할 수 있습니다.

<div class="content-ad"></div>

```js
<!-- <dialog>를 사용한 HTML -->
<dialog>
  <p>대화 상자 내용...</p>
  <button class="close-button">닫기</button>
</dialog>
```

```js
// 대화 상자를 표시하고 닫기 위한 JavaScript
const dialog = document.querySelector("dialog");
dialog.showModal(); // 대화 상자 표시
dialog.querySelector(".close-button").addEventListener("click", () => dialog.close());
```

show() 메서드를 사용하여 백그라운드가 없는 비모달 대화 상자도 표시할 수 있습니다.

```js
// 비모달 대화 상자 표시를 위한 JavaScript
const dialog = document.querySelector("dialog");
dialog.show(); // 비모달 대화 상자 표시
```

<div class="content-ad"></div>

#### 대화 상자: 중요한 UI 구성 요소

대화 상자는 사용자의 주의를 끌고 중요한 정보를 전달하는 강력한 도구였습니다. 이것은 Material Design부터 Fluent Design까지 모든 UI 디자인 시스템에서 주요 기능입니다.

그러나 대화 상자를 사용하려면 종종 제삼자 라이브러리나 사용자 정의 구성 요소가 필요했습니다. 이러한 라이브러리 중 많은 것들이 사용성 및 접근성을 위한 최고의 관행을 준수하지 않았으며, 예를 들어 Escape 키로 대화 상자를 닫는 기능이 없었습니다.

새로운 `dialog` 태그는 이 모든 것을 간소화합니다.

<div class="content-ad"></div>

#### 자동으로 열리는 대화상자

페이지를 로드하는 순간부터 대화상자를 열어 놓기 위해 open 속성을 사용합니다:

```js
<dialog open>
  <p>자동으로 열리는 대화상자 내용...</p>
</dialog>
```

#### 자동으로 닫히는 버튼

<div class="content-ad"></div>

`dialog`을 사용하면 표준 이벤트 리스너와 close() 메서드를 추가하여 닫기 기능을 구현할 수 있지만 JavaScript가 필요하지 않습니다.

```js
<dialog>
  <p>Dialog content...</p>
  <button class="close-button" onclick="this.closest('dialog').close()">
    Close
  </button>
</dialog>
```

#### `dialog` 태그 스타일링

`dialog` 태그는 배경을 스타일링하기 위한 특별한 ::backdrop 가상 요소가 있습니다.

<div class="content-ad"></div>

```js
dialog::backdrop {
  background: rgba(0, 0, 0, 0.5);
}
```

주요 요소를 스타일링하는 것은 간단합니다:

```js
dialog {
  background: white;
  padding: 20px;
  border-radius: 5px;
  border: none;
}
```

#### 마지막으로 생각할 것들

<div class="content-ad"></div>

새로운 HTML `dialog` 태그를 사용하면 웹 앱에서 모달 창과 대화 상자를 만드는 것이 이전보다 쉽고 빠릅니다. 이 태그는 대화 상자를 구현하는 복잡성을 크게 줄이고 접근성을 향상시키며 개발자가 손쉽게 가장 좋은 방법을 따를 수 있도록 합니다.  
제 블로그를 공유해주시고 마음껏 반응해주세요...

저의 블로그에서 만나요: https://topmate.io/manojgohel
