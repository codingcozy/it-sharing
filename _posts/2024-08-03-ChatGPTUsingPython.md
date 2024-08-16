---
title: "Python으로 ChatGPT 사용 방법"
description: ""
coverImage: "/assets/img/2024-08-03-ChatGPTUsingPython_0.png"
date: 2024-08-03 21:01
ogImage: 
  url: /assets/img/2024-08-03-ChatGPTUsingPython_0.png
tag: Tech
originalTitle: "ChatGPT Using Python"
link: "https://medium.com/@jrday3/chatgpt-using-python-c416b887239f"
isUpdated: true
updatedAt: 1723817086791
---



![image](/assets/img/2024-08-03-ChatGPTUsingPython_0.png)

파이썬 스크립트에서 ChatGPT의 기능을 편안하게 활용하세요. 브라우저에서 OpenAI에 다시 로그인하지 않아도 됩니다!

# API 키를 환경 변수로 저장

참고: 이 단계를 건너뛰고 api_key를 다음 코드 블록에 직접 복사할 수 있습니다. 저는 키를 공개하고 코드를 공개하기 전에 제거하는 것을 잊지 않기 위해 이렇게 사용합니다.

<div class="content-ad"></div>

```js
# 터미널에서 적절한 값을 플러그인하여 변수 생성
!setx environmental_variable_name api_key

# 터미널 재시작

# 변수가 있는지 확인
!echo %environmental_variable_name%

# 변수 로드하기
api_key = os.getenv(environmental_variable_name)
```

# OpenAI API에 연결

```js
import os
import openai
from openai import OpenAI

gpt_api_key = os.getenv(api_key)
client = OpenAI(
    api_key = gpt_api_key,
)
```

# 사용자 지정 프롬프트와 프롬프트 응답을 저장할 데이터 프레임 생성하기

<div class="content-ad"></div>


```js
import pandas as pd

# load dictionary
data = {
  'weekday': ['일요일', '월요일', '화요일', '수요일', '목요일', '금요일', '토요일'],
  'mood': ['행복한', '신나서 불안한', '금요일 준비된', '피곤한', '행복한', '슬픈', '전문적인'],
  'topic': ['영화', '일', '일', '일', '일', '음주', '여행']
}

# data into pandas df
df = pd.DataFrame(data)
```

# Generate Prompt

```js
prompt = (f"""
              작은 블로그를 운영 중인데 팔로워들에게 제가 무엇을 생각하고 있는지 한 문장 메시지를 게시합니다.
              오늘 아침 포스트를 작성하는 데 도와주세요. 포스트에는 요일이 {weekday}인 것을 포함해주세요.
              제 기분은 {mood}이니 글에 그 감정을 잘 나타내고 주제는 {topic}에 관한 것으로 해주세요.
              감사합니다, 최고에요!
            """)
```

# Gather Prompt Responses


<div class="content-ad"></div>

```js
# 응답을 추가할 빈 열
df['메시지'] = 'na'

# 데이터프레임에 프롬프트 응답 반복적으로 추가
for index, row in df.iterrows():
    # 열에서 콘텐츠 추출
    weekday = row['weekday']
    mood = row['mood']
    topic = row['topic']

    prompt_api = prompt

    response = client.chat.completions.create(
        model="gpt-3.5-turbo",
        messages=[
          {"role": "system", "content": "You are a numbers minded assistant"},
          {"role": "user", "content": prompt_api},
        ]
      )

    # 여기는 원시적인 gpt 응답이며 매우 불일치하기 때문에 나이만 추출해야 함
    response = response.choices[0].message.content

    print(response)
    df.at[index, '메시지'] = response
```

# 응답

안녕하세요, 행복한 일요일입니다! 오늘은 영화의 마법 세계에 푹 빠져 기분이 더 좋아요. 🎥 오늘 영화 여행 즐기세요! #일요일즐거움 #영화이야기

모두들 행복한 월요일! 일의 새로운 기회를 품고 새로운 주간에 들어가면서 설레고 긴장되는 기분이에요. 💼 #월요일돋보기


<div class="content-ad"></div>

## 즐거운 화요일! 이미 금요일을 생각하며 기분 좋네요... 생산성으로 가득 찬 저의 일꾼 정신이 박자를 맞추고 있나 봐요! 💼 #일하는모드

## 즐거운 수요일, 오늘은 피곤하긴 한데 일하는 그 나날을 이겨내고 있어요 — 성공은 종종 피로와 함께 오는 것을 상기시킵니다.

## 모두들 즐거운 목요일! 오늘은 제가 하는 일에 대해 감사하게 느껴지고, 일이 가져다주는 기회에 대해 감사합니다. 목표를 향해 노력하고, 열심히 일하는 것이 항상 보람을 주는 것을 기억하세요! 💼✨

## 금요일 아침에 한 주의 무게를 느끼고, 잠시 와인 한 잔으로 잠시 위로를 받아볼까 고민 중이에요.

<div class="content-ad"></div>

좋은 아침, 행복한 토요일이에요! 오늘은 전문성과 동경심의 시각을 통해 세계를 탐험해봐요. 여행이 가진 변화력을 느껴보며! ✈️ #주말동경

# 결론

이 코드를 사용하여 ChatGPT를 쉽게 파이썬 워크플로우에 통합하세요. 매일 아침 블로그를 운영 중이라면 — 환영합니다.

더 많은 유용한 코드 팁은 여기에서 찾을 수 있어요.