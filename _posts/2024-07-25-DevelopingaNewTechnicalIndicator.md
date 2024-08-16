---
title: "새로운 기술 지표 개발 방법"
description: ""
coverImage: "/assets/img/2024-07-25-DevelopingaNewTechnicalIndicator_0.png"
date: 2024-07-25 12:07
ogImage: 
  url: /assets/img/2024-07-25-DevelopingaNewTechnicalIndicator_0.png
tag: Tech
originalTitle: "Developing a New Technical Indicator"
link: "https://medium.com/@kaabar-sofien/developing-a-new-technical-indicator-6b22943c04e3"
isUpdated: true
updatedAt: 1723816804222
---



![그림](/assets/img/2024-07-25-DevelopingaNewTechnicalIndicator_0.png)

커스텀 기술 지표는 시장을 다른 관점에서 살펴보는 데 자주 사용되는 간단한 계산입니다. 일반적으로 기술 분석가들은 이미 가치가 입증된 클래식 인디케이터인 RSI와 이동평균을 사용합니다. 그러나 항상 편안한 영역을 벗어나는 것이 흥미로울 수 있습니다.

이 글은 새로운 기술 지표에 대한 간단한 아이디어와 활용 방법, 그리고 TradingView에서 어떻게 코딩하는지를 소개합니다.

# K’s Reversal Accelerator

<div class="content-ad"></div>

이 지표의 아이디어는 간단합니다. 일정 기간 동안 양봉의 수를 전체 캔들스틱 수로 나누는 것입니다. 이를 통해 0%와 100% 사이로 자연스럽게 제한된 지표를 얻을 수 있습니다 (100%가 모두 양봉이 발생하는 지역을 의미합니다).

기본 롤링 윈도우는 30입니다. 따라서, 최근 30개의 캔들스틱이 모두 음봉인 경우(종가가 시가보다 낮은 경우), 지표는 0%가 됩니다.

다음 차트를 살펴보세요.

해당 지표가 0%부터 100% 사이에서 변동하는 모습을 볼 수 있습니다. 실제 경계 값으로 70과 30을 사용하는 것은 RSI의 기본 경계 값을 기억해보세요.

<div class="content-ad"></div>

내 작품을 더 보고 싶다면, 아래 사진에 첨부된 링크를 따라서 PDF 도서 목록 카탈로그를 확인할 수 있어요:

![PDF books catalogue](/assets/img/2024-07-25-DevelopingaNewTechnicalIndicator_1.png)

# 코드로 지표 코딩하기 및 사용 방법

다음 코드를 사용하여 지표를 코딩할 수 있어요:

<div class="content-ad"></div>

```js
//@version=5
indicator("K의 V 역전 가속기 - 패널", overlay=false)

// 이동 윈도우의 길이에 대한 입력
rolling_window_length = input.int(30, minval=1, title="이동 윈도우 길이")

// 상승 캔들스틱 정의: 종가 > 시가
is_bullish = close > open

// 상승 캔들스틱의 이동 개수 계산
bullish_count = math.sum(is_bullish ? 1 : 0, rolling_window_length)

// 상승 캔들스틱의 백분율 계산
bullish_percentage = bullish_count * 100 / rolling_window_length

plot(bullish_percentage)
hline(30)
hline(70)
```

이 인디케이터를 사용하는 방법은 다음과 같습니다. 인디케이터가 초과매도 수준 (30)을 넘은 후에 아래에 있었을 때 또는 인디케이터가 초과매수 수준 (70)을 넘은 후에 위에 있을 때입니다.

다음 신호 차트를 살펴보세요.

이 인디케이터를 사용하는 기본 기술은 다음과 같습니다.


<div class="content-ad"></div>

- 인디케이터가 30을 초과하지만 40 미만으로 유지될 때마다 상승 신호가 생성됩니다.
- 인디케이터가 70을 돌파하지만 60 위로 유지될 때마다 하락 신호가 생성됩니다.

![Img](/assets/img/2024-07-25-DevelopingaNewTechnicalIndicator_2.png)

다음 코드를 사용하여 기법에서 신호를 받으세요:

```js
//@version=5
indicator("K's V Reversal Accelerator", overlay = true)

// 이동 창 길이에 대한 입력
rolling_window_length = input.int(30, minval = 1, title = "Rolling Window Length")

// 상승 캔들스틱 정의: 종가 > 시가
is_bullish = close > open

// 상승 캔들스틱의 이동 카운트 계산
bullish_count = math.sum(is_bullish ? 1 : 0, rolling_window_length)

// 상승 캔들스틱의 백분율 계산
bullish_percentage = bullish_count * 100 / rolling_window_length

buy_signal  = bullish_percentage >= 30 and bullish_percentage < 40 and bullish_percentage[1] < 30
sell_signal = bullish_percentage <= 70 and bullish_percentage > 60 and bullish_percentage[1] > 70

plotshape(buy_signal,  style = shape.triangleup,   color = color.green,  location =  location.belowbar, size = size.small)
plotshape(sell_signal,  style = shape.triangledown, color = color.red,    location =  location.abovebar, size = size.small)
```

<div class="content-ad"></div>

다음 차트는 지표에 의해 생성된 더 많은 신호를 보여줍니다.

여기 있습니다, 복잡하지 않은 가벼운 지표가 있습니다. 그러나 기술 지표에도 한계가 있다는 점, 또한 참 성공은 여러분과 여러분의 마인드에 달려 있음을 이해해야 합니다. 현재 시장 규칙과 기본에 대해 인식이 없는 단순한 수학적 공식으로만은 성공할 수 없다는 것을 이해해야 합니다.