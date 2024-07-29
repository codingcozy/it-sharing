---
title: "Java에서 동시 네트워크 요청을 효율적으로 실행하는 현대적인 방법"
description: ""
coverImage: "/assets/img/2024-07-29-AModernWaytoEfficientlyExecuteConcurrentNetworkRequestsinJava_0.png"
date: 2024-07-29 13:54
ogImage: 
  url: /assets/img/2024-07-29-AModernWaytoEfficientlyExecuteConcurrentNetworkRequestsinJava_0.png
tag: Tech
originalTitle: "A Modern Way to Efficiently Execute Concurrent Network Requests in Java"
link: "https://medium.com/better-programming/a-modern-way-to-efficiently-execute-concurrent-network-requests-in-java-c8dceb4e7f4f"
---


<img src="/assets/img/2024-07-29-AModernWaytoEfficientlyExecuteConcurrentNetworkRequestsinJava_0.png" />

자바 19에서 소개된 가상 스레드는 동시 네트워크 요청을 가속화하기 위한 것입니다. 이 게시물에서는 일반 스레드와 가상 스레드가 HTTP 요청을 하는 처리량을 비교하고 싶습니다. 이를 위해 Google Cloud의 두 가상 머신을 사용했습니다. 각 머신은 8 CPUs와 16GB 메모리를 가지고 있습니다. 한 대는 서버로, 다른 한 대는 클라이언트로 사용합니다.

서버 머신은 작은 스프링 부트 애플리케이션을 실행합니다. URL에서 받은 i 매개변수의 값을 반환하는 예제입니다:

```java
@SpringBootApplication
@RestController
public class Main {

    public static void main(String[] args) {
        SpringApplication.run(Main.class, args);
    }

    @GetMapping("/")
    public String hello(@RequestParam(value = "i") String i) {
        return i;
    }
}
```

<div class="content-ad"></div>

클라이언트 애플리케이션에서는 반환된 값들의 합을 사용하여 모든 동시 요청이 유효한 응답을 반환했는지 간단히 확인합니다.

클라이언트 애플리케이션은 전통적인 캐시된 스레드 풀 및 개별 가상 스레드를 사용하여 동일한 세트의 동시 HTTP 요청을 보냅니다. 각 HTTP 요청의 일련 번호는...