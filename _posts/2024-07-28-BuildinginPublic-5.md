---
title: "공개 개발하기 - 5가지 핵심 팁"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-07-28 13:55
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Building in Public - 5"
link: "https://dev.to/liaob/building-in-public-5-2i3j"
isUpdated: true
updatedAt: 1723816976492
---



내 React 프로젝트에 localStorage 사용을 추가했어요. 앱이 클라이언트 전용이라 데이터를 지속적으로 유지할 수 있게 했더라고 좋겣어요.

localStorage에서 데이터를 불러오는 함수와 데이터를 추가하는 함수 두개를 만들었어요. localStorage의 key-value 쌍은 문자열만 가능해서 데이터를 다루기 위해 JSON.stringify()와 JSON.parse()를 사용해야 했어요:

```js
const context = {...};
localStorage.setItem('appData', JSON.stringify(context));

const localData = localStorage.getItem('appData');
if(localData){
  const parsedData = JSON.parse(localData);
  ...
}
```

처음에는 폼 제출 시 데이터 추가 함수를 호출했지만, React를 사용하고 있어서 useEffect에 넣는 게 더 쉬울 것 같아요. 기존 데이터를 종속성으로 사용하여, 데이터가 변경될 때마다 localStorage를 업데이트할 수 있었어요.

<div class="content-ad"></div>

저는 데이터를 불러오는 함수를 useEffect 내에서 호출했어요. useRef를 사용해서 첫 번째 렌더링을 판별하고, 이미 저장된 데이터를 로드하거나 데이터가 변경되면 localStorage를 업데이트하도록 설정했어요. 데이터 지속성을 위한 좋은 방법이죠! 🎉