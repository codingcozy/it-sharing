---
title: "타입스크립트로 npm 패키지 만드는 방법 Step-by-Step 가이드"
description: ""
coverImage: "/assets/img/2024-07-09-CreatingannpmpackagewithTypeScript_0.png"
date: 2024-07-09 16:35
ogImage:
  url: /assets/img/2024-07-09-CreatingannpmpackagewithTypeScript_0.png
tag: Tech
originalTitle: "Creating an npm package with TypeScript"
link: "https://medium.com/@the_nick_morgan/creating-an-npm-package-with-typescript-c38b97a793cf"
---

![Creating an npm package with TypeScript](/assets/img/2024-07-09-CreatingannpmpackagewithTypeScript_0.png)

npm 패키지를 만드는 것은 다른 개발자들과 전 세계적으로 코드를 공유할 수 있는 흥미로운 모험이 될 수 있습니다. TypeScript를 사용하면 코드를 보다 유지 관리하기 쉽게 만들 뿐만 아니라 패키지 이용자들에게 유용한 타입 정보를 제공할 수 있습니다. 이 안내서에서는 TypeScript로 작성된 두 숫자를 더하는 간단한 npm 패키지를 만드는 과정을 안내합니다.

# 준비 사항

- Node.js와 npm이 설치되어 있는지 확인하세요. 아닌 경우 Node.js 공식 웹사이트에서 다운로드하여 설치하세요.
- TypeScript와 npm에 대한 기본 지식이 있어야 합니다.

<div class="content-ad"></div>

# 단계별 안내

# 1. 프로젝트 초기화

프로젝트를 위한 새 디렉토리를 만들고 해당 디렉토리로 이동하세요:

```js
mkdir adder-ts
cd adder-ts
```

<div class="content-ad"></div>

새로운 npm 프로젝트를 초기화해보세요:

```js
npm init -y
```

이 명령어는 기본값으로 package.json 파일을 생성합니다.

# 2. TypeScript 설정하기

<div class="content-ad"></div>

TypeScript 및 Node.js용 TypeScript 정의를 제공하는 @types/node를 설치해주세요.

```js
npm install typescript @types/node --save-dev
```

이제 TypeScript 구성을 초기화하세요.

```js
npx tsc --init
```

<div class="content-ad"></div>

이 명령은 기본 TypeScript 구성이 포함된 tsconfig.json 파일을 생성합니다.

# 3. TypeScript 코드 작성

프로젝트 디렉토리에서 index.ts라는 파일을 생성하세요. 이 파일은 패키지의 주 파일이 될 것입니다.

```js
export function add(x: number, y: number): number {
  return x + y;
}
```

<div class="content-ad"></div>

# 4. 빌드 프로세스 구성하기

당신의 tsconfig.json 파일에서, 컴파일된 JavaScript의 출력 디렉토리를 구성할 수 있습니다. 예를 들어:

```js
{
  "compilerOptions": {
    ...
    "outDir": "./dist",
    ...
  },
  ...
}
```

빌드 스크립트를 포함하기 위해 package.json을 업데이트하세요.

<div class="content-ad"></div>

```js
{
  ...
  "scripts": {
    "build": "tsc"
  },
  ...
}
```

## 5. 출판을 위한 준비

출판하기 전에 라이브러리의 진입점을 결정해야합니다. package.json에서 main 필드를 코드의 컴파일된 버전으로, types를 타입 정의 파일로 설정하십시오:

```js
{
  ...
  "main": "dist/index.js",
  "types": "dist/index.d.ts",
  ...
}
```

<div class="content-ad"></div>

# 6. npm에 배포하기

npm에 게시하기 전에 npm에 로그인되어 있는지 확인해주세요:

```js
npm login
```

로그인한 후에 TypeScript 코드를 빌드하세요.

<div class="content-ad"></div>

```js
npm run build
```

이제 패키지를 게시할 수 있어요:

```js
npm publish
```

# 7. 패키지 사용하기

<div class="content-ad"></div>

게시한 후에는 누구나 패키지를 설치하고 사용할 수 있어요:

```js
npm install adder-ts
```

그리고 TypeScript(또는 JavaScript) 코드에서:

```js
import { add } from "adder-ts";
```

<div class="content-ad"></div>

```js
console.log(add(2, 3)); // 출력: 5
```

# 사용자 지정 이름과 범위 추가

# 1. npm에서 범위 만들기

사용자 정의 범위 아래 패키지를 게시하기 전에 npm 계정이 있어야 하며 해당 범위는 계정과 연결되어 있어야 합니다.

<div class="content-ad"></div>

예를 들어, @nicks-adding-func이 원하는 범위라면, 당신만의 것을 추가해야 합니다.

다음을 해야 합니다:

- 이미 가입하지 않았다면 npmjs.com에 가입하세요.
- npm CLI에 로그인하세요:

```js
npm login
```

<div class="content-ad"></div>

- 조직 생성 (이 조직은 당신의 스코프로 사용될 것입니다):

```js
npm org create @nicks-adding-func
```

# 2. 패키지 이름 정하기

패키지의 package.json 파일에서 name 필드는 사용자 정의 스코프와 원하는 패키지 이름을 반영해야 합니다.

<div class="content-ad"></div>

```js
{
  "name": "@nicks-adding-func/cli",
  ...
}
```

# 3. 스코프 아래로 발행하기

기본적으로, 스코프가 지정된 패키지는 비공개입니다. 공개 스코프 패키지를 발행하려면 발행할 때 명시해야 합니다:

```js
npm publish --access public
```

<div class="content-ad"></div>

# 4. 스코프 패키지 설치하기

스코프 패키지를 설치하는 것은 일반 패키지와 비슷합니다. 사용자는 전체 스코프를 포함해야 합니다:

```js
npm install @nicks-adding-func/cli
```

# 5. 스코프 패키지의 장점

<div class="content-ad"></div>

- 이름 충돌 방지: npm에는 수백만 개의 패키지가 있어 원하는 이름이 이미 사용 중일 수 있습니다. 스코프를 사용하면 더 자유롭게 이름을 짓을 수 있습니다.
- 브랜딩과 신뢰: 특히 조직에서는 공식적인 스코프를 갖는 것이 사용자들이 공식 패키지를 사용하고 있고 포크나 모방이 아님을 확신시켜 줄 수 있습니다.
- 명확한 의미: 스코프는 추가적인 컨텍스트를 제공할 수 있습니다. 예를 들어 사용자들은 @nicks-adding-func/cli가 nicks-adding-func 프로젝트의 공식 CLI 도구라고 추측할 수 있습니다.

# 결론

TypeScript로 npm 패키지를 생성하는 것은 간단합니다. TypeScript는 타입 안전성을 추가함으로써 개발자 경험을 향상시키며, 위의 단계를 따라 TypeScript 라이브러리를 세계와 쉽게 공유할 수 있습니다. 즐거운 코딩 되세요!
