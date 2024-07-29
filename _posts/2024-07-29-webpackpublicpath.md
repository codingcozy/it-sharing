---
title: "Webpack 설정 publicPath를 설정하는 방법"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-07-29 13:53
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "webpack publicpath"
link: "https://dev.to/zwd321081/webpack-publicpath-321h"
---


publicPath 구성 옵션은 정적 자산의 경로를 동적으로 조정할 수 있도록 하는 자리 표시자 역할을 합니다.

예를 들어, 당신이 commons.js와 같은 정적 자산을 CDN에 업로드한 상황을 생각해 봅시다. 이 파일의 실제 URL은 다음과 같을 수 있습니다:

https://s1.cdn.com/my-project/commons.js

하지만, HTML 페이지는 일반적으로 자체 도메인에서 제공됩니다. 예를 들어:
https://my-own-domain.com/my-project/index.html

<div class="content-ad"></div>

index.html이 CDN에 호스팅된 정적 파일을 올바르게 참조하도록 하려면 publicPath 설정을 활용할 수 있습니다. 이 구성은 index.html 내의 commons.js의 로컬 경로를 다음과 같이 변경합니다:

https://my-own-domain.com/my-project/commons.js
다음과 같은 CDN 경로로:

https://s1.cdn.com/my-project/commons.js
publicPath를 설정함으로써 HTML 파일 내의 모든 정적 자산에 대한 참조가 올바른 CDN URL을 가리키도록 하여 효율적인 콘텐츠 전달을 용이하게 할 수 있습니다.