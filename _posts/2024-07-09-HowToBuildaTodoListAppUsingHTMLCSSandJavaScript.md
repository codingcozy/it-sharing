---
title: "HTML, CSS, JavaScript로 할일 목록 앱 만드는 방법"
description: ""
coverImage: "/assets/img/2024-07-09-HowToBuildaTodoListAppUsingHTMLCSSandJavaScript_0.png"
date: 2024-07-09 17:38
ogImage:
  url: /assets/img/2024-07-09-HowToBuildaTodoListAppUsingHTMLCSSandJavaScript_0.png
tag: Tech
originalTitle: "How To Build a Todo List App Using HTML, CSS, and JavaScript"
link: "https://medium.com/@sharathchandark/how-to-build-a-todo-list-app-using-html-css-and-javascript-340cda6e3f9f"
---

## 개요:

JavaScript Todo List App 코딩 튜토리얼을 어서오세요.

이 문서에서는 HTML, CSS 및 물론 JavaScript를 사용하여 처음부터 완전히 기능적인 할 일 목록 애플리케이션을 구축하는 단계별 가이드를 안내해 드리겠습니다.

당신은 이 글을 통해 중요한 CRUD 작업 (생성, 읽기, 업데이트 및 삭제)를 구현하는 방법을 배우게 됩니다. 이는 목록에서 데이터 읽기, 작업 추가, 완료된 것으로 표시하기, 기존 작업 편집 및 목록에서 작업 제거와 같은 기능을 포함할 것이며, 위 작업에 대한 멋진 😍 애니메이션이 있습니다.

<div class="content-ad"></div>

💫 실시간 미리보기 👉 할 일 목록 앱

이 비디오를 통해 DOM 조작, 이벤트 처리, 로컬 저장소 통합과 같은 주요 개념을 다룰 예정이며, 이를 통해 할 일을 저장하고 검색할 수 있는 기능을 마음껏 활용할 수 있게 될 거예요.

📝 자바스크립트를 이용한 로컬 저장소를 활용한 할 일 앱 👇

이 비디오를 따라가면, 매일의 작업 관리를 손쉽게 할 수 있는 강력한 할 일 목록 앱을 만들 수 있을 거에요. 원하는 대로 사용자 정의하여 활용할 수 있게 되죠.

<div class="content-ad"></div>

## 중요 사항:

이제 자바스크립트로 구동되는 할 일 목록 앱을 만들기 시작해 봅시다! 즐거운 코딩하세요!

다음 내용을 좋아하실지도 모르겠어요: 초보자가 프로그래밍을 시작하는 방법

## 소개:

<div class="content-ad"></div>

할 일 목록 앱은 생산성을 높이는 환상적인 방법이며 작업을 효과적으로 조직화하고 관리하는 데 탁월한 방법입니다. 이 간단한 가이드에서는 자신만의 할 일 목록 앱을 만드는 단계를 안내해 드리겠습니다.

## 사전 준비 사항:

HTML, CSS 및 JavaScript의 기본 지식.
선택한 코드 편집기.

단계 1: 프로젝트 설정:
프로젝트용 새 폴더를 만들어 주세요. 원하는대로 프로젝트명을 지정하세요. 저는 "할 일 앱"이라는 프로젝트명을 만들었습니다. 이 폴더 안에 index.html, style.css 및 script.js 세 개의 파일을 만들어 주세요. 이 파일들이 당신의 할 일 목록 앱의 기초 역할을 합니다. 그렇지 않은 경우, "Project-Structure-Example"을 복제하고 나와 함께 작업을 시작할 수 있습니다.

<div class="content-ad"></div>

Step 2: 기본 HTML 구조 만들기:
index.html 파일에서 앱을 위한 HTML 구조를 만들고 HTML 보일러플레이트를 추가하고 HTML 파일에 파일(style.css, script.js)을 연결할 것입니다.

```js
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>To Do Application - By Sharathchandar.K</title>
    <link rel="stylesheet" href="style.css" />
  </head>
  <body>
    <script type="text/javascript" src="script.js"></script>
  </body>
</html>
```

먼저, body 요소 내에 class가 container인 div를 추가할 것입니다. div 태그는 HTML 문서에서 섹션이나 구역을 정의합니다.

```js
<div class="container">
<!-- 컨테이너 내부에 할 일을 추가하기 위한 입력 필드, 할 일을 표시하는 목록, 할 일 추가 및 삭제 버튼을 추가할 것입니다.-->
</div>
```

<div class="content-ad"></div>

Step 3: CSS로 Todo 앱 스타일링하기:
style.css 파일에 CSS 규칙을 추가하여 앱을 시각적으로 매력적으로 만듭니다.

우리는 모든 요소에 마진과 패딩을 제로로 설정하여 모든 브라우저에서 동일하게 보이도록 합니다. \* 와 내가 가장 좋아하는 폰트 패밀리 Poppins의 도움으로.

```js
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600&display=swap");
* {
  margin: 0;
  padding: 0;
  font-family: "Poppins", sans-serif;
  box-sizing: border-box;
}
```

이제 모든 HTML 요소를 화면 중앙에 배치할 것이므로 body와 .container에 스타일을 추가할 것입니다.

<div class="content-ad"></div>

```js
body {
  background: #78c1f3;
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}

.container {
  background: #ffffff;
  padding: 25px;
  width: 550px;
  border-radius: 10px;
}
```

<img src="/assets/img/2024-07-09-HowToBuildaTodoListAppUsingHTMLCSSandJavaScript_0.png" />

Step 4: 할 일 헤더 및 본문 구현:
이제 컨테이너 내부에 할 일 헤더를 위한 HTML 요소를 추가할 것입니다.

```js
<div class="todo-header">
  <h2>할 일 목록</h2>
  <img src="/images/notebook.gif" height="50px" />
</div>
```

<div class="content-ad"></div>

```css
.todo-header {
  display: flex;
  align-items: center;
  margin-bottom: 20px;
  padding-left: 5px;
  justify-content: center;
}
```

<img src="/assets/img/2024-07-09-HowToBuildaTodoListAppUsingHTMLCSSandJavaScript_1.png" />

이제 우리는 할 일 헤더 아래에 할 일 본문을 넣을 HTML 요소를 추가할 것입니다. 할 일을 추가할 수 있는 입력 필드, 할 일을 표시할 목록 및 할 일을 추가하고 제거할 수 있는 버튼이 필요합니다.

```html
<div class="todo-body">
  <input type="text" id="todoText" class="todo-input" placeholder="Add your items" />
  <img src="/images/plus.png" alt="+" id="AddUpdateClick" onclick="CreateToDoItems();" />
</div>
```

<div class="content-ad"></div>

```js
.todo-body {
  display: flex;
  align-items: center;
  justify-content: space-between;
  background: #edeef0;
  border-radius: 30px;
  padding-left: 20px;
  margin-bottom: 25px;
}

.todo-body input {
  flex: 1;
  border: none;
  outline: none;
  background: transparent;
  padding: 15px 0;
  font-size: 20px;
}

.todo-body img {
  cursor: pointer;
  border-radius: 40px;
  height: 55px;
  width: 55px;
  padding: 15px;
  background: limegreen;
}
```

<img src="/assets/img/2024-07-09-HowToBuildaTodoListAppUsingHTMLCSSandJavaScript_2.png" />

단계 5: 유효성 검사가 포함된 목록 만들어 Todo 정보 표시하기:
여기에는 사용자가 수행한 활동에 대한 유효성 검사를 표시하기 위해 h5 태그를 추가할 것입니다. 예: 사용자가 할 일을 추가, 업데이트, 완료 또는 삭제한 정보가 포함됩니다. 그런 다음 사용자가 추가한 정보로 데이터 목록을 추가하기 위해 ul 태그를 추가할 것입니다.

```js
<h5 id="Alert"></h5>
<ul id="list-items" class="list-items"></ul>
```

<div class="content-ad"></div>

```js
ul li {
  list-style: none;
  font-size: 18px;
  cursor: pointer;
  padding: 10px;
}

li {
  display: flex;
  align-items: center;
  justify-content: space-between;
  background: #edeef0;
  margin-bottom: 10px;
  border-radius: 5px;
}

h5 {
  text-align: center;
  margin-bottom: 10px;
  color: green;
}

.todo-controls {
  width: 25px;
  height: 25px;
  padding: 3px;
  margin-right: 5px;
}
```

6단계: JavaScript 기능 구현:
script.js 파일을 열고 앱을 상호 작용 가능하게 만들기 위한 JavaScript 코드를 작성하세요. 할 일 추가, 할 일 삭제, 전체 목록 지우기와 관련된 함수를 만들어보세요. HTML의 요소를 조작하기 위해 Document Object Model (DOM)를 사용할 수 있습니다.

먼저 document 객체를 사용하여 선언된 모든 선언을 선언합니다. document 객체는 웹 페이지에서 다른 모든 객체의 소유자를 나타냅니다.

```js
const todoValue = document.getElementById("todoText");
const todoAlert = document.getElementById("Alert");
const listItems = document.getElementById("list-items");
const addUpdate = document.getElementById("AddUpdateClick");
```

<div class="content-ad"></div>

다음은 key:todo-list를 사용하여 localstorage 객체를 선언합니다.

```js
let todo = JSON.parse(localStorage.getItem("todo-list"));
if (!todo) {
  todo = [];
}
```

다음으로 CREATE, READ, UPDATE 및 DELETE 기능을 추가할 것입니다.
함수는 특정 작업을 수행하기 위해 설계된 코드 블록입니다. JavaScript 함수는 "무언가"가 호출할 때 실행됩니다.

아래 함수는 리스트에 새로운 작업을 만들고 몇 가지 추가 기능을 도와줍니다:

<div class="content-ad"></div>

- 첫 번째 도구는 사용자가 작업을 입력했는지 여부를 확인합니다. 만약 입력하지 않은 채로 도구를 추가하려고 한다면 "할 일 텍스트를 입력해 주세요!"라는 오류 메시지가 표시됩니다.
- 사용자가 텍스트 상자에 작업을 입력하고 옆에 있는 버튼을 누르면 도구가 자동으로 목록과 로컬 저장소에 추가됩니다.
- 사용자가 이미 목록에 있는 동일한 작업을 추가하면 도구가 "이 항목은 이미 목록에 있습니다!"라고 알립니다.
- 작업을 추가하는 것은 독특한데, 이미 있는 작업을 추가해도 소용이 없으므로 도구가 알림창을 표시합니다.
- 사용자가 작업을 목록에 추가한 후 도구는 "할 일 항목이 성공적으로 생성되었습니다!"라는 응답 메시지를 표시합니다.
- 목록에서 사용자는 두 가지 옵션을 가진 목록을 볼 수 있습니다. 하나는 🖉 편집이고 다른 하나는 🗑️ 삭제입니다. (아래 기능으로 설명되어 있음)

```js
function CreateToDoItems() {
  if (todoValue.value === "") {
    todoAlert.innerText = "할 일 텍스트를 입력해 주세요!";
    todoValue.focus();
  } else {
    let IsPresent = false;
    todo.forEach((element) => {
      if (element.item == todoValue.value) {
        IsPresent = true;
      }
    });

    if (IsPresent) {
      setAlertMessage("이 항목은 이미 목록에 있습니다!");
      return;
    }

    let li = document.createElement("li");
    const todoItems = `<div title="더블 클릭하여 완료" ondblclick="CompletedToDoItems(this)">${todoValue.value}</div><div>
                    <img class="edit todo-controls" onclick="UpdateToDoItems(this)" src="/images/pencil.png" />
                    <img class="delete todo-controls" onclick="DeleteToDoItems(this)" src="/images/delete.png" /></div></div>`;
    li.innerHTML = todoItems;
    listItems.appendChild(li);

    if (!todo) {
      todo = [];
    }
    let itemList = { item: todoValue.value, status: false };
    todo.push(itemList);
    setLocalStorage();
  }
  todoValue.value = "";
  setAlertMessage("할 일 항목이 성공적으로 생성되었습니다!");
}
```

<img src="/assets/img/2024-07-09-HowToBuildaTodoListAppUsingHTMLCSSandJavaScript_3.png" />

이 아래 함수는 로컬 저장소에서 데이터를 읽어와 할 일 목록에 표시하는 데 도움이 됩니다. 몇 가지 추가 기능이 있습니다:

<div class="content-ad"></div>

- 목록에서 사용자는 작업 앱에 추가된 모든 데이터를 나열할 수 있습니다. 이 데이터는 로컬 스토리지에서 검색됩니다.
- 사용자가 브라우저를 새로 고치거나 닫아도 로컬 스토리지는 데이터를 유지하며 언제든지 데이터를 다시 표시합니다.
- 목록에서 사용자는 두 가지 옵션이 포함된 목록을 볼 수 있습니다: 🖉 편집과 🗑️ 삭제. (아래 함수로 설명되어 있습니다)

```js
function ReadToDoItems() {
  todo.forEach((element) => {
    let li = document.createElement("li");
    let style = "";
    if (element.status) {
      style = "style='text-decoration: line-through'";
    }
    const todoItems = `<div ${style} title="더블 클릭하여 완료" ondblclick="CompletedToDoItems(this)">${element.item}
    ${style === "" ? "" : '<img class="todo-controls" src="/images/check-mark.png" />'}</div><div>
    ${style === "" ? '<img class="edit todo-controls" onclick="UpdateToDoItems(this)" src="/images/pencil.png" />' : ""}
    <img class="delete todo-controls" onclick="DeleteToDoItems(this)" src="/images/delete.png" /></div></div>`;
    li.innerHTML = todoItems;
    listItems.appendChild(li);
  });
}
ReadToDoItems();
```

<img src="/assets/img/2024-07-09-HowToBuildaTodoListAppUsingHTMLCSSandJavaScript_4.png" />

아래 함수는 사용자가 추가한 작업을 동일한 목록에 추가하고 추가 기능을 제공합니다:

<div class="content-ad"></div>

- 사용자는 리스트에 이미 존재하는 할 일 데이터를 수정할 수 있습니다. 🖉 리스트에서 연필 아이콘 편집 옵션을 사용해야 합니다.
- 사용자가 이 🖉 도구를 클릭하면 선택된 작업 값이 텍스트 상자에 할당되고 버튼 아이콘이 업데이트로 변경됩니다.
- 사용자가 작업을 변경하고 업데이트를 클릭하면 리스트 및 로컬 저장소에서 업데이트됩니다.
- 아무 것도 변경하지 않고 사용자가 업데이트를 시도하면 도구가 "이 항목은 이미 목록에 있습니다!"라는 경고 메시지를 표시합니다.
- 사용자가 변경하고 업데이트를 클릭하면 도구가 "할 일 항목이 성공적으로 업데이트되었습니다!"라는 응답 메시지를 보냅니다!

![이미지](/assets/img/2024-07-09-HowToBuildaTodoListAppUsingHTMLCSSandJavaScript_5.png)

```js
function UpdateToDoItems(e) {
  if (e.parentElement.parentElement.querySelector("div").style.textDecoration === "") {
    todoValue.value = e.parentElement.parentElement.querySelector("div").innerText;
    updateText = e.parentElement.parentElement.querySelector("div");
    addUpdate.setAttribute("onclick", "UpdateOnSelectionItems()");
    addUpdate.setAttribute("src", "/images/refresh.png");
    todoValue.focus();
  }
}

function UpdateOnSelectionItems() {
  let IsPresent = false;
  todo.forEach((element) => {
    if (element.item == todoValue.value) {
      IsPresent = true;
    }
  });

  if (IsPresent) {
    setAlertMessage("이 항목은 이미 목록에 있습니다!");
    return;
  }

  todo.forEach((element) => {
    if (element.item == updateText.innerText.trim()) {
      element.item = todoValue.value;
    }
  });
  setLocalStorage();

  updateText.innerText = todoValue.value;
  addUpdate.setAttribute("onclick", "CreateToDoItems()");
  addUpdate.setAttribute("src", "/images/plus.png");
  todoValue.value = "";
  setAlertMessage("할 일 항목이 성공적으로 업데이트되었습니다!");
}
```

![이미지](/assets/img/2024-07-09-HowToBuildaTodoListAppUsingHTMLCSSandJavaScript_6.png)

<div class="content-ad"></div>

아래 함수는 목록에서 작업 데이터를 삭제하고 추가 기능을 제공합니다:

- 사용자가 목록 도구에서 🗑️ 삭제 아이콘을 클릭하면 확인 메시지 "정말 삭제하시겠습니까? $TaskName을(를) 삭제하시겠습니까?"가 표시됩니다.
- 사용자가 확인을 클릭하면 도구는 자동으로 목록과 로컬 저장소에서 작업 데이터를 삭제합니다.
- 사용자가 취소를 클릭하면 아무 일도 변경되지 않습니다. 이 확인 메시지는 사용자가 올바른 이벤트를 클릭하는 데 인식시킵니다.

```js
function DeleteToDoItems(e) {
  let deleteValue = e.parentElement.parentElement.querySelector("div").innerText;

  if (confirm(`정말 삭제하시겠습니까? ${deleteValue}을(를) 삭제하시겠습니까?`)) {
    e.parentElement.parentElement.setAttribute("class", "deleted-item");
    todoValue.focus();

    todo.forEach((element) => {
      if (element.item == deleteValue.trim()) {
        todo.splice(element, 1);
      }
    });

    setTimeout(() => {
      e.parentElement.parentElement.remove();
    }, 1000);

    setLocalStorage();
  }
}
```

![이미지](/assets/img/2024-07-09-HowToBuildaTodoListAppUsingHTMLCSSandJavaScript_7.png)

<div class="content-ad"></div>

아래 함수는 작업의 상태를 COMPLETE ✔로 변경하는 데 도움을 줍니다. 추가적인 기능을 제공합니다:

- 사용자가 작업을 완료하면 해당 작업이 목록에서 삭제되지 않고 남아 있기를 원한다면, 스타일을 변경하고 완료된 작업에 ✔ 아이콘을 설정하는 아이디어를 고안했습니다.
- 사용자가 작업을 완료하면 라인 스타일로 변경하고 옆에 체크 ✔ 아이콘을 추가할 수 있도록 더블 클릭할 수 있습니다.
- 사용자가 작업을 완료하면 수정할 수 없습니다. 완료 전에 확인이 필요합니다.
- 사용자에게는 삭제하기 🗑️ 옵션이 하나만 표시됩니다.

```js
function CompletedToDoItems(e) {
  if (e.parentElement.querySelector("div").style.textDecoration === "") {
    const img = document.createElement("img");
    img.src = "/images/check-mark.png";
    img.className = "todo-controls";
    e.parentElement.querySelector("div").style.textDecoration = "line-through";
    e.parentElement.querySelector("div").appendChild(img);
    e.parentElement.querySelector("img.edit").remove();

    todo.forEach((element) => {
      if (e.parentElement.querySelector("div").innerText.trim() == element.item) {
        element.status = true;
      }
    });
    setLocalStorage();
    setAlertMessage("Todo 항목이 성공적으로 완료되었습니다!");
  }
}
```

<img src="/assets/img/2024-07-09-HowToBuildaTodoListAppUsingHTMLCSSandJavaScript_8.png" />

<div class="content-ad"></div>

아래 함수는 로컬스토리지 아이템을 설정하는 데 도움이 됩니다. 이를 별도의 함수로 분리해 CREATE, UPDATE 및 DELETE 함수에서 호출했는데요, 이를 통해 동일한 코드를 여러 곳에 사용하지 않고도 작업할 수 있게 되었어요.

```js
function setLocalStorage() {
  localStorage.setItem("todo-list", JSON.stringify(todo));
}
```

아래 함수는 앱 사용자 활동에 따라 알림 메시지를 설정하는 데 도움이 됩니다. 사용자 함수 호출에 따라 매개변수로 메시지를 보내고 예약합니다. innerText를 사용하여 알림을 HTML 요소에 설정합니다.

```js
function setAlertMessage(message) {
  todoAlert.removeAttribute("class");
  todoAlert.innerText = message;
  setTimeout(() => {
    todoAlert.classList.add("toggleMe");
  }, 1000);
}
```

<div class="content-ad"></div>

**단계 6: 할 일 항목에 애니메이션 적용 및 알림 메시지 숨기기 구현:**

아래 코드는 할 일 항목에 애니메이션을 설정하는 데 도움이 될 것입니다.

```js
li {
  opacity: 0;
  animation: new-item-animation 0.3s linear forwards;
}

@keyframes new-item-animation {
  from {
    opacity: 0;
    transform: translateY(-400px);
  }

  to {
    opacity: 1;
    transform: translateY(0);
  }
}

li.deleted-item {
  animation: removed-item-animation 1s cubic-bezier(0.55, -0.04, 0.91, 0.94)
    forwards;
  transform-origin: 0% 100%;
}

@keyframes removed-item-animation {
  0% {
    opacity: 1;
    transform: rotateZ(0);
  }

  100% {
    opacity: 0;
    transform: translateY(600px) rotateZ(90deg);
  }
}
```

아래 코드는 사용자가 목록에서 작업을 완료하면 알림 메시지를 천천히 숨기는 데 사용됩니다.

```js
.toggleMe {
  animation: hideMe 5s forwards;
}
@keyframes hideMe {
  0% {
    opacity: 1;
  }
  100% {
    opacity: 0;
  }
}
```

<div class="content-ad"></div>

7단계: 테스트 및 사용자 정의:
할 일 목록 앱을 테스트하려면 웹 브라우저에서 index.html 파일을 열어보세요. 작업을 추가하고 제거하며 목록을 지워서 모든 것이 예상대로 작동하는지 확인해보세요. 작업 중요도, 마감일 등의 기능을 추가하여 앱을 더욱 사용자 정의하고 향상시킬 수 있습니다. 또한 로컬 저장소를 이용한 데이터 지속성도 구현할 수 있습니다.

우리의 커뮤니티에 참여해 보세요. 함께 Full Stack 개발을 탐구하고 배우며 즐거운 시간을 보내는 유튜브 채널입니다. 구독하시면 저희가 고품질이고 유익한 영상을 계속 제작할 수 있으며, 여러분과 친구, 가족과 함께 공유할 수 있는 콘텐츠를 즐기실 수 있습니다.

아직 구독하지 않으셨다면, 지금 바로 구독 버튼을 눌러주시고 알림 벨을 클릭하여 우리 유튜브 가족의 일원이 되어주세요. 함께 즐겁게 배우고 성장하며 지식을 공유해 나가요.

지지해 주셔서 다시 한번 감사드리며, 유튜브 채널에서 만나기를 기대할게요!
