---
title: "React 23 Webpack과 Babel을 사용하여 React 프로젝트 설정하는 방법"
description: ""
coverImage: "/assets/img/2024-07-09-React30-Project23SettingUpaReactProjectUsingWebpackandBabel_0.png"
date: 2024-07-09 16:57
ogImage:
  url: /assets/img/2024-07-09-React30-Project23SettingUpaReactProjectUsingWebpackandBabel_0.png
tag: Tech
originalTitle: "React30-Project23: Setting Up a React Project Using Webpack and Babel"
link: "https://medium.com/@saurabhnativeblog/react30-project23-setting-up-a-react-project-using-webpack-and-babel-f4ca5554dfec"
---

![이미지](/assets/img/2024-07-09-React30-Project23SettingUpaReactProjectUsingWebpackandBabel_0.png)

# 소개

React는 사용자 인터페이스를 구축하는 데 인기 있는 JavaScript 라이브러리가 되었으며, React 프로젝트를 처음부터 설정하는 것은 개발자에게 유용한 기술일 수 있습니다. 현대 JavaScript 생태계에서 중요한 도구 중 하나는 Webpack입니다. Webpack는 프로젝트의 종속성과 에셋 관리를 단순화하는 강력한 모듈 번들러입니다. 이 자습서에서는 Webpack을 사용하여 React 프로젝트를 처음부터 설정하는 과정을 안내해 드리겠습니다.

# 필수 준비물

<div class="content-ad"></div>

프로젝트 설정에 들어가기 전에, 컴퓨터에 다음 사전 요구 사항이 설치되어 있는지 확인해 주세요:

Node.js와 npm: Node.js가 설치되어 있어야 합니다. Node.js에는 npm(Node Package Manager)이 함께 제공됩니다.

# 구현

단계 1: 프로젝트 초기화
프로젝트를 위한 새 디렉토리를 만들고 터미널을 사용하여 해당 디렉토리로 이동해 주세요.

<div class="content-ad"></div>

```js
mkdir react-webpack-from-scratch
cd react-webpack-from-scratch
```

다음 명령을 실행하여 새로운 npm 프로젝트를 초기화하세요. 그리고 안내에 따라 진행하세요.

```js
npm init -y
```

단계 2: 종속 항목 설치
필요한 종속 항목을 설치하세요. React와 ReactDOM을 포함합니다.

<div class="content-ad"></div>

```js
npm install react react-dom
```

이제 Webpack과 babel과 같은 관련 도구들을 설치해 보겠습니다:

```js
npm install webpack webpack-cli webpack-dev-server babel-loader @babel/core @babel/preset-env @babel/preset-react html-webpack-plugin --save-dev
```

단계 3: Webpack 구성 만들기
프로젝트 루트에 webpack.config.js 파일을 만들어 Webpack을 구성하세요:

<div class="content-ad"></div>

```js
// webpack.config.js
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
  entry: path.join(__dirname, "src", "index.js"),
  output: {
    path: path.resolve(__dirname, "dist"),
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: path.join(__dirname, "public", "index.html"),
    }),
  ],
  module: {
    rules: [
      {
        test: /\.?js$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader",
          options: {
            presets: ["@babel/preset-env", "@babel/preset-react"],
          },
        },
      },
    ],
  },
  devServer: {
    port: 3000,
  },
};
```

웹팩 사용 설명

- Entry와 Output: webpack.config.js에서 entry 필드는 애플리케이션의 진입점을 지정하는데 사용되며(babel.js), output 필드는 번들링된 코드를 생성할 위치를 구성합니다.
- Babel Loader: module.rules 섹션은 웹팩이 자바스크립트 파일을 변환하기 위해 Babel을 사용하도록 구성합니다. 이를 통해 이전 버전의 브라우저와 최신 ECMAScript 기능을 사용할 수 있습니다.
- Webpack Dev Server: devServer 섹션은 개발 서버를 구성하여 라이브 리로딩 및 쉬운 디버깅과 같은 기능을 제공합니다.
- HTML Webpack 플러그인: html-webpack-plugin은 웹팩 플러그인 중 인기가 많으며 번들링된 JavaScript 파일을 제공하기 위한 HTML 파일을 만드는 프로세스를 간소화합니다. 생성된 스크립트 태그를 자동으로 HTML 파일에 삽입하며 React 프로젝트에 특히 유용합니다.
  html-webpack-plugin은 번들된 JavaScript를 제공하기 위해 dist 디렉토리에 HTML 파일을 자동으로 생성합니다. 이는 설정을 간소화하고 React 애플리케이션이 올바르게 HTML 파일에 포함되며 필요한 종속성이 자동으로 포함되도록 보장합니다.

CSS 지원 추가:

<div class="content-ad"></div>

CSS를 처리하기 위한 적절한 로더를 사용하려면 웹팩 설정과 Babel에 CSS 지원을 활성화해야 합니다. 아래는 style-loader와 css-loader를 사용하여 CSS 파일을 처리하는 지원을 포함한 업데이트된 웹팩 구성입니다:

필요한 로더를 설치하세요:

```js
npm install style-loader css-loader --save-dev
```

웹팩 설정 파일(webpack.config.js)을 업데이트하세요:

<div class="content-ad"></div>

```js
...
module: {
    rules: [
        {
            test: /\.js$/,
            exclude: /node_modules/,
            use: {
                loader: 'babel-loader',
                options: {
                    presets: ['@babel/preset-env', '@babel/preset-react'],
                },
            },
        },
        {
            test: /\.css$/,
            use: ['style-loader', 'css-loader'],
        },
    ],
},
...
```

웹팩 구성에서 이미지 지원을 추가하려면 `file-loader` 또는 `url-loader`를 사용하여 이미지 파일을 처리할 수도 있습니다.

단계 4: Babel 구성
Babel을 구성하려면 프로젝트 루트에 .babelrc 파일을 만드세요:

```js
// .babelrc
{
    "presets": ["@babel/preset-env", "@babel/preset-react"]
}
```

<div class="content-ad"></div>

이 구성은 Babel이 모던 JavaScript와 React 모두를 위한 프리셋을 사용하도록 지시합니다.

단계 5: React 컴포넌트 생성

src/App.js에서 간단한 React 컴포넌트를 생성하세요:

```js
// src/App.js
import React from "react";

const App = () => {
  return (
    <div>
      <h1>Hello, React with Webpack!</h1>
    </div>
  );
};

export default App;
```

<div class="content-ad"></div>

### 단계 6: 진입 지점

React 컴포넌트를 렌더링하는 src/index.js 파일을 생성하세요:

```js
// src/index.js
import React from "react";
import { createRoot } from "react-dom/client";
import App from "./App";

const domNode = document.getElementById("root");
const root = createRoot(domNode);

root.render(<App />);
```

### 단계 7: HTML 템플릿

public 디렉토리에 간단한 index.html 파일을 생성하세요:

```html
<!-- public/index.html -->
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>React with Webpack</title>
  </head>
  <body>
    <div id="root"></div>
  </body>
</html>
```

<div class="content-ad"></div>

단계 8: NPM 스크립트
`package.json` 파일의 `scripts` 섹션을 다음과 같이 업데이트하세요.

```json
"scripts": {
  "start": "webpack serve --mode development --open",
  "build": "webpack --mode production"
}
```

정의된 NPM 스크립트 (start 및 build)를 사용하면 개발 서버를 실행하고 제품용 빌드를 손쉽게 만들 수 있습니다.

단계 9: 애플리케이션 실행

<div class="content-ad"></div>

터미널에서 아래 명령을 실행하여 애플리케이션을 시작하세요:

```js
npm start
```

이제 브라우저에서 앱을 확인할 수 있을 거에요.

참고용 소스코드:
Github

<div class="content-ad"></div>

# 결론

React 프로젝트를 웹팩을 사용하여 처음부터 설정하는 것은 웹 개발자에게 필수적인 기술입니다. 올바른 구성을 통해 현대적인 JavaScript 기능, React의 선언적 구문, 그리고 Webpack의 강력한 번들링 기능을 활용할 수 있습니다. 이 자습서는 스트리밍된 개발 워크플로우로 더 복잡한 React 응용 프로그램을 구축하는 튼튼한 기초 역할을 합니다. 즐거운 코딩 되세요!

내 유튜브 채널에서 더 많은 콘텐츠를 확인하세요:
SaurabhNative-Youtube

시리즈의 다음 파트에서는 리액트 테스팅 라이브러리를 설정하고 사용하여 리액트에서 테스트 케이스를 작성하는 방법을 알아보겠습니다.

<div class="content-ad"></div>

리액트30-파트24
