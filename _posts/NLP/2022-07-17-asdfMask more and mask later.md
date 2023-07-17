---
title: "[Paper] Mask more and mask later: Efficient Pre-training of Masked Language Models by Disentangling the [MASK] Token"
toc: false
toc_sticky: false
toc_lable: "Main Contents"
use_math: true
categories:
  - NLP
tags:
  - [Masked Language Model]
---

ACL 2022년의 "Mask more and mask later: Efficient Pre-training of Masked Language Models by Disentangling the [MASK] Token"이란 논문에 대한 리뷰입니다.

본 페이퍼에서는 하나의 token은 2가지의 정보를 가지고 있다고 보고 있습니다.
- token information
- position information

그리고 흔히 사용되는 attention 연산은 각 token이 가진 information의 flow를 만들어 주며 Masking은 4가지의 flow를 만든다고 합니다.
- 2가지 entity(masked & unmasked) * 2가지 flow(self loop & transfer)

다만 중요한 것은 1)unmasked token 간에서 정보를 모아서 context를 만드는 flow와 2)이를 masked로 transfer하는 flow가 됩니다. 즉 attention연산에서 얻고자 하는 정보 흐름이 앞서 말한 2가지 흐름이 됩니다. . 그렇기에 이 flow에 집중을 하도록 모델 구조를 바꾼다면 efficient한 알고리즘이 될 것으로 생각하고 고안하게 되었습니다.

그리고 masking rate이 이러한 flow를 결정하는 값이 되기에 mask more라는 이름이 붙여 진 것으로, 흔히 사용하는 masking rate보다 더 많은 masking을 가져야 한다고 주장합니다.

또한 mask 토큰의 information flow를 무시하고 최종 estimation할 때 embedding을 가져와서 사용하기 때문에 mask later라는 이름이 붙여진 것. 

- Paper Review post [Notion Link](https://yejin109.notion.site/Mask-more-and-mask-later-Efficient-Pre-training-of-Masked-Language-Models-by-Disentangling-the-MAS-c68148e850d34528aaa2be5b2f566e7f?pvs=4)
