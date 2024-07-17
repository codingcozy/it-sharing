---
title: "코딩에 5S 원칙 적용하는 방법"
description: ""
coverImage: "/assets/img/2024-07-07-Usingthe5Sprincipleincoding_0.png"
date: 2024-07-07 02:02
ogImage:
  url: /assets/img/2024-07-07-Usingthe5Sprincipleincoding_0.png
tag: Tech
originalTitle: "Using the 5S principle in coding"
link: "https://medium.com/@santhoshsundar/using-the-5s-principle-in-coding-6237a1614a02"
---

![판크 법인을 사용할 때 5S 원칙 이용] ( /assets/img/2024-07-07-Usingthe5Sprincipleincoding_0.png )

몇 년 전, 저는 애자일 소프트웨어 개발의 기원에 대한 호기심 때문에 도요타 웨이(The Toyota Way)를 읽었습니다. 그러나 도요타의 생산 공정과 효율성 향상에 더 매료되었습니다. 이 책은 많은 소중한 통찰과 채택할 수 있는 실천 방법을 제공했습니다. 개발자로서 5S 원칙이 특히 내 주목을 끌었습니다. 우리가 일하는 데 이 원칙을 상당 부분 적용해 왔지만 체계적인 방식으로 적용하지는 않았다는 것을 깨달았습니다.

5S 원칙은 가치 흐름을 기반으로 한 일본 방법론인 리안 제조로부터 유래되어 효율성과 생산성을 향상시키기 위한 것입니다. 그것은 분류(Sort), 정돈(Set in Order), 광내기(Shine), 표준화(Standardize), 유지(Sustain)를 나타냅니다.

코딩에 5S 원칙을 적용하면 소프트웨어 개발의 다양한 측면을 크게 향상시킬 수 있습니다:

<div class="content-ad"></div>

- 코드 품질 향상: 정기적인 코드 리뷰, 일관된 형식 지정, 표준화된 오류 처리 등을 도입함으로써 팀은 버그를 크게 줄이고 전체적인 코드 품질을 향상시킬 수 있습니다. 이는 더 견고하고 신뢰할 수 있는 소프트웨어를 만들어 유지 및 확장하기 쉽게 만듭니다.
- 기술 부채 감소: 5S 원칙은 코드의 꾸준한 "청소"와 조직화를 촉진합니다. 이 지속적인 유지 보수는 기술 부채 증가를 방지해줍니다. 기술 부채는 쉬운 해결책을 선택하여 더 나은 접근 방식을 사용하는 대신 길게 걸리는 방법으로 처리하면 발생하는 추가 작업의 암묵적인 비용을 말합니다. 문제를 즉시 대처하고 깨끗한 코드 기반을 유지함으로써 팀은 방치되고 엉망인 코드로 인한 장기적 비용을 피할 수 있습니다.
- 팀 생산성 향상: 잘 조직화되고 표준화된 코드 기반은 개발자가 프로젝트를 더 쉽게 탐색할 수 있게 하며, 특정 코드 조각을 찾거나 불명확한 구현을 이해하는 데 드는 시간을 줄입니다. 이 증가된 효율성은 직접적으로 더 빨리 기능을 제공하고 문제를 더 적게 겪을 수 있도록 팀에 반영됩니다.
- 협업 강화: 표준화된 관행과 명확한 조직화는 팀원들이 서로의 작업을 이해하기 쉽게 만듭니다. 이 개선은 더 나은 협업을 도와주며, 신규 팀원의 숙련도를 높이고 팀 내에서 더 효과적인 지식 공유를 가능하게 합니다.
- 지속적인 개선: 5S의 '유지' 측면은 지속적인 개선 문화를 육성합니다. 팀은 주기적으로 자신들의 프로세스와 도구를 반성하고 향상할 부분을 찾아 소프트웨어 개발 분야의 끊임없이 진화하는 최신 최상의 관행을 파악합니다.

이 글에서는 각각의 5S 원칙이 어떻게 코딩에 적용될 수 있는지와 주 프로그래밍 언어로 자바스크립트를 사용하여 이에 대해 탐색해 볼 것입니다. 우리는 구체적인 실행 방법, 이 프로세스를 지원하는 도구, 그리고 이러한 원칙을 개발 워크플로에 통합하는 방법에 대해 논의할 것입니다. 마지막에는 5S 원칙이 코딩 관행을 개선하는 데 어떻게 도움이 되는지에 대해 포괄적으로 이해하게 될 것입니다.

기억해 주세요, 아래 예시와 관행은 모든 프로덕션 환경에 모두 적용되는 것은 아니며 시작점으로 이해해주시면 됩니다.

# 1. 분류(整理) — 작업 공간에서 불필요한 항목을 제거합니다.

<div class="content-ad"></div>

코딩 관점에서는 이것이 코드베이스를 관리하고 이해하기 어렵게 만들 수 있는 사용되지 않는 코드, 종속성 및 기타 쓰레기를 식별하고 제거하는 것을 의미합니다. Sort 원칙을 적용하는 방법과 도움이 될 수 있는 도구들을 함께 살펴보겠습니다.

## 사용되지 않는 코드 식별:

사용되지 않는 코드인 함수, 변수 및 더 이상 필요하지 않은 import 등을 먼저 식별해 보세요. ESLint는 JavaScript 코드에서 이러한 문제를 식별하고 수정하는 데 사용되는 인기 있는 JavaScript 린트 도구입니다. 잠재적인 오류를 분석하고 일관된 코딩 스타일을 시행하여 코드 품질을 유지하고 버그를 줄이는 데 도움이 됩니다.

Before:

<div class="content-ad"></div>

```js
// 사용되지 않는 함수
function unusedFunction() {
  console.log("사용되지 않는 함수입니다");
}

// 주요 함수
function mainFunction() {
  console.log("이 함수는 사용됩니다");
}

mainFunction();
```

```js
import React from "react";
import { useState } from "react";
import { unusedFunction } from "./utils";

const MyComponent = () => {
  const [state, setState] = useState(0);

  return <div>{state}</div>;
};

export default MyComponent;
```

이후:

```js
// 주요 함수
function mainFunction() {
  console.log("이 함수는 사용됩니다");
}

mainFunction();
```

<div class="content-ad"></div>

```js
import React from "react";
import { useState } from "react";

const MyComponent = () => {
  const [state, setState] = useState(0);

  return <div>{state}</div>;
};

export default MyComponent;
```

## 사용되지 않는 종속성 식별 및 제거:

depcheck와 같은 도구를 사용하면 프로젝트에서 사용되지 않는 npm 패키지를 찾을 수 있습니다.

```js
npx depcheck
```

<div class="content-ad"></div>

```js
미사용된 종속성
* lodash
```

```js
npm uninstall lodash
```

# 2. 정리 (Seiton) — 효율과 흐름을 보장하기 위해 항목을 조직화합니다.

코딩의 맥락에서는 프로젝트의 코드베이스를 논리적이고 일관된 방식으로 구조화하고, 파일과 디렉터리를 최적의 이해, 재사용성, 및 유지보수성을 위해 개발자들을 위한 최선의 방법으로 배열하는 것을 의미합니다.

<div class="content-ad"></div>

## 프로젝트 구조 정리:

원자 디자인은 UI 구성 요소를 더 작고 재사용 가능한 부분으로 쪼개어 디자인 시스템을 만드는 방법론입니다. 이 접근 방식은 프로젝트 내 구조와 재사용성을 향상시킵니다.

```js
project/
├── src/
│   ├── components/
│   │   ├── atoms/
│   │   │   └── Button.tsx
│   │   ├── molecules/
│   │   │   └── Form.tsx
│   │   ├── organisms/
│   │   │   └── Header.tsx
│   │   ├── templates/
│   │   │   └── PageTemplate.tsx
│   │   └── pages/
│   │       └── HomePage.tsx
│   ├── assets/
│   │   ├── images/
│   │   └── styles/
│   ├── utils/
│   │   └── helpers.ts
│   └── App.ts
├── public/
│   └── index.html
├── package.json
└── README.md
```

기능 기반 구조: 기능별로 조직화하면 특정 기능과 관련된 모든 파일을 그룹화하여 대규모 응용 프로그램을 보다 쉽게 관리하고 확장할 수 있습니다.

<div class="content-ad"></div>

```js
src/
│
├── features/
│   ├── UserManagement/
│   │   ├── components/
│   │   │   ├── UserProfile.tsx
│   │   │   ├── UserList.tsx
│   │   │   └── UserForm.tsx
│   │   ├── hooks/
│   │   │   ├── useUserData.ts
│   │   │   └── useUserList.ts
│   │   ├── api/
│   │   │   └── userApi.ts
│   │   ├── utils/
│   │   │   └── userFormatters.ts
│   │   └── index.ts
│   │
│   ├── Authentication/
│   │   ├── components/
│   │   │   ├── LoginForm.tsx
│   │   │   └── RegistrationForm.tsx
│   │   ├── hooks/
│   │   │   └── useAuth.ts
│   │   ├── api/
│   │   │   └── authApi.ts
│   │   ├── utils/
│   │   │   └── authValidators.ts
│   │   └── index.ts
│   │
│   └── Dashboard/
│       ├── components/
│       │   ├── DashboardLayout.tsx
│       │   ├── AnalyticsWidget.tsx
│       │   └── ActivityFeed.tsx
│       ├── hooks/
│       │   └── useDashboardData.ts
│       ├── api/
│       │   └── dashboardApi.ts
│       ├── utils/
│       │   └── dashboardHelpers.ts
│       └── index.ts
│
├── shared/
│   ├── components/
│   │   ├── Button.tsx
│   │   ├── Input.tsx
│   │   └── Modal.tsx
│   ├── hooks/
│   │   └── useForm.ts
│   └── utils/
│       ├── formatters.ts
│       └── validators.ts
│
├── App.ts
└── index.ts
```

파일과 디렉토리를 논리적으로 정리하여 쉽게 접근하고 유지보수할 수 있도록 배치하세요.

- 유틸리티 함수는 utils 디렉터리에 배치하세요.
- 관련된 컴포넌트들을 components 디렉터리에 그룹화하여 배치하세요. 원자 디자인 원칙이나 다른 필요에 맞는 패턴을 따라주세요.
- 이미지나 스타일과 같은 정적 자원은 assets 디렉터리에 저장하세요.

주기적으로 프로젝트를 검토하고 정련함으로써 장기적인 프로젝트 성공과 개발자 생산성을 확보할 수 있습니다.

<div class="content-ad"></div>

# 3. Shine (Seiso) — Clean the workspace to maintain standards and identify problems.

코딩 문맥에서는 코드가 깨끗하고 가독성이 있으며 코딩 표준을 준수하는지 확인하는 것을 의미합니다. Shine은 주로 항목을 청소하고 정리하는 것을 의미하지만, 문제를 해결하고 해결하는 것도 포함합니다. 코드베이스를 정기적으로 검토하고 개선함으로써 시스템을 효율적으로 유지하고 시간이 지남에 따라 작업하기 쉽게 만들 수 있습니다. 이러한 관행과 도구를 개발 워크플로에 통합함으로써 코드의 가독성, 유지보수성, 전체 프로젝트 품질을 향상시킬 수 있습니다.

## 코드 포매팅 및 스타일:

Prettier 및 ESLint와 같은 도구를 사용하여 일관된 코드 포매팅 및 품질을 보장할 수 있습니다.

<div class="content-ad"></div>

```js
// 서식이 적용되기 전
function fetchData() {
  return fetch("https://api.example.com/data")
    .then((response) => response.json())
    .then((data) => {
      console.log(data);
    });
}
```

```js
// Prettier로 서식이 적용된 후
function fetchData() {
  return fetch("https://api.example.com/data")
    .then((response) => response.json())
    .then((data) => {
      console.log(data);
    });
}
```

## 성능 최적화:

알고리즘을 최적화하고 복잡성을 줄이며 효율적인 데이터 구조를 사용하여 코드 성능을 향상시킵니다. SonarQube와 같은 도구를 사용하면 포괄적인 통찰력을 제공합니다.

<div class="content-ad"></div>

```js
// 개선 전 (최적화 필요)
function findMax(arr) {
  let max = arr[0];
  for (let i = 1; i < arr.length; i++) {
    if (arr[i] > max) {
      max = arr[i];
    }
  }
  return max;
}
```

```js
// 최적화 후
function findMax(arr) {
  return Math.max(...arr);
}
```

## 정기적인 리팩토링:

기능을 변경하지 않고 코드를 정리하고 개선하여 가독성과 유지보수성을 향상시키는 것을 의미합니다. 개선 사항을 정기적으로 확인하고 수정하는 것이 중요합니다. 다른 팀으로부터 코드베이스를 상속 받으면 효율성을 위해 리팩토링이 중요합니다.

<div class="content-ad"></div>

```js
// 단순화하기 전 (개선이 필요함)
function calculateTotalPrice(products) {
  let totalPrice = 0;
  for (let i = 0; i < products.length; i++) {
    totalPrice += products[i].price;
  }
  return totalPrice;
}
```

```js
// 개선 후
function calculateTotalPrice(products) {
  return products.reduce((total, product) => total + product.price, 0);
}
```

# 4. 표준화 (Seiketsu) — 조직과 효율을 유지하기 위한 표준 및 절차 설정.

코딩에서의 표준화는 프로젝트나 조직 전체에 일관된 관행, 규칙 및 지침을 수립하고 준수하는 것을 의미합니다. 이 원칙은 코드 품질, 가독성 및 유지 보수성을 향상시키고 오류를 줄이며 팀원 간의 협업을 증진시키는 데 도움이 됩니다.

<div class="content-ad"></div>

## 네이밍 규칙:

변수, 함수, 클래스 및 파일에 일관된 및 설명적인 네이밍 규칙을 사용하세요. 이러한 방법을 통해 코드를 더 읽기 쉽고 유지보수하기 쉽게 만들 수 있습니다. 몇 가지 예시:

변수 및 함수 이름에는 camelCase를 사용하세요.

```js
const userName = "JohnDoe";
let userAge = 30;

function fetchUserData() {
  // 사용자 데이터 가져오기
}
```

<div class="content-ad"></div>

```js
class UserProfile {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
}
```

## 스타일 가이드 수립 및 활용:

스타일 가이드는 코드베이스 전체에서 일관성을 유지하기 위한 코딩 규약과 모범 사례를 정의합니다. 귀하의 요구 사항을 충족하는 스타일 가이드를 선택하여 작성하거나 이미 존재하는 스타일 가이드 중 하나를 선택할 수 있습니다.

<div class="content-ad"></div>

에어비앤비 자바스크립트 스타일 가이드: 자바스크립트 코딩의 다양한 측면을 다루는 인기 있는 스타일 가이드입니다.

구글 타입스크립트 스타일 가이드: 구글의 스타일 가이드는 타입스크립트에 대한 포괄적인 코딩 표준과 관행을 제공합니다.

```js
// 표준화 전
function fetchData() {
  return fetch("https://api.example.com/data")
    .then((response) => response.json())
    .then((data) => {
      console.log(data);
    });
}

// 표준화 후
function fetchData() {
  return fetch("https://api.example.com/data")
    .then((response) => response.json())
    .then((data) => {
      console.log(data);
    });
}
```

## 일관된 에러 처리:

<div class="content-ad"></div>

에러 처리 및 로깅 관행을 표준화하여 일관성을 유지하고 디버깅을 개선하는 것이 중요합니다.

```js
// 표준화 전
function fetchData() {
  return fetch("https://api.example.com/data")
    .then((response) => response.json())
    .catch((error) => {
      console.error(error);
    });
}

// 표준화 후
async function fetchData() {
  try {
    const response = await fetch("https://api.example.com/data");
    const data = await response.json();
    return data;
  } catch (error) {
    console.error("데이터를 불러오는 중 오류 발생:", error);
    throw error;
  }
}
```

## 테스팅 표준:

테스트 작성과 구성에 대한 표준을 정의하고, 명명 규칙과 코드 커버리지 요구 사항을 포함하여 담아보세요.

<div class="content-ad"></div>

일관된 네이밍 규칙을 사용하여 테스트 파일을 생성해보세요. 예를 들어, filename.test.js와 같이 이름 짓기:

```js
// fetchData.test.js
const fetchData = require("./fetchData");

test("API에서 데이터를 가져옵니다", async () => {
  const data = await fetchData();
  expect(data).toBeDefined();
});
```

코드 커버리지 임계값을 측정하고 설정해보세요:
이것은 커버리지 보고서가 생성되고, 최소 커버리지 임계값이 설정되며, 커버리지 분석에서 포함하거나 제외할 파일을 지정합니다.

<div class="content-ad"></div>

```js
module.exports = {
  collectCoverage: true,
  coverageDirectory: "coverage",
  coverageProvider: "v8",
  coverageReporters: ["text", "lcov", "clover"],
  coverageThreshold: {
    global: {
      branches: 80,
      functions: 80,
      lines: 80,
      statements: 80,
    },
  },
  collectCoverageFrom: ["src/**/*.{js,jsx,ts,tsx}", "!src/**/*.d.ts", "!src/index.tsx", "!src/serviceWorker.ts"],
};
```

## Git을 활용한 버전 관리:

Git과 같은 버전 관리 시스템을 사용하여 코드 관리를 표준화하고 일관성 있게 유지하며 협업을 증진시키세요. 또한 표준 브랜치 네이밍 규칙을 사용하세요:

- 기능 브랜치: feature/짧은-설명
- 버그 수정 브랜치: bugfix/짧은-설명
- 릴리스 브랜치: release/x.y.z

<div class="content-ad"></div>

```js
주요

기능/사용자 인증
기능/대시보드 개선

버그 수정/로그인 오류

릴리스/1.0.0
```

# 5. 유지 (Shitsuke) — 장기적인 준수를 보장하도록 규범을 정기적으로 유지 및 검토하십시오.

5S의 다섯 번째이자 마지막 원칙은 시간이 흐름에 따라 표준을 유지하고 지속적으로 개선함을 강조합니다. 이 원칙은 이전 네 가지 원칙에 의해 수립된 관행이 준수되고 정기적인 업무흐름의 일환으로 변하는 것을 보장합니다. 소프트웨어 개발 분야에서는 지속적인 통합, 정기적인 코드 검토, 자동화된 테스트, 그리고 새로운 기술과 관행에 대한 계속된 학습 및 적응이 포함됩니다.

## Git Hooks 사용법

<div class="content-ad"></div>

Git 훅과 Husky를 사용하면 Git 워크플로의 다양한 단계에서 자동으로 체크와 작업을 수행할 수 있어요. 이를 통해 코드 품질을 유지하고, 커밋 전에 테스트를 통과하고, 팀 전체에서 커밋 메시지 규칙을 강제할 수 있어요.

```js
// package.json

{
  "scripts": {
    "prepare": "husky install"
  },
  "husky": {
    "hooks": {
      "pre-commit": "npm run lint && npm test",
      "pre-push": "npm run build",
      "commit-msg": "commitlint -E HUSKY_GIT_PARAMS"
    }
  }
}
```

## 지속적 통합과 지속적 배포 (CI/CD):

CI/CD 파이프라인을 구축하여 코드 변경 사항이 자동으로 테스트되고, 통합되며, 배포되도록 보장할 수 있어요. 아래는 GitHub Actions를 사용한 예시에요.

<div class="content-ad"></div>

메인 브랜치에 대한 모든 푸시 또는 풀 리퀘스트가 CI 파이프라인을 트리거하도록 설정하세요. 이렇게 하면 테스트 및 린팅이 자동으로 실행됩니다.

```js
name: CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Set up Node.js
        uses: actions/setup-node@v2
        with:
          node-version: '14'

      - name: Install dependencies
        run: npm install

      - name: Run tests
        run: npm test

      - name: Lint code
        run: npm run lint
```

## 코드 문서화:

코드 문서화를 유지하여 코드 가독성과 유지 관리성을 향상시키세요. JSDoc와 같은 도구를 사용하여 문서를 생성하세요.

<div class="content-ad"></div>

```js
/**
 * 직사각형의 넓이를 계산합니다.
 *
 * @param {number} length - 직사각형의 길이입니다.
 * @param {number} width - 직사각형의 너비입니다.
 * @returns {number} 직사각형의 넓이입니다.
 * @throws {Error} 만약 길이 또는 너비 중 하나라도 음수이면 발생합니다.
 *
 * @example
 * // 50을 반환합니다
 * calculateRectangleArea(10, 5);
 *
 * @since 1.0.0
 * @author Jane Doe <jane@example.com>
 */
function calculateRectangleArea(length, width) {
  if (length < 0 || width < 0) {
    throw new Error("길이와 너비는 음수일 수 없습니다");
  }
  return length * width;
}
```

## 정기적인 코드 리뷰:

코딩 표준과 최선의 실천 방법을 준수하는지 확인하기 위해 정기적인 코드 리뷰를 진행하십시오.

코드 리뷰 체크리스트:

<div class="content-ad"></div>

- 코드는 스타일 가이드 (예: Airbnb, Google)를 준수합니다.
- 기능이 정확하고 요구 사항을 충족합니다.
- 코드는 깨끗하고 가독성이 있으며 유지 관리가 쉽습니다.
- 테스트가 충분히 포함되어 있습니다.
- 성능 고려 사항이 다루어집니다.

## 지속적인 학습과 적응:

소프트웨어 산업이 빠르게 발전하기 때문에 최신 정보를 파악하는 것이 중요합니다. 지속적인 기술 습득은 개발자가 새로운 기술을 적용하여 문제를 해결하고 새로운 경력 기회를 탐색하는 데 도움이 됩니다.

- 온라인 코스, 책, 동영상 등을 활용하여 JavaScript 개발의 최신 동향, 기술 및 최선의 사례를 파악합니다.
- JavaScript 커뮤니티 (예: Stack Overflow, GitHub)에 참여합니다.
- 컨퍼런스 및 밋업 (예: JSConf, 지역 JavaScript 밋업)에 참석합니다.

<div class="content-ad"></div>

이러한 원칙을 실현하는 것은 처음에는 어려울 수 있습니다. 특히 큰 규모의 프로젝트나 팀에서는 그렇습니다. 그러나 장기적인 혜택은 처음에 시간과 노력을 투자한 것보다 훨씬 큽니다. 더 깨끗하고 조직적인 코드는 버그를 줄이고, 새로운 팀원을 온보딩하기 쉽게 만들며, 보다 더 효율적인 개발 주기를 제공합니다.

5S 코딩의 목표는 완벽함이 아니라 지속적인 개선입니다. 작게 시작해보세요. 예를 들어 프로젝트에 린터를 도입하거나 코드 리뷰 프로세스를 수립하는 것부터 시작할 수 있습니다. 팀이 이러한 변화에 익숙해지면, 점차적으로 더 많은 5S 실천 방안을 도입해보세요.

참고 자료:

- https://www.toyotaforklift.com/resource-library/blog/toyota-solutions/toyota-lean-management-and-5s
- https://thetoyotaway.org/product/the-toyota-way/
