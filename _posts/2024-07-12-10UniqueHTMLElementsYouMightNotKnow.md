---
title: "잘 알려지지 않은 독특한 HTML 요소 10가지"
description: ""
coverImage: "/it-shar.ing/assets/no-image.jpg"
date: 2024-07-12 18:22
ogImage:
  url: /it-shar.ing/assets/no-image.jpg
tag: Tech
originalTitle: "10 Unique HTML Elements You Might Not Know"
link: "https://dev.to/futuristicgeeks/10-unique-html-elements-you-might-not-know-3djl"
---

웹 개발의 세계는 익숙한 `div`, `p`, `img` 태그를 넘어서 더욱 다양합니다. HTML은 창작물을 높여주고 사용자 경험을 더욱 흥미롭게 만들어줄 수 있는 잊혀진 요소들의 보물창고를 제공합니다. 여기에서는 아직 경험하지 않았을 수도 있는 10가지 독특한 HTML 요소에 대해 살펴보겠습니다. 이를 통해 그들의 잠재력을 보다 명확히 이해할 수 있습니다:

## 1. 반응형 이미지 처리 및 :

어떤 디바이스에도 완벽한 이미지 크기를 자동으로 제공하는 웹사이트를 상상해보세요. 그것이 바로 와 가 하는 마법입니다. 작동 방식을 알아보겠습니다:

picture
source srcset="image-large.jpg" media="(min-width: 768px)"
source srcset="image-medium.jpg" media="(min-width: 480px)"
img src="image-small.jpg" alt="아름다운 풍경"
/picture

<div class="content-ad"></div>

이 코드는 데스크톱용으로 큰 이미지, 태블릿용으로 중간 이미지 및 작은 기본 이미지의 세 가지 이미지 옵션을 정의합니다. 브라우저는 사용자의 화면 크기를 기준으로 가장 적합한 이미지를 선택하여 로딩 시간을 단축하고 원활한 경험을 제공합니다.

## 2. 확장 가능한 아코디언과 :

요구에 따라 더 많은 정보를 드러내는 확장 가능한 섹션을 만들고 싶나요? 다이나믹한 이 중요한 요소를 `details`와 `summary`태그의 동적 컴비네이션으로 확인해보세요.

details
summary 클릭해서 저희에 대해 더 배워보세요 summary
p 우리는 우수한 웹 경험을 만드는 데 헌신하는 열정적인 팀입니다. p
details

<div class="content-ad"></div>

이 코드는 접는 섹션을 만듭니다. "Click to learn more about us" 요약을 클릭하면 단락 콘텐츠의 가시성이 토글되어 정보 흐름을 제어하고 페이지를 정리할 수 있습니다.

### 3. Annotated Text with `<mark>` Tag:

중요한 변경 또는 수정 사항을 강조하는 것이 쉬워집니다. `<mark>` 요소는 특정 텍스트에 주목을 끕니다:

우리는 새로운 웹사이트를 발표하는 것을 기쁘게 생각합니다!

<div class="content-ad"></div>

반면에, 삭제된 콘텐츠를 나타내는 것으로 일반적으로 취소선 효과와 함께 표시됩니다:

우리 이전 웹사이트는 `del`www.oldwebsite.com`/del`에 있었습니다.

## 4. 진행 상황 시각화하기 :

정보를 제공하며 계속되는 로딩 바를 만난 적 있나요? 그것이 작동 중인 요소입니다.

<div class="content-ad"></div>

`progress value="50" max="100"`50% complete`/progress`

이 코드는 반 나온 (값이 50인) 진행 표시 막대를 보여줍니다. 최대 값은 100입니다. CSS를 사용하여 외관을 스타일링하여 정보를 제공하고 시각적으로 매력적인 진행 표시기를 만들 수 있습니다.

## 5. 게이지 측정 :

와 유사하게 , 요소는 정의된 범위 내의 값을 나타냅니다. 저장 공간 사용량을 표시하는 게이지를 상상해보세요:

<div class="content-ad"></div>

아래는 Markdown 형식으로 변환된 코드입니다.

meter value="70" min="0" max="100" 70% full /meter

이 코드는 70%로 채워진 게이지를 표시합니다 (값은 70으로 설정되어 있음). 최솟값은 0이고 최댓값은 100입니다. CSS를 사용하여 게이지의 모습을 자유롭게 사용자 정의할 수 있어서 다양한 스칼라 측정을 효과적으로 표현할 수 있습니다.

## 6. 용도에 따른 시멘틱한 날짜와 시간:

단순히 날짜와 시간을 표시하는 것 이상으로 나아가세요. : 요소는 검색 엔진과 보조 기술이 콘텐츠의 시간적 성격을 이해하는데 유용한 시멘틱한 방법을 제공합니다.

<div class="content-ad"></div>

이 문서는 `time datetime="2024-07-10"`2024년 7월 10일`/time`에 마지막으로 업데이트되었습니다.

이 코드는 날짜를 표시하는데 그치지 않고 datetime 속성을 사용하여 기계가 읽을 수 있는 정보를 포함하여 접근성과 SEO를 개선합니다.

## 7. 적절한 텍스트 줄 바꿈을 위한 :

여러 화면 크기에서 적절한 텍스트 줄 바꿈을 보장하는 것은 까다로울 수 있습니다. 요소는 섬세한 해결책을 제공합니다. 이 요소는 줄 바꿈을 강요하지 않지만 해당 지점에서 필요하다면 단어를 분리할 수 있다고 브라우저에 제안합니다.

<div class="content-ad"></div>

이 URL은 화면이 좁을 경우에는 오버플로우될 수 있습니다. `wbr`을 추가하여 더 나은 줄 바꿈을 할 수 있도록 하겠습니다: http://www.verylongwebsitewithalongeverylongname.com

이 코드는 긴 URL 내에서 줄 바꿈 지점을 제안하여 브라우저가 작은 화면에서 텍스트를 더 우아하게 줄 바꿈할 수 있도록 합니다.

## 8. 재사용 가능한 내용 만들기 :

코드를 중복하지 않고 블로그 게시물이나 제품 목록을 위한 표준 레이아웃을 생성해보세요. 이 요소는 이를 가능하게 합니다.

<div class="content-ad"></div>

## 'title'

'content'

```javascript
const template = document.getElementById("postTemplate");
const title = "A New Blog Post";
const content = "This is some exciting content for the new blog post!";
```

```javascript
// Clone the template and populate its content
const newPost = template.content.cloneNode(true);
newPost.querySelector("h2").textContent = title;
newPost.querySelector("p").textContent = content;
```

```javascript
// Append the populated template to the document body
document.body.appendChild(newPost);
```

<div class="content-ad"></div>

이 코드는 제목과 내용을 나타내는 자리 표시자가 있는 템플릿을 정의합니다. 그런 다음 JavaScript는 템플릿을 복제하고 자동 데이터로 자리 표시자를 채워서 새 게시물을 웹페이지에 삽입합니다.

## 9. Declarative Popups with Markdown :

모달 창을 위한 복잡한 JavaScript를 잊으세요. 이 요소는 선언적인 접근 방식을 제공합니다.

<div class="content-ad"></div>

## 정말 확실한가요?

이 작업은 되돌릴 수 없습니다.

확인`

이 코드는 콘텐츠와 버튼이 있는 모달 대화상자를 정의합니다. "확인" 버튼을 클릭하면 JavaScript의 showModal() 메소드를 사용하여 대화상자가 열립니다. CSS를 사용하여 대화상자의 모양을 스타일링하여 매끄러운 사용자 경험을 제공할 수 있습니다.

<div class="content-ad"></div>

## 10. 향상된 양식 작성:

양식 작성을 간편하게 만들어 주는 자동 완성 제안을 제공하여 양식 작성을 가속화합니다.

`label for="color"`색상 선택:`/label`
`input list="colorOptions" id="color" name="color"`
`datalist id="colorOptions"`
`option value="Red"`
`option value="Green"`
`option value="Blue"`
`/datalist`

FuturisticGeeks에서 더 많은 정보를 확인하세요.
