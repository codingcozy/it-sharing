---
title: "스타일링을 더욱 발전시키는 방법"
description: ""
coverImage: "/it-shar.ing/assets/no-image.jpg"
date: 2024-07-09 12:51
ogImage:
  url: /it-shar.ing/assets/no-image.jpg
tag: Tech
originalTitle: "Going Further with Styling"
link: "https://dev.to/nmiller15/going-further-with-styling-1dnp"
---

안녕하세요! 다시 한번 Learn As You Code: HTML & CSS에 오신 것을 환영합니다! 오늘은 스타일링의 세계로 더 깊게 들어가보려고 해요. 지금까지는 요소를 직접 스타일링했었죠. 하지만 만약에 `h2` 요소가 두 개 있고 각각을 다르게 보이게 하고 싶다면 어떻게 할까요? 바로 CSS 셀렉터가 등장합니다!

## 요소 셀렉터

이미 친숙할지도 모르겠지만, 간단하게 복습해보죠:

```js
h1 {
  font-size: 32px;
  font-family: Arial;
  font-weight: 500;
}
```

<div class="content-ad"></div>

이 규칙 집합은 모든 `h1` 요소를 대상으로 하며, 그들의 글꼴 크기, 글꼴 종류 및 굵기를 설정합니다. 요소 선택기는 페이지 전반에 대한 스타일 가이드를 설정하는 것과 같이 일반적인 스트로크를 위해 좋습니다. 그렇지만 모든 `p` 태그가 동일하게 보이면 안 되는 것도 사실입니다. 더 구체적인 스타일링을 위해 우리는 더 높은 수준으로 올라가야 할 필요가 있습니다!

## 클래스 선택기

클래스가 구원을 줍니다! 두 개의 `p` 태그를 다르게 보이도록 하고 싶나요? 클래스를 추가하세요:

```js
<p class="big red">이 텍스트는 크고 빨간색입니다.</p>
<p class="small blue">이 텍스트는 작고 파란색입니다.</p>
```

<div class="content-ad"></div>

각 `p` 태그는 두 개의 클래스가 있습니다. CSS에서 이러한 클래스를 선택하려면 `.`을 사용하세요:

```js
.big {
  font-size: 100px;
}

.small {
  font-size: 9px;
}

.red {
  color: red;
}

.blue {
  color: blue;
}
```

야호! 스타일이 적용되었어요. "스타일을 더 적은 클래스로 결합하는 게 더 좋지 않을까?"라고 물을 수도 있어요. 좋은 질문이에요! 저는 클래스를 유연하게 유지하는 걸 좋아해요. small을 blue 없이 재사용하고 싶을 때가 언제일지 모르니까요.

## 아이디 선택자

<div class="content-ad"></div>

고유 요소에는 ID를 사용하세요. 아래 내용을 확인해보세요:

```js
<p id="name">제 이름은 Nolan입니다!</p>
```

ID는 한 페이지당 한 번만 사용하세요. CSS에서 이를 대상으로 할 때는 #을 사용하세요:

```js
#name {
  text-decoration: underline;
}
```

<div class="content-ad"></div>

간단하죠?

## 충돌하는 스타일

그럼 클래스와 ID 둘 다 가지고 있는 요소가 있다면 어떨까요? 이렇게요:

```js
<p id="red" class="blue">
  나는 빨간색인가 파란색인가요?
</p>
```

<div class="content-ad"></div>

빨간색으로 바뀌게 됩니다! 왜냐하면 ID가 클래스보다 특정성이 높기 때문이죠. 간단한 예제를 한 번 보여 드리겠습니다:

```js
<p id="red" class="underline">
  세 개의 규칙 세트로 스타일이 적용된 저에요!
</p>
```

```js
p {
  font-size: 12px;
  color: black;
  text-decoration: none;
}

.underline {
  text-decoration: underline;
}

#red {
  color: red;
}
```

이 텍스트가 빨간색으로 바뀌고 밑줄이 그어지며, 12px의 글꼴 크기로 나타날 거에요. ID는 클래스를 무시하고, 이는 엘리먼트 선택기를 무시합니다. 이 스타일 계층구조를 통해 반복 코드 없이도 당신의 페이지가 세련되게 보이게 만들 수 있습니다.

<div class="content-ad"></div>

## 도전 과제

About Me 페이지를 레벨 업할 시간입니다! 이제 미션을 수행해 보세요:

- element 선택자를 사용하여 `h1`, `h2`, 및 `p`에 대한 기본 스타일을 설정합니다.
- 이름 아래에 `p` 태그로 태그 라인을 추가하고 ID를 사용하여 스타일을 지정합니다.
- 클래스 선택자를 사용하여 다른 텍스트를 러프하게 꾸며봅니다.

충돌하는 스타일로 놀아보고 어떤 규칙이 우선되는지 확인하세요. 그 이유를 알아낼 수 있을까요?

<div class="content-ad"></div>

읽어주셔서 감사합니다! 이 시리즈에서 다루고 싶은 다른 주제가 있다면 댓글로 알려주세요. 시리즈를 어떻게 즐기고 계시는지 알려주셔도 좋아요!
