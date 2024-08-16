---
title: "JavaScript를 사용하여 배열에서 값을 찾는 방법"
description: ""
coverImage: "/assets/img/2024-07-29-SearchingforaValueinanArrayUsingJavaScript_0.png"
date: 2024-07-29 13:50
ogImage: 
  url: /assets/img/2024-07-29-SearchingforaValueinanArrayUsingJavaScript_0.png
tag: Tech
originalTitle: "Searching for a Value in an Array Using JavaScript"
link: "https://dev.to/sudhanshu_developer/searching-for-a-value-in-an-array-using-javascript-fh4"
isUpdated: true
updatedAt: 1723816424771
---



이 프로그램은 JavaScript에서 while 루프를 사용하여 배열 내에서 특정 값(val)을 검색하는 방법을 보여줍니다. 사용자에게 값을 입력하라는 메시지가 나타나며, 프로그램은 이 값이 미리 정의된 다양한 값들 중에 있는지 확인합니다. 값이 발견되면 해당 값의 가용성을 나타내는 메시지가 표시되고, 그렇지 않으면 값이 존재하지 않음을 나타내는 메시지가 표시됩니다.

간단한 예제:

```js
let Val = prompt("값을 입력하세요!");
let numbers = [10, 20, 30, 40, 50, 60, 70, 80, 90, 100, 200, 300];

let i = 0;
let Find = false;
while (i < numbers.length) {
  if (numbers[i] == Val) {
    Find = true;
    break;
  }
  i++;
}
if (Find) {
  console.log("해당 값이 존재합니다!");
} else {
  console.log("해당 값이 존재하지 않습니다!");
}
```

결과:

<div class="content-ad"></div>

표 태그를 Markdown 형식으로 변경하겠습니다.

이 프로그램은 JavaScript에서 while 루프를 사용하여 배열 내에서 특정 값을 검색하는 방법을 보여줍니다. 사용자가 입력 필드에 값을 입력하면 프로그램이 미리 정의된 배열 내에 해당 값이 있는지 확인합니다. 값이 발견되면 해당 값의 가용성을 나타내는 메시지가 표시되고, 그렇지 않으면 해당 값이 없음을 나타내는 메시지가 표시됩니다.

예시:

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Find Element in Array Using DOM in JavaScript </title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
        }
        h4, h2 {
            color: #333;
        }
        input {
            padding: 10px;
            margin-right: 10px;
            border: 1px solid #ccc;
            border-radius: 5px;
        }
        button {
            padding: 10px 20px;
            border: none;
            border-radius: 5px;
            background-color: #28a745;
            color: white;
            cursor: pointer;
        }
        button:hover {
            background-color: #218838;
        }
    </style>
</head>
<body>
    <h4>Find Element in Array</h4>
    <h4 id="NumData"></h4>
    <input type="text" placeholder="Enter Value" id="Num" />
    <button onclick="SearchEle()">Find</button>
    <h2 id="Ans"></h2>

    <script>
        let Data = [10, 20, 30, 40, 50, 60, 70, 80, 90, 100, 200, 300];
        document.getElementById("NumData").innerHTML += "Our Array is: " + Data.join(', ');

        function SearchEle() {
            let NewValue = document.getElementById("Num").value;
            let i = 0;
            let Find = false;
            while (i < Data.length) {
                if (Data[i] == NewValue) {
                    Find = true;
                    break;
                }
                i++;
            }

            if (Find) {
                document.getElementById("Ans").innerHTML = `${NewValue} 
                is Available..!`;
            } else {
                document.getElementById("Ans").innerHTML = `${NewValue} 
               is not Available..!`;
            }
        }
    </script>
</body>
</html>
```

<div class="content-ad"></div>

출력:

![이미지1](/assets/img/2024-07-29-SearchingforaValueinanArrayUsingJavaScript_0.png)

![이미지2](/assets/img/2024-07-29-SearchingforaValueinanArrayUsingJavaScript_1.png)