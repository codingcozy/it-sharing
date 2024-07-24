---
title: "2024 최신 Java 고급 동시성 인터뷰 질문 모음"
description: ""
coverImage: "/assets/img/2024-07-23-JavaAdvancedConcurrencyInterviewQuestions01_0.png"
date: 2024-07-23 21:51
ogImage:
  url: /assets/img/2024-07-23-JavaAdvancedConcurrencyInterviewQuestions01_0.png
tag: Tech
originalTitle: "Java Advanced Concurrency Interview Questions01"
link: "https://medium.com/@vikas.taank_40391/java-advanced-concurrency-interview-questions-01-5ca048dfb844"
---

![image](/assets/img/2024-07-23-JavaAdvancedConcurrencyInterviewQuestions01_0.png)

면접에서는 종종 주제에 대한 깊이를 평가하려고 할 것입니다. 예를 들어, Java 동시성과 비동기 작업 실행의 일부 고급 구조를 사용할 수 있더라도 멀티 스레딩 및 동시성이 시간이 지남에 따라 어떻게 발전했는지를 이해하려고 할 것입니다. 예를 들어 Java에서 스레드를 관리하고 생성하는 여러 가지 방법을 알고 있는지 알아볼 것입니다. Java에서 스레드 생성 및 관리에 대한 훌륭한 참고 자료가 있습니다.

그러나 동시성 개념을 이해하고 구현하기 위해서는 어떻게 스레딩이 작동하는지, 간단한 문제를 해결하는 동안 이러한 개념을 어떻게 구현할 수 있는지 이해해야 할 수도 있습니다.

Java에서 스레드 간 통신 및 스레드의 상태를 이해하는 데 좋은 리딩 자료가 있습니다. 프로그래밍에 활용할 수 있습니다.

<div class="content-ad"></div>

지금 이 글에서는 한 가지 문제에 대해 살펴보고, 해당 문제를 간단한 스레드 생성 또는 Completable Future의 일부 추상화를 사용하여 해결하고자 합니다.

여기서 유의해야 할 중요한 점은 여러 스레드가 객체의 상태를 공유하도록 허용하려면 동기화 구조체에는 대체물이 없다는 것입니다. 동시성을 처리하고 여러 비동기 작업을 만드는 것은 전혀 다른 개념이며 서로 다른 사용 사례를 구현합니다.

# 문제:

![이미지](/assets/img/2024-07-23-JavaAdvancedConcurrencyInterviewQuestions01_1.png)

<div class="content-ad"></div>

<img src="/assets/img/2024-07-23-JavaAdvancedConcurrencyInterviewQuestions01_2.png" />

```java
import java.util.concurrent.CompletableFuture;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class EvenOddPrinter {
    public static void main(String[] args) {
        final int maxNumber = 20; // 출력할 최대 숫자 정의
        ExecutorService executor = Executors.newFixedThreadPool(2); // 두 개의 스레드를 가진 스레드 풀 생성

        Runnable printEven = () -> {
            for (int i = 2; i <= maxNumber; i += 2) { // 짝수는 2부터 시작
                System.out.println("짝수: " + i);
                try {
                    Thread.sleep(100); // 몇 가지 연산을 시뮬레이션하고 출력을 더 가독성 있게 만들기 위해 슬립
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt(); // 중단된 상태를 복원
                    System.out.println("스레드가 중단되었습니다: " + e.getMessage());
                }
            }
        };

        Runnable printOdd = () -> {
            for (int i = 1; i <= maxNumber; i += 2) { // 홀수는 1부터 시작
                System.out.println("홀수: " + i);
                try {
                    Thread.sleep(100); // 몇 가지 연산을 시뮬레이션하고 출력을 더 가독성 있게 만들기 위해 슬립
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt(); // 중단된 상태를 복원
                    System.out.println("스레드가 중단되었습니다: " + e.getMessage());
                }
            }
        };

        CompletableFuture<Void> evenFuture = CompletableFuture.runAsync(printEven, executor);
        CompletableFuture<Void> oddFuture = CompletableFuture.runAsync(printOdd, executor);

        CompletableFuture<Void> combinedFuture = CompletableFuture.allOf(evenFuture, oddFuture);
        combinedFuture.join(); // 두 작업이 완료될 때까지 기다림

        executor.shutdown(); // Executor 서비스 종료
    }
}
```

# 출력:

<img src="/assets/img/2024-07-23-JavaAdvancedConcurrencyInterviewQuestions01_3.png" />

<div class="content-ad"></div>

# 이제 인터뷰에서는 고급 구조를 사용하지 않고 쓰레드를 본질적으로 사용할 수 있는지 확인하려고 합니다.

![이미지](/assets/img/2024-07-23-JavaAdvancedConcurrencyInterviewQuestions01_4.png)

# Completable Future를 사용하지 않고 더 효율적인 방법.

짝수와 홀수 번호 출력 프로그램을 더 효율적이고 동기화된 상태로 만들기 위해 두 쓰레드 간의 조정을 위해 공유 객체를 활용할 수 있습니다. 짝수와 홀수 출력 작업이 번갈아 가며 실행되도록 보장하기 위해 sleep() 대신에 wait()와 notify()를 사용하여 쓰레드 실행을 효율적으로 관리할 수 있습니다. 이 접근 방식은 불필요한 대기를 줄이고 프로그램을 더 반응적으로 만듭니다.

<div class="content-ad"></div>

<img src="/assets/img/2024-07-23-JavaAdvancedConcurrencyInterviewQuestions01_5.png" />

```java
public class EvenOddPrinterUsingThreads {
    private static final Object lock = new Object();
    private static final int MAX_NUMBER = 20;
    private static int current = 1;

    public static void main(String[] args) {
        Thread evenThread = new Thread(() -> printEvenNumbers());
        Thread oddThread = new Thread(() -> printOddNumbers());

        evenThread.start();
        oddThread.start();
    }

    private static void printEvenNumbers() {
        while (current <= MAX_NUMBER) {
            synchronized (lock) {
                if (current % 2 == 1) { // If current is odd, wait
                    try {
                        lock.wait();
                    } catch (InterruptedException e) {
                        Thread.currentThread().interrupt();
                    }
                } else {
                    System.out.println("Even: " + current);
                    current++;
                    lock.notifyAll(); // Notify the other thread
                }
            }
        }
    }

    private static void printOddNumbers() {
        while (current <= MAX_NUMBER) {
            synchronized (lock) {
                if (current % 2 == 0) { // If current is even, wait
                    try {
                        lock.wait();
                    } catch (InterruptedException e) {
                        Thread.currentThread().interrupt();
                    }
                } else {
                    System.out.println("Odd: " + current);
                    current++;
                    lock.notifyAll(); // Notify the other thread
                }
            }
        }
    }
}
```

Output:

<div class="content-ad"></div>

# 주요 향상 사항:

- 동기화된 블록은 한 번에 하나의 스레드만 블록을 실행할 수 있도록 보장합니다. 이로 인해 두 스레드가 동시에 숫자를 인쇄하려고 하는 것을 방지합니다.
- 효율적인 대기 및 통지: sleep() 대신 wait() 및 notifyAll()을 사용하여 스레드가 실행을 일시 중지하고 자신의 차례가 될 때까지 기다릴 수 있어서 유휴 시간을 줄입니다.
- 공유 카운터: 두 스레드가 현재 카운터를 공유하고 이를 사용하여 인쇄할 숫자를 결정합니다. 이를 통해 별도로 범위를 계산하거나 확인할 필요 없이 올바른 순서를 보장합니다.

# 작동 방식:

- 두 스레드가 시작되고 숫자를 인쇄하기를 시도합니다.
- 현재 값에 따라 한 스레드가 숫자를 인쇄하고 현재 값을 증가시키고, 다른 스레드는 기다립니다.
- 인쇄를 마친 후, 스레드는 notifyAll()을 호출하여 다른 스레드를 깨우고, 그 다음 반복에서 다시 현재 값을 확인합니다. 만일 자신의 차례가 아니라면 다시 대기합니다.
- 이 과정은 현재 값이 MAX_NUMBER를 초과할 때까지 반복되며, 그때 두 스레드가 실행을 완료하고 자연스럽게 종료됩니다.

<div class="content-ad"></div>

# 이제 인터뷰어가 더 이에 대해 질문할 수 있을 것 같습니다.

![이미지](/assets/img/2024-07-23-JavaAdvancedConcurrencyInterviewQuestions01_7.png)

Predicate 인터페이스를 사용하면 두 가지 조건을 설계하고 이러한 조건을 CompletableFuture에 전달하여 더 쉽게 구현할 수 있습니다. 유일한 차이점은 이제 홀수와 짝수를 확인하는 로직이 Predicate 자체에 의해 제어된다는 것입니다.

```java
import java.util.concurrent.CompletableFuture;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.atomic.AtomicInteger;
import java.util.function.Predicate;

public class EvenOddPrinterWithPredicate {

    private static final int MAX_NUMBER = 20;
    private static AtomicInteger current = new AtomicInteger(1);
    private static final Object lock = new Object();

    public static void main(String[] args) {
        ExecutorService executor = Executors.newFixedThreadPool(2);

        Predicate<Integer> isEven = num -> num % 2 == 0;
        Predicate<Integer> isOdd = isEven.negate();

        CompletableFuture<Void> evenTask = CompletableFuture.runAsync(() -> printNumbers(isEven), executor);
        CompletableFuture<Void> oddTask = CompletableFuture.runAsync(() -> printNumbers(isOdd), executor);

        CompletableFuture.allOf(evenTask, oddTask).join(); // 두 작업이 모두 완료될 때까지 기다림

        executor.shutdown();
    }

    private static void printNumbers(Predicate<Integer> condition) {
        while (current.get() <= MAX_NUMBER) {
            synchronized (lock) {
                if (condition.test(current.get())) {
                    System.out.println((condition == isEven ? "짝수: " : "홀수: ") + current.getAndIncrement());
                    lock.notify();
                } else {
                    try {
                        lock.wait();
                    } catch (InterruptedException e) {
                        Thread.currentThread().interrupt();
                    }
                }
            }
        }
    }
}
```

<div class="content-ad"></div>

# 주요 변경 사항 및 기능:

- 우리는 두 개의 Predicate`Integer` 인스턴스인 isEven과 isOdd를 정의하여 각각 짝수와 홀수 숫자에 대한 조건을 캡슐화했습니다. 이렇게 함으로써 printNumbers 메서드 내의 조건 확인이 더 표현력 있고 유연해집니다.
- printNumbers 메서드는 숫자를 인쇄할 조건을 나타내는 Predicate`Integer`를 조건 매개변수로 사용합니다. 이 메서드는 짝수와 홀수 작업에서 모두 사용되어 코드 중복을 줄입니다.
- 함수형 프로그래밍 스타일: Predicate를 조건 확인에 사용하여 함수형 프로그래밍 스타일을 보여줌으로써 코드를 더 모듈화하고 이해하기 쉽게 만듭니다.
- 비동기 실행을 위한 CompletableFuture: CompletableFuture.runAsync()와 executor를 사용하여 작업을 비동기적으로 실행하여 병렬로 실행되지만 동기화를 통해 조정됩니다.

우리가 본 모든 예제에서는 성능을 벤치마크해보지 않았지만, 주제의 심도를 나타내기 위해 동일한 문제를 여러 가지 방법으로 해결하려고 노력했습니다.

# 주요 포인트:

<div class="content-ad"></div>

- 비동기 이벤트 처리는 동기화 필요성을 제거하지 않습니다.
- 스레드 간 동기화는 서로 작업하고 통신할 때 항상 필요합니다.
- 모든 솔루션은 동일한 목표를 달성하지만 약간 다른 프로그래밍 및 구조 스타일을 가지고 있습니다.
- 이 글은 다른 것보다 더 나은 목록을 증명하려는 것이 아닙니다.

클랩을 공유해주시고 고급 Java 개발자에게는 매우 사소한 문제일 수 있겠지만, 이 글은 다양한 멀티스레딩 스타일에 대한 지식을 넓히는 데 초점을 맞추고 있습니다. 클랩 공유해주시고 다시 방문해주시기 바랍니다.
