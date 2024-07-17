---
title: "HTML Table 태그 완벽 가이드 기초부터 고급까지"
description: ""
coverImage: "/it-shar.ing/assets/no-image.jpg"
date: 2024-07-07 20:45
ogImage:
  url: /it-shar.ing/assets/no-image.jpg
tag: Tech
originalTitle: "HTML Table tag A to Z"
link: "https://dev.to/ridoy_hasan/html-tabl-tag-a-to-z-13an"
---

### HTML Tables: 포괄적인 가이드

표는 탭형식 데이터를 표시하는 데 사용되는 HTML의 기본 요소입니다. 이 기사에서는 효과적으로 표를 생성하고 스타일링하는 방법을 살펴보며 예제와 모범 사례를 제시합니다.

#### 1. 기본 테이블 구조

기본 HTML 테이블은 `table` 요소를 사용하여 생성됩니다. 표 안에서 행은 `tr` 태그를 사용하여 정의하고, 각 행 내의 셀은 `td` 태그를 사용하여 정의됩니다. 표머리는 `th` 태그를 사용하여 정의할 수 있습니다.

<div class="content-ad"></div>

예시: 기본 테이블

```js
<!DOCTYPE html>
<html>
<head>
    <title>Basic Table Example</title>
</head>
<body>
    <h1>기본 HTML 테이블</h1>
    | Header 1 | Header 2 | Header 3 |
    |----------|----------|----------|
    | Row 1, Cell 1 | Row 1, Cell 2 | Row 1, Cell 3 |
    | Row 2, Cell 1 | Row 2, Cell 2 | Row 2, Cell 3 |
</body>
</html>
```

결과:

기본 HTML 테이블

<div class="content-ad"></div>

이 예시에서 표는 테두리가 있으며, 각각이 세 개의 셀을 포함한 두 개의 데이터 행이 있습니다.

### 2. 캡션 추가

표 캡션은 표의 제목이나 설명을 제공하며, `caption` 태그를 사용하여 정의됩니다.

예시: 캡션을 포함한 표

<div class="content-ad"></div>

```js
<!DOCTYPE html>
<html>
<head>
    <title>Table with Caption Example</title>
</head>
<body>
    <h1>HTML Table with Caption</h1>
    <table border="1">
        <caption>월별 매출 데이터</caption>
        <tr>
            <th>월</th>
            <th>매출</th>
        </tr>
        <tr>
            <td>1월</td>
            <td>$1000</td>
        </tr>
        <tr>
            <td>2월</td>
            <td>$1200</td>
        </tr>
    </table>
</body>
</html>
```

출력:

HTML Table with Caption

월별 매출 데이터

<div class="content-ad"></div>

이 예시에서 테이블에는 데이터를 설명하는 캡션이 포함되어 있습니다.

### 3. 테이블 헤더 및 푸터

테이블은 각각 `thead` 및 `tfoot` 태그를 사용하여 헤더와 푸터를 정의할 수 있습니다. 이러한 요소들은 테이블 데이터를 구성하는 데 도움이 됩니다.

예시: 헤더와 푸터가 있는 테이블

<div class="content-ad"></div>

```js
<!DOCTYPE html>
<html>
<head>
    <title>Table with Headers and Footers Example</title>
</head>
<body>
    <h1>HTML Table with Headers and Footers</h1>
    <table border="1">
        <thead>
            <tr>
                <th>Product</th>
                <th>Price</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>Apples</td>
                <td>$1.00</td>
            </tr>
            <tr>
                <td>Oranges</td>
                <td>$0.80</td>
            </tr>
        </tbody>
        <tfoot>
            <tr>
                <td>Total</td>
                <td>$1.80</td>
            </tr>
        </tfoot>
    </table>
</body>
</html>
```

결과:

HTML 헤더와 풋터를 포함한 테이블입니다.

이 예제에서는 제품 이름과 가격을 나타내는 헤더가 포함된 테이블이 있습니다. 제품 데이터가 있는 본문과 총 가격이 표시된 풋터가 있습니다.

<div class="content-ad"></div>

#### 4. 셀 병합

셀을 병합하려면 colspan 및 rowspan 속성을 사용하여 여러 열이나 행을 포함하도록 설정할 수 있습니다.

예시: 병합된 셀이 있는 테이블

```js
<!DOCTYPE html>
<html>
<head>
    <title>Table with Merged Cells Example</title>
</head>
<body>
    <h1>HTML Table with Merged Cells</h1>
    <table border="1">
        <tr>
            <th>Item</th>
            <th>Details</th>
        </tr>
        <tr>
            <td rowspan="2">과일</td>
            <td>사과</td>
        </tr>
        <tr>
            <td>오렌지</td>
        </tr>
        <tr>
            <td colspan="2">총 항목: 2개</td>
        </tr>
    </table>
</body>
</html>
```

<div class="content-ad"></div>

출력:

HTML 테이블에 병합된 셀이 있습니다.

이 예시에서는 두 번째 행의 첫 번째 셀이 두 행을 횡배로, 그리고 마지막 행의 셀이 두 열을 세로로 가로지르는 것이 있습니다.

#### HTML 테이블을 사용하는 장점

<div class="content-ad"></div>

- 조직화: 테이블은 데이터를 명확하고 조직적으로 표시하는 방법을 제공합니다.
- 가독성: 숫자 및 텍스트 데이터를 읽고 비교하기 쉽게 만듭니다.
- 접근성: 화면 읽기기를 사용하면 테이블 구조를 쉽게 해석하여 시갖 장애가 있는 사용자들을 위한 접근성을 향상시킵니다.

### 결론

HTML 테이블을 사용하는 방법을 이해하는 것은 탭으로 된 데이터를 효과적으로 조직화하고 표시하는 데 중요합니다. 기본 테이블을 생성하거나 캡션을 추가하거나 머릿글과 바닥글을 사용하거나 셀을 병합하는 등 이러한 요소들을 숙달하면 웹 페이지의 가독성과 사용성이 향상될 것입니다.

제 LinkedIn 팔로우하기 - https://www.linkedin.com/in/ridoy-hasan7
