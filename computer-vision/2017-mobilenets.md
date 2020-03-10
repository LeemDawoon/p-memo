---
description: >-
  MobileNets: Efficient Convolutional Neural Networks for Mobile Vision
  Applications
---

# \(2017\) MobileNets

* 논문링크: [https://arxiv.org/pdf/1704.04861.pdf](https://arxiv.org/pdf/1704.04861.pdf)
* 참조 블로그 및 페이지:
  * [https://hichoe95.tistory.com/53](https://hichoe95.tistory.com/53)

## **MobileNets 특징:**

### 1. Depthwise separable convolution

* 모델의 크기를 줄이기 위한 목적.

![Figure 2. The standard convolutional filters in \(a\) are replaced by two layers: depthwise convolution in \(b\) and pointwise convolution in \(c\) to build a depthwise separable filter.](../.gitbook/assets/image%20%2886%29.png)

* standard convolution 2\(a\) 는 depthwise convolution 2\(b\) 와  1 × 1 pointwise convolution 2\(c\)로 factorization 된다.
* standard convolution은 convolution kernel에 의해 feature를 filtering하고 combination하는 2가지 스탭을 수행하는 효과를 가진다.
* 이 두 스텝을 위와 같은 factorization을 통해  2개의 convolution 으로 나누면, 동일한 효과를 가지면서, 계산 비용을 줄일 수 있다.



![standard convolution &#xACC4;&#xC0B0;&#xC2DD;\(stride-1, padding-1\)&#xC774;&#xB77C;&#xACE0; &#xAC00;&#xC815;.](../.gitbook/assets/image%20%28105%29.png)

* F: Input feature map
  * **D**F × **D**F × **M**
  * **M**: input channel
  * **D**F: feature map의 size
* G: Output feature map
  * **D**F × **D**F × **N**
  * **N:** output channel
* K: Convolution Kernel
  * **D**K ×**D**K ×**M**×**N**

![&#xC2DD;\(1\)&#xC758;  computational cost ](../.gitbook/assets/image%20%28101%29.png)

* Depthwise separable convolution를 구성하는 2가지 레이어.
  * depthwise convolutions : 
    * input channel을 filtering 하는 효과.
    * m-th 필터\(channel\)에 대해서 계산한 depthwise convolution 결과.
      * ![](../.gitbook/assets/image%20%28125%29.png)
    * 계산 비용:
      * ![](../.gitbook/assets/image%20%2894%29.png)
  * pointwise convolutions: 
    * depthwise convolution의 결과를 linear combination 하는 효과.
    * 새로운 feature 생성.
    * 1 X 1 convolution을 이용.
* Depthwise separable convolutions cost:
  * ![](../.gitbook/assets/image%20%2826%29.png)
    * 좌측텀: depthwise convolutions  계산비용.
    * 우측텀: pointwise convolutions 계산비용.
* 계산 비용이 얼마나 줄어드는지:
  * ![](../.gitbook/assets/image%20%2835%29.png)
  * 예를 들어 3 × 3 depthwise separable convolutions 을 사용하면, standard conv보다 8~9배 정도는 계산비용이 적게 든다.

### 2. Shrinking hyperparameter\(Width mutiplier, Resolution multiplier\)

* Width Multiplier: Thinner Models
  * width multiplier - α는 채널의 수를 조절하는데,  α 값이 1이면 baseline MobileNet 이고, 1보다 작으면, reduced MobileNets이다.
  * 계산 비용을 아래 식처럼 조절할 수 있음:
    * ![](../.gitbook/assets/image%20%2831%29.png)
  * \(1, 0.75, 0.5, 0.25\)
* Resolution Multiplier: Reduced Representation

  * resolution multiplier - ρ는 입력 이미지의 크기와 feature map에 곱해져서 크기를 조절할 수 있는 인자.
  * \(224, 192, 160 or 128\)

* α 와 ρ 가 둘다 포함되어 있을 때, 계산비용:
  * ![](../.gitbook/assets/image%20%2880%29.png)

### 3. 전체구조

![](../.gitbook/assets/image%20%2846%29.png)

![](../.gitbook/assets/image%20%2818%29.png)



