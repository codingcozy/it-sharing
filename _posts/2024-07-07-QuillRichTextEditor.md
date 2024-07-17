---
title: "Quill 리치 텍스트 에디터 사용 가이드"
description: ""
coverImage: "/assets/img/2024-07-07-QuillRichTextEditor_0.png"
date: 2024-07-07 20:58
ogImage:
  url: /assets/img/2024-07-07-QuillRichTextEditor_0.png
tag: Tech
originalTitle: "Quill Rich Text Editor"
link: "https://medium.com/@sinera2000/quill-rich-text-editor-cd4d3a0ec496"
---

![Quill Rich Text Editor](/assets/img/2024-07-07-QuillRichTextEditor_0.png)

콘텐츠 작성은 웹 애플리케이션의 기본적인 요소입니다. `textarea` 요소는 대부분의 웹 애플리케이션에 대해 기본적이고 필수적인 해결책을 제공하지만, 일부 특정한 사용 사례에서는 텍스트 입력의 서식 지정 및 사용자 정의에 제약이 있습니다. 이때 리치 텍스트 편집기가 필수적입니다. 다양한 옵션 중에서 현대적인 기능과 접근 방식으로 빛을 발하는 Quill이 있습니다.

Quill은 현대 웹을 위해 설계된 무료 오픈 소스 리치 텍스트 편집기입니다. 모듈식 아키텍처와 표현력 있는 API를 통해 어떤 요구 사항에도 완벽히 맞춤화할 수 있습니다.

본 문서에서는 Next.js 애플리케이션에 Quill을 설치하는 방법을 설명합니다.

<div class="content-ad"></div>

# Quill을 시도해 봅시다

⦿ Next.js 어플리케이션 만들기

```js
npx create next-app@latest text-editor-quill
cd text-editor-quill
```

⦿ 다음 패키지들이 필요합니다. 설치해 주세요.

<div class="content-ad"></div>

```js
npm install react-quill
install html-react-parser
npm i quill
```

⦿ src 디렉토리 안에 "TextEditor.tsx" 파일을 생성해주세요. 아래 코드 스니펫을 붙여넣어주세요.

```js
"use client";
import parse from "html-react-parser"; // Imports the html-react-parser library to safely parse HTML
import React, { useState } from "react"; // Imports React and the useState hook
import ReactQuill from "react-quill"; // Imports ReactQuill, the Quill editor component for React
import "react-quill/dist/quill.snow.css"; // Imports the Quill editor's default "snow" theme CSS

// The main TextEditor component
const TextEditor = () => {
  // State for the content of the text editor
  const [body, setBody] = useState("");
  // State for the submitted content
  const [submittedContent, setSubmittedContent] = useState("");

  // Handler to update the body state when the editor content changes
  const handleBody = (e) => {
    setBody(e);
  };

  // Handler for when the content is submitted
  const handleSubmit = () => {
    // Sets the submitted content to the current body content
    setSubmittedContent(body);
    // Logs the current body content to the console
    console.log(body);
    // Logs the parsed HTML content to the console
    console.log(parse(body));
  };

  // Renders the TextEditor component
  return (
    <>
      <div>
        <ReactQuill
          placeholder="Write something..." // Placeholder text for the editor
          modules={TextEditor.modules} // Custom toolbar modules
          formats={TextEditor.formats} // Custom formats
          onChange={handleBody} // Event handler for content changes
          value={body} // Current value of the editor content
        />
        <button onClick={handleSubmit}>Submit</button> {/* Button to submit the content */}
      </div>
      {/* Div to display the submitted content, parsed as HTML */}
      <div className="ql-editor">{parse(submittedContent)}</div>
    </>
  );
};

// Custom toolbar modules for the Quill editor
TextEditor.modules = {
  toolbar: [
    [{ header: "1" }, { header: "2" }, { font: [] }],
    [{ size: [] }],
    ["bold", "italic", "underline", "strike", "blockquote"],
    [{ list: "ordered" }, { list: "bullet" }, { indent: "-1" }, { indent: "+1" }],
    ["link", "image", "video"],
    ["clean"],
    [{ align: "" }, { align: "center" }, { align: "right" }, { align: "justify" }],
  ],
};

// Custom formats for the Quill editor
TextEditor.formats = [
  "header",
  "font",
  "size",
  "bold",
  "italic",
  "underline",
  "strike",
  "blockquote",
  "list",
  "bullet",
  "indent",
  "link",
  "image",
  "video",
  "align",
];

export default TextEditor; // Exports the TextEditor component as the default export
```

⦿ 이제 원하는 곳에서 "TextEditor.tsx"를 호출해보세요.

<div class="content-ad"></div>

```js
import dynamic from "next/dynamic"; // Next.js에서 동적 컴포넌트를 불러옵니다

// 메인 페이지 컴포넌트인 Home을 정의합니다
export default function Home() {
  // "./Components/TextEditor/TextEditor"에서 TextEditor 컴포넌트를 동적으로 불러옵니다
  const TextEditor = dynamic(
    () => import("./Components/TextEditor/TextEditor"), // TextEditor 컴포넌트 파일의 경로
    {
      ssr: false, // 이 컴포넌트의 서버 측 렌더링(SSR)을 비활성화합니다
    }
  );

  // Home 컴포넌트를 렌더링합니다
  return (
    <div>
      <TextEditor /> {/* 동적으로 불러온 TextEditor 컴포넌트를 렌더링합니다 */}
    </div>
  );
}
```

## 프로젝트를 실행하면 텍스트 편집기가 다음과 같이 표시됩니다

<img src="/assets/img/2024-07-07-QuillRichTextEditor_1.png" />

## 콘텐츠를 추가하고 Quill 텍스트 편집기를 확인해보세요. 분명히 여러분에게 잘 맞을 것입니다.

<div class="content-ad"></div>

![이미지](/assets/img/2024-07-07-QuillRichTextEditor_2.png)

➡ 데모 텍스트 편집기 링크
➡ GitHub 저장소
➡ 더 많은 정보는 Quill.com에서 확인하세요.
