---
title: "GraphQL을 REST API로 사용하면 안 되는 이유"
description: ""
coverImage: "/assets/img/2024-07-09-DontUseGraphQLasaRESTAPI_0.png"
date: 2024-07-09 08:29
ogImage:
  url: /assets/img/2024-07-09-DontUseGraphQLasaRESTAPI_0.png
tag: Tech
originalTitle: "Don’t Use GraphQL as a REST API"
link: "https://medium.com/@tomas-svojanovsky/dont-use-graphql-as-a-rest-api-264c17adc811"
---

<img src="/assets/img/2024-07-09-DontUseGraphQLasaRESTAPI_0.png" />

암호 예제를 사용하여 변이 및 쿼리를 어떻게 사용할 수 있는지 확인해 보겠습니다. REST API로서 GraphQL을 사용하는 것이 이상적이지 않다는 것을 쉽게 알 수 있습니다.

## 패키지 설치

```js
npm i @apollo/server @paralleldrive/cuid2 cors better-sqlite3 express
npm i graphql knex
```

<div class="content-ad"></div>

## knex 초기화

```js
knex init
```

knexfile.js에서 기본 옵션을 아래 구성으로 대체하고, better-sqlite3 패키지를 사용할 것입니다. 그리고 데이터베이스가 위치한 곳과 마이그레이션 파일이 위치한 곳을 정의할 것입니다.

```js
export default {
  development: {
    client: "better-sqlite3",
    connection: {
      filename: "./data/db.sqlite3",
    },
    useNullAsDefault: true, // SQLite는 기본값을 설정하기 위해 이 설정이 필요합니다
    migrations: {
      directory: "./migrations",
    },
  },
};
```

<div class="content-ad"></div>

## 마이그레이션

테이블의 구조를 정의할 것이며, 특히 사용자(users)와 할일(todos) 테이블에 대해 정의할 것입니다. 마이그레이션 폴더를 생성하고 다음 명령을 실행하세요.

```js
knex migrate:make create_tables
```

마이그레이션 폴더에 생성된 파일의 내용을 해당 테이블의 정의로 대체하세요. Knex는 새로운 테이블을 만들기 위한 up 함수와 동작을 되돌리기 위한 down 함수를 가져야 합니다.
