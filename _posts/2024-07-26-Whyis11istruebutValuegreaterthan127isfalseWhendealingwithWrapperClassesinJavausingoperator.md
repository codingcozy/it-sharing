---
title: "자바 Wrapper 클래스에서 1  1은 참이지만 값이 127을 초과하면  연산자가 거짓이 되는 이유는자바에서 Wrapper 클래스예 Integer, Long는 기본 타입을 객체로 감싸주는 역할을 합니다 이 클래스들은 참조 타입이므로,  연산자를 사용할 때 값이 아닌 참조를 비교하게 됩니다 그러나 Integer 클래스의 경우, -128에서 127 사이의 값은 자주 사용되므로 캐싱됩니다 이 때문에 같은 값의 참조가 동일해  비교가 참이지만, 이 범위를 벗어나는 값은 새로운 객체로 생성되어 참조가 달라져  비교가 거짓이 됩니다"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-07-26 12:00
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Why is 1  1 is true but Value greater than 127 is false When dealing with Wrapper Classes in Java using  operator"
link: "https://medium.com/@gainjavaknowledge/why-is-1-1-is-true-but-value-greater-than-127-is-false-when-dealing-with-wrapper-classes-in-java-46c872fff802"
---


이번 튜토리얼에서는 Wrapper 클래스를 비교할 때 '==' 연산자를 사용한 경우 발생하는 동작에 대해 설명하겠습니다.

아래 예시를 살펴보겠습니다:

```java
public class TestWrapper {

    public static void main(String[] args) {
        Integer a = Integer.valueOf(1);
        Integer b = 1;              // 자동 박싱
        
        System.out.println(a == b); // true
    }
}
```

이 예시는 간단한 예제입니다. Integer 클래스가 제공하는 valueOf() 메서드를 사용하여 두 개의 Integer 객체를 생성하고, 다른 하나는 자동 박싱 기능을 사용했습니다.

<div class="content-ad"></div>

다른 예제를 살펴보겠습니다. 이번에는 값이 127보다 커야 합니다:

```js
public class TestWrapper {

    public static void main(String[] args) {
        Integer a = Integer.valueOf(200);
        Integer b = 200;            // auto-boxing

        System.out.println(a == b); // false
    }
}
```

왜 출력 결과가 다른 걸까요?

== 연산자는 메모리 참조를 비교합니다. Java는 -128부터 127까지의 Integer를 캐싱하기 때문에, 첫 번째 Boolean은 실제로 동일한 캐시된 Integer를 비교하고 있습니다.

<div class="content-ad"></div>

두 번째 예시에서는 127보다 큰 값인 200이 캐시되지 않습니다. 이는 맵에 넣을 때 두 개의 다른 정수가 생성된다는 것을 의미합니다. 따라서 두 번째 불리언에 대해 ==를 호출하면 false가 반환됩니다.