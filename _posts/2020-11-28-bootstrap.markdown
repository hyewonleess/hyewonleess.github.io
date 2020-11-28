---
title:  "[데이터분석이론] #2 Bootstrap"
categories:
  - theory
tags:
  - bootstrap
toc: true  
sitemap: 
use_math: true
---

오늘은 **Bootstrap** 에 대하여 공부해볼 것이다. 이 포스트에서는 모델링에서의 bootstrap이 아닌, 모수 $\theta$ 를 추정할 때 사용하는 bootstrap에 대해서 다룬다.

## 1. Bootstrap 개념
먼저 미지의 모집단 F에서 관측 표본 $x_{1},x_{2},...,x_{n}$ 을 얻었다고 가정한다. 여기서 우리가 관심을 가질 것은 모집단의 모수 중 하나인 $\theta$ 이다. 이 $\theta$ 를 추정하기 위해서 관측 표본에서 n개의 표본을 sampling(with replacement)를 하여 재표본을 뽑는다. 이 재표본을 $x_{1k},...,x_{nk}$ 이라 하자. Bootstrap은 재표본을 여러번 sampling 하고 각 sampling 마다 표본에서 추정 모수 $\hat{\theta}$를 구하는 과정으로 이루어진다.  <br>

**Bootstrap 과정**
 1. 관측 표본  $x_{1},x_{2},...,x_{n}$ 에서 모수 $\theta$ 를 추정하는 통계량  $\hat{\theta} = T(x_{1},...,x_{n})$ 를 계산한다.
 2. $x_{1},x_{2},...,x_{n}$ 에서 sampling with replacement를 하여 $x_{1k},...,x_{nk}$ 를 구성하고, $x_{1k},...,x_{nk}$ 를 이용해 $\hat{\theta_{new}}= T(x_{1k},...,x_{nk})$ 를 계산한다.
 3. 위 작업을 B (boostrap 횟수) 만큼 반복한다.
 4. 각 B 에서 추정한 모수 $\theta$ 의 평균을 내 $\theta$ 의 boostrap 추정량을 구한다. $\hat{\theta_{boot}}=\frac{1}{B}\sum\hat{\theta_{new}}$ <br>
 그리고 $\hat{\theta_{boot}}$의 bias와 신뢰구간을 구한다.
 
**추정모수의 편향(bias)와 신뢰구간 구하는 방법**
 + Bias: $\hat{\theta_{boot}}-\hat{\theta}$ 
 + 신뢰구간: 
   - 백분위수방법(percentile method): 모수의 분포가 정규분포를 따를 때 적합한 방법. 예를 들어 90% 백분위수 신뢰구간을 구할 때, 하위 5%, 상위 5%가 신뢰구간의 기준이 된다.
   - BCa 방법(bias-corrected and accelerated method): 모수의 분포의 skewness가 클 때 사용하는 방법이다. 우리가 일반적으로 아는 신뢰구간을 구하는 방법이다.($\alpha$% 신뢰구간)
  
  





