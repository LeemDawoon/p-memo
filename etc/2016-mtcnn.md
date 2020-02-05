---
description: >-
  Joint Face Detection and Alignment usingMulti-task Cascaded Convolutional
  Networks
---

# \(2016\) MTCNN

* 논문링크: [htt ps://arxiv.org/ftp/arxiv/papers/1604/1604.02878.pdf](https://arxiv.org/ftp/arxiv/papers/1604/1604.02878.pdf)
* 참조 블로그 및 페이지:
  * [https://yeomko.tistory.com/16](https://yeomko.tistory.com/16)
  * [https://towardsdatascience.com/how-does-a-face-detection-program-work-using-neural-networks-17896df8e6ff](https://towardsdatascience.com/how-does-a-face-detection-program-work-using-neural-networks-17896df8e6ff)

## Abstract

* The major contributions of this paper are summarized as follows: 
* \(1\) We propose a new cascaded CNNs based framework for joint face detection and alignment, and carefully design lightweight CNN architecture for real time performance. 
* \(2\) We propose an effective method to conduct online hard sample mining to improve the performance. 
* \(3\) Extensive experiments are conducted on challenging benchmarks, to show the significant performance improvement of the proposed approach compared to the state-of-the-art techniques in both face detection and face alignment tasks.

## APPROACH

### A. Overall Framework 

![](../.gitbook/assets/image%20%289%29.png)

* The overall pipeline of our approach is shown in Fig. 1. 
* Given an image, we initially resize it to different scales to build an image pyramid, which is the input of the following three-stage cascaded framework: 
* 이미지가 주어지면, 몇개의 다른 비율로 resize 하여 이미지 피라미드를 만든다. 그리고 이걸 아래에 3개 stage 에 밉력으로 사용한다.
* **Stage 1: Proposal Network \(P-Net\)**
  * **P-Net:** 후보 windows와 BBox regression vector를 얻는다.
  * BBox regression vector를 교정\(calibrate\) 한다.
  * non-maximum suppression \(NMS\)를 사용해서 너무 겹치는 후보를 합친다.
    * 참조\) NMS는 [\(2016\) SSD](https://leemdawoon.gitbook.io/p-memo/computer-vision/2016-ssd) 페이지에 정리된 내용 
* **Stage 2: Refine Network \(R-Net\)**
  * all candidates are fed to another CNN, called Refine Network \(R-Net\), which further rejects a large number of false candidates, performs calibration with bounding box regression, and NMS candidate merge.
  * R-Net: P-Net에서 획득한 BBox 중에서 진짜 얼굴에 해당하는 영역을 추려내고, BBox Regression을 더 정교히 수행한.
  * P-Net과 다르게 끝단에 FC 레이어가 있.
  * 찾아낸 박스는 원래 입력 이미지 크기로 되돌린 다음, NMX와 BBox Regression 을 수행한다.
  * 여기서 살아남은 BBox 들만 O-Net\(stage3\)에 전달한다.
* **Stage 3: Output Network \(O-Net\)**
  * This stage is similar to the second stage, but in this stage we aim to describe the face in more details. 
  * In particular, the network will output five facial landmarks’ positions. 
  * 최종 Face Detection, Face Alignment 결과 값을 도출한다.
  * 역시 NMS를 수행한다.

### B. CNN Architectures 

* In \[19\], multiple CNNs have been designed for face detection. 
* However, we noticed its performance might be limited by the following facts: 
  * \(1\) Some filters lack diversity of weights that may limit them to produce discriminative description. 
  * \(2\) Compared to other multi-class objection detection and classification tasks, face detection is a challenge binary classification task, so it may need less numbers of filters but more discrimination of them. 
* To this end, we reduce the number of filters and change the 5×5 filter to a 3×3 filter to reduce the computing while increase the depth to get better performance. 
* With these improvements, compared to the previous architecture in \[19\], we can get better performance with less runtime . 
* Our CNN architectures are showed in Fig. 2.

![](../.gitbook/assets/image%20%282%29.png)

### C. Training

* 1\) Face classification: 
  * 얼굴인지 아닌지 구분하는 테스크로 loss는 cross entropy.
  * ![](../.gitbook/assets/image%20%2845%29.png)
* 2\) Bounding box regression: 
  * 각각의 window에 대해서, 가까운 ground truth 간의 offset을 예측\(left, top, height, width\). loss는 regression.
  * ![](../.gitbook/assets/image%20%285%29.png)
* 3\) Facial landmark localization: 
  * 이것도 regression.
  * landmark는 left eye, right eye, nose, left mouth corner, and right mouth corner 로 5개이고, 좌표이기 때문에, 차원은 10.
  * ![](../.gitbook/assets/image%20%288%29.png)
* 4\) Multi-source training:
  * Eq. \(1\)-\(3\)를 모두 쓰지는 않는데, 예를 들어, 배경영역의 경우 식\(1\)만 계산하고 나머지 Loss는 0으로 설정한다. 이는 type indicator\(_β\) 로 구현된다._
  * task별 가중치\(α\)를 줘서 식\(4\)처럼 한다.
  * ![](../.gitbook/assets/image%20%2828%29.png)
    * α: Task의 중요도를 나타내는 가중치.
      * P-Net와 R-Net의 경우:  α-det = 1,  α-box=0.5, α-landmark=0.5
      * O-Net의 경우:  α-det = 1,  α-box=0.1, α-landmark=0.5
    *  β: type indicator. 0 또는 1의 값을 가짐.
* 5\) Online Hard sample mining:

  * 현재 알고리즘이 구별하기 어려워하는 대상을 학습에 더 많이 참여 시키는 방법.
  * 각 미니 배치에서 모든 샘플에서 계산된 손실을 정렬하고 상위 70 %를 하드 샘플로 선택한다.



