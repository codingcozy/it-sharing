---
title: "소셜 미디어에서 웹사이트를 아름답게 만드는 방법"
description: ""
coverImage: "/assets/img/2024-07-09-Yourwebsitedeservestolookgoodonsocialmedias_0.png"
date: 2024-07-09 16:15
ogImage:
  url: /assets/img/2024-07-09-Yourwebsitedeservestolookgoodonsocialmedias_0.png
tag: Tech
originalTitle: "Your website deserves to look good on social medias"
link: "https://dev.to/jospinevans/your-website-deserves-to-look-good-on-social-medias-305e"
---

가끔 궁금해 하셨을 수도 있어요. GitHub이나 Dev와 같은 웹사이트가 어떻게 하면 소셜 미디어나 메시징 앱에서 링크를 공유할 때 이미지와 설명이 나타날 수 있는지요. WhatsApp에서보여지는 것처럼 말이에요.

저는 좋은 소식이 있어요. 그렇게 복잡한 일은 아니에요. 오히려 간단해요 (농담이 아니에요, 진지해요). 그저 오픈 그래프 프로토콜을 구현하기만 하면 돼요. 그래서 소셜 미디어 로봇이 정보를 파싱해서 그렇게 렌더링할 수 있도록 하는 거죠.

빠른 스니펫을 찾고 있다면 여기서 다음을 복사해 가세요.

<div class="content-ad"></div>

```js
<!DOCTYPE html>
<html lang="en">

<head>
    <! Other infos of the head goes here -->
    <! -- Open Graph -->
    <meta property="og:type" content="website">
    <meta property="og:url" content="https://ike-designo-website.vercel.app/">
    <meta property="og:locale" content="en_US">
    <meta property="og:title" content="Designo a digital agency for your digital needs">
    <meta property="og:description" content="With over 10 years in the industry, we are experienced
        in creating fully responsive websites, app design, and engaging brand experiences.
        Find out more about our services.">
    <meta property="og:image" content="https://ike-designo-website.vercel.app/assets/home-og-image.png">
    <meta property="og:image:width" content="1110">
    <meta property="og:image:height" content="610">
    <meta property="og:image:alt" content="the screen shot of the web page at https://ike-designo-website.vercel.app">
</head>

<body>
    <! the body goes here -->
</body>
</html>
```

여기까지 오셨다면, 꼼꼼한 설명을 찾고 있으신 걸로 파악돼요. 보시다시피, 모든 것이 페이지의 head 부분에 메타데이터로 나열돼 있어요. 그러니 단계별로 설명하려고 해 볼게요.

## og:type

이것은 제공하는 콘텐츠의 유형을 나타내요. 우리의 경우에는 웹사이트예요.

<div class="content-ad"></div>

## og:url

이것은 웹사이트의 URL입니다. 로봇이 당신의 웹사이트를 식별하는 데 사용됩니다.

## og:locale

이 속성은 국제화를 위한 웹사이트의 기본 언어에 대한 정보를 제공하는 데 사용됩니다.

<div class="content-ad"></div>

## og:title

방문자가 볼 수있는 웹 사이트의 제목입니다.

## og:description

콘텐츠를 간략히 설명하는 페이지에 대한 짧은 설명입니다 (잠재적인 방문자를 혼란스럽게 하지 않기 위해 최대 2 문장으로 작성하는 것이 중요합니다).

<div class="content-ad"></div>

## og:image

당신의 웹사이트에 쇼케이스하고 싶은 이미지를 나타내는 URL입니다. (이 부분은 선택하기가 어려울 수 있지만, 어떤 것을 선택할지 모르겠다면 페이지의 스크린샷을 찍는 것이 좋습니다)

## og:image:alt

이미지에 나타나는 내용에 대한 설명입니다. 시각적으로 정보를 얻을 수 없거나 이미지를 보는 데 어려움을 겪는 사람들에게 정보를 제공하기 때문에 매우 중요합니다. 이미지를 제공하려면 이 내용을 절대 잊지 말아야 합니다.

<div class="content-ad"></div>

## og:image:width, og:image:height

이미지의 픽셀 수폭과 높이 정보를 제공하십시오. 이 정보는 선택 사항이지만 제공하는 것이 좋습니다.

모든 이 데이터는 다음과 같은 결과를 생성합니다(최근 작업한 사이트 프로젝트입니다)

![이미지](/assets/img/2024-07-09-Yourwebsitedeservestolookgoodonsocialmedias_1.png)

<div class="content-ad"></div>

요약하면, 오픈그래프 프로토콜 덕분에 소셜 미디어에서 링크를 공유할 때 콘텐츠 개요를 제공할 수 있습니다. 이제 여러분의 웹사이트를 소셜 미디어에서 멋지게 보이도록 만드는 차례입니다.

읽어 주셔서 감사합니다. 피드백은 언제든 환영합니다.

더 읽어보기

- 오픈그래프 프로토콜 문서
