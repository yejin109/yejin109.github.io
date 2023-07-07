---
title: "[Paper] Deriving Language Models from Masked Language Models"
toc: true
toc_sticky: true
toc_lable: "Main Contents"
use_math: true
categories:
  - Information Theory
tags:
  - [Random Field, Stochastic Process]
---

Deriving Language Models from Masked Language Models이란 논문에 대한 리뷰로 기존 Masked Language Model(MLM)에서 joint distribution을 계산하기 위해 unary conditional을 이용한 여러 방법들(Markov Random Field 및 기타 다른 방법)을 P-PPL, U-PPL 등의 평가지표를 기준으로 비교하였고, 향후 학습에 있어 MLM에서 conditional independence 가정을 완화하기 위한 regularization을 제안하였습니다.

이를 이해하기 위한 추가적인 포스트도 함께 공유드립니다!

- Random Field [Notion Link](https://yejin109.notion.site/Random-Field-eb40b139c4804608a685e2e2ec986233?pvs=4)

- Paper Review post [Notion Link](https://yejin109.notion.site/Deriving-Language-Models-from-Masked-Language-Models-19450703008b49909872c5319c3e71b9?pvs=4)
