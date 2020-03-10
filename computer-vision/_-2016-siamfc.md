---
description: Fully-Convolutional Siamese Networks for Object Tracking
---

# \_\(2016\) SiamFC

* 논문링크: [https://arxiv.org/pdf/1606.09549.pdf](https://arxiv.org/pdf/1606.09549.pdf)

2 Deep similarity learning for tracking

* f\(z, x\) 란 함수를 학습하는 모델을 제안.
  * f\(z, x\)는  exemplar image z 와 candidate image x 를 비교하는 함수인데, 두 이미지가 동일한 object를 나타내면, 높은 점수를 아니면, 낮은 점수를 리턴한다.
* 
![](../.gitbook/assets/image%20%2881%29.png)

* g는 거리나, 유사도 메트릭 함수로 볼 수 있다.
* ϕ 는 임베딩함 수로 볼 수 있다.
* Deep Siamese conv-nets 은 이전에, 이런 연구들에 쓰여왔다.
  * face verification
  * keypoint descriptor learning
  * one-shot character recognition

2.1 Fully-convolutional Siamese architecture

![](../.gitbook/assets/image%20%2872%29.png)



2.2 Training with large search images

