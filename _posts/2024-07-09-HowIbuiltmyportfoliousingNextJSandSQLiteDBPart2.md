---
title: "Part 2 NextJS와 SQLite DB로 포트폴리오 사이트 만들기 버튼 텍스트 적용 방법"
description: ""
coverImage: "/assets/img/2024-07-09-HowIbuiltmyportfoliousingNextJSandSQLiteDBPart2_0.png"
date: 2024-07-09 17:14
ogImage:
  url: /assets/img/2024-07-09-HowIbuiltmyportfoliousingNextJSandSQLiteDBPart2_0.png
tag: Tech
originalTitle: "How I built my portfolio using Next.JS and SQLite DB — [Part 2]"
link: "https://medium.com/@krimsonhart/how-i-built-my-portfolio-using-next-js-and-sqlite-db-part-2-37595ca4dc40"
---

# API 라우트 구축 및 Next.JS에서 SQLite 데이터베이스 사용하기

안녕하세요! 개발자 여러분, 다시 만나서 반가워요! Next.JS와 SQLite DB를 사용하여 포트폴리오를 구축하는 Part 2에 오신 것을 환영합니다! 이번 파트에서는 포트폴리오 앱의 API 구조와 SQLite 데이터베이스에 대해 알아보겠습니다.

다음 내용을 자세히 살펴볼 예정입니다 —

- API 폴더 구조를 생성하는 첫 단계 및 API용 Next.JS AppRouter의 작동 방식.
- 포트폴리오 프로젝트용 SQLite DB 인스턴스를 생성하는 방법.
- 이 DB 인스턴스를 사용하여 테이블을 생성, 조회, 수정, 삭제하는 방법.

<div class="content-ad"></div>

지금까지 우리는 매우 기본적인 Next.JS 앱 설치를 진행했습니다. 이 앱은 기본 Next.JS 스타터 페이지와 대시보드 인증을 위한 관리자/로그인 페이지를 제공합니다. 이미 확인하지 않은 경우, 이 링크를 확인해보시길 권합니다 -

제 포트폴리오에 대한 제 생각 과정 중 하나는 프로젝트 내에서 변경 사항을 만들고, 기사를 쓰거나 정적 텍스트를 업데이트할 때마다 매번 코드베이스 수정에 의존하고 싶지 않다는 것이었습니다. 그것이 Next.JS 경로를 선택한 몇 가지 이유 중 하나이기도 했습니다 - 대시보드를 통해 데이터를 관리하기 위한 빠른 API 설정을 가질 수 있도록 하기 위해서였죠. 이에 대해 SQLite와 Next.JS의 통합을 살펴보고 포트폴리오를 위한 간단한 테이블 세트를 만들어보겠습니다.

먼저, Next.JS 14에서 우리는 PageRouter 대신 AppRouter를 선택했습니다. 따라서 이제 AppRouter가 작동하려면 앱 디렉토리를 준수해야 합니다. 그래서 아래 이미지와 같이 앱 폴더 내에 api 폴더를 생성하는 것부터 시작하겠습니다.

<div class="content-ad"></div>

우리의 API 관련 코드(백엔드 코드)는 이 폴더 안에 위치하게 될 거에요. API용 AppRouter는 우리의 클라이언트 코드와 거의 같은 방식으로 작동합니다 — 다른 점은 page.tsx 대신에 인덱스 파일용 route.ts 파일을 사용한다는 점이에요.

# 1. 요구 사항 설치

먼저 Next.JS 패키지에서 SQLite 데이터베이스에 필요한 패키지를 설치해봅시다. SQLite와 sqlite3 두 가지 패키지를 설치해야 해요. 이를 설치하려면 프로젝트 루트 디렉토리에서 다음 명령어를 실행하세요 —

```js
yarn add sqlite sqlite3
```

<div class="content-ad"></div>

# 2. 데이터베이스에 연결하기

의존성이 설치되었으므로, 먼저 필요할 때 데이터베이스 인스턴스를 만들고 액세스할 수 있게 해줄 데이터베이스 파일을 생성해 보겠습니다. 아래 코드를 사용하여 api 디렉토리에 database.ts라는 파일을 만드세요.

```js
// /src/app/api/database.ts

import path from "path";
import sqlite3 from "sqlite3";

const dbPath = path.join(process.cwd(), "profile.db");
export const db = new sqlite3.Database(dbPath, sqlite3.OPEN_READWRITE | sqlite3.OPEN_CREATE, (err) => {
  if (err) {
    console.error(err.message);
  }
  console.log("프로필 데이터베이스에 연결되었습니다.");
});
```

이곳에서 주목해야 할 것은 dbPath 변수입니다. 이 변수는 process.cwd()를 사용하고 있습니다. 이 메서드는 프로젝트의 루트 경로를 얻어주며 특별히 중요합니다. 왜냐하면 Next.JS 배포에서 보통 사용하는 \_\_dirname이 올바른 경로를 가져오지 못하는 경우가 있기 때문입니다.

<div class="content-ad"></div>

profile.db라는 이름의 데이터베이스를 만들 예정이지만, 귀하의 요구에 더 적합한 이름을 선택하셔도 괜찮아요. 이 profile.db는 위의 이미지에서 보는 것처럼 루트 디렉토리에 생성될 거예요.

### 3. 테이블 생성

우리는 Medium의 기사를 통해 이것을 배우고 있으므로(저도 포트폴리오에 쇄곽하고 싶어요), 우리의 첫 번째 테이블 articles를 생성해볼게요. 이 테이블은 아래의 정보를 저장하는 데 사용될 것입니다 —
id, name, description, imageUrl, articleUrl, slug.

아래 코드를 읽어보시고 export const db 함수 바로 다음에 코드를 복사하여 데이터베이스와 테이블을 첫 번째 API 실행 시 자동으로 작성해보세요 —

<div class="content-ad"></div>

```js
// /src/app/api/migrations.ts

import { db } from "./database";

export const migrate = () => {
  db.serialize(() => {
    db.run(
      `
      CREATE TABLE IF NOT EXISTS articles (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        name TEXT NOT NULL,
        description TEXT NOT NULL,
        imageUrl TEXT NOT NULL,
        articleUrl TEXT NOT NULL,
        slug TEXT UNIQUE NOT NULL
      );
    `,
      (err: Error) => {
        if (err) {
          console.error(err.message);
        }
        console.log("articles table created successfully.");
      }
    );
  });
};
```

이 코드 블록은 migrations.ts라는 별도의 파일에 있습니다. 이것을 매번 실행하려고는 하지 않습니다. 이를 실행하는 세 가지 방법이 있습니다.

- UI에서 작업을 트리거하는 버튼을 두어이 기능을 감싼 함수를 제공하고 migrate 핸들러를 할당할 수 있습니다. — /src/app/api/migrate/route.ts 파일과 GET 핸들러 아래에서 이를 호출하여 마이그레이션을 실행하려면 GET 요청을 수행하여 마이그레이션을 실행할 수 있습니다.
- 데이터베이스.ts 파일에서 수동으로 migrate()를 호출할 수도 있습니다. 각 db 인스턴스에서이 마이그레이션이 실행됩니다.
- typescript 파일을 실행할 수 있는 ts-node라는 패키지를 설치하고 마이그레이션을 실행하는 "migrate": "ts-node src/app/api/migrations.ts"와 같은 package.json 정의를 사용할 수 있습니다. 이를 실행하려면 yarn migrate를 실행합니다.

# 4. API 호출하기

<div class="content-ad"></div>

이제 데이터베이스와 테이블이 생성되었습니다. 저는 하나의 테이블과 라우트만 가지고 있지 않을 것이라고 확신합니다. 결과적으로 데이터베이스에 대한 C.R.U.D 작업을 여러 번 작성해야 할 것입니다. SQLite의 경우 비동기적이기 때문에 대부분의 경우 실제로 데이터베이스 트랜잭션이 닫히기 전에 API에서 전달된 정보가 잘못될 수 있다는 약간의 우려가 있습니다. 이를 해결하기 위해 실제로 트랜잭션을 수동으로 Promise로 감싸야 합니다. 이것은 반복적으로 작성을 피하고자 하는 보일러플레이트입니다. 따라서 다음과 같이 API 트랜잭션 함수를 만들어 내보내 봅시다. (이 게시물에서는 GET 및 POST를 작성하는 데 초점을 맞출 것이지만 PATCH 및 DELETE에 대해서도 작성할 수 있습니다)

```js
// /src/app/api/database.ts

export const apiGet = async (query: string) => {
  return await new Promise((resolve, reject) => {
    db.all(query, (err: Error, row) => {
      if (err) {
        console.log(err);
        return reject(err);
      }
      return resolve(row);
    });
  });
};

export const apiPost = async (query: string, values: string[]) => {
  return await new Promise((resolve, reject) => {
    db.run(query, values, function (err) {
      if (err) {
        console.log(err);
        reject(err);
      }
      resolve(null);
    });
  });
};
```

우리의 데이터베이스 연결 및 공통 코드 설정이 완료되었으니, 이제 atricles 테이블에 대한 첫 번째 GET 및 POST API를 작성해 보겠습니다.

Next.JS 문서의 RouteHandlers 아래를 살펴보면 index 파일을 route.ts로 지정해야한다는 것을 알 수 있습니다. 따라서 articles에 대해서는 /api/articles 아래에 route.ts 파일을 생성해야합니다. 또한, Next.JS는 이제 실행할 의도된 메서드에 대한 핸들러를 직접적으로 명명하는 접근 방식을 따릅니다. 예를 들어, 라우트에서 GET 작업을 수행하려면 다음과 같이 핸들러를 사용해야합니다.

<div class="content-ad"></div>

```js
// /src/app/api/articles/route.ts

export async function GET(req: Request, res: Response) {}
```

먼저 POST API를 살펴볼까요? 데이터를 가져오기 전에 기사를 생성해야 하니까요 😅. 아래는 기사를 POST하고 profile.db에 저장하는 코드입니다.

```js
// /src/app/api/articles/route.ts

import { apiPost } from "../database";

export async function POST(req: Request, res: Response) {
  const body = await req.json();
  const { name, description, imageUrl, articleUrl, slug } = body;

  const query = `
    INSERT INTO articles(name, description, imageUrl, articleUrl, slug)
    VALUES(?, ?, ?, ?, ?)
  `;
  const values = [name, description, imageUrl, articleUrl, slug];

  let status, respBody;
  await apiPost(query, values)
    .then(() => {
      status = 200;
      respBody = { message: "Successfully created article" };
    })
    .catch((err) => {
      status = 400;
      respBody = err;
    });
  return Response.json(respBody, {
    status,
  });
}
```

이 코드의 몇 가지 주의해야 할 점이 있습니다 —

<div class="content-ad"></div>

이제 http://localhost:3000/api/articles로 postman POST 요청을 보내보세요. 필요한 body 객체와 함께 보내면 데이터베이스에 항목이 생성된 것을 확인할 수 있어요!

이제 JSON 형식으로 저장된 항목을 출력할 수 있는 GetAll 메소드를 구현해보겠습니다 —

```js
// /src/app/api/articles/route.ts

import { apiGet } from "../database";

export async function GET(req: Request, res: Response) {
 const query = `
    SELECT * from articles
  `;

 let status, body;
 try {
  await apiGet(query)
   .then((res) => {
    status = 200;
    body = res;
   })
   .catch((err: Error) => {
    status = 400;
    body = { error: err };
   });
  return Response.json(body, {
   status,
  });
 } catch (error: any) {
  console.error(error.message);
  return Response.json(
   { error: error },
   {
    status: 400,
   }
  );
 }
}
```

이 구현의 대부분은 POST와 동일합니다. 여기서 주의해야 할 부분이 있다면

<div class="content-ad"></div>

이제 브라우저에서 http://localhost:3000/api/articles 링크로 접속하면 앞서 POST 호출로 작성한 데이터베이스의 모든 기사 항목이 표시됩니다.

이로써 당신의 Next.JS 앱에서 SQLite DB 생성, GET 및 POST API 구현에 대한 매우 기본적인 내용을 마치게 되었습니다. PATCH 및 DELETE는 위 구현과 큰 차이가 없으므로 해당 부분은 이 글에서 건너뛰도록 하겠습니다.

지금부터 여러분의 마음껏 문서를 참고하고 수정하여 놀다가 Part 3 기사에서 다음 단계인 기본 인증 시스템으로 넘어가기 전까지 구현을 직접 해보시길 바랍니다 😉

그때까지 개발자 여러분! 코드와 함께 하길 기원합니다 ✋ 잘 가고 건배하세요!!

<div class="content-ad"></div>

다음 파트 —
