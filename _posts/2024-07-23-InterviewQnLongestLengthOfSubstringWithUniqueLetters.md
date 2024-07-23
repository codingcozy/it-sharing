---
title: "면접 질문 고유한 문자로 이루어진 가장 긴 부분 문자열 찾기 방법"
description: ""
coverImage: "/assets/img/2024-07-23-InterviewQnLongestLengthOfSubstringWithUniqueLetters_0.png"
date: 2024-07-23 21:59
ogImage: 
  url: /assets/img/2024-07-23-InterviewQnLongestLengthOfSubstringWithUniqueLetters_0.png
tag: Tech
originalTitle: "Interview Qn Longest Length Of Substring With Unique Letters"
link: "https://medium.com/gitconnected/interview-qn-longest-length-of-substring-with-unique-letters-434afd67dea4"
---


<img src="/assets/img/2024-07-23-InterviewQnLongestLengthOfSubstringWithUniqueLetters_0.png" />

저는 전에 만난 진짜 코딩 면접 문제에 대해 이야기하려고 해요.

주어진 문자열 `abaabcabaaax` 와 같이 문자열이 있다면, 오직 고유한 문자만 포함하는 가장 긴 부분 문자열의 길이를 찾아야 합니다.

- 가장 긴 부분 문자열은 abc 또는 bca 또는 cab 중 하나입니다.
- 우리는 길이에만 관심이 있으며, 그 길이는 3입니다.

<div class="content-ad"></div>

# 당신을 위한 몇 가지 테스트 케이스

```js
def longest_substring_length(string: str) -> int:
    """
    구현해야 함
    """

testcases = [
    ('', 0),
    ('a', 1),
    ('aa', 1),
    ('aaa', 1),
    ('aaabbb', 2),
    ('ababababbababa', 2),
    ('aabbccddeeffgg', 2),
    ('aaaaaaabbbbbbbbcccc', 2),
    ('abaaaabcabaaax', 3),
    ('abcbabcababcacbacab', 3),
    ('abababcdbbcc', 4),
    ('abcdcbabcdcba', 4),
    ('bbbabcdeabcdeddd', 5),
    ('abcdef', 6)
]

for string, answer in testcases:
    guess = longest_substring_length(string)
    print(string, answer, guess)
```

이것을 시도하는 데 시간을 내어주세요!!

# 잠시 멈춤

<div class="content-ad"></div>

최근 Python, 프로그래밍, 그리고 많은 사람들이 알지 못하는 Python 관련 랜덤 팩트에 대한 Substack 뉴스레터를 시작했어요. 함께해요!

제 Substack: [https://zlliu.substack.com](https://zlliu.substack.com)

또한 최근에 책 101 Things I Never Knew About Python을 썼어요. 제가 더 이른 시기부터 배울 수 있었을 Python 관련 이야기들을 자세히 다루었어요.

여기서 확인해보세요: [https://payhip.com/b/vywcf](https://payhip.com/b/vywcf)

<div class="content-ad"></div>

# 이 문제를 해결하는 일반적인 아이디어

이 문제를 O(n)의 선형 시간으로 해결해 봅시다.

그리고 O(n)의 시간으로 이 문제를 해결하기 위해서는 문자열을 왼쪽에서 오른쪽으로 순회해야 합니다.

- left 포인트 left와 right 포인터 right를 유지합니다
- right - left + 1은 가장 긴 부분 문자열의 길이입니다
- 각 반복마다, left와 right를 업데이트하고, 가장 긴 길이를 추적합니다.

<div class="content-ad"></div>

표의 태그를 Markdown 형식으로 변경해 주세요.

<div class="content-ad"></div>

아래의 표 태그를 Markdown 형식으로 변경하세요.

<div class="content-ad"></div>


![image](/assets/img/2024-07-23-InterviewQnLongestLengthOfSubstringWithUniqueLetters_3.png)

이제 문자가 반복됩니다. 우리의 부분 문자열은 aba로, 이는 잘못된 부분 문자열입니다.

- 사전에서 a의 값을 업데이트합니다.
- 가능하다면 마지막 a의 앞에있는 문자를 가리키는 left를 증가시킵니다.

![image](/assets/img/2024-07-23-InterviewQnLongestLengthOfSubstringWithUniqueLetters_4.png)


<div class="content-ad"></div>

다음으로, 우리는 오른쪽을 다시 1만큼 증가시킵니다:

![image](/assets/img/2024-07-23-InterviewQnLongestLengthOfSubstringWithUniqueLetters_5.png)

그리고 다시 다른 'a'를 발견합니다. 이제 우리의 사전과 왼쪽을 업데이트할 때입니다.

![image](/assets/img/2024-07-23-InterviewQnLongestLengthOfSubstringWithUniqueLetters_6.png)

<div class="content-ad"></div>

다음으로, 다시 오른쪽으로 1씩 증가합니다:


<img src="/assets/img/2024-07-23-InterviewQnLongestLengthOfSubstringWithUniqueLetters_7.png" />


이제 우리가 또 다른 b를 만나게 됩니다. 우리의 dict와 left를 업데이트하는 시간입니다.


<img src="/assets/img/2024-07-23-InterviewQnLongestLengthOfSubstringWithUniqueLetters_8.png" />


<div class="content-ad"></div>

우리는 b의 값을 4로 업데이트했습니다.

원래는 left를 2로 업데이트하려고 했지만, 이미 left가 그보다 크기 때문에 left를 그대로 두기로 결정했습니다.

다음으로 right를 다시 1 증가시켰습니다:

<div class="content-ad"></div>

다음 표식이나 목록을 마크다운 형식으로 전환해주세요.

<div class="content-ad"></div>

우리는 또 다른 a를 만났어요. 

이제 left와 우리의 딕셔너리를 업데이트해야 해요.

![이미지](/assets/img/2024-07-23-InterviewQnLongestLengthOfSubstringWithUniqueLetters_11.png)

left는 이제 마지막 a 다음의 b를 가리키고 있어요.

<div class="content-ad"></div>

그리고 우리는 사전에서 a의 값을 업데이트합니다.

다음으로, right를 다시 1씩 증가시키겠습니다:

![이미지](/assets/img/2024-07-23-InterviewQnLongestLengthOfSubstringWithUniqueLetters_12.png)

또 다른 b를 만나게 되었군요. 그리고 나서 left와 우리의 사전을 업데이트합니다.

<div class="content-ad"></div>

아래와 같이 Markdown 형식으로 수정해주세요.


<img src="/assets/img/2024-07-23-InterviewQnLongestLengthOfSubstringWithUniqueLetters_13.png" />

다음으로, right를 다시 1씩 증가시켜요:

<img src="/assets/img/2024-07-23-InterviewQnLongestLengthOfSubstringWithUniqueLetters_14.png" />

또 다시 a가 반복돼요. left와 딕셔너리를 다시 업데이트해요


<div class="content-ad"></div>

다음으로, 우리는 오른쪽을 다시 1 증가시킬 거에요:

<img src="/assets/img/2024-07-23-InterviewQnLongestLengthOfSubstringWithUniqueLetters_16.png" />

a를 반복해봐요. 우리는 이해하죠. 왼쪽을 업데이트하고 우리의 딕셔너리를 갱신해요

<div class="content-ad"></div>

다음으로 오른쪽을 다시 1씩 증가시킵니다:

![image1](/assets/img/2024-07-23-InterviewQnLongestLengthOfSubstringWithUniqueLetters_18.png)

a. 동일한 작업을 반복합니다. 왼쪽을 업데이트하고 사전을 갱신합니다.

<div class="content-ad"></div>

아래 표를 Markdown 형식으로 변경해주세요.

<div class="content-ad"></div>

이 과정 동안에 우리는 가능한 가장 긴 부분 문자열의 길이도 추적합니다. 그리고 마지막에 이 값을 반환합시다.

# 코드 솔루션

```js
def longest_substring_length(string: str) -> int:

    # 만약 문자열의 길이가 0이거나 1이라면, 답을 반환하세요
    if len(string) <= 1:
        return len(string)

    # 우리의 딕셔너리를 초기화합니다
    d = {string[0]:0}

    # 왼쪽과 오른쪽 포인터를 설정합니다
    l = r = 0

    # 이 변수는 유효한 부분 문자열의 가장 긴 길이를 추적합니다
    largest = 0

    while True:
        # 오른쪽을 1씩 증가시킵니다
        r += 1
    
        # 만약 범위를 벗어나면, 종료합니다
        if r >= len(string): break

        # 현재 문자를 추출합니다
        ch = string[r]

        
        if ch not in d:
            # 만약 우리의 딕셔너리에 없는 새로운 문자를 만나면
            d[ch] = r
        else:
            # 반복된 문자를 만나면
            l = max(d[ch] + 1, l)
            d[ch] = r

        # 가장 긴 부분 문자열을 추적합니다
        if largest < r-l+1:
            largest = r-l+1

    return largest

testcases = [
    ('', 0),
    ('a', 1),
    ('aa', 1),
    ('aaa', 1),
    ('aaabbb', 2),
    ('ababababbababa', 2),
    ('aabbccddeeffgg', 2),
    ('aaaaaaabbbbbbbbcccc', 2),
    ('abaaaabcabaaax', 3),
    ('abcbabcababcacbacab', 3),
    ('abababcdbbcc', 4),
    ('abcdcbabcdcba', 4),
    ('bbbabcdeabcdeddd', 5),
    ('abcdef', 6)
]

for string, answer in testcases:
    guess = longest_substring_length(string)
    print(string, answer, guess)
```

결과:

<div class="content-ad"></div>


```js
 0 0
a 1 1
aa 1 1
aaa 1 1
aaabbb 2 2
ababababbababa 2 2
aabbccddeeffgg 2 2
aaaaaaabbbbbbbbcccc 2 2
abaaaabcabaaax 3 3
abcbabcababcacbacab 3 3
abababcdbbcc 4 4
abcdcbabcdcba 4 4
bbbabcdeabcdeddd 5 5
abcdef 6 6
``` 
# 결론

이해하기 쉽고 명확했으면 좋겠습니다.

# 만약 크리에이터로서 저를 지원하고 싶다면

<div class="content-ad"></div>

- 제 Substack 뉴스레터에 가입해 주세요. https://zlliu.substack.com/ 에서 확인 가능합니다. 제가 파이썬과 관련된 주간 이메일을 보내드립니다.
- 제 책인 101 Things I Never Knew About Python을 구매해 보세요. https://payhip.com/b/vywcf 에서 구매 가능합니다.
- 이 이야기에 대해 50번 박수를 치세요.
- 여러분의 생각을 나에게 말씀해 주세요. 댓글을 달아주세요.
- 이 이야기 중 마음에 드는 부분을 강조해 주세요.

감사합니다! 이런 작은 행동들이 큰 힘이 됩니다. 정말 감사드립니다!

YouTube: https://www.youtube.com/@zlliu246

LinkedIn: https://www.linkedin.com/in/zlliu/