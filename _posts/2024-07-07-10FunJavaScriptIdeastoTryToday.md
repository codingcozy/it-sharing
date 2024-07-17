---
title: "오늘 바로 시도해볼 수 있는 재미있는 자바스크립트 아이디어 10가지"
description: ""
coverImage: "/it-shar.ing/assets/no-image.jpg"
date: 2024-07-07 12:18
ogImage:
  url: /it-shar.ing/assets/no-image.jpg
tag: Tech
originalTitle: "10 Fun JavaScript Ideas to Try Today"
link: "https://dev.to/mukeshb/10-fun-javascript-ideas-to-try-today-1gha"
---

JavaScript은 개발자가 동적이고 인터랙티브한 웹 애플리케이션을 구축할 수 있는 강력한 프로그래밍 언어입니다. 경험이 많거나 막 시작한 사람이던 간에, 실제 프로젝트에 참여하는 것은 JavaScript 기술을 향상시키는 좋은 방법입니다.

이 안내서는 웹 애플리케이션을 위한 일반적인 작업과 기능을 다루는 실용적인 JavaScript 예제 모음을 제시합니다.

각 예제에는 코드 조각과 동작 방식에 대한 설명이 포함되어 있습니다. 이를 통해 JavaScript 개념에 대한 실습 경험을 쌓고 더 인터랙티브하고 사용자 친화적인 웹 페이지를 만드는 방법을 배울 수 있습니다.

예제

<div class="content-ad"></div>

운영 체제 세부 정보 찾기

```js
const os = navigator.platform;
alert("사용 중인 운영 체제: " + os);
```

- navigator.platform: 이 속성은 브라우저의 플랫폼(운영 체제)을 나타내는 문자열을 반환합니다.
- alert 함수는 감지된 운영 체제를 보여주는 팝업 상자를 표시하는 데 사용됩니다. 이는 웹 애플리케이션이 실행되는 환경을 이해하는 데 유용합니다.

사용자의 브라우저 감지

<div class="content-ad"></div>

```js
const browser = navigator.userAgent;
alert("Your Browser: " + browser);
```

- navigator.userAgent: 이 속성은 브라우저의 사용자 에이전트 문자열을 반환합니다. 브라우저 버전, 운영 체제 및 기타 세부 정보를 포함합니다.
- 이 정보는 브라우저별 기능이나 사용자가 사용하는 브라우저를 이해하기 위한 분석 목적으로 활용될 수 있습니다.

디지털 시계 만들기

```js
<div id="clock"></div>
<script>
  setInterval(() => {
    const now = new Date();
    document.getElementById('clock').innerText = now.toLocaleTimeString();
  }, 1000);
</script>
```

<div class="content-ad"></div>

- ` div id="clock"``/div `: 현재 시간이 표시되는 HTML 요소입니다.
- setInterval(callback, 1000): 이 JavaScript 함수는 1000 밀리초(1초)마다 콜백 함수를 반복해서 호출합니다.
- new Date(): 현재 날짜와 시간을 나타내는 새로운 Date 객체를 생성합니다.
- now.toLocaleTimeString(): 현재 시간을 사람이 읽을 수 있는 문자열로 형식화합니다.
- document.getElementById(`clock`).innerText = now.toLocaleTimeString(): div의 내용을 현재 시간으로 업데이트합니다.

양식 유효성

```js
<html>
<head>
    <title>Form Validation</title>
</head>
<body>
    <form id="myForm">
        <input type="text" id="name" placeholder="이름을 입력하세요">
        <button type="submit">제출</button>
    </form>
    <script>
        document.getElementById('myForm').addEventListener('submit', function(event) {
            const name = document.getElementById('name').value;
            if (!name) {
                alert("이름을 입력하세요");
                event.preventDefault();
            }
        });
    </script>
</body>
</html>
```

- `form id="myForm"`: 사용자가 이름을 입력할 수 있는 양식 요소입니다.
- document.getElementById(`myForm`).addEventListener(`submit`, function(event) ' ... '): 폼의 제출 이벤트에 이벤트 리스너를 추가합니다.
- document.getElementById(`name`).value: 입력 필드에 입력된 값 가져옵니다.
- if (!name) ' ... ': 입력 필드가 비어 있는지 확인합니다.
- alert("이름을 입력하세요"): 입력 필드가 비어 있을 때 경고 메시지를 표시합니다.
- event.preventDefault(): 유효성 검사에 실패하면 폼이 제출되지 않도록 하여 사용자가 입력을 수정하도록 합니다.

<div class="content-ad"></div>

마우스 좌표 표시

```js
<div id="coords">이 영역 위를 마우스로 이동해 주세요.</div>
<script>
  document.addEventListener('mousemove', function(event) {
    const coords = `X: ${event.clientX}, Y: ${event.clientY}`;
    document.getElementById('coords').innerText = coords;
  });
</script>
```

- `div id="coords"`이 영역 위를 마우스로 이동해 주세요.`/div`: 마우스 좌표를 표시하기 위한 div 요소.
- document.addEventListener(`mousemove`, function(event) ' ... '): 문서의 마우스 이동 이벤트에 이벤트 리스너를 추가합니다.
- const coords = \X: $'event.clientX', Y: $'event.clientY': 뷰포트 기준으로 마우스 좌표를 가져옵니다.
- document.getElementById(`coords`).innerText = coords: 현재 마우스 좌표로 div를 업데이트합니다.

랜덤 숫자 생성

<div class="content-ad"></div>

```js
<button id="generate">Generate Random Number</button>
<p id="randomNumber"></p>
<script>
  document.getElementById('generate').addEventListener('click',
function() {
    const randomNumber = Math.floor(Math.random() * 100) + 1;
    document.getElementById('randomNumber').innerText = "Random Number: " + randomNumber;
  });
</script>
```

- `button id="generate"`: 랜덤 숫자 생성 버튼.
- `p id="randomNumber"`: 생성된 랜덤 숫자를 표시하는 단락.
- document.getElementById(`generate`).addEventListener(`click`, function() ' ... '): 버튼에 클릭 이벤트 리스너 추가.
- const randomNumber = Math.floor(Math.random() \* 100) + 1;: 1부터 100 사이의 랜덤 숫자 생성.
- document.getElementById(`randomNumber`).innerText = "Random Number: " + randomNumber;: 생성된 랜덤 숫자를 단락에 표시.

랜덤 배경색 변경

```js
<button id="randomColor">Change Background Color</button>
<script>
  document.getElementById('randomColor').addEventListener('click', function() {
    const colors = ['red', 'green', 'blue', 'yellow', 'purple'];
    const randomColor = colors[Math.floor(Math.random() * colors.length)];
    document.body.style.backgroundColor = randomColor;
  });
</script>
```

<div class="content-ad"></div>

- `button id="randomColor"`Change Background Color`/button`: 배경색을 변경하는 버튼입니다.
- document.getElementById(`randomColor`).addEventListener(`click`, function() ' ... '): 클릭 이벤트 리스너를 버튼에 추가합니다.
- const colors = [`red`, `green`, `blue`, `yellow`, `purple`];: 색상 옵션을 담은 배열입니다.
- const randomColor = colors[Math.floor(Math.random() * colors.length)];: 배열에서 무작위 색상을 선택합니다.
- document.body.style.backgroundColor = randomColor;: body의 배경색을 무작위로 선택된 색상으로 설정합니다.

할 일 목록 만들기

```js
<input type="text" id="taskInput" placeholder="할 일을 입력하세요">
<button id="addButton">할 일 추가</button>
<ul id="taskList"></ul>
<script>
  document.getElementById('addButton').addEventListener('click', function() {
    const task = document.getElementById('taskInput').value;
    const li = document.createElement('li');
    li.innerText = task;
    document.getElementById('taskList').appendChild(li);
  });
</script>
```

- `input type="text" id="taskInput" placeholder="할 일을 입력하세요"`: 할 일을 입력하는 입력 필드입니다.
- `button id="addButton"`할 일 추가`/button`: 목록에 할 일을 추가하는 버튼입니다.
- ` ul id="taskList"``/ul `: 할 일을 표시하는 목록입니다.
- document.getElementById(`addButton`).addEventListener(`click`, function() ' ... '): 버튼의 클릭 이벤트에 이벤트 리스너를 추가합니다.
- document.getElementById(`taskInput`).value: 입력 필드에 입력된 값을 가져옵니다.
- const li = document.createElement(`li`): 새로운 목록 항목 요소를 생성합니다.
- li.innerText = task: 목록 항목의 텍스트를 입력된 할 일로 설정합니다.
- document.getElementById(`taskList`).appendChild(li): 새로운 목록 항목을 할 일 목록에 추가합니다.

<div class="content-ad"></div>

드롭다운 메뉴 만들기

```js
<select id="dropdown">
  <option value="Option 1">Option 1</option>
  <option value="Option 2">Option 2</option>
  <option value="Option 3">Option 3</option>
</select>
<button id="showSelection">선택한 옵션 보기</button>
<script>
  document.getElementById('showSelection').addEventListener('click', function() {
    const selectedOption = document.getElementById('dropdown').value;
    alert("선택한 옵션: " + selectedOption);
  });
</script>
```

- `select id="dropdown"`: 세 개의 옵션이 있는 드롭다운 메뉴.
- `button id="showSelection"`: 선택한 옵션 보기`/button`: 드롭다운 메뉴에서 선택한 옵션을 보여주는 버튼.
- document.getElementById(`showSelection`).addEventListener(`click`, function() ' ... '): 버튼에 클릭 이벤트 리스너 추가.
- const selectedOption = document.getElementById(`dropdown`).value;: 선택한 옵션의 값 가져오기.
- alert("선택한 옵션: " + selectedOption);: 선택한 옵션을 알림으로 표시하기.

요소의 가시성 전환

<div class="content-ad"></div>

```js
<div id="toggleDiv">이것은 토글 가능한 div입니다.</div>
<button id="toggleButton">가시성 전환</button>
<script>
  document.getElementById('toggleButton').addEventListener('click',
function() {
    const div = document.getElementById('toggleDiv');
    if (div.style.display === 'none') {
      div.style.display = 'block';
    } else {
      div.style.display = 'none';
    }
  });
</script>
```

- `div id="toggleDiv"`이것은 토글 가능한 div입니다.`/div`: 일부 텍스트를 포함한 div 요소로, 표시하거나 숨기려는 내용이 있습니다.
- `button id="toggleButton"`가시성 전환`/button`: div의 가시성을 전환하는 버튼입니다.
- document.getElementById(`toggleButton`).addEventListener(`click`, function() ' ... '): 버튼에 클릭 이벤트 리스너를 추가합니다.
- const div = document.getElementById(`toggleDiv`);: div 요소를 선택합니다.
- if (div.style.display === `none`) ' div.style.display = `block`; ' else ' div.style.display = `none`; ': div의 display 속성을 `none` (숨김)과 `block` (표시) 사이에서 전환합니다.
