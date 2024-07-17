---
title: "여러 탭에서 sessionStorage로 데이터를 공유할 수 있을까"
description: ""
coverImage: "/assets/img/2024-07-09-CansessionStorageShareDataAcrossMultipleTabs_0.png"
date: 2024-07-09 13:14
ogImage:
  url: /assets/img/2024-07-09-CansessionStorageShareDataAcrossMultipleTabs_0.png
tag: Tech
originalTitle: "Can sessionStorage Share Data Across Multiple Tabs?"
link: "https://medium.com/@kxming/can-sessionstorage-share-data-across-multiple-tabs-6614c162bbb6"
---

![image](/assets/img/2024-07-09-CansessionStorageShareDataAcrossMultipleTabs_0.png)

localStorage과 sessionStorage은 종종 함께 언급되지만, 그들 사이에는 무엇이 다른가요?

localStorage는 동일한 웹사이트의 서로 다른 탭 간에 데이터를 공유할 수 있습니다. 예시를 보여드리겠습니다:

```js
// 첫 번째 탭에 데이터를 저장할 수 있습니다
localStorage.setItem("name", "fatfish");

// 다른 탭에서 이 데이터를 읽을 수 있습니다
localStorage.getItem("name"); // fatfish
```

<div class="content-ad"></div>

## sessionStorage은 어떻게 됩니까?

MDN에서 설명했습니다:
