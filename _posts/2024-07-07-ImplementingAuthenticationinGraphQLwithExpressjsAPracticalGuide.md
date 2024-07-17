---
title: "GraphQL과 Expressjs를 사용해 인증 구현하기 실전 가이드"
description: ""
coverImage: "/assets/img/2024-07-07-ImplementingAuthenticationinGraphQLwithExpressjsAPracticalGuide_0.png"
date: 2024-07-07 20:56
ogImage:
  url: /assets/img/2024-07-07-ImplementingAuthenticationinGraphQLwithExpressjsAPracticalGuide_0.png
tag: Tech
originalTitle: "Implementing Authentication in GraphQL with Express.js: A Practical Guide"
link: "https://medium.com/@tomas-svojanovsky/implementing-authentication-in-graphql-with-express-js-a-practical-guide-648171e0054e"
---

<img src="/assets/img/2024-07-07-ImplementingAuthenticationinGraphQLwithExpressjsAPracticalGuide_0.png" />

이 기사에서 스켈레톤을 사용할 것입니다: Express.js와 Apollo Server 연결: 단계별 가이드

## 패키지 설치

로그인 및 로그아웃은 Express를 통해 처리하고, 데이터는 GraphQL을 통해 제공될 것입니다.

<div class="content-ad"></div>

```js
npm i @apollo/server jsonwebtoken graphql express-jwt
npm i express cors bcrypt better-sqlite3 @paralleldrive/cuid2
npm install knex -g
```

## Init knex

```js
knex init
```

knexfile.js에 기본 옵션을 아래 구성으로 대체하여 초기화합니다. 여기서는 better-sqlite3 패키지를 사용할 것입니다. 그리고 데이터베이스의 위치와 마이그레이션 파일의 위치를 정의할 것입니다.

<div class="content-ad"></div>

```json
export default {
  development: {
    client: "better-sqlite3",
    connection: {
      filename: "./data/db.sqlite3"
    },
    useNullAsDefault: true,  // SQLite requires this setting for default values
    migrations: {
      directory: "./migrations",
    },
  },
};
```

## 마이그레이션

테이블 구조를 정의할 것입니다. 특히 사용자(users) 및 할일(todos) 테이블에 대해 정의할 것입니다. 마이그레이션 폴더를 만들고 다음 명령어를 실행하세요.

```bash
knex migrate:make create_tables
```
