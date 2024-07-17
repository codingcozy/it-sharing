---
title: "반응형 비디오 - 비디오 콘텐츠 크기를 줄이는 방법은"
description: ""
coverImage: "/it-shar.ing/assets/no-image.jpg"
date: 2024-07-12 18:21
ogImage:
  url: /it-shar.ing/assets/no-image.jpg
tag: Tech
originalTitle: "Responsive video - How can we reduce the size of video content served?"
link: "https://dev.to/robole/responsive-video-how-can-we-reduce-the-size-of-video-content-served-19nb"
---

비디오는 웹사이트에서 사용되는 미디어 중 가장 무겁습니다. 비디오가 포함된 중간 페이지는 모바일 장치에서 거의 4메가바이트의 용량을 차지합니다. 이 무게는 성능, 사용자의 데이터 비용, 웹사이트 소유자의 호스팅 비용 및 에너지 소비에 큰 영향을 미칩니다. 전송되는 비디오 콘텐츠의 크기를 어떻게 줄일 수 있을까요?

문제의 큰 부분은 종종 모두에게 동일한 큰 비디오 파일이 제공된다는 점입니다. 이상적으로는 장치의 뷰포트 너비와 해상도에 따라 크기가 크거나 작은 비디오가 제공되는 것이 바람직합니다. 이를 어떻게 할 수 있을까요?

## 비디오에 대해 scrset와 sizes 속성을 사용할 수 있을까요?

아니요!

<div class="content-ad"></div>

익숙할 수도 있는 이미지에 대한 반응형 이미지 세트를 제공하기 위해 사용되는 srcset 및 sizes 속성들에 대해 알고 계실 수 있습니다. 그러나 `video` 요소는 관련된 `source` 요소에 srcset 속성을 사용할 수 없습니다.

HTML 표준에 대한 비디오 파일의 srcset 및 sizes 속성 지원을 추가하는 제안이 있습니다.

이에 대해 어떻게 대처해야 할까요?

## 어떻게 비디오 콘텐츠 크기를 줄일 수 있을까요?

<div class="content-ad"></div>

주요 옵션이 두 가지 있어요.

### 반응형 HTML 방법

`source` 요소에 media 속성을 사용하여 미디어 쿼리를 제공할 수 있어요. 브라우저의 너비, 해상도 또는 방향에 따라 비디오 파일을 조건부로 제공할 수 있어요. 이렇게 사용할 수 있어요:

```js
<video>
    <source src="/video/small.mp4" media="(max-width: 599px)">
    <source src="/video/large.mp4">
</video>
```

<div class="content-ad"></div>

이 예시에서, 브라우저 뷰포트의 너비가 600픽셀 미만이면 브라우저는 작은 mp4 비디오 파일을 사용하고, 그렇지 않으면 큰 mp4 비디오 파일을 사용할 것입니다.

이 기능의 특이한 점은 2014년에 HTML 표준에서 삭제되었다는 점입니다. 이는 다른 방법들이 우수하다는 주장에 일부 기반을 두고 있었습니다. 이로 인해 Chrome과 Firefox는 변경된 표준을 준수하기 위해 안정적인 코드를 제거했지만, Safari는 그대로 남아 있었습니다. 2021년에 Scott Jehl로부터 기능을 복원할 것을 요청받은 후에야 다시 기능이 복원되었습니다.

제가 알기로 미디어 속성은 모든 브라우저에서 다시 지원됩니다. web.dev 블로그에는 "웹의 새로운 기능" 글에서 반응형 비디오에 대해 약간 모호한 노트가 있습니다 - 미디어 속성을 언급하거나 srcset 속성을 언급하는지 명확하지 않습니다.

더 많은 세부 정보를 보려면 '반응형 HTML 비디오 사용 방법' 글을 읽는 것을 추천합니다.

<div class="content-ad"></div>

### 스트리밍 프로토콜 사용하기

비디오 콘텐츠의 크기를 줄이는 또 다른 방법은 HTTP 라이브 스트리밍(HLS) 표준과 같은 스트리밍 프로토콜을 사용하는 것입니다. HLS의 가장 중요한 기능은 비디오의 비트율을 연결 속도에 실제로 적응시키는 능력입니다. HLS 비디오는 다른 해상도와 비트율로 인코딩됩니다.

스트리밍 프로토콜은 반응형 HTML 접근법보다 구현이 좀 더 복잡합니다. HLS는 Safari에서 지원되지만 다른 브라우저를 위해 일부 JavaScript가 필요합니다. 이것은 조금 아쉬운 부분입니다.

HTML 부분은 간단합니다. 소스 스트림을 사용하는 `video` 요소를 사용하면 됩니다.

<div class="content-ad"></div>

```js
<!-- 스트리밍 비디오 -->
<video src="https://test-streams.mux.dev/x36xhzz/x36xhzz.m3u8"></video>
```

이 글에서 HLS에 대해 더 알아볼 수 있어요.

만약 비디오로 YouTube를 포함하려고 하신다면, 이전 게시물에서 YouTube 포함을 어떻게 최적화하는지에 대해 논의했어요. 기본 포함 코드 스니펫을 사용하지 마세요!!

Rob O'Leary가 작성함

<div class="content-ad"></div>

최신 기사를 받아보려면 웹 피드에 가입해주세요.

© Rob OLeary 2024
