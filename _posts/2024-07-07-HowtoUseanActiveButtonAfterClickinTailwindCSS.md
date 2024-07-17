---
title: "클릭 후 활성화된 버튼을 Tailwind CSS로 설정하는 방법"
description: ""
coverImage: "/assets/img/2024-07-07-HowtoUseanActiveButtonAfterClickinTailwindCSS_0.png"
date: 2024-07-07 12:17
ogImage:
  url: /assets/img/2024-07-07-HowtoUseanActiveButtonAfterClickinTailwindCSS_0.png
tag: Tech
originalTitle: "How to Use an Active Button After Click in Tailwind CSS"
link: "https://dev.to/saim_ansari/how-to-use-an-active-button-after-click-in-tailwind-css-2g8d"
---

Tailwind CSS에서 클릭한 후 활성 버튼 상태를 사용하려면 활성 유틸리티 클래스를 활용할 수 있습니다. 버튼을 클릭하거나 활성화되었을 때 활성 클래스가 적용됩니다.

```js
<button class="rounded bg-blue-500 px-4 py-2 text-white hover:bg-blue-700 active:bg-blue-800">Click me</button>
```

![이미지](/assets/img/2024-07-07-HowtoUseanActiveButtonAfterClickinTailwindCSS_0.png)

Tailwind CSS 클래스 active:bg-blue-800을 이용하여 클릭 후 활성화되는 버튼을 만들고, focus:outline-none 및 focus:ring focus:border-blue-300을 함께 사용하세요.

<div class="content-ad"></div>

```js
<button class="bg-blue-500 hover:bg-blue-700 text-white py-2 px-4 rounded focus:outline-none focus:ring focus:border-blue-300 active:bg-blue-800">
  Active Button
</button>
```

<img src="/assets/img/2024-07-07-HowtoUseanActiveButtonAfterClickinTailwindCSS_1.png" />

```js
<div>
    <button class="rounded border border-blue-500 px-4 py-2 font-semibold text-blue-500 hover:bg-blue-500 hover:text-white focus:border-blue-300 focus:outline-none focus:ring">Outlined Active Button</button>
</div>
<div>
    <button class="rounded bg-blue-500 px-4 py-2 text-white shadow-md hover:bg-blue-700 focus:border-blue-300 focus:outline-none focus:ring active:bg-blue-800">Primary Button</button>
</div>
<div>
    <button class="rounded bg-gradient-to-r from-purple-400 to-blue-500 px-4 py-2 text-white hover:from-purple-500 hover:to-blue-600 focus:border-blue-300 focus:outline-none focus:ring active:bg-blue-800">
        <svg xmlns="http://www.w3.org/2000/svg" class="inline-block h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
            <path fill-rule="evenodd" d="M10 2a1 1 0 00-1 1v5H5a1 1 0 100 2h4v5a1 1 0 102 0v-5h4a1 1 0 100-2h-4V3a1 1 0 00-1-1z" clip-rule="evenodd" />
        </svg>
        Button with Icon
    </button>
</div>
<img src="/assets/img/2024-07-07-HowtoUseanActiveButtonAfterClickinTailwindCSS_2.png" />
```
