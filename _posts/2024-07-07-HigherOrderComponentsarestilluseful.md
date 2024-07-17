---
title: "Higher Order Components HOC는 여전히 유용할까"
description: ""
coverImage: "/it-shar.ing/assets/no-image.jpg"
date: 2024-07-07 21:05
ogImage:
  url: /it-shar.ing/assets/no-image.jpg
tag: Tech
originalTitle: "Higher Order Components are still useful"
link: "https://medium.com/@psbakre/higher-order-components-are-still-useful-c894ce1383ba"
---

# 안녕하세요

리액트 훅스가 도입되기 전에는 클래스 컴포넌트가 주를 이뤘을 때, 고차 컴포넌트(Higher Order Components, HOCs)만 있으면 데이터 전달, 리덕스 스토어에 연결, 추가 프롭스 전달, React Router를 이용한 라우팅 설정 등이 모두 가능했습니다. 그러나 이제는 훅스와 컨텍스트가 이 모든 역할을 대신하고 있어요.

- withRouter()가 useRouter()로 대체되었어요.
- connect()()가 useSelector()로 대체되었습니다.
- Wrapper들은 훅스로 변환되었습니다.

네, 대부분의 경우에 훅스가 간단히 더 좋아요. 가상 DOM 트리를 채울 필요가 없는 불필요한 컴포넌트가 없죠. 단순히 useFunctions()를 사용하여 부모에서 자식으로 데이터를 전달하는 복잡한 코드를 작성할 필요가 없어요. 동일한 컴포넌트에서 작업을 처리할 수 있죠.

<div class="content-ad"></div>

그럼, 훅이 HOC를 작성하거나 사용하는 필요성을 거의 없애는 경우, 우리는 실제로 HOC에 대해 배울 필요가 있을까요?

# 고차 컴포넌트란 무엇인가요?

HOC가 무엇인지 설명하기 전에, 먼저 데코레이터에 대해 이해해 봅시다.

우리가 무언가를 장식할 때, 더 많은 속성을 추가합니다. 아름답게 만들어 주거나 추가 기능을 제공할 수도 있습니다. 전구는 빛을 발산합니다. 그것을 스탠드에 부착하면 조명을 얻을 수 있습니다. 교통 신호에 전구를 넣으면 교통을 제어할 수 있는 방법을 제공합니다. 이것이 바로 데코레이터라는 디자인 패턴입니다.

<div class="content-ad"></div>

위와 같이 정의됩니다.

고차 컴포넌트는 데코레이터 패턴의 구현입니다. 이는 새로운 속성을 추가하여 전달된 엔티티를 강화할 컴포넌트 또는 함수입니다.

# HOC를 어떻게 작성하고 사용할까요?

사용법은 간단합니다. 컴포넌트를 HOC로 감싸고 반환된 값을 일반 컴포넌트처럼 사용하면 됩니다.

<div class="content-ad"></div>

예를 들어

```js
const WrappedComponent = withData(Component);

<WrappedComponent />;

// 컴포넌트에서의 사용 예시
function Component({ data }) {
  return <>{data}</>;
}
```

데이터는 withData HOC를 통해 전달됩니다.

반면에, 글을 쓰는 것은 조금 복잡하다.

<div class="content-ad"></div>

이 코드는 새로운 컴포넌트를 정의하고 반환하는 함수를 작성합니다. 새 컴포넌트는 원래 컴포넌트에 몇 가지 추가적인 속성을 제공한 후 원래 컴포넌트를 반환합니다.

예:

```js
import { useState } from "react";

export function Counter({ counter, setCounter }) {
  return (
    <>
      <div>{counter}</div>
      <button onClick={setCounter}></button>
    </>
  );
}

export function withCounter(OriginalComponent) {
  const UpdatedComponent = function () {
    const [counter, setCounter] = useState(0);

    return <OriginalComponent counter={counter} setCounter={() => setCounter((c) => ++c)} />;
  };

  return UpdatedComponent;
}

export const MyCounter = withCounter(Counter);
```

그래서 코드는 실제로 어떤 작업을 했는지 알고 계신가요?

<div class="content-ad"></div>

요청하신 것은 'table' 태그를 Markdown 형식으로 변경하는 것입니다.

| Header One   | Header Two   |
| ------------ | ------------ |
| Row 1, Col 1 | Row 1, Col 2 |
| Row 2, Col 1 | Row 2, Col 2 |

<div class="content-ad"></div>

withCounter은 고차 컴포넌트입니다.

withCounter는 카운터 상태가 정의된 래퍼 컴포넌트를 정의합니다. 그런 다음이 래퍼 컴포넌트는 실제로 우리가 정의한 컴포넌트(Counter)에 상태 변수를 전달합니다.

```js
export function withCounter(OriginalComponent) {
  const UpdatedComponent = function () {
    const [counter, setCounter] = useState(0);

    return <OriginalComponent counter={counter} setCounter={() => setCounter((c) => ++c)} />;
  };

  return UpdatedComponent;
}
```

MyCounter은 구현입니다.

<div class="content-ad"></div>

```js
export const MyCounter = withCounter(Counter);
```

알겠어요. 고차 컴포넌트가 무엇인지, 어떻게 작성하고 사용하는지 이해했어요. 이제 어디에 사용할 수 있나요?

# 문제점

재사용 가능한 파일 업로드 컴포넌트를 구축하는 중입니다.

<div class="content-ad"></div>

사용자가 미디어를 업로드할 수 있도록 하고 싶어요. 이미지(프로필 사진), 동영상(소개), 첨부 파일(PDF, PPT 등)을 올릴 수 있는 몇 군데의 장소가 있어요. 각각의 장소마다 다른 UI가 있지만 API는 동일해요.

구체적으로는

- UI가 바뀔 수 있어요
- 파일 유형을 속성으로 전달할 수 있어야 해요
- 파일 이름을 덮어쓸 수 있어야 해요
- 스크린에 업로드 진행 상황을 렌더링할 수 있어야 해요
- 일부 요소는 파일을 끌어다 놓는 것을 지원해야 해요

가장 중요한 것은, 한 페이지에 여러 개의 파일 업로드 구성 요소가 있을 수 있다는 것이에요.

<div class="content-ad"></div>

# 해결책

이 문제를 해결하는 여러 가지 방법이 있습니다.

**해결책 1: 개별 컴포넌트**
모든 사용 사례에 대해 개별 구성 요소를 정의하는 최악의 해결책입니다. 이 해결책은 재사용이 불가능합니다. 동일한 "키"에 대해 다른 위치에서 UI를 변경해야 하는 경우 다시 새로운 컴포넌트를 만들어야 합니다.

<div class="content-ad"></div>

해결 방법 2: useUpload 훅 사용하기

파일 이름(fileName)과 파일 타입(fileType)을 입력값(props)으로 받아서 onDrop, onFileInput, progress, fileUrl을 출력값으로 반환하는 공통 훅을 만들 수 있습니다.

이렇게 하면 API를 공통 훅으로 옮기고 UI에만 집중할 수 있게 됩니다.

좋은 해결책이지만 함정이 있습니다. 여전히 해당 훅을 사용하는 곳마다 개별적인 컴포넌트를 만들어야 합니다. 왜냐하면 페이지에서 이미지와 파일을 업로드할 수 있는 기능이 있는 경우, 같은 훅을 컴포넌트에서 두 번 사용할 수는 없습니다. 이러한 문제를 해결하려면 반환된 매개변수의 이름을 바꿔야 합니다. 여전히 사용 가능하지만 더 개선할 수 있는 부분이 있습니다.

<div class="content-ad"></div>

구성 요소의 부분들.

구성 요소는 3개의 부분으로 나뉠 수 있어요.

- 변수 (상태) 및 함수 (핸들러)
- API 호출, 로직 등 (효과)
- JSX (UI)

컨테이너 구성 요소가 크면 UI가 상태와 로직으로부터 멀어질 수 있어요. 만약 로직을 UI에 더 가깝게 가져올 수 있다면 좋을텐데요.

<div class="content-ad"></div>

해결책 3: HOC의 변형입니다.

이 구현은 정확히 고차 컴포넌트는 아니지만 파생된 것입니다.

내가 한 일은 다음과 같습니다.

```js
import axios from 'axios';
import {
  UploadAccess,
  UploadType,
  uploadAPIService,
} from 'lib/UploadAPIService';
import {
  ChangeEventHandler,
  DragEventHandler,
  ReactNode,
  useCallback,
  useState,
} from 'react';

export interface UploadMediaChildrenProps {
  url?: string;
  onDrop: DragEventHandler<HTMLElement>;
  uploadPercentage: number;
  isUploading: boolean;
  onFileSelect: ChangeEventHandler<HTMLInputElement>;
}
export interface UploadMediaProps {
  children: (props: UploadMediaChildrenProps) => ReactNode;
  uploadAccess: UploadAccess;
  uploadType: UploadType;
  onUpload: (url: string) => void;
  filePath: string;
}
export function UploadMedia({
  children,
  uploadAccess,
  uploadType,
  onUpload,
  filePath,
}: UploadMediaProps) {
  const [isUploading, setIsUploading] = useState(false);
  const [uploadPercentage, setUploadPercentage] = useState(0);
  const [url, setUrl] = useState<string | undefined>();

  const uploadMedia = useCallback(
    async (file: File) => {
      setIsUploading(true);
      const urls = await uploadAPIService.getUploadURL({
        file_name:
          filePath + '/' + file.name ??
          (+new Date() * Math.random()).toString(36).substring(0, 5),
        access: uploadAccess,
        type: uploadType,
      });

      await axios.put(urls.uploadUrl, file, {
        onUploadProgress: progressEvent => {
          setUploadPercentage(
            Math.round((progressEvent!.loaded * 100) / progressEvent!.total!)
          );
        },
      });
      setUrl(urls.retrievalUrl);
      setIsUploading(false);
      onUpload(urls.retrievalUrl);
    },
    [uploadAccess, uploadType, onUpload, filePath]
  );

  const onDrop: DragEventHandler<HTMLElement> = useCallback(
    e => {
      if (e.type === 'drop') {
        e.preventDefault();
        const file = e.dataTransfer.files[0];
        uploadMedia(file);
      }
    },
    [uploadMedia]
  );

  const onFileSelect: ChangeEventHandler<HTMLInputElement> = useCallback(
    e => {
      if (!e.target.files) throw new Error('No files found');
      const file = e.target.files[0];
      uploadMedia(file);
    },
    [uploadMedia]
  );
  const Component = children;
  return (
    <Component
      onDrop={onDrop}
      uploadPercentage={uploadPercentage}
      isUploading={isUploading}
      onFileSelect={onFileSelect}
      url={url}
    />
  );
}
```

<div class="content-ad"></div>

사용 방법

```js
<UploadMedia
  uploadAccess={UploadAccess.public}
  uploadType={UploadType.picture}
  filePath={`users/username/${userId}/picture`}
  onUpload={(url) => {
    setPictureUrl(url);
  }} // onUpload={setPictureUrl}로 줄일 수 있습니다.
>
  {({ uploadPercentage, isUploading, onFileSelect }) => (
    <Box
      sx={{
        position: "relative",
        width: "40px",
        height: "40px",
      }}
    >
      <FormLabel
        htmlFor={`user-${userId}-picture`}
        sx={{
          position: "absolute",
          left: 0,
          top: 0,
          width: "100%",
          height: "100%",
          cursor: "pointer",
          display: "flex",
          alignItems: "center",
          justifyContent: "center",
          background: !pictureUrl ? "rgb(255,255,255,0.05)" : "transparent",
          borderRadius: "50%",
        }}
      >
        {!pictureUrl && <Add sx={{ color: "white" }} />}
        {isUploading && uploadPercentage}
      </FormLabel>
      <input
        name={`user-${userId}-picture`}
        type="file"
        id={`user-${userId}-picture`}
        hidden
        style={{ display: "none" }}
        onChange={onFileSelect}
      />
      {pictureUrl && (
        <Box
          component="img"
          src={pictureUrl}
          sx={{
            width: "40px",
            height: "40px",
            borderRadius: "50%",
          }}
        />
      )}
    </Box>
  )}
</UploadMedia>
```

그럼, 이 코드가 무엇을 하는 건가요?

- UploadMedia는 고차 컴포넌트 패턴의 변형이며, 드롭 및 파일 선택 이벤트 핸들러, 업로드 진행률, URL 및 업로드 여부를 상태 변수로 정의하며, 파일을 처리하는 uploadMedia 함수도 포함합니다.
- uploadMedia 함수는 파일을 받아들이며(드롭 및 파일 선택 메서드에 의해 호출), 상태를 설정하는 코드를 첨부합니다(진행률, isUploading 상태 및 파일 업로드 후 URL 설정).
- UploadMedia 컴포넌트는 논리의 책임을 완전히 맡아 자식에게 렌더링을 맡깁니다. 위 코드에서 확인할 수 있습니다.
- UploadMedia는 자식으로 컴포넌트 정의를 받아들이고 렌더링을 위해 필요한 속성을 자식에게 전달합니다.

<div class="content-ad"></div>

이 코드의 장점:

- 훅의 경우와 같이 상태 변수에 별칭을 선언하고 할당할 필요가 없습니다.
- 업로드 로직을 반복하지 않고 여러 업로드 컴포넌트를 추가할 수 있습니다.
- 상태 변수와 렌더링 JSX가 그룹화되어 있습니다.

단점:

- 그런데 JSX 코드가 더 깁니다.

<div class="content-ad"></div>

프로퍼 HOC를 만들고 적절하게 사용하면 이 문제를 제거할 수 있어요. 이렇게 하면 자식 컴포넌트로 프롭스를 전달하고 JSX에 함수 정의를 갖지 않고 자식 컴포넌트를 정의할 수 있어요.

2. 또 하나 주의할 점은 프롭스를 전달한 방식이에요. 이상적으로는 함수를 선언한 다음 JSX 안에서 화살표 함수로 선언하는 대신 해당 함수를 아래로 전달해야 해요.

# 결론

내가 소속한 회사에서 만들어진 학습 프로그램의 세부 정보를 업로드하는 관리자 화면을 개발할 때 이 접근 방식을 생각해보았어요. 동일한 페이지의 여러 위치에서 다른 이미지를 업로드했던 점을 알게 되었죠. 업로드 코드는 항상 같았어요. 유일한 차이점은 UI를 어떻게 렌더링해야 하는지였어요. Hooks와 개별 컴포넌트는 항상 사용돼 왔어요. 하지만 이것이 더 확장 가능하고 여러 곳에서 사용할 수 있는 대안 솔루션이 있는지 의문을 제기했어요.

<div class="content-ad"></div>

이것을 나쁜 아이디어로 생각하신다면, 의견을 남겨주시고 더 나은 접근 방법이 무엇인지 알려주세요. 마음에 드신다면 알려주세요.
