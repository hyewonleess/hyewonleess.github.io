---
title:  "[데이터분석이론] #2 Bootstrap(부트스트랩)"
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
먼저 미지의 모집단 F에서 관측 표본 $x_{1},x_{2},\cdots,x_{n}$ 을 얻었다고 가정한다. 여기서 우리가 관심을 가질 것은 모집단의 모수 중 하나인 $\theta$ 이다. 이 $\theta$ 를 추정하기 위해서 관측 표본에서 n개의 표본을 sampling(with replacement)를 하여 재표본을 뽑는다. 이 재표본을 $x_{1}^{*},\cdots,x_{n}^{*}$ 이라 하자. Bootstrap은 재표본을 여러번 sampling 하고 각 sampling 마다 표본에서 추정 모수 $\hat{\theta}$를 구하는 과정으로 이루어진다.  <br>

**Bootstrap 과정**
 1. 관측 표본  $x_{1},x_{2},...,x_{n}$ 에서 모수 $\theta$ 를 추정하는 통계량  $\hat{\theta}$ 를 계산한다.
 2. 


