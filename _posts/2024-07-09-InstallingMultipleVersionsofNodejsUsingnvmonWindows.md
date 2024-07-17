---
title: "윈도우에서 nvm을 사용해 여러 버전의 Nodejs 설치 방법"
description: ""
coverImage: "/assets/img/2024-07-09-InstallingMultipleVersionsofNodejsUsingnvmonWindows_0.png"
date: 2024-07-09 17:20
ogImage:
  url: /assets/img/2024-07-09-InstallingMultipleVersionsofNodejsUsingnvmonWindows_0.png
tag: Tech
originalTitle: "Installing Multiple Versions of Node.js Using nvm on Windows"
link: "https://medium.com/@ahmedm3bead/installing-multiple-versions-of-node-js-using-nvm-on-windows-9123751b8bc2"
---

<img src="/assets/img/2024-07-09-InstallingMultipleVersionsofNodejsUsingnvmonWindows_0.png" />

윈도우 시스템에 여러 버전의 Node.js를 설치하는 것은 굉장히 유용할 수 있습니다, 특히 다른 Node.js 버전이 필요한 프로젝트를 작업하고 있을 때입니다. 이 안내서에서는 Windows용 Node 버전 관리자인 nvm-windows를 사용하여 여러 Node.js 버전을 설치하고 관리하는 방법을 살펴보겠습니다.

# 단계 1: nvm-windows 다운로드 및 설치

- GitHub의 nvm-windows 릴리스 페이지로 이동합니다.
- 최신 nvm-setup.zip 파일을 다운로드합니다.
- 다운로드한 ZIP 파일을 압축 해제합니다.
- nvm-setup.exe 파일을 실행하여 시스템에 nvm-windows를 설치합니다.

<div class="content-ad"></div>

# 단계 2: nvm-windows를 사용하여 Node.js 버전 설치하기

- 명령 프롬프트(cmd.exe)를 엽니다.
- 다음 명령어를 사용하여 특정 버전의 Node.js를 설치합니다:

```js
nvm install <version_number>
```

- 예를 들어, Node.js 버전 14를 설치하려면 다음을 실행하면 됩니다:

<div class="content-ad"></div>

```js
nvm install 14
```

- Node.js 버전 16을 설치하려면 다음을 실행합니다:

```js
nvm install 16
```

# 단계 3: 특정 Node.js 버전 사용하기

<div class="content-ad"></div>

- 특정 버전의 Node.js를 사용하려면 명령 프롬프트에서 다음 명령을 실행하십시오:

```js
nvm use <version_number>
```

- 예를 들어, Node.js 버전 14로 전환하려면 다음과 같이 실행하면 됩니다:

```js
nvm use 14
```

<div class="content-ad"></div>

# 단계 4: 설치 확인

- 설치와 현재 Node.js 버전을 확인하려면 명령 프롬프트에서 다음 명령을 실행하세요:

```js
node - v;
```

# 추가 명령

<div class="content-ad"></div>

- 설치된 Node.js 버전 목록:

```js
nvm list
```

- 기본 Node.js 버전 설정:

```js
nvm alias default <version_number>
```

<div class="content-ad"></div>

그거야! 이제 윈도우 시스템에 여러 버전의 Node.js가 설치되어 있고 nvm-windows를 사용하여 쉽게 전환할 수 있어요.
