---
description: Focal Loss for Dense Object Detection
---

# \(2018\) RetinaNet & Focal Loss

* 논문링크:  [https://arxiv.org/pdf/1708.02002.pdf](https://arxiv.org/pdf/1708.02002.pdf)
* 참조 블로그 및 페이지:
  * [https://uk-kim.github.io/2018/12/07/Focal-loss-for-dense-object-detection.html](https://uk-kim.github.io/2018/12/07/Focal-loss-for-dense-object-detection.html)

## Abstract

* one-stage detectors 학습에서 전경과 배경이 극단적으로 불균형\(imbalance\)한 것이 성능 저하의 주요 원인이었다.
* 이러한 class imbalance한 문제를 보통의 cross entropy loss를 잘 분류된 샘플에 대해서 가중치를 낮추는 방식으로 변형하여 해결하는 방법을 제안.

## Focal Loss

* Focal Loss는 학습 중에서 전경과 배경이 극도로 imbalance\(예, 1:1000\)한 one-stage Object Detection 시나리오를 다루기 위해 설계되었다.

![cross entropy \(CE\) loss for binary classification](../.gitbook/assets/image%20%2826%29.png)

* y: 0 또는 1의 값\(Ground Truth\)
* p: 0~1 사이의 값\(예측확률\)

![&#xD3B8;&#xC758;&#xB97C; &#xC704;&#xD574; pt&#xB97C; &#xC815;&#xC758;\)](../.gitbook/assets/image%20%2854%29.png)

*  CE\(p, y\) = CE\(pt\) = − log\(pt\)



### 1. Balanced Cross Entropy

![](../.gitbook/assets/image%20%2886%29.png)

* 불균형을 다룰때, 보통은 weight로 클래스 빈도수의 역수인  α 를 곱해준다.

### 2. Focal Loss Definition

![](../.gitbook/assets/image%20%2883%29.png)

* 감마 값이 0이면 일반 Cross Entropy와 동일하다.
* 본 논문에서는 2일때의 실험 결과가 가장 좋다고 한다.
* 아래 그래프를 보면 잘 분류된 데이터에 대해서는 로스가 크게 작아짐

![](../.gitbook/assets/image%20%2856%29.png)

## RetinaNet Detector

![](../.gitbook/assets/image%20%287%29.png)

* Feature Pyramid Network Backbone
  *  피라미드 레벨을 P3에서 P7로.
    *  숫자의 의미는 입력 이미지 크기에 대해 2의 승수만큼 작다는 것.
  *  모든 피라미드 레벨의 채널 C는 256 으로 동일.
* Anchors
  *  RPN\(Region Proposal Network\) 에서 사용하는 translation-invariant anchor box\(이동 불변 앵커박스\) 를 사용.
  * The anchors have areas of 32^2 to 512^2 on pyramid levels P3 to P7, respectively. 
  * anchor는 3가지 aspect ratios {1:2, 1:1, 2:1}를 가진다.
  * 각 pyramid level에서 3가지 aspect ratio에 3가지 anchors of size{2^0, 2^1/3, 2^2/3}를 더한다.
    * 3x3 = 9
  * 각 anchor에는 물체 클래스수인 K개의 one-hot vector와 BBox regression 변수 4-vector가 할당된다.
  * anchor와 GT BBox간 IoU 0.4이하면 배경취급한다.
    * 0.5 이상이면 그 물체를 할당한다.
    * \[0.4 ~0.5\) 에 들어오는 것은 학습시 무시한다.
  * anchor는 최대 1개에 object에 할당될수 있고, 해당 K-클래스의 벡터를 1로 설정해준다.
  * Box regression targets are computed as the offset between each anchor and its assigned object box, or omitted if there is no assignment.
* Classification Subnet
  * 각 position에 대해 각 A anchor\(9개\)와 K-object에 대해 object의 존재확률을 예측하는 작은 FCN.
  * 각 pyramid level에 붙는다.
  * 구조:
    * 4개의 conv layer
      * kernel = 3x3
      * activiation ReLU
      * channel = 256
    * 5-th conv layer
      * 마지막 feature 수는 \(9 x K\)
      * 마지막에 sigmoid
* Box Regression Subnet
  * 각 anchor의 offset을 regression하는 목적.
  * 구조:
    * 4개의 conv layer - classification subnet과 동일.
    * 5-th conv layer
      * 마지막 feature 수는 \(9 x 4\)

Optimization:

* SGD
  * Weight decay of 0.0001 and momentum of 0.9 are used.
* 16 images per minibatch
* , all models are trained for 90k iterations with an initial learning rate of 0.01, which is then divided by 10 at 60k and again at 80k iterations.
* We use horizontal image flipping
* 




