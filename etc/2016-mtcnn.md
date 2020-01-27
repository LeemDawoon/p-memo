---
description: >-
  Joint Face Detection and Alignment usingMulti-task Cascaded Convolutional
  Networks
---

# \(2016\) MTCNN

* 논문링크: [https://arxiv.org/ftp/arxiv/papers/1604/1604.02878.pdf](https://arxiv.org/ftp/arxiv/papers/1604/1604.02878.pdf)
* 참조 블로그 및 페이지:

## Abstract

* The major contributions of this paper are summarized as follows: 
* \(1\) We propose a new cascaded CNNs based framework for joint face detection and alignment, and carefully design lightweight CNN architecture for real time performance. 
* \(2\) We propose an effective method to conduct online hard sample mining to improve the performance. 
* \(3\) Extensive experiments are conducted on challenging benchmarks, to show the significant performance improvement of the proposed approach compared to the state-of-the-art techniques in both face detection and face alignment tasks.

## APPROACH

### A. Overall Framework 

![](../.gitbook/assets/image%20%287%29.png)

* The overall pipeline of our approach is shown in Fig. 1. 
* Given an image, we initially resize it to different scales to build an image pyramid, which is the input of the following three-stage cascaded framework: 
* Stage 1: 
  * Proposal Network \(P-Net\): 후보 windows와 BBox regression vector를 얻는다.
  * BBox regression vector를 교정\(calibrate\) 한다.
  * non-maximum suppression \(NMS\)를 사용해서 너무 겹치는 후보를 합친다.
* Stage 2: 
  * all candidates are fed to another CNN, called Refine Network \(R-Net\), which further rejects a large number of false candidates, performs calibration with bounding box regression, and NMS candidate merge.
  * Refine Network \(R-Net\)
* Stage 3: 
  * This stage is similar to the second stage, but in this stage we aim to describe the face in more details. 
  * In particular, the network will output five facial landmarks’ positions. 

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

* 1\) Face classification: 얼굴인지 아닌지 구분하는 테스크로 loss는 cross entropy.
* 2\) Bounding box regression: 각각의 window에 대해서, 가까운 ground truth 간의 offset을 예측\(left, top, height, width\). loss는 regression.
* 3\) Facial landmark localization: 이것도 regression.
  * landmark는 left eye, right eye, nose, left mouth corner, and right mouth corner 로 5개이고, 좌표이기 때문에, 차원은 10.
* 4\) Multi-source training:



