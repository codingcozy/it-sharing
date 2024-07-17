---
title: "React  Nextjs  Tailwind로 Github Pages에 배포하는 방법"
description: ""
coverImage: "/assets/img/2024-07-07-ReactNextjstailwindGithubpages_0.png"
date: 2024-07-07 19:02
ogImage:
  url: /assets/img/2024-07-07-ReactNextjstailwindGithubpages_0.png
tag: Tech
originalTitle: "React + Next.js + tailwind Github pages"
link: "https://medium.com/@sidcode/react-next-js-tailwind-github-pages-ca90e386ae93"
---

# 단계 1. Next.js 프로젝트 설정하기

1–0. Next.js 프로젝트 초기화:

```js
$ npx create-next-app@latest
```

1–1. 도메인을 설정하세요.

<div class="content-ad"></div>

```bash
$ cd my_project_directory
$ echo -e 'sidcode.me' > public/CNAME
$ cat public/CNAME
sidcode.me
```

1–2. next.config.js 또는 next.config.mjs 업데이트

```js
/** @type {import('next').NextConfig} */
const nextConfig = {
  output: "export",
  images: {
    unoptimized: true,
  },
  reactStrictMode: true,
  assetPrefix: ".",
};
```

```js
export default nextConfig;
```

<div class="content-ad"></div>

1–3. postcss.config.js 또는 postcss.config.mjs 업데이트

```js
/** @type {import('postcss-load-config').Config} */
const config = {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  },
};
```

```js
export default config;
```

1–3–1. autoprefixer 설치

<div class="content-ad"></div>

```js
$ npm install autoprefixer --save-dev
```

1–4. Github

```js
git init
git add .
git commit -m "Initial commit"
git remote add origin <https://<id>:<token>@github.com/><username>/<repo>.git
git push -u origin master
```

# Step 2: GitHub Pages로 배포하기

<div class="content-ad"></div>

2–1. gh-pages 설치: 앱을 배포하는 데 도움을 주는 gh-pages 패키지를 설치해보세요:

```js
$ npm install --save-dev gh-pages
```

2–2. package.json 업데이트: 다음 스크립트를 package.json에 추가해주세요:

```js
"scripts": {
    "dev": "next dev",
    "start": "next start",
    "lint": "next lint",
    "build": "next build",
    "deploy": "touch out/.nojekyll && gh-pages -d out -t true",
    "deploy-npm": "npm run build && npm run deploy"
  },
```

<div class="content-ad"></div>

```js
//    "deploy": "gh-pages -d out -f" // 한 번 강제로 / 안 될 때 1회성
```

2–3. 앱을 배포하세요: 다음 명령어를 실행하여 앱을 GitHub Pages에 배포하세요:

```js
$ npm run deploy-npm
```

이렇게요!

<div class="content-ad"></div>

![React Next.js Tailwind GitHub Pages](/assets/img/2024-07-07-ReactNextjstailwindGithubpages_0.png)
