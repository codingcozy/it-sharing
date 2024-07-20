---
title: "Python 없이 자바 앱에 AI 통합하기  쉬운 방법"
description: ""
coverImage: "/assets/img/2024-07-20-IntegrateAIintoJavaAppsNoPythonRequired_0.png"
date: 2024-07-20 11:35
ogImage: 
  url: /assets/img/2024-07-20-IntegrateAIintoJavaAppsNoPythonRequired_0.png
tag: Tech
originalTitle: "Integrate AI into Java Apps  No Python Required"
link: "https://medium.com/donvitocodes/integrate-ai-into-java-apps-no-python-required-3e1d996a2d92"
---


![image](/assets/img/2024-07-20-IntegrateAIintoJavaAppsNoPythonRequired_0.png)

# Java 애플리케이션에 AI 통합하기 - 파이썬 필요 없음

이 블로그 포스트에서는 OpenAI의 언어 모델과 Java를 사용하여 간단한 채팅 API를 만들기 위해 Spring AI를 활용하는 방법을 알아볼 것입니다. 실제 예시에 대해 설명하기 전에, 이것이 왜 흥미로운지 살펴봅시다!

# 자바 개발자 여러분, 환영합니다!

<div class="content-ad"></div>

새로운 언어를 배울 필요가 없어요!

Java 개발자로서, AI 개발에 뛰어들기 위해서는 Python과 같은 새로운 언어를 배워야 한다고 생각할 수 있습니다. 어찌됐든 Python은 강력한 AI 라이브러리와 프레임워크로 잘 알려져 있습니다. 그러나 Spring AI를 활용하면 Java 개발자들은 익숙한 환경을 벗어나지 않고도 AI 통합을 성취할 수 있습니다.

대부분의 기업 애플리케이션은 안정성, 확장성, 기업 수준의 지원 덕분에 Java로 개발됩니다. Spring AI를 사용하여 AI를 이러한 애플리케이션에 직접 통합하면 비즈니스는 기술 스택을 전면적으로 변경할 필요 없이도 기능과 사용자 경험을 향상시킬 수 있습니다. 또한, 개발자들이 익숙한 환경에서 작업할 수 있으며, 생산성을 향상시키고 학습 곡선을 줄일 수 있습니다.

Java 생태계 내에서 활동한다는 것은 기존 기술과 지식을 계속 활용할 수 있다는 뜻입니다. 새로운 언어를 배우거나 다른 패러다임에 적응할 필요가 없습니다. 이를 통해 시간과 자원을 절약할 뿐만 아니라 코드베이스의 일관성을 유지하여, 향후 업데이트와 유지보수를 더 쉽게 만들 수 있습니다.

<div class="content-ad"></div>

# Spring AI란 무엇인가요?

Spring AI는 개발자들이 인공지능 기능을 갖춘 애플리케이션을 쉽게 만들 수 있도록 하는 라이브러리입니다. Spring AI의 주요 목표는 회사의 데이터와 시스템을 인공지능 모델과 간단하게 연결하는 데 도움을 주는 것입니다.

💡

핵심적으로 Spring AI는 AI 통합의 기본적인 과제에 대응합니다: 기업 데이터와 API를 AI 모델과 연결하는 것입니다.

<div class="content-ad"></div>

도서관은 AI 애플리케이션을 구축하는 데 사용할 수 있는 도구와 기능 세트를 제공합니다. 이에는 OpenAI, Microsoft, Amazon, Google 및 Hugging Face와 같은 다양한 AI 모델 제공업체를 지원하는 기능이 포함됩니다. 이는 채팅 및 이미지 생성과 같은 다양한 유형의 AI 모델을 지원합니다. Spring AI는 개발자가 많은 코드를 다시 작성하지 않고도 쉽게 서로 다른 구성 요소 간을 전환할 수 있도록 유연하게 설계되었습니다.

# Spring AI 아키텍처

여기 Spring AI의 아키텍처가 있습니다 (출처: 공식 문서)

![Spring AI Architecture](/assets/img/2024-07-20-IntegrateAIintoJavaAppsNoPythonRequired_1.png)

<div class="content-ad"></div>

지원되는 모델

OpenAI, Microsoft, Amazon, Google, Amazon Bedrock, Hugging Face 등을 포함합니다.

![이미지](/assets/img/2024-07-20-IntegrateAIintoJavaAppsNoPythonRequired_2.png)

Spring AI의 주요 기능 중 일부는 서로 일관된 인터페이스로 다양한 제공업체의 AI 모델을 사용할 수 있는 기능, AI 출력을 Java 객체로 변환하는 지원, 그리고 AI 사용을 위해 데이터를 관리하고 처리하는 도구가 있습니다. 또한 Spring Boot와 잘 작동하는 사전 제작된 구성 요소를 제공하여 Java 응용 프로그램을 구축하는 인기있는 프레임워크와 원활하게 작업할 수 있습니다. 이러한 기능을 통해 개발자는 회사 문서를 기반으로 질문에 답하거나 회사의 지식 베이스를 활용한 대화와 같은 작업을 보다 쉽게 수행할 수 있습니다.

<div class="content-ad"></div>

# 실하게 통합된 자바

Spring AI는 AI 기능을 자바 응용 프로그램에 손쉽게 통합하도록 설계되었습니다. Java를 사용하여 언어 모델과 연결하는 간단한 방법을 제공하여 기술 스택을 일관되게 유지하고 싶은 사람들에게 우수한 선택지가 됩니다. 이 라이브러리를 사용하면 기존 코드베이스를 다른 언어로 다시 작성할 필요없이 대화형 AI의 능력을 활용할 수 있습니다.

고객 문의 처리, 맞춤형 추천 제공 또는 루틴 작업 자동화와 같은 기능을 수행할 수 있는 기업 응용 프로그램에 대화형 인터페이스를 추가한다고 상상해보세요. Spring AI를 사용하면 이러한 기능을 빠르고 효율적으로 구현할 수 있습니다. 새로운 프로그래밍 언어의 복잡성을 걱정하지 않고 비즈니스 로직 및 사용자 경험을 개발하는 데 집중할 수 있습니다.

# 필수 사항

<div class="content-ad"></div>

시작하기 전에 다음 사항을 확인해주세요:

- Java Development Kit (JDK) 17 이상
- Maven
- OpenAI API 키

# 프로젝트 설정

필요한 종속성을 갖춘 새 Spring Boot 프로젝트를 설정하는 것부터 시작해봅시다.

<div class="content-ad"></div>

# pom.xml

먼저, pom.xml 파일을 설정합니다:

```js
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.3.0</version>
        <relativePath/> <!-- repository에서 parent 찾기 -->
    </parent>
    <groupId>com.donvitocodes</groupId>
    <artifactId>openai-java</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>OpenAI Java</name>
    <description>OpenAPI 서비스를 활용한 간단한 인공지능 애플리케이션</description>
    <properties>
        <java.version>17</java.version>
    </properties>
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.ai</groupId>
                <artifactId>spring-ai-bom</artifactId>
                <version>1.0.0-SNAPSHOT</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.ai</groupId>
            <artifactId>spring-ai-openai-spring-boot-starter</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
    <repositories>
        <repository>
            <id>spring-milestones</id>
            <name>Spring Milestones</name>
            <url>https://repo.spring.io/milestone</url>
            <snapshots>
                <enabled>false</enabled>
            </snapshots>
        </repository>
        <repository>
            <id>spring-snapshots</id>
            <name>Spring Snapshots</name>
            <url>https://repo.spring.io/snapshot</url>
            <releases>
                <enabled>false</enabled>
            </releases>
        </repository>
    </repositories>
</project>
```

이 설정은 Spring Boot 및 OpenAI와 작업하는 데 필요한 종속성으로 프로젝트를 설정합니다.

<div class="content-ad"></div>

# 응용 프로그램 구조

이제 주 응용 프로그램 클래스인 Application.Java를 생성해 보겠습니다.

```js
패키지 com.donvitocodes;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
``` 

# 구성 클래스

<div class="content-ad"></div>

다음으로, ChatClient를 설정하기 위한 구성 클래스 Config.java를 작성할 것입니다.

```java
package com.donvitocodes;

import org.springframework.ai.chat.client.ChatClient;
import org.springframework.ai.openai.OpenAiChatOptions;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
class Config {
    
    @Bean
    ChatClient chatClient(ChatClient.Builder builder) {
        String modelName = "gpt-4o-mini";
        float temperature = 0f;
        int maxTokens = 1024;
        return builder
                .defaultOptions(OpenAiChatOptions.builder()
                        .withModel(modelName)
                        .withTemperature(temperature)
                        .withMaxTokens(maxTokens)
                        .withStreamUsage(true)
                        .build())
                .build();
    }
}
```

이 구성은 ChatClient를 OpenAI 모델에 대한 사용자 정의 옵션과 함께 설정하며, 모델 이름, 온도 및 최대 토큰을 포함합니다.

# 컨트롤러 생성하기

<div class="content-ad"></div>

마지막으로 컨트롤러를 만들어 봅시다:

```js
package com.donvitocodes;

import org.springframework.ai.chat.client.ChatClient;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;
import reactor.core.publisher.Flux;

import java.util.Map;

@RestController
class AIController {
    private final ChatClient chatClient;
    AIController(ChatClient chatClient) {
        this.chatClient = chatClient;
    }
    @GetMapping("/chat")
    Map<String, String> completion(@RequestParam(value = "message", defaultValue = "") String message) {
        return Map.of(
                "completion",
                chatClient.prompt()
                        .user(message)
                        .call()
                        .content());
    }
    // Streaming 
    @GetMapping(value = "/chat-stream", produces = "application/stream+json")
    public Flux<String> streamCompletion(@RequestParam(value = "message", defaultValue = "") String message) {
        return  chatClient.prompt()
                .user(message)
                .stream()
                .content();
    }

}
```

이 컨트롤러는 메시지 매개변수를 받아들이고 AI가 생성한 응답을 반환하는 간단한 GET 엔드포인트를 노출합니다. 이 경우 응답은 스트리밍되지 않습니다.

# 채팅 응답을 스트리밍하기

<div class="content-ad"></div>

응답을 스트리밍하려면 코드의 해당 부분은 다음과 같습니다

```js
@GetMapping(value = "/chat-stream", produces = "application/stream+json")
public Flux<String> streamCompletion(@RequestParam(value = "message", defaultValue = "") String message) {
    return chatClient.prompt()
          .user(message)
          .stream()
          .content();
}
```

# 애플리케이션 실행하기

애플리케이션을 실행하려면 OpenAI API 키를 환경 변수로 설정했는지 확인하십시오.

<div class="content-ad"></div>

```js
export SPRING_AI_OPENAI_API_KEY=your_api_key_here
```

그런 다음 Maven을 사용하여 애플리케이션을 시작할 수 있습니다:

```js
mvn spring-boot:run
```

# API 테스트하기

<div class="content-ad"></div>

URL: http://localhost:8080/chat

Method: GET

Query Parameter:

- message (string): API에서 처리할 프롬프트나 지시문입니다. 예시: "부동산 투자에 대한 짧은 블로그 포스트 작성하기".

<div class="content-ad"></div>

한 번 응용프로그램이 실행 중이면 curl 또는 HTTP 클라이언트를 사용하여 API를 테스트할 수 있습니다:

```js
curl --location 'http://localhost:8080/chat?message=create%20a%20short%20blog%20post%20about%20investing%20in%20real%20estate'
```

또는 Postman을 사용하세요

![이미지](/assets/img/2024-07-20-IntegrateAIintoJavaAppsNoPythonRequired_3.png)

<div class="content-ad"></div>

# 스트리밍 테스트 중

curl을 실행할 때 --no-buffer 또는 -N을 사용해야 합니다. localhost:8080//chat-stream 엔드포인트를 호출해야 합니다.

```js
curl --no-buffer --location 'http://localhost:8080/chat-stream?message=create%20a%20short%20blog%20post%20about%20investing%20in%20real%20estate%20in%20the%20philippines'
```

API는 입력 메시지를 기반으로 AI가 생성한 응답을 반환합니다.

<div class="content-ad"></div>

블로그 글에서는 Spring AI, OpenAI 및 Java를 사용하여 채팅 API를 만드는 방법을 살펴보았어요. 이 통합은 Spring 생태계 내에서 AI 기반 대화형 응용 프로그램을 구축하는 강력한 기반을 제공해줘요. Spring AI의 추상화 레이어를 활용하면 미래에 다양한 AI 제공 업체나 모델 간에 쉽게 전환할 수 있어서 애플리케이션을 더 유연하고 유지보수하기 쉽게 만들어줄 수 있어요.

API 요청 제한을 처리하고 적절한 오류 처리를 구현하며, API를 프로덕션 환경에 배포하기 전에 인증을 추가하는 것을 고려하는 것을 잊지 마세요.

즐거운 코딩하세요!

Generative AI로 앱 개발에 대해 더 배우고 싶으시다면, 더 많은 튜토리얼 및 샘플 코드를 제공하는 제 블로그를 구독해주세요. Twitter에서 제를 팔로우하거나 LinkedIn에서 저와 연결해주세요.