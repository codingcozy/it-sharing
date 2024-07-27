---
title: "Google 폼 자동화하는 방법"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-07-27 13:53
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Automao para formulrios Google"
link: "https://dev.to/mirangod/automacao-para-formularios-google-3jie"
---


요청하신 대로 특정 항목을 업데이트하는 스마트한 양식을 만들라는데요. 사용자가 입력하는 내용에 따라 특정 아이템을 업데이트하는 것이 목적입니다. 일종의 ID처럼 말이죠.

AppsScript를 사용하여 다음과 같은 코드를 만들었습니다.

```js
function updateForms() {
    const id = "여기에 스프레드시트 ID를 입력하세요!";
    const sheetName = "자동 업로드할 항목이 있는 시트의 이름을 입력하세요!";

    const ss = SpreadsheetApp.openById(id);
    const sheet = ss.getSheetByName(sheetName);
    const range = sheet.getDataRange().getValues(); // 데이터를 설정할 시트를 권장합니다.

    const choiceValues = [...new Set(range.map(row => row[0]).filter(value => value))];

    const form = FormApp.openById("여기에 양식 ID를 입력하세요!");
    const items = form.getItems();

    for (var i in items) {
        if (items[i].getTitle() == "양식의 질문 이름을 여기에 입력하세요!") {
            items[i].asListItem().setChoiceValues(choiceValues);
            return;
        }
    }
    Logger.log("찾는 항목이 없습니다...");
}
```

이 코드는 매주 업데이트되는 보고서를 공급하기 위한 데이터를 업데이트하는 방식으로 만들어졌습니다.

<div class="content-ad"></div>

저장소에서 더 많은 정보를 찾을 수 있어요.

특정 항목을 업데이트하는 스마트 폼을 생성하라는 요청을 받았어요. 사용자가 입력한 내용에 따라 업데이트되는 아이템 같은 것이지요. 

AppsScript를 사용하여 다음과 같은 코드를 작성했어요:

```js
function updateForms() {
    const id = "여기에 스프레드시트 ID를 입력해 주세요!";
    const sheetName = "자동으로 업로드할 아이템을 포함하고 있는 시트 이름을 입력해 주세요!";

    const ss = SpreadsheetApp.openById(id);
    const sheet = ss.getSheetByName(sheetName);
    const range = sheet.getDataRange().getValues(); // 데이터가 업로드될 시트 설정을 권장해요

    const choiceValues = [...new Set(range.map(row => row[0]).filter(value => value))];

    const form = FormApp.openById("여기에 Forms ID를 입력해 주세요!");
    const items = form.getItems();

    for (var i in items) {
      if (items[i].getTitle() == "Forms의 질문 이름을 여기에 입력해 주세요!") {
        items[i].asListItem().setChoiceValues(choiceValues);
        return;
      }
    }
    Logger.log("아무 것도 찾지 못했습니다...");
}
```

<div class="content-ad"></div>

매주 보고서에 사용되는 데이터를 업데이트하는 코드를 찾았습니다.

저장소에서 더 많은 정보를 찾을 수 있습니다.