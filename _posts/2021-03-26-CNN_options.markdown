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
이번 포스팅의 주제는 CNN 모델에서의  Dropout layer, batch size 이다. 관련된 논문 요약과 더불어 이 옵션들이 MNIST 데이터에 적용했을 때 모델 성능에 어떤 영향을 미치는지 알아보자!

<br>

# 1. Dropout Layer
