---
title: "Java 8 Predicates를 사용하는 최고의 방법"
description: ""
coverImage: "/assets/img/2024-07-23-ThebestwaytouseJava8Predicates_0.png"
date: 2024-07-23 11:30
ogImage: 
  url: /assets/img/2024-07-23-ThebestwaytouseJava8Predicates_0.png
tag: Tech
originalTitle: "The best way to use Java 8 Predicates"
link: "https://medium.com/@kouomeukevin/the-best-way-to-use-java-8-predicates-e7dad8331863"
---


이 상세 가이드에서는 Java 8의 새로운 유용한 기능인 Java Predicates에 대해 알아보게 됩니다. 이 기사에서는 Java Predicates가 무엇이고 다양한 방법으로 그들을 어떻게 사용하는지에 대해 설명합니다.

![Java Predicates](/assets/img/2024-07-23-ThebestwaytouseJava8Predicates_0.png)

데이터 필터링, Predicates 연결, 일반적인 실수 피하기에 대해 알아가실 수 있습니다. Java Predicates를 사용하면 Java 프로그래밍 기술을 향상시키고 응용 프로그램을 더 나은 수준으로 끌어올릴 수 있습니다.

# Java Predicates란 무엇인가요?

<div class="content-ad"></div>

자바 8에서는 predicate이 java.util.function 패키지에 소개된 함수형 인터페이스입니다. 이는 인수를 받아 참 또는 거짓을 반환하는 부울 값 함수를 나타냅니다. Predicate 인터페이스에는 평가하는 부울 조건에 사용되는 test()라는 하나의 추상 메서드가 있습니다.

```java
@FunctionalInterface
public interface Predicate<T> {
  boolean test(T t);
}
```

일반적으로 predicate은 컬렉션 내 요소를 필터링하거나 평가하는 데 유용한 도구입니다.

# 시작하기

<div class="content-ad"></div>

자바 Predicate는 람다식을 사용하여 정의할 수 있는 함수형 인터페이스입니다. 문자열이 짝수 길이인지 확인하는 Predicate의 간단한 예시를 살펴보겠습니다:

```java
Predicate<String> startWithLetterN = s -> s.startsWith("N");
```

위 코드에서 startWithLetterN은 문자열 s가 문자 N으로 시작하는지 확인하는 Predicate입니다. 이 로직을 정의하기 위해 람다 표현식 `s -> s.startsWith("N")`을 사용합니다.

Predicate를 정의했다면 test() 메서드를 사용하여 데이터를 평가할 수 있습니다.

<div class="content-ad"></div>

```java
// Boolean result = startWithLetterN.test("Mateo");

@Test
void verify() {
    // assertThat(result).isFalse();
}
```

# 연쇄적인 Predicates

Predicates에 대해 연쇄적인 작업을 적용하는 것은 매우 간단하며, 첫 번째 Predicate에 대해 method 및 두 번째 Predicate를 매개변수로 전달하여 이를 달성할 수 있습니다.

```java
Predicate<String> startWithLetterP = s -> s.startsWith("P");
Predicate<String> lengthGreaterThan5 = s -> s.length() > 5;
Predicate<String> startWithPAndGreaterThan5 = startWithLetterP.and(lengthGreaterThan5);

@Test
void check() {
    assertThat(startWithPAndGreaterThan5.test("Penelope")).isTrue();
}
```

<div class="content-ad"></div>

# Predictates로 데이터 필터링하기

Predictates는 데이터를 필터링하는 데 자주 사용됩니다. 예를 들어, 특정 조건과 일치하는 항목의 컬렉션을 필터링하는 데 사용할 수 있습니다. 다음은 Predictates를 사용하여 숫자 목록을 필터링하는 예제입니다:

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

Predicate<Integer> isGreaterThan4 = n -> n > 4;
Predicate<Integer> isEven = n -> n % 2 == 0;
List<Integer> matchingNumbers = numbers.stream()
        .filter(isEven.and(isGreaterThan4))
        .collect(Collectors.toList());

@Test
void check() {
    assertThat(matchingNumbers).hasSize(3).containsExactly(6, 8, 10);
}
```

다양한 Predictates 메서드를 사용하여 모든 필요한 경우에 일치시킬 수 있습니다.

<div class="content-ad"></div>

자바 Predicates은 몇 가지 이점을 제공합니다. 인수로 전달되거나 결과로 반환될 수 있기 때문에 더 유연합니다. 또한 다른 문맥에서 조합하고 재사용할 수도 있습니다.

예를 들어, startWithLetterN과 같은 Predicate가 있다면 코드 전반에 걸쳐 여러 군데에서 사용할 수 있습니다. 이것은 루프 내에 정의된 조건이나 Stream의 filter() 메서드 내에서는 불가능합니다.

요약하자면, 자바에서 데이터를 필터링하는 데는 루프와 Stream을 사용할 수 있지만, 복잡한 조건과 큰 애플리케이션에서는 Predicates가 더 유연하고 재사용 가능한 접근법을 제공합니다.

# 피해야 할 함정들

<div class="content-ad"></div>

자바 프레디케이트를 사용할 때 잠재적인 함정이 있습니다. 가장 흔한 함정 중 하나는 메소드 참조와 함께 발생하는 NullPointerExceptions입니다. 이는 일반적으로 null 값을 포함하는 컬렉션을 다룰 때 발생합니다.

```java
List<String> categories = Arrays.asList("Culture", "Entertainment", null);
Predicate<String> isShortWord = s -> s != null && s.length() < 4;

@Test
void check() {
    categories.stream()
        .filter(isShortWord)
        .forEach(System.out::println);
}
// 이 부분은 java.lang.NullPointerException을 발생시킬 것입니다. 🪲
```

이 예제에서는 짧은 단어를 가진 문자열 목록을 필터링하여 가져오려고 합니다. 그러나 목록에는 null 값이 포함되어 있어서 null 값을 호출하려고 할 때 NullPointerException이 발생합니다.

이 문제를 피하려면 Predicate에 null 값을 무시하는 추가적인 확인을 추가할 수 있습니다:

<div class="content-ad"></div>

```java
List<String> categories = Arrays.asList("Culture", "Entertainment", null);
Predicate<String> isShortWord = s -> Objects.nonNull(s) && s.length() < 4;

@Test
void check() {
    categories.stream()
        .filter(isShortWord)
        .forEach(System.out::println);
}
// This will return an empty array []
```

또 다른 피해야 할 함정은 Predicate를 사용하여 연결된 작업의 순서입니다. and(), or(), negate()를 사용하여 Predicate를 연결할 때는 연산의 순서가 결과에 영향을 미칠 수 있다는 점을 기억해야 합니다.

예를 들어, null 값 처리에 대해 and() 연산은 교환 법칙을 따르지 않습니다:

```java
Predicate<Integer> isNotNull = Objects::nonNull;
Predicate<Integer> isGreaterThan3 = s -> s > 3;

Predicate<Integer> correctOrder = isNotNull.and(isGreaterThan3);
Predicate<Integer> incorrectOrder = isGreaterThan3.and(isNotNull);

@Test
void check() {
    System.out.println(correctOrder.test(null));  // false
    System.out.println(incorrectOrder.test(null));  // NullPointerException
}
```

<div class="content-ad"></div>

# 끝

본 글은 Java Predicates를 마스터하는 포괄적인 가이드로 제공됩니다. Predicates가 어떻게 작동하는지, Java 프로그래밍에서 얼마나 중요한지, 그리고 코드에서 잘 활용하는 방법에 대해 배웠습니다.

이 가이드를 읽으면서 Java Predicates에 대해 배우고, 몇 가지 고급 기술을 습득하여 프로젝트에서 자신감 있고 능숙하게 사용할 수 있게 되었습니다.

이 글이 마음에 들었기를 바라며, 이 튜토리얼이 도움이 되었는지 궁금합니다. 아래 댓글로 의견을 알려주세요.

<div class="content-ad"></div>

다가오는 블로그 포스트를 놓치지 않으려면 아래쪽에서 뉴스레터를 구독하는 걸 잊지마세요! 👇🏾