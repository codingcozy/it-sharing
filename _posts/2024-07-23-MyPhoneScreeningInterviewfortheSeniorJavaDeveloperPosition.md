---
title: "시니어 자바 개발자 면접 준비 및 질문 정리"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-07-23 11:28
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "My Phone Screening Interview for the Senior Java Developer Position"
link: "https://medium.com/@kolheankita15/my-phone-screening-interview-for-the-senior-java-developer-position-d93813b62123"
isUpdated: true
updatedAt: 1723812790191
---



이번 면접에서 면접관은 Java, Spring, Microservice, SQL, Kafka, 그리고 Hibernate에서 내 자신을 평가하라고 했어요. 아래에는 이번 면접에서 받은 모든 질문들을 적어봤어요.

- 마이크로서비스끼리 어떻게 효과적으로 통신할 수 있나요?

답변: 마이크로서비스는 서로 HTTP/REST나 gRPC와 같은 동기 방식, 실시간 요청을 위한 방법과 RabbitMQ, Kafka와 같은 메시지 브로커와 같은 비동기 방식을 사용하여 디커플링하고 확장 가능한 상호 작용을 할 수 있어요. 서비스 검색 도구와 API 게이트웨이를 활용하여 이러한 통신을 효율적으로 관리하고 최적화할 수 있어요.

2. SQL에서 언제 프로시저와 함수를 사용하나요?
답변: SQL에서 프로시저는 값을 반환하지 않는 작업을 수행하기 위해 자주 일련의 SQL 문을 실행하는 데 사용돼요. 그에 반해, 함수는 계산을 수행하고 단일 값을 반환하기 위해 사용돼요. 복잡한 비즈니스 로직에는 프로시저를 사용하고 SQL 쿼리 내에서 재사용 가능한 계산이나 변환을 위해 함수를 사용해요.

<div class="content-ad"></div>

3. 어떤 캐싱 메커니즘을 사용해보셨나요?
답변: 저는 여러 가지 캐싱 메커니즘을 사용해봤어요, 그중에는 다음과 같은 것들이 있습니다:

- In-Memory Caching: 자주 액세스되는 데이터를 빠르게 검색하기 위해 Java 애플리케이션에서 Ehcache나 Guava와 같은 라이브러리를 사용하여 메모리에 저장합니다.
- Distributed Caching: Redis나 Memcached와 같은 도구를 사용하여 캐시된 데이터를 여러 서버 간에 공유하여 확장성과 신뢰성을 향상시킵니다.
- HTTP Caching: Varnish와 같은 캐싱 헤더 및 프록시를 구현하여 HTTP 응답을 캐시하고 백엔드 서버의 부하를 줄입니다.

4. 응집도와 결합도란 무엇이며, 어떤 종류의 결합도가 있나요?
답변: 응집도는 단일 모듈이나 구성 요소의 책임이 얼마나 관련되고 집중되어 있는지를 나타내며, 유지보수성을 위해 높은 응집도가 바람직합니다.

<div class="content-ad"></div>

결합이란 모듈 간 상호 의존성의 정도를 나타내며, 낮은 결합은 유연성을 위해 바람직합니다. 결합의 종류에는 내용, 공통, 제어, 스탬프, 데이터 및 메시지 결합이 있으며, 의존성이 가장 높은 것부터 가장 낮은 것까지 다양합니다.

5. eh-cache에 대해 설명해 줄 수 있나요? 캐시에 모든 데이터를 로드하거나 특정 레코드를 로드하나요?
답변: Eh-cache는 효율적인 데이터 캐싱을 제공하는 Java 기반 인-메모리 캐싱 라이브러리입니다. 구성 설정에 따라 요청 시 특정 레코드를 로드하거나 캐시로 데이터를 미리로드(preload)할 수 있습니다.

6. get()과 load()의 차이는 무엇인가요?
답변: 

- get(): 즉시 데이터베이스에서 객체를 검색하거나 객체를 찾을 수 없는 경우 null을 반환하며, 검색 시점에 객체가 완전히 초기화되도록 보장합니다.
- load(): 데이터베이스에 즉시 접근하지 않고 프록시 객체를 반환하며, 최초 접근 시 객체를 찾을 수 없으면 예외를 throw합니다.

<div class="content-ad"></div>

7. Hibernate에서 뷰와 조인을 어떻게 다루나요?
답변: Hibernate에서 뷰와 조인은 아래와 같이 다룹니다:

뷰 처리:

- Entity에 매핑:

- 뷰를 테이블처럼 취급하여 엔티티 클래스에 매핑합니다.

<div class="content-ad"></div>

```java
@Entity
@Table(name = "view_name")
public class ViewEntity {
    // fields and mappings
}
```

2. 읽기 전용 구성:

- 보통 뷰는 읽기 전용이므로, 해당 엔티티를 읽기 전용으로 구성할 수 있습니다.

```java
@Entity
@Table(name = "view_name")
public class ViewEntity {
    // fields and mappings
}
```

<div class="content-ad"></div>

# 조인 처리:

- JPQL/HQL:

- 엔티티 간 조인을 수행하려면 JPQL(Java Persistence Query Language) 또는 HQL(Hibernate Query Language)을 사용하세요.

```java
String hql = "SELECT e FROM Employee e JOIN e.department d WHERE d.name = :deptName";
List<Employee> results = session.createQuery(hql)
                                 .setParameter("deptName", "Sales")
                                 .list();
```

<div class="content-ad"></div>

2. Criteria API:

Criteria API를 사용하여 조인에 대해 더 프로그래밍적인 접근 방식을 사용하세요.

```java
CriteriaBuilder cb = session.getCriteriaBuilder();
CriteriaQuery<Employee> cq = cb.createQuery(Employee.class);
Root<Employee> employee = cq.from(Employee.class);
Join<Employee, Department> department = employee.join("department");
cq.select(employee).where(cb.equal(department.get("name"), "Sales"));
List<Employee> results = session.createQuery(cq).getResultList();
```

3. Named Queries:

<div class="content-ad"></div>

- 엔티티 클래스 내에서 명명된 쿼리에서 조인을 정의합니다.

```js
@NamedQuery(
    name = "Employee.findByDepartment",
    query = "SELECT e FROM Employee e JOIN e.department d WHERE d.name = :deptName"
)
```

위의 방법을 사용하여 하이버네이트에서 뷰와 조인을 처리하여 필요한대로 데이터를 검색하고 조작할 수 있습니다.

Kafka 용어와 작동 방식에 대해 설명하시오.

<div class="content-ad"></div>

답변: Apache Kafka는 프로듀서가 레코드를 토픽에 보내고, 컨슈머가 이러한 레코드를 토픽에서 읽는 분산 이벤트 스트리밍 플랫폼입니다. 브로커는 레코드의 저장 및 검색을 관리하며, 토픽 내 파티션은 병렬 처리와 확장성을 가능하게 합니다. Kafka는 실시간 데이터 처리의 고 처리량, 내고장성 및 내구성을 보장합니다.

9. 언제 SQL 및 NoSQL을 사용하나요?

답변: SQL (구조화된 쿼리 언어) 및 NoSQL(Not Only SQL) 데이터베이스는 데이터의 성격 및 응용 프로그램의 특정 요구 사항에 따라 다른 시나리오에서 사용됩니다:

SQL을 사용하는 경우:

<div class="content-ad"></div>

- 구조화된 데이터: 데이터가 매우 구조화되어 있고 스키마를 따를 때는 SQL 데이터베이스가 데이터 무결성과 일관성을 보장하기에 적합합니다.
- ACID 트랜잭션: 트랜잭션이 원자성, 일관성, 고립성, 지속성 보장이 필요할 때 SQL 데이터베이스는 견고한 지원을 제공합니다.
- 복잡한 쿼리: 복잡한 조인, 집계 및 주어진 쿼리를 필요로 하는 응용프로그램에서는 SQL 데이터베이스가 강력한 쿼링 기능을 제공하는 데 뛰어납니다.
- 수직 확장성: 수직 확장성(단일 서버에 더 많은 자원을 추가하여 확장)으로 응용프로그램의 성능 요구를 충족할 때.

NoSQL을 사용하는 경우:

- 비구조화 또는 반구조화된 데이터: 데이터가 스키마 없거나 관계형 모델에 잘 적합하지 않을 때는 NoSQL 데이터베이스가 유연성을 제공합니다.
- 확장성 및 가용성: NoSQL 데이터베이스는 수평 확장성(여러 서버에 걸쳐 확장)을 위해 설계되어 있어 대량의 데이터 및 고 처리량을 다루기에 적합합니다.
- 최종 일관성: 최종 일관성(데이터 일관성이 시간이 지남에 따라 달성)이 허용되고 가용성 및 분할 허용에 대한 일관성 요구가 완화된 경우.
- 문서, 키-값, 그래프 또는 칼럼 데이터 모델: 특정 NoSQL 데이터베이스 유형(예: 문서용 MongoDB, 키-값 쌍용 Redis, 그래프용 Neo4j)에 따라 데이터 모델이 응용프로그램 요구 사항과 일치하는 경우.

SQL과 NoSQL 중 어느 것을 선택할지는 데이터 구조, 확장성 요구 사항, 일관성 요구 사항 및 응용프로그램의 특정 사용 사례와 같은 요인에 따라 결정됩니다. 종종 각 유형의 장점을 활용하기 위해 하이브리드 접근법이나 다중 데이터 저장소(동일 시스템 내에서 SQL 및 NoSQL 데이터베이스 모두 사용)이 채택됩니다.

<div class="content-ad"></div>

10. 성능 테스트에는, 웹 애플리케이션의 부하 테스트에 Apache JMeter를 주로 사용하고, 실시간 모니터링 및 상세 보고에는 Gatling을 사용하며 때로는 간단한 HTTP 서버 벤치마킹을 위해 Apache Bench (ab)를 사용합니다. 각 도구는 다양한 조건 하에서 응용 프로그램 성능을 측정하는 데 특정 강점을 제공합니다.

11. 서비스 및 데이터베이스 계층을 어떻게 테스트합니까?
답변: 서비스 계층을 테스트하기 위해, Mockito와 같은 mocking 프레임워크를 사용하여 의존성을 격리하고 비즈니스 로직을 검증하는 단위 테스트를 사용합니다. 데이터베이스 계층 테스트에는 올바른 데이터 상호작용 및 쿼리 실행을 보장하기 위해 내부 메모리 데이터베이스나 테스트 컨테이너와 같은 통합 테스트를 활용합니다. 단, 프로덕션 데이터에 영향을 미치지 않습니다.

12. Java에서 데이터 동시성을 처리하는 방법은 무엇인가요?
답변: 저는 이 주제에 대해 상세한 기사를 작성했습니다.

<div class="content-ad"></div>

이 링크를 방문해주세요: Java에서 데이터 동시성을 다루는 방법

감사합니다. 여러분들이 면접 준비에 도움이 될 수 있기를 바랍니다.

- 👏 이 글에 박수를 보내주세요. 글을 널리 퍼지게 도와주세요 👏
- 🔔 팔로우하기: Medium | LinkedIn
- ✉️ 뉴스레터 구독하기