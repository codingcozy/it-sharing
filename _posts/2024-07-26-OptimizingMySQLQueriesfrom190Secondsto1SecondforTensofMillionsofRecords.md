---
title: "수천만 레코드의 MySQL 쿼리 시간을 190초에서 1초로 단축시킨 방법"
description: ""
coverImage: "/assets/img/2024-07-26-OptimizingMySQLQueriesfrom190Secondsto1SecondforTensofMillionsofRecords_0.png"
date: 2024-07-26 12:03
ogImage: 
  url: /assets/img/2024-07-26-OptimizingMySQLQueriesfrom190Secondsto1SecondforTensofMillionsofRecords_0.png
tag: Tech
originalTitle: "Optimizing MySQL Queries from 190 Seconds to 1 Second for Tens of Millions of Records"
link: "https://medium.com/stackademic/optimizing-mysql-queries-from-190-seconds-to-1-second-for-tens-of-millions-of-records-c9d61b7e75b9"
isUpdated: true
updatedAt: 1723813341780
---



<img src="/assets/img/2024-07-26-OptimizingMySQLQueriesfrom190Secondsto1SecondforTensofMillionsofRecords_0.png" />

우선, 수천만 개의 데이터를 처리하는 것은 일반적으로 MySQL의 최적 사용 사례가 아님을 명심해야 합니다.

수십만 개의 레코드를 처리하는 MySQL의 최적화 전략에는 다음이 있습니다:

- Sharding (테이블과 데이터베이스 분할)
- 요약 테이블 생성
- 여러 서브쿼리 사용을 위한 쿼리 수정

<div class="content-ad"></div>

이 토론에서는 수백만 개의 레코드를 포함하는 단일 MySQL 테이블이 있는 시나리오를 고려하고 있습니다.

테이블 설계가 부족하고 비즈니스 규칙 상 SQL 쿼리를 여러 하위 쿼리로 분할할 수 없는 경우를 가정합니다.

이러한 상황에서 개발자들은 SQL을 최적화하여 쿼리 목표를 달성할 수 있습니다.

단일 MySQL 테이블에 수백만 개의 레코드가 포함되어 있는 경우 특별한 고려 사항이 발생합니다.

<div class="content-ad"></div>

본 토론은 주로 극단적인 경우에 대한 SQL 최적화 전략에 집중하고 있어요.