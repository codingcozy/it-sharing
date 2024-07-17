---
title: "모든 개발자가 알아야 할 12가지 쉬운 SEO 팁"
description: ""
coverImage: "/assets/img/2024-07-06-12easySEOTipsEveryDeveloperShouldKnow_0.png"
date: 2024-07-06 01:50
ogImage:
  url: /assets/img/2024-07-06-12easySEOTipsEveryDeveloperShouldKnow_0.png
tag: Tech
originalTitle: "12 easy SEO Tips Every Developer Should Know"
link: "https://dev.to/acidop/12-easy-seo-tips-every-developer-should-know-52k3"
---

좋은 UI/UX를 갖춘 웹 사이트를 만드는 것은 시간과 좋은 커피만 있다면 쉬운 일이죠.

하지만 방문자와 사용자가 없는 웹 사이트는 무슨 의미가 있을까요?

다음 웹 사이트를 만들 때 주의해야 할 12가지 쉬운 팁을 소개합니다. 더 많은 유기적 방문자를 얻어보세요.

## 1. URL 구조

<div class="content-ad"></div>

URL을 짧고 간결하게 유지하세요. 이해하기 쉬운 URL이 목표입니다. 적은 단어로 많은 정보를 전달하는 게 중요해요. 메인 키워드를 URL에 포함해야 합니다.

예를 들면:
nextjs-tutorial vs superfine_nextjs-tutorial_aimed-at_100%-beginners

위의 두 URL 중 어떤 것이 SEO 측면에서 더 나은 것 같나요?

### 하이픈 vs. 언더스코어 사용하기

<div class="content-ad"></div>

특수 문자인 언더스코어(\_)를 사용하는 URL은 검색 엔진을 혼란스럽게 할 수 있습니다. 밑줄 대신 하이픈을 사용하는 것이 더 좋습니다.

URL의 가독성을 높이고 각 웹 사이트의 본래 목적을 검색 엔진이 이해할 수 있도록 단어들을 나누는 데 하이픈을 사용하는 것이 좋습니다.

![image](/assets/img/2024-07-06-12easySEOTipsEveryDeveloperShouldKnow_0.png)

## 2. 타이틀 태그 및 메타 설명

<div class="content-ad"></div>

여기 제목 태그가 어떻게 보이는지에요:

```js
<head>
  <title>AcidOP | 프리랜싱 개발자</title>
</head>
```

여기서의 목표는 가능한 많은 정보를 가능한 적은 양으로 전달하는 것이에요. 글자 수에는 제한이 없지만 60자 미만으로 작성해야 해요.

제목은 웹사이트나 글에 대한 당신의 특정 분야 또는 제품을 설명해야 해요.

<div class="content-ad"></div>

### 매력적인 메타 설명 작성하기

다음은 설명 태그의 예시입니다:

```js
<head>
  <meta name="description" content="인도 출신 훌륭한 개발자입니다." />
</head>
```

웹 사이트로 방문자를 유도하는 가장 중요한 요소 중 하나가 메타 설명 태그입니다.

<div class="content-ad"></div>

메타 설명은 페이지의 대상 키워드를 능숙하게 포함하여 독자들이 페이지로 이동하도록 설득해야 합니다. 구글과 같은 검색 엔진은 설명을 표시할 때 쿼리에서 용어를 강조하여 사용자의 관심을 끌곤 합니다. 설명을 높은 가치의 검색어와 근접하게 맞추는 것이 중요하지만 지나치게 맞추는 것은 좋지 않습니다.

아래는 제목과 설명 태그가 작동하는 예시입니다:

![2024-07-06-12easySEOTipsEveryDeveloperShouldKnow_1.png](/assets/img/2024-07-06-12easySEOTipsEveryDeveloperShouldKnow_1.png)

<div class="content-ad"></div>

## 3. 헤더 태그 (H1, H2, H3 등)

헤더 태그는 사용자가 페이지를 탐색하는 데 도움을 줍니다. 콘텐츠를 분리하고 페이지를 전용 섹션으로 나눕니다.

검색 엔진은 페이지의 섹션을 더 잘 이해하기 위해 헤더를 활용하며, 이는 SEO에 도움이 됩니다.

몇 단락으로 이루어진 아주 긴 글을 쓰는 대신 여러 부분으로 나누어 각각 제목을 붙여 기사를 구성하는 것이 좋습니다. 이 블로그에서 한 것처럼요.

<div class="content-ad"></div>

### 헤더 태그를 사용하여 콘텐츠 구조화하기

문서의 주요 제목은 `h1`이어야 합니다. H1 태그는 문서 전체 주제를 나타내며 큰 텍스트로 표시됩니다.

주요 포인트는 `h2`로 감싸야 합니다. H2 태그는 콘텐츠를 분할하여 쉽게 스캔하고 읽기 쉽도록 만들기 위해 사용되는 두 번째 수준의 제목입니다.

그 다음의 헤딩 태그는 H6까지 이어지며, 헤딩 계층 구조에서 그다지 중요하지 않습니다.

<div class="content-ad"></div>

참고:

- 모든 태그는 계층적이어야 합니다. 즉, H1`H2`H3...H6.
- 전체 문서에서 H1 태그는 하나만 있어야 합니다.

예시:

나의 블로그 마크다운 편집기에서, 제가 제목을 아래 순서대로 배열합니다.

<div class="content-ad"></div>

# Next.js와 TailwindCss를 사용하여 Markdown 편집기를 만들었어요! 🔥

## 목표

### 1. 랜딩 페이지 생성

### 2. 데이터를 저장할 상태 추가

### 3. react-markdown 및 @tailwindcss/typography 설정

### 4. 코드 하이라이팅 및 사용자 정의 컴포넌트

#### 선택 사항

### 5. Rehype 및 Remark 플러그인 추가

### 6. Markdown 버튼이 있는 헤더

## 4. SEO를 위한 이미지 최적화

Optinmonster에 따르면, 이미지가 포함된 콘텐츠는 최대 94% 더 많은 조회수를 받을 수 있어요.

시각적인 이미지는 사람들을 자연스럽게 매혹시키는데, 만족스러운 이미지보다 더 효과적인 것은 없어요.

<div class="content-ad"></div>

이는 독자들을 확보하고 가시성을 높이기 위해 콘텐츠에 시각 자료를 활용해야 한다는 것을 의미합니다.

### 설명적인 대체 텍스트 사용

예시:

```js
<img src="https://acidop.codes/og.jpg" alt="내 웹사이트 배너" />
```

<div class="content-ad"></div>

알트 태그(기술적으로 태그가 아니라 속성이지만, 걱정하지 않으셔도 됩니다)는 몇 가지 중요한 이유로 중요합니다:

- 향상된 접근성: 시각 장애가 있는 사람들이 이미지를 이해하는 데 도움이 됩니다.
- SEO 향상: 이미지에 대한 가치 있는 정보를 검색 엔진에 제공하여 검색 순위에서 더 잘 보이도록 합니다.
- 사용자 경험 향상: 이미지에 대한 정보를 전달하므로 이미지가 로드되지 않아도 알 수 있습니다.

### 적절한 이미지 형식 선택

- JPEG는 스크린샷, 블로그 포스트 이미지 및 사이트 속도가 중요한 컨텐츠에 적합합니다.
- PNG는 품질과 해상도로 인해 더 좋지만 보통 파일 크기가 크기 때문에 페이지 로드 시간이 더 오래 걸릴 수 있습니다.
- WebP는 이미지 품질을 많이 손상시키지 않으면서 다른 것들보다 우수한 압축 기능을 제공합니다. 웹 페이지의 로드 시간을 향상시키고 대역폭을 최소화합니다. GIF의 애니메이션 기능과 PNG의 투명 배경을 모두 지원합니다.
- SVG는 아이콘 및 로고와 관련하여 가장 적합합니다. 해상도를 잃지 않고 어떤 크기로든 확대할 수 있습니다.

<div class="content-ad"></div>

### 반응형 이미지 구현하기 (srcset)

당신의 웹사이트는 큰 그래픽으로 인해 성능이 느릴 수 있습니다. 우리는 이 과정을 더 빠르게 만들고 파일 크기를 줄일 수 있습니다. 하지만 모바일 기기에서의 시각적 품질은 데스크톱 컴퓨터에서와 동일합니다.

srcset을 사용하면 브라우저가 실제 기기의 해상도에 기반하여 가장 적합한 이미지를 선택할 수 있는 이미지 리소스 목록을 정의할 수 있습니다.

예시:

<div class="content-ad"></div>

<img
  src="image.jpg"
  srcset="small.jpg 300w, medium.jpg 600w, large.jpg 900w"
/>

## 5. 모바일 친화성

온라인 검색에서 스마트폰 및 태블릿과 같은 모바일 기기 사용이 증가함에 따라, Google의 웹 크롤러는 이제 웹 사이트 콘텐츠의 모바일 버전을 색인화하는 것을 우선시합니다.

### 반응형 디자인 보장하기

<div class="content-ad"></div>

웹 사이트는 화면 크기에 따라 콘텐츠가 어떻게 보이는지 조정할 수 있지만, 사용하는 장치(컴퓨터, 태블릿, 휴대폰 또는 이미지를 표시하지 않는 브라우저)에 관계없이 동일한 URL에서 정확히 동일한 HTML 코드를 보여줍니다.

Google은 반응형 웹 디자인을 사용하는 것을 권장하는데, 이는 설정하고 유지하는 데 가장 간단한 디자인 접근 방법이기 때문입니다.

모바일 친화성을 테스트하기 위한 무료 SEO 도구:

- Small SEO Tools
- Browserstack
- Websiteplanet
- ready.mobi
- Seomator

<div class="content-ad"></div>

## 6. 사이트 속도

![image](/assets/img/2024-07-06-12easySEOTipsEveryDeveloperShouldKnow_2.png)

구글이 실시한 연구에 따르면, 사용자의 53%가 웹사이트가 3초 내에 로드되지 않으면 그 사이트를 방문하지 않습니다.

따라서 페이지 속도는 이제 SEO의 순위 결정 요소입니다.

<div class="content-ad"></div>

로딩 속도에 대한 통찰을 얻기 위해 다음 3가지 메트릭을 사용하세요: 속도, 상호 작용 및 시각적 안정성에 기반해요:

- Largest Contentful Paint (LCP): 주요 콘텐츠가 로드되는 데 걸리는 시간입니다. 2.5초 이하가 좋아요.
- First Input Delay (FID): 사용자가 페이지와 상호 작용할 때까지 걸리는 시간입니다. 100밀리초 이하가 좋아요.
- Cumulative Layout Shift (CLS): 사용자가 레이아웃이 변화하는 빈도입니다. CLS 점수는 가능한 0에 가까워야 해요.

![이미지](/assets/img/2024-07-06-12easySEOTipsEveryDeveloperShouldKnow_3.png)

- PageSpeed Insights
- Google Lighthouse
- BrowserStack Speedlab
- Gtmetrix
- Webpagetest

<div class="content-ad"></div>

## 7. SEO를 위한 내부 링크

동일한 도메인에 있는 다른 페이지로 링크를 연결하는 것입니다.

다른 기사 또는 제품을 세련된 방식으로 연결하면 사용자가 사이트에 머무는 시간이 증가하고 관련 페이지의 링크 네트워크가 생성됩니다.

내부 링크는 페이지를 더 쉽게 발견하도록하며 Google이 페이지를 순위 매기는 데 사용하는 Page Rank도 전달합니다.

<div class="content-ad"></div>

### 내부 링크 구조의 최상의 방법

내부 링크를 스팸으로 남기는 것이 좋은 결정인가요?

- 아니요. 절대 그렇지 않아요.

내용이 자연스럽게 느껴지는 곳에 링크를 걸고, 목적지를 설명하는 앵커 텍스트로 연결하는 것이 중요해요.

<div class="content-ad"></div>

예시:
글 전체에서 내 다른 블로그에 링크도 걸어 놓았네요 😉.

## 8. 스키마 마크업과 구조화된 데이터

스키마는 구글의 랭킹 요소는 아니에요. 그렇지만, 간접적으로 구글에서 높게 랭크 될 수 있도록 돕습니다.

구글, 빙, 야후, 야후!는 서로 협력하여 Schema.org를 만들었어요. 이를 통해 검색 엔진이 콘텐츠를 이해하고 가장 관련성 높은 결과를 제공할 수 있도록 필요한 데이터를 제공할 수 있도록 했어요.

<div class="content-ad"></div>

### 구조화된 데이터 구현

Google에서 제안한 3가지 방법 중 원하는 것을 선택하여 사용할 수 있어요:

- Json-ld
- Microdata
- RDFa

#### 1. 블로그에서 가장 중요한 자료를 선택하세요.

<div class="content-ad"></div>

일반적인 키는 다음과 같습니다:

- 작가 정보
- 게시 날짜
- 카테고리 또는 태그

#### 2. 스크립트 생성

```js
{
  "@context": "https://schema.org",
  "@type": "BlogPosting",
  ...기타 데이터,
  "author": {
    "@type": "Person",
    "name": "당신의 이름"
  },
  "datePublished": "2024-07-01",
}
```

<div class="content-ad"></div>

#### 3. HTML에 JSON-LD 임베드하기

이 JSON-LD 스크립트를 지금 블로그의 HTML에 삽입해야 합니다.

이를 위해 `script type="application/ld+json"`과 같은 특별한 스크립트 태그를 사용합니다.
이를 HTML의 head 또는 body에 삽입할 수 있습니다.

내 GitHub 자기소개 블로그의 스키마 마크업은 다음과 같이 보입니다:

<div class="content-ad"></div>

```js
<script type="application/ld+json">
  {
    "@context": "https://schema.org",
    "@type": "BlogPosting",
    "url": "https://acidop.codes/blogs/beautify-your-github-profile",
    "image": {
      "@type": "ImageObject",
      "url": "/blogs/beautify-your-github-profile/cover.jpg",
      "height": 630,
      "width": 1200
    },
    "author": {
      "@type": "Person",
      "name": "AcidOP"
    },
    "inLanguage": "en-US",
    "headline": "Beautify Your GitHub Profile like a Pro",
    "dateCreated": "2023-03-15",
    "datePublished": "2023-03-15",
    "description": "Beautify your GitHub ... appealing for developers."
  }
</script>
```

위 스크립트는 `head` 또는 `body`에 배치할 수 있습니다.

스키마.org 검증기를 통해 구조화된 데이터의 유효성을 확인할 수 있습니다.

## 9. XML 사이트맵과 로봇.txt

<div class="content-ad"></div>

XML 사이트맵은 검색 엔진에게 귀하의 웹 사이트의 어떤 URL이 색인되어야 하는지 알려줍니다. 이는 귀하의 웹 사이트의 모든 중요한 콘텐츠 목록입니다.

여기 저의 웹 사이트의 일부분을 담은 사이트맵이 있습니다:

```js
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://acidop.codes</loc>
    <changefreq>Yearly</changefreq>
    <priority>0.9</priority>
  </url>
  <url>
    <loc>https://acidop.codes/blogs/nextjs-markdown-editor</loc>
    <changefreq>Yearly</changefreq>
    <priority>0.7</priority>
  </url>
</urlset>
```

귀하의 프로젝트 루트에 sitemap.xml로 사이트맵을 저장하세요. 그런 다음 Google Search Console에 사이트맵을 제출하세요.

<div class="content-ad"></div>

### 로봇.txt

로봇.txt 파일은 웹 사이트에서 검색 엔진에게 어떤 페이지를 방문해도 되는지 말해주는 간단한 텍스트 파일입니다.

예시:

```js
Sitemap: https://acidop.codes/sitemap.xml
User-agent: *
Allow: /
Disallow: /admin
```

<div class="content-ad"></div>

위의 예에서는 사이트맵을 제출하고, 관리자 디렉토리를 색인화하지 않기 위해 명확히 표현했습니다.

## 10. 카노니컬 태그 (중요)

```html
<head>
  <link rel="canonical" href="https://acidop.codes/blogs/developer-seo-tips" />
</head>
```

SEO에서 카노니컬 태그는 여러 페이지에 유사한 내용이나 동일한 내용이 있는 경우 혼란을 피하기 위해 검색 엔진에게 해당 웹페이지의 원본 버전을 알려주는 링크 속성입니다.

<div class="content-ad"></div>

Google은 중복 콘텐츠가 있는 사이트를 벌점을 받게 될 수 있다고 알려져 있어요.

예시:

![이미지](/assets/img/2024-07-06-12easySEOTipsEveryDeveloperShouldKnow_4.png)

UTM 매개변수를 사용하더라도 두 링크가 같은 페이지를 가리키는 거 맞나요?

<div class="content-ad"></div>

하지만 Google은 두 URL을 다른 페이지로 간주할 것입니다.

적절한 캐노니컬 태그를 사용하면 Google이 두 페이지가 동일한 것임을 이해하는 데 도움이 됩니다.

### 내 이야기 :

저는 2개의 사이트에 블로그를 씁니다. Dev.to (캐노니컬 URL을 지원하는)와 acidop.codes인 제 웹사이트입니다. 제 주요 출처는 당연히 제 웹사이트입니다.

<div class="content-ad"></div>

첫 번째로 내 블로그를 내 웹사이트에 올려요. 그런 다음에 동일한 블로그를 dev.to에 게시하는데 canonical URL이 내 웹사이트를 가리키도록 설정해요.

이렇게 하면 더 많은 관객을 유치할 수 있으면서도 구글로부터 처벌을 받게 되지 않아요.

## 11. 사용자 경험 (UX)에 대한 SEO

구글은 사용자가 사이트에서(페이지 경험) 원활하고 직관적이며 즐거운 경험을 할 수 있기를 바라요.

<div class="content-ad"></div>

구글은 페이지 경험을 평가하기 위한 가이드라인을 가지고 있어요.

- 귀하의 페이지는 우수한 코어 웹 핵심 지표를 가지고 있나요?
- 귀하의 페이지는 안전하게 제공되고 있나요?
- 귀하의 콘텐츠는 모바일 기기에서 잘 표시되나요?
- 귀하의 콘텐츠는 주요 콘텐츠를 방해하거나 주요 콘텐츠를 가릴 수 있는 과도한 광고 사용을 피하나요?
- 귀하의 페이지는 방해적인 중간 페이지를 사용하지 않나요?
- 방문자들이 귀하의 페이지에서 주요 콘텐츠를 다른 콘텐츠와 쉽게 구별할 수 있도록 디자인되어 있나요?

## 12. 정기적인 SEO 감사 및 모니터링

SEO 감사는 검색 엔진을 위해 웹사이트가 최적화되어 있는지를 결정합니다. 웹사이트의 순위에 해를 끼칠 수 있는 문제점을 식별하고 이를 수정하는 방법을 제안합니다.

<div class="content-ad"></div>

친구야, SEO 감사를 위한 무료 도구들을 소개할게:

- Aioseo (최고!)
- seoptimer.com
- Seomator
- Seobility
- Semrush
- Ahrefs

### 결론

여기 12가지 SEO 개선을 위한 쉬운 팁이야.

<div class="content-ad"></div>

그들을 모두 구현하는 것은 크게 손 댈 필요가 없으며 매우 직관적입니다.
