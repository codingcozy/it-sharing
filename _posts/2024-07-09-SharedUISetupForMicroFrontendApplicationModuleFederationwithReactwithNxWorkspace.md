---
title: "Nx 워크스페이스로 React와 모듈 연합을 이용한 마이크로 프론트엔드 애플리케이션의 공유 UI 설정 방법"
description: ""
coverImage: "/assets/img/2024-07-09-SharedUISetupForMicroFrontendApplicationModuleFederationwithReactwithNxWorkspace_0.png"
date: 2024-07-09 16:28
ogImage:
  url: /assets/img/2024-07-09-SharedUISetupForMicroFrontendApplicationModuleFederationwithReactwithNxWorkspace_0.png
tag: Tech
originalTitle: "Shared UI Setup For Micro Frontend Application (Module Federation with React) with Nx Workspace"
link: "https://medium.com/javascript-in-plain-english/shared-ui-setup-for-micro-frontend-application-module-federation-with-react-with-nx-workspace-7bdffdc0161d"
---

아래는 Markdown 형식으로 테이블 태그를 변경한 것입니다.

![이미지](/assets/img/2024-07-09-SharedUISetupForMicroFrontendApplicationModuleFederationwithReactwithNxWorkspace_0.png)

이 튜토리얼에서는 Nx Workspace, React 및 Tailwind CSS를 사용하여 Micro Frontend 애플리케이션용 공유 UI 라이브러리를 설정하는 방법을 안내합니다. UI 구성 요소로 Shadcn UI를 사용할 것입니다.

# 최종 구현 링크

튜토리얼의 최종 구현은 다음 리포지토리 커밋에서 확인할 수 있습니다:

<div class="content-ad"></div>

- Shadcn 컴포넌트를 포함한 UI 패키지를 추가하고 앱에서 사용해보세요.
- Button 컴포넌트를 포함한 UI 패키지를 추가하고 의존성을 업데이트하세요.

# 준비 사항

시작하기 전에 다음 사항이 설정되어 있는지 확인하세요:

- ESLint, Prettier 및 Husky 구성을 사용한 Nx 워크스페이스 생성을 위한 기본 저장소.
- Nx 워크스페이스를 사용한 Micro Frontend 아키텍처 빌드를 위한 기본 저장소.
- Nx 워크스페이스를 사용한 Micro Frontend 애플리케이션을 위한 공유 Tailwind 설정.
- Nx 워크스페이스: Nx는 Google, Facebook 및 Microsoft과 같이 개발을 지원하는 확장 가능한 개발 도구 모음입니다.
- Nx Console: Nx Console은 Nx CLI에 대한 UI를 제공하는 Visual Studio Code 확장 프로그램입니다.
- React: 사용자 인터페이스를 구축하기 위한 JavaScript 라이브러리.
- Tailwind CSS: 사용자 정의 디자인을 신속하게 구축할 수 있는 유틸리티 중심의 CSS 프레임워크.
- ESLint: JavaScript에서 패턴을 식별하고 보고하는 데 사용되는 플러그 가능하고 구성 가능한 린터 도구.
- Prettier: 일관된 코드 스타일을 강제하는 의견이 분분한 코드 포매터.
- Netlify: 지속적인 배포, 서버리스 함수 등을 제공하는 플랫폼.
- Shadcn UI: 앱에 복사하여 붙여넣을 수 있는 아름답게 디자인된 컴포넌트. 접근성 높음. 사용자 정의 가능. 오픈 소스.

<div class="content-ad"></div>

# 목차

- UI 라이브러리 생성
- Tailwind CSS 설정 추가
- Shadcn UI 설정
- 버튼 구성요소 추가
- Shadcn UI 호버 카드 추가
- Shadcn UI 배지 추가
- 결론

# UI 라이브러리 생성

먼저, Nx Workspace를 사용하여 UI 라이브러리를 생성해야 합니다. UI 라이브러리를 생성하기 위해 @nx/react:library 생성자를 사용할 것입니다.

<div class="content-ad"></div>

```js
pnpm exec nx generate @nx/react:library --name=ui --bundler=vite --directory=packages/ui --projectNameAndRootFormat=as-provided --no-interactive
```

![Image](/assets/img/2024-07-09-SharedUISetupForMicroFrontendApplicationModuleFederationwithReactwithNxWorkspace_1.png)

# Tailwind CSS 설정 추가

다음으로, UI 라이브러리에 Tailwind CSS 설정을 추가해야 합니다. Tailwind CSS 설정을 추가하기 위해 @nx/react:setup-tailwind 생성기를 사용하겠습니다.

<div class="content-ad"></div>

```js
npm exec nx generate @nx/react:setup-tailwind --project=ui --no-interactive
```

- Tailwind Config 수정: 다음 내용으로 packages/ui/tailwind.config.js 파일을 업데이트합니다:

```js
/* eslint-disable @typescript-eslint/unbound-method */
/* eslint-disable @typescript-eslint/no-var-requires */
const { createGlobPatternsForDependencies } = require("@nx/react/tailwind");
const { join } = require("path");
const baseConfig = require("../../tailwind.config");

/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    ...(baseConfig?.content || []),
    join(__dirname, "{src,pages,components,app}/**/*!(*.stories|*.spec).{ts,tsx,html}"),
    ...createGlobPatternsForDependencies(__dirname),
  ],
  ...baseConfig,
};
```

- PostCSS Config 수정: 다음 내용으로 packages/ui/postcss.config.js 파일을 업데이트합니다:

<div class="content-ad"></div>

```js
/* eslint-disable @typescript-eslint/unbound-method */
const { join } = require("path");

// 참고: 라이브러리별 PostCSS/Tailwind 구성을 사용하는 경우, 프로젝트의 구성(즉, project.json)에서 `postcssConfig` 빌드 옵션을 제거해야 합니다.
//
// 참조: https://nx.dev/guides/using-tailwind-css-in-react#step-4:-applying-configuration-to-libraries
module.exports = {
  plugins: {
    tailwindcss: {
      config: join(__dirname, "tailwind.config.js"),
    },
    autoprefixer: {},
  },
};
```

이제 UI 라이브러리는 Tailwind CSS 설정이 준비되었습니다. 이제 라이브러리에 UI 컴포넌트를 추가할 수 있습니다.

# Shadcn UI 설정

이 섹션은 UI 라이브러리에서 Shadcn UI 컴포넌트를 설정하는 방법에 대해 안내합니다. Shadcn UI는 아름답게 디자인된 컴포넌트를 제공하며, 이를 앱에 복사하여 필요에 맞게 사용자 정의할 수 있습니다.

<div class="content-ad"></div>

먼저 UI 라이브러리에 Shadcn UI 패키지를 설치해야 해요. UI 구성 요소에 @shadcn/ui 패키지를 사용할 거예요.

- Shadcn UI 패키지 설치하기: 다음 명령을 실행하여 UI 라이브러리에 @shadcn/ui 패키지를 설치하세요:

```js
pnpm dlx shadcn-ui@latest init
```

- 구성 선택: 다음 단계에서 Shadcn UI 패키지의 구성 옵션을 선택하세요.

<div class="content-ad"></div>

![image](/assets/img/2024-07-09-SharedUISetupForMicroFrontendApplicationModuleFederationwithReactwithNxWorkspace_2.png)

- 참고: components 및 utils 디렉토리는 packages/ui/src 디렉토리에 생성될 것입니다. packages/ui/src/\* 파일에서 utils 경로를 변경할 것입니다. Shadcn CLI가 생성한 패스는 packages/ui/src/lib/utils 이지만, ../../lib/utils로 변경해야 합니다.

기본 Tailwind CSS 구성을 덮어쓰려면 루트 디렉토리의 tailwind.config.js 파일을 업데이트하세요. 모든 애플리케이션 및 라이브러리가 이 구성을 사용할 것입니다.

```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  darkMode: ["class"],
  content: [
    "./apps/**/*.{js,ts,jsx,tsx}",
    "./packages/**/*.{js,ts,jsx,tsx}",
    "./{src,pages,components,app}/**/*.{ts,tsx,html}",
  ],
  prefix: "",
  theme: {
    container: {
      center: true,
      padding: "2rem",
      screens: {
        "2xl": "1400px",
      },
    },
    extend: {
      colors: {
        border: "hsl(var(--border))",
        input: "hsl(var(--input))",
        ring: "hsl(var(--ring))",
        background: "hsl(var(--background))",
        foreground: "hsl(var(--foreground))",
        primary: {
          DEFAULT: "hsl(var(--primary))",
          foreground: "hsl(var(--primary-foreground))",
        },
        secondary: {
          DEFAULT: "hsl(var(--secondary))",
          foreground: "hsl(var(--secondary-foreground))",
        },
        destructive: {
          DEFAULT: "hsl(var(--destructive))",
          foreground: "hsl(var(--destructive-foreground))",
        },
        muted: {
          DEFAULT: "hsl(var(--muted))",
          foreground: "hsl(var(--muted-foreground))",
        },
        accent: {
          DEFAULT: "hsl(var(--accent))",
          foreground: "hsl(var(--accent-foreground))",
        },
        popover: {
          DEFAULT: "hsl(var(--popover))",
          foreground: "hsl(var(--popover-foreground))",
        },
        card: {
          DEFAULT: "hsl(var(--card))",
          foreground: "hsl(var(--card-foreground))",
        },
      },
      borderRadius: {
        lg: "var(--radius)",
        md: "calc(var(--radius) - 2px)",
        sm: "calc(var(--radius) - 4px)",
      },
      animation: {
        wiggle: "wiggle 1s ease-in-out infinite",
        "accordion-down": "accordion-down 0.2s ease-out",
        "accordion-up": "accordion-up 0.2s ease-out",
      },
      keyframes: {
        wiggle: {
          "0%, 100%": { transform: "rotate(-3deg)" },
          "50%": { transform: "rotate(3deg)" },
        },
        "accordion-down": {
          from: { height: "0" },
          to: { height: "var(--radix-accordion-content-height)" },
        },
        "accordion-up": {
          from: { height: "var(--radix-accordion-content-height)" },
          to: { height: "0" },
        },
      },
    },
  },
  plugins: [require("@tailwindcss/forms"), require("tailwindcss-animate")],
};
```

<div class="content-ad"></div>

# 버튼 컴포넌트 추가하기

Shadcn UI를 프로젝트에 추가한 후, 프로젝트에서 버튼 컴포넌트를 사용할 수 있습니다.

```js
pnpm dlx shadcn-ui@latest add button
```

```js
import { Button } from '@mfe-tutorial/ui';

export function App() {
  return (
    // ...코드의 나머지 부분
    <Button>클릭하기</Button>
    <Button variant="destructive">클릭하기</Button>
    <Button variant="secondary">클릭하기</Button>
    // ...코드의 나머지 부분
  );
}
```

<div class="content-ad"></div>

# Shadcn UI Hover Card 추가

Shadcn UI를 프로젝트에 추가한 후에는 프로젝트에서 HoverCard 구성 요소를 사용할 수 있습니다.

```js
pnpm dlx shadcn-ui@latest add hover-card
```

```js
// apps/container/src/components/hover-card/index.tsx
import { Button, HoverCard, HoverCardContent, HoverCardTrigger } from "@mfe-tutorial/ui";
import { CalendarDays } from "lucide-react";

export function HoverCardDemo() {
  return (
    <HoverCard>
      <HoverCardTrigger asChild>
        <Button variant="link">@nextjs</Button>
      </HoverCardTrigger>
      <HoverCardContent className="w-80">
        <div className="flex justify-between space-x-4">
          <div className="space-y-1">
            <h4 className="text-sm font-semibold">@nextjs</h4>
            <p className="text-sm">React 프레임워크 - @vercel이 만들고 유지보수합니다.</p>
            <div className="flex items-center pt-2">
              <CalendarDays className="w-4 h-4 mr-2 opacity-70" />{" "}
              <span className="text-xs text-muted-foreground">2021년 12월 가입</span>
            </div>
          </div>
        </div>
      </HoverCardContent>
    </HoverCard>
  );
}
```

<div class="content-ad"></div>

# Shadcn UI Badge 추가하기

프로젝트에 Shadcn UI를 추가한 후에는 프로젝트에서 Badge 컴포넌트를 사용할 수 있습니다.

```js
pnpm dlx shadcn-ui@latest add badge
```

```js
// apps/container/src/components/social-links/index.tsx
import { Badge } from '@mfe-tutorial/ui';
import { BadgeAlert, BadgeCheck } from 'lucide-react';

export function App() {
  return (
    // ...코드의 나머지 부분
    <Badge className="gap-x-2" variant="secondary">
      <BadgeCheck />
      Primary Badge
    </Badge>
    <Badge className="gap-x-2" variant="default">
      <BadgeCheck />
      Shadcn Badge
    </Badge>
    <Badge className="gap-x-2" variant="destructive">
      <BadgeAlert />
      Destructive Badge
    </Badge>
    // ...코드의 나머지 부분
  );
}
```

<div class="content-ad"></div>

# 결론

이 튜토리얼에서는 Nx Workspace, React 및 Tailwind CSS를 사용하여 Micro Frontend 응용 프로그램용 공유 UI 라이브러리를 설정하는 방법을 배웠습니다. UI 구성 요소에 Shadcn UI를 사용하고 Button, HoverCard 및 Badge 구성 요소를 UI 라이브러리에 추가했습니다. 또한이러한 구성 요소를 응용 프로그램에서 사용하는 방법에 대해 배웠습니다.

이 튜토리얼에서 설명 된 단계를 따라가면 이제 공유 UI 라이브러리를 사용하여 Micro Frontend 응용 프로그램을 만들 수있는 튼튼한 기초가 마련됩니다. 이 접근 방식은 다중 응용 프로그램 간의 더 나은 코드 재사용성, 유지 보수성 및 일관성을 제공합니다.

Shadcn UI 문서에서 사용 가능한 구성 요소 및 사용자 정의 옵션에 대한 자세한 내용을 탐색하십시오. 즐거운 코딩 되세요!

<div class="content-ad"></div>

# 쉬운 영어로 🚀

In Plain English 커뮤니티의 일원이 되어 주셔서 감사합니다! 이전에 떠나시기 전에:

- 작가에게 박수를 보내고 팔로우해주세요 👏️️
- 팔로우하기: X | LinkedIn | YouTube | Discord | 뉴스레터
- 다른 플랫폼 방문하기: Stackademic | CoFeed | Venture | Cubed
- PlainEnglish.io에서 더 많은 콘텐츠 확인하기
