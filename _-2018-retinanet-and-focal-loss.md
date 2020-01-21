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

![cross entropy \(CE\) loss for binary classification](.gitbook/assets/image%20%285%29.png)

* y: 0 또는 1의 값\(Ground Truth\)
* p: 0~1 사이의 값\(예측확률\)

![&#xD3B8;&#xC758;&#xB97C; &#xC704;&#xD574; pt&#xB97C; &#xC815;&#xC758;\)](.gitbook/assets/image%20%2815%29.png)

*  CE\(p, y\) = CE\(pt\) = − log\(pt\)



1. Balanced Cross Entropy

![](.gitbook/assets/image%20%2823%29.png)

* 불균형을 다룰때, 보통은 weight로 클래스 빈도수의 역수인  α 를 곱해준다.

2. Focal Loss Definition

![](.gitbook/assets/image%20%2822%29.png)

* .

RetinaNet Detector

![](.gitbook/assets/image%20%281%29.png)

* Feature Pyramid Network Backbone
  *  피라미드 레벨을 P3에서 P7로.
    *  숫자의 의미는 입력 이미지 크기에 대해 2의 승수만큼 작다는 것.
  *  모든 피라미드 레벨의 채널 C는 256 으로 동일.
* Anchors
  *  RPN\(Region Proposal Network\) 에서 사용하는 translation-invariant anchor box\(이동 불변 앵커박스\) 를 사용.
  * The anchors have areas of 322 to 5122 on pyramid levels P3 to P7, respectively. As in \[20\], at each pyramid level we use anchors at three aspect ratios {1:2, 1:1, 2:1}.
  * 
* Classification Subnet
* Box Regression Subnet
* 




