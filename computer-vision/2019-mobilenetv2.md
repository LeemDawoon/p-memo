---
description: 'MobileNetV2: Inverted Residuals and Linear Bottlenecks'
---

# \(2019\) MobileNetV2

* 논문링크: [https://arxiv.org/pdf/1801.04381.pdf](https://arxiv.org/pdf/1801.04381.pdf)
* 참조 블로그 및 페이지:
  * [https://n1094.tistory.com/29](https://n1094.tistory.com/29)
  * [https://seongkyun.github.io/papers/2019/01/09/mobilenetv2/](https://seongkyun.github.io/papers/2019/01/09/mobilenetv2/)
  * [http://www.navisphere.net/6145/mobilenetv2-inverted-residuals-and-linear-bottlenecks/](http://www.navisphere.net/6145/mobilenetv2-inverted-residuals-and-linear-bottlenecks/)

## MobileNetV2 특징:

### 1. Depthwise Separable Convolution

* MobileNet V1과 동일

### 2. Linear Bottlenecks

* 딥러닝에서 각 레이어는 $$h * w * d$$ 형태의 activation tensor를 내뱉는다.
* h x w는 pixel로 d는 dimension으로 매칭하여 이해할수있다.
* 하지만 CNN은 보통 ReLU 같은 non-linear per coordinate transformations 을 가지기 때문에 위와 같은 매칭은 온전하지 않다\(정보의 손실\).
* 하지만 입력의 채널의 수가 충분히 큰 경우에는 이를 극복할 수 있다.
  * 주요 2가지 성질:
    * 1. ReLU 연산후,  manifold of interest가 0이 아닌 volume을 유지하면, 이는 선형 변환에 해당한다.
      2. 입력 매니 폴드가 입력 공간의 저 차원 하위 공간에있는 경우, ReLU는 입력 매니 폴드에 대한 완전한 정보를 보존 할 수 있다.

![](../.gitbook/assets/image%20%28122%29.png)

* 2d 에서 보듯이, 첫번째 레이어가 채널을 확장하는 역할을 하고 뒤에 두 레이어는 Depthwise Separable Convolution 이다. 

### 3. Inverted residuals

* ResNet 처럼 shortcut connection을 사용하는데,  3b 처럼 채널 수가 적은 텐서를 연결하는 것이 더 메모리 효율적이다.

![](../.gitbook/assets/image%20%28120%29.png)

### 4. 전체구조

![](../.gitbook/assets/image%20%2878%29.png)

![](../.gitbook/assets/image%20%2834%29.png)







