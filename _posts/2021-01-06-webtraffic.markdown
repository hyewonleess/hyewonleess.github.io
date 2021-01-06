---
title:  "[Time Series Analysis] #1. Simple Web Traffic Data Analysis - Part 1"
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
# Part 1. Data Cleansing & EDA

지금까지 시계열분석에 대한 필요성을 느끼고 KMOOC 강좌로 이론을 공부하고 블로그에 포스팅을 했었다. 이론만 공부해서는 실제로 시계열분석 기법들이 어떻게 적용되는지 공부하기 힘들다.
그래서 이번 포스팅부터는 Kaggle 데이터를 분석해보는 작은 미니 프로젝트를 진행해보려고 한다. <br>
<br>

포스팅은 다음과 같은 두 단계로 나누어서 진행할 예정이다.
 + Part 1. Data Cleansing & EDA
 + Part 2. Model comparisons
 
 
이번 포스팅인 Part 1에서는 기본적인 데이터 처리 및 이해를 위한 Data Cleansing 과 EDA 과정을 다루겠다!

## Web Traffic data 개요
 + link: <https://www.kaggle.com/c/web-traffic-time-series-forecasting/data>
 + 145063 rows, 551 columns
 + rows: access 방법, agent 종류를 포함한 위키피디아 page name
 + columns: 2015년 7월 1일부터 2016년 12월 31일까지의 일 단위
 + values: Page 방문자 수
![rawdata](/assets/rawdata.PNG)
보다시피 데이터의 크기가 너무 크기 때문에 필자는 Page 중 'en.wikipedia.org'를 포함하는, 즉 영문 위키피디아 페이지만을 대상으로 분석을 진행했다.

## 1. Data Cleansing
### (1) 데이터 재구조화
Raw data 를 보면 row가 Page 명, column이 일별 날짜로 이루어져있는데 이를 그대로 쓰게 되면 데이터를 분석하기 다소 힘들다는 판단을 했다. 따라서 `pd.melt` 를 이용해 데이터재구조화를
진행했다. Column을 이루는 날짜들을 변수로 변환하여, Page별 일별 Visits 수를 볼 수 있도록 하였다.
![rawdata](/assets/melt_data.PNG)

### (2) Missing values
