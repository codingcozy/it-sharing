---
title: "ESLint 구성 파일을 검사하는 방법"
description: ""
coverImage: "/assets/img/2024-07-09-HowtoinspectESLintconfigurationfile_0.png"
date: 2024-07-09 17:34
ogImage:
  url: /assets/img/2024-07-09-HowtoinspectESLintconfigurationfile_0.png
tag: Tech
originalTitle: "How to inspect ESLint configuration file"
link: "https://medium.com/javascript-everyday/how-to-inspect-eslint-configuration-file-b7c23455b02e"
---

![이미지](/assets/img/2024-07-09-HowtoinspectESLintconfigurationfile_0.png)

ESLint은 프로젝트에서 정적 분석을 통해 품질을 강화하는 뛰어난 도구이며, 이제 구성 프로세스를 간소화하는 평탄한 구성을 사용합니다.

내 시각에서는 ESLint가 설정된 후에 예상대로 작동한다면 훌륭하지만, 구성 파일을 디버깅하는 것은 쉽지 않았습니다.

다행히, 평탄한 구성을 사용하면 ESLint Config Inspector라는 유용한 개발자 도구를 사용할 수 있습니다. 이에 대해 더 자세히 알아볼 예정입니다.

<div class="content-ad"></div>

가정해 봅시다. Angular 애플리케이션에 ESLint를 추가했고, eslint.config.js 파일이 다음과 같이 보입니다:

```js
// @ts-check
const eslint = require("@eslint/js");
const tseslint = require("typescript-eslint");
const angular = require("angular-eslint");

module.exports = tseslint.config(
  {
    files: ["**/*.ts"],
    extends: [
      eslint.configs.recommended,
      ...tseslint.configs.recommended,
      ...tseslint.configs.stylistic,
      ...angular.configs.tsRecommended,
    ],
    processor: angular.processInlineTemplates,
    rules: {
      "@angular-eslint/directive-selector": [
        "error",
        {
          type: "attribute",
          prefix: "app",
          style: "camelCase",
        },
      ],
      "@angular-eslint/component-selector": [
        "error",
        {
          type: "element",
          prefix: "app",
          style: "kebab-case",
        },
      ],
    },
  },
  {
    files: ["**/*.html"],
    extends: [...angular.configs.templateRecommended, ...angular.configs.templateAccessibility],
    rules: {},
  }
);
```

이것은 잘 작동합니다; TypeScript 및 HTML 파일이 모두 고려됩니다. 하지만 실제로 내부에서 어떻게 처리되는 걸까요?

다음 명령을 실행하여 ESLint Config Inspector를 시작해 봅시다:

<div class="content-ad"></div>

```js
npx eslint --inspect-config
```

<img src="/assets/img/2024-07-09-HowtoinspectESLintconfigurationfile_1.png" />

인터페이스는 세 개의 탭으로 구성되어 있습니다:

- Configs
- Rules
- Files

<div class="content-ad"></div>

구성

이 뷰는 ESLint 구성에서 모든 구성 객체 (구성 항목)를 표시합니다. 총 19개의 구성 항목이 있지만, 그 출처가 명확하지 않을 수 있습니다. TypeScript 파일에 적용되는 구성을 살펴보면, 세 번째 당사자 항목을 네 개 확장한다는 것을 알 수 있습니다:

```js
module.exports = tseslint.config(
  {
    files: ["**/*.ts"],
    extends: [
      eslint.configs.recommended,
      ...tseslint.configs.recommended,
      ...tseslint.configs.stylistic,
      ...angular.configs.tsRecommended,
    ],
    ...
  },
  ...
);
```

HTML 파일의 경우, 세 번째 당사자 항목이 두 개 있습니다.

<div class="content-ad"></div>

```js
module.exports = tseslint.config(
  {
    files: ["**/*.html"],
    extends: [
      ...angular.configs.templateRecommended,
      ...angular.configs.templateAccessibility,
    ],
    rules: {},
  },
  ...
);
```

tseslint.configs.recommended 항목에 초점을 맞춰서 Configs 뷰가 19개의 구성 항목을 보여주는 이유를 이해해 봅시다. 이 항목은 구성 객체의 배열을 포함하고 있는데, 이것이 총 개수에 상당한 영향을 미치고 있습니다.

![ESLint 구성 파일 검사 방법](/assets/img/2024-07-09-HowtoinspectESLintconfigurationfile_2.png)

- base: 플러그인 및 파서 필드를 설정합니다.

<div class="content-ad"></div>

![eslint-recommended](/assets/img/2024-07-09-HowtoinspectESLintconfigurationfile_3.png)

- eslint-recommended: Adjust rules from the eslint.configs.recommended to work well with TypeScript files.

![recommended](/assets/img/2024-07-09-HowtoinspectESLintconfigurationfile_4.png)

- recommended: Configure rules specific to TypeScript files.

<div class="content-ad"></div>

![이미지](/assets/img/2024-07-09-HowtoinspectESLintconfigurationfile_5.png)

해당 테이블에서 규칙 이름 앞의 아이콘이 제공된 구성 객체가 규칙의 심각도(off, warning, error)에 어떤 영향을 미치는지 쉽게 볼 수 있습니다. 이는 해당 규칙이 권장되는지, 자동 수정을 지원하는지, 그리고 간단한 설명을 제공합니다.

![이미지](/assets/img/2024-07-09-HowtoinspectESLintconfigurationfile_6.png)

요약하면, Configs 탭을 통해 ESLint 구성 파일에 적용되는 모든 구성 객체를 검사할 수 있습니다.

<div class="content-ad"></div>

규칙

이 뷰는 Configs 탭에서 본 항목들에 구성된 규칙을 보여줍니다.

![image](/assets/img/2024-07-09-HowtoinspectESLintconfigurationfile_7.png)

처음에는 많은 필터가 있어 처음에는 압도될 수 있습니다. 규칙을 필터링하는 방법은 다음과 같습니다:

<div class="content-ad"></div>

- 플러그인 (규칙의 플러그인)
- 사용법 (규칙의 심각도 및 그룹화된 필터)
- 상태 (규칙의 메타 정보 – 권장되는지, 수정 가능한지, 사용 중지된 것인지)

마지막으로, 검색 기능이 있어서 구를 문구로 필터링할 수 있습니다.

이 보기의 가장 강력한 기능은 주어진 구성 객체가 규칙의 결과 설정에 어떻게 영향을 미치는지를 보여줄 수 있다는 것입니다.

![이미지](/assets/img/2024-07-09-HowtoinspectESLintconfigurationfile_8.png)

<div class="content-ad"></div>

![이미지](/assets/img/2024-07-09-HowtoinspectESLintconfigurationfile_9.png)

구성 항목이 구성에서 이후에 정의된 경우 규칙의 심각도 수준을 덮어쓰고 우선시합니다. 이는 특정 규칙이 최종 구성에서 활성화되거나 비활성화된 이유를 이해하고 싶을 때 유용합니다.

파일

마지막으로, 특정 파일에 초점을 맞추어 해당 파일에 적용되는 구성 객체를 확인할 수 있습니다.

<div class="content-ad"></div>

아래는 동일한 표시를 Markdown 형식으로 제공합니다.

![HowtoinspectESLintconfigurationfile_10](/assets/img/2024-07-09-HowtoinspectESLintconfigurationfile_10.png)

![HowtoinspectESLintconfigurationfile_11](/assets/img/2024-07-09-HowtoinspectESLintconfigurationfile_11.png)

또한 일치하는 규칙 목록을 확인하고 어떤 구성 개체가 규칙의 원천인지 확인할 수 있습니다.

![HowtoinspectESLintconfigurationfile_12](/assets/img/2024-07-09-HowtoinspectESLintconfigurationfile_12.png)

<div class="content-ad"></div>

![이미지](/assets/img/2024-07-09-HowtoinspectESLintconfigurationfile_13.png)

ESLint Config Inspector은 ESLint의 flat configuration을 사용하는 개발자에게 귀중한 도구입니다. 이 도구는 구성 객체, 규칙 및 해당 파일에 대한 적용의 자세한 분석을 제공합니다. 이를 통해 설정을 디버깅하고 이해하는 것이 더 쉬워집니다. 이 도구는 ESLint 설정을 명확하게 표시하여 프로젝트 전반에서 예상대로 작동하도록 보장하므로 시간을 절약하고 귀찮음을 줄일 수 있습니다.

제 블로그 글을 좋아해주셨으면 좋겠어요. 읽어주셔서 감사합니다! 🙂
