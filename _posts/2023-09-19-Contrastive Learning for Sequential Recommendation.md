---
title: [논문리뷰] Contrastive Learning for Sequential Recommendation
description:
categories:
tags:
---

논문 : [Contrastive Learning for Sequential Recommendation
](https://arxiv.org/pdf/2010.14395.pdf)

# 개요
Contrastive learning을 유저의 아이템 시퀀스에 적용함으로써 향상된 user representation을 얻음.

# Self-supervised Learning
self-supervised learning은 라벨없는 데이터에서 representation을 학습하는데에 우월하다고 한다. 기본 아이디어는 pretext task로 training signal을 구축하는 것이다. 

# Contrastive Learning Framework

크게 세 가지로 구성된다.

1. Data Augmentation Module
각 데이터마다 2개의 augmentation 데이터를 만든다.

2. User Representation Encoder
인코더로 트랜스포머를 사용한다. augmented sequences로부터 유저 representation을 추출한다.

3. Contrastive Loss Function
손실함수는 두 representation이 같은 유저 시퀀스로부터 추출된 것인지 구별한다.
최소화 : 한 명의 유저 시퀀스로부터 다른 두 개의 증강 관점
최대화 : 다른 유저로부터 증강된 시퀀스간의 차이

$$\mathcal{L}_{\text{cl}}(s_u^{a_i}, s_u^{a_j})=-\log \frac{\exp(\sim(s_u^{a_i}, s_u^{a_j}))}{\exp(\sim(s_u^{a_i}, s_u^{a_j}))+\sum_{s^- \in S^-}\exp(\sim(s_u^{a_i}, s_u^-))}$$

$sim(u, v)=u^Tv$, 즉 내적이다. 손실을 작게 한다 = -를 뗀 log를 크게 한다 = $s_u^{a_i}$와 $s_u^{a_i}$를 유사하게 한다 + $S^-$에 속한 것과는 멀게 한다가 되겠다.

# Data Augmentation Operators

$\mathcal{A}$를 augmentation들의 집합이라고 하자.

1. Crop
시퀀스 내부의 연속된 일부만 떼어낸다. 예를 들어 $[v_1, v_2, v_3, v_4, v_5, v_6, v_7]$이라는 7개의 아이템으로 구성된 시퀀스가 있고, 전체 길이 7의 0.6만큼만 crop한다고 하고(내림하여 4개), 2번 아이템부터 crop한다고 했을때 augmentation 결과는 $[v_2, v_3, v_4, v_5]$가 된다.
2. Mask
일부 아이템을 [mask]라는 스페셜 아이템으로 변환한다. 만약 시퀀스 길이의 0.3만큼만 마스킹하고싶다면 7 X 0.3 = 2.1을 내림한 2개를 랜덤으로 마스킹한다.
3. Reorder
시퀀스 내부의 연속된 시퀀스를 무작위 재배치한다. 예를들어 3번 아이템부터 0.6만큼의 아이템, 즉 4개를 재배치한다면 $[v_1, v_2, v_5, v_3, v_6, v_4, v_7]$처럼 된다.

# User Representation Model
인코더 모델로는 SASRec을 사용한다(SASRec 논문리뷰 참고). 유저 representation으로는 마지막 아이템까지 어텐션을 적용한 representation, 즉 $t=|s_u|$(마지막)째 아이템의 representation을 사용한다.

# Multi-task Training
multi-task 전략을 사용한다고 하는데, 다른게 아니라 $\mathcal{L}_{\text{cl}$뿐만 아니라 SASRec 자체의 next item prediction 손실까지 더하겠다는 뜻이다. 이를 $\mathcal{L}_{\text{main}}$이라고 할 때, total loss는 다음과 같다.
$$\mathcal{L}_{\text{total}} = \mathcal{L}_{\text{main}} + \lambda \mathcal{L}_{\text{cl}}$$

&\mathcal{L}_{\text{main}}&은 다음과 같이 정의된다.

$$\mathcal{L}_{\text{main}}(s_{u, t}) = \frac{\exp(s_{u,t}^T v_{t+1}^+)}{\exp(s_{u, t}^T v_{t+1}^+) + \sum_{v_{t+1}^- \in \mathcal{V}^-} \exp(s_{u,t}^T v_{t+1}^-)}$$

$s_{u, t}$는 계산된 유저 representation을 의미하고, $v_{t+1}^+$는 $t+1$에서의 아이템, $v_{t+1}^-$는 무작위 샘플링된 네거티브 아이템을 의미한다.

# Experiments

사용한 데이터셋은 다음과 같다.
![0](/assets/images/cl4srec_dataset_statistics.png)

Crop, mask, reorder에서 사용하는 비율은 다음과 같다.
- eta : $\eta$ : crop
- gamma : $\gamma$ : mask
- beta : $\beta$ : reorder


![0](/assets/images/cl4srec_figure4.png)
