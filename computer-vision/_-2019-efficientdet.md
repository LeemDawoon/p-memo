---
description: 'EfficientDet: Scalable and Efficient Object Detection'
---

# \(2019\) EfficientDet

* 논문링크: [https://arxiv.org/pdf/1911.09070.pdf](https://arxiv.org/pdf/1911.09070.pdf)
* 참조 블로그 및 페이지:
  * [https://github.com/xuannianz/EfficientDet](https://github.com/xuannianz/EfficientDet)

## 3. BiFPN

BiFPN: efficient bidirectional cross-scale connections and weighted feature fusion.

![](../.gitbook/assets/image%20%2848%29.png)

### 3.1.Problem Formulation

*  Multi-scale feature fusion의 목적은 다른 resolution의 feature를 통합하는 것이다.
* 이를 공식화 하면 다음과 같다:

![](../.gitbook/assets/image%20%28136%29.png)

* 우리의 목표는 multi-scale feature의 리스트인 P-in 이 주어 졌을때, 그것을 효과적으로 통합하여 P-out을 내뱉는 함수 f를 찾는 것이다.

![](../.gitbook/assets/image%20%288%29.png)

* P-in의 하위 첨자는 레벨을 뜻한다.
* 전통적인 FPN은 multi-scale features를 top-down 방식으로 통합한다.

![](../.gitbook/assets/image%20%28139%29.png)

### 3.2. Cross-Scale Connections

* top-down 방식의  **FPN**은 정보의 흐름이 한방향으로 제한된다.
* 이러한 이슈를 다루기 위해 **PANet**은 extra bottom-up path aggregation network을 추가 했다\(Figure 2\(b\)\). 
* Cross-scale connections은 여러 다른 논문에서 연구되어 왔는데, 최근에 **NAS-FPN**에서는 더 나은 Cross-scale connections을 찾기 위해 'neural architecture search to search'를 사용하였다\(Figure 2\(c\)\).
*  본 논문의 실험에서 보면, PANet이 FPN과 NAS-FPN보다 성능이 좋았는데, 파라미터 수가 더 많고 계산이 더 많이 필요하다는 단점이 있었다. 
* 이를 극복하기 위한 최적화 방법 몇가지를 소개한다. - bidirectional feature pyramid network \(BiFPN\)



* 1\) 1개의 input edge를 가진 노드를 제거한다 - Figure 2\(e\)
  * 왜나면, input edge가 1개 뿐이라면, feature fusion을 할 필요가 없다.
* 2\)  같은 level에서 original input 에서 output node 까지 가는 extra edge를 하나 추가한다 - Figure 2\(f\)
  * 이는 적은 비용으로 feature fusion을  더 하기 위함이다.
* 3\) PANet과는 다르게, \(PANet는 top-down\), bottom-up path도 다룬다.
  * Third, unlike PANet \[19\] that only has one top-down and one bottom-up path, we treat each bidirectional \(top-down & bottom-up\) path as one feature network layer, and repeat the same layer multiple times to enable more high-level feature fusion. 

### 3.3. Weighted Feature Fusion

* 보통 사이즈가 다른 여러 개의 input feature를 fusion 할때는, 동일한 사이즈로 변경한 후, 더 하는 방식으로 처리한다.
* 그런데, input feature 들은 output feature에 동일하게 기여하지 않는다.
* 이를 다루기 위해 본 논문에서는, 추가적인 가중치 레이어를 학습하도록 한다. 
*  weighted fusion의 3가지 접근 방법을 다룬다. 

#### Unbounded fusion:

![](../.gitbook/assets/image%20%282%29.png)

* w가 학습할 가중치인데, 이게 크기에 제약이 없어서 학습이 좀 불안정하다.
* 그래서 정규화를 통해 범위를 좀 제한한다.

#### Softmax-based fusion:

![](../.gitbook/assets/image%20%28122%29.png)

* weight에 softmax를 적용하는 방법이다.
* 하지만 계산 비용이 너무 많이든다.

#### Fast normalized fusion:

![](../.gitbook/assets/image%20%2850%29.png)

* w 다음에 relu를 적용해서, w는 0이상의 값을 가진다.
* 분모의 입실론으 0.0001 과 같은 값을 가지는데, 분모가 0인 상황 방지용이다.
* 0~1사의 값으로 normalize 되는데, softmax 연산이 없어서 효율적이다.
* 효과는 softmax와 유사한데, 30%  더 빠르다고 한다.



* 다음은 최종 BiFPN, level 6 에서의 예시이다.
* bidirectional cross-scale connections과 fast normalized fusion을 합친거라고 보면된다.
* 첫번째 식은 top-down pathway에서 중간 결과 feature이며, 
* 마지막 식은 bottom-up pathway에서 output feature 이다.
* 효율성을 더 높이기 위해, feature fusion에서 depthwise separable convolution을 사용하였고,  add batch normalization 과 activation을 각 convolution 마다 추가하였다.

![](../.gitbook/assets/image%20%28109%29.png)

![](../.gitbook/assets/image%20%2860%29.png)

* 빨간색 박스를 첫번째 식에, 파란색 박스를 두번째 식에 매칭시켜서 이해하면 되겠다.

## 4. EfficientDet

* BiFPN 기반으로 detection EfficientDet이라는 모델을 만들었다.

### 4.1. EfficientDet Architecture

![](../.gitbook/assets/image%20%28118%29.png)

* EfficientDet는 크게 one-stage detector의 패러다임을 따른다.
* ImageNet으로 pretrained된 EfficientNet을 백본으로 사용하였다.
* \(전체적인 구조는 RetinaNet과 거의 동일하다.\)

### 4.2. Compound Scaling

* 우리가 고려해야할 조합들은 아래와 같다.
  * Backbone network
  * BiFPN network
  * Box/class prediction network
  * Input image resolution
* 모든 조합을 gridsearch로 찾는 것은 매우 힘든데,  본 논문에선 heuristic-based scaling 접근 방법을 사용하였다. 살펴보자.
* 1\)  Backbone network
  * EfficientNet-B0 to B6
* 2\) BiFPN network
  * 

![](../.gitbook/assets/image%20%2897%29.png)

* 3\) Box/class prediction network
* 
![](../.gitbook/assets/image%20%2814%29.png)

* 4\) Input image resolution
  * 

![](../.gitbook/assets/image%20%2846%29.png)



![](../.gitbook/assets/image%20%2873%29.png)









## Experiments

* optimizer
  * SGD
  * momentum 0.9 
  *  weight decay 4e-5
* Learning rate is first linearly increased from 0 to 0.08 in the initial 5% warm-up training steps and then annealed down using cosine decay rule.
* Batch normalization is added after every convolution with batch norm decay 0.997 and epsilon 1e-4.
* We use exponential moving average with decay 0.9998. We also employ commonly-used focal loss \[17\] with α = 0.25 and γ = 1.5, and aspect ratio {1/2, 1, 2}.

