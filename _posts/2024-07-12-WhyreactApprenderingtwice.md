---
title: "리액트 앱이 두 번 렌더링 되는 이유"
description: ""
coverImage: "/it-shar.ing/assets/no-image.jpg"
date: 2024-07-12 19:01
ogImage:
  url: /it-shar.ing/assets/no-image.jpg
tag: Tech
originalTitle: "Why react App rendering twice?"
link: "https://medium.com/@chanduthedev/why-react-app-rendering-twice-e31339ed9461"
---

React.StrictMode을 애플리케이션에서 사용할 때, 개발 모드에서 React는 두 번 렌더링됩니다. 이는 부작용이 없는지 확인하기 위한 것입니다. 그러나 프로덕션 환경에서는 한 번만 렌더링됩니다.

```js
// index.js
// 이는 개발 환경에서 모든 것을 두 번 렌더링합니다
<React.StrictMode>
    <App />
</React.StrictMode>

//app.js
function App() {

  useEffect(() => {
    console.log(`useEffect 안에 있음.`);
  });

  return (
    <div>
      {console.log("렌더링 중")}
      <h1>이것은 React 앱입니다</h1>
    </div>
  );
}



// 콘솔 로그가 두 번 표시됨
렌더링 중
렌더링 중
useEffect 안에 있음
useEffect 안에 있음
```

개발 환경에서 두 번 렌더링되는 것을 피하고 싶다면, index.js 파일에서 React.StrictMode를 제거하세요.

```js
// index.js
// 개발 환경에서만 한 번만 렌더링됨
<App />

//app.js
function App() {

  useEffect(() => {
    console.log(`useEffect 안에 있음.`);
  });

  return (
    <div>
      {console.log("렌더링 중")}
      <h1>이것은 React 앱입니다</h1>
    </div>
  );
}



// 콘솔 로그가 한 번만 표시됨
렌더링 중
useEffect 안에 있음
```

<div class="content-ad"></div>

앱이 두 번 렌더링되는 것과 useEffect() 훅이 두 번 렌더링되는 것을 혼동하지 말아 주세요. useEffect() 렌더링은 앱 컴포넌트 라이프사이클 메소드를 기반으로합니다. 나중에 useEffect() 훅이 어떻게 작동하는지에 대해 별도의 기사를 작성할 예정입니다.
