---
title: "shadcn-ui CLI 작동 방식 분석  Part 28 코드베이스 자세히 들여다보기"
description: ""
coverImage: "/assets/img/2024-07-09-shadcn-uiuicodebaseanalysisHowdoesshadcn-uiCLIworkPart28_0.png"
date: 2024-07-09 13:41
ogImage:
  url: /assets/img/2024-07-09-shadcn-uiuicodebaseanalysisHowdoesshadcn-uiCLIworkPart28_0.png
tag: Tech
originalTitle: "shadcn-ui ui codebase analysis: How does shadcn-ui CLI work? — Part 2.8"
link: "https://medium.com/@ramu.narasinga_61050/shadcn-ui-ui-codebase-analysis-how-does-shadcn-ui-cli-work-part-2-8-e04653ac69db"
---

shadcn-ui CLI의 작동 방식을 알아보려고 했어요. 이 글에서는 shadcn-ui/ui CLI를 구축하는 데 사용된 코드에 대해 이야기하고 있어요.

2.7 부에서 `isTypescriptProject` 함수를 살펴봤어요. 이 함수는 cwd (현재 작업 디렉토리)에 tsconfig.json 파일이 있는지 확인해요.

이제 다음 코드 라인으로 넘어가 봅시다.

![](/assets/img/2024-07-09-shadcn-uiuicodebaseanalysisHowdoesshadcn-uiCLIworkPart28_0.png)

<div class="content-ad"></div>

우리는 getProjectConfig의 구현 세부 사항을 2.1-2.7 부분에서 살펴봤어요. cli/src/commands/init.ts로 돌아가 봅시다.

![이미지](/assets/img/2024-07-09-shadcn-uiuicodebaseanalysisHowdoesshadcn-uiCLIworkPart28_1.png)

아래 코드 조각에서 promptForMinimalConfig 함수를 이해해 봅시다:

```js
if (projectConfig) {
  const config = await promptForMinimalConfig(cwd, projectConfig, opts.defaults);
  await runInit(cwd, config);
}
```

<div class="content-ad"></div>

# promptForMinimalConfig

이 코드는 cli/commands/init.ts에서 선택된 것입니다.

```js
export async function promptForMinimalConfig(cwd: string, defaultConfig: Config, defaults = false) {
  const highlight = (text: string) => chalk.cyan(text);
  let style = defaultConfig.style;
  let baseColor = defaultConfig.tailwind.baseColor;
  let cssVariables = defaultConfig.tailwind.cssVariables;

  if (!defaults) {
    const styles = await getRegistryStyles();
    const baseColors = await getRegistryBaseColors();

    const options = await prompts([
      {
        type: "select",
        name: "style",
        message: `어떤 ${highlight("스타일")}을 사용하시겠습니까?`,
        choices: styles.map((style) => ({
          title: style.label,
          value: style.name,
        })),
      },
      {
        type: "select",
        name: "tailwindBaseColor",
        message: `${highlight("기본 색")}으로 사용하고 싶은 색은 무엇인가요?`,
        choices: baseColors.map((color) => ({
          title: color.label,
          value: color.name,
        })),
      },
      {
        type: "toggle",
        name: "tailwindCssVariables",
        message: `${highlight("CSS 변수")}를 사용하시겠습니까?`,
        initial: defaultConfig?.tailwind.cssVariables,
        active: "예",
        inactive: "아니요",
      },
    ]);

    style = options.style;
    baseColor = options.tailwindBaseColor;
    cssVariables = options.tailwindCssVariables;
  }

  const config = rawConfigSchema.parse({
    $schema: defaultConfig?.$schema,
    style,
    tailwind: {
      ...defaultConfig?.tailwind,
      baseColor,
      cssVariables,
    },
    rsc: defaultConfig?.rsc,
    tsx: defaultConfig?.tsx,
    aliases: defaultConfig?.aliases,
  });

  // 파일로 쓰기
  logger.info("");
  const spinner = ora(`components.json을 작성 중...`).start();
  const targetPath = path.resolve(cwd, "components.json");
  await fs.writeFile(targetPath, JSON.stringify(config, null, 2), "utf8");
  spinner.succeed();

  return await resolveConfigPaths(cwd, config);
}
```

여기에는 많은 코드가 있습니다. 작은 코드 조각을 이해하는 것으로 나누어 보겠습니다.

<div class="content-ad"></div>

## 매개변수:

`promptForMinimalConfig` 함수는 `cwd`라는 매개변수와 함께 호출됩니다 — 이것은 현재 작업 디렉토리를 나타냅니다. `projectConfig` — 이것은 `getProjectConfig`에서 반환된 값입니다. `opts.defaults` — 이것은 Commander.js로 구성된 CLI 인수이며 아래와 같이 표시됩니다:

```js
export const init = new Command()
  .name("init")
  .description("프로젝트를 초기화하고 종속성을 설치합니다.")
  .option("-y, --yes", "확인 프롬프트를 건너뛰기.", false)
  .option("-d, --defaults,", "기본 구성 사용.", false)
  .option("-c, --cwd <cwd>", "작업 디렉토리. 현재 디렉토리로 기본 설정됩니다.", process.cwd());
```

## Chalk

<div class="content-ad"></div>

```js
const highlight = (text: string) => chalk.cyan(text);
```

위의 코드 라인은 packages/cli/src/commands/init.ts#L232에서 찾을 수 있습니다. highlight는 터미널에 표시되는 텍스트에 색상을 적용하는 작은 유틸 함수입니다. chalk를 사용하여 색상을 적용합니다.

chalk 패키지에 대한 자세한 내용은 여기에서 확인할 수 있습니다.

다음 `promptForMinimalConfig` 스타일의 몇 줄의 코드에서, baseColor와 cssVariables는 defaultConfig에서 설정된 것으로 발견됩니다.

<div class="content-ad"></div>

```javascript
let style = defaultConfig.style;
let baseColor = defaultConfig.tailwind.baseColor;
let cssVariables = defaultConfig.tailwind.cssVariables;
```

## getRegistryStyles

만약 CLI에서 초기화 명령을 실행할 때 `-d` 또는 `— defaults`와 같은 옵션이 전달되지 않으면, if 블록 안에서 정렬해야 할 몇 가지 사항이 있습니다.

```javascript
if (!defaults) {
    const styles = await getRegistryStyles();
```

<div class="content-ad"></div>

getRegistryStyles는 utils/registry/index.ts#L29에서 가져온 것입니다.

```js
export async function getRegistryStyles() {
  try {
    const [result] = await fetchRegistry(["styles/index.json"]);

    return stylesSchema.parse(result);
  } catch (error) {
    throw new Error(`레지스트리에서 스타일을 가져오는데 실패했습니다.`);
  }
}
```

이 코드는 fetchRegistry라는 함수를 호출합니다. 이에 대해 더 자세히 다음 기사에서 살펴보겠습니다.

# 결론:

<div class="content-ad"></div>

지금 shadcn-ui/ui CLI 소스 코드 분석에서 getProjectConfig 함수가 완료되었습니다. 이 함수에 이어서 몇 줄의 코드를 더 살펴보았습니다.

이 함수에는 promptForMinimalConfig라는 함수가 있습니다.

```js
export async function promptForMinimalConfig(
  cwd: string,
  defaultConfig: Config,
  defaults = false
) {
  const highlight = (text: string) => chalk.cyan(text)
  let style = defaultConfig.style
  let baseColor = defaultConfig.tailwind.baseColor
  let cssVariables = defaultConfig.tailwind.cssVariables
```

이 함수에는 `기본 색상으로 사용할 색상은 무엇입니까`, `기본 색상으로 사용할 색상은 무엇입니까`, `CSS 변수를 사용하시겠습니까`와 같은 프롬프트가 포함되어 있습니다. 이 프롬프트가 어떻게 구성되는지는 정확히 파악하지 못했지만, highlight는 chalk 패키지를 사용하여 터미널에서 보이는 텍스트에 색상을 적용하는 작은 유틸 함수임을 발견했습니다.

<div class="content-ad"></div>

# 나에 대해:

웹사이트: [https://ramunarasinga.com/](https://ramunarasinga.com/)

Linkedin: [https://www.linkedin.com/in/ramu-narasinga-189361128/](https://www.linkedin.com/in/ramu-narasinga-189361128/)

Github: [https://github.com/Ramu-Narasinga](https://github.com/Ramu-Narasinga)

<div class="content-ad"></div>

이메일: ramu.narasinga@gmail.com

shadcn-ui/ui를 처음부터 빌드하기

# 참고자료:

- https://github.com/shadcn-ui/ui/blob/main/packages/cli/src/commands/init.ts#L232
- https://github.com/shadcn-ui/ui/blob/main/packages/cli/src/commands/init.ts#L70C7-L77C8
- https://github.com/shadcn-ui/ui/blob/main/packages/cli/src/utils/registry/index.ts#L29
