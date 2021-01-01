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
이전 평활기법들과 달리, Holt-Winters 모델은 시계열 데이터의 계절성이 어떤 패턴을 보이는지에 따라 Additive(가법) 과 Multiplicative(승법) 으로 나눠진다.

<br>

여기서 잠깐! Additive 와 Multiplicative 의 차이는 무엇이고, 계절성의 패턴이 어떤 것에 해당하는지 어떻게 판단할까?

![season](/assets/comparison.png)

위의 두 가지 그림은 모두 계절성을 띄고 점점 증가하는 추세를 보이고 있다. 차이는 시계열데이터의 **진폭** 찾을 수 있다. 여기서 말하는 진폭이란, 한 주기에서 고점과 저점의 차이를 말한다. Additive 의 경우 증가 추세를 보이지만, 각 주기마다 진폭이 대체로 일정한 반면, Multiplicative의 경우 각 주기의 진폭이 점차적으로 증가한다. 

<br>
그러면 다시 본론으로 돌아와 Holt-Winters의 이론에 대해 살펴보자. Holt 모형과 마찬가지로 Holt-Winters 모형은 수평수준(levels) $L_{t}$와 추세(trend) $b_{t}$ 요소를 갖고 있다. 추가적으로 Holt-Winters는 계절성을 반영하기 때문에 이를 나타내는 계절성지수 $s_{t}$ 를 포함한다. (이 때 계절성지수들의 평균은 1이 되도록 조정한다.) 따라서 시점 t에서 시점 t+k의 값을 예측한 값은 다음과 같이 나타낼 수 있다. 두 기법의 차이는 계절성지수 $s_{t-m+k}$ 를 $L_{t}+kb_{t}$ 에 곱했는지 더했는지에 있다.
+ Additive: $f_{t,k}=L_{t}+kb_{t}+s_{t-m+k}$
+ Multiplicative: $f_{t,k}=(L_{t}+kb_{t})s_{t-m+k}$














<br>

[저번 포스팅]: https://hyewonleess.github.io/theory/timeseries-1/
