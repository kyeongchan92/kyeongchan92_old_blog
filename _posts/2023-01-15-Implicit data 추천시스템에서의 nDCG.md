---
title: Implicit data 추천시스템에서의 nDCG
description:
categories:
 - Recommendation System Basics
tags:nDCG, implicit data, implicit feedback
---

$$ NDCG@K = Z_K \sum_{i=1}^{K} frac{2^{rel_i****}-1}{\log_2(i+1)} $$

$Z_K$는 제일 좋은 성능일 때를 1로 만들기 위한 normalizer이다. $rel_i$는 i번째 아이템의 graded relavance이다. (implicit data이기 때문에) rel_i는 1 또는 0이며, 1일 때는 아이템이 test set에 존재할 때이고, 0일 때는 그렇지 않을 때이다.

![0](/Users/kyeongchanlee/PycharmProjects/kyeongchan92.github.io/assets/images/2023-01-15-Implicit data 추천시스템에서의 nDCG/0.png)