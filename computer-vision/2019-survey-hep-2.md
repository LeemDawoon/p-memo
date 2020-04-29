---
description: 'Deep Learning based HEp-2 Image Classification: A Comprehensive Review'
---

# \(2019\) Survey: HEp-2

* 논문링크: [https://arxiv.org/pdf/1911.08916.pdf](https://arxiv.org/pdf/1911.08916.pdf)



## Abstract

* HEp-2 cell patteren을 분류 하는 것은 immunofluorescence 검사에서 autoimmune diseases\(자가면역질환\)을 식별하는데 간접적으로

  중요한 역할을 한다.

* 본 논문에서는 이 Task에 대한 근래의 딥러닝 기법들에 대한 리뷰를 수행한다.
* 근래의 방법 들은 cell-level 과 specimen-level 두가지의 레벨로 분류를 수행한다.



## 1 Introduction

* Indirect immunofluorescence \(IIF\)은 autoimmune diseases 진단에 표준 검사방법.
* autoimmune diseases 에는 rheumatoid arthritis\(류마티스 관절염\), pulmonary fibrosis\(폐 섬유,\) Sjogren’s syndrom, and Addison disease. 등이 있다.
* IIF는 blood serum\(혈정\)에 적용되고, auto-antibodies\(자가 항체\)는 humane elliptical 2 \(HEp-2\) cells에 있는 형관퍼턴으로 부터 발견된다.
* HEp-2 cells 은 거의 100 가지 유형의 auto-antibodies\(자가 항체\)에 존재하는 30가지 이상의  cytoplasmic\(세포질\)과 nuclear\(핵\) 패턴을 나타낸다. 
* 하지만 다음과 같은 아주 적은 수의 패턴만 진단 목적으로 쓸만한다:
  * homogeneous\(균질\), nucleolar\(핵\), centromereseen, cytoplasmatic, fine speckled\(미세 얼룩\), coarse speckled\(거친얼룩\), cytoplasmatic, nucleolar membrane\(핵막\), and golgi.
*  Anti Nuclear Antibodies \(_ANA_\)

## 3 Deep Learning for HEp-2 Image Classification

### 3.1 Cell-level HEp-2 image classification \(CL-HEP2IC\) methods

\(생략\)

### 3.2 Specimen-level HEp-2 image classification \(SL-HEP2IC\) methods

![](../.gitbook/assets/image%20%28113%29.png)



#### 3.2.1 Single cell processing based specimen-level HEp-2 image classification methods \(SCP-SL-HEP2IC\)

* bounding box나 segmentation mask로 cell 을 검출 한 뒤, cell image를 분류한다.
* \[46\] Deep CNN for IIF Images Classification in Autoimmune Diagnostics

#### 3.2.2 Multi-cell processing based specimen-level HEp-2 image classification methods \(MCP-SL-HEP2IC\)

**Pixel-wise prediction based methods.**

* 픽셀 단위로 classification. \(그냥 여러 클래스를 segmentation 한다고 보면된다.\)
* **\[63\] Deeply supervised full convolution network for HEp-2 specimen image segmentation**

  \*\*\*\*

![Results of \[63\]](../.gitbook/assets/image%20%2887%29.png)

* segmentation accuracy \(SEG\), sensitivity \(SE\), Jaccard index \(JA\) and accuracy \(AC\)
* Crop + Mirror + Rotate \(CMR\)
* hierarchical supervision structure \(HS\)



**Image-wise prediction based methods.**





## 4 HEp-2 Public Datasets

![](../.gitbook/assets/image%20%2884%29.png)

4.1 ICPR 2012 dataset



4.2 SNHEp-2 dataset

* There are 1,884 cell images extracted from 40 specimen images. 
* The specimen images are divided into training and testing sets with 20 images each \(4 images for each pattern\). In total there are 905 and 979 cell images extracted for training and testing. 
* Five-fold validation of training and testing were created by randomly selecting the training and test images. Both training and testing in each fold contain around 900 cell images \(approx. 450 images each\).

4.3 ICPR 2014 dataset



4.4 AIDA dataset



