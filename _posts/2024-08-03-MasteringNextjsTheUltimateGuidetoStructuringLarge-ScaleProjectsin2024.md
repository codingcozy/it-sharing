---
title: "Nextjs 2024년 대형 프로젝트 구조화 하는 방법"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-03 20:42
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Mastering Nextjs The Ultimate Guide to Structuring Large-Scale Projects in 2024"
link: "https://dev.to/vyan/mastering-nextjs-the-ultimate-guide-to-structuring-large-scale-projects-in-2024-h4e"
isUpdated: true
updatedAt: 1723816521859
---



## 소개: Next.js 정복하기

안녕하세요, 코드 마스터들과 Next.js 애호가 여러분! 👋 컴포넌트, 훅, 그리고 설정 파일들이 무성한 정글을 헤치며 인디아나 존스 같은 기분이 드시나요? 걱정 마세요, 이 모험에서 혼자가 아닙니다. 저도 이전에 이런 곳을 가 보았어요. 대규모 Next.js 프로젝트의 깊은 정글을 가르고자 힘겹게 나무꾼했던 기억이 있거든요.

그런데 중요한 건, 올바른 지도와 도구가 있다면, 여러분의 Next.js 정글은 잘 정리되고 번영하는 생태계가 될 수 있어요. 이 포괄적인 가이드에서, 대규모 Next.js 프로젝트를 구조화하는 데 있어서 제가 쌓아 온 지혜를 공유할 거에요. 기존 앱을 확장하거나 새로운 거대한 프로젝트를 처음부터 시작하든, 이 안내서가 여러분의 믿음직한 나침반 역할을 할 거예요.

## Next.js 프로젝트 구조가 여러분을 무너뜨릴 수도 있고 아닐 수도 있는 이유

<div class="content-ad"></div>

세부 사항에 들어가기 전에, 프로젝트 구조에 시간을 투자하는 것이 좋은 코딩 신발 구매와 같다는 이유에 대해 이야기해보겠습니다. 이것은 여러분을 멀리 데려다주고 편안함을 유지합니다:

- 개발자의 정신건강: 좋은 구조는 구성 요소를 찾느라 "월도를 찾아라" 하는 시간을 줄이고 실제 코딩에 더 많은 시간을 할애할 수 있습니다.
- 팀의 조화: 팀이 프로젝트를 눈가리고 쉽게 이해할 수 있다면, 협업은 전쟁이 아닌 브리즈가 됩니다.
- 확장성: 잘 구조화된 프로젝트는 코드 몬스터가 되는 대신 행복한 식물처럼 유기적으로 성장합니다.
- 성능 향상: Next.js 최적화 기능은 프로젝트가 논리적으로 구성되어 있을 때 가장 잘 작동합니다.
- 유지 보수성: 미래의 여러분(또는 여러분 프로젝트를 상속받는 불쌍한 이)은 깨끗하고 직관적인 구조에 영원히 감사할 것입니다.

## 프레임에 담고 싶은 Next.js 프로젝트 구조

자, 준비는 됐나요? 🥁 여기 대규모 Next.js 개발 현장에서 싸워본 구조가 있습니다:

<div class="content-ad"></div>

```js
📁 my-awesome-nextjs-project
|
|_ 📁 app
|  |_ 📁 (auth)
|  |  |_ 📁 login
|  |  |  |_ 📄 page.tsx
|  |  |  |_ 📄 layout.tsx
|  |  |_ 📁 register
|  |     |_ 📄 page.tsx
|  |     |_ 📄 layout.tsx
|  |_ 📁 dashboard
|  |  |_ 📄 page.tsx
|  |  |_ 📄 layout.tsx
|  |_ 📁 api
|  |  |_ 📁 users
|  |  |  |_ 📄 route.ts
|  |  |_ 📁 posts
|  |     |_ 📄 route.ts
|  |_ 📄 layout.tsx
|  |_ 📄 page.tsx
|
|_ 📁 components
|  |_ 📁 ui
|  |  |_ 📄 Button.tsx
|  |  |_ 📄 Card.tsx
|  |  |_ 📄 Modal.tsx
|  |_ 📁 forms
|  |  |_ 📄 LoginForm.tsx
|  |  |_ 📄 RegisterForm.tsx
|  |_ 📁 layouts
|     |_ 📄 Header.tsx
|     |_ 📄 Footer.tsx
|     |_ 📄 Sidebar.tsx
|
|_ 📁 lib
|  |_ 📄 api.ts
|  |_ 📄 utils.ts
|  |_ 📄 constants.ts
|
|_ 📁 hooks
|  |_ 📄 useUser.ts
|  |_ 📄 useAuth.ts
|  |_ 📄 usePosts.ts
|
|_ 📁 types
|  |_ 📄 user.ts
|  |_ 📄 post.ts
|  |_ 📄 api.ts
|
|_ 📁 styles
|  |_ 📄 globals.css
|  |_ 📄 variables.css
|
|_ 📁 public
|  |_ 📁 images
|  |  |_ 📄 logo.svg
|  |  |_ 📄 hero-image.png
|  |_ 📁 fonts
|     |_ 📄 custom-font.woff2
|
|_ 📁 config
|  |_ 📄 seo.ts
|  |_ 📄 navigation.ts
|
|_ 📄 next.config.js
|_ 📄 package.json
|_ 📄 tsconfig.json
|_ 📄 .env.local
|_ 📄 .gitignore
```

이제 한 가지씩 살펴보면서 당신의 Next.js 작품에 왜 각 부분이 중요한지 알아봅시다.

## Next.js 앱의 핵심: app 디렉터리

app 디렉터리는 마법이 벌어지는 곳입니다. 이것이 Next.js 13+ 프로젝트의 핵심이자 새로운 App 라우터를 활용하는 곳입니다.

<div class="content-ad"></div>

```js
📁 app
|_ 📁 (auth)
|  |_ 📁 login
|  |_ 📁 register
|_ 📁 dashboard
|_ 📁 api
|_ 📄 layout.tsx
|_ 📄 page.tsx
```

### Route 그룹화 with (auth)

(auth) 폴더는 URL 구조에 영향을주지 않으면서 관련된 경로를 그룹화하는 똑똑한 방법입니다. 인증 관련 페이지를 구성하는 데 완벽합니다.

```js
// app/(auth)/login/page.tsx
export default function LoginPage() {
  return <h1>로그인 페이지에 오신 것을 환영합니다</h1>;
}
```

<div class="content-ad"></div>

### API 라우트: Disguise 된 당신의 백엔드

백엔드 로직을 api 디렉토리에 깔끔하게 유지하십시오. 각 파일이 API 라우트가 됩니다:

```js
// app/api/users/route.ts
import { NextResponse } from 'next/server';

export async function GET() {
  // 사용자 데이터 가져오기 로직
  return NextResponse.json({ users: ['Alice', 'Bob'] });
}
```

### 레이아웃 및 페이지: UI의 구성 요소들

<div class="content-ad"></div>

페이지들 간에 일관된 디자인을 제공하기 위해 layout.tsx를 사용하세요:

```js
// app/layout.tsx
export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  );
}
```

각 page.tsx 파일은 애플리케이션 내의 고유한 경로를 나타냅니다:

```js
// app/page.tsx
export default function HomePage() {
  return <h1>Welcome to our awesome Next.js app!</h1>;
}
```

<div class="content-ad"></div>

## 구성 요소: 당신의 Next.js LEGO 세트

구성 요소를 LEGO 블록으로 생각해보세요. 잘 정리되어 있으면 쉽게 찾을 수 있고 사용하기 즐겁습니다:

```js
📁 components
|_ 📁 ui
|_ 📁 forms
|_ 📁 layouts
```

### UI 구성 요소: 건물 블록들

<div class="content-ad"></div>

앱 전체에서 일관성을 유지하는 재사용 가능한 UI 요소를 만들어보세요:

```js
// components/ui/Button.tsx
export default function Button({ children, onClick }) {
  return (
    <button onClick={onClick} className="bg-blue-500 text-white py-2 px-4 rounded">
      {children}
    </button>
  );
}
```

### 폼 컴포넌트: 데이터 입력이 쉬워집니다

더 깔끔하고 유지보수가 쉬운 코드를 위해 폼 로직을 캡슐화하세요:

<div class="content-ad"></div>

```js
// components/forms/LoginForm.tsx
import { useState } from 'react';
import Button from '../ui/Button';

export default function LoginForm({ onSubmit }) {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');

  return (
    <form onSubmit={(e) => {
      e.preventDefault();
      onSubmit(email, password);
    }>
      <input type="email" value={email} onChange={(e) => setEmail(e.target.value)} />
      <input type="password" value={password} onChange={(e) => setPassword(e.target.value)} />
      <Button type="submit">Log In</Button>
    </form>
  );
}
```

### 레이아웃 구성 요소: UI의 프레임워크

재사용 가능한 레이아웃 구성 요소로 일관된 페이지 구조를 만들어보세요:

```js
// components/layouts/Header.tsx
import Link from 'next/link';

export default function Header() {
  return (
    <header>
      <nav>
        <Link href="/">홈</Link>
        <Link href="/dashboard">대시보드</Link>
        <Link href="/profile">프로필</Link>
      </nav>
    </header>
  );
}
```

<div class="content-ad"></div>

## 지원 캐스트: lib, hooks 및 types

이 디렉토리들은 당신의 프로젝트의 무명의 영웅들입니다:

### lib: 당신의 유틸리티 벨트

도움이 되는 함수와 상수를 저장하세요:

<div class="content-ad"></div>


// lib/utils.ts
export function formatDate(date: Date): string {
  return date.toLocaleDateString('en-US', { year: 'numeric', month: 'long', day: 'numeric' });
}

// lib/constants.ts
export const API_BASE_URL = process.env.NEXT_PUBLIC_API_URL || 'https://api.example.com';

### hooks: Custom React Superpowers

Create custom hooks to encapsulate complex logic:

// hooks/useUser.ts
import { useState, useEffect } from 'react';
import { fetchUser } from '../lib/api';

export function useUser(userId: string) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetchUser(userId).then(userData => {
      setUser(userData);
      setLoading(false);
    });
  }, [userId]);

  return { user, loading };
}


<div class="content-ad"></div>

### types: TypeScript의 베스트 프렌드

TypeScript 인터페이스와 유형을 정의하세요:

```js
// types/user.ts
export interface User {
  id: string;
  name: string;
  email: string;
  role: 'admin' | 'user';
}

// types/post.ts
export interface Post {
  id: string;
  title: string;
  content: string;
  authorId: string;
  createdAt: Date;
}
```

## 다음 Next.js 작품을 스타일링하기

<div class="content-ad"></div>

스타일을 깔끔하게 유지하세요. 스타일은 styles 디렉토리에 저장됩니다:

```js
/* styles/globals.css */
@tailwind base;
@tailwind components;
@tailwind utilities;

/* 여기에 사용자 정의 전역 스타일 추가 */
body {
  font-family: 'Arial', sans-serif;
}

/* styles/variables.css */
:root {
  --primary-color: #3490dc;
  --secondary-color: #ffed4a;
  --text-color: #333333;
}
```

## 공용 에셋: 당신의 앱의 얼굴

public 디렉토리는 정적 에셋의 홈입니다. 이미지를 최적화하고 사용자 지정 글꼴을 사용하여 앱을 빛나게 만드세요.

<div class="content-ad"></div>

```js
import Image from 'next/image';

export default function Logo() {
  return <Image src="/images/logo.svg" alt="Company Logo" width={200} height={50} />;
}
```

## 설정: 프로젝트의 중추

루트 디렉토리에 있는 이 중요한 파일들을 잊지마세요:

```js
// next.config.js
module.exports = {
  images: {
    domains: ['example.com'],
  },
  // 다른 Next.js 구성 옵션
};

// .env.local
DATABASE_URL=postgresql://username:password@localhost:5432/mydb
NEXT_PUBLIC_API_URL=https://api.example.com
```

<div class="content-ad"></div>

## 대규모 Next.js 성공을 위한 전략 팁

- 앱 라우터 활용: 이 것은 그저 새로운 것뿐만이 아니라 성능과 중첩 레이아웃을 바꿀 수 있는 요소입니다.
- 코드 분할이 당신의 친구입니다: 앱의 반응 속도를 유지하기 위해 동적 가져오기를 활용하세요:

```js
   import dynamic from 'next/dynamic';

   const DynamicComponent = dynamic(() => import('../components/HeavyComponent'));
```

- 이미지 최적화하기: Next.js의 이미지 구성 요소는 당신의 이미지를 위한 개인 트레이너와 같습니다.

<div class="content-ad"></div>

```js
   import Image from 'next/image';

   export default function Hero() {
     return <Image src="/hero-image.png" alt="Hero" width={1200} height={600} priority />;
   }
```

- 서버 컴포넌트 대박: 클라이언트 측 JavaScript를 줄이기 위해 사용하세요:

```js
   // 이 컴포넌트는 Next.js 13+에서 기본적으로 서버에 렌더링됩니다
   export default async function UserProfile({ userId }) {
     const user = await fetchUser(userId);
     return <div>Welcome, {user.name}!</div>;
   }
```

- 우승을 향한 API 루트: 서버 측 로직을 안전하고 분리하세요:

<div class="content-ad"></div>

```js
   // pages/api/posts.ts
   import type { NextApiRequest, NextApiResponse } from 'next';

   export default async function handler(req: NextApiRequest, res: NextApiResponse) {
     if (req.method === 'GET') {
       const posts = await fetchPosts();
       res.status(200).json(posts);
     } else {
       res.status(405).end(); // Method Not Allowed
     }
   }
```

## 마무리: 조직화되고 확장 가능한 Next.js 프로젝트

여기에 있습니다! 이 구조는 대규모 Next.js 프로젝트를 원활하게 만들어줄 것입니다. 기억하세요, 이것은 일괄적인 해결책은 아닙니다. 프로젝트의 고유한 요구 사항에 맞게 자유롭게 조정해보세요.

이 구조를 따르면 무엇이 어디에 위치하는지 고민하는 시간을 덜 들이고, 멋진 기능을 더 많이 구축하는 데 더 많은 시간을 할애할 수 있습니다. 코드가 더 깔끔해지고, 팀원들이 더 행복해지며, 프로젝트가 원활하게 확장될 것입니다.

<div class="content-ad"></div>

그럼 무엇을 기다리고 있나요? 다음 프로젝트에서 이 구조에 한 번 도전해보세요. 미래의 당신 (그리고 팀원들)이 그것에 대해 엄지 손가락을 치켜들 것입니다!

코딩 즐기세요, 그리고 여러분의 Next.js 프로젝트가 항상 체계적이고 버그가 없기를 바랍니다! 🚀

성공적인 대규모 Next.js 프로젝트의 핵심은 초기 설정뿐만 아니라 프로젝트가 성장함에 따라 구조를 어떻게 유지하고 발전시키는지에 있습니다. 유연하게 대응하고 계속 배우며 필요할 때 리팩토링을 두렵지 않게 하세요. 여러분은 할 수 있어요!