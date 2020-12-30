---
title:  "[Time Series Analysis] #1 시계열 평활기법"
categories:
  - theory
tags:
  - timeseries
toc: true
sitemap: 
---

**시계열분석(Time Series Analysis)** 은 하나의 변수에 대한 시간에 따른 관측치인 시계열데이터를 분석하는 것이다. 

시계열 데이터를 다루는 공모전이 많았는데 나는 비전공자(수학과)에다가 데이터의 길로 입문한지 얼마 되지 않았기 때문에 시계열분석에 대한 지식이 없어서 참가하지 못했던 아쉬움이 있었다.
그래서 이제부터 [KMOOC 시계열분석 강의]를 듣고 이론을 공부하고, 자율적으로 python에서는 어떤 코드를 써야 하는지 공부하고 포스팅하기로 마음먹었다. <br>

<br>

첫번째 주제로는 시계열분석의 기초인 **평활기법** 에 대해서 리뷰하도록 하겠다. 그리고 각 방법에 해당하는 코드 실습을 첨부한다. 사용한 데이터는 temperature 데이터이다.

## Preview
우선 본격적인 이론 및 코드 적용에 앞서, 사용한 데이터에 대해 간단히 살펴보자. <br>
[Data information]
 + 1981년 1월 1일부터 1990년 12월 31일까지의 온도 데이터


## 1. 이동평균(Moving Average)
**이동평균법** 은 매 시점에서 직전 N개의 데이터를 이용해 평균을 구하는 방법이다. 보통 N이 커질수록 평활 정도가 커진다. 
## 

[KMOOC 시계열분석 강의]: http://www.kmooc.kr/courses/course-v1:POSTECHk+IMEN677+2020_2/about
