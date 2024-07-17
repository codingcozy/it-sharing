---
title: "웹 개발 여정 첫날 HTML  배우기"
description: ""
coverImage: "/assets/img/2024-07-06-MyWebDevelopmentJourneyDay-1HTML_0.png"
date: 2024-07-06 01:54
ogImage:
  url: /assets/img/2024-07-06-MyWebDevelopmentJourneyDay-1HTML_0.png
tag: Tech
originalTitle: "My Web Development Journey Day-1: HTML 🚀"
link: "https://dev.to/shemanto_sharkar/my-web-development-journey-day-1-html-364b"
---

저번 달, Jhankar Mahbub bhai - Programming Hero로 웹 개발 강좌에 등록하기로 결심했어요. Programming Hero의 강력한 지원 시스템을 이유로 선택했고, 강좌를 마친 후 취업/인턴십을 얻는 것이 목표로 합니다.

내년 (2025년) 대학을 졸업할 예정이라, 그 이후에 실업 상태가 되고 싶지 않아요. 그래서 2024년 말까지 웹 개발자가 되는 미션을 수행하고 있어요.

#### Module 1: Learn and Explore HTML 📚

배운 주제:

<div class="content-ad"></div>

- HTML 구조 (제목, 파비콘)
- HTML 태그: 단락, 제목, 버튼, 줄 바꿈
- 태그 속성, 요소
- 텍스트 형식 지정 (small, strong, bold, italic, emphasize)
- 블록 vs 인라인
- 목록 (순서가 있는 목록, 순서가 없는 목록)
- 순서가 있는 목록 유형
- 순서가 없는 목록 유형
- 앵커 태그 (새 탭에서 열기, 다운로드)
- 이미지

이것은 연습용으로 작성한 HTML입니다:

" ` ` ` js

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Travel Blog</title>
    <link rel="icon" href="\travel.png">
</head>
<body>
    <div>
        <!-- 이것은 환영 섹션입니다 -->
        <h1>Welcome To My Blog!</h1>
        <h2>I am Shemanto, <span style="color: #e0a82e; font-weight: bold;">THE TRAVELER</span></h2>
    </div>

    <div>
        <!-- 내가 여행한 장소들 -->
        <h3>These are the places I traveled so far:</h3>
        <ol type="i">
            <li>Sundorban</li>
            <li>Rajshahi</li>
            <li>Chittagong</li>
            <li>Rongpur</li>
            <li>Mymensingh</li>
        </ol>
    </div>

    <div>
        <!-- 좋아하는 음식 목록 -->
         <h3>My favorite food lists</h3>
         <ul type="square">
            <li>Rice & Egg vaji with hot lantil</li>
            <li>Mutton Biriani</li>
            <li>Guava Fruit</li>
            <li>Pocket Gate er Khichuri & Dim vaji with Tomato vorta</li>
         </ul>
    </div>

    <div>
        <!-- 음식 이미지들 -->
         <h3>This are the images of Foods that I liked so far</h3>
         <span><img src="https://images.immediate.co.uk/production/volatile/sites/30/2020/08/chorizo-mozarella-gnocchi-bake-cropped-9ab73a3.jpg?quality=90&resize=556,505" alt="torkari" width="300" height="300"></span>
         <span><img src="https://loveincorporated.blob.core.windows.net/contentimages/gallery/d99329d6-ce59-4e2d-9513-96a879c7a4ae-gnocchibake.jpg" alt="sukna torkari" width="300" height="300"></span>
         <span><img src="https://media.self.com/photos/5acf7250164ba7734f61bca2/4:3/w_320%2Cc_limit/DINNER_09_BAKEDEGGS_071.jpg" alt="tomato torkari" width="300" height="300"></span>
         <span><img src="https://images.immediate.co.uk/production/volatile/sites/30/2022/08/Corndogs-7832ef6.jpg?quality=90&resize=556,505" alt="fastfood" width="300" height="300"></span>
    </div>

    <div>
        <button> <a href="https://www.linkedin.com/in/shemanto/" target="_blank">My LinkedIn Account</a> </button>
        <button><a href="\travel.png" download="download image">Download FabIcon</a></button>
    </div>

</body>
</html>
" ` ` `

![](/assets/img/2024-07-06-MyWebDevelopmentJourneyDay-1HTML_0.png)
