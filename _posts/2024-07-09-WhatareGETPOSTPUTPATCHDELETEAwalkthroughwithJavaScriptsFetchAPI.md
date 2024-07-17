---
title: "GET, POST, PUT, PATCH, DELETE JavaScript Fetch API로 배우는 요청 방법 튜토리얼"
description: ""
coverImage: "/assets/img/2024-07-09-WhatareGETPOSTPUTPATCHDELETEAwalkthroughwithJavaScriptsFetchAPI_0.png"
date: 2024-07-09 16:58
ogImage:
  url: /assets/img/2024-07-09-WhatareGETPOSTPUTPATCHDELETEAwalkthroughwithJavaScriptsFetchAPI_0.png
tag: Tech
originalTitle: "What are GET, POST, PUT, PATCH, DELETE? A walkthrough with JavaScript’s Fetch API."
link: "https://medium.com/@9cv9official/what-are-get-post-put-patch-delete-a-walkthrough-with-javascripts-fetch-api-17be31755d28"
---

![image](/assets/img/2024-07-09-WhatareGETPOSTPUTPATCHDELETEAwalkthroughwithJavaScriptsFetchAPI_0.png)

GET, POST, PUT, PATCH, 그리고 DELETE는 서버로부터 데이터를 검색하거나 데이터를 전송하는 데 가장 일반적으로 사용되는 다섯 가지 HTTP 메서드입니다.

우리는 typicode의 GitHub에 있는 이 가짜 API를 데모용으로 사용할 것입니다: https://jsonplaceholder.typicode.com/todos

또한 JavaScript의 Fetch API를 활용하여 실제 요청을 만들어보겠습니다. Fetch API는 서버로 요청을 보내기 위한 JavaScript의 간단한 내장 인터페이스입니다. (다른 인터페이스를 가져와야 하는 시대는 끝났습니다!)

<div class="content-ad"></div>

# 주요 용어:

- 자원(Resource): 하나의 데이터 항목입니다.
- URI: 자원의 식별자입니다.

![image](/assets/img/2024-07-09-WhatareGETPOSTPUTPATCHDELETEAwalkthroughwithJavaScriptsFetchAPI_1.png)

서버 https://jsonplaceholder.typicode.com/todos 에는 200개의 자원이 있습니다. 링크를 클릭하여 직접 확인해보세요.

<div class="content-ad"></div>

# GET 메서드

GET 메서드는 서버에서 데이터를 검색하는 데 사용됩니다. 이는 읽기 전용 메서드이므로 데이터를 변경하거나 손상시키는 위험이 없습니다. 예를 들어, API에서 GET 메서드를 호출하면 모든 할 일 목록을 반환합니다.

# 한 번 해보세요!

로컬 브라우저에서 실행할 수 있는 HTML 파일을 설정해 봅시다. index.html이라는 파일을 만들고 다음 코드를 추가하세요:

<div class="content-ad"></div>

```js
<!DOCTYPE html>
<html>
<body>
<h1>JavaScript의 Fetch API를 활용해보세요</h1>
<p>요청을 스크립트에 작성하고 콘솔과 네트워크 로그를 확인해보세요.</p>
<script>
// 모든 할 일 항목 가져오기 (GET)
fetch('https://jsonplaceholder.typicode.com/todos')
.then(response => response.json())
.then(json => console.log(json))
// 모든 리소스를 반환할 것입니다
// 특정 URI의 할 일 항목 가져오기 (이 경우 id = 5)
fetch('https://jsonplaceholder.typicode.com/todos/5')
.then(response => response.json())
.then(json => console.log(json))
/* 다음과 같은 특정 리소스를 반환할 것입니다:
{
“userId”: 1,
“id”: 5,
“title”: “laboriosam mollitia et enim quasi adipisci quia provident illum”,
“completed”: false
}
*/
</script>
</body>
</html>
```

이 파일을 로드하면 스크립트가 실행됩니다. 첫 번째 요청을 살펴봅시다:

```js
// 모든 할 일 항목 가져오기 (GET)
fetch("https://jsonplaceholder.typicode.com/todos")
  .then((response) => response.json())
  .then((json) => console.log(json));
// 모든 리소스를 반환할 것입니다
```

이 요청은 https://jsonplaceholder.typicode.com/todos 서버에서 볼 수 있는 모든 것을 반환합니다. 응답은 200개의 리소스 목록이므로 여기에 표시하지는 않겠습니다. 그러나 응답을 볼 수 있도록 네트워크 로그로 이동하실 수 있습니다. 특정 리소스를 보려면 URL에 URI를 지정하면 됩니다:

<div class="content-ad"></div>

```js
// GET은 특정 URI(이 경우 id = 5)의 할 일을 가져옵니다.
fetch("https://jsonplaceholder.typicode.com/todos/5")
  .then((response) => response.json())
  .then((json) => console.log(json));
/* 다음 특정 리소스를 반환합니다:
{
“userId”: 1,
“id”: 5,
“title”: “laboriosam mollitia et enim quasi adipisci quia provident illum”,
“completed”: false
}
*/
```

응답은 오직 한 개의 리소스만을 포함합니다.

# POST 메서드

POST 메서드는 데이터를 서버에 보내고 새 리소스를 생성합니다. 생성되는 리소스는 다른 부모 리소스에 종속됩니다. 새 리소스가 부모에 POST되면 API 서비스가 이를 자동으로 연결하여 새 리소스에 ID(새 리소스의 URI)를 할당합니다. 즉, 이 방법은 새 데이터 항목을 생성하는 데 사용됩니다.

<div class="content-ad"></div>

# 도전해 보세요!

Fetch API를 사용한 POST 방법은 다음과 같습니다:

```js
// POST 메서드는 전송된 객체에 임의의 ID를 추가합니다
fetch("https://jsonplaceholder.typicode.com/todos", {
  method: "POST",
  body: JSON.stringify({
    userId: 1,
    title: "clean room",
    completed: false,
  }),
  headers: {
    "Content-type": "application/json; charset=UTF-8",
  },
})
  .then((response) => response.json())
  .then((json) => console.log(json));
/* 반환값
{
    "userId": 1,
    "title": "clean room",
    "completed": false,
    "id": 201
}
*/
```

이 코드를 index.html의 스크립트 섹션에 추가하세요. index.html 페이지를 새로고침하고 네트워크 로그를 확인해보세요.

<div class="content-ad"></div>

요청 방법, 본문 및 헤더를 전달해야 했습니다. GET 방법에 대해서는 이전에 이러한 요소를 전달하지 않았습니다. 왜냐하면 기본적으로 이러한 필드는 GET 요청에 대해 구성되어 있지만, 다른 모든 유형의 요청에 대해 이를 지정해야 합니다. 본문에서는 자원의 속성에 값들을 할당하며, 문자열로 변환합니다. URI을 할당할 필요가 없다는 것에 주목하세요 — API가 대신 처리해 줄 것입니다. 응답에서 볼 수 있듯이, API는 새로 생성된 리소스에 ID 201을 할당합니다. (참고: 사용 중인 서버는 플레이스홀더 서비스이므로, 서버는 올바른 응답을 시뮬레이션하기만 합니다. API에 실제 변경사항이 되는 것은 아니므로 https://jsonplaceholder.typicode.com/todos로 이동해도 새 리소스가 추가되지 않았을 때 당황하지 마세요.)

# PUT 방법

PUT 방법은 대부분 기존 리소스를 업데이트할 때 가장 자주 사용됩니다. 특정 리소스를 업데이트하려면 (해당하는 URI가 있는 특정 리소스에) 업데이트하려는 리소스의 새로운 전체 버전을 포함하는 요청 본문을 가지고 해당 리소스 URI로 PUT 방법을 호출할 수 있습니다.

# 한번 시도해 보세요!

<div class="content-ad"></div>

여기에는 id 5를 가진 작업의 이름 변경을 요청하는 PUT 방법이 있습니다:

```js
// id = 5를 가진 리소스로 PUT하여 작업 이름 변경
fetch("https://jsonplaceholder.typicode.com/todos/5", {
  method: "PUT",
  body: JSON.stringify({
    userId: 1,
    id: 5,
    title: "안녕 작업",
    completed: false,
  }),
  headers: {
    "Content-type": "application/json; charset=UTF-8",
  },
})
  .then((response) => response.json())
  .then((json) => console.log(json));
/* 리턴될 내용
{
  "userId": 1,
  "id": 5,
  "title": "안녕 작업",
  "completed": false
}
*/
```

다시 한번 이 코드를 index.html 파일에 추가하고 네트워크 변경을 관찰해보세요. PUT 방법을 명시해야 하고 body에 전달한 JSON 객체를 문자열로 변환해야 한다는 점에 유의하세요. 요청 URL은 변경하려는 리소스를 명시해야 하며, body에는 모든 리소스 속성이 포함되어야 한다는 점을 주의하세요. 응답은 리소스의 새 버전이 될 것입니다. (다시 강조하지만, 이는 시뮬레이션된 응답임을 유의해주세요.)

# PATCH 방법

<div class="content-ad"></div>

PATCH 메소드는 기존 리소스를 수정하는 PUT 메소드와 매우 유사합니다. 차이점은 PUT 메소드의 경우 요청 본문에 완전히 새로운 버전이 포함되지만, PATCH 메소드의 경우 요청 본문에 리소스를 변경해야 하는 구체적인 변경 사항만 포함되어야 합니다. 구체적으로, 리소스가 어떻게 변경되어야 하는지 설명하는 일련의 지침이 있어야 하며, API 서비스는 해당 지침에 따라 새로운 버전을 생성합니다.

# 한 번 시도해 봅시다!

```js
// 리소스 id = 1에 PATCH
// 해당 작업이 완료되었음을 업데이트
fetch("https://jsonplaceholder.typicode.com/todos/1", {
  method: "PATCH",
  body: JSON.stringify({
    completed: true,
  }),
  headers: {
    "Content-type": "application/json; charset=UTF-8",
  },
})
  .then((response) => response.json())
  .then((json) => console.log(json));
/* 결과
{
"userId": 1,
"id": 1,
"title": "delectus aut autem",
"completed": true
}
*/
```

여기서 보듯이, 요청은 PUT 요청과 매우 유사하지만, 요청 본문에는 변경해야 하는 리소스의 속성만 포함되어야 합니다. 응답은 리소스의 새 버전입니다.

<div class="content-ad"></div>

# DELETE 메서드

DELETE 메서드는 URI로 지정된 리소스를 삭제하는 데 사용됩니다.

# 한번 해보죠!

특정 리소스를 삭제하는 DELETE 요청은 간단히 다음과 같습니다:

<div class="content-ad"></div>

```js
// id가 1인 작업 삭제
fetch("https://jsonplaceholder.typicode.com/todos/1", {
  method: "DELETE",
});
// 빈 응답: {}
```

# 결론

완전한 애플리케이션을 구축하려는 사람은 데이터베이스 쿼리하는 방법을 알아야 합니다. 대부분의 애플리케이션은 데이터를 가져오고 저장해야 할 것입니다. 이러한 요청 방법은 완전히 기능적인 애플리케이션에 필요한 것 이상입니다. 자바스크립트의 새로운 Fetch API는 매우 간단히 사용할 수 있습니다. API를 이미 다룬 적이 있다면 익히거나 적응하기 쉽습니다. 그럼에도 불구하고 각각의 요청에 대한 기본적인 이해는 다른 종류의 HTTP 요청 방법에 적응할 수 있도록 잘 준비시켜줍니다.

저의 저장소에서 전체 index.html 코드를 확인해보세요: https://github.com/dalisc/fetch-api-tutorial
