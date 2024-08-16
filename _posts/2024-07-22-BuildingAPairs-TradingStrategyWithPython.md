---
title: "파이썬으로 페어 트레이딩 개발하는 방법 정리"
description: ""
coverImage: "/assets/img/2024-07-22-BuildingAPairs-TradingStrategyWithPython_0.png"
date: 2024-07-22 11:35
ogImage: 
  url: /assets/img/2024-07-22-BuildingAPairs-TradingStrategyWithPython_0.png
tag: Tech
originalTitle: "Building A Pairs-Trading Strategy With Python"
link: "https://medium.com/gitconnected/building-a-pairs-trading-strategy-with-python-c27a57732c30"
isUpdated: true
updatedAt: 1723812573309
---



페어 트레이딩은 양적 거래자들이 종종 개별 자산에 집중하는 대신 포트폴리오를 관리하는 데 사용하는 정교한 접근 방식입니다. 본 문서에서는 두 가지 밀접한 관련 금융 상품의 상대 가격 변동에서 이윤을 얻기 위해 설계된 시장 중립 전략의 분석을 탐색할 것입니다. 이 분석을 위해 자유로운 접근이 가능한 Yahoo Finance 데이터를 활용한 시뮬레이션을 진행할 것입니다.

이 전략의 핵심은 시간이 흐름에 따라 두 자산 간의 가격 간격이 안정적으로 유지될 것이라는 믿음에 기반하고 있습니다. 결과적으로 거래자들은 종합적인 시장 동향과 관계없이 가격이 수렴함으로써 수익을 얻을 수 있습니다. 페어 트레이딩은 선택한 자산 간의 관계를 대상으로 하며 종합적인 시장 방향보다는 관련성에 초점을 맞춥니다.

시작하기 전에, 저의 배당 투자 커뮤니티에 참여하시라는 초대를 드립니다. 가입하시면 중요한 기사를 놓치지 않을 뿐만 아니라 투자자로서 기술을 향상시킬 수도 있습니다. 보너스로 2024년 최신 거래 전략이 담긴 모델북(옵션 및 배당 포함)을 무료로 받으실 수 있습니다.

# 환경 설정하기

<div class="content-ad"></div>

필요한 라이브러리를 Jupyter 환경에 설치하려면 다음 명령어를 사용하세요:

```js
!pip install numpy
!pip install pandas
!pip install yfinance
```

이 명령어들을 Jupyter Notebook 셀에서 실행하여 지정된 라이브러리를 직접 환경에 설치할 수 있습니다. 설치가 완료되면 분석을 진행할 수 있습니다.

# 데이터 수집

<div class="content-ad"></div>

가상화폐를 이용한 페어 트레이딩 전략에 대한 데이터 수집 및 산정 구현을 위해서는 다음 단계를 따르세요:

- 누락된 값 보간: 데이터의 간격을 채워 연속성을 유지합니다.
- 이상값 평활화: 분석 왜곡을 피하기 위해 극단적인 데이터 포인트를 평활화하는 방법을 적용합니다.
- 일관된 시계열 길이 보장: 가상화폐 시장의 지속적인 거래 활동에도 불구하고 일관된 길이의 시계열 데이터를 표준화합니다.

Python에서의 구현은 다음과 같습니다:

```python
crypto_forex_stocks = ['BTC-USD', 'ETH-USD', 'BNB-USD', 'XRP-USD', 'ADA-USD', 'DOGE-USD', 'ETC-USD', 'XLM-USD', 'AAVE-USD', 'EOS-USD', 'XTZ-USD', 'ALGO-USD', 'XMR-USD', 'KCS-USD',
                       'MKR-USD', 'BSV-USD', 'RUNE-USD', 'DASH-USD', 'KAVA-USD', 'ICX-USD', 'LINA-USD', 'WAXP-USD', 'LSK-USD', 'EWT-USD', 'XCN-USD', 'HIVE-USD', 'FTX-USD', 'RVN-USD', 'SXP-USD', 'BTCB-USD']
bank_stocks = ['JPM', 'BAC', 'WFC', 'C', 'GS', 'MS', 'DB', 'UBS', 'BBVA', 'SAN', 'ING', ' BNPQY', 'HSBC', 'SMFG', 'PNC', 'USB', 'BK', 'STT', 'KEY', 'RF', 'HBAN', 'FITB',  'CFG',
               'BLK', 'ALLY', 'MTB', 'NBHC', 'ZION', 'FFIN', 'FHN', 'UBSI', 'WAL', 'PACW', 'SBCF', 'TCBI', 'BOKF', 'PFG', 'GBCI', 'TFC', 'CFR', 'UMBF', 'SPFI', 'FULT', 'ONB', 'INDB', 'IBOC', 'HOMB']
global_indexes = ['^DJI', '^IXIC', '^GSPC', '^FTSE', '^N225', '^HSI', '^AXJO', '^KS11', '^BFX', '^N100',
                  '^RUT', '^VIX', '^TNX']

START_DATE = '2021-01-01'
END_DATE = '2023-10-31'
universe_tickers = crypto_forex_stocks + bank_stocks + global_indexes
universe_tickers_ts_map = {ticker: load_ticker_ts_df(
    ticker, START_DATE, END_DATE) for ticker in universe_tickers}

def sanitize_data(data_map):
    TS_DAYS_LENGTH = (pd.to_datetime(END_DATE) -
                      pd.to_datetime(START_DATE)).days
    data_sanitized = {}
    date_range = pd.date_range(start=START_DATE, end=END_DATE, freq='D')
    for ticker, data in data_map.items():
        if data is None or len(data) < (TS_DAYS_LENGTH / 2):
            # We cannot handle shorter TSs
            continue
        if len(data) > TS_DAYS_LENGTH:
            # Normalize to have the same length (TS_DAYS_LENGTH)
            data = data[-TS_DAYS_LENGTH:]
        # Reindex the time series to match the date range and fill in any blanks (Not Numbers)
        data = data.reindex(date_range)
        data['Adj Close'].replace([np.inf, -np.inf], np.nan, inplace=True)
        data['Adj Close'].interpolate(method='linear', inplace=True)
        data['Adj Close'].fillna(method='pad', inplace=True)
        data['Adj Close'].fillna(method='bfill', inplace=True)
        assert not np.any(np.isnan(data['Adj Close'])) and not np.any(
            np.isinf(data['Adj Close']))
        data_sanitized[ticker] = data
    return data_sanitized

# 샘플 데이터 보기
uts_sanitized = sanitize_data(universe_tickers_ts_map)
uts_sanitized['JPM'].shape, uts_sanitized['BTC-USD'].shape
```

<div class="content-ad"></div>

`date_range` 변수는 'pd.date_range(start=START_DATE, end=END_DATE, freq=’D’)'로 정의되어 있어 데이터의 원하는 시간 범위를 설정합니다. 다음으로 선형 보간을 사용하여 누락된 값(NaN 또는 None)을 채우거나, 실패하는 경우 최근 유효한 값으로 채워넣습니다.

데이터 무결성을 확인하기 위해 assert 문을 사용하고, 두 가지 무작위로 선택한 기기의 모양을 확인하여 그들이 일치하는 차원임을 확인합니다.

# 깊게 들어가보기

분석에서 예시로 최근 FTX 스캔들에 대해 자세히 살펴봅시다. 이 거래소의 붕괴로 인해 투자자들은 암호화폐 거래소에 대한 신뢰를 잃고, 이 사건이 잊힐 때까지 임시로 전통 은행으로 금융 거래를 이전할 수 있습니다.

<div class="content-ad"></div>

이 가설을 탐색하기 위해 암호화폐 시장과 전통 금융 부문의 성과 사이의 패턴 또는 관계를 식별하기 위해 상관 및 공적분석을 사용할 수 있습니다. 상관 분석은 두 데이터 세트 사이의 선형 관계 정도를 이해하는 데 도움이 되고, 공적분석은 두 섹터 간에 장기적 관계가 있는지를 결정하여 잠재적인 페어 트레이딩 기회를 시사할 수 있습니다.

이러한 지표를 검토함으로써 최근 사건이 암호화폐 및 전통 은행 부문에 대한 시장 심리에 영향을 미쳤는지를 평가할 수 있습니다.

# 상관관계와 공적분석

상관관계는 피어슨 상관계수(r)를 사용하여 두 변수 간의 관계를 측정하며, 값은 -1에서 1까지 변합니다. -1의 값은 완벽한 음적 선형 관계를 나타내고, 0은 선형 관계가 없음을 의미하며, 1은 완벽한 양적 선형 관계를 나타냅니다.

<div class="content-ad"></div>


![Screenshot 1](/assets/img/2024-07-22-BuildingAPairs-TradingStrategyWithPython_0.png)

Cointegration, in contrast, assesses whether two assets are linked over time, indicating that their price spreads tend to revert to the mean. This creates opportunities for trading when they temporarily diverge from their historical relationship. Cointegration is evaluated using statistical tests like the Augmented Dickey-Fuller (ADF) test, which examines whether the spread between the two assets is stationary. If the spread is stationary, it suggests that the assets are cointegrated and share a long-term relationship.

![Screenshot 2](/assets/img/2024-07-22-BuildingAPairs-TradingStrategyWithPython_1.png)

Fortunately, the numpy and stats libraries offer functions that simplify these statistical tests, making it easier to perform correlation and cointegration analysis.


<div class="content-ad"></div>

# 쌍 찾기

양적 거래에서는, 분석가들은 자산 간의 스프레드가 역사적 평균을 벗어났을 때 매수 및 매도 신호를 생성하기 위해 공적 분석을 사용합니다. 이러한 이상 현상은 스프레드가 장기 평형으로 복귀함에 따라 수익 기회를 제공합니다. 따라서, 이 분석을 위해 철저한 데이터가 필수적입니다.

아래 코드는 다양한 주식 및 다른 금융 상품을 조사하여 숨겨진 관계를 발견할 것입니다. 이 코드는 상관 관계가 없거나 영향이 없다는 귀무 가설(H0)을 테스트할 것입니다. 보통 시험에서의 p-값이 0.02보다 낮으면, 귀무 가설(H0)을 기각하여 이 쌍이 어느 정도의 공적 통합을 보여주거나 더 심층적인 조사가 필요한 관계를 보여줍니다.

```js
from statsmodels.tsa.stattools import coint
from itertools import combinations
from statsmodels.tsa.stattools import coint

def find_cointegrated_pairs(tickers_ts_map, p_value_threshold=0.2):
    """
    수정된 디키-풀러(ADF) 테스트를 기반으로 한 주식 쌍의 공적 통합을 찾습니다.
    Parameters:
    - tickers_ts_map (dict): 키가 주식 티커이고 값이 시계열 데이터인 사전.
    - p_value_threshold (float): 공적 통합 테스트의 유의 수준.
    Returns:
    - pvalue_matrix (numpy.ndarray): 주식 쌍 간의 공적 통합 p-값을 나타내는 행렬.
    - pairs (list): 공적 통합 주식 쌍 및 그들의 p-값을 나타내는 튜플 목록.
    """
    tickers = list(tickers_ts_map.keys())
    n = len(tickers)
    # '수정 종가'를 행렬로 추출합니다 (각 열은 시계열)
    adj_close_data = np.column_stack(
        [tickers_ts_map[ticker]['Close'].values for ticker in tickers])
    pvalue_matrix = np.ones((n, n))
    # 유일한 쌍 조합에 대해 공적 통합 p-값 계산
    for i, j in combinations(range(n), 2):
        result = coint(adj_close_data[:, i], adj_close_data[:, j])
        pvalue_matrix[i, j] = result[1]
    pairs = [(tickers[i], tickers[j], pvalue_matrix[i, j])
             for i, j in zip(*np.where(pvalue_matrix < p_value_threshold))]
    return pvalue_matrix, pairs

# 이 섹션은 최대 5분이 소요될 수 있습니다
P_VALUE_THRESHOLD = 0.02
pvalues, pairs = find_cointegrated_pairs(
    uts_sanitized, p_value_threshold=P_VALUE_THRESHOLD)
```

<div class="content-ad"></div>

자산 간의 관계를 시각화하는 것은 알고리즘 거래에서도 인간이 이해하고 의사결정을 하는 데 중요합니다. 열 지도는 코인티그레이션 테스트에서 얻은 p-값을 기반으로 어떤 자산이 연관되어 있는지 시각적으로 보여줄 수 있습니다. 이 열 지도는 중요한 관계가 있는 잠재적인 쌍을 식별하여 추가 분석 및 거래 기회를 찾는 데 도움이 될 것입니다.

열 지도를 생성하고 표시하는 방법은 다음과 같습니다:

```js
import seaborn as sns

plt.figure(figsize=(26, 26))
heatmap = sns.heatmap(pvalues, xticklabels=uts_sanitized.keys(),
                      yticklabels=uts_sanitized.keys(), cmap='RdYlGn_r',
                      mask=(pvalues > (P_VALUE_THRESHOLD)),
                      linecolor='gray', linewidths=0.5)
heatmap.set_xticklabels(heatmap.get_xticklabels(), size=14)
heatmap.set_yticklabels(heatmap.get_yticklabels(), size=14)
plt.show()
```

<img src="/assets/img/2024-07-22-BuildingAPairs-TradingStrategyWithPython_2.png" />

<div class="content-ad"></div>

우리의 분석을 간소화하고 암호화폐 간의 가장 강력한 관계에 집중하기 위해, 가장 낮은 p-값을 가진 상위 세 쌍을 선택할 수 있습니다. 이러한 쌍들은 가장 강력한 공적통합을 나타내며 따라서 거래 기회로 이어질 수 있습니다. 이러한 쌍과 각각의 p-값을 막대 차트를 사용하여 시각화할 수 있습니다.

다음과 같이 구현할 수 있습니다:

- 상위 세 쌍 추출:
- P-값을 시각화하기 위한 막대 차트 그리기:

```js
sorted_pairs = sorted(pairs, key=lambda x: x[2], reverse=False)
sorted_pairs = sorted_pairs[0:35]
sorted_pairs_labels, pairs_p_values = zip(
    *[(f'{y1} <-> {y2}', p*1000) for y1, y2, p in sorted_pairs])
plt.figure(figsize=(12, 18))
plt.barh(sorted_pairs_labels,
         pairs_p_values, color='red')
plt.xlabel('P-Values (1000)', fontsize=8)
plt.ylabel('Pairs', fontsize=6)
plt.title('Cointegration P-Values (in 1000s)', fontsize=20)
plt.grid(axis='both', linestyle='--', alpha=0.7)
plt.show()
```

<div class="content-ad"></div>


![image](/assets/img/2024-07-22-BuildingAPairs-TradingStrategyWithPython_3.png)

특정 반대 매매를 위해 식별된 페어에 대한 시계열 데이터를 시각화하기 위해 AAVE-USD 및 시티그룹 ©, XMR-USD 및 시티그룹 ©, FTX-USD 및 Ally Financial Inc (ALLY)의 페어를 사용하고, 암호화폐와 주식 간의 비교를 용이하게 하기 위해 scikit-learn의 MinMax 스케일링을 사용합니다. 또한 페어들 간의 정상성을 개선하기 위해 롤링 윈도우를 사용하여 부드럽게 처리합니다.

## 이렇게 구현할 수 있습니다:

```python
from sklearn.preprocessing import MinMaxScaler

ticker_pairs = [("AAVE-USD", "C"), ("XMR-USD", "C"), ("FTX-USD", "ALLY")]
fig, axs = plt.subplots(3, 1, figsize=(18, 14))
scaler = MinMaxScaler()
for i, (ticker1, ticker2) in enumerate(ticker_pairs):
    # MIN MAX를 사용하여 각 페어에 대한 가격 데이터 스케일링
    scaled_data1 = scaler.fit_transform(
        uts_sanitized[ticker1]['Adj Close'].values.reshape(-1, 1))
    scaled_data2 = scaler.fit_transform(
        uts_sanitized[ticker2]['Adj Close'].values.reshape(-1, 1))
    axs[i].plot(scaled_data1, label=f'{ticker1}', color='lightgray', alpha=0.7)
    axs[i].plot(scaled_data2, label=f'{ticker2}', color='lightgray', alpha=0.7)
    # 윈도우 크기 15로 롤링 평균 적용
    scaled_data1_smooth = pd.Series(scaled_data1.flatten()).rolling(
        window=15, min_periods=1).mean()
    scaled_data2_smooth = pd.Series(scaled_data2.flatten()).rolling(
        window=15, min_periods=1).mean()
    axs[i].plot(scaled_data1_smooth, label=f'{ticker1} SMA', color='red')
    axs[i].plot(scaled_data2_smooth, label=f'{ticker2} SMA', color='blue')
    axs[i].set_ylabel('*Scaled* Price $', fontsize=12)
    axs[i].set_title(f'{ticker1} vs {ticker2}', fontsize=18)
    axs[i].legend()
    axs[i].set_xticks([])
plt.tight_layout()
plt.show()
``` 


<div class="content-ad"></div>

<img src="/assets/img/2024-07-22-BuildingAPairs-TradingStrategyWithPython_4.png" />

아래는 제공된 텍스트의 의역 버전입니다:

AAVE-USD와 시티그룹 주식 사이의 잠재적 거래 신호를 탐색하기 위해, 초기 시리즈의 불일치에도 불구하고 그들의 가격이 상대적으로 안정적임을 관찰합니다.

거래 신호를 생성하기 위해, 우리는 롤링 윈도우 방식의 Z-점수 방법을 사용할 것이며, 별도의 훈련 및 테스트 세트가 필요 없습니다. Z-점수는 가격 시리즈를 해당 과거 평균에 상대적으로 표준화합니다.

<div class="content-ad"></div>


![이미지](/assets/img/2024-07-22-BuildingAPairs-TradingStrategyWithPython_5.png)

## 어디에서:

Z-점수는 현재 가격 비율이 과거 평균에서 얼마나 벗어나 있는지를 측정합니다. Z-점수가 +1 이상 또는 -1 미만이면 일반적으로 거래 신호가 발생합니다. +1 이상의 Z-점수는 한 자산이 다른 자산 대비 과대평가되었음을 나타내며, 이는 과대평가된 자산에 대한 매도 신호와 저평가된 자산에 대한 매수 신호를 의미합니다.

반대로, -1 미만의 Z-점수는 저평가된 자산이 과도하게 평가되었음을 나타내며, 이는 전자에 대한 매도와 후자에 대한 매수를 시그널합니다. 이 전략은 임시적인 이견 및 기대되는 과거 평균 수익을 활용하는 평균 회귀 동적을 활용합니다.


<div class="content-ad"></div>

```js
# TRAIN과 TEST를 계산합니다.
TRAIN = int(len(uts_sanitized["AAVE-USD"]) * 0.85)
TEST = len(uts_sanitized["AAVE-USD"]) - TRAIN

# AAVE와 C의 Adjusted Close 데이터를 가져옵니다.
AAVE_ts = uts_sanitized["AAVE-USD"]["Adj Close"][:TRAIN]
C_ts = uts_sanitized["C"]["Adj Close"][:TRAIN]

# 가격 비율 계산 (AAVE-USD 가격 / C 가격)
ratios = C_ts / AAVE_ts

# 그래프 초기화
fig, ax = plt.subplots(figsize=(12, 8))

# 가격 비율의 평균과 표준편차 계산
ratios_mean = np.mean(ratios)
ratios_std = np.std(ratios)

# Z-Score 계산
ratios_zscore = (ratios - ratios_mean) / ratios_std

# Z-Score 그래프로 표시
ax.plot(ratios.index, ratios_zscore, label="Z-Score", color='blue')

# 기준선 그리기
ax.axhline(1.0, color="green", linestyle='--', label="상한선 (1.0)")
ax.axhline(-1.0, color="red", linestyle='--', label="하한선 (-1.0)")
ax.axhline(0, color="black", linestyle='--', label="평균")

# 그래프 제목과 레이블 설정
ax.set_title('AAVE-USD / C: 가격 비율 및 Z-Score', fontsize=18)
ax.set_xlabel('날짜')
ax.set_ylabel('가격 비율 / Z-Score')
ax.legend()

# 그래프 레이아웃 조정
plt.tight_layout()

# 그래프 표시
plt.show()
```

<img src="/assets/img/2024-07-22-BuildingAPairs-TradingStrategyWithPython_6.png" />

위 그림은 녹색 수평 선이 시장에서 Citigroup Inc ©의 매수 및 Aave (AAVE)의 매도를 나타내는 시그널로, 이를 횡단하는 경우에 해당합니다. 반면, 빨간색 선은 그 반대를 나타냅니다. 이 차트는 주로 정상성을 시각화한 것을 주목해 주세요. 실제로는 거래 시그널을 적용할 때 임의로 변화하는 시장 움직임에 적응하기 위해 이동 윈도우를 사용하여 기준선을 조정합니다.

다음은 거래 시그널을 구현해 보겠습니다.

<div class="content-ad"></div>

```python
def signals_zscore_evolution(ticker1_ts, ticker2_ts, window_size=15, first_ticker=True):
    """
    두 시계열 비율의 z-점수 분석에 기반한 거래 신호 생성.
    매개변수:
    - ticker1_ts (pandas.Series): 첫 번째 보안의 시계열 데이터.
    - ticker2_ts (pandas.Series): 두 번째 보안의 시계열 데이터.
    - window_size (int): z-점수 및 비율 통계 계산을 위한 창 크기.
    - first_ticker (bool): 주 신호 소스로 첫 번째 티커를 사용하려면 True로 설정하고 두 번째를 사용하려면 False로 설정하십시오.
    반환값:
    - signals_df (pandas.DataFrame): 'signal' 및 'orders' 열을 포함하며 매수 (1) 및 매도 (-1) 신호를 나타내는 DataFrame.
    """
    ratios = ticker1_ts / ticker2_ts
    ratios_mean = ratios.rolling(
        window=window_size, min_periods=1, center=False).mean()
    ratios_std = ratios.rolling(
        window=window_size, min_periods=1, center=False).std()
    z_scores = (ratios - ratios_mean) / ratios_std
    buy = ratios.copy()
    sell = ratios.copy()
    if first_ticker:
        # 신호 없어야 하는 빈 영역들, 나머지는 비율에 의해 시그널
        buy[z_scores > -1] = 0
        sell[z_scores < 1] = 0
    else:
        buy[z_scores < 1] = 0
        sell[z_scores > -1] = 0
    signals_df = pd.DataFrame(index=ticker1_ts.index)
    signals_df['signal'] = np.where(buy > 0, 1, np.where(sell < 0, -1, 0))
    signals_df['orders'] = signals_df['signal'].diff()
    signals_df.loc[signals_df['orders'] == 0, 'orders'] = None
    return signals_df

AAVE_ts = uts_sanitized["AAVE-USD"]["Adj Close"]
C_ts = uts_sanitized["C"]["Adj Close"]
plt.figure(figsize=(26, 18))
signals_df1 = signals_zscore_evolution(AAVE_ts, C_ts)
profit_df1 = calculate_profit(signals_df1, AAVE_ts)
ax1, _ = plot_strategy(AAVE_ts, signals_df1, profit_df1)
signals_df2 = signals_zscore_evolution(AAVE_ts, C_ts, first_ticker=False)
profit_df2 = calculate_profit(signals_df2, C_ts)
ax2, _ = plot_strategy(C_ts, signals_df2, profit_df2)
ax1.legend(loc='upper left', fontsize=10)
ax1.set_title(f'시티그룹과 아베 연계', fontsize=18)
ax2.legend(loc='upper left', fontsize=10)
ax2.set_title(f'아베와 시티그룹 연계', fontsize=18)
plt.tight_layout()
plt.show()
```

<img src="/assets/img/2024-07-22-BuildingAPairs-TradingStrategyWithPython_7.png" />

<img src="/assets/img/2024-07-22-BuildingAPairs-TradingStrategyWithPython_8.png" />

알고리즘 거래 시스템에서는 여러 거래 신호가 동시에 작동하는 경우가 많습니다. 따라서 모든 신호의 수익을 모아 전체 성능을 파악하는 것이 관습적입니다.


<div class="content-ad"></div>

계산을 마무리하는 것으로 넘어가보죠. 추가로 포함하고 싶은 구체적인 세부 정보나 계산이 있으시면 제공해주세요. 그렇다면 더 도와드릴 수 있어요!

```js
plt.figure(figsize=(12, 6))
누적이익_합산 = 이익_데이터프레임1 + 이익_데이터프레임2
누적이익_차트 = 누적이익_합산.plot(
    label='이익%', color='green')
plt.legend(loc='upper left', fontsize=10)
plt.title(f'아베 & 시티그룹 페어링 - 누적 이익', fontsize=18)
plt.tight_layout()
plt.show()
```

<img src="/assets/img/2024-07-22-BuildingAPairs-TradingStrategyWithPython_9.png" />

분석 기간 동안 페이퍼 수익률이 100%로 강한 성과를 보였으며 50%의 하락을 겪었습니다. 이 성과는 S&P 500의 2년 수익률 10%를 앞섰습니다. 그러나 이 전략은 암호화폐와 Citigroup Inc ©의 변동성이 높아서 높은 분산을 나타냈습니다.

<div class="content-ad"></div>

실제로 양적 분석가들은 Sortino 비율과 같은 위험 조정 지표를 사용하여 전략 성과를 평가합니다. Sortino 비율은 하락 리스크에 초점을 맞춥니다.

결론적으로, 우리는 페어 트레이딩 전략의 주요 측면을 탐색했습니다. 시장 중립적 접근, Z-스코어와 코인테그레이션과 같은 통계 도구에 의존하며, 평균 회귀의 이용 등이 그 중 일부입니다. 그러나 실제 시나리오에서 이 전략을 시행하는 데는 거래 비용이나 자산 상관 관계 또는 코인테그레이션의 비정상성 위험 등과 같은 도전에 직면할 수 있습니다.

양적 분석과 금융 공학은 복잡할 수 있지만, 특히 YouTube와 같은 플랫폼에서 거래 콘텐츠를 만날 때에는 비평적 시각을 유지하는 것이 중요합니다.

저의 추천 링크를 통해 Tradingview 구독비를 최대 100% 절약할 수 있습니다. 해당 링크를 이용하려면 해당 페이지의 상단 좌측에 있는 Tradingview 아이콘을 클릭하여 무료 요금제로 이동하실 수 있습니다.

<div class="content-ad"></div>

➡️ 제 블로그를 구독하려면 여기를 클릭해주세요 ➡️ https://medium.com/@aamurtazin/subscribe

제 다가오는 블로그에서 공유할 많은 이야기들이 준비되어 있어요.