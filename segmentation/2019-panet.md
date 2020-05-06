---
description: 'PANet: Few-Shot Image Semantic Segmentation with Prototype Alignment'
---

# \(2019\) PANet

* 논문링크: ​[http://openaccess.thecvf.com/content\_ICCV\_2019/papers/Wang\_PANet\_Few-Shot\_Image\_Semantic\_Segmentation\_With\_Prototype\_Alignment\_ICCV\_2019\_paper.pdf](http://openaccess.thecvf.com/content_ICCV_2019/papers/Wang_PANet_Few-Shot_Image_Semantic_Segmentation_With_Prototype_Alignment_ICCV_2019_paper.pdf)
* 코드링크:  [https://github.com/kaixin96/PANet](https://github.com/kaixin96/PANet)

## **3.** Method

### 3.1. Problem setting

**C-way K-shot segmentation.**

* $$C_{seen} $$ : 학습에 사용되는 클래스.
* $$C_{unseen} $$ : 학습에 사용되지 않는 클래스.
* $$ D_{train}$$: $$ C_{seen} $$ 으로 부터 구성.  몇개의 _episode_ 로 구성.
* $$ D_{test}$$:$$ = \{(S_i, Q_i)\}_{i=1}^{N_{test}}$$
  * $$ C_{unseen} $$ 으로 부터 구성.  몇개의 _episode_ 로 구성.
  * $$ N $$: episode 
* episode:  $$=  (S_i, Q_i)$$
  * support 이미지 $$S$$ \(with annotation\)과 와 query 이미지셋 $$Q$$로 구성.
* $$ S_i = \{(I_{c,k}, M_{c,k})\}$$, K개의 &lt;image, mask&gt;
  * $$  $$$$c \in C_i  \quad , \quad |C_i| = C$$
  * $$k=1, 2, \dots , K $$
* $$Q_i$$:$$N_{query}$$개의 &lt;image, mask&gt;
* 각 episode는 다른 semantic 클래스를 가진다.테스트 할때는 각 episode별 주어진 support set에대한 query을 평가한다.

### 3.2. Method overview

* 아래그림.
* VGG-16을 feature extractor로 쓰는데, 조금 바꿈.
  * 처음 5 convolutional blocks을 씀.
  * maxpool4 layer의 stride를 1로\(해상도 보존\).
  * conv5 block을 dilation=2인 dilated conv conv 로 교체\(receptive field 증\).

![](../.gitbook/assets/image%20%2846%29.png)

### 3.3. Prototype learning

* c클래스의  prototype은 masked average pooling으로 계산된다:
  * F는 입력 이미지의 feature map.
  * \(x, y\)는 이미지 좌표.

![](../.gitbook/assets/image%20%28152%29.png)

![](../.gitbook/assets/image%20%28127%29.png)

### 3.4. Non-parametric metric learning

* ​모델의 확률 map:
  * query 이미지의 feature 와 prototype 간의 distance를 계산하고, softmax를 취함.
  * distance는 cosine distance로 계산.
  * 알파는 실험적으로 20 으로 정해줌. 



![](../.gitbook/assets/image%20%28121%29.png)

* 예측 segmentation mask:

![](../.gitbook/assets/image%20%28106%29.png)

* Loss:

![](../.gitbook/assets/image%20%283%29.png)



### 3.5. Prototype alignment regularization \(PAR\)

* support image I\_{c,k}의 segmentation 확률:

![](../.gitbook/assets/image%20%2874%29.png)

* 전체 Loss:



![](../.gitbook/assets/image%20%2828%29.png)

![](../.gitbook/assets/image%20%2879%29.png)

* 알고리즘:

![](../.gitbook/assets/image%20%2858%29.png)









