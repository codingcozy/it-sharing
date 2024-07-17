---
title: "HTML, CSS, JavaScript로 반응형 카드 슬라이더 만드는 방법"
description: ""
coverImage: "/assets/img/2024-07-07-HowtoCreateResponsiveCardSliderinHTMLCSSJavaScript_0.png"
date: 2024-07-07 18:49
ogImage:
  url: /assets/img/2024-07-07-HowtoCreateResponsiveCardSliderinHTMLCSSJavaScript_0.png
tag: Tech
originalTitle: "How to Create Responsive Card Slider in HTML CSS , JavaScript"
link: "https://dev.to/codingnepal/how-to-create-responsive-card-slider-in-html-css-javascript-4f4a"
---

<img src="/assets/img/2024-07-07-HowtoCreateResponsiveCardSliderinHTMLCSSJavaScript_0.png" />

웹 사이트에서 카드 또는 이미지 슬라이더를 보신 적이 있을 것입니다. 그런데 여러분은 직접 만들어 보는 것에 대해 생각해 보신 적이 있나요? SwiperJS를 사용하면 간단하게 모던하고 터치에 반응하는 반응형 슬라이더를 만들 수 있습니다.

이 블로그 글에서 HTML, CSS 및 JavaScript(SwiperJS)를 사용하여 반응형 카드 슬라이더를 만드는 방법을 안내하고, Glassmorphism 효과로 매력적으로 만드는 방법을 알려 드리겠습니다. 글을 마치면 시각적으로 매력적이고 상호 작용이 가능한 슬라이더를 웹 사이트 프로젝트나 포트폴리오에 사용할 수 있을 것입니다.

SwiperJS를 사용하지 않고 빠닐라 JavaScript로 슬라이더를 만들고 싶다면, HTML CSS & JavaScript에서 반응형 이미지 슬라이더에 대한 이 블로그 글을 확인해 보세요. 빠닐라 JavaScript로 슬라이더를 만드는 것은 슬라이더의 기본 메커니즘을 이해하고 JavaScript 기술을 향상시킬 수 있습니다.

<div class="content-ad"></div>

## HTML CSS & JavaScript로 반응형 카드 슬라이더의 비디오 튜토리얼

만약 비디오 튜토리얼을 선호하신다면, 위의 YouTube 비디오는 훌륭한 자원입니다. 각 코드 라인을 설명하고 프로젝트를 만드는 과정을 쉽게 이해할 수 있도록 주석을 제공합니다. 만약 읽는 것을 선호하시거나 단계별 안내가 필요하다면, 계속해서 이 포스트를 따라주세요.

## HTML 및 JavaScript로 반응형 카드 슬라이더 생성하는 단계

HTML, CSS 및 JavaScript (SwiperJS)를 사용하여 반응형 카드 슬라이더를 만들려면 다음과 같은 간단한 단계별 지침에 따라 진행하세요:

<div class="content-ad"></div>

- 마음에 드는 이름의 폴더를 만들어주세요. 예를 들어, card-slider와 같이.
- 폴더 안에 index.html, style.css, 그리고 script.js와 같은 필수 파일을 생성해주세요.
- 이미지 폴더를 다운로드하여 프로젝트 디렉토리에 넣어주세요. 해당 폴더에는 카드 슬라이더에 필요한 모든 이미지가 포함되어 있습니다. 물론 다른 이미지를 사용해도 괜찮습니다.

index.html 파일에는 다양한 의미있는 태그와 SwiperJS CDN 링크를 추가하여 카드 슬라이더 레이아웃을 만들어주세요.

```js
<!DOCTYPE html>
<!-- Coding By CodingNepal - www.codingnepalweb.com -->
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Card Slider HTML and CSS | CodingNepal</title>
  <!-- Linking SwiperJS CSS -->
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/swiper@11/swiper-bundle.min.css">
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div class="container swiper">
    <div class="slider-wrapper">
      <div class="card-list swiper-wrapper">
        <div class="card-item swiper-slide">
          <img src="images/img-1.jpg" alt="User Image" class="user-image">
          <h2 class="user-name">James Wilson</h2>
          <p class="user-profession">Software Developer</p>
          <button class="message-button">Message</button>
        </div>

        <div class="card-item swiper-slide">
          <img src="images/img-2.jpg" alt="User Image" class="user-image">
          <h2 class="user-name">Sarah Johnson</h2>
          <p class="user-profession">Graphic Designer</p>
          <button class="message-button">Message</button>
        </div>

        <div class="card-item swiper-slide">
          <img src="images/img-3.jpg" alt="User Image" class="user-image">
          <h2 class="user-name">Michael Brown</h2>
          <p class="user-profession">Project Manager</p>
          <button class="message-button">Message</button>
        </div>

        <div class="card-item swiper-slide">
          <img src="images/img-4.jpg" alt="User Image" class="user-image">
          <h2 class="user-name">Emily Davis</h2>
          <p class="user-profession">Marketing Specialist</p>
          <button class="message-button">Message</button>
        </div>

        <div class="card-item swiper-slide">
          <img src="images/img-5.jpg" alt="User Image" class="user-image">
          <h2 class="user-name">Christopher Garcia</h2>
          <p class="user-profession">Data Scientist</p>
          <button class="message-button">Message</button>
        </div>

        <div class="card-item swiper-slide">
          <img src="images/img-6.jpg" alt="User Image" class="user-image">
          <h2 class="user-name">Richard Wilson</h2>
          <p class="user-profession">Product Designer</p>
          <button class="message-button">Message</button>
        </div>
      </div>

      <div class="swiper-pagination"></div>
      <div class="swiper-slide-button swiper-button-prev"></div>
      <div class="swiper-slide-button swiper-button-next"></div>
    </div>
  </div>

  <!-- Linking SwiperJS script -->
  <script src="https://cdn.jsdelivr.net/npm/swiper@11/swiper-bundle.min.js"></script>

  <!-- Linking custom script -->
  <script src="script.js"></script>
</body>
</html>
```

style.css 파일에는 카드 슬라이더를 스타일링하여 세련되고 현대적인 글래스모피즘 효과를 주세요. 색상, 글꼴, 배경 등과 같은 다양한 CSS 속성을 실험하여 슬라이더를 더욱 매력적으로 만들어보세요.

<div class="content-ad"></div>

```js
/* 구글 폰트 Montserrat을 가져오기 */
@import url('https://fonts.googleapis.com/css2?family=Montserrat:ital,wght@0,100..900;1,100..900&display=swap');

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Montserrat", sans-serif;
}

body {
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  background: url("images/bg.jpg") #030728 no-repeat center;
}

.slider-wrapper {
  overflow: hidden;
  max-width: 1200px;
  margin: 0 70px 55px;
}

.card-list .card-item {
  height: auto;
  color: #fff;
  user-select: none;
  padding: 35px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  border-radius: 10px;
  backdrop-filter: blur(30px);
  background: rgba(255, 255, 255, 0.2);
  border: 1px solid rgba(255, 255, 255, 0.5);
}

.card-list .card-item .user-image {
  width: 150px;
  height: 150px;
  border-radius: 50%;
  margin-bottom: 40px;
  border: 3px solid #fff;
  padding: 4px;
}

.card-list .card-item .user-profession {
  font-size: 1.15rem;
  color: #e3e3e3;
  font-weight: 500;
  margin: 14px 0 40px;
}

.card-list .card-item .message-button {
  font-size: 1.25rem;
  padding: 10px 35px;
  color: #030728;
  border-radius: 6px;
  font-weight: 500;
  cursor: pointer;
  background: #fff;
  border: 1px solid transparent;
  transition: 0.2s ease;
}

.card-list .card-item .message-button:hover {
  background: rgba(255, 255, 255, 0.1);
  border: 1px solid #fff;
  color: #fff;
}

.slider-wrapper .swiper-pagination-bullet {
  background: #fff;
  height: 13px;
  width: 13px;
  opacity: 0.5;
}

.slider-wrapper .swiper-pagination-bullet-active {
  opacity: 1;
}

.slider-wrapper .swiper-slide-button {
  color: #fff;
  margin-top: -55px;
  transition: 0.2s ease;
}

.slider-wrapper .swiper-slide-button:hover {
  color: #4658ff;
}

@media (max-width: 768px) {
  .slider-wrapper {
    margin: 0 10px 40px;
  }

  .slider-wrapper .swiper-slide-button {
    display: none;
  }
}
```

`script.js` 파일에 SwiperJS를 초기화하고 카드 슬라이더를 작동할 수 있도록 JavaScript 코드를 추가하세요.

```js
const swiper = new Swiper(".slider-wrapper", {
  loop: true,
  grabCursor: true,
  spaceBetween: 30,

  // 페이지네이션
  pagination: {
    el: ".swiper-pagination",
    clickable: true,
    dynamicBullets: true,
  },

  // 네비게이션 화살표
  navigation: {
    nextEl: ".swiper-button-next",
    prevEl: ".swiper-button-prev",
  },

  // 반응형 브레이크포인트
  breakpoints: {
    0: {
      slidesPerView: 1,
    },
    768: {
      slidesPerView: 2,
    },
    1024: {
      slidesPerView: 3,
    },
  },
});
```

이제 코드를 올바르게 추가했다면, 카드 슬라이더를 확인할 준비가 되었습니다. 브라우저에서 `index.html` 파일을 열어 슬라이더를 확인해보세요!

<div class="content-ad"></div>

## 결론과 마지막으로

위 단계를 따라하셔서 HTML, CSS 및 JavaScript(SwiperJS)를 사용하여 반응형 카드 슬라이더를 만드셨습니다. 이 슬라이더 프로젝트를 통해 웹 개발의 기본을 이해할 뿐만 아니라 라이브러리를 활용하여 쉽게 상호작용하고 유용한 웹 구성 요소를 만드는 능력을 보여줍니다.

슬라이더를 자유롭게 사용자 정의하고 다양한 설정을 실험하여 여러분만의 것으로 만드세요. 추가적인 사용자 정의 세부 정보는 SwiperJS 문서를 확인하시기 바랍니다.

더 많은 영감을 얻으시려면, 이미지 슬라이더 10개 이상 소스 코드 블로그 포스트를 확인해보세요. 이 슬라이더 중 일부는 SwiperJS를 활용하고, 어떤 것은 Owl Carousel을 사용하며, 다른 것들은 바닐라 JavaScript로 만들어져 다양한 학습 예제를 제공합니다.

<div class="content-ad"></div>

만약 슬라이더를 만드는 동안 문제가 발생하면, "다운로드" 버튼을 클릭하여 이 프로젝트의 소스 코드 파일을 무료로 다운로드할 수 있습니다. 또한 "실시간 미리보기" 버튼을 클릭하여 실시간 데모를 확인할 수도 있습니다.

실시간 데모 보기

코드 파일 다운로드
