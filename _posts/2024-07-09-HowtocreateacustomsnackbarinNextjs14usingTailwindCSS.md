---
title: "Tailwind CSS를 사용하여 Nextjs 14에서 커스텀 스낵바 만드는 방법"
description: ""
coverImage: "/it-shar.ing/assets/no-image.jpg"
date: 2024-07-09 08:31
ogImage:
  url: /it-shar.ing/assets/no-image.jpg
tag: Tech
originalTitle: "How to create a custom snackbar in Next.js 14 using Tailwind CSS"
link: "https://medium.com/@amngoyal/how-to-create-a-custom-snackbar-in-next-js-14-using-tailwind-css-69b17322ad49"
---

스낵바는 현대 웹 애플리케이션의 중요한 부분입니다. 사용자에게 빠르고 한눈에 보이는 피드백을 제공하여 종종 작업의 성공 또는 실패를 알립니다.

그러나 React나 Next.js에서 스낵바를 만들기 위한 라이브러리를 찾을 때 성능 문제를 유발할 수 있는 무겁고 오래된 React 버전으로 작성된 구식 라이브러리 또는 구식 라이브러리를 종종 마주하게 됩니다.

이 블로그 포스트에서는 Next.js 14, TypeScript 및 Tailwind CSS를 사용하여 사용자 정의 가능하고 가벼운 스낵바 컴포넌트를 만드는 방법을 안내합니다.

# 요구 사항

<div class="content-ad"></div>

- Node.js
- Next.js 14
- TypeScript
- Tailwind CSS

# Next.js 14과 TypeScript, Tailwind CSS 설정하기

우선, TypeScript로 새 Next.js 프로젝트를 설정해보겠습니다. 아직 진행하지 않으셨다면, 다음 명령어를 사용하여 새 Next.js 프로젝트를 만들 수 있습니다:

```js
npx create-next-app 프로젝트명 --typescript
cd 프로젝트명
```

<div class="content-ad"></div>

필요한 종속성을 설치해주세요:

```js
npm install classnames react-icons
```

classnames는 조건부 클래스 관리에 사용됩니다. 원하시면 삼항 연산자를 사용하여 조건부 클래스를 추가할 수도 있습니다. react-icons는 스낵바 안에 닫기 아이콘을 포함하는 데 사용됩니다.

이제 Tailwind CSS를 구성해봅시다. Next.js용 공식 Tailwind CSS 설치 가이드에 따라 진행해주세요.

<div class="content-ad"></div>

# Snackbar 컴포넌트를 만들기

우리는 먼저 Snackbar 컴포넌트와 해당 컨텍스트를 생성할 것입니다. 이 컴포넌트는 사용자에게 메시지를 표시하는 역할을 맡을 것입니다.

## 단계 1: 컨텍스트 생성하기:

context 디렉토리에 SnackbarContext.tsx라는 새 파일을 생성해주세요.

<div class="content-ad"></div>

```js
// src/context/SnackbarContext.tsx

import { createContext, ReactNode } from "react";

// SnackbarContext 및 SnackbarProvider props에 대한 타입 정의
interface SnackbarContextType {
  (message: string, variant?: "success" | "error" | "warning" | "info"): void;
}

interface SnackbarProviderProps {
  children: ReactNode;
}

interface SnackbarState {
  show: boolean;
  message: string;
  variant: "success" | "error" | "warning" | "info";
}

const SnackbarContext = (createContext < SnackbarContextType) | (undefined > undefined);
```

여기서는 스낵바 컨텍스트 및 프로바이더 props에 대한 타입을 정의합니다. SnackbarContextType은 컨텍스트 함수의 형태를 정의하고, SnackbarProviderProps는 프로바이더의 props의 형태를 정의합니다. SnackbarState는 스낵바의 상태를 나타냅니다.

## 단계 2: SnackbarProvider 컴포넌트 생성하기:

SnackbarProvider 컴포넌트를 정의하여 해당 자식 컴포넌트에 스낵바 컨텍스트를 제공합니다.

<div class="content-ad"></div>

```js
export const SnackbarProvider: React.FC<SnackbarProviderProps> = ({ children }) => {
  const [snackbar, setSnackbar] =
    useState <
    SnackbarState >
    {
      show: false,
      message: "",
      variant: "success",
    };

  const handleSnackbarClose = () => {
    setSnackbar((prev) => ({ ...prev, show: false }));
  };

  const showSnackbar =
    useCallback <
    SnackbarContextType >
    ((message, variant = "success") => {
      setSnackbar({ show: true, message, variant });
      setTimeout(handleSnackbarClose, 5000);
    },
    []);

  return (
    <SnackbarContext.Provider value={showSnackbar}>
      {children}

      <div
        className={classNames(
          "transition-transform bottom-8 font-medium text-white left-8 fixed flex justify-between gap-2 items-center shadow-md min-h-[48px] max-w-[50vw] px-4 py-2 rounded-lg min-w-[300px] text-sm truncate whitespace-nowrap",
          {
            ["bg-successBg "]: snackbar?.variant === "success",
            ["bg-errorBg "]: snackbar?.variant === "error",
            ["bg-warningBg "]: snackbar?.variant === "warning",
            ["bg-infoBg "]: snackbar?.variant === "info",
            ["-translate-x-[200%]"]: !snackbar?.show,
            ["translate-x-0"]: snackbar?.show,
          }
        )}
      >
        {snackbar?.message}
        <div className="hover:bg-black/20 p-1 rounded-full cursor-pointer" onClick={handleSnackbarClose}>
          <IoMdClose size={20} />
        </div>
      </div>
    </SnackbarContext.Provider>
  );
};
```

이 컴포넌트에서는 useState를 사용하여 snackbar 상태를 관리합니다. showSnackbar 함수는 메시지와 변형을 가진 snackbar를 표시합니다. handleSnackbarClose 함수는 snackbar를 숨깁니다.

이제, 일정 시간이 지난 후 자동으로 snackbar를 닫기 위해 타임아웃을 추가할 것입니다. showSnackbar 함수 내에서 setTimeout을 사용하여 5초 후에 닫기 함수를 호출하도록 할 것입니다.

```js
const SNACKBAR_TIMER = 5000;

// 5초 뒤에 snackbar를 숨기기 위한 새로운 타이머 설정
setTimeout(() => {
  handleSnackbarClose();
}, SNACKBAR_TIMER);
```

<div class="content-ad"></div>

이제 Snackbar을 5초 후에 닫습니다. 그러나 5초 이내에 두 번째 Snackbar을 열 경우, 두 번째 Snackbar에 대한 타이머는 재설정되지 않습니다.

예를 들어, 성공 Snackbar을 열면 5초 후에 닫히도록 타이머가 설정됩니다. 그리고 3초 후에 오류 Snackbar을 발생시킨다면, Snackbar 내용과 종류는 변경되지만 남은 2초 후에 닫힙니다.

이 문제를 해결하기 위해 우리는 useRef를 사용하여 타이머를 저장하고 5초 타임아웃 이전에 다른 Snackbar이 열릴 경우 이를 지우게 합니다. 이렇게 하면 새로운 Snackbar이 트리거될 때마다 타이머가 재설정됩니다. 여기에 업데이트된 코드가 있습니다:

```js
  const timerRef = useRef<ReturnType<typeof setTimeout> | null>(null);

  const SNACKBAR_TIMER = 5000;

  const showSnackbar = useCallback<SnackbarContextType>(
    (message, variant = 'success') => {
      setSnackbar({ show: true, message, variant });

      // Clear the existing timer if it exists
      if (timerRef.current) {
        clearTimeout(timerRef.current);
      }

      // Set a new timer to hide the snackbar after 5 seconds
      timerRef.current = setTimeout(() => {
        handleSnackbarClose();
        timerRef.current = null;
      }, SNACKBAR_TIMER);
    },
    []
  );

  useEffect(() => {
    // Clean up the timer when the component unmounts
    return () => {
      if (timerRef.current) {
        clearTimeout(timerRef.current);
      }
    };
  }, []);
```

<div class="content-ad"></div>

## 단계 3: 스낵바 컨텍스트를 사용하는 훅 작성하기:

다른 컴포넌트에서 스낵바 컨텍스트를 사용하도록 훅을 정의하세요.

```js
export const useSnackbar = (): SnackbarContextType => {
  const context = useContext(SnackbarContext);
  if (!context) {
    throw new Error("useSnackbar must be used within a SnackbarProvider");
  }
  return context;
};
```

이 훅은 스낵바 프로바이더 내에서만 컨텍스트가 사용되도록 보장합니다.

<div class="content-ad"></div>

제안하신 마지막 SnackbarContext.tsx 파일은 여기 있습니다:

```js
// src/context/SnackbarContext.tsx

'use client';

import classNames from 'classnames';
import {
  ReactNode,
  createContext,
  useCallback,
  useContext,
  useEffect,
  useRef,
  useState,
} from 'react';
import { IoMdClose } from 'react-icons/io';

interface SnackbarContextType {
  (message: string, variant?: 'success' | 'error' | 'warning' | 'info'): void;
}

interface SnackbarProviderProps {
  children: ReactNode;
}

interface SnackbarState {
  show: boolean;
  message: string;
  variant: 'success' | 'error' | 'warning' | 'info';
}

const SnackbarContext = createContext<SnackbarContextType | undefined>(
  undefined
);

const SNACKBAR_TIMER = 5000;

export const SnackbarProvider: React.FC<SnackbarProviderProps> = ({
  children,
}) => {
  const [snackbar, setSnackbar] = useState<SnackbarState>({
    show: false,
    message: '',
    variant: 'success',
  });
  const timerRef = useRef<ReturnType<typeof setTimeout> | null>(null);

  const handleSnackbarClose = () => {
    setSnackbar((prev) => ({ ...prev, show: false }));
  };

  const showSnackbar = useCallback<SnackbarContextType>(
    (message, variant = 'success') => {
      setSnackbar({ show: true, message, variant });

      // Clear the existing timer if it exists
      if (timerRef.current) {
        clearTimeout(timerRef.current);
      }

      // Set a new timer to hide the snackbar after 5 seconds
      timerRef.current = setTimeout(() => {
        handleSnackbarClose();
        timerRef.current = null;
      }, SNACKBAR_TIMER);
    },
    []
  );

  useEffect(() => {
    // Clean up the timer when the component unmounts
    return () => {
      if (timerRef.current) {
        clearTimeout(timerRef.current);
      }
    };
  }, []);

  return (
    <SnackbarContext.Provider value={showSnackbar}>
      {children}

      <div
        className={classNames(
          'transition-transform bottom-8 font-medium text-white left-8 fixed flex justify-between gap-2 items-center shadow-md min-h-[48px] max-w-[50vw] px-4 py-2 rounded-lg min-w-[300px] text-sm truncate whitespace-nowrap',
          {
            ['bg-successBg ']: snackbar?.variant === 'success',
            ['bg-errorBg ']: snackbar?.variant === 'error',
            ['bg-warningBg ']: snackbar?.variant === 'warning',
            ['bg-infoBg ']: snackbar?.variant === 'info',
            ['-translate-x-[200%]']: !snackbar?.show,
            ['translate-x-0']: snackbar?.show,
          }
        )}
      >
        {snackbar?.message}
        <div
          className="hover:bg-black/20 p-1 rounded-full cursor-pointer"
          onClick={handleSnackbarClose}
        >
          <IoMdClose size={20} />
        </div>
      </div>
    </SnackbarContext.Provider>
  );
};

export const useSnackbar = (): SnackbarContextType => {
  const context = useContext(SnackbarContext);
  if (!context) {
    throw new Error('useSnackbar must be used within a SnackbarProvider');
  }
  return context;
};
```

# 애플리케이션에서 Snackbar Context 사용하기

이제 Snackbar 컨텍스트와 프로바이더가 준비되었으니, 애플리케이션에서 이를 사용할 수 있습니다. 애플리케이션의 레이아웃 컴포넌트를 다음과 같이 SnackbarProvider로 감싸보세요:

<div class="content-ad"></div>

```js
// app/layout.tsx
import "./globals.css";
import { SnackbarProvider } from "@/context/SnackbarContext";

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en">
      <body>
        <SnackbarProvider>{children}</SnackbarProvider>
      </body>
    </html>
  );
}
```

# Tailwind CSS 스타일 추가하기

tailwind.config.js 파일에서 이 라인을 content 배열에 추가하여 Snackbar 컴포넌트에 tailwind 클래스를 적용하세요.

```js
"./src/context/**/*.{js,ts,jsx,tsx,mdx}";
```

<div class="content-ad"></div>

스낵바에 사용자 정의 스타일을 추가하세요:

```js
// tailwind.config.js

import type { Config } from "tailwindcss";
const config: Config = {
  content: [
    "./src/pages/**/*.{js,ts,jsx,tsx,mdx}",
    "./src/components/**/*.{js,ts,jsx,tsx,mdx}",
    "./src/app/**/*.{js,ts,jsx,tsx,mdx}",
    "./src/context/**/*.{js,ts,jsx,tsx,mdx}",
  ],
  theme: {
    extend: {
      colors: {
        successBg: "#4caf50",
        errorBg: "#f44336",
        warningBg: "#ff9800",
        infoBg: "#2196f3",
      },
    },
  },
  plugins: [],
};
export default config;
```

# 스낵바 트리거

이제 어떤 컴포넌트에서든 useSnackbar 훅을 사용하여 스낵바를 트리거할 수 있습니다. 여기서는 이 데모 프로젝트의 코드를 예시로 사용하고 있습니다.:

<div class="content-ad"></div>

```js
// app/page.tsx

"use client";
import { useSnackbar } from "@/context/SnackbarContext";

export default function Home() {
  const showSnackbar = useSnackbar();

  const handleClick = (variant: "success" | "error" | "warning" | "info") => {
    showSnackbar(`This is a ${variant} message!`, variant);
  };

  return (
    <main className="p-10 min-h-screen text-center">
      <h1 className="mt-10 font-bold text-3xl xl:text-4xl">사용자 정의 스낵바 데모</h1>
      <div className="flex flex-col justify-center items-center gap-6 px-5 sm:px-24 py-24 font-medium">
        <button className="bg-green-500 p-4 rounded-xl text-white" onClick={() => handleClick("success")}>
          성공 알림 보기
        </button>
        <button className="bg-red-500 p-4 rounded-xl font-medium text-white" onClick={() => handleClick("error")}>
          에러 알림 보기
        </button>
        <button className="bg-yellow-500 p-4 rounded-xl font-medium text-white" onClick={() => handleClick("warning")}>
          경고 알림 보기
        </button>
        <button className="bg-blue-500 p-4 rounded-xl font-medium text-white" onClick={() => handleClick("info")}>
          정보 알림 보기
        </button>
      </div>
    </main>
  );
}
```

<img src="https://miro.medium.com/v2/resize:fit:1200/1*-s3wb_kkr2fyE-2GZ-hPEw.gif" />

# 결론

이 블로그 포스트에서 TypeScript와 Tailwind CSS를 사용하여 Next.js 14 애플리케이션의 사용자 정의 스낵바 컴포넌트를 만들었습니다. 이러한 단계를 따르면 사용자의 작업에 대한 피드백을 손쉽게 제공하여 애플리케이션의 전반적인 사용자 경험을 향상시킬 수 있습니다.

<div class="content-ad"></div>

프로덕션 배포는 여기서 확인할 수 있어요.

전체 소스 코드는 제 GitHub 저장소에서 확인할 수 있어요.

이 가이드는 여기까지에요. 다음에 또 봐요! 만약 이 가이드가 도움이 되었다면 좋아요를 남겨주세요 👍 즐거운 코딩 되세요!
