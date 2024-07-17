---
title: "HTML과 CSS로 프로필 카드 UI 만들기 - Flexbox 사용 방법"
description: ""
coverImage: "/assets/img/2024-07-06-ProfileCardUIusingHtmlCss-flexbox_0.png"
date: 2024-07-06 01:54
ogImage:
  url: /assets/img/2024-07-06-ProfileCardUIusingHtmlCss-flexbox_0.png
tag: Tech
originalTitle: "Profile Card UI using Html , Css - flexbox"
link: "https://dev.to/syedmuhammadaliraza/pro-file-card-ui-using-html-css-flexbox-518f"
---

## 프로필 CARD UI

<img src="/assets/img/2024-07-06-ProfileCardUIusingHtmlCss-flexbox_0.png" />

### HTML

```js
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="stylesheet" href="style.css" />
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css">
    <title>프로필 카드</title>
  </head>
  <body>
    <div class="parent">
      <div class="card">
        <div class="card-header">
            <img class="pro-image"  src="image.jpg" alt="프로필 이미지">
        </div>
        <div class="card-body">
          <div class="profile ">
            <h2>시드 무함마드 알리 라자</h2>
            <h6>@syed-muhammad-ali-raza</h6>
          </div>
          <div class="social-link">
            <a href="#" class="fa fa-facebook"></a>
            <a href="#" class="fa fa-twitter"></a>
            <a href="#" class="fa fa-google"></a>
            <a href="#" class="fa fa-linkedin"></a>
            <a href="#" class="fa fa-pinterest"></a>
          </div>
          <div class="description">
            JavaScript | React js | Next.js Frontend Engineer | Software Engineer | Software Developer | Web3.0 | API Integration | T-Shaped Engineer | MUI | Tailwind | SDE | Microfrontend
          </div>
          <div class="buttons">
            <button>팔로우</button>
            <button>메시지</button>
          </div>
        </div>
      </div>
    </div>
  </body>
</html>
```

<div class="content-ad"></div>

### CSS

```js
.fa {
    padding: 10px;
    font-size: 20px;
    width: 20px;
    text-align: center;
    text-decoration: none;
    margin: 5px 1px;
    border-radius: 50%;
    color: white;
}

.fa-facebook { background: #3B5998; }
.fa-twitter { background: #55ACEE; }
.fa-google { background: #dd4b39; }
.fa-linkedin { background: #007bb5; }
.fa-pinterest { background: #cb2027; }

.card-header {
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 20px;
}

.card {
    background-color: black;
    color: white;
    border-radius: 10px;
    overflow: hidden;
    width: 25%;
    margin: auto;
}

.pro-image {
    width: 130px;
    height: 130px;
    border-radius: 50%;
}

.parent {
    background-color: white;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
}

.buttons {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-top: 20px;
}

button {
    color: black;
    background-color: white;
    padding: 10px;
    border-radius: 20px;
    border: none;
    cursor: pointer;
    font-size: 16px;
    margin-left: 20px;
}

.card-body {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 20px;
}

.description {
    margin-top: 20px;
    text-align: center;
}

.profile h2,
.profile h6 {
    margin: 5px 0;
}

.social-link {
    display: flex;
    justify-content: center;
    margin-top: 20px;
}

.profile {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}
```

Here is the GitHub Repo Link [GitHub Repo](GitHub Repo 링크)
