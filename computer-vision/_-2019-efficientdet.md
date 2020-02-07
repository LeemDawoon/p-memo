---
description: 'EfficientDet: Scalable and Efficient Object Detection'
---

# \_\(2019\)EfficientDet

* [https://github.com/xuannianz/EfficientDet](https://github.com/xuannianz/EfficientDet)



## Experiments

* optimizer
  * SGD
  * momentum 0.9 
  *  weight decay 4e-5
* Learning rate is first linearly increased from 0 to 0.08 in the initial 5% warm-up training steps and then annealed down using cosine decay rule.
* Batch normalization is added after every convolution with batch norm decay 0.997 and epsilon 1e-4.
* We use exponential moving average with decay 0.9998. We also employ commonly-used focal loss \[17\] with α = 0.25 and γ = 1.5, and aspect ratio {1/2, 1, 2}.

