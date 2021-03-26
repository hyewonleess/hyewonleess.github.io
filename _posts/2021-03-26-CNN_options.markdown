---
title:  "[CNN] #2. CNN model 옵션 살펴보기"
categories:
  - deeplearning
tags:
  - CNN
  - deeplearning
  - DL
  - image
  - tensorflow
toc: true
use_math: true
sitemap: 
---

이전 포스팅에서는 가장 basic한 CNN 모델을 구현하는 방법에 대해 다루었다. 이번 포스팅에서는 CNN 모델을 보다 더 구체적으로 만드는 옵션들에 대해 살펴볼 것이다. 여러가지 옵션이 있겠지만,
이번 포스팅의 주제는 CNN 모델에서의  <mark style='background-color: #fff5b1'> Dropout layer, batch size </mark> 이다. 관련된 논문 요약과 더불어 이 옵션들이 MNIST 데이터에 적용했을 때 모델 성능에 어떤 영향을 미치는지 알아보자!

<br>

# 1. Dropout Layer
Dropout layer은 말 그대로 딥러닝/CNN 모델에서 hidden layer(은닉층)에 있는 노드의 일부를 drop하는 layer이다. 보통은 "노드를 없애면 오히려 모델 성능이 떨어지지 않을까?" 라는 의구심을 품을 수 있다. Dropout layer가 모델의 성능을 높여준다는 보장은 없지만, 적어도 **CNN 모델의 overfitting은 방지할 수 있다.**

Overfitting(과적합)은 모델이 훈련 데이터셋를 과도하게 학습하여 훈련 데이터셋에 대해서는 모델의 성능이 좋지만 새로운 데이터셋(예를 들면 검증 데이터셋)에 대해서는 오히려 성능이 떨어지는 현상을 말한다. 좋은 모델이라면 어떤 데이터셋이 들어와도 잘 예측을 해야 하기 때문에 CNN 모델링에 있어서 overfitting이 발생하지 않도록 하는 것은 매우 중요하다. 

