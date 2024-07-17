---
title: "CSS Translate 속성을 사용한 동적 검색 창 만들기 방법"
description: ""
coverImage: "/it-shar.ing/assets/no-image.jpg"
date: 2024-07-07 18:52
ogImage:
  url: /it-shar.ing/assets/no-image.jpg
tag: Tech
originalTitle: "Creating dynamic Search Bar using CSS Translate property"
link: "https://dev.to/code_passion/creating-dynamic-search-bar-using-css-translate-property-59a5"
---

CSS의 변환 속성은 요소를 X 및 Y 축을 따라 이동시킵니다. 위치 속성과 같은 다른 위치 지정 속성과 달리 변환은 문서의 자연스러운 흐름을 방해하지 않으므로 부드러운 애니메이션과 전환을 만드는 데 우수합니다.

시각적으로 매력적이고 동적인 사용자 인터페이스는 끊임없이 변화하는 웹 개발 환경에서 표준이 되었습니다. CSS (계층형 스타일 시트)는 개발자들에게 제공되는 많은 도구 중에서도 두드러진 스타일 및 레이아웃 기술로 핵심 기술입니다. CSS의 변환 속성은 웹 페이지의 항목 표시를 제어하는 강력한 기술로 나타납니다.

구문:

```js
선택기 {
    transform: 변환-함수;
}
```

<div class="content-ad"></div>

선택자(selector)라는 용어는 변환을 적용할 요소를 가리키며, 변환 함수(transform-function)는 수행할 변환 유형을 설명합니다.

**가장 일반적으로 사용되는 Translate 함수를 살펴보겠습니다**

CSS Translate 속성을 사용하여 동적 검색 바 생성하기:

폼은 웹에서 사용자 참여에 필수적입니다. 폼은 가장 좋아하는 웹사이트에 로그인하거나 구독을 신청하거나 구매를 완료하는 등 참여의 문을 열어줍니다. 그러나 기능적이고 미적으로 아름다운 폼을 작성하는 것은 어려울 수 있습니다. 이때 .form-group와 같은 CSS 클래스가 유용하게 사용됩니다. 이는 폼 디자인에 조직적인 접근을 제공합니다. (예제 더 읽기)

<div class="content-ad"></div>

출력:

<img src="https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fzukjudbaq6lwqduj3m9z.gif" />

HTML:

```js
<!DOCTYPE html>
<html>
<head>
    <meta charset='utf-8'>
    <meta http-equiv='X-UA-Compatible' content='IE=edge'>
    <title>페이지 제목</title>
    <meta name='viewport' content='width=device-width, initial-scale=1'>
</head>
<body>
    <div class="form-group">
        <input type="text" placeholder=" " id="name" required>
        <label for="name">당신의 이름</label>
    </div>
</body>
</html>
```

<div class="content-ad"></div>

CSS:

```js
.form-group {
    position: relative;
    margin-bottom: 15px;
    max-width: 900px;
    margin-top: 50px;
}
.form-group input {
    width: 100%;
    padding: 10px;
    font-size: 16px;
    border: 1px solid #ccc;
    border-radius: 5px;
    outline: none;
    border-radius: 20px;
}
.form-group label {
    position: absolute;
    top: 10px;
    left: 10px;
    font-size: 17px;
    color: #999;
    pointer-events: none;
    transition: all 0.3s;
}
.form-group input:focus + label,
.form-group input:not(:placeholder-shown) + label {
    top: -20px;
    left: 10px;
    font-size: 16px;
    color: #ee3646;
    transform: translateY(-10px);
}
```

- **.form-group** 클래스는 우리의 마법 같은 향상을 위한 기초 역할을 합니다. 이는 양식 요소들을 잘 구성되고 시각적으로 일관된 상태로 유지하는 구조적 레이아웃을 만듭니다. 우리의 양식 그룹은 position: relative, margin 및 max-width와 같은 속성을 사용하여 우아하게 배치되고 스타일이 적용됩니다.
- **.form-group input** 및 **.form-group label** 선택자는 마법이 일어나는 곳입니다. 이러한 선택자는 입력 필드와 라벨에 특정한 스타일을 적용하여 상호작용 가능한 요소로 만듭니다.
- **.form-group**의 입력 필드는 컨테이너의 전체 너비로 늘어나는 스타일을 상속합니다. 편의를 위한 패딩과 범위 설정을 위한 약간의 테두리가 포함되어 있습니다. outline: none 선언은 깔끔한 디자인을 유지하며, border-radius는 둥근 테두리로 아름다움을 더합니다.

**결론**
웹 개발의 변화무쌍한 세계에서, 변환과 같은 CSS 속성을 습득하는 것은 매력적이고 유연한 사용자 경험을 개발하는 데 중요합니다.
