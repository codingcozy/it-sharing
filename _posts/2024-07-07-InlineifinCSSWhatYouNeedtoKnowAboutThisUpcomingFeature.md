---
title: "CSS에서 if 함수 사용법  알아야 할 최신 기능"
description: ""
coverImage: "/it-shar.ing/assets/no-image.jpg"
date: 2024-07-07 01:49
ogImage:
  url: /it-shar.ing/assets/no-image.jpg
tag: Tech
originalTitle: "Inline if() in CSS 🤔: What You Need to Know About This Upcoming Feature"
link: "https://dev.to/hunzaboy/inline-if-in-css-what-you-need-to-know-about-this-upcoming-feature-1cak"
---

지난 주에 CSS Working Group이 스페인에서 모였고, 흥미로운 소식이 있었어요 🥁. 그들은 CSS에 inline if() 함수를 추가하기로 합의했어요.

이 아이디어는 얼마 동안 논의되어 왔지만, 그들은 마침내 빠르게 구현할 수 있는 MVP로 정착했어요. 구현자들과 친근한 대화를 나눈 뒤 탄탄한 제안을 내놓은 결과, 그들은 합의에 도달했어요. CSS에 새로운 멋진 기능들을 추가하여, CSS에 대한 중요한 발전을 이룰 것 같아요 🎉.

<div class="content-ad"></div>

이게 어떻게 작동하는지 보자 (제안에 따라):

```js
.my-element {
    color: if(
        style(--status: active) ? var(--color-active-30) :
        style(--status: error) ? var(--color-error-30) :
        /* 다른 상태를 여기에 추가하세요 */
        var(--color-default-30)
    );
    background-color: if(
        style(--status: active) ? var(--color-active-95) :
        style(--status: error) ? var(--color-error-95) :
        /* 다른 상태를 여기에 추가하세요 */
        var(--color-default-95)
    );
}
```

--status 변수를 기반으로 CSS 속성을 조건부로 변경할 수 있습니다.

미리 정의된 변수를 사용해도 됩니다.

<div class="content-ad"></div>

```js
:root {
    --xl: media(width > 1600px);
    --l: media (width > 1200px);
    --m: media (width > 800px);
}
/* 사용법 */
border-width: if(
    var(--xl) ? var(--size-3) :
    var(--l) or var(--m) ? var(--size-2) :
    var(--size-1)
);
```

### 브라우저 지원:

지금 너무 흥분하지 마세요. 브라우저에 이 기능이 들어가려면 적어도 1-2년이 걸릴 것으로 예상됩니다. CSS의 미래가 밝아보이지만, 우리는 조급해하지 않아도 될 거에요 😀!

앞으로의 CSS 및 다른 웹 기술에 대한 최신 업데이트와 팁을 얻으려면 주시하고, 브라우저에 새로운 기능들이 도착할 때 가장 먼저 알 수 있도록 팔로우해주세요!

<div class="content-ad"></div>

친근한 어조로 번역하면 다음과 같습니다.

Unsplash에서 Cam Adams님의 사진
