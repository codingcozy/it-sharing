---
title: "묘지 사업을 위한 데이터 입력 간소화 방법"
description: ""
coverImage: "/assets/img/2024-07-24-StreamliningDataEntryforaCemeteryBusiness_0.png"
date: 2024-07-24 12:16
ogImage: 
  url: /assets/img/2024-07-24-StreamliningDataEntryforaCemeteryBusiness_0.png
tag: Tech
originalTitle: "Streamlining Data Entry for a Cemetery Business"
link: "https://medium.com/@dennisyd/streamlining-data-entry-for-a-cemetery-business-e1e2758b2322"
---


Markdown 형식으로 변경해 드릴게요.

![이미지](/assets/img/2024-07-24-StreamliningDataEntryforaCemeteryBusiness_0.png)

# 사례 연구:

고객 배경

묘지 사업에서 장례지 구매를 담당하는 영업팀이 수동으로 PDF 양식을 작성해야 하는 지루한 과정을 겪고 있었습니다. 이 과정은 시간이 오래 걸리는 것뿐만 아니라 실수가 발생하기 쉬워 운영의 비효율성을 야기했습니다.

<div class="content-ad"></div>

도전

고객은 영업팀이 수집한 데이터로 PDF 양식을 자동으로 채우는 솔루션이 필요했습니다. 목표는 데이터 입력 프로세스를 가속화하고 정확성을 보장하며 필요할 때 양식을 감사하고 재생성하기 쉽게하는 것이었습니다.

해결책

YDD Consulting은 Python 라이브러리 pdfforms를 사용한 혁신적인 솔루션을 제공했습니다. 이 라이브러리를 사용하면 PDF 양식 필드 데이터의 추출과 조작이 가능합니다. pdfforms를 Excel과 통합함으로써 데이터 입력 프로세스가 크게 간소화되었습니다.

<div class="content-ad"></div>

# 구현

- Excel 통합:

  - Excel 시트를 만들어 드롭다운 및 다른 입력 기능을 사용하여 데이터를 빠르게 입력할 수 있도록 했습니다.
  - 이 데이터를 사용하여 PDF 양식을 자동으로 채웁니다.

- 자동화 프로세스:

<div class="content-ad"></div>

- openpyxl 라이브러리를 사용하여 Excel 시트에서 데이터를 읽었습니다.
- pdfforms 라이브러리를 활용하여 PDF 양식을 열고, Excel 시트의 데이터로 양식 필드를 채우고, 그런 다음 양식을 플랫하게 만들어 필드를 편집할 수 없도록 했습니다.
- 완성된 양식은 새로운 PDF로 저장되었습니다.

# 기술적 방법

- 사용된 라이브러리:

- pdfforms: PDF 양식을 다루는 데 사용되며, 필드를 채우고 플랫하게 만드는 등의 기능이 있습니다.
- openpyxl: Excel 스프레드시트에서 데이터를 읽는 데 사용됩니다.

<div class="content-ad"></div>

아래와 같이 Markdown 포맷으로 표태그를 변경해주세요.

| 코드 예시 |

```js
import pdfforms
import openpyxl

// Excel 스프레드시트 열기
workbook = openpyxl.load_workbook("data.xlsx")
sheet = workbook.active

// 스프레드시트의 첫 번째 행에서 데이터 읽기
name = sheet.cell(row=1, column=1).value
address = sheet.cell(row=1, column=2).value
ssn = sheet.cell(row=1, column=3).value

// PDF 양식 열기
form = pdfforms.open("form.pdf")

// 양식 필드 작성
form.fields["name"] = name
form.fields["address"] = address
form.fields["ssn"] = ssn

// 양식 평면화 (필드를 편집할 수 없게 만듦)
form.flatten()

// 작성된 양식 저장
form.save("filled_form.pdf")

// 양식 닫기
form.close()
```

결과:

YDD Consulting에 의한 이 솔루션의 구현은 상당한 이점을 가져왔습니다.

<div class="content-ad"></div>

효율성:

- PDF 양식을 작성하는 수동 프로세스가 수 시간에서 몇 분으로 줄었으며, 판매 팀이 고객 상호작용에 집중할 수 있는 소중한 시간이 확보되었습니다.

정확성:

- 데이터 입력 프로세스를 자동화하여 오류를 최소화하여 모든 정보가 정확하게 기록되고 PDF 양식으로 전송되었습니다.

<div class="content-ad"></div>

감사 가능성:

- 필요할 때 신속하게 수집된 정보를 감사하고 PDF 양식을 재생성할 수 있는 능력은 비즈니스의 전반적인 운영 효율성을 높였습니다.

결론

YDD 컨설팅의 PDF 양식 작성을 위한 자동화 솔루션이 묘원 비즈니스의 데이터 입력 프로세스를 혁신적으로 변화시켰습니다. Python 라이브러리를 활용하여 Excel과 PDF 양식을 통합함으로써, 비즈니스는 더 효율적이고 정확하며 감사 가능한 결과를 얻었습니다. 이 사례 연구는 복잡한 프로세스를 간소화하고 운영 효율성을 향상시키는 자동화가 가치 있는 지를 보여 줍니다.