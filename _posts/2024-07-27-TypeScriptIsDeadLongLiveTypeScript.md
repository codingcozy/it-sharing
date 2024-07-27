---
title: "TypeScript는 죽었다, TypeScript 만세 2024년 최신 동향과 전망"
description: ""
coverImage: "/assets/img/2024-07-27-TypeScriptIsDeadLongLiveTypeScript_0.png"
date: 2024-07-27 13:56
ogImage: 
  url: /assets/img/2024-07-27-TypeScriptIsDeadLongLiveTypeScript_0.png
tag: Tech
originalTitle: "TypeScript Is Dead, Long Live TypeScript"
link: "https://medium.com/@impure/typescript-is-dead-long-live-typescript-4bd426dcabcb"
---


<img src="/assets/img/2024-07-27-TypeScriptIsDeadLongLiveTypeScript_0.png" />

Node가 실험적인 TypeScript 지원을 추가했어요. 이 이야기를 읽을 때, "쿨하다, 2015년 Node에 오신 것을 환영합니다" 라고 생각했죠.

하지만 이 영상을 보았어요.

컴파일 단계 없이 TypeScript 코드를 실행하는 것. 그리고 나는 흥미를 느꼈어요. 왜냐하면 이건 그냥 TypeScript만이 아니라고 생각했거든. 사실, 이건 JavaScript를 위한 TypeScript와 비슷한 타입 구문이에요. 그게 상당히 중요하다고 생각되네요.

<div class="content-ad"></div>

우선 풀 리퀘스트 내용을 살펴봅시다. 제목은 module: add — experimental-strip-types#53725로 TypeScript에 대한 언급은 제목에 없네요. 그래도 첫 문장에 TypeScript가 등장합니다:

하지만 이것을 수행하는 방식은 TypeScript를 직접 실행하는 것이 아니라, 타입 스트리핑을 사용한다는 것입니다: