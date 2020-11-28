---
title:  "[데이터분석실습] #1 주식데이터 탐색 - 1편"
categories:
  - application
tags:
  - analysis
  - finance
toc: true
sitemap: 
---
안녕하세요! 요즘에 저는 주식 투자에 관심이 생겼습니다. 그래서 주식 데이터를 통해 주식 상황을 더 잘 이해해보려고 합니다. 그래서 오늘부터는 미래에셋 주식 데이터를 가지고 여러가지 분석을 해보고 주식에 대한 기본적인 개념과 주식시장에서의 현상을 이해하는 포스팅을 하고자 합니다. 시작은 간단하게, 1편에서는 주식 그래프의 기본적인 개념과 그래프를 공부하고, 주요 대기업의 주가 및 거래량 동향을 살펴보도록 하겠습니다.<br>

# 주식데이터 탐색 - 1편: 주요 기업 주식 탐색

사용 데이터: 미래에셋 주식 데이터 <https://programmers.co.kr/competitions/252/2020-miraeasset> <br>

데이터는 2020 미래에셋 금융빅데이터 페스티벌 '주식투자' 분야에서 가져왔습니다. 첫번째 주식 포스팅에서는, stocks dataset을 이용하여 주요 대기업의 주식 주가 및 거래량 변동 트렌드에 대해서 분석하려고 합니다. <br>

## 1. stocks dataset 살펴보기
사용할 stocks dataset을 로드하여 대략적인 데이터 구성을 보면 다음과 같습니다. `기준일자` 변수는 2019년 7월 1일부터 2020년 7월 28일까지를 포함하고 있으며, 일별 주가를 나타내는 `종목시가`,`종목고가`,`종목저가`,`종목고가`, 그리고 종목을 나타내는 변수들과 종목이 속하는 산업군 변수가 있습니다. 이 때, `표준산업코드_중분류`, `표준산업코드_소분류`는 전체적인 분석에는 필요하지 않기에 제거하고 분석을 했습니다. 그리고 일별 종목의 총 매도 및 매수 건수를 나타내는 매우 중요한 변수인 `거래량`, `거래금액_만원단위` 변수도 있습니다. <br>

![data preview](/assets/preview.PNG)
<br>
<br>
이제 stocks data에 이상치가 있는지 `describe()` 함수를 사용하여 이용해 확인해보겠습니다. 거래량과 종목 가격 변수의 minimum이 0인데, 상식적으로 종목의 주가와 거래량이 0이 되는 것은 있을 수 없는 일입니다. 따라서 이 변수들의 값이 0인 데이터를 분석에서 제외하겠습니다.
<br>
![describe](/assets/describe.PNG)
<br>
<br>
## 2. 주요 대기업 주가/거래량 변동
**분석대상기업**: 삼성전자, 현대차, NAVER <br>
이제 삼성전자, 현대차, NAVER의 주식 데이터 트렌드를 살펴보겠습니다. 여기서 알아야 할 기본적인 주식 개념들이 있습니다. 주식 차트의 캔들과 이동평균선에 대한 설명은 다음 글을 참고했습니다. <br>
<br>
캔들 설명: <https://monstock.tistory.com/69 
<br>
이동평균선 설명: <https://tree9odic.tistory.com/21> <br>
<br>
**캔들과 이동평균선**은 주식 차트를 볼 때 주의깊게 봐야 할 중요한 요소입니다. 그래서 캔들을 plot하기 위핸 code를 추가했고, 이동평균은 종목종가 변수를 기준으로  `rolling` 함수를 사용하여 5일, 20일, 60일, 120일 단위로 계산하여 새로운 변수로 추가했습니다. <br>
```
samsung['MA5'] = round(samsung['종목종가'].rolling(window=5, min_periods=1).mean(),0)
samsung['MA20'] = round(samsung['종목종가'].rolling(window=20, min_periods=1).mean(),0)
samsung['MA60'] = round(samsung['종목종가'].rolling(window=60, min_periods=1).mean(),0)
samsung['MA120'] = round(samsung['종목종가'].rolling(window=120, min_periods=1).mean(),0)
```
그리고 다음은 주가변동 트렌트 plot을 그릴 때 정의한 함수입니다.
```
def plot_trend(df,name):
  from matplotlib import gridspec

  plt.figure(figsize=(20, 8))                  
  gs = gridspec.GridSpec(nrows=2, ncols=1,    
                       height_ratios=[5, 1]) 

##############################################
  plt.subplot(gs[0])                         
  # 캔들스틱 그리는 코드 넣을 자리 
  plt.subplot(gs[0])
  plt.title(f'{name} 주식캔들',fontsize=15)
  plt.bar(df['기준일자'],height=df['종목종가']-df['종목시가'],bottom=df['종목시가'],color=cmap)
  plt.vlines(df['기준일자'],df['종목저가'],df['종목고가'],colors=cmap)
  plt.axvspan(datetime.datetime(2020,3,1),datetime.datetime(2020,4,1),facecolor='yellow',alpha=0.5)
  sns.lineplot(df['기준일자'],df['MA5'],color='orange',label='MA5',alpha=0.5);sns.lineplot(df['기준일자'],df['MA20'],color='green',label='MA20',alpha=0.5);
  sns.lineplot(df['기준일자'],df['MA60'],color='purple',label='MA60',alpha=0.5);sns.lineplot(df['기준일자'],df['MA120'],color='pink',label='MA120',alpha=0.5);
  plt.ylabel('주가');

  plt.subplot(gs[1])                       
  sns.lineplot(data=df,x='기준일자',y='거래량',color='green')
  plt.axvspan(datetime.datetime(2020,3,1),datetime.datetime(2020,4,1),facecolor='yellow',alpha=0.5)
  plt.title(f'{name} 거래량 변동 추이',fontsize=15)

##############################################

  plt.subplots_adjust(wspace = 0, hspace = 0)  
  plt.show()

def plot_trend_ver2(df,name):
  from matplotlib import gridspec

  plt.figure(figsize=(20, 8))                 
  gs = gridspec.GridSpec(nrows=2, ncols=1,    
                       height_ratios=[5, 1]) 

##############################################
  plt.subplot(gs[0])                         
  # 캔들스틱 그리는 코드 넣을 자리 
  plt.subplot(gs[0])
  plt.title(f'{name} 주식캔들',fontsize=15)
  plt.bar(df['기준일자'],height=df['종목종가']-df['종목시가'],bottom=df['종목시가'],color=cmap)
  plt.vlines(df['기준일자'],df['종목저가'],df['종목고가'],colors=cmap)
  sns.lineplot(df['기준일자'],df['MA5'],color='orange',label='MA5',alpha=0.5);sns.lineplot(df['기준일자'],df['MA20'],color='green',label='MA20',alpha=0.5);
  sns.lineplot(df['기준일자'],df['MA60'],color='purple',label='MA60',alpha=0.5);sns.lineplot(df['기준일자'],df['MA120'],color='pink',label='MA120',alpha=0.5);
  plt.ylabel('주가');

  plt.subplot(gs[1])                       
  sns.lineplot(data=df,x='기준일자',y='거래량',color='green')
  plt.title(f'{name} 거래량 변동 추이',fontsize=15)

##############################################

  plt.subplots_adjust(wspace = 0, hspace = 0)  # 구획(subplot) 간의 간격을 없앤다.
  plt.show()
  ```

### (1) 삼성전자
삼성전자는 한국에서 가장 큰 기업 중 하나입니다. 지난 1년간 삼성전자의 주가와 거래량은 어떻게 변화했는지 캔들차트와 거래량 변동 그래프를 통해 살펴보겠습니다. 아래 그래프에서 위쪽에
위치한 그래프는 캔들 차트를 나타내며 (빨간색 - 양봉, 파란색 - 음봉), 아래에 위치한 그래프는 거래량 변동을 나타냅니다. <br>
![samsung](/assets/samsung.png)
<br>
2019년 7월부터 2020년 7월까지 삼성전자의 주가는 변동이 많습니다. 전체적인 이동평균선의 흐름을 보면 2020년 2월까지는 주가가 상승하는 추세를 보이나 3월에 급락 후 그 이후에는 다시 주가가 반등하
는 경향을 보이고 있습니다. 특이한 것은 2020년 1월, 2월에는 주가가 최고점을 찍었다가 3월에는 급락하는데, 반면에 거래량은 2020년 3월에 최고점을 찍습니다. <br>
<br>
그러면 주가가 최고점을 찍은 이후 급락하는 이유는 무엇일까요? 그 이유는 다음과 같이 예측할 수 있습니다. <br>
<br>
**세력이 더 많은 이익을 얻기 위해 공매도를 통해 주가를 떨어뜨린다-> 상대적으로 주가 하락에 예민하고 취약한 개인투자자들은 불안심리로 인해 보유하고 있는 주식을 매도 -> 세력은 떨어진 주가에서, 즉 낮은 가격에서 해당 종목의 주식을 매수 -> 보유 주식 증가** <br>
<br>
우선 주식 투자자에는 개인투자자(일명: 개미)와 세력(기관투자자, 기업 등등)이 있는데, 세력은 일반적으로 개인투자자보다 종목에 대한 지분이 훨씬 많습니다. 삼성전자의 세력이 주가의 고점에서 충분한
이익을 얻으면(2020년 1월, 2월) 더 많은 주식 물량을 확보하기 위해 공매도를 하는 것으로 보입니다. (여기서 공매도란, 주가 하락이 예상되는 종목의 주식을 미리 빌려서 팔고 나중에 실제로 주가가 내려가면 싼값에 다시 사들여 빌린 주식을 갚아 차익을 남기는 투자 기법) 종목 지분이 많은 세력이 공매도를 하면 주가는 떨어지는데, 그러면 상대적으로 주가 변동에 예민한 개인투자자들은 불안심리로 인해 보유하고 있는 주식을 매도하기 시작합니다. 세력은 다시 낮은 가격에 매도된 주식을 다시 사들임으로써 주식 물량을 증가시키는 것이죠! 그렇기 때문에 2020년 3월에 삼성전자의 주가가 하락해도 거래량은 늘어나는 것이라고 생각됩니다. 하지만, 이러한 현상이 일어나는 또 다른 이유도 있을 수 있기 때문에 이에 대해서는 더 공부한 뒤 알아보도록 하겠습니다.



### (2) 현대차
다음으로는 현대차의 주가/거래량 변동입니다. 현대차 주가의 이동평균선을 보면 2019년 7월부터 주가가 하락세를 보이지만 2020년 6월부터는 조금씩 반등하고 있습니다. 그리고 삼성전자와 마찬가지로
2020년 3월에 주가가 급락하는 반면, 거래량은 증가합니다. 이러한 현상은 삼성전자 사례와 같은 세력에 의한 공매도에 의한 것이라기보다는 올해 가장 큰 이슈였던 코로나-19로 인한 것이로 볼 수 있습니다. 2020년 2월 말부터 코로나 확진자수가 급증하면서 현대차 생산공장 라인 가동을 중단하면서 현대차의 주가는 하락한 것으로 보입니다. 게다가 코로나가 전세계적으로 퍼지면서 자동차 부품 조달 및 수출의 어려움이라는 악재가 겹치면서 주가가 폭락한 것으로 볼 수 있습니다. 이 때 거래량이 증가한 것은, 주가가 떨어지면서 많은 투자자들이 보유하고 있던 현대차 주식을 매도하는 건수가 증가했기 때문이라고 예상할 수 있습니다.
<br>
![hyundai](/assets/hyundai.png)
<br>
참고 기사: <https://news.mt.co.kr/mtview.php?no=2020032211182830835>

### (3) 네이버
네이버의 주식 이동평균선과 캔들차트를 보면 네이버의 주가는 전체적으로 상승세를 그리고 있다는 것을 알 수 있습니다. 코로나 19 시기(2020년 3월)에 네이버 주가는 잠시 하락했지만 다시 상승세를
회복했습니다. 그리고 주가 감소폭도 삼성전자, 현대차에 비하면 크지 않았습니다. 그 이유에 대해 알아보기 위해 자료조사를 했는데, 네이버는 코로나 19 시기에도 네이버 쇼핑 서비스의 굳건화 그리고 네이버 금융 사업으로의 확장 등 다양한 신사업에 도전했습니다. 그리고 코로나19로 인해 대부분의 시간을 자택에서 보내는 사람들을 겨냥해 네이버 콘텐츠 사업을 확장함으로써 개인투자자들의 기대와 신뢰를 얻고 있는 것으로 보입니다. 
<br>
![naver](/assets/naver.png)
<br>
참고 기사: <http://www.citydaily.kr/news/articleView.html?idxno=1400>

<br>
이렇게 첫번째 포스팅에서는 주요 기업의 주가 및 거래량 변동 트렌드를 살펴보고, 이러한 추세를 보이는 이유에 대해 자료 조사를 하고 분석해보았습니다. 분석을 위해 사용했던 코드는 아래 링크에 첨부했습니다. <br>
Code link: <https://github.com/hyewonleess/github_blog_posts/tree/main/stocks>
 
