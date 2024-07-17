---
title: "파일 압축하는 방법 2024 최신"
description: ""
coverImage: "/assets/img/2024-07-12-HowtoCompressYourFiles_0.png"
date: 2024-07-12 18:36
ogImage:
  url: /assets/img/2024-07-12-HowtoCompressYourFiles_0.png
tag: Tech
originalTitle: "How to Compress Your Files"
link: "https://medium.com/@yasin.deger/how-to-compress-your-files-3435f9b3c7dc"
---

테스트 자동화 프로젝트에서 생성된 테스트 보고서를 저장하고 공유하는 것은 중요한 측면입니다. 특히 보고서가 크거나 여러 파일이 포함되어 있는 경우에는 이러한 보고서를 압축하기 위해 zip하는 것이 유익합니다. 이 글에서는 Java를 사용하여 테스트 보고서를 zip하는 방법을 설명하겠습니다. 그리고 다음 글에서는 해당 zip 파일을 훅 클래스에서 이메일을 통해 전송하는 방법을 공유할 것입니다.

![이미지](/assets/img/2024-07-12-HowtoCompressYourFiles_0.png)

# ZipUtil 클래스

먼저, 테스트 보고서를 zip하기 위한 유틸리티 클래스가 필요합니다. 아래는 ZipUtil 클래스의 코드입니다. 이 클래스를 사용하면 지정된 디렉토리를 zip할 수 있습니다. 여기서는 예시 allure 보고서 파일을 zip하는 방법을 보여드리겠습니다.

<div class="content-ad"></div>

```java
package com.yasindeger.utilities.emailHelper;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.util.zip.ZipEntry;
import java.util.zip.ZipOutputStream;

public class ZipUtil {
    public static void zipDirectory(File dir, String zipDirName) {
        try {
            FileOutputStream fos = new FileOutputStream(zipDirName);
            ZipOutputStream zos = new ZipOutputStream(fos);

            zipFile(dir, dir.getName(), zos);
            zos.close();
            fos.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    private static void zipFile(File fileToZip, String fileName, ZipOutputStream zos) throws IOException {
        if (fileToZip.isHidden()) {
            return;
        }
        if (fileToZip.isDirectory()) {
            if (fileName.endsWith("/")) {
                zos.putNextEntry(new ZipEntry(fileName));
                zos.closeEntry();
            } else {
                zos.putNextEntry(new ZipEntry(fileName + "/"));
                zos.closeEntry();
            }
            File[] children = fileToZip.listFiles();
            for (File childFile : children) {
                zipFile(childFile, fileName + "/" + childFile.getName(), zos);
            }
            return;
        }
        FileInputStream fis = new FileInputStream(fileToZip);
        ZipEntry zipEntry = new ZipEntry(fileName);
        zos.putNextEntry(zipEntry);
        byte[] bytes = new byte[1024];
        int length;
        while ((length = fis.read(bytes)) >= 0) {
            zos.write(bytes, 0, length);
        }
        fis.close();
    }

    public static void main(String[] args) {
        File dir = new File("target/allure-report");
        String zipDirName = "target/allure-report.zip";
        zipDirectory(dir, zipDirName);
    }
}
```

## 설명

### zipDirectory 메서드

이 메서드는 지정된 디렉토리를 압축하는 데 FileOutputStream과 ZipOutputStream를 사용합니다. 디렉토리 내의 모든 파일 및 하위 디렉토리를 압축하기 위해 zipFile 메서드를 호출합니다.

<div class="content-ad"></div>

## zipFile 메소드

이 메소드는 지정된 파일 또는 디렉토리를 압축합니다. 파일이 숨겨져 있으면 건너뛰며, 디렉토리인 경우 하위 디렉토리와 파일도 함께 압축됩니다.

## main 메소드

이 main 메소드는 예제 사용 시나리오를 보여줍니다. 여기서 target/allure-report 디렉토리가 압축되어 target/allure-report.zip으로 저장됩니다.

<div class="content-ad"></div>

# 다음 단계

이 글에서는 Java를 사용하여 테스트 보고서를 압축하는 유틸리티 클래스를 생성했습니다. 다음 글에서는 해당 zip 파일을 훅스(hooks)에서 이메일로 보내는 방법을 설명하겠습니다. 이렇게 하면 테스트 결과를 자동으로 관련 있는 사람들에게 보낼 수 있습니다.

계속 주목해 주세요!
