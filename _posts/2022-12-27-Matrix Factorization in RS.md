---
title: 추천시스템에서의 Matrix Factorization
description:
categories:
tags:
---

MF는 유저와 아이템을 latent feature의 실수 벡터와 연관짓는다. $\mathbf{p}_u$와 $\mathbf{q}_i$가 유저 와 아이템 를 나타낸다고 해보자. MF는 $\mathbf{p}_u$와 $\mathbf{q}_i$를 내적하여 상호작용 $y_{ui}$를 계산한다.