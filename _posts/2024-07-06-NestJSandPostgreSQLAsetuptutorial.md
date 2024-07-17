---
title: "NestJS와 PostgreSQL 설정 튜토리얼"
description: ""
coverImage: "/assets/img/2024-07-06-NestJSandPostgreSQLAsetuptutorial_0.png"
date: 2024-07-06 02:06
ogImage:
  url: /assets/img/2024-07-06-NestJSandPostgreSQLAsetuptutorial_0.png
tag: Tech
originalTitle: "NestJS and PostgreSQL: A set up tutorial"
link: "https://medium.com/@ihor.ivlievv/nestjs-and-postgresql-a-set-up-tutorial-2cd4505671a1"
---

## Neon 서버리스 PostgreSQL을 사용한 NestJS 구성: 클라우드 최적화 접근 방식

/assets/img/2024-07-06-NestJSandPostgreSQLAsetuptutorial_0.png

# 배울 내용:

이 강좌에서는 PostgreSQL 클라우드 데이터베이스를 NestJS에 연결하고 구성을 설정하며 TypeORM을 사용하여 데이터베이스에 테이블을 만드는 방법을 배울 것입니다. NestJS는 효율적이고 신뢰할 수 있으며 확장 가능한 서버 측 애플리케이션을 구축하기 위한 혁신적인 Node.js 프레임워크입니다. Neon은 클라우드용으로 제작된 Serverless Postgres입니다.

<div class="content-ad"></div>

# 준비 사항:

- JavaScript와 TypeScript에 대한 기본 지식
- 컴퓨터에 LTS 버전의 Node.js가 설치되어 있어야 합니다

# 시작해 봅시다:

## 단계 1: Neon에서 계정을 만들기

<div class="content-ad"></div>

회원 가입 페이지로 이동하여 새 계정을 생성해주세요.

## 단계 2: Neon 프로젝트 생성

새 계정을 만든 후에 프로젝트를 생성하라는 안내가 나타납니다. 필요에 따라 양식을 작성해야 합니다. "지역" 필드에서는 최상의 연결 속도를 얻기 위해 가장 가까운 지역을 선택하는 것이 좋습니다.

/assets/img/2024-07-06-NestJSandPostgreSQLAsetuptutorial_1.png

<div class="content-ad"></div>

## 단계 3: NestJS CLI 전역 설치

NestJS CLI를 전역으로 설치하려면 터미널에서 아래 명령어를 사용하십시오.

```js
npm i -g @nestjs/cli
```

## 단계 4: NestJs 앱 생성

<div class="content-ad"></div>

새로운 NestJS 프로젝트를 생성하려면 아래 명령을 사용하세요:

```js
nest new project-name
```

- "project-name"을 원하는 프로젝트 이름으로 바꿀 수 있습니다. 이 튜토리얼에서는 프로젝트 이름을 "grocery-store-server"로 할 것입니다.

## 단계 5: 의존성 추가 및 리소스 생성

<div class="content-ad"></div>

프로젝트를 생성한 후 해당 디렉토리로 이동하여 구성 설정, TypeORM, 그리고 Node.js용 PostgreSQL 클라이언트를 추가하기 위해 아래 명령어를 실행해주세요:

```js
npm i --save @nestjs/config @nestjs/typeorm typeorm pg
```

다음은 새로운 리소스를 생성하는 것이 필요합니다. 이를 위해 다음 명령어(CLI 명령어)를 사용할 것입니다:

```js
nest g res resource-name --no-spec
```

<div class="content-ad"></div>

- "resource-name"을 원하는 자원 이름으로 바꿀 수 있어요. 이 튜토리얼의 프로젝트 이름이 "grocery-store-server"이라면, 제 자원 이름은 "products"가 될 거에요.

## 단계 6: 프로젝트에 환경 변수 추가하기

환경 변수를 가져오려면 Neon의 계정으로 돌아가야 해요. 사이드바에서 "빠른 시작"을 선택해야 해요. 나타날 목록에서 "개발 브랜치 생성 및 연결"을 선택하고 필요한 구성을 선택해야 해요. 저희의 경우에는 Node.js에요.

/assets/img/2024-07-06-NestJSandPostgreSQLAsetuptutorial_2.png

<div class="content-ad"></div>

저희의 환경 변수를 복사하여 프로젝트 루트의 .env 파일에 붙여넣기 해주세요.

## 설정 7: 프로젝트 구성 설정

프로젝트에 데이터베이스에 대한 정보를 알리기 위해서는 구성을 조정해야 합니다. 이를 위해 데이터베이스 환경 변수를 해당 키에 대한 값으로 설정하겠습니다. 이를 통해 데이터베이스의 테이블, 레코드 관리 등이 가능해집니다.

다음 코드를 app.module.ts에 추가해주세요:

<div class="content-ad"></div>

```js
import { Module } from "@nestjs/common";
import { AppController } from "./app.controller";
import { AppService } from "./app.service";
import { ProductsModule } from "./products/products.module";
import { ConfigModule, ConfigService } from "@nestjs/config";
import { TypeOrmModule } from "@nestjs/typeorm";

@Module({
  imports: [
    ProductsModule,
    ConfigModule.forRoot({
      isGlobal: true,
    }),
    TypeOrmModule.forRootAsync({
      imports: [ConfigModule],
      useFactory: (configService: ConfigService) => ({
        type: "postgres",
        host: configService.get("PGHOST"),
        port: 5432,
        username: configService.get("PGUSER"),
        password: configService.get("PGPASSWORD"),
        database: configService.get("PGDATABASE"),
        autoLoadEntities: true,
        synchronize: true,
        extra: {
          ssl: true,
        },
      }),
      inject: [ConfigService],
    }),
  ],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}
```

## Step 8: Creating a table

데이터를 저장하려면 테이블이 필요합니다. src/products/entities/product.entity.ts 파일로 이동하여 다음 코드를 추가해주세요:

```js
import { Column, Entity, PrimaryGeneratedColumn } from "typeorm";

@Entity()
export class Product {
  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  name: string;

  @Column()
  price: string;
}
```

<div class="content-ad"></div>

제품 엔티티를 생성한 후에는 제품 모듈로 가져와야 합니다.

따라서 다음 코드를 src/products/product.module.ts에 추가해 봅시다:

```js
import { Module } from "@nestjs/common";
import { ProductsService } from "./products.service";
import { ProductsController } from "./products.controller";
import { TypeOrmModule } from "@nestjs/typeorm";
import { Product } from "./entities/product.entity";

@Module({
  imports: [TypeOrmModule.forFeature([Product])],
  controllers: [ProductsController],
  providers: [ProductsService],
})
export class ProductsModule {}
```

## 단계 9: 앱 실행

<div class="content-ad"></div>

우리 애플리케이션을 실행해 봅시다! 이를 위해 터미널에서 다음 명령어를 실행할 것입니다:

```js
npm run start:dev
```

Neon 계정으로 이동하여 사이드바에서 "테이블"을 선택하면 생성된 테이블을 볼 수 있어요.

/assets/img/2024-07-06-NestJSandPostgreSQLAsetuptutorial_3.png

<div class="content-ad"></div>

깃허브 레포 - grocery-store-server

## 즐거운 코딩!

만약 이 게시물이 도움이 되었다면 몇 개의 👏 👏 👏으로 감사를 표현해주세요!

최신 소식을 받아보세요! 더 많은 업데이트를 위해 내 LinkedIn을 구독해주세요.
