---
description: Deep Residual Learning for Image Recognition
---

# \(2015\) ResNet

* 논문링크: [https://arxiv.org/pdf/1512.03385.pdf](https://arxiv.org/pdf/1512.03385.pdf)
* 참조 블로그 및 페이지:
  * [https://datascienceschool.net/view-notebook/958022040c544257aa7ba88643d6c032/](https://datascienceschool.net/view-notebook/958022040c544257aa7ba88643d6c032/)
  * [https://dalpo0814.tistory.com/24](https://dalpo0814.tistory.com/24)
  * [http://openresearch.ai/t/resnet-deep-residual-learning-for-image-recognition/41](http://openresearch.ai/t/resnet-deep-residual-learning-for-image-recognition/41)



## 레이어가 많을수록 좋을까?

* 아니오: degradation problem
  * 성능이 빠르게 수렴하고, 하락하는데, 이는 오퍼피팅 때문이 아니라 그냥 학습이 잘안되서 높은 Training 에러를 갖는다.
  * gradient vanishing/exploding 문제 때문이라고 볼수 있다. 

## ResNet 특징:

### 1. Residual Learning

![](../.gitbook/assets/image%20%2857%29.png)

* H\(x\)를 어떤 함수로 근사화 할 수 있다면, residual function\(H\(x\)-x\) 또한 가능하다.
  * H\(x\):  몇개의 레이어\(stacked layers\)를 의미.
  * x: 는 H\(x\)의 input.
* 너무 당연한 소리인데, 무튼 이렇게 바꾸면, 학습을 좀더 잘된다고 한다.
* Figure 2의 stacked layers는 residual function\(F\(X\) :=  H\(x\) - x\) 을 근사한다. 
* 마지막에 입력값이 더해져서, 결국 block은 H\(x\)를 내뱉는 함수가 된다.

### 2. Identity Mapping by Shortcuts

* Figure 2의 building block 정의:
  * ![](../.gitbook/assets/image%20%2890%29.png)
  * Shortcut Connections: 좌측텀과 우측텀을 연결하는 element-wise addition

### 3. Deeper Bottleneck Architectures

![](../.gitbook/assets/image%20%2873%29.png)

* 좌측과 우측의 time complexity는 유사.
* 우측과 같은 bottleneck 구조가 더 낫다고 함.

  * 3개의 conv 레이어로 이루어지며,  앞의 2개의 레이어에서 채널 수를  줄였다가 마지막 레이어에서 채널 수를 복원하는 구조.







