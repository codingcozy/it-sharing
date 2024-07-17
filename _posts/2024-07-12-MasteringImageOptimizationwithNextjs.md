---
title: "Nextjs로 이미지 최적화 마스터하기 최신 방법과 기술"
description: ""
coverImage: "/assets/img/2024-07-12-MasteringImageOptimizationwithNextjs_0.png"
date: 2024-07-12 19:06
ogImage:
  url: /assets/img/2024-07-12-MasteringImageOptimizationwithNextjs_0.png
tag: Tech
originalTitle: "Mastering Image Optimization with Next.js"
link: "https://medium.com/stackademic/mastering-image-optimization-with-next-js-f10acb0a6830"
---

요즘의 디지털 시대에는 사용자 경험이 중요합니다. 웹 애플리케이션이 빠르고 효율적으로 최적화되어 있는지 확인하는 것이 중요합니다. 웹 성능에 영향을 미치는 중요한 요소 중 하나는 이미지 로딩 시간입니다. 최적화되지 않은 이미지는 사이트의 속도를 현저히 느리게 만들어 바운스율을 높이고 사용자를 좌절시킬 수 있습니다.

![image](/assets/img/2024-07-12-MasteringImageOptimizationwithNextjs_0.png)

# 이미지 최적화의 중요성

Next.js 이미지 최적화에 대해 알아보기 전에, 이것이 왜 중요한지 이해해 보겠습니다. 이미지는 종종 웹페이지 콘텐츠의 가장 큰 부분을 차지하며, 최적화되지 않은 이미지는 다음과 같은 문제를 유발할 수 있습니다:

<div class="content-ad"></div>

- 느린 로딩 시간: 큰 이미지 파일은 다운로드하는 데 시간이 오래 걸려 전체 웹페이지의 로드 시간을 늘립니다.
- 높은 대역폭 사용량: 제한된 데이터 요금제를 사용하는 사용자는 큰 이미지 다운로드로 인해 더 높은 요금을 낼 수 있습니다.
- 사용자 경험 저하: 느린 로딩 이미지는 사용자를 짜증나게 할 수 있으며, 이로 인해 이탈률 증가와 참여율 하락이 발생할 수 있습니다.

# Next.js: 내장 이미지 최적화의 힘

Next.js는 사용자가 요청할 때 이미지를 자동으로 최적화하는 next/image 구성 요소를 통해 이미지 최적화를 간단하게 합니다. 다음은 Next.js가 이미지 최적화를 처리하는 방법입니다:

- 자동 크기 조정 및 압축: Next.js는 기기의 화면 크기와 해상도에 따라 이미지의 크기를 조정하고 압축하여 각 사용자에게 적절한 크기의 이미지를 제공합니다.
- 지연 로딩: 이미지는 뷰포트에 들어올 때만 로드되므로 초기 로드 시간이 개선되고 대역폭이 절약됩니다.
- 캐싱 및 CDN: 최적화된 이미지는 캐시되어 콘텐츠 전송 네트워크(CDN)를 통해 제공되어 사용자에게 더 가까운 위치에서 이미지를 제공함으로써 로드 시간을 줄입니다.

<div class="content-ad"></div>

# Next.js에서 이미지 최적화 구현하기

Next.js 애플리케이션에서 이미지 최적화를 어떻게 구현할 수 있는지 알아보겠습니다.

## 단계 1: next/image 컴포넌트 사용

Next.js는 이미지 최적화를 손쉽게 처리하기 위한 Image 컴포넌트를 제공합니다. 아래는 그 사용 예시입니다:

<div class="content-ad"></div>

```js
import Image from "next/image";

const HomePage = () => {
  return (
    <div>
      <h1>내 Next.js 앱에 오신 것을 환영합니다</h1>
      <Image src="/이미지/경로/여기에.jpg" alt="이미지 설명" width={500} height={300} quality={75} />
    </div>
  );
};
export default HomePage;
```

이 예제에서:

- src 속성은 이미지 원본을 지정합니다.
- alt 속성은 이미지에 대한 접근성 있는 설명을 제공합니다.
- width 및 height 속성은 이미지의 크기를 정의합니다.
- quality 속성은 이미지 품질을 제어할 수 있습니다 (0부터 100까지).

사용 가능한 이미지 속성:

<div class="content-ad"></div>

<img src="/assets/img/2024-07-12-MasteringImageOptimizationwithNextjs_1.png" />

## Step 2: 도메인 및 디바이스 크기 구성

Next.js를 사용하면 이미지 로딩을 더욱 최적화하기 위해 외부 이미지 도메인 및 디바이스 크기를 구성할 수 있습니다. 다음과 같이 next.config.js 파일을 업데이트하십시오:

```js
module.exports = {
  images: {
    domains: ["example.com"],
    deviceSizes: [320, 420, 768, 1024, 1200],
    imageSizes: [16, 32, 48, 64, 96],
  },
};
```

<div class="content-ad"></div>

- domains: 이미지에 대한 허용된 외부 도메인을 지정합니다.
- deviceSizes: 다양한 기기 크기에 대한 브레이크포인트를 정의합니다.
- imageSizes: 최적화된 이미지에 대한 사용 가능한 크기를 정의합니다.

또는 다음과 같이 이미지 구성 요소에서 직접 크기를 사용할 수 있습니다:

```js
import Image from "next/image";

export default function Page() {
  return (
    <div className="grid-element">
      <Image fill src="/example.png" sizes="(max-width: 768px) 100vw, (max-width: 1200px) 50vw, 33vw" />
    </div>
  );
}
```

화면에 표시되는 처음 이미지에 대해 우선순위 사용하기

<div class="content-ad"></div>

```js
우선순위={false} // {false} | {true}
```

'우선순위'가 true로 설정되면 이미지가 높은 우선순위로 간주되어 사전로드 됩니다. 우선순위를 사용하는 이미지에 대해 지연로드가 자동으로 비활성화됩니다.

임의적으로 사용할 수 있는 이미지는 가장 큰 콘텐츠 로드(LCP) 요소로 감지된 이미지에 '우선순위' 속성을 사용해야 합니다. 서로 다른 뷰포트 크기에 따라 다른 이미지가 LCP 요소일 수 있으므로 여러 우선순위 이미지를 사용하는 것이 적절할 수 있습니다.

이미지가 폴드 위에 보이는 경우에만 사용해야 합니다. 기본값은 false입니다.

<div class="content-ad"></div>

# 이미지 최적화를 위한 최상의 실천 방법

Next.js에서 이미지 최적화의 혜택을 극대화하기 위해 다음과 같은 최상의 실천 방법을 고려해보세요:

- 모던 포맷 사용: 가능한 경우 WebP와 같은 모던 이미지 포맷을 사용하세요. 이러한 포맷은 JPEG나 PNG과 같은 전통적인 포맷보다 더 나은 압축을 제공합니다.

![이미지](/assets/img/2024-07-12-MasteringImageOptimizationwithNextjs_2.png)

<div class="content-ad"></div>

웹P 형식에 대해 더 알아보세요!

- 반응형 이미지: 이미지가 다양한 화면 크기와 해상도에 적응하여 반응성을 유지합니다.

![이미지](/assets/img/2024-07-12-MasteringImageOptimizationwithNextjs_3.png)

- 원본 최적화: 업로드하기 전에 ImageMagick 같은 도구나 온라인 서비스를 사용하여 이미지 크기를 최적화하세요.

<div class="content-ad"></div>

<img src="/assets/img/2024-07-12-MasteringImageOptimizationwithNextjs_4.png" />

# 결론

이미지 최적화는 웹 성능의 중요한 부분이며, Next.js를 사용하면 이를 구현하기가 이전보다 쉬워집니다. next/image 컴포넌트를 활용하고 모베스트 프랙티스를 따르면 웹 애플리케이션의 속도를 보장하고 효율적이며 훌륭한 사용자 경험을 제공할 수 있습니다. 지금 바로 Next.js로 이미지 최적화를 시작하고 차이를 경험해보세요!

Next.js에서 이미지 최적화에 대해 더 자세히 알고 싶다면 공식 Next.js 문서를 참조할 수 있습니다.

<div class="content-ad"></div>

## ❤️ 만약 제 작품을 좋아하신다면, 팔로우와 구독 부탁드립니다 ❤️

- 팔로우
- 구독
- LinkedIn
- Front-end World 방문 및 구독

# Stackademic 🎓

끝까지 읽어주셔서 감사합니다. 떠나시기 전에:

<div class="content-ad"></div>

- 작가에게 박수를 보내고 팔로우해주세요! 👏
- 팔로우하기: X | LinkedIn | YouTube | Discord
- 다른 플랫폼에서도 만나보세요: In Plain English | CoFeed | Differ
- Stackademic.com에서 더 많은 콘텐츠 확인하기
