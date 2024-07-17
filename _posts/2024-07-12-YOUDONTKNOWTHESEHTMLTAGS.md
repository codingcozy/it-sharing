---
title: "모두가 모르는 HTML 태그 10가지 "
description: ""
coverImage: "/it-shar.ing/assets/no-image.jpg"
date: 2024-07-12 18:20
ogImage:
  url: /it-shar.ing/assets/no-image.jpg
tag: Tech
originalTitle: "YOU DON'T KNOW THESE HTML TAGS! 🫣"
link: "https://dev.to/mb337/you-dont-know-these-html-tags-1629"
---

HTML 작업을 할 때 대부분의 개발자들은 `div`, `span`, 그리고 `a`와 같은 기본 태그에 익숙합니다. 하지만 HTML에는 특정 상황에서 매우 유용한 다양한 잘 알려지지 않은 태그들이 포함되어 있습니다.

다음은 도움이 될 수 있는 일부 자주 사용되지 않는 HTML 태그들입니다:

## `abbr`

`abbr` 태그는 약어나 머리글을 정의하는 데 사용되며 그 의미에 대한 명확한 정보를 제공합니다.

<div class="content-ad"></div>

```js
<abbr title="하이퍼텍스트 마크업 언어">HTML</abbr>
```

이 예제에서 "HTML" 위로 마우스를 올리면 "하이퍼텍스트 마크업 언어"가 표시됩니다.

## `address`

`address` 태그는 문서나 글의 저자의 연락처 정보를 정의하는 데 사용됩니다.

<div class="content-ad"></div>

```js
<address>
  Written by <a href="mailto:webmaster@example.com">John Doe</a>.<br>
  Visit us at:<br>
  Example.com<br>
  Box 564, Disneyland<br>
  USA
</address>
```

이 태그는 구조화된 연락처 정보를 제공하는 데 유용합니다.

## `bdo`

`bdo` 태그는 "양방향 재정의"를 의미하며 텍스트 방향을 변경하는 데 사용됩니다.

<div class="content-ad"></div>

```js
<bdo dir="rtl">이 텍스트는 오른쪽에서 왼쪽으로 쓰여질 것입니다</bdo>
```

이 태그는 오른쪽에서 왼쪽으로 읽는 언어에 특히 유용합니다.

## `datalist`

`datalist` 태그는 입력 필드에 미리 정의된 옵션 목록을 제공합니다.

<div class="content-ad"></div>

```js
<input list="browsers" name="browser">
<datalist id="browsers">
  <option value="Chrome">
  <option value="Firefox">
  <option value="Internet Explorer">
  <option value="Opera">
  <option value="Safari">
</datalist>
```

## `details`

`details` 태그는 추가적인 상호작용 가능한 세부 정보를 포함할 수 있는 접을 수 있는 상자를 만드는 데 사용됩니다.

```js
<details>
  <summary>자세한 정보</summary>
  <p>여기에는 요약을 클릭하면 볼 수 있는 추가 정보가 있습니다.</p>
</details>
```

<div class="content-ad"></div>

이 태그는 웹페이지에서 확장 가능한 섹션을 만드는 데 유용합니다.

## `meter`

`meter` 태그는 디스크 사용량과 같이 알려진 범위 내에서 스칼라 측정 값을 나타냅니다.

```js
<meter value="2" min="0" max="10">
  10 중의 2
</meter>
```

<div class="content-ad"></div>

이것은 설정한 범위 내에서 진행 상황이나 레벨을 표시하는 데 유용합니다.
