---
description: Focal Loss for Dense Object Detection
---

# \_\(2018\) RetinaNet & Focal Loss

* 논문링크:  [https://arxiv.org/pdf/1708.02002.pdf](https://arxiv.org/pdf/1708.02002.pdf)
* 참조 블로그 및 페이지:
  * [https://uk-kim.github.io/2018/12/07/Focal-loss-for-dense-object-detection.html](https://uk-kim.github.io/2018/12/07/Focal-loss-for-dense-object-detection.html)



Abstract

* one-stage detectors 학습에서 전경과 배경이 극단적으로 불균형\(imbalance\)한 것이 성능 저하의 주요 원인
* 이러한 class imbalance한 문제를 보통의 cross entropy loss를 잘 분류된 샘플에 대해서 가중치를 낮추는 방식으로 변형하여 해결하는 방법을 제안.

Focal Loss

* Focal Loss는 학습 중에서 전경과 배경이 극도로 imbalance\(예, 1:1000\)한 one-stage Object Detection 시나리오를 다루기 위해 설계되었다.

![cross entropy \(CE\) loss for binary classification](.gitbook/assets/image%20%284%29.png)

* y: 0 또는 1의 값\(Ground Truth\)
* p: 0~1 사이의 값\(예측확률\)

![&#xD3B8;&#xC758;&#xB97C; &#xC704;&#xD574; pt&#xB97C; &#xC815;&#xC758;\)](.gitbook/assets/image%20%2814%29.png)

*  CE\(p, y\) = CE\(pt\) = − log\(pt\)
* 


