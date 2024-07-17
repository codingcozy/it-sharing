---
title: "Nextjs 프로젝트에서 기본적인 무한 캔버스 만드는 방법"
description: ""
coverImage: "/assets/img/2024-07-12-CreatingABasicInfinityCanvasForYourNextJsProject_0.png"
date: 2024-07-12 19:09
ogImage:
  url: /assets/img/2024-07-12-CreatingABasicInfinityCanvasForYourNextJsProject_0.png
tag: Tech
originalTitle: "Creating A Basic Infinity Canvas For Your Next.Js Project"
link: "https://medium.com/@programming-advice/creating-a-basic-infinity-canvas-from-your-next-js-project-629b4eec7993"
---

## 나는 결국 쟈니의 조언을 들었어요…

만약 여러분이 미디움 회원이 아니시라면, 이 기사를 읽어 보세요. niftylittleme.com으로 이동하세요.

결국 쟈니가 맞았어요: 뭔가를 원한다면 내가 직접 해야 해요. 그래서, 나는 손을 더럽혔고, 나의 옷장에 더 많은 비밀을 감췄어요. 그 덕분에, 쟈니는 더 이상 우리와 함께하지 않아요. 저는 농담이에요, 당연히요.

당신이 생각하는 것을 했던 것 대신에, 나는 무한 캔버스에 대한 해결책을 만드는 데 착수하기로 했어요. 그리고 쟈니가 누구인지 모른다면, 이 소개 이후에 설명할게요. 궁금하다면 '이전 프로그래밍 조언' 섹션으로 건너뛰어서 확인해보세요.

<div class="content-ad"></div>

이 글에서는 아직 존재하지 않는 라이브러리나 도구를 사용하지 않고 기본적인 무한 캔버스를 만들어 볼 것입니다. 사실 하나는 있지만, 다시 언급하지는 않을게요. 알고 있다면 무한 캔버스를 만드는 것이 그렇게 어렵지 않다는 것을 알게 되었어요. 기본 기능으로 하나를 성공적으로 만들었죠. 지금까지 한 것을 공유해볼까요? 소개에서 벗어나서 만들기 시작해봐요!

![image](/assets/img/2024-07-12-CreatingABasicInfinityCanvasForYourNextJsProject_0.png)

# 이전 프로그래밍 조언에서는...

기본적으로 지난 글은 React 프로젝트에서 무한 캔버스를 쉽게 설정할 수 있는 도구나 라이브러리가 없다는 것에 대한 내용이었어요. 불행하게도 무한 캔버스 기능을 갖춘 도구가 많이 있기는 하지만, 해결책은 있지만 공유되지 않는다는 점이 있어요.

<div class="content-ad"></div>

존니는 이전 기사에서 한 말이 뭔지 그냥 이야기한 뒤에 솔루션을 만들어서 그런 게 있는 것에 대해 투덜거리는 대신에 그 솔루션을 만드는 게 좋다고 제안했어요.

리액트 프로젝트에 비슷한 기능을 추가하기 위한 문서가 제공되는 해결책이 하나 있어요. 제 마지막 기사에서 그에 관해 읽어보세요.

# 툴바 만들기

Next.js 프로젝트를 만드는 방법은 이미 알고 계실 테니 초보자 안내 부분을 건너뛰죠. 또한 프로젝트 내부에 설치할 것이 없으니까요.

<div class="content-ad"></div>

src/app/ 디렉토리 내에 components 폴더를 만들어주세요. components 폴더 안에 toolbar.jsx라는 파일을 만들어주세요. 이 파일 안에 추가하고 싶은 모든 도구의 버튼들이 있을 겁니다:

```js
"use client";
import React from "react";

const Toolbar = ({ addItem }) => {
  return (
    <div className="relative top-4 left-10 bg-white p-4 rounded shadow z-10">
      <div className="gap-4 flex flex-row">
        <button onClick={() => addItem("rectangle")}>Add Rectangle</button>
        <button onClick={() => addItem("circle")}>Add Circle</button>
      </div>
    </div>
  );
};

export default Toolbar;
```

# 캔버스 만들기

components 폴더 안에 canvas.jsx라는 새 파일을 만들어주세요. 이 파일 안에서 몇 가지 작업을 할 거에요.

<div class="content-ad"></div>

위에 있는 코드로 모든 작업을 완료할 수 있어요:
들어온 툴바 추가
패닝 추가
확대/축소 추가
그리드 생성
확대 비율 표시

```js
"use client";
import { useRef, useEffect, useState } from "react";
import Toolbar from "./toolbar";

const InfiniteCanvas = () => {
  const canvasRef = useRef(null);
  const [scale, setScale] = useState(1);
  const [translateX, setTranslateX] = useState(0);
  const [translateY, setTranslateY] = useState(0);
  const [isPanning, setIsPanning] = useState(false);
  const [startX, setStartX] = useState(0);
  const [startY, setStartY] = useState(0);
  const [items, setItems] = useState([]);

  useEffect(() => {
    const canvas = canvasRef.current;
    const ctx = canvas.getContext("2d");

    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;

    const draw = () => {
      ctx.save();
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      ctx.translate(translateX, translateY);
      ctx.scale(scale, scale);

      // Example drawing: An infinite grid
      const gridSize = 50;
      const width = canvas.width;
      const height = canvas.height;

      // Calculate the top-left corner of the grid to start drawing
      const startX = Math.floor(-translateX / scale / gridSize) * gridSize;
      const startY = Math.floor(-translateY / scale / gridSize) * gridSize;

      for (let x = startX; x < width / scale - translateX / scale; x += gridSize) {
        for (let y = startY; y < height / scale - translateY / scale; y += gridSize) {
          ctx.strokeRect(x, y, gridSize, gridSize);
        }
      }

      // Draw added items
      items.forEach((item) => {
        if (item.type === "rectangle") {
          ctx.fillStyle = "blue";
          ctx.fillRect(item.x, item.y, 100, 50);
        } else if (item.type === "circle") {
          ctx.fillStyle = "red";
          ctx.beginPath();
          ctx.arc(item.x, item.y, 50, 0, 2 * Math.PI);
          ctx.fill();
        }
      });

      ctx.restore();
    };

    draw();

    const handleResize = () => {
      canvas.width = window.innerWidth;
      canvas.height = window.innerHeight;
      draw();
    };

    window.addEventListener("resize", handleResize);

    return () => {
      window.removeEventListener("resize", handleResize);
    };
  }, [scale, translateX, translateY, items]);

  const handleWheel = (event) => {
    event.preventDefault();
    const zoomIntensity = 0.1;
    const mouseX = event.clientX - canvasRef.current.offsetLeft;
    const mouseY = event.clientY - canvasRef.current.offsetTop;
    const zoomFactor = 1 + event.deltaY * -zoomIntensity;

    const newScale = Math.min(Math.max(0.5, scale * zoomFactor), 5); // Limit zoom scale

    setTranslateX(translateX - (mouseX / scale) * (newScale - scale));
    setTranslateY(translateY - (mouseY / scale) * (newScale - scale));
    setScale(newScale);
  };

  const handleMouseDown = (event) => {
    setIsPanning(true);
    setStartX(event.clientX - translateX);
    setStartY(event.clientY - translateY);
  };

  const handleMouseMove = (event) => {
    if (!isPanning) return;
    setTranslateX(event.clientX - startX);
    setTranslateY(event.clientY - startY);
  };

  const handleMouseUp = () => {
    setIsPanning(false);
  };

  const handleMouseLeave = () => {
    setIsPanning(false);
  };

  const addItem = (type) => {
    const newItem = {
      type,
      x: (canvasRef.current.width / 2 - translateX) / scale,
      y: (canvasRef.current.height / 2 - translateY) / scale,
    };
    setItems([...items, newItem]);
  };

  return (
    <div>
      <Toolbar addItem={addItem} />
      <div style={{ position: "relative", width: "100%", height: "100%" }}>
        <canvas
          ref={canvasRef}
          onWheel={handleWheel}
          onMouseDown={handleMouseDown}
          onMouseMove={handleMouseMove}
          onMouseUp={handleMouseUp}
          onMouseLeave={handleMouseLeave}
          style={{ width: "100%", height: "100%", display: "block" }}
        />
        <div className="text-center fixed bottom-0 left-4 right-4 z-10 bg-gray-100 p-2 rounded shadow">
          Zoom: {(scale * 100).toFixed(0)}%
        </div>
      </div>
    </div>
  );
};

export default InfiniteCanvas;
```

# 무한캔버스 표시

<div class="content-ad"></div>

이제, 페이지.tsx 파일 코드에 몇 줄을 추가하여 모든 것을 표시해 봅시다:

```js
import InfiniteCanvas from './components/canvas';

export default function Home() {
    return (
        <div style={width: '100vw', height: '100vh', overflow: 'hidden'}>
            <InfiniteCanvas />
        </div>
    );
}
```

Next.js에서 간단한 무한 확장 캔버스를 만드는 기사를 마무리 지었습니다. 물론, 추가하고 싶은 것들이 더 있을 수 있습니다 — 제가 추가하고 싶은 것도 많지만, 기본적인 것에 대한 이야기입니다.

만약 이 기사를 좋아하셨다면 박수를 치거나 팔로우하시고 응답해주세요.

<div class="content-ad"></div>

행복한 코딩하세요!
