---
title: "웹 개발자를 위한 숨겨진 보석 잘 알려지지 않은 HTML 태그 탐구"
description: ""
coverImage: "/it-shar.ing/assets/no-image.jpg"
date: 2024-07-09 08:09
ogImage:
  url: /it-shar.ing/assets/no-image.jpg
tag: Tech
originalTitle: "Exploring Lesser-Known HTML Tags: Hidden Gems for Web Developers"
link: "https://dev.to/hallowshaw/exploring-lesser-known-html-tags-hidden-gems-for-web-developers-26fm"
---

웹 개발자로서, 우리는 종종 친숙한 HTML 태그 세트에 의존하여 웹 페이지를 구축합니다. 그러나 HTML은 많은 태그를 가지고 있는 풍부한 언어이며, 이러한 알려지지 않은 HTML 태그를 통해 웹 프로젝트를 독특하고 강력하게 향상시킬 수 있습니다. 이 블로그 포스트에서는 이러한 알려지지 않은 HTML 태그들을 자세히 살펴보고, 그 유틸리티를 보여줌과 동시에 어떻게 사용하는지 예시를 제공할 것입니다.

- `details` 및 `summary`

이 태그는 사용자가 열고 닫을 수 있는 드롭다운 위젯을 생성합니다. `summary` 태그와 함께 사용되면 클릭하여 콘텐츠를 표시하거나 숨길 수 있는 제목을 제공합니다.

```js
<details>
  <summary>추가 정보</summary>
  <p>이 섹션에는 기본적으로 숨겨진 추가 정보가 들어 있습니다.</p>
</details>
```

<div class="content-ad"></div>

- `dialog`

이 태그는 대화 상자 또는 창을 정의하는 데 사용되며 JavaScript에 크게 의존하지 않고 모달 및 팝업을 쉽게 생성할 수 있습니다.

```js
<dialog id="myDialog">
  <p>이것은 대화 상자입니다.</p>
  <button onclick="document.getElementById('myDialog').close()">닫기</button>
</dialog>
<button onclick="document.getElementById('myDialog').showModal()">대화 상자 열기</button>
```

- `meter`

<div class="content-ad"></div>

다음은 Markdown 형식으로 표현된 내용입니다.

```js
<label for="diskUsage">Disk usage:</label>
<meter id="diskUsage" value="0.6" min="0" max="1">60%</meter>
```

- `progress`

`meter`과 유사한 이 태그는 다운로드나 파일 업로드와 같은 작업의 완료 진행 상황을 표시합니다.

<div class="content-ad"></div>

<label for="file">Downloading file:</label>
<progress id="file" value="32" max="100">32%</progress>

- `template`

태그는 페이지가 로드될 때 렌더링되지 않고 JavaScript에 의해 복제 및 삽입될 수 있는 HTML 단편을 선언하는 데 사용됩니다.

<template id="myTemplate">
  <div class="myClass">This is a template content.</div>
</template>

<script>
  const template = document.getElementById('myTemplate').content.cloneNode(true);
  document.body.appendChild(template);
</script>

<div class="content-ad"></div>

- `datalist`

이 태그는 입력 요소에 자동완성 기능을 제공하여 사용자에게 사전 정의된 옵션 목록을 제공합니다.

```js
<label for="browsers">브라우저를 선택하세요:</label>
<input list="browsers" id="browser" name="browser">
<datalist id="browsers">
  <option value="Chrome">
  <option value="Firefox">
  <option value="Safari">
  <option value="Edge">
  <option value="Opera">
</datalist>
```

- `output`

<div class="content-ad"></div>

여기 표입니다.

| 태그   | 설명                                                                     |
| ------ | ------------------------------------------------------------------------ |
| result | 계산 결과나 사용자 작업의 결과를 나타냅니다.                             |
| abbr   | 약어나 머리글자를 정의하는 데 사용되며 호버 시 확장된 설명을 제공합니다. |

<div class="content-ad"></div>

The WHO was founded in 1948.

- **time**
  The `time` tag represents a specific period in time, or a time on a 24-hour clock.

The event will start at 14:00 on July 10, 2024.

<div class="content-ad"></div>

- `fieldset`와 `legend`

이 태그는 양식 내에서 관련 요소를 그룹화하는 데 사용되며, `fieldset`에는 캡션을 제공합니다.

```js
<form>
  <fieldset>
    <legend>개인 정보</legend>
    <label for="name">이름:</label>
    <input type="text" id="name" name="name">
    <label for="email">이메일:</label>
    <input type="email" id="email" name="email">
  </fieldset>
</form>
```

- `samp`

<div class="content-ad"></div>

해당 태그는 컴퓨터 프로그램의 샘플 출력을 정의하는 데 사용됩니다.

```js
<p>
  계산 결과는: <samp>42</samp>입니다.
</p>
```

- `var`

해당 태그는 수학 표현이나 프로그래밍 문맥에서 변수를 정의하는 데 사용됩니다.

<div class="content-ad"></div>

```js
<p>
  이 방정식은 <var>x</var> = <var>y</var> + 2 입니다.
</p>
```

- `address`

이 태그는 문서나 기사의 작성자 또는 소유자의 연락처 정보를 정의하는 데 사용됩니다.

```js
<address>
  John Doe가 작성하였습니다.<br>
  방문해 보세요:<br>
  Example.com<br>
  Box 564, Disneyland<br>
  미국
</address>
```
