---
description: ICNet for Real-Time Semantic Segmentationon High-Resolution Images
---

# \(2018\) ICNet

* 논문링크: [https://arxiv.org/pdf/1704.08545.pdf](https://arxiv.org/pdf/1704.08545.pdf)
  * [https://eccv2018.org/openaccess/content\_ECCV\_2018/papers/Hengshuang\_Zhao\_ICNet\_for\_Real-Time\_ECCV\_2018\_paper.pdf](https://eccv2018.org/openaccess/content_ECCV_2018/papers/Hengshuang_Zhao_ICNet_for_Real-Time_ECCV_2018_paper.pdf)



image cascade network \(ICNet\)

## 1 Introduction

* We develop a novel and unique image cascade network for real-time semantic segmentation, it utilizes semantic information in low resolution along with details from high-resolution images efficiently.
* The developed cascade feature fusion unit together with cascade label guidance can recover and refine segmentation prediction progressively with a low computation cost.
* Our ICNet achieves 5× speedup of inference time, and reduces memory consumption by 5× times. It can run at high resolution 1024×2048 in speed of 30 fps while accomplishing high-quality results. It yields real-time inference on various datasets including Cityscapes \[7\], CamVid \[17\] and COCO-Stuff \[18\].

2 Related Work

High Quality Semantic Segmentation

High Efficiency Semantic Segmentation

Video Semantic Segmentation



![](../.gitbook/assets/image%20%2867%29.png)

## 3 Image Cascade Network

3.1 Speed Analysis

### 3.2 Network Architecture

![](../.gitbook/assets/image%20%28124%29.png)

* 3.1에서 연산량을 분석해본 결과, 속도를 높이기 위해 이러한 전략들을 사용했다.
  * downsampling input, 
  * shrinking feature maps 
  * conducting model compression.
* ICNet은입력 이미지를 low-, medium- and high resolution 이미지로 캐스캐이딩하고, cascade feature fusion\(CFF\) 유닛을사용한다\(Sec. 3.3\). 그리고 그것을  cascade label guidance \(Sec. 3.4\)에 따라 학습한다.
* 고해상도의 이미지를 FCN을 통해 segmentation 하는 것은 계산량이 많다. 이를 극복하기 위해 Fig2의 상단 가지처럼, 저해상도 이미지를 사용해서  semantic extraction을 수행한다.
* 저행상도 이미지를 1/8 배 비율로  downsampling 후 PSPNet의 입력으로넣어 1/32 크기의 feature map을 얻는다.
* 좋은 퀄리티의 segmentation을 얻기 위해, medium 과 high resolution branch 는 coarse prediction을 정제하는 역할을 한다.
* 이런 식으로 네트워크를 구성하면, Fig2에서 녹색 점선 박스 처럼, 중간해상도 고해상도 관련 브랜치의 파라미터를 줄일 수 있다.
* 각 브랜치에서 나온 output feature map은 cascade-feature-fusion\(CFF\) 유닛으로 융합된다.

### 3.3 Cascade Feature Fusion

![](../.gitbook/assets/image%20%2815%29.png)

* CFF의 입력은 3가지 컴포넌트로 이루어져 있다:
  * feature map F1 \(C1 X H1 X W1\)
  * feature map F2 \(C2 X H2 X W2\)
  * Ground-Truth Label \(1 X H2 X W2\)
* F2는 F1의 크기의 2배다.
* 1\) F1은 2배로 업샘플링\(bilinear interpolation이용\)하여 F2와 동일한 크기로 변환한다.
* 2\) upsampling feature를 정제하기 위해, dilated convolution 거친다.
  * 커널 크기는 C3 X 3 X 3
  * dilation = 2
  * =&gt; 결과 feature 크기는 C3 X H2 X W2
  * dilated convolution은 이웃 픽셀\(근접픽셀\)로 부터 정보를 결합한다.
  * dilated convolution에 의한 upsampling은 deconvolution과 비교하여, 같은 receptive field 얻기위해 더  작은 크기의  커널을 사용한다. 이 같은 차이는 계산량을 줄여 준다.
* 3\) F2 feature map에는 projection convolution이 수행된다.
  * 커널 사이즈 = C3 X 1 X 1
  * F1의  output과 채널 수가  C3으로 같다.
* 4\) 그림에서 처럼 두 브랜치의 처리된 feature map을 정규화하기 위해 2개의 batch normalization 레이어가 사용된다.
* 5\) F1 브랜치와 F2브랜치에서 나온 feature를 융합한다.
  * element-wise sum 레이어
  * ReLU
  * =&gt; 결과 F2' \(C3 X H2 X W2\)
* F1 브랜치의 학습을 향상시키기 위해,  보조 라벨 가이던스\(auxiliary label guidance\)를 사용한다.

### 3.4 Cascade Label Guidance

* 각 브랜치의 학습 절차를 향상시키기 위해 , cascade label guidance 전략을 채택했다.
* 이것은 다른 스캐일 ground-truth 라벨을 사용한다.
  * \(e.g., 1/16, 1/8, and 1/4\) 
* 최소화할 Loss:
  * The corresponding ground truth label for 2D position \(y, x\) is **n^**

![Loss](../.gitbook/assets/image%20%2864%29.png)

![branch t&#xC5D0;&#xC11C;&#xC758; predicted feature map](../.gitbook/assets/image%20%28132%29.png)

![Branch t&#xC758; Loss&#xC758; &#xAC00;&#xC911;&#xCE58;](../.gitbook/assets/image%20%2825%29.png)

## 4 Structure Comparison and Analysis

![](../.gitbook/assets/image%20%286%29.png)

* 


