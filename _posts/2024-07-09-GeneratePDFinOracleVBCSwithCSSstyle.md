---
title: "Oracle VBCS에서 CSS 스타일을 사용하여 PDF 생성하는 방법"
description: ""
coverImage: "/assets/img/2024-07-09-GeneratePDFinOracleVBCSwithCSSstyle_0.png"
date: 2024-07-09 08:22
ogImage:
  url: /assets/img/2024-07-09-GeneratePDFinOracleVBCSwithCSSstyle_0.png
tag: Tech
originalTitle: "Generate PDF in Oracle VBCS with CSS style"
link: "https://medium.com/@nadabashar6/generate-pdf-in-oracle-vbcs-with-css-style-897024fcf9d6"
---

<img src="/assets/img/2024-07-09-GeneratePDFinOracleVBCSwithCSSstyle_0.png" />

이 블로그에서는 Oracle VBCS에서 Custom Oracle JET Web Component, pdfmake 및 html-to-pdfmake 외부 JS 라이브러리를 사용하여 CSS 스타일로 PDF 파일을 생성하는 단계를 살펴볼 것입니다.

<img src="/assets/img/2024-07-09-GeneratePDFinOracleVBCSwithCSSstyle_1.png" />

먼저 사용할 사용자 정의 Oracle JET Web Component에 대해 이야기해보겠습니다. 해당 컴포넌트는 풍부한 텍스트를 제공하며 아래 링크에서 찾을 수 있습니다:

<div class="content-ad"></div>

이 컴포넌트를 사용하면 글꼴 크기/색상을 변경하고 텍스트를 굵게 표시하거나 기울임꼴로 만들 수 있을 뿐만 아니라 순서대로 또는 정렬되지 않은 목록을 만들 수 있습니다.

![이미지 1](/assets/img/2024-07-09-GeneratePDFinOracleVBCSwithCSSstyle_2.png)

VBCS의 기능을 사용하여 사용자 정의 컴포넌트를 사용하면 애플리케이션에 리치 텍스트 컴포넌트를 가져온 후 html에 추가할 수 있습니다.

![이미지 2](/assets/img/2024-07-09-GeneratePDFinOracleVBCSwithCSSstyle_3.png)

<div class="content-ad"></div>

```js
<div class="oj-flex">
  <rich-text-editor
    id="richText"
    class="oj-flex-item oj-sm-12 oj-md-12"
    style="background: white;"
    toolbar-options='["text-sizes","bold","color","order","bullet","italic"]'
    content=""
  ></rich-text-editor>
</div>
```

이 구성 요소 내부의 텍스트를 가져오려면 아래 자바스크립트 함수가 필요합니다.

```js
// RichText 값 가져오기
PageModule.prototype.getRichTextValue = function () {
  let innerHtm = $("#richText")[0].getUpdatedHTML();

  return innerHtm;
};
```

이제 pdfmake 라이브러리 파일을 가져와야 합니다. 아래 링크에서 두 파일을 다운로드할 수 있습니다:

<div class="content-ad"></div>

vfs_fonts.js는 라이브러리에서 사용할 다양한 폰트를 로드하는 데 사용되고, pdfmake.js는 라이브러리 자체를 사용하는 데 사용됩니다.

그래서, 이제 VBCS 어플리케이션에서 2개 파일을 가져온 후, 웹 어플리케이션의 html 페이지에 다음 3줄을 추가해야 합니다.

```js
<script src="resources/js/pdfmake.js"></script>
<script src="resources/js/vfs_fonts.js"></script>
<script src="https://cdn.jsdelivr.net/npm/html-to-pdfmake/browser.js"></script>
```

이제 남은 것은 PDF를 생성하는 우리 자바스크립트 함수입니다.

<div class="content-ad"></div>

```js
function getFontSize(richText) {
  if (richText.style && richText.style.filter((word) => word == "ql-size-small").length > 0) {
    richText.fontSize = 10;
  } else if (richText.style && richText.style.filter((word) => word == "ql-size-large").length > 0) {
    richText.fontSize = 20;
  } else if (richText.style && richText.style.filter((word) => word == "ql-size-huge").length > 0) {
    richText.fontSize = 30;
  }
  return richText;
}

function handleRichTextOL(richText) {
  for (let y = 0; y < richText.length; y++) {
    if (richText.ol[y].text && typeof richText.ol[y].text == "object") {
      if (richText.ol[y].text[0].text && typeof richText.ol[y].text[0].text == "string") {
        richText.ol[y].text[0] = getFontSize(richText.ol[y].text[0]);
      } else if (richText.ol[y].text[0].text && typeof richText.ol[y].text[0].text == "object") {
        richText.ol[y].text[0].text[0] = getFontSize(richText.ol[y].text[0].text[0]);
      }
    }
  }

  return richText;
}

function handleRichTextUL(richText) {
  for (let y = 0; y < richText.ul.length; y++) {
    if (richText.ul[y].text && typeof richText.ul[y].text == "object") {
      if (richText.ul[y].text[0].text && typeof richText.ul[y].text[0].text == "string") {
        richText.ul[y].text[0] = getFontSize(richText.ul[y].text[0]);
      } else if (richText.ul[y].text[0].text && typeof richText.ul[y].text[0].text == "object") {
        richText.ul[y].text[0].text[0] = getFontSize(richText.ul[y].text[0].text[0]);
      }
    }
  }
  return richText;
}

function handleRichTextEn(innerHtm) {
  let richText = htmlToPdfmake(innerHtm);
  let tableContent = [];

  for (let x = 0; x < richText.length; x++) {
    richText[x].font = "Amiri";

    richText[x].alignment = "left";

    if (richText[x].text && typeof richText[x].text == "object") {
      if (richText[x].text[0].text && typeof richText[x].text[0].text == "string") {
        richText[x].text[0] = getFontSize(richText[x].text[0]);
      } else if (richText[x].text[0].text && typeof richText[x].text[0].text == "object") {
        richText[x].text[0].text[0] = getFontSize(richText[x].text[0].text[0]);
      }
    } else {
      if (richText[x].ol) {
        richText[x] = handleRichTextOL(richText[x]);
      } else if (richText[x].ul) {
        richText[x] = handleRichTextUL(richText[x]);
      }
    }

    tableContent.push(richText[x]);
  }

  return richText;
}

PageModule.prototype.generateRichPDF = function (logoBase64, innerHtm) {
  let pdfContent = handleRichTextEn(innerHtm);

  //Define the fonts
  pdfMake.fonts = {
    Amiri: {
      normal: "Amiri-Regular.ttf",
      bold: "Amiri-Bold.ttf",
      italics: "Amiri-Italic.ttf",
      bolditalics: "Amiri-BoldItalic.ttf",
      // Add other styles (bold, italics, etc.) if needed
    },
  };

  let dd = {
    pageMargins: [60, 80, 60, 80], // [left, top, right, bottom]
    header: function (currentPage, pageCount) {
      return {
        columns: [
          {
            image: logoBase64,
            width: 70, // Adjust the size as needed
            alignment: "left",
            absolutePosition: { x: 30, y: 10 }, // Position the header outside the margins
          },
        ],
      };
    },
    footer: function (currentPage, pageCount) {
      return {
        text: currentPage.toString() + " / " + pageCount,
        font: "Amiri",
        alignment: "center",
      };
    },
    content: [
      {
        text: "Report",
        font: "Amiri",
        color: "green",
        alignment: "center",
        fontSize: 16,
        //margin: [0, 10, 0, 0] // Add margin above the text
      },
      pdfContent,
    ],
    styles: {
      header: {
        fontSize: 13,
        font: "Amiri",
        alignment: "center",
      },
      subheader: {
        fontSize: 12,
        font: "Amiri",
        alignment: "center",
      },
    },
  };

  pdfMake.createPdf(dd).download("Report.pdf");
};
```

PDF를 생성하는 버튼은 먼저 언급된 복잡한 텍스트의 innerHTML을 가져오는 함수를 호출한 다음 가져온 값을 generateRichPDF 함수에 전달합니다.

generateRichPDF 함수에서 먼저 handleRichTextEn 함수를 호출하여 복잡한 텍스트로부터 가져온 HTML을 pdfmake 라이브러리가 이해할 수 있는 형식으로 변환합니다. "Amiri" 글꼴을 사용하고 글꼴 크기를 조정해야 하기 때문에 결과를 수정하고 각 항목에 getFontSize 함수를 호출하여 선택한 크기를 지정하였습니다.

또한 결과에 OL 또는 UL이 포함되어 있는지 확인하여 객체를 깊게 탐색하고 위의 사용자 정의를 적용해야 했습니다.

<div class="content-ad"></div>

마지막으로 해야 할 일은 pdfmake 라이브러리에서 "Amiri" 글꼴을 사용하도록 정의하는 것입니다. 일반, 볼드, 이탤릭 등의 경우에 모두 해당 글꼴을 사용하도록 설정하고, 그런 다음 모든 pdf 내용을 포함하는 최종 객체를 생성하고 pdf를 다운로드하는 것입니다.

필요한 모든 링크:

[http://pdfmake.org/#/](http://pdfmake.org/#/)

[https://github.com/Aymkdn/html-to-pdfmake](https://github.com/Aymkdn/html-to-pdfmake)

<div class="content-ad"></div>

위 링크를 방문해주셔서 감사합니다! 유용한 정보를 얻으셨기를 바랍니다.
