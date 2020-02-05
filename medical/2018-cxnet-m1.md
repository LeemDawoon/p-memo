---
description: 'CXNet-m1: Anomaly Detection on Chest X-Rays with Image-Based Deep Learning'
---

# \(2018\) CXNet-m1

* 논문링크: [https://ieeexplore.ieee.org/document/8575127](https://ieeexplore.ieee.org/document/8575127)

## Abstract:

* 기존 딥러닝 네트워크를 fine-tuning에서 사용하는 것에는 한계가 존재한다.
* 이러한 한계를 극복하기 위해 ChestX-ray14에 대한 계층적 CNN 구조와 훨씬 작지만 더 나은 CXNet-m1 이라는 네트워크를 제안한다.
* 분류를 잘 못한 이미지로 부터 더 학습할 수 있는 sin-loss 라는 함수도 제안한다.
* 실험 결과에서 Find-tuning 모델보다 더 나은 성능을 보여줬다.

## Method: 

### A. HIERARCHICAL STRUCTURE:

![](../.gitbook/assets/image%20%2821%29.png)

* CXNet -m1: Normal/Abnormal을 분류.
* CXNet -m2: Abnormal 데이터가 Multi-label인지 Single Label 인지 분류.
* CXNet -m3: Multi-label 분류.
* CXNet -m4: Single-label 분류.

### B. CXNET-M1:

* 요 논문은 CXNet -m1 모델에 포커싱한다.
* 1\) Problem Formulation 
  * sin-loss에 대한 설명.
  * \(4\)식이 일반적인 cross entropy에 대한 식이고, \(10\)식이 sin-loss에 대한 식인데, 차이점은 곱해지는 베타라고 볼수 있다.
  * 식\(9\)는 베타에 대한 식.

![](../.gitbook/assets/image%20%2853%29.png)

![](../.gitbook/assets/image%20%2856%29.png)

* 2\) Model Architecture:
  * CXNet-m1 \(Chest X-ray Network-model 1\)

![](../.gitbook/assets/image%20%2813%29.png)

## Experiments

### A. DATASET:

* It contains more than 30,000 patients, 297,541 labeled chest x-ray images and 14 kinds abnormal images including Infiltration, Effusion, Atelectasis, Nodule, Mass, Pneumothorax, Consolidation, Pleural Thickening, Cardiomegaly, Emphysema, Edema, Fibrosis, Pneumonia and Hernia.

![](../.gitbook/assets/image%20%2830%29.png)

![](../.gitbook/assets/image%20%2844%29.png)

B. METRICS

C. TRAINING

* We trained our network four times on 80458 of 112120 training images \(setting aside 6056 for validation and 25596 for test\) and 84090 of 112120 training images \(setting aside 11212 for validation and 16818 for test\) on Chest x-ray14 dataset in the first time and the last three times, respectively.
* The size of input images were set as 224_224 and 512_512 in the first three times and the last time, respectively. 
* optimizer: SGD
* loss: h standard cross entropy loss and our sin-loss
* e batch size = 32
* learning rate of 0.01, decayed by a factor of 2 or 5 or 10 manually through monitoring the loss curve in TensorBoard.
* The networks were validated against the validation set after every 3000 iterations

![](../.gitbook/assets/image%20%2812%29.png)

![](../.gitbook/assets/image%20%281%29.png)

