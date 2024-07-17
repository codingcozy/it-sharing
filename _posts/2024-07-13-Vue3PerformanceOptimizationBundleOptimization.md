---
title: "Vue3 성능 최적화  번들 최적화 방법"
description: ""
coverImage: "/assets/img/2024-07-13-Vue3PerformanceOptimizationBundleOptimization_0.png"
date: 2024-07-13 18:35
ogImage:
  url: /assets/img/2024-07-13-Vue3PerformanceOptimizationBundleOptimization_0.png
tag: Tech
originalTitle: "Vue3 Performance Optimization — Bundle Optimization"
link: "https://medium.com/stackademic/vue3-performance-optimization-bundle-optimization-70bc1d269e03"
---

![image](/assets/img/2024-07-13-Vue3PerformanceOptimizationBundleOptimization_0.png)

Vue 프레임워크는 데이터 양방향 바인딩과 가상 DOM 기술을 통해 프런트엔드 개발에서 가장 귀찮고 지저분한 부분을 처리합니다. 더 이상 DOM을 어떻게 조작해야 하는지 또는 가장 효율적으로 어떻게 할 지 신경 쓸 필요가 없어졌습니다. 그러나 Vue 프로젝트에서는 여전히 첫 화면 최적화와 컴파일 구성 최적화와 같은 문제가 존재합니다. 따라서 Vue 프로젝트의 성능 최적화에 여전히 집중하여 더 효율적인 성능과 더 나은 사용자 경험을 이끌어야 합니다.

Vue3 성능 최적화 - 코드 수준 최적화

# Gzip 압축

<div class="content-ad"></div>

프론트엔드 자원이 너무 크면 서버 자원 요청이 느려질 수 있습니다. 프론트엔드는 Gzip 압축을 통해 파일 크기를 대략 60% 정도 줄일 수 있습니다. 백엔드에서 간단한 처리 후에 압축된 파일은 브라우저에서 정상적으로 해독될 수 있습니다.

## 플러그인 설치

```js
yarn add vite-plugin-compression -D
```

## Vite 구성

<div class="content-ad"></div>

```js
// vite.config.ts
import viteCompression from "vite-plugin-compression";

export default defineConfig({
  plugins: [
    // ....
    viteCompression({
      algorithm: "gzip",
      deleteOriginFile: false, // 압축 후 소스 파일을 삭제할지 여부
    }),
  ],
});
```

더 많은 구성을 위해서는 공식 문서를 참조해주세요.

## 서버 구성

Vue 부분을 구성한 후에 nginx에 직접 배포해도 즉시 적용되지 않습니다. nginx의 gzip 기능을 활성화해야 합니다. 먼저 현재 nginx 설치에 gzip 모듈이 포함되어 있는지 확인해야 합니다. 일반적으로 ngx_http_gzip_static_module이 기본적으로 설치되지만, nginx -V 명령어를 실행하여 --with-http_gzip_static_module이 포함되어 있는지 확인할 수 있습니다.

<div class="content-ad"></div>

```js
// nginx.conf 파일

http{
  #......다른 구성

  # 서버 모듈화
  include xxx/*.conf; # nginx.conf 파일이 있는 디렉토리에 xxx라는 폴더를 만듭니다.

  # gzip 활성화
  gzip on;

  # 정적 압축
  gzip_static on;

  # gzip 압축 최소 파일 크기, 이 값보다 작은 파일은 압축되지 않음
  gzip_min_length 1k;

  # gzip 압축 레벨, 1-10까지 가능. 숫자가 높을수록 더 좋은 압축 및 더 많은 CPU 시간 소비
  gzip_comp_level 2;

  # 압축할 파일 유형. 다양한 형식의 javascript. 값을 mime.types 파일에서 찾을 수 있습니다.
  gzip_types text/plain application/javascript application/x-javascript text/css application/xml text/javascript application/x-httpd-php image/jpeg image/gif image/png image/jpg;

  # http 헤더에 Vary: Accept-Encoding 추가 여부, 권장됨
  gzip_vary on;

  # IE 6 gzip 비활성화
  gzip_disable "MSIE [1-6].";

  server{
    ...
  }
  #......다른 구성
}
```

그런 다음 nginx 서버를 재시작하면 일반적으로 접근할 수 있어야 합니다.

# 패키지 분리

여기 개인적인 관점이 있습니다: 서로 다른 모듈이 대부분 같은 플러그인을 사용하는 경우, HTTP 요청 수를 줄이기 위해 가능한 경우 같은 파일에 패키지화하는 것이 좋습니다. 서로 다른 모듈이 뚜렷하게 다른 플러그인을 사용하는 경우, 이를 다른 모듈로 패키지화해야 합니다. 이것은 모순을 제시합니다.

<div class="content-ad"></div>

여기서는 최소한의 패키지 분할이 사용되었습니다. 이전 시나리오라면, 'vendor'를 직접 반환할 수 있습니다.

```js
rollupOptions: {
  output: {
    manualChunks(id) {
      if (id.includes("node_modules")) {
        // 각 플러그인을 독립적인 파일로 패키징
        return id.toString().split("node_modules/")[1].split("/")[0].toString();
      }
    }
  }
}
```

# 정적 리소스 카테고리별 패키징

Vite는 esbuild와 rollup 위에 구축되었습니다. 패키징할 때 아웃풋이 구성되지 않으면 모든 파일이 섞여져 나옵니다.

<div class="content-ad"></div>

출력을 구성한 후에는 모든 파일이 범주화되어 저장되어 파일을 찾기가 쉬워집니다.

vite.config.js 파일에서는 출력을 다음과 같이 구성합니다:

```js
build: {
  sourcemap: false,
  // 500kb를 초과하는 패키지 크기에 대한 경고 제거
  chunkSizeWarningLimit: 4000,
  rollupOptions: {
    input: {
      index: pathResolve("index.html")
    },
    // 정적 리소스를 범주화하여 패키징
    output: {
      chunkFileNames: "static/js/[name]-[hash].js",
      entryFileNames: "static/js/[name]-[hash].js",
      assetFileNames: "static/[ext]/[name]-[hash].[ext]"
    }
  }
}
```

Vite Code Splitting Plugin — vite-plugin-chunk-split. 다양한 코드 분할 전략을 지원하며 manualChunks 작업을 통해 잠재적인 순환 의존성 문제를 피할 수 있습니다.

<div class="content-ad"></div>

# 프로덕션 환경에서 디버거 제거하기

플랫폼에서 개발한 vite-plugin-remove-console 플러그인을 사용하여 패키징 및 빌드 프로세스 중에 플랫폼에서 모든 console.log를 제거할 수 있습니다.

# 패키징 분석 플러그인

Rollup Plugin Visualizer는 종속성 분석 플러그인으로 직관적인 뷰 분석, 선버스트(원형 계층도 차트, 스펙트럼처럼), 트리맵(사각형 계층도 차트, 직관적이며 기본 매개변수도 됨), 네트워크(포함 관계 보기 위한 그리드 차트), 원시 데이터(원시 데이터 모드, JSON 형식) 및 목록(목록 모드) 등 다양한 분석 모드를 제공합니다. 원하는 관찰 모드를 선택할 수 있습니다.

<div class="content-ad"></div>

rollup-plugin-visualizer 플러그인을 설치해주세요

```js
yarn add rollup-plugin-visualizer -D
```

vite.config.ts 파일에서 다음과 같이 import하고 설정을 추가해주세요

```js
// Import
import { visualizer } from "rollup-plugin-visualizer";

plugins: [
  // 플러그인 구성 배열에 추가해주세요
  visualizer({
    open: true, // 이 항목을 true로 설정해야 함에 유의해주세요. 그렇지 않으면 작동하지 않습니다. 로컬 서버 포트가 있다면 패키징 후 자동으로 표시됩니다.
    gzipSize: true,
    file: "stats.html", // 생성된 분석 차트의 파일 이름
    brotliSize: true,
  }),
];
```

<div class="content-ad"></div>

패키징 후 자동으로 표시되며, 효과 차트가 표시됩니다:

![Vue3 성능 최적화 번들 최적화](/assets/img/2024-07-13-Vue3PerformanceOptimizationBundleOptimization_1.png)

# 이미지 리소스 압축

unplugin-imagemin 플러그인을 설치하세요.

<div class="content-ad"></div>

```js
pnpm add unplugin-imagemin@latest -D
```

vite.config.js에서 설정하기

```js
// Import
import imagemin from "unplugin-imagemin/vite";
import path from "path";

// 플러그인 구성 배열에 추가
plugin: [
  imagemin({
    // 기본 모드인 sharp입니다. squoosh와 sharp를 지원합니다.
    mode: "squoosh",
    beforeBundle: true,
    // 다양한 이미지를 압축하기 위한 기본 구성 옵션
    compress: {
      jpg: {
        quality: 10,
      },
      jpeg: {
        quality: 10,
      },
      png: {
        quality: 10,
      },
      webp: {
        quality: 10,
      },
    },
    conversion: [
      { from: "jpeg", to: "webp" },
      { from: "png", to: "webp" },
      { from: "JPG", to: "jpeg" },
    ],
  }),
];
```

# SVG 압축

<div class="content-ad"></div>

일반적으로 다운로드된 SVG 또는 복사된 SVG 코드에는 관련이 없는 요소가 포함되어 있을 수 있습니다. 이러한 요소를 제거하여 SVG 크기를 줄일 수 있습니다.

vite-plugin-svg-icons 플러그인을 설치해주세요.

```js
yarn add vite-plugin-svg-icons -D
```

vite.config.js에서 설정해주세요.

<div class="content-ad"></div>

```js
import { createSvgIconsPlugin } from 'vite-plugin-svg-icons'
import path from 'path'

plugins: [
  createSvgIconsPlugin({
    // 캐시할 아이콘 폴더 지정
    iconDirs: [path.resolve(process.cwd(), 'src/icons')],
    // 심볼 ID 형식 지정
    symbolId: 'icon-[dir]-[name]',

    /**
     * 사용자 정의 삽입 위치
     * @default: body-last
     */
    inject?: 'body-last' | 'body-first'

    /**
     * 사용자 정의 DOM ID
     * @default: __svg__icons__dom__
     */
    customDomId: '__svg__icons__dom__',
  }),
],
```

src/main.ts에 등록 스크립트를 소개합니다.

```js
import "virtual:svg-icons-register";
```

참고:

<div class="content-ad"></div>

만약 "fast-glob를 찾을 수 없다"는 오류가 발생하면 다음 명령을 실행하여 설치하세요:

```js
npm install fast-glob -D
```

## 사용법

/src/components/SvgIcon.vue

<div class="content-ad"></div>

```js
<template>
  <svg aria-hidden="true">
    <use :href="symbolId" :fill="color" />
  </svg>
</template>

<script>
import { defineComponent, computed } from 'vue'

export default defineComponent({
  name: 'SvgIcon',
  props: {
    prefix: {
      type: String,
      default: 'icon',
    },
    name: {
      type: String,
      required: true,
    },
    color: {
      type: String,
      default: '#333',
    },
  },
  setup(props) {
    const symbolId = computed(() => `#${props.prefix}-${props.name}`)
    return { symbolId }
  },
})
</script>
```

/src/component-a.vue

```js
<template>
  <div>
    <SvgIcon name="icon1"></SvgIcon>
    <SvgIcon name="icon2"></SvgIcon>
    <SvgIcon name="icon3"></SvgIcon>
    <SvgIcon name="dir-icon1"></SvgIcon>
  </div>
</template>
```

참고:

<div class="content-ad"></div>

플러그인의 SVG는 해시를 생성하여 구분하지 않고, 폴더로 구분합니다.

만약 iconDirs에 해당하는 폴더에 다른 폴더가 포함되어있다면,

예시:

그렇다면 생성된 SymbolId가 주석에 작성됩니다.

<div class="content-ad"></div>

```js
# src/icons
- icon1.svg # icon-icon1
- icon2.svg # icon-icon2
- icon3.svg # icon-icon3
- dir/icon1.svg # icon-dir-icon1
- dir/dir2/icon1.svg # icon-dir-dir2-icon1
```

# CDN 가속

vite-plugin-cdn-import 플러그인을 설치하세요.

```js
pnpm add vite-plugin-cdn-import -D
```

<div class="content-ad"></div>

vite.config.js에서 다음과 같이 설정해주세요.

```js
    plugins: [
        cdn({
            modules: [
                {
                  // Import할 때 사용하는 패키지 이름
                  "name": "...",
                  // app.use()로 전역으로 등록할 때 할당되는 모듈 변수
                  "var": "...",
                  // 자체 버전 번호에 따라 해당 CDN URL 찾기
                  "path": "...",
                  // 자체 버전 번호에 따라 해당 CSS CDN URL 찾기
                  "css": "...",
                },
            ],
        }),
    ],
```

# 필요한 컴포넌트를 필요할 때 Import하려면

unplugin-vue-components 플러그인을 설치하세요.

<div class="content-ad"></div>

```js
yarn add unplugin-vue-components -D
```

vite.config.js에서 설정합니다.

```js
import Components from "unplugin-vue-components/vite";

export default defineConfig({
  plugins: [
    Components({
      // 자동으로 등록되는 구성 요소 구성
      dts: true,
      resolvers: [
        (name) => {
          if (name.startsWith("Base")) {
            return { importName: name.slice(4), path: `@/components/${name}.vue` };
          }
        },
      ],
    }),
  ],
});
```

# 브라우저 캐싱

<div class="content-ad"></div>

페이지 로드 속도를 향상시키기 위해 사용자에게 정적 자원을 캐싱하는 것이 중요합니다. 서버에 요청을 다시 초기화해야 할 필요가 있는지 여부에 따라 HTTP 캐싱 규칙은 두 가지 주요 범주(강력한 캐싱 및 유효성 검사 캐싱)로 나뉩니다.

강력한 캐싱이 효과적인 경우 서버와 상호 작용할 필요가 없지만, 유효성 검사 캐싱은 효과 여부에 관계 없이 서버와 상호 작용을 필요로 합니다. 강력한 캐싱과 유효성 검사 캐싱 규칙은 동시에 존재할 수 있으며, 강력한 캐싱이 유효성 검사 캐싱보다 우선 순위를 갖습니다. 즉, 강력한 캐싱 규칙이 실행될 때, 캐시가 효과적인 경우 유효성 검사 캐싱 규칙을 실행하지 않고 바로 사용됩니다.

## 클라이언트 측 캐시 제어

클라이언트 측 캐싱 전략은 주로 다음 구현에 의존합니다:

<div class="content-ad"></div>

- 브라우저의 새로 고침 또는 다시로드 버튼;
- 브라우저의 시크릿 모드(프라이빗 모드);
- 브라우저의 앞뒤 이동;
- 브라우저 개발자 도구의 캐시 비활성화 옵션.

구현은 비교적 간단하며, 일반적으로 요청에 Pragma: no-cache 및 Cache-Control 헤더를 추가하는 것을 포함합니다. 이를 통해 서버에서 문서를 다시 확인하거나 무조건 서버에서 문서를 가져옵니다.

## 서버 측 캐시 통제

서버 측 캐싱 전략은 HTTP 응답 헤더의 해당 필드입니다:

<div class="content-ad"></div>

- 만료일
- 캐시-제어
- 마지막 수정일
- ETag

더 자세한 정보는 MDN 문서를 참조해주세요.

# Stackademic 🎓

끝까지 읽어주셔서 감사합니다. 가기 전에:

<div class="content-ad"></div>

- 작가를 응원하고 팔로우해주세요! 👏
- 팔로우하기 X | LinkedIn | YouTube | Discord
- 다른 플랫폼 방문: In Plain English | CoFeed | Differ
- 더 많은 콘텐츠는 Stackademic.com에서 확인하세요.
