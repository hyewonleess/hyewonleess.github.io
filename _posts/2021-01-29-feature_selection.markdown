---
title:  "[Feature Selection] #1. 다양한 Feature Selection method 소개"
categories:
  - theory
tags:
  - feature selection
  - ML
  - modeling
toc: true
use_math: true
sitemap: 
---
## Feature Selection Methods
오랜만에 블로그 포스팅이다! Kaggle이나 공모전, 대학원 프로젝트에서 모델링을 할 때 가장 막막했던 부분은 feature engineering 파트이다. 모델의 성능 향상에 기여하는 feature를 생성하는 것도 중요하지만,
이와 못지 않게 필요한 feature만 선택하여 모델링 과정에서 computation cost를 줄이고 해석의 용이성을 높이는 <mark style='background-color: #fff5b1'> feature selection </mark> 도 매우 중요하다.
<br>
모델링을 할 때, feature가 너무 많아지면 차원의 저주(Curse of dimensionality) 문제가 발생할 수 있다. 차원의 저주는 간단하게 말하면 학습 변수(=feature)의 수가 너무 많아져 모델링 및 데이터의 처리에 문제가
생기는 것을 말한다. 이 문제를 피하기 위해 target value 예측에 기여를 하는 feature만 고르는 과정이 필요한데, feature selection이 이 과정을 수행하는 단계라고 보면 될 것이다!

<br>
이번 포스팅에서는 대표적인 feature selection 방법들에 대해 설명하고, Kaggle의 House price 데이터에 적용하는 내용을 다룰 것이다.

### Feature Selection method 종류
Feature selection method는 크게 다음의 세 타입으로 나눌 수 있다. 
 1. Filter's methods
 2. Wrapper's methods
 3. Embedded methods
