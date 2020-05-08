---
description: A Simple Baseline for Multi-Object Tracking
---

# \(2020\) One-shot MOT

* 논문링크: [https://arxiv.org/pdf/2004.01888v2.pdf](https://arxiv.org/pdf/2004.01888v2.pdf)
* 코드링크: [https://github.com/ifzhang/FairMOT](https://github.com/ifzhang/FairMOT)

## 1 Introduction

* Multi-Object Tracking \(MOT\)의 최신모델들은 2개의 모델로 문제를 나누어 다룬다.
  * \(1\) detection: bbox로 관심 물체를 localize 하는 모델.
  * \(2\) association: bbox로 부터 Re-identification \(Re-ID\) features를 추출하고, 기존에 추척하던 거랑 연결하는 모델.
* 이 두 가지를 합처서 구성하는 one-shot 방법이 속도 측면에서 더 좋아보이지만, two-step 방법 보다 성능이 많이 떨어진다.
* 원인이 뭔지 기존 연구들을 살펴봤는데, 3가지 요소를 찾았다.
  * **\(1\) Anchors don’t fit Re-ID**
    * 현재 one-shot trackers는 object detectors로 부터 수정하였기 때문에 모두 anchors 기반의 모델이다.
    * 하지만 anchor는 Re-ID features 를 학습하는데 부적절하다 \(2가지 이유가 있음\).
    * 첫번째 이유는, 서로 다른 이미지 패치에 상응하는 multiple anchors는 동일한 물체의 identity를 추정하는데 책임이 있으며, 이는 network에 심각한 모호성을 야기한다 \(see FIg. 1\).
    * 두번째 이유는, 일반적으로 feature map이 8번씩 down-sampling 되는데, detection에는 적합할지 몰라도 ReID에는 너무 거칠다 \(coarse\).
    * 제안방법:
      * We solve the problem by treating the MOT problem as a pixel-wise keypoint \(object center\) estimation and identity classification problem on top of a high-resolution feature map.
  * **\(2\) Multi-Layer Feature Aggregation**
    * 이는 scale variation 을 다루 능력이 향상되어 one-shot 방법의 ID 스위치를 줄이는 데 도움이된다.
  * **\(3\) Dimensionality of the ReID Features**
    * ReID 보다 학습 데이터가 적어서  ReID feature는 차원 수가 작을 수록 더 좋았다. 

![](../.gitbook/assets/image%20%28162%29.png)



* 위 요소를 분석해서, 적용한 simple baseline을 제안한다.

![](../.gitbook/assets/image%20%28124%29.png)

* high-resolution feature map 에서 object center를 추정하기 위해, anchor-free object detection 방식을 채택한다.
* object의 id를 예측하는데 사용되는 pixel-wise Re-ID features를 추정하기 위해, 병렬적으로 branch 하나를 추가한다.
* 이 branch 는 low-dimensional Re-ID features 을 학습하여, 계산 속도도 빠르고, robust하다.
* 그리고 다양한 스케일의 물체를 다루기 위해,  multiple layers feature를 결합하는 Deep Layer Aggregation operator를 사용하여 backbone을 구성한다.

## 2 Related Work

2.1 Two-Step MOT Methods

2.2 One-Shot MOT Methods



## 3 The Technical Approach

### 3.1 Backbone Network

* DLA-34의 특징
  * **ResNet-34:** 정확도와 속도의 적절한 균형을 위해.
  * **Deep Layer Aggregation \(DLA\):** original DLA와 다르게 low-level 과  high-level features간 더 많은 skip connection을 갖고 있다. 이건 Feature Pyramid Network \(FPN\)와 비슷하다.
  * **deformable convolution layers:** up-sampling module 에 있는 convolution layer는 deformable convolution layers 로 교체되었다. 따라서, 물체의 크기나 포즈에 따라 receptive field를 동적으로 조절할 수 있다.
*  DLA-34의 output feature map의 크기는 입력 이미지의 1/4 배이다.

### 3.2 Object Detection Branch

* bbox의 center를 추정하기 떄문에,  3개의 병렬 regression head를 가진다.
* 각 head는 backbone의 output feature를 입력으로 하여, 3x3 conv\(n\_filter=256\)과 1x1 conv를 거친다.
* Heatmap Head: 
* Center Offset Head:
* Box Size Head: 

### 3.3 Identity Embedding Branch

* 다른 물체를 구별할 수 있는 feature를 만드는 branch 이다.
* 이상적으로 동일한 물체간의 distance는 distance보다 커야 한다.
* 각 location 마다  identity embedding features를 추출하기 위해, conv\(n\_filter=128\)를 적용하였다.
* 이 결과 feature map은 128 X W X H의 차원을 가진다.

### 3.4 Loss Functions

* Heatmap Loss:
  * 
* Offset and Size Loss:
* Identity Embedding Loss:
* 




