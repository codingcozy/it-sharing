---
title: "Prisma, Drizzle, TypeORM, Sequalize - 대규모 확장을 염두에 둔 선택 가이드"
description: ""
coverImage: "/assets/img/2024-07-09-PrismaDrizzleTypeORMorSequalizeWhenYourFocusIsScaleWhichOneToChoose_0.png"
date: 2024-07-09 16:48
ogImage:
  url: /assets/img/2024-07-09-PrismaDrizzleTypeORMorSequalizeWhenYourFocusIsScaleWhichOneToChoose_0.png
tag: Tech
originalTitle: "Prisma, Drizzle, TypeORM or Sequalize — When Your Focus Is Scale, Which One To Choose?"
link: "https://medium.com/@tamartwena/node-js-data-access-layer-tools-which-orm-to-choose-40473bc09ad0"
---

![이미지](/assets/img/2024-07-09-PrismaDrizzleTypeORMorSequalizeWhenYourFocusIsScaleWhichOneToChoose_0.png)

Node.js 생태계는 ORM 분야에서 흥미로운 도구 개발을 경험하고 있습니다. 몇 년 전에는 베테랑 ORM인 Sequalize와 TypeORM이 있었습니다. 작년 한 해 동안, Prisma와 Drizzle와 같은 2개의 라이브러리가 인기를 얻고 있습니다. 이 4개 라이브러리 각각은 서로 다른 도구 패밀리의 일부입니다. 그래서 이들은 무엇이며, 각각의 장점은 무엇일까요? 그리고 새 프로젝트를 시작한다면 어떤 것을 선택해야 할까요?

먼저, 각 ORM에 대해 그 특징에 대해 이야기해봅시다.

TypeORM과 Sequalize는 전통적인 ORM입니다. 전통적인 ORM은 관계형 데이터베이스와 작업하기 위한 객체 지향적 방법을 제공하며, 프로그래밍 언어에서 모델 클래스를 테이블에 매핑합니다. 모델을 만들고 DB 테이블에 매핑하며, 대부분의 DB 작업은 모델을 통해 수행됩니다. 코드에서 볼 수 있듯이 해당 각각의 ORM에 대해 다뤄보겠습니다:

<div class="content-ad"></div>

![Drizzle](/assets/img/2024-07-09-PrismaDrizzleTypeORMorSequalizeWhenYourFocusIsScaleWhichOneToChoose_1.png)

Drizzle는 더 가벼우며 실제로 "헤드리스 ORM"으로 정의됩니다. Drizzle에서는 DB 드라이버가 ORM 자체와 분리되어 있으며 여러 DB 드라이버 중에서 선택할 수 있습니다 :

![Prisma](/assets/img/2024-07-09-PrismaDrizzleTypeORMorSequalizeWhenYourFocusIsScaleWhichOneToChoose_2.png)

Prisma는 다른 접근 방식을 제공합니다. Prisma에서 코드를 작성할 때는 동일한 서비스 내에 정의된 모든 스키마를 관리하는 자동 생성된 클라이언트를 사용합니다.

<div class="content-ad"></div>

![image](/assets/img/2024-07-09-PrismaDrizzleTypeORMorSequalizeWhenYourFocusIsScaleWhichOneToChoose_3.png)

# 데이터 시나리오의 복잡성을 고려하여 ORM 선택하기

제 개인적인 경험을 통해 말씀드리면, 규모가 커질수록 데이터 시나리오는 더 복잡해집니다. 이 시점에서는 사용 중인 데이터베이스에서 더 많은 쿼리 최적화를 구현해야하며 쿼리에 더 많은 제어가 필요합니다. 그러므로 어떤 ORM을 선택할지 결정하기 위해 과거에 사용한 여러 쿼리 최적화를 살펴보았습니다. 그 목표는 다양한 ORM 도구로 이러한 최적화를 구현할 수 있는지 이해하는 것이었습니다.

# 기본 — select \*은 사용하지 말기

<div class="content-ad"></div>

select \*를 사용하지 않는 것이 프로덕션에서 권장되는 이유는 무엇인가요?

이유는 여러 가지 있지만, 여기서는 2가지 주요 이유를 언급해 드릴게요:

첫째, select _는 모든 열을 DB에서 가져오기 때문에 필요하지 않은 열까지 모두 가져오게 됩니다. 대형 테이블에 대형 필드가 추가되는 경우가 있었는데, 이는 모든 select _ 쿼리에서 느려지는 결과를 가져왔습니다. 이로 인해 필요하지 않던 데이터까지 DB에서 가져와 네트워크를 통해 어플리케이션 서버로 이동함으로써 해당 쿼리들이 더 많은 메모리와 리소스를 사용하게 되었습니다.

둘째는 인덱스 사용입니다. SELECT \*를 사용하면 쿼리 최적화와 인덱스 효율적인 사용이 방해될 수 있습니다. 쿼리에서 필요한 열을 명시함으로써 데이터베이스 최적화 프로그램은 인덱스를 더 효과적으로 활용하여 쿼리 실행 속도를 높일 수 있습니다.

<div class="content-ad"></div>

# Drizzle 내에서는 select \*를 생성하지 않는 어플리케이션 코드를 만들 수 있을까요?

Drizzle을 사용할 때, 명시적으로 쿼리에 포함시키고 싶은 필드를 지정하지 않더라도, Drizzle은 모델의 모든 필드를 나열하는 select 쿼리를 생성합니다. 또한, Drizzle은 쿼리를 위해 특정 필드를 선택할 수 있는 기능을 제공합니다. 아래에 Drizzle 코드 스니펫을 확인할 수 있습니다:

![Drizzle Code Snippet](/assets/img/2024-07-09-PrismaDrizzleTypeORMorSequalizeWhenYourFocusIsScaleWhichOneToChoose_4.png)

<div class="content-ad"></div>

프리즈마 —

프리즈마는 기본적으로 select \* 쿼리를 생성하지만 쿼리에서 특정 필드를 선택할 수 있는 기능을 제공합니다. 아래 코드에서 예제를 확인할 수 있어요:

![이미지](/assets/img/2024-07-09-PrismaDrizzleTypeORMorSequalizeWhenYourFocusIsScaleWhichOneToChoose_5.png)

시퀄라이즈와 TypeORM도 기본적으로 특정 필드를 선택할 수 있도록 해주지만, 필드를 선택하지 않는 경우에는 select \* 쿼리를 생성합니다.

<div class="content-ad"></div>

그래서 말씀드린대로, 모든 라이브러리는 특정 필드를 지정하여 선택 쿼리를 생성하는 것을 지원합니다.

![이미지](/assets/img/2024-07-09-PrismaDrizzleTypeORMorSequalizeWhenYourFocusIsScaleWhichOneToChoose_6.png)

# 고급 — 효율적인 페이지네이션을 위해 DB 커서 사용하기

페이지네이션을 구현하는 데는 2가지 방법이 있습니다.

<div class="content-ad"></div>

오프셋 기반 페이징 - 데이터베이스 쿼리를 사용하여 데이터 세트를 페이지별로 구분하기 위해 아이템을 건너뛰는 수(오프셋)와 각 페이지에서 검색할 아이템 수(제한)를 지정하는 기술입니다. 아래 예시를 확인할 수 있습니다.

![Offset Pagination](/assets/img/2024-07-09-PrismaDrizzleTypeORMorSequalizeWhenYourFocusIsScaleWhichOneToChoose_7.png)

커서 기반 페이징 - 데이터베이스 쿼리를 사용하여 대규모 데이터 세트에서 페이지를 검색하는 기술로, 마커 또는 커서를 사용하여 데이터 세트 내 현재 위치를 추적합니다. 오프셋 값 대신, 커서 기반 페이징은 다음 페이지의 결과를 가져오기 위해 커서나 키를 참조점으로 사용합니다. 커서는 일반적으로 데이터 세트 내 위치를 나타내는 고유 식별자이며, 색인화되고 정렬할 수 있어야 합니다.

![Cursor Pagination](/assets/img/2024-07-09-PrismaDrizzleTypeORMorSequalizeWhenYourFocusIsScaleWhichOneToChoose_8.png)

<div class="content-ad"></div>

커서 페이지네이션은 오프셋 기반 페이지네이션보다 효율적인 이유가 뭘까요?

커서는 고유하며 색인되고 정렬할 수 있습니다. 데이터베이스는 불필요한 데이터를 반복 순회하지 않고 직접 레코드로 이동합니다. 따라서 더 효율적입니다.

# Drizzle, Prisma, TypeORM 및 Sequelize에서 커서 기반 페이지네이션을 구현할 수 있을까요?

Prisma —

<div class="content-ad"></div>

프리즈마에서는 커서 기반 페이징을 구현할 수 있습니다. 아래 예시를 확인해보세요:

![이미지](/assets/img/2024-07-09-PrismaDrizzleTypeORMorSequalizeWhenYourFocusIsScaleWhichOneToChoose_9.png)

![이미지](/assets/img/2024-07-09-PrismaDrizzleTypeORMorSequalizeWhenYourFocusIsScaleWhichOneToChoose_10.png)

Drizzle -

<div class="content-ad"></div>

Drizzle에서도 아래 코드에서 볼 수 있는 것처럼 커서 기반 페이징을 구현할 수 있습니다:

![image](/assets/img/2024-07-09-PrismaDrizzleTypeORMorSequalizeWhenYourFocusIsScaleWhichOneToChoose_11.png)

그러나 TypeORM 및 Sequelize에서는 효율적인 방법으로 커서 페이징을 구현하는 방법을 찾지 못했습니다. TypeORM 및 Sequelize에서 커서 기반 페이징을 구현하는 여러 라이브러리가 있지만 다운로드 수가 많지 않습니다.

그래서 다양한 도구에서 커서 기반 페이징을 요약하면:

<div class="content-ad"></div>

![이미지](/assets/img/2024-07-09-PrismaDrizzleTypeORMorSequalizeWhenYourFocusIsScaleWhichOneToChoose_12.png)

# 고급 — 중첩 조회에서 여러 조인 유형 구현

중첩 엔티티를 다룰 때 하나 이상의 테이블에 저장된 엔티티와 일대일, 일대다, 다대일, 많대많의 관계를 갖는 경우가 있습니다. 사용자와 포스트의 일대다 엔티티 예시를 살펴봅시다 — DB 표현과 API 표현.

![이미지](/assets/img/2024-07-09-PrismaDrizzleTypeORMorSequalizeWhenYourFocusIsScaleWhichOneToChoose_13.png)

<div class="content-ad"></div>

![Image](/assets/img/2024-07-09-PrismaDrizzleTypeORMorSequalizeWhenYourFocusIsScaleWhichOneToChoose_14.png)

해당 엔티티들을 쿼리하기 위해서는 여러 종류의 조인이 존재하는데, 왼쪽 조인, 오른쪽 조인, 내부 조인 등이 있습니다.

여러 종류의 조인을 구현할 수 있는 기능을 갖추는 것이 왜 중요한가요?

적절한 조인 기술과 조건을 선택하여 조인에 관여하는 행의 수를 최소화하면 쿼리 성능을 크게 향상시킬 수 있습니다. 데이터 관계를 이해하는 것이 조인 작업을 최적화하는 데 중요합니다.

<div class="content-ad"></div>

# Drizzle, Prisma, TypeORM, 그리고 Sequelize에서 다른 조인 유형을 적용할 수 있을까요?

Drizzle —

Drizzle은 모든 조인 유형을 구현할 수 있는 기능을 제공합니다.

<img src="/assets/img/2024-07-09-PrismaDrizzleTypeORMorSequalizeWhenYourFocusIsScaleWhichOneToChoose_15.png" />

<div class="content-ad"></div>

프리즈마 -

프리즈마 클라이언트의 기본 구성은 중첩된 읽기에 조인을 사용하지 않습니다. 다음 코드는 다음 쿼리를 생성합니다:

![image1](/assets/img/2024-07-09-PrismaDrizzleTypeORMorSequalizeWhenYourFocusIsScaleWhichOneToChoose_16.png)

![image2](/assets/img/2024-07-09-PrismaDrizzleTypeORMorSequalizeWhenYourFocusIsScaleWhichOneToChoose_17.png)

<div class="content-ad"></div>

Prisma 클라이언트에는 "relationJoins"라는 설정이 있습니다. 이 설정은 PostgreSQL에 Lateral Join 쿼리와 JSON 집계를 결합하여 다음 쿼리를 생성합니다.

![](/assets/img/2024-07-09-PrismaDrizzleTypeORMorSequalizeWhenYourFocusIsScaleWhichOneToChoose_18.png)

그러나 다양한 조인 유형에 대해 명시적인 제어가 없습니다. Prisma 문서에는 데이터 세트 및 쿼리의 특성에 따라 쿼리가 더 효율적 일 수 있는 경우가 있음을 명시하고 있습니다.

TypeORM에서는 여러 조인 유형을 구현할 수 있습니다. Sequalize에서도 여러 Join 유형을 구현할 수 있지만 ORM의 내부를 파고들어 각 상황에서 어떤 쿼리가 생성될지 이해해야 합니다. 다음은 TypeORM에서 Inner Join을 구현하는 코드의 예시입니다:

<div class="content-ad"></div>

![Image 1](/assets/img/2024-07-09-PrismaDrizzleTypeORMorSequalizeWhenYourFocusIsScaleWhichOneToChoose_19.png)

Nested Reads Summary

For implementing multiple join types in Nested reads, here is the summary of the abilities of each tool :

![Image 2](/assets/img/2024-07-09-PrismaDrizzleTypeORMorSequalizeWhenYourFocusIsScaleWhichOneToChoose_20.png)

<div class="content-ad"></div>

# 중첩 쓰기 — 단일 엔티티 및 대량 쓰기

중첩 쓰기를 구현할 때 중요한 2가지 포인트가 있습니다.

단일 엔티티 삽입의 경우 — 테이블 간에 데이터를 삽입할 때는 트랜잭션을 사용해야 합니다. 그렇지 않으면 에러 시에 데이터베이스가 일관성 없는 상태가 될 수 있습니다.

여러 엔티티를 삽입해야 하는 경우, 특히 대규모로 작업해야 하는 경우 — 다양한 이유로 대량 쓰기를 사용하는 것이 중요합니다. 대량 쓰기는 여러 개의 삽입 문을 실행하는 오버헤드를 줄여주고, 데이터베이스가 삽입을 더 효율적으로 최적화하게 하며, 데이터베이스 라운드 트립을 줄여줍니다.

<div class="content-ad"></div>

# 중첩 쓰기 Drizzle, Prisma, TypeORM, 그리고 Sequalize

Drizzle —

단일 중첩 엔티티를 삽입하는 경우 — Drizzle은 기본적으로 트랜잭션을 실행하지 않습니다. 아래는 단일 중첩 엔티티를 삽입하는 Drizzle 코드 예제입니다. 이 코드는 DB에서 트랜잭션을 실행하지 않으며 트랜잭션을 실행하려면 Drizzle의 트랜잭션 API를 사용해야 합니다.

![이미지](/assets/img/2024-07-09-PrismaDrizzleTypeORMorSequalizeWhenYourFocusIsScaleWhichOneToChoose_21.png)

<div class="content-ad"></div>

대량 삽입의 경우, Drizzle은 중첩 엔티티의 대량 삽입을 지원합니다. 그러나 이는 모든 DB 드라이버에 대해 지원되는 것이 아니라 일부 DB 드라이버에 대해서만 지원됩니다. 그래서 결국 선택한 DB 드라이버에 따라 다르게 작동합니다.

Prisma -

단일 중첩 엔티티 삽입의 경우, Prisma는 기본적으로 트랜잭션을 지원합니다. 다음 코드는 사용자를 삽입하고 사용자가 생성한 게시물과 해당 프로필을 삽입하기 위해 DB 트랜잭션을 생성합니다.

<div class="content-ad"></div>

<img src="/assets/img/2024-07-09-PrismaDrizzleTypeORMorSequalizeWhenYourFocusIsScaleWhichOneToChoose_23.png" />

대량 삽입 시 — Prisma는 대량 작업을 지원하지만 중첩된 엔티티로의 대량 작업은 지원하지 않습니다.

<img src="/assets/img/2024-07-09-PrismaDrizzleTypeORMorSequalizeWhenYourFocusIsScaleWhichOneToChoose_24.png" />

Prisma로 대량 작업을 작성하기 위해서는 Prisma의 원시 쿼리 API를 사용해 볼 수 있습니다.

<div class="content-ad"></div>

TypeORM과 Sequelize는 한 개의 중첩된 엔티티를 삽입할 때 기본적으로 트랜잭션을 지원하며 중첩된 엔티티에 대한 대량 삽입은 지원하지 않습니다.

다양한 도구에 대한 단일 중첩 엔티티 삽입 요약은 다음과 같습니다:

![이미지](/assets/img/2024-07-09-PrismaDrizzleTypeORMorSequalizeWhenYourFocusIsScaleWhichOneToChoose_25.png)

또한 다양한 도구의 대량 삽입 능력에 대한 요약이 여기 있습니다. Drizzle을 제외하고는 어떤 도구도 중첩된 엔티티에 대한 대량 삽입을 지원하지 않습니다. 이에 대한 해결책은 해당 도구들이 노출하는 raw query API를 사용하는 것입니다.

<div class="content-ad"></div>

![이미지](/assets/img/2024-07-09-PrismaDrizzleTypeORMorSequalizeWhenYourFocusIsScaleWhichOneToChoose_26.png)

# 다양한 도구들의 능력을 비교한 후 — 어떤 ORM을 선택해야 할까요?

데이터 액세스 레이어에 어떤 ORM을 선택해야 할지 — 이는 데이터 시나리오에 따라 다릅니다.

데이터 집중적인 작업과 복잡한 시나리오

<div class="content-ad"></div>

이런 상황에서는 가능한한 많은 쿼리 제어가 필요합니다. 대규모 규모에서 작업할 수 있도록 최적의 선택은 Drizzle이며, 네이티브 드라이버로 작업하고 쿼리를 직접 구현하는 것도 고려해야 합니다.

중간 복잡성

TypeORM과 Sequelize는 당신에게 견고한 쿼리 제어와 탁월한 개발 경험을 제공할 것입니다.

간단한 CRUD 시나리오, 풀 스택 팀

<div class="content-ad"></div>

해당 경우에는 Prisma가 탁월한 선택지입니다. 매우 좋은 개발 경험을 제공하며 배우기 쉽고, 해당 경우에는 탁월한 능력을 제공합니다.
