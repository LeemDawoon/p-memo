---
description: 'EfficientDet: Scalable and Efficient Object Detection'
---

# \_\(2019\)EfficientDet

* [https://github.com/xuannianz/EfficientDet](https://github.com/xuannianz/EfficientDet)





## BiFPN

BiFPN: efficient bidirectional cross-scale connections and weighted feature fusion.

![](../.gitbook/assets/image%20%2823%29.png)

### 1.Problem Formulation

*  Multi-scale feature fusion의 목적은 다른 resolution의 feature를 통합하는 것이다.
* 이를 공식화 하면 다음과 같다:

![](../.gitbook/assets/image%20%2859%29.png)

* 우리의 목표는 multi-scale feature의 리스트인 P-in 이 주어 졌을때, 그것을 효과적으로 통합하여 P-out을 내뱉는 함수 f를 찾는 것이다.

![](../.gitbook/assets/image%20%284%29.png)

* P-in의 하위 첨자는 레벨을 뜻한다.
* 전통적인 FPN은 multi-scale features를 top-down 방식으로 통합한다.

![](../.gitbook/assets/image%20%2860%29.png)

### 2. Cross-Scale Connections

* top-down 방식의  FPN은 정보의 흐름이 한방향으로 제한된다.
* 이러한 이슈를 다루기 위해 **PANet**은 extra bottom-up path aggregation network을 추가 했다\(Figure 2\(b\)\). 
* Cross-scale connections은 여러 다른 논문에서 연구되어 왔는데, 최근에 **NAS-FPN**에서는 더 나은 Cross-scale connections을 찾기 위해 'neural architecture search to search'를 사용하였다\(Figure 2\(c\)\).
* 
## Experiments

* optimizer
  * SGD
  * momentum 0.9 
  *  weight decay 4e-5
* Learning rate is first linearly increased from 0 to 0.08 in the initial 5% warm-up training steps and then annealed down using cosine decay rule.
* Batch normalization is added after every convolution with batch norm decay 0.997 and epsilon 1e-4.
* We use exponential moving average with decay 0.9998. We also employ commonly-used focal loss \[17\] with α = 0.25 and γ = 1.5, and aspect ratio {1/2, 1, 2}.

