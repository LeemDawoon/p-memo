---
description: Pyramid Scene Parsing Network
---

# \(2017\) PSPNet

* 논문링크: [https://arxiv.org/pdf/1612.01105.pdf](https://arxiv.org/pdf/1612.01105.pdf)
* 참조자료:
  * [https://m.blog.naver.com/sogangori/220952866661](https://m.blog.naver.com/sogangori/220952866661)



1.Introduction

* Our main contributions are threefold. 
  * • We propose a pyramid scene parsing network to embed difficult scenery context features in an FCN based pixel prediction framework. 
  * • We develop an effective optimization strategy for deep ResNet \[13\] based on deeply supervised loss. 
  * • We build a practical system for state-of-the-art scene parsing and semantic segmentation where all crucial implementation details are included.

2. Related Work

* FCN
* dilated CNN
* multi-scale feature
* conditional random field

## 3. Pyramid Scene Parsing Network

### 3.1. Important Observations

* 1\) Mismatched Relationship
* 2\) Confusion Categories
* 3\) Inconspicuous Classes
* To summarize these observations, many errors are partially or completely related to **contextual relationship** and **global information** for **different receptive fields**. Thus a deep network with a suitable global-scene-level prior can much improve the performance of scene parsing.

### 3.2. Pyramid Pooling Module

* DNN에서,  receptive field의 크기는 대략적으로 context information를 얼마나 사용할지를 나타낸다.
* 이론적으로 ResNet의 receptive field는 입력 이미지보다 큰데, 실제적인\(empirical\) 크기는 더 작다.
* 특히 high-level layer일수록 더 그렇다.
* 어런 특성은 딥러닝 네트워크가 큰 global scenery prior를 충분히 통합하지 못한다.
* 이러한 이슈를 해결하기 위해, 본 논문은 효과적인 global prior representation을 제안한다.



* Global average pooling\(GAP\)은 global contextual prior의 좋은 baseline 모델이다.
* 주로 classification 분야에 쓰였는데, segmentation에도잘 적용되었다. 하지만, 복잡한 이미지에서 GAP는 필요한 정보를 다루기에 불충분하다.
* 여러 물체가 있는 복잡한 이미지를 싱글벡터로 융합하면, 공간적 관계 정보를 잃고 모호하게 된다.
* Global context information along with sub-region context는 다양한 카테고리를 구별하는데 도움이 된다.
* A more powerful representation could be fused information from different sub-regions with these receptive fields
* \(이러한 receptive fields를 통해 다른 하위 지역의 정보를보다 강력하게 표현할 수 있습니다.\)



* \[Spatial pyramid pooling in deep convolutional networks for visual recognition\] 연구에서, pyramid pooling을 통해 만들어진 다른 level의 feature map들은 flatten된 후, concat 되어 fully connected layer로 입력된다.
* 이러한 global prior는 CNN의 fixed-size 제약\(같은 크기 입력?!\)을 제거하기 위해 디자인 되었다.
* 다른 sub-regions 간 context information 손실을 줄이기 위해서, 본 논문은 hierarchical global prior을 제안한다.
* hierarchical global prior은 sub-regions에 따라 다른 크기의 정보를 포함한다. 
* 이걸 pyramid pooling module이라고 한다.

![PSPNet](../.gitbook/assets/image%20%2892%29.png)

* pyramid pooling module은  4가지 스케일의 feature를 융합한다.
* 1\) pyramid
  * 위 그림에서 빨간색 부분은,  coarsest level로 한개의 bin output을 내뱉는 global pooling이다.
  * 나머지 pyramid level은 feature map을 different sub-regions으로 나누고, different location에 pooled representation을 형성한다.
  * pyramid pooling module의 different level의 output은 다양한 크기의 feature map을 포함한다.
* 2\) 1x1 conv
  * the weight of global feature를 유지하기 위해서, 각 pyramid level 마다 1x1 conv 레이어를 사용한다. 
  * 이는 pyramid의 level size가 N이라고 한다면,  원본 이미지의 1/N개로 context representation 차원을 줄인다.
    * \(b\)의 feature map는 지역적 정보를 가지고 있고, \(c\)를 통해 나온 feature map은 정역적 정보를 가지고 있는데,
    * 위와 같이 차원을 줄여 줘야 지역특징과 전역 특징의 비율이 5:5 비율을 유지할 수 있다.
* 3\) upsample
  * 그리고 original feature map과 동일한 크기로 upsampling 한다.
  *  bilinear interpolation 사용.
* 4\) concat
  * 마지막으로 different levels의 feature 들은 concat되어 최종 pyramid pooling global feature가 된다.



* pyramid levels의 수과 각 level의 size는 바뀔 수 있다.
* 이는 pyramid pooling layer의 입력이되는 feature map의 size와 관련이 있다.
* The structure abstracts different sub-regions by adopting varying-size pooling kernels in a few strides 
* \(이 구조는 다양한 pooling kernels 크기\(in a few strides \)를 채택하 different sub-regions을 추상화한다.\)
* 그러므로 이 multi-stage 커널은 representation에서  reasonable gap을 유지해야 한다.
* 본 논문의 Our pyramid pooling module은 4-레벨이고, 각 bin의 size는 1×1, 2×2, 3×3, 6×6 이다.
* pooling 연산자의 종류는 max pooling과 average pooling을 사용하여 실험하였다.

3.3. Network Architecture

* **pyramid scene parsing network \(PSPNet\)**
* \(b\)의 feature map을 뽑는데, dilated network strategy의 ResNet pretrained 모델을 사용하였다.
* ResNet 모델의 최종 feature map 크기는 입력 이미지의 1/8 크기 이다.  
* 


## 5. Experiments

![](../.gitbook/assets/image%20%28107%29.png)

 



