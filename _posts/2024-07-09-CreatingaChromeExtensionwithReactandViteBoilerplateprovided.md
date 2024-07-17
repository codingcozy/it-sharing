---
title: "React와 Vite로 크롬 확장 프로그램 만들기 보일러플레이트 제공"
description: ""
coverImage: "/assets/img/2024-07-09-CreatingaChromeExtensionwithReactandViteBoilerplateprovided_0.png"
date: 2024-07-09 13:46
ogImage:
  url: /assets/img/2024-07-09-CreatingaChromeExtensionwithReactandViteBoilerplateprovided_0.png
tag: Tech
originalTitle: "Creating a Chrome Extension with React and Vite (Boilerplate provided)"
link: "https://medium.com/@5tigerjelly/creating-a-chrome-extension-with-react-and-vite-boilerplate-provided-db3d14473bf6"
---

## 크롬 익스텐션을 만드는 것이 이렇게 복잡할 필요는 없어요

요약: Boilerplate은 여기에 있어요 https://github.com/5tigerjelly/chrome-extension-react-template

이 글에서는 제가 배달하고 싶은 내용은, 바닐라 HTML, JS 및 CSS를 사용하여 크롬 익스텐션을 만드는 과정부터 React로 마이그레이션하고 Vite를 사용하여 컴파일하는 과정까지 공유하겠습니다. 이 과정은 도전과정, 배운 교훈들로 가득 차 있었고, 결국 비슷한 상황에 있는 다른 사람들에게 도움이 될 것으로 기대하는 솔루션을 찾을 수 있었어요.

<img src="/assets/img/2024-07-09-CreatingaChromeExtensionwithReactandViteBoilerplateprovided_0.png" />

<div class="content-ad"></div>

## HTML, JS,와 CSS로 시작하기

내가 처음 시작할 때, 공식 구글 문서를 따랐어. 그 문서에는 Vanilla HTML, JS, 그리고 CSS로 된 예제가 있었어. 이 방법은 기본 기능을 빠르게 구현하는 데 좋았어. 문서가 명확했고, 그를 통해 Chrome 확장 프로그램의 기초를 이해할 수 있었어. 나는 원하는 작업을 수행하는 간단한 확장 프로그램을 만들 수 있었어. 그러나 더 고급 기능에 대해 깊이 파헤치면서, Vanilla HTML, JS,와 CSS로 작업하는 것에는 유지 관리 및 확장성 측면에서 한계가 있음을 깨달았어.

한 가지 중요한 도전 과제는 최근 Manifest 버전 2에서 버전 3으로의 이관을 특히 manifest 파일을 관리하는 것이었어. Manifest 파일은 Chrome 확장 프로그램의 권한, 백그라운드 스크립트 및 기타 설정을 정의하는 데 중요해. 버전 3로의 전환으로 많은 부분이 호환되지 않는 변경 사항이 도입되었기 때문에, 많은 블로그 및 스택 오버플로 답변이 이전 버전을 참조하여 관련 솔루션을 찾기가 어려웠어. 나는 상당히 많은 시간을 내 manifest 파일을 업데이트하고 새로운 요구사항을 충족시키도록 해야 했어. 이러한 장애물에도 불구하고, 대부분의 상황을 다루고 프로젝트를 진행하는 데 성공했어.

![Chrome Extension](/assets/img/2024-07-09-CreatingaChromeExtensionwithReactandViteBoilerplateprovided_1.png)

<div class="content-ad"></div>

## 제 3자 도구 추가하여 인증 및 저장 공간 설정하기

인증 기능을 구현해야 했고, 처음에는 Supabase를 사용하는 것을 고려했습니다. Supabase는 프로젝트에 빠르게 인증 및 기타 백엔드 서비스를 추가할 수 있는 훌륭한 도구입니다. 그러나 필요한 몇 가지 사례는 지원되지 않았고, 가격이 저의 편에 맞지 않았습니다. 예를 들어, 더 세분화된 액세스 제어와 Supabase에서 고가로 제공되는 파일 저장 공간이 필요했습니다. 그래서 다른 옵션을 찾아보게 되었습니다. Supabase에 대한 제 경험에 관해 나중에 자세히 다룰 예정이며, Supabase에 대해 알아보는 다른 분들에게 도움이 될 수 있는 가치 있는 경험이었습니다.

과거 프로젝트에서 Firebase에 익숙했기 때문에 Firebase의 웹 SDK를 사용하여 인증을 구현하기로 결정했습니다. Firebase는 인증, 데이터베이스 관리 및 기타 다양한 기능을 제공하는 강력한 도구 모음을 제공합니다. 그러나 이 결정으로 예상치 못한 문제가 발생했습니다. Firebase 웹 SDK가 순수 JS를 잘 지원하지 않아서, 그리고 Chrome 확장 프로그램에서 내용 스크립트 로딩에 엄격한 제한이 있어서 해결할 수 없는 문제에 직면했습니다. 주된 문제는 Firebase가 초기화하고 사용자를 인증하는 방식이 Chrome 확장 프로그램에서 시행되는 콘텐츠 보안 정책과 충돌되었던 것입니다. Firebase 라이브러리를 로컬로 다운로드해서 실행하는 등 여러 가지 접근 방법을 시도했지만 아무것도 도움이 되지 않았습니다. 이는 상당한 타격이었고 괴로운 경험이었습니다.

## 알겠어요, 리액트를 사용하겠습니다.

<div class="content-ad"></div>

하루 종일 문제 해결을 하다가 결국 프로젝트를 React로 마이그레이션하기로 결정했어요. React는 사용자 인터페이스를 구축하기 위한 강력한 라이브러리로, 이전 프로젝트에서 성공적으로 활용한 경험이 있어요. ChatGPT를 사용하여 기존의 바닐라 JS 코드를 React로 번역했어요. 이 전환으로 인해 새로운 문제가 발생했는데, 특히 앱을 구축하고 실행하는 부분에서 어려움을 겪었어요. React의 컴포넌트 기반 아키텍처에 맞게 많은 코드를 리팩토링해야 했고, 상태 관리와 라이프사이클 메소드와 관련된 문제들이 해결되는 데 시간이 걸렸어요. 문제를 해결한 후에는 React 앱을 Chrome용으로 컴파일하기 위해 Webpack을 사용했어요. 아쉽게도, 디버깅하기 어려운 지속적인 컴파일 문제가 발생했지요.

## Vite가 도와줬어요

조사를 통해 Webpack을 사용할 때 Chrome 확장 프로그램용으로 컴파일할 때 많은 개발자들이 유사한 문제에 직면한다는 것을 알게 됐어요. Webpack의 복잡성과 구성 오버헤드로 인해 모든 것을 매끄럽게 작동시키는 것이 어려웠어요. 그때 Vite를 발견하게 됐죠. Vite는 더 빠른 빌드와 더 간단한 구성을 제공하는 차세대 프론트엔드 도구로, Webpack에 비해 더 쉬운 구성을 제공해요. Vite로 모든 문제를 해결했고, 바로 사용할 수 있었어요. 그들의 웹사이트 안내를 따라가니, 몇 분 내에 모든 것을 실행할 수 있었어요. 설정 과정이 무척 순조롭었고, 빌드 도구와 싸우는 대신 기능 개발에 집중할 수 있었어요. ChatGPT가 Chrome 확장 프로그램으로 앱을 실행하는 데 필요한 최소한의 코드를 생성하는 데 도움이 되었어요.

새 Vite 프로젝트를 생성하세요

<div class="content-ad"></div>

```js
npm create vite@latest my-chrome-extension --template react-ts
cd my-chrome-extension
npm install vite-plugin-static-copy
```

manifest.json 파일을 생성하세요. 파일은 'public' 폴더에 넣어주세요.

```js
{
  "manifest_version": 3,
  "name": "React Chrome Extension",
  "version": "1.0.0",
  "description": "A simple React app as a Chrome extension",
  "action": {
    "default_popup": "index.html"
  },
  "permissions": []
}
```

vite.config.ts 파일을 생성하세요.

<div class="content-ad"></div>

```js
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";
import { viteStaticCopy } from "vite-plugin-static-copy";

export default defineConfig({
  plugins: [
    react(),
    viteStaticCopy({
      targets: [
        {
          src: "public/manifest.json",
          dest: ".",
        },
      ],
    }),
  ],
  build: {
    outDir: "build",
    rollupOptions: {
      input: {
        main: "./index.html",
      },
    },
  },
});
```

Vite를 사용하여 React 앱을 빌드하세요.

```js
npm run build
```

빌드 성공 후, 빌드된 필요한 파일이 포함된 새 디렉토리인 build/가 생성됩니다. 빌드가 완료되면 chrome://extensions/로 이동하여 '개발자 모드'와 '압축 해제된 확장 프로그램 로드'를 활성화하고 방금 생성된 빌드 디렉토리를 선택해주세요!

<div class="content-ad"></div>

<img src="/assets/img/2024-07-09-CreatingaChromeExtensionwithReactandViteBoilerplateprovided_2.png" />

## 결론

앞으로 개발자들이 같은 실수를 반복하지 않도록 도와주기 위해 누구나 포크하고 프로젝트를 빌드할 수 있는 템플릿 리포지토리를 만들었습니다. 이 템플릿에는 React와 Vite를 사용한 Chrome 확장 프로그램을 위한 기본 설정과 필요한 구성 요소 및 스크립트가 포함되어 있습니다. 본 템플릿을 제공함으로써 다른 사람들이 내가 경험한 시간과 좌절을 더 이상 느끼지 않길 희망합니다. 해당 리포지토리는 잘 문서화되어 있으며 시작하는 데 필요한 단계별 지침이 포함되어 있습니다. 이 블로그 글이 도움이 되기를 바랍니다.

즐거운 코딩 :)
