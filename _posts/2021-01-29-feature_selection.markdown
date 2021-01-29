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

<br>

Feature selection method는 크게 다음의 세 타입으로 나눌 수 있다. 이 세가지 method에 속하는 대표적인 feature selection 기법에 대해 알아보도록 하겠다!
 1. Filter's methods
 2. Wrapper's methods
 3. Embedded methods
 
 ### 1. Filter's methods
 Filter's method는 세 가징 method 중 가장 심플하다고 말할 수 있는 방법으로, 일변량 통계량(univariate statistics)을 이용해 feature의 특성을 추출하고 선택하는 방법이다. 다른 변수들과의 관계성은 고려하지 않고 단일 변수를 통계량을 이용해 평가하기 때문에 다른 methods에 비해 계산량과 시간이 적게 소모된다는 장점이 있다. 대표적인 filter's methods feature selection 방법에는
 (트리기반) feature importance, variance threshold, corrleation coefficient 방법이 있다.
 
 #### (1) Feature importance
 Feature importance는 트리기반 알고리즘을 사용하는 모델에 대해 적용되는 방법이다. 트리기반 모델을 학습하면 각 feature 마다 feature importance를 추출할 수 있는데, 여기서 말하는 feature importance는 information gain과 관련이 있다. 트리 모델은 불순도(impurity)를 최소화 하는 방향으로 분기를 한다. 예를 들어 트리 분기를 할 때, 분기를 할 node를 선택하는데 이 때 information gain을 이용한다. Information gain은 쉽게 말하면 (상위 노드의 불순도 - 분기한 후 좌/우 노드의 불순도) 를 계산한 것으로, information gain이 크면 노드가 분기했을 때 불순도가 많이 감소한다는 것을 의미한다. 요약하면 트리 모델은 information gain이 큰 node를 선택해 그 node를 기준으로 트리를 분기하게 되는 것이다. 
 <br>
 Feature importance는 해당 feature가 얼마나 불순도를 감소시켰는지를 기준으로 계산된다. 즉, 트리기반 모델에서는 feature가 불순도를 많이 감소시킬수록 importance가 커진다.
 
 
 #### (2) Variance Threshold
 Variance threshold는 말 그대로 변수의 variance가 지정한 threshold 보다 작으면 drop하는 방법이다. 변수의 variance가 너무 작으면 target을 예측하는 성능이 낮아진다고 판단하는데, 어떤 변수의 variance가 작으면 변수의 각 value가 target에 미치는 영향의 차이는 미비하기 때문이다. 이러한 이유로 feature을 선택할 때 이 방법을 사용하기도 한다.
<br>
Variance threshold 기법의 단점은 개별 변수의 variance만 고려한다는 것이다. 예를 들면, 만약 어떤 변수의 variance가 작더라도 target 변수와 상관관계가 높으면 target을 예측하는데 영향을 줄 수 있다. 하지만 variance가 작다는 이유로 이 변수를 제거하면 모델의 성능은 낮아질 것이다.

 #### (3) Correlation coefficient
 이 방법은 매우 간단하다. Target 변수와 개별 feature 변수의 상관계수를 구하고, 상관계수의 절댓값이 큰 feature를 선택하는 방법이다. 
 
 
