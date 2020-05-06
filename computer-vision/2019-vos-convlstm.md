---
description: Semantic Segmentation of Video Sequences with Convolutional LSTMs
---

# \(2019\) VOS with ConvLSTM

* 논문링크: [https://arxiv.org/pdf/1905.01058.pdf](https://arxiv.org/pdf/1905.01058.pdf)
* ICNet & PSPNet 코드: [https://github.com/oandrienko/fast-semantic-segmentation](https://github.com/oandrienko/fast-semantic-segmentation)

## 종합정리

* **LSTM-ICNet**
  * \[\(2019\) VOS with ConvLSTM\]논문은  Convolutional LSTM을 이용한 Video Segmentation 모델인 LSTM-ICNet을 제안한다.
* \*\*\*\*[**Convolutional LSTM**](https://leemdawoon.gitbook.io/p-memo/computer-vision/2015-convolutional-lstm)\*\*\*\*
  * 시공간 정보를 모델링할 때 쓴다.
* \*\*\*\*[**ICNet**](https://leemdawoon.gitbook.io/p-memo/computer-vision/2018-icnet)\*\*\*\*
  * image cascade network \(ICNet\)
  * 실시간 segmentation 을 위해서 고안한 모델.
  * 이 당시 성능이 좋은 모델은 PSPNet 었는데, 속도가 느려 실시간으로 사용하기엔 어려웠다.
  * 이유를 분석해보니, 고해상도 이미지를 FCN를 통해 Segmentation 하는 것은 연산량이 너무 많다.
  * 따라서 ICNet은 입력 이미지를 1/2과 1/4배로 줄여서 저해상도 이미지에는 복갑한 CNN\(PSPNet\)을 사용하여 feature를 뽑고, 중간 해상도 및 고해상도 이미지에서는 가벼운 CNN 구조를 사용하여 feature를 뽑아 이를 fusion하는 전략을 사용한다.
* \*\*\*\*[**PSPNet**](https://leemdawoon.gitbook.io/p-memo/computer-vision/2017-pspnet)\*\*\*\*
  * 딥러닝 네트워크에서, 레이어가 깊을수록 전역적 context 정보를 잘 추출하지 못하는 이슈를 해결하기 위해 제안된 모델.
  * \(context 정보 때문에,  global average pooling을 사용하면,  지역정보를 잃어버리는 현상이 발생...\)
  * CNN에서 추출된 Feature map을 다양한 크기로 pooling 하여 기존 feature map에 추가해 주면, 지역 정보와 전역 정보를 동시에 사용할 수 있다.



------------

I. INTRODUCTION

SegNet \[2\], ENet \[15\], and ICNet \[25\],

convolutional LSTMs \(convLSTMs \[19\]\)



II. RELATED WORK

VOS에서 RNN의 두가지 적용분야.

* 1\)  On one side, they are used to capture temporal information of a video sequence.
* 2\) On the other side, RNNs are used to learn the global context of one image.
  * 각 이미지의 segment 정보가 다른 LSTM에 물려서 들어가는데, 이미지 region 간의 spatial relationship을 학습할 수 있다.

Recurrent Fully Convolutional Network \(RFCN, \[22\]\), which was introduced by Valipour et al. in 2017. They extended the FCN approach \[18\] by placing a recurrent unit between the encoder and the decoder, and exhibit better performance on the SegTrack, Davis, and Moving MNIST dataset.



III. RECURRENT NETWORK ARCHITECTURES FOR VIDEO SEGMENTATION

A. LSTM-SegNet\(패스...\)

B. LSTM-ICNet

![LSTM-ICNet](../.gitbook/assets/image%20%2863%29.png)

IV. EVALUATION

* LSTM-ICNet version 6가 좋은편.

![](../.gitbook/assets/image%20%28131%29.png)

![](../.gitbook/assets/image%20%28145%29.png)









