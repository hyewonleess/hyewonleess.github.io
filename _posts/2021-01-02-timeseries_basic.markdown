---
title:  "[Time Series Analysis] #2 ARMA/ARIMA를 위한 개념 Build-up"
categories:
  - theory
tags:
  - timeseries
toc: true
use_math: true
sitemap: 
---

이제 시계열분석의 핵심 모델인 ARMA/ARIMA를 위한 개념을 잡는 마지막 관문이다! 앞선 두 포스팅에서는 시계열 평활기법에 대해 살펴보았다. 이번 포스팅에서 다룰 내용은 다음과 같다.
 1. 정상성 시계열
 2. 자기상관함수(ACF)/편자기상관함수(PACF)
 3. AR 모델/MA 모델


## 1. 정상성 시계열(Stationary Time Series)
**정상성 시계열** 간단하게 말하면 시계열 데이터 ${Z_{t}}$의 expectation과 variance가 일정한 경우를 말한다. 이 때, 정상성 시계열은 조건에 따라 강 정상성(Strong Stationary)와 약정상성(Weak Stationary)
로 분류할 수 있다.
 + 강 정상성
  1) $(Z_{1},\cdonts,Z_{t})$ 와 $(Z_{1+k},\cdonts,Z_{t+k})$ 가 같은 결합확률분포를 가진다.
  2) $E(Z_{t})$ 는 항상 일정
  3) $Var(Z_{t})$ 는 항상 일정
  4) $Cov(Z_{t},Z_{t+1}) = Cov(Z_{t+k},Z_{t+k+1})$ : 두 시점의 공분산은 time lag에만 의존한다.
  
하지만 강정상성의 조건을 만족하는 실제 데이터는 거의 존재하지 않는다. 따라서 좀 더 약한 조건을 적용하는데, 이것이 약 정상성이다.
 + 약정상성
  1) $E(Z_{t})$ 는 항상 일정
  2) $Cov(Z_{t},Z_{s})$ 는 두 시점의 차이의 절대값인 $|s-t|$ 에만 의존한다.
  

## 2. ACF/PACF

### (1) 자기상관함수(ACF)
**ACF**는 쉽게 말하면 시계열 데이터 ${Z_{t}}$ 에서 시점 t와 시점 t+k 가 얼마나 상관성이 있는지를 나타내는 함수이다. 시계열데이터분석을 할 때, 이전 시점의 데이터가 현재 시점의
데이터에 미치는 영향을 파악하는 것이 중요하다. 이를 영향을 설명할 수 있는 함수 중 하나가 ACF 라는 것이다!
<br>
ACF 수식 표현: $\rho(k) = Cor(Z_{t},Z_{t+k})$ 

### (2) 편자기상관함수(PACF)
**PACF**는 시계열 데이터 ${Z_{t}}$ 에서 시점 t와 시점 t+k의 관계를 두 시점 사이에 있는 시점 데이터($Z_{t+1},\cdots,Z_{t+k-1}$)를 고려하여 설명하는 함수이다.
<br>
ACF 수식 표현: $\rho(k) = Cor[Z_{t},Z_{t+k}|$Z_{t+1},\cdots,Z_{t+k-1}$]$ 
