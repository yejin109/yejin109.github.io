---
title: "[Paper] Calibration, Entropy Rates, and Memory in Language Models"
toc: true
toc_sticky: true
toc_lable: "Main Contents"
use_math: true
categories:
  - Information Theory
tags:
  - [Infromation Theory, Entropy rate]
---

Calibration, Entropy Rates, and Memory in Language Models이란 논문에 대한 리뷰입니다.
기존의 연구들은 long term property를 반영하기 위해 architecture등을 제안했지만 perplexity와 같은 사용하고 있던 metric과 cross entropy와 같은 loss는 정작 long term property에 대한 영향을 주고 있지 않았습니다.

그래서 entropy rate을 사용해서 calibration을 한다면 기존에 사용하던 training regime을 개선함과 동시에 long term property를 반영하고 있는 entropy rate도 개선할 수 있음을 이론적인 근거와 실험적 결과로 페이퍼에서 제시하였습니다. 
- 실제로 Theorem 4.4에서 upper bound가 개선되는 것을 보였습니다.
- Figure 2에서 calibration을 거친 경우 perplexity가 개선된 것을 확인할 수 있습니다.

Language model에서 memory라는 개념을 반영한 정의를 utual information을 바탕으로 제시, 실험적으로 text generation과정에서 recent past에 보다 더 많은 정보를 사용하고 있음을 보였습니다.

이를 이해하기 위한 추가적인 포스트도 함께 공유드립니다!

- Paper Review post [Notion Link](https://yejin109.notion.site/Calibration-Entropy-Rates-and-Memory-in-Language-Models-4dc118ffcf8b492490a75531d6f04263?pvs=4)
