---
title: "React와 Material UI로 간단한 팝업 만드는 방법"
description: ""
coverImage: "/assets/img/2024-07-07-CreatesimplepopupinReactandMaterialUI_0.png"
date: 2024-07-07 19:04
ogImage:
  url: /assets/img/2024-07-07-CreatesimplepopupinReactandMaterialUI_0.png
tag: Tech
originalTitle: "Create simple popup in React and Material UI"
link: "https://medium.com/@vishhxh/create-simple-popup-in-react-and-material-ui-b5600f2efc81"
---

참고 파일 구조

![이미지1](/assets/img/2024-07-07-CreatesimplepopupinReactandMaterialUI_0.png)

![이미지2](/assets/img/2024-07-07-CreatesimplepopupinReactandMaterialUI_1.png)

기대되는 결과:

<div class="content-ad"></div>

<img src="/assets/img/2024-07-07-CreatesimplepopupinReactandMaterialUI_2.png" />

먼저 카드를 만들어봅시다.

카드에 대한 JavaScript 코드입니다.

기억해야 할 두 가지 사항입니다.

<div class="content-ad"></div>

- 팝업을 열고 닫아주어야 해요.
- 삭제 팝업 및 편집 팝업 처리 [버튼이 두 개이므로 동적 텍스트를 보내야 해요]

![이미지1](/assets/img/2024-07-07-CreatesimplepopupinReactandMaterialUI_3.png)

![이미지2](/assets/img/2024-07-07-CreatesimplepopupinReactandMaterialUI_4.png)

이것이 전체 카드 구성요소입니다. 토글, 편집, 삭제에 사용된 상태 및 함수를 그대로 복사해 사용하세요.

<div class="content-ad"></div>

아래 코드에서 버튼을 찾아서 아래로 이동하십시오

```js
import { Grid, Typography } from "@mui/material";
import React, { useState } from "react";
import img1 from "../Assets/images/articles.jpg";
import Popup from "./Popup";
function PostCard() {
  const [owner, setOwner] = useState(true);
  const [isOpen, setOpen] = useState(false);
  const [pop, setPop] = useState("");
  function toggleOpen() {
    setOpen(true);
  }
  function toggleClose() {
    setOpen(false);
  }

  function deletePop() {
    setPop("Delete");
    toggleOpen();
  }
  function EditPop() {
    setPop("Edit");
    toggleOpen();
  }
  return (
    <>
      <Grid className="p-wrap">
        <Grid className="p-w-1">
          <Grid container className=" pw1-w">
            {/* area for img */}
            <Grid item className="pw1-img ">
              <Grid className="pw12-1 ">
                <img className="img-post" src={img1} alt="pst ing" />
              </Grid>
            </Grid>
            {/* area for text */}
            <Grid item className="pw1-ta ">
              {/* headding */}
              <Grid container className=" pwta-w">
                <Grid className=" pwta-1">
                  <Typography variant="h5" sx={{ fontWeight: 700 }}>
                    How to create a simple pop up in React and Material ui?{" "}
                  </Typography>
                </Grid>
                <Grid className=" pwta-2">
                  <Typography variant="body">this is a detailed blog</Typography>{" "}
                </Grid>
                <Grid className=" pwta-3">
                  <Grid container className="pwta-3-1">
                    <Grid className="pwta-btn">
                      <button className="btn">Read more</button>
                    </Grid>
                    {owner && (
                      <>
                        <Grid className="pwta-btn">
                          <button onClick={EditPop} className="btn-1">
                            Edit
                          </button>
                        </Grid>
                        <Grid className="pwta-btn">
                          <button className="btn-3" onClick={deletePop}>
                            Delete
                          </button>
                        </Grid>{" "}
                      </>
                    )}

                    {isOpen && (
                      <>
                        <Popup handleClose={toggleClose} popText={pop} />
                      </>
                    )}
                  </Grid>
                </Grid>
              </Grid>
            </Grid>
          </Grid>
        </Grid>
      </Grid>
    </>
  );
}

export default PostCard;
```

팝업 상자 및 CSS를 위한 간단한 React 구성 요소 코드

```js
import { Grid, Typography } from "@mui/material";
import React from "react";

function Popup(props) {
  return (
    <>
      <Grid container className="popup-box">
        <Grid className="box">
          <Grid container className="pop-center">
            <Grid container item className="textarea-pop">
              <Typography variant="body">Do you want to {props.popText} this blogg?</Typography>
            </Grid>
            <Grid item container className="pop-center btn-pops">
              <Grid className="btn-wr-p">
                <Grid item className="p-w-b">
                  <Grid item>
                    <button className="btn-3" onClick={props.handleClose}>
                      Close
                    </button>
                  </Grid>
                </Grid>
                <Grid item className=" p-w-b">
                  <Grid item>
                    <button className="btn">{props.popText}</button>
                  </Grid>
                </Grid>{" "}
              </Grid>
            </Grid>
          </Grid>
        </Grid>
      </Grid>
    </>
  );
}

export default Popup;
```

<div class="content-ad"></div>

SCSS:

파일 구조

![이미지](/assets/img/2024-07-07-CreatesimplepopupinReactandMaterialUI_5.png)

여기서 mixin이 사용되었습니다 😉

<div class="content-ad"></div>

만약 CSS로 이 내용을 작성하려면, flex에 대한 전체 CSS 코드를 작성해보세요.

```css
@mixin flex($align: stretch, $justify: flex-start, $direction: row) {
  display: flex;
  flex-direction: $direction;
  align-items: $align;
  justify-content: $justify;
}

@mixin respond-to($breakpoint) {
  @if $breakpoint == small {
    @media (max-width: 600px) {
      @content;
    }
  } @else if $breakpoint == tablet {
    @media (max-width: 1000px) {
      @content;
    }
  } @else if $breakpoint == mid {
    @media (max-width: 1200px) {
      @content;
    }
  } @else if $breakpoint == desktop {
    @media (max-width: 1500px) {
      @content;
    }
  }
}
```

배경을 흐리게 하는 팝업을 위한 중요한 CSS

```css
.popup-box {
  background: #00000050;
  width: 100%;
  height: 100vh;
  position: fixed;
  top: 0;
  left: 0;
  @include flex(center, center, row);
}

.box {
  position: relative;
  width: 35%;
  margin: 0 auto;
  height: 25%;
  margin-top: calc(100vh - 85vh - 20px);
  background: #fff;
  border-radius: 2vh;
  padding: 2vh;
  border: 1px solid #999;
  overflow: auto;
  @include flex(center, center, row);
  @include respond-to(small) {
    width: 90%;
  }
}
```

<div class="content-ad"></div>

전체 팝업 상자에 대한 CSS입니다.

```js
/* 팝업 스타일 */

.popup-box {
    background: #00000050;
    width: 100%;
    height: 100vh;
    position: fixed;
    top: 0;
    left: 0;
    @include flex(center,center,row);

  }

  .box {
    position: relative;
    width: 35%;
    margin: 0 auto;
    height: 25%;
    margin-top: calc(100vh - 85vh - 20px);
    background: #fff;
    border-radius: 2vh;
    padding: 2vh;
    border: 1px solid #999;
    overflow: auto;
    @include flex(center,center,row);
    @include respond-to(small){
        width: 90%;

    }

  }


  .pop-center{
    @include flex(center,center,row);
    height: 100%;

  }

  .p-w-b{
    width: 50%;
    @include flex(center,center,row);

  }

  .btn-pops{
    height: 50%;
    width: 100%;
    @include flex(center,center,row);

  }

  .btn-wr-p{
    @include flex(center,center,row);
    width: 70%;
    height: 100%;
    @include respond-to(tablet){
        width: 100%;
    }
  }
  .textarea-pop{
    width: 100%;
    height: 50%;
    @include flex(center,center,row);

  }
```

팝업을 만들어야 했는데 어떻게 해야 할지 몰라서 많은 참고 자료를 검색했고 드디어 해내었어요.

이제 이 팝업을 어디서든 재사용할 수 있어요 UwU :)

<div class="content-ad"></div>

아래 링크를 참고하시면 도움이 될 것 같아요:

[ReactJS Popup](https://stackblitz.com/edit/reactjs-popup?file=src%2FApp.js)

읽어 주셔서 감사합니다.

도움이 되었으면 좋겠어요 :)
