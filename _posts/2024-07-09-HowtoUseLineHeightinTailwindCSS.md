---
title: "Tailwind CSS에서 Line Height 사용하는 방법"
description: ""
coverImage: "/assets/img/2024-07-09-HowtoUseLineHeightinTailwindCSS_0.png"
date: 2024-07-09 16:14
ogImage:
  url: /assets/img/2024-07-09-HowtoUseLineHeightinTailwindCSS_0.png
tag: Tech
originalTitle: "How to Use Line Height in Tailwind CSS"
link: "https://dev.to/saim_ansari/how-to-use-line-height-in-tailwind-css-5aon"
---

Tailwind CSS에서 줄 간격을 설정하려면 leading 유틸리티 클래스를 사용할 수 있어요. leading 클래스는 요소의 줄 간격을 제어해요.

```js
<!-- 기본 줄 간격 -->
<p class="leading-normal">기본 줄 간격의 문단입니다.</p>

<!-- 여유로운 줄 간격 -->
<p class="leading-loose">여유로운 줄 간격의 문단입니다.</p>

<!-- 탄탄한 줄 간격 -->
<p class="leading-tight">탄탄한 줄 간격의 문단입니다.</p>
```

![이미지](/assets/img/2024-07-09-HowtoUseLineHeightinTailwindCSS_0.png)

```js
<!-- 줄 간격: 글꼴 크기의 1.25배 -->
<p class="leading-5">글꼴 크기의 1.25배 줄 간격의 문단입니다.</p>

<!-- 줄 간격: 글꼴 크기의 1.75배 -->
<p class="leading-7">글꼴 크기의 1.75배 줄 간격의 문단입니다.</p>
```

<div class="content-ad"></div>

Tailwind CSS에서 사용할 수 있는 주요 유틸리티 클래스 목록을 안내해드릴게요:

- leading-none
- leading-tight
- leading-snug
- leading-normal
- leading-relaxed
- leading-loose

leading-`크기` (예: leading-3, leading-4, leading-5, 등)

REM 단위로 사용자 정의 줄 높이를 설정할 수 있어요.

<div class="content-ad"></div>

```js
<div class="flex flex-col items-center justify-center h-screen space-y-4">
  <p class="max-w-xl leading-[3rem]">
    그래서 나는 물 속으로 걸어들기 시작했다. 솔직히 말하자면, 난 두려웠어. 하지만 나는 계속 전진했고, 파도를 지나가며
    이상하게도 평정이 내려왔어. 그 순간이야말로 신의 개입인지 살아 있는 모든 것들과의 유대감인지 모르겠지만, 제리야, 그
    순간 나는 해양 생물학자였어.
  </p>
</div>
```

<img src="/assets/img/2024-07-09-HowtoUseLineHeightinTailwindCSS_1.png" />
