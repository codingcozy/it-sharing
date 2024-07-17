---
title: "HTML 웹 스토리지와 웹 스토리지 객체 이해하기"
description: ""
coverImage: "/it-shar.ing/assets/no-image.jpg"
date: 2024-07-09 12:51
ogImage:
  url: /it-shar.ing/assets/no-image.jpg
tag: Tech
originalTitle: "HTML web storage and web storage objects"
link: "https://dev.to/wasifali/html-web-storage-and-web-storage-objects-hcd"
---

## HTML Web Storage

웹 스토리지를 사용하면 웹 응용 프로그램이 사용자의 브라우저 내에서 데이터를 로컬로 저장할 수 있습니다. 웹 스토리지는 보안이 더 우수하며 대량의 데이터를 로컬에 저장할 수 있어도 웹 사이트의 성능에 영향을 주지 않습니다. 웹 스토리지는 원점당 즉, 도메인 및 프로토콜당으로 구분됩니다. 같은 원점에서 가져온 모든 페이지는 동일한 데이터를 저장하고 액세스할 수 있습니다.

## API 및 웹 스토리지

- Google= 4.0
- Microsoft Edge= 8.0
- Firefox= 3.5

<div class="content-ad"></div>

## HTML 웹 저장 객체들

HTML 웹 저장은 클라이언트에 데이터를 저장할 수 있는 두 가지 객체를 제공합니다:
window.localStorage - 만료 날짜가 없는 데이터를 저장합니다
window.sessionStorage - 한 세션 동안 데이터를 저장합니다

```js
if (typeof Storage !== "undefined") {
  // localStorage/sessionStorage 코드
} else {
  // 죄송합니다! 웹 저장을 지원하지 않습니다.
}
```

## localStorage 객체

<div class="content-ad"></div>

로컬 스토리지 객체는 만료 날짜가 없는 데이터를 저장합니다.
브라우저를 닫아도 데이터가 삭제되지 않으며, 다음 날, 주, 또는 년도에도 사용할 수 있습니다.

```js
// 저장
localStorage.setItem("lastname", "Smith");

// 검색
document.getElementById("result").innerHTML = localStorage.getItem("lastname");
```
