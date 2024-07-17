---
title: "React에서 이미지 추가하는 방법"
description: ""
coverImage: "/assets/img/2024-07-09-HowToAddImagesInReact_0.png"
date: 2024-07-09 13:37
ogImage:
  url: /assets/img/2024-07-09-HowToAddImagesInReact_0.png
tag: Tech
originalTitle: "How To Add Images In React"
link: "https://medium.com/javascript-in-plain-english/how-to-add-images-in-react-50eb7757f28b"
---

이 게시물에서는 React에서 이미지를 추가하는 일반적인 패턴을 사용하는 방법을 보여줍니다.

먼저, 다음과 같이 Vite를 사용하여 React를 설치합니다:

```js
npm create vite react-images
```

여기서 react-images는 React 앱의 이름입니다. Vite를 설치해야 하는 경우 npm install -D vite을 실행하면됩니다. 그리고 이제 준비가 끝났습니다.

<div class="content-ad"></div>

npm run dev을 실행해서 React 앱을 구동해보세요.

![이미지](/assets/img/2024-07-09-HowToAddImagesInReact_0.png)

# React에 이미지 추가하는 방법 — Import

## Public에서 이미지 가져오기

<div class="content-ad"></div>

App.tsx 파일을 열어보시면 import 키워드를 사용하여 이미지를 가져올 수 있다는 것을 확인할 수 있어요.

이 경우, vite.svg 이미지는 public 폴더 안에 있어요.

```js
import viteLogo from '/vite.svg'

...

function App() {
  return (
    <img src={viteLogo} className="logo" alt="Vite logo" />
  )
}
```

public 폴더 안의 모든 것은 앱에서 사용할 수 있으므로, 이미지 태그의 src 속성을 파일의 올바른 위치로 지정할 수도 있어요.

<div class="content-ad"></div>

아래는 또 다른 예제입니다:

```js
import viteLogo from '/vite.svg'…
```
