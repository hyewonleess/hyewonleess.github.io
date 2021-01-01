---
title:  "[Time Series Analysis] #1 시계열 평활기법(2)"
categories:
  - theory
tags:
  - timeseries
toc: true
use_math: true
sitemap: 
---

이번 포스팅에서는 [저번 포스팅]에 이어 평활기법 중 Holt 및 Holt-winters 에 대해 다루도록 하겠다. 지난 포스트처럼 이론에 대해 설명한 후, Python code로 실습을 하는 흐름으로 진행하겠다.

<br>

## 1. Holt 와 Holt-Winters, 그리고 그 차이
Holt와 Holt-winters 기법은 비슷한 듯 보이지만 분명한 차이가 존재한다. 그 차이와 각 기법의 이론에 대해  알아보자! 

### (1) Holt 
Holt 모형은 시계열데이터게 추세 패턴을 갖고 있는 경우 적용하는 시계열 평활기법이다. 저번 포스팅에서 다룬 단순지수평활법 이 수평 추세 패턴을 갖고 있는 데이터에 적합한 것에 반해,
이 데이터는 선형 등의 추세 패턴을 가진 데이터에 적합한 방법이다. 다만, **Holt 모형은 계절성을 반영하지 않는다.** <br>
t 시점에서의 Holt 모형은 수평수준(levels)를 나타내는 $L_{t}$ 와 추세(trend)를 나타내는 $b_{t}$ 로 표현할 수 있다. 가령, 시점 t에서 t+k 시점의 값을 예측한 값은 
$f_{t}= L_{t} + kb_{t}$ 로 계산한다.

### (2) Holt-Winters
Holt-Winters 모형은 Holt 모형이 계절성을 반영하지 못한다는 단점을 보완한 모형이다. 즉, Holt-Winters 모형은 시계열데이터의 추세패턴과 계절성을 모두 반영한다. 
이전 평활기법들과 달리, Holt-Winters 모델은 시계열 데이터가 어떤 패턴을 보이는지에 따라 Additive(가법) 과 Multiplicative(승법) 으로 나눠진다.

<br>

여기서 잠깐! Additive 와 Multiplicative 의 차이는 무엇이고, 시계열데이터가 어떤 것에 해당하는지 어떻게 판단할까?


















<br>

[저번 포스팅]: https://hyewonleess.github.io/theory/timeseries-1/
