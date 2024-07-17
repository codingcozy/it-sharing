---
title: "React 19 Actions  폼 제출 및 로딩 상태 간단하게 처리하는 방법"
description: ""
coverImage: "/it-shar.ing/assets/no-image.jpg"
date: 2024-07-09 13:39
ogImage:
  url: /it-shar.ing/assets/no-image.jpg
tag: Tech
originalTitle: "React 19 Actions — Simpliying form submission and loading states"
link: "https://medium.com/javascript-in-plain-english/react-19-actions-simpliying-form-submission-and-loading-states-40d92dbadcb7"
---

React 19에서는 비동기 함수인 Actions이 소개되었습니다. Actions는 양식 제출을 쉽게 만드는 데 도움이 됩니다. 이 블로그 게시물에서는 Actions가 무엇이고 어떻게 사용하는지에 대해 자세히 알아봅니다.

이 블로그 게시물에서는 다음을 배우게 됩니다:

- 새로운 React 19 기능 — Actions
- 새로운 React 19 훅 — useActionState 및 useFormStatus
- React 18 양식을 React 19 양식으로 변환하기

# 기능: React Actions

<div class="content-ad"></div>

행동을 이해하기 위해 먼저 오늘날 어떻게 양식을 관리하는지 살펴봅시다. React 18 이전에는 버튼에서 handleSubmit 함수를 사용하여 양식을 제출했습니다. 다음은 하나의 입력 필드인 이름이 있는 간단한 양식입니다:

```js
// React 18에서 양식 제출
console.info('React 18 form');
const [name, setName] = useState('');
const [isPending, setIsPending] = useState(false);

const handleChange = (event) => {
setName(event.target.value);
};

const handleSubmit = (event) => {
event.preventDefault();
setIsPending(true);
setTimeout(() => {
// API 호출
setIsPending(false);
}, 500);
};


return (
<form>
<input type="text" name="name" onChange={handleChange} />
{isPending ? <p>Loading…</p> : <p>Hello in React 18, {name}</p>}
<button onClick={handleSubmit} disabled={isPending}>
Update
</button>
</form>…
```
