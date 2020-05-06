---
description: Deep High-Resolution Representation Learning for Visual Recognition
---

# \(2020\) HRNet

* ​논문링크: [https://arxiv.org/pdf/1908.07919.pdf](https://arxiv.org/pdf/1908.07919.pdf)
* 코드링크: [https://github.com/HRNet/](https://github.com/HRNet/HRNet-Semantic-Segmentation)

요약.

* CCN 에서 feature map의 크기가 작아지면서,  low-resolution representation의 문제를 제기.
* 따라서 본 논문에서 High-Resolution Network \(HRNet\)을 제안. 2가지 특징이 있다.
  * \(i\) Connect the high-to-low resolution convolution streams in parallel
  * \(ii\) Repeatedly exchange the information across resolutions. 

## 1 INTRODUCTION

* high -resolution representation은 다음과 같은 위치에 민감한 테스크에 필요하다.
  * semantic segmentation,
  * human pose estimation
  * object detection
* 이전의 state-of-the-art 방법들도 high-resolution로 복원하는 방법을 채택한다.
* **HRNetV1**: outputs the high-resolution representation computed from the high-resolution convolution stream. \(Deep high-resolution representation learning for human pose estimation\)
* **HRNetV2**: combines the representations from all the high-to-low resolution parallel streams
* **HRNetV2p**:  multi-level representation from the high-resolution representation output from HRNetV2,

## 2 RELATED WORK

* Learning low-resolution representations: 
  * 
* Recovering high-resolution representations:
  * 
* **Maintaining high-resolution representations:**
* **Multi-scale fusion:**

## 3 HIGH-RESOLUTION NETWORKS

![](../.gitbook/assets/image%20%2864%29.png)

* 처음 입력이미지를 stride2 3×3 convolutions을 거쳐서 입력이미지 해상도의 1/4로 줄이는데, 이 resolution을 계속 유지한다.
* 세부적으로는 다음과 같은 컴포넌트들로 구성된다.
  * parallel multi-resolution convolutions
  * repeated multi-resolution fusions
  * representation head

![](../.gitbook/assets/image%20%2856%29.png)

### 3.1 Parallel Multi-Resolution Convolutions

![](../.gitbook/assets/image%20%28118%29.png)

* $$ N_{sr}$$은 s-th 번째 stage에 r resolution index를 가지는 sub-stream.
* r에 해당하는 resolution은 $$ 1/2^{r-1}$$

### 3.2 Repeated Multi-Resolution Fusions

* The goal of the fusion module is to exchange the information across multi-resolution representations
* It is repeated several times \(e.g., every 4 residual units\)

![](../.gitbook/assets/image%20%2832%29.png)

### 3.3 Representation Head

* **HRNetV1.** The output is the representation only from the high-resolution stream. Other three representations are ignored. 
* **HRNetV2.** We rescale the low-resolution representations through bilinear upsampling without changing the number of channels to the high resolution, and concatenate the four representations, followed by a 1 × 1 convolution to mix the four representations.
* **HRNetV2p.** We construct multi-level representations by downsampling the high-resolution representation output from HRNetV2 to multiple levels.

![](../.gitbook/assets/image%20%28128%29.png)

* In this paper, we will show the results of applying HRNetV1 to human pose estimation, HRNetV2 to semantic segmentation, and HRNetV2p to object detection.





* HRNet-W32에서 32는 채널 수를 의미하는데,  HRNet-W32의 경우에는, resolution 별로\(그림에서 블럭 색깔별로\) 32, 64, 128, 256 채널을 유지한다. 





