---
title: "Spiredoc을 사용한 Spring Boot 통합 및 Word 문서 조작 방법"
description: ""
coverImage: "/assets/img/2024-07-24-SpringBootintegrationSpiredocimplementmanipulationofword_0.png"
date: 2024-07-24 12:03
ogImage: 
  url: /assets/img/2024-07-24-SpringBootintegrationSpiredocimplementmanipulationofword_0.png
tag: Tech
originalTitle: "Spring Boot integration Spiredoc implement manipulation of word"
link: "https://medium.com/@jxausea/spring-boot-integration-spire-doc-implement-manipulation-of-word-afed9208065d"
---


# 1. 스파이어 독은 무엇인가요?

Spire.Doc for Java은 개발자가 자신의 Java 애플리케이션에서 Word 문서를 쉽게 변환하고 만들고 읽고 편집하고 변환하고 인쇄하는 등의 기능을 사용할 수 있는 전문적인 Java Word 구성 요소입니다. 완전히 독립적인 구성 요소인 Spire.Doc for Java는 Microsoft Office를 설치할 필요없이 실행됩니다. 동시에 대부분의 국내 운영 체제와 호환되며 키린(Kirin), 중케 방더(Zhongke Fangde) 등 국내 운영 체제에서 정상적으로 실행할 수 있습니다. Spire.Doc for Java는 WPS에서 생성된 Word 형식 문서(.wps, .wpt)를 지원합니다. Spire.Doc for Java는 다음과 같은 다양한 Word 문서 처리 작업을 수행할 수 있습니다: Word 문서 생성, 읽기, 변환, 인쇄, 그림 삽입, 헤더 및 푸터 추가, 표 작성, 양식 필드 및 메일 병합 도메인 추가, 책갈피 추가, 텍스트와 이미지 워터마크 추가, 배경 색 및 배경 이미지 설정, 각주 및 미주 추가, 하이퍼링크 추가, 디지털 서명, 워드 문서 암호화 및 해독, 주석 추가, 도형 추가 등이 있습니다.

# 2. 코드 엔지니어링

## 실험 목적

<div class="content-ad"></div>

- Word 파일 생성 구현
- 페이지 추가 기능 구현
- Word 내용 읽기

## pom.xml

```js
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>springboot-demo</artifactId>
        <groupId>com.et</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>
    <artifactId>spire-doc</artifactId>
    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-autoconfigure</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>e-iceblue</groupId>
            <artifactId>spire.doc</artifactId>
            <version>12.7.6</version>
        </dependency>
    </dependencies>
    <repositories>
        <repository>
            <id>com.e-iceblue</id>
            <name>e-iceblue</name>
            <url>https://repo.e-iceblue.com/nexus/content/groups/public/</url>
        </repository>
    </repositories>
</project>
```

## Word 파일 생성

<div class="content-ad"></div>

간단한 워드 문서를 만드는 Spire.Doc for Java를 사용하여 여러 단락이 포함된 워드 문서를 만드는 단계는 다음과 같습니다.

- Document 객체를 작성합니다.
- Document.addSection() 메서드를 사용하여 섹션을 추가합니다.
- Section.getPageSetup().setMargins() 메서드를 사용하여 페이지 여백을 설정합니다.
- Section.addParagraph() 메서드를 사용하여 섹션에 여러 단락을 추가합니다.
- Paragraph.appendText() 메서드를 사용하여 단락에 텍스트를 추가합니다.
- ParagraphStyle 객체를 작성하고 Paragraph.applyStyle() 메서드를 사용하여 각각의 단락에 적용합니다.
- Document.saveToFile() 메서드를 사용하여 문서를 워드 파일로 저장합니다.

```java
package com.et.spire.doc;
import com.spire.doc.Document;
import com.spire.doc.FileFormat;
import com.spire.doc.Section;
import com.spire.doc.documents.HorizontalAlignment;
import com.spire.doc.documents.Paragraph;
import com.spire.doc.documents.ParagraphStyle;
import java.awt.*;

public class CreateWordDocument {
    public static void main(String[] args) {
        // Document 객체 생성
        Document doc = new Document();

        // 섹션 추가
        Section section = doc.addSection();

        // 페이지 여백 설정
        section.getPageSetup().getMargins().setAll(40f);

        // 타이틀로 단락 추가
        Paragraph titleParagraph = section.addParagraph();
        titleParagraph.appendText("Spire.Doc for Java 소개");

        // 본문으로 두 개의 단락 추가
        Paragraph bodyParagraph_1 = section.addParagraph();
        bodyParagraph_1.appendText("Spire.Doc for Java는 Java 애플리케이션에 워드 문서를 생성, 변환, 조작, " +
                "인쇄하는 기능을 제공하는 전문적인 Word API로, Microsoft Word에 의존하지 않고 작동합니다.");

        Paragraph bodyParagraph_2 = section.addParagraph();
        bodyParagraph_2.appendText("이 다기능 라이브러리를 사용하면 이미지 삽입, 하이퍼링크, 디지털 서명, " +
                "책갈피 및 워터마크 설정, 헤더 및 풋터 설정, 표 작성, 배경 이미지 설정, 각주 및 미주 추가 등의 " +
                "작업을 쉽게 처리할 수 있습니다.");

        // 타이틀 단락에 스타일 생성 및 적용
        ParagraphStyle style1 = new ParagraphStyle(doc);
        style1.setName("titleStyle");
        style1.getCharacterFormat().setBold(true);
        style1.getCharacterFormat().setTextColor(Color.BLUE);
        style1.getCharacterFormat().setFontName("Times New Roman");
        style1.getCharacterFormat().setFontSize(12f);
        doc.getStyles().add(style1);
        titleParagraph.applyStyle("titleStyle");

        // 본문 단락에 스타일 생성 및 적용
        ParagraphStyle style2 = new ParagraphStyle(doc);
        style2.setName("paraStyle");
        style2.getCharacterFormat().setFontName("Times New Roman");
        style2.getCharacterFormat().setFontSize(12);
        doc.getStyles().add(style2);
        bodyParagraph_1.applyStyle("paraStyle");
        bodyParagraph_2.applyStyle("paraStyle");

        // 단락의 가로 정렬 설정
        titleParagraph.getFormat().setHorizontalAlignment(HorizontalAlignment.Center);
        bodyParagraph_1.getFormat().setHorizontalAlignment(HorizontalAlignment.Justify);
        bodyParagraph_2.getFormat().setHorizontalAlignment(HorizontalAlignment.Justify);

        // 첫 줄 들여쓰기 설정
        bodyParagraph_1.getFormat().setFirstLineIndent(30);
        bodyParagraph_2.getFormat().setFirstLineIndent(30);

        // 후방 여백 설정
        titleParagraph.getFormat().setAfterSpacing(10);
        bodyParagraph_1.getFormat().setAfterSpacing(10);

        // 파일로 저장
        doc.saveToFile("/Users/liuhaihua/tmp/WordDocument.docx", FileFormat.Docx_2013);
        doc.close();
    }
}
```

새 페이지를 추가하려면 위 코드를 사용하세요.

<div class="content-ad"></div>

워드 문서의 끝에 새로운 페이지를 추가하는 단계는 마지막 섹션을 찾은 다음 해당 섹션의 마지막 단락에 페이지 나누기를 삽입하는 것입니다. 이렇게 하면 이후 추가된 내용이 새 페이지에서 표시되어 문서 구조의 명확성과 일관성을 유지합니다. 상세한 절차는 다음과 같습니다:

- Document 객체를 생성합니다.
- Document.loadFromFile() 메서드를 사용하여 워드 문서를 로드합니다.
- Document.getLastSection().getBody()를 사용하여 문서의 마지막 섹션의 본문을 가져옵니다.
- Paragraph.appendBreak(BreakType.Page_Break) 메서드를 호출하여 페이지 나누기를 추가합니다.
- 새로운 단락 스타일 ParagraphStyle 객체를 생성합니다.
- Document.getStyles().add(paragraphStyle) 메서드를 사용하여 문서 스타일 컬렉션에 새 단락 스타일을 추가합니다.
- 새로운 단락 Paragraph 객체를 생성하고 텍스트 내용을 설정합니다.
- Paragraph.applyStyle(paragraphStyle.getName()) 메서드를 사용하여 이전에 생성한 단락 스타일을 새 단락에 적용합니다.
- Body.getChildObjects().add(paragraph) 메서드를 사용하여 새 단락을 문서에 추가합니다.
- Document.saveToFile() 메서드를 사용하여 결과 문서를 저장합니다.

마크다운 형식의 테이블 태그를 변경하면 코드를 수정할 수 있습니다.

<div class="content-ad"></div>

FixedLayoutDocument 클래스와 FixedLayoutPage 클래스를 사용하면 특정 페이지에서 콘텐츠를 추출하기가 쉬워집니다. 추출된 콘텐츠를 볼 수 있도록 다음 예제 코드는 추출된 콘텐츠를 새 Word 문서로 저장합니다. 자세한 단계는 다음과 같습니다:

- Document 개체를 생성합니다.
- Document.loadFromFile() 메서드를 사용하여 Word 문서를 로드합니다.
- FixedLayoutDocument 개체를 생성합니다.
- 문서에서 페이지를 나타내는 FixedLayoutPage 개체를 얻습니다.
- FixedLayoutPage.getSection() 메서드를 사용하여 페이지가 있는 섹션을 가져옵니다.
- 해당 섹션 내에서 페이지의 첫 번째 단락의 인덱스 위치를 가져옵니다.
- 해당 섹션 내에서 페이지의 마지막 단락의 인덱스 위치를 가져옵니다.
- 또 다른 Document 개체를 생성합니다.
- Document.addSection()을 사용하여 새 섹션을 추가합니다.
- Section.cloneSectionPropertiesTo(newSection) 메서드를 사용하여 원본 섹션의 속성을 새 섹션으로 복제합니다.
- 페이지의 콘텐츠를 원본 문서에서 새 문서로 복사합니다.
- Document.saveToFile() 메서드를 사용하여 결과 문서를 저장합니다.

위의 코드는 주요 부분만을 담고 있으며, 전체 코드는 아래 저장소에서 찾을 수 있습니다.

<div class="content-ad"></div>

## 코드 저장소

- [Spring Boot 데모 코드 저장소](https://github.com/Harries/springboot-demo)（스파이어-doc）

## 3. 테스트

위의 Java 클래스의 main 메서드를 실행하면 해당 디렉토리에 3개의 파일이 생성됩니다.

<div class="content-ad"></div>

![이미지](/assets/img/2024-07-24-SpringBootintegrationSpiredocimplementmanipulationofword_0.png)

## 4. 참고 자료

- Spire.Doc for Java에서 Word 문서 작성하기: [링크](https://www.e-iceblue.com/Tutorials/Java/Spire.Doc-for-Java/Program-Guide/Document-Operation/Create-Word-Document-in-Java.html)
- Java에서 Word 문서 만들기: [링크](http://www.liuhaihua.cn/archives/710952.html)