---
title:  "[Time Series Analysis] #1. Simple Web Traffic Data Analysis - Part 2"
categories:
  - application
tags:
  - timeseries
  - kaggle
  - webtraffic
toc: true
use_math: true
sitemap: 
---
# Kaggle data에 시계열분석 적용해보기
# Part 2: ARIMA application

지난 포스트에서는 Web Traffic 데이터에 대한 EDA를 진행했다. 이번 포스트에서는 시계열분석이론에서 공부했던 **ARIMA** 모델을 적용한 결과를 다룬다!<br>
일단 결론부터 말하자면... ARIMA를 이용한 Web Traffic 데이터 예측의 성능은 매우매우 좋지 않았다..! 그래서 이번 포스트에서는 왜 성능이 안좋았는지에 대해서 나름 생각해본 부분도
다뤄보도록 하겠다.

<br>

### 1. 세부 데이터 선택
저번 포스트에서 Page 별로 클러스터링을 했을 때 클러스터 4에 속하는 Page의 추세가 전체적인 추세와 가장 비슷하다는 결과를 얻을 수 있었다. 따라서 ARIMA 모델 분석을 위해, 클러스터 4에
속하는 데이터 중 평균 Visits 수가 가장 높은 Page를 선정했다. 해당 Page는 '메인 페이지'로 위키피디아 홈페이지에 접속하면 가장 먼저 뜨는 페이지이다.(그러니 당연히 평균 방문수가 높을 것이다.)
메인 페이지 방문 수는 곧 위키피디아 방문 수를 대변할 수 있기 때문에 이 데이터를 활용해보겠다.
![main](/assets/ma.png)

