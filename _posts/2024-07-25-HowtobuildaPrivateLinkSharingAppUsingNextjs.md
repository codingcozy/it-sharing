---
title: "Nextjs로 비공개 링크 공유 앱 만드는 방법(코드 있음)"
description: ""
coverImage: "/assets/img/2024-07-25-HowtobuildaPrivateLinkSharingAppUsingNextjs_0.png"
date: 2024-07-25 11:59
ogImage: 
  url: /assets/img/2024-07-25-HowtobuildaPrivateLinkSharingAppUsingNextjs_0.png
tag: Tech
originalTitle: "How to build a Private Link Sharing App Using Nextjs"
link: "https://dev.to/priyankarpal/how-to-build-a-private-link-sharing-app-using-nextjs-nap"
isUpdated: true
updatedAt: 1723813241966
---



## 소개

요즘 진행 중인 프로젝트 중 하나에서는 사용자 권한을 관리하여 개인 및 민감한 콘텐츠에 접근할 수 있도록 해야 했습니다. 그러나 이 솔루션을 구현하는 동안 많은 어려움을 겪었습니다.

문제를 연구한 후, 문제를 해결할 수 있는 해결책을 찾았습니다. 그때 이 글을 쓰기로 결심했습니다.

이 글에서는 Next.js와 Permit.io를 사용하여 개인 링크 공유 앱을 구축할 것입니다. 구현 과정을 통해 액세스 요청을 처리하는 방법, 승인을 관리하는 방법, 민감한 데이터를 보호하기 위한 권한을 강제하는 방법을 살펴볼 것입니다.

<div class="content-ad"></div>

## 필수 조건

이 애플리케이션을 빌드하기 위해 "Permit Share-If"을 사용하는 Next.js를 사용할 것입니다. 이는 허가 관리를 간단하게하는 라이브러리입니다. "Permit Share-If"은 사전 구축된 임베디드 UI 구성 요소들의 집합으로, 애플리케이션에서의 액세스 공유를 쉽게 만들어줍니다.

본 내용으로 넘어가기 전에, Next.js 구조의 기본을 알고 계신지 확인하세요. 원한다면 여기서 더 자세히 읽어보세요.

Next.js 애플리케이션에 Permit.io를 구현하기 위해 먼저 Permit.io를 설정해야 합니다. 시작해봅시다!

<div class="content-ad"></div>

## 전용 링크 공유 앱 만들기

### Next.js 프로젝트 설정

시작하려면, Next.js 프로젝트를 생성해보세요.

- 터미널을 열고 새 Next.js 프로젝트를 만들기 위한 명령어를 붙여넣어보세요.

<div class="content-ad"></div>

```js
npx create-next-app@latest .
```

- 이제 필요한 종속성을 설치하겠습니다.

```js
npm install @permitio/permit-js dotenv permitio
```

- 이제 ENV 및 JWT 비밀을 저장할 .env 파일을 생성하겠습니다.

<div class="content-ad"></div>

```js
// .env
ENV=<허가된 env 키>
JWT=<당신의 JWT 키>
```

이제 앱을 실행할 준비가 되었어요. 프로젝트를 성공적으로 빌드했어요.

### 액세스 요청 요소 추가

액세스 요청 요소를 만들려면 먼저 사용자 관리 요소를 만들어야 해요. 함께 만들어 볼까요?

<div class="content-ad"></div>

- 허가 대시보드의 요소 섹션으로 이동하세요.
- 사용자 관리 아래에 있는 "요소 생성"을 클릭하세요.

![이미지](/assets/img/2024-07-25-HowtobuildaPrivateLinkSharingAppUsingNextjs_0.png)

- 다음 단계를 따라 사용자 관리 요소를 설정하세요:
이름을 추가하세요. 예: "사용자 관리."
RBAC 역할을 선택하세요.
관리자 역할을 "숨겨진 역할" 섹션에서 "레벨 1 - 워크스페이스 소유자"로 이동하세요.
마지막으로 요소를 생성하세요.

사용자 관리 생성이 완료되었습니다. 이제 다음 단계로 넘어가봐요.

<div class="content-ad"></div>

- Elements 옵션을 클릭하세요.
- 액세스 요청 아래에 있는 "요소 생성"을 클릭하세요.

![이미지](/assets/img/2024-07-25-HowtobuildaPrivateLinkSharingAppUsingNextjs_1.png)

- 연결하려는 사용자 관리 요소를 선택하세요.
- 필요에 따라 요소를 사용자 정의하세요.

### 애플리케이션 내에 액세스 요청 요소를 구현하세요.

<div class="content-ad"></div>

- 저희 프로젝트를 위해 Linklist 컴포넌트를 생성해보세요.
Linklist 컴포넌트는 링크 목록을 표시하고 그 중에서 삭제할 수 있는 옵션을 제공합니다.
이 컴포넌트는 삭제 버튼이 있는 순서 없는 목록을 렌더링합니다.
- Linklist 컴포넌트는 링크 목록을 표시하고 그 중에서 삭제할 수 있는 옵션을 제공합니다.
- 이 컴포넌트는 삭제 버튼이 있는 순서 없는 목록을 렌더링합니다.

```js
// src/components/Linklist.jsx
const LinkList=({ links, deleteLink }) => {
    return (
        <ul>
            {links.map((link, index) => (
                <li key={index}>
                    <a href={link} target="_blank" rel="noopener noreferrer">{link}</a>
                    <button onClick={() => deleteLink(index)}>삭제</button>
                </li>
            ))}
        </ul>
    );
};

export default LinkList;
```

- 사용자가 링크를 추가할 수 있는 Linkform이라는 컴포넌트를 생성해보세요.

```js
// src/components/Linkform.jsx
import { useState } from 'react';

const LinkForm=({ addLink }) => {
    const [url, setUrl] = useState('');

    const handleSubmit = (e) => {
        e.preventDefault();
        if (url) {
            addLink(url);
            setUrl('');
        }
    };

    return (
        <form onSubmit={handleSubmit}>
            <input className='text-black'
                type="url"
                value={url}
                onChange={(e) => setUrl(e.target.value)}
                placeholder="URL을 입력해주세요"
                required
            />
            <button type="submit">링크 추가</button>
        </form>
    );
};

export default LinkForm;
```

<div class="content-ad"></div>

사용자가 '링크 추가' 버튼을 작동시키면 링크가 추가됩니다.

- 이제 사용자 관리를 위한 또 다른 컴포넌트인 UserManagement.jsx를 생성해야 합니다.

```js
import React from 'react'

function UserManagement() {
  return (
    <iframe
      title="퍼밋 엘리먼트 사용자 관리"
      src="https://embed.permit.io/user-management?envId=e0ffba3632274e8085d92c793d34f0c1&darkMode=false"
      width="100%"
      height="100%"
    />
  )
}

export default UserManagement;
```

- 이제 액세스 요청을 위한 또 다른 컴포넌트인 AccessRequest.jsx를 생성해야 합니다.

<div class="content-ad"></div>

```js
import React from 'react'

function AccessRequest() {
  return (
 <iframe
            title="Permit Element AR"
            src="https://embed.permit.io/ar?envId=e0ffba3632274e8085d92c793d34f0c1&darkMode=false&tenantKey=default"
            width="100%"
            height="100%"
            style={{ border: 'none' }}
        />
  )
}

export default AccessRequest;
```

그리고 요청 허용 시 Permit Share-If를 사용하는 대시보드용으로 다른 컴포넌트를 만들어보세요.

```js
// src/components/Dashboard.jsx

'use client';
import permit, { LoginMethod } from '@permitio/permit-js';
import 'dotenv/config';
import { useRouter } from 'next/navigation';
import { useEffect, useState } from 'react';

const Dashboard = () => {
    const router=useRouter();
    const [authenticated, setAuthenticated]=useState(false);

    useEffect(() => {
        const login=async () => {
            try {
                await permit.elements.login({
                    loginMethod: LoginMethod.frontendOnly,
                    userJwt: process.env.NEXT_PUBLIC_JWT,
                    tenant: 'default',
                    envId: process.env.NEXT_PUBLIC_ENV,
                });
                console.log('로그인 성공');
                setAuthenticated(true);
            } catch (err) {
                console.error('로그인 에러:', err);
            }
        };

        if (process.env.NEXT_PUBLIC_JWT&&process.env.NEXT_PUBLIC_ENV) {
            login();
        } else {
            console.error('JWT 토큰 또는 ENV 변수가 정의되지 않았습니다.');
        }
    }, []);

    useEffect(() => {
        if (authenticated) {
            router.push('/links');
        }
    }, [authenticated, router]);

    return (
     <AccessRequest />
     <UserManagement />

    );
};

export default Dashboard;
```

인증이 성공하면 사용자가 인증됐음을 나타내기 위해 상태를 업데이트합니다. 인증에 성공하면 성공 메시지를 기록하고, 실패하면 오류 메시지를 기록합니다.


<div class="content-ad"></div>

리다이렉션: 인증이 성공하면 컴포넌트가 사용자를 /links 페이지로 리다이렉트합니다.

그럼 이제 page.js 파일에서 AccessRequest 컴포넌트를 호출해야 합니다.

```js
import Dashboard from '../components/Dashboard';

export default function Home() {
  return (
    <div>
      <h1>개인 링크 공유 앱</h1>
      <Dashboard/>
    </div>
  );
}
```

마지막으로, 링크 페이지 경로를 구현하여 링크 페이지를 처리해야 합니다.

<div class="content-ad"></div>

```js
// src/app/links/page.jsx
'use client';
import LinkForm from '@/components/Linkform';
import LinkList from '@/components/Linklist';
import { useState } from 'react';

const Links=() => {
    const [links, setLinks] = useState([]);

    const addLink=(url) => {
        setLinks([...links, url]);
    };

    const deleteLink = (index) => {
        setLinks(links.filter((_, i) => i!==index));
    };

    return (
        <div className='p-5'>
            <h1>링크 관리</h1>
            <LinkForm addLink={addLink} />
            <LinkList links={links} deleteLink={deleteLink} />
        </div>
    );
};

export default Links;
```

그렇게 해서 우리의 애플리케이션이 완전히 완성되었습니다.

## 결론

이 가이드를 통해 Next.js 및 Permit.io를 사용하여 개인 링크 공유 앱을 만드는 방법을 이해했습니다. 액세스 요청 생성, 사용자 승인 및 개인 정보 보호를 위해 구현해야 하는 권한에 대해 살펴보았습니다.

<div class="content-ad"></div>

만약 Permit을 사용하여 더 많은 유사한 도구를 탐색하고 만들고 싶다면, 그들의 최신 Product Hunt 런칭 "Permit Share-If"를 확인해보세요. 더 많은 정보를 얻을 수 있을 거에요.

그리고 궁금한 점이 있으면 언제든지 댓글로 남겨주시면 됩니다.

코딩 즐거움을 느끼세요!