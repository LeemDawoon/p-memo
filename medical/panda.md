---
description: Prostate cANcer graDe Assessment (PANDA) Challenge
---

# PANDA

* 전립선 암 grade 평가.
* 링크: [https://www.kaggle.com/c/prostate-cancer-grade-assessment](https://www.kaggle.com/c/prostate-cancer-grade-assessment)

## 약자 및 용어:

* prostate cancer \(PCa\)
* Gleason pattern & Gleason grading system
  * [https://en.wikipedia.org/wiki/Gleason\_grading\_system](https://en.wikipedia.org/wiki/Gleason_grading_system)

## Description:

* PCa 진단은 prostate tissue biopsies에 기초한다.
* 조직 샘플은 병리사에 의해, Gleason grading system에 따라 평가된다.
* 본 과제는, 전립선 조직 샘플 이미지에서, PCa를 탐지하기 위한 모델을 개발하고,  질병의 중등도를 추정하는 것이다 .
  * Gleason grading이 안되어있는 데이터에 대해서??
  * In this challenge, you will develop models for detecting PCa on images of prostate tissue samples, and estimate severity of the disease using the most extensive multi-center dataset on Gleason grading yet available.
* The grading process 는 finding과 classifying으로 구성된다. 
* classify:  
  * cancer tissue를 Gleason patterns \(3, 4, or 5\) 로 분류.
  *  Gleason pattern은 종양의 architectural growth 패턴에 기초한다 \(Fig. 1\).
* biopsy에 Gleason score 가 할당되면, 1에서 5사이의 ISUP grade로 변환한다.
* Gleason grading system는 가장 중요한, prognostic marker.
* ISUP grade: 환자를 치료하는 방법을 결정할 때, 중요한 역할.
  * [https://www.kjuo.or.kr/journal/Figure.php?xn=kjuo-15-3-93.xml&id=](https://www.kjuo.or.kr/journal/Figure.php?xn=kjuo-15-3-93.xml&id=)
  * 같은 GS 7 전립선 암이라 하더라도 GS 4+3 에 비해 GS 3+4 전립선암 환자의 예후가 더 좋다는 결과들이 많이 보고.
  * 전립선 생검에서 Gleason grade가 절대적으로 중요한데, Epstein 등이 새로운 Gleason grading system을 제시하였는데, 전체 5 그룹이고, GS 6가 가장 낮은 score로 grade group 1, GS 7 \(3+4\)은 grade group 2, GS 7 \(4+3\)은 grade group 3, GS 8은 grade group 4, GS 9–10을 grade group 5로 정의하였다.
  * ![](../.gitbook/assets/image%20%28116%29.png)

  * 
* 이러한 평가에는, 암을 진단하지 못하거나\(빠뜨리거나\), overgrade 하여 불필요한 치료를 할 위험이있다.
* 병리사간 평가의 차이가 많이 난다.
* 최근 연구에서는, 병리사가 하는 평가를 딥러닝으로 수행하여, 좋은 성과를 보였는데, 대규모의 다중 센터 데이터에 해서는 지 않았다.
* The training set:
  * 약 11,000 whole-slide images of digitized H&E-stained biopsies originating from two centers. 
  * [CAMELYON17](https://camelyon17.grand-challenge.org/) 보다 약 8배 많음.
* Furthermore, in contrast to previous challenges, we are making full diagnostic biopsy images available. 
  * 이전 챌린지와 달리, full diagnostic biopsy images 를 제공한다.





![](../.gitbook/assets/image%20%2889%29.png)

 **Figure 1**: An illustration of the Gleason grading process for an example biopsy containing prostate cancer. 

* The most common \(blue outline, Gleason pattern 3\) and second most common \(red outline, Gleason pattern 4\) cancer growth patterns present in the biopsy dictate the Gleason score \(3+4 for this biopsy\), which in turn is converted into an ISUP grade \(2 for this biopsy\) following guidelines of the International Society of Urological Pathology. 
* 본 과제에서는, 암이 없는 Biopsies  ISUP grade = 0 으로 나타냄.





## Data:

**\[train/test\].csv**

* `image_id`: ID code for the image.
* `data_provider`: 
  * The name of the institution that provided the data. 
  * Both the [Karolinska Institute](https://ki.se/en/meb) and [Radboud University Medical Center](https://www.radboudumc.nl/en/research) contributed data. 
  * They used different scanners with slightly different maximum microscope resolutions and worked with different pathologists for labeling their images.
* `isup_grade`: Train only. **The target variable**. The severity of the cancer on a 0-5 scale.
* `gleason_score`: Train only. 
  * An alternate cancer severity rating system with more levels than the ISUP scale. 
  * For details on how the gleason and ISUP systems compare, see the [Additional Resources tab](https://www.kaggle.com/c/prostate-cancer-grade-assessment/overview/additional-resources).

**\[train/test\]\_images**: 

* Each is a large multi-level tiff file. 
* You can expect roughly 1,000 images in the hidden test set. 
* 테스트 세트에 사용 된 이미지에 대해 트레이닝 세트와 약간 다른 절차가 수행되었음..
* training set image의 일부에는 펜 표시가 있음.

**train\_label\_masks**: 

* Segmentation masks showing which parts of the image led to the ISUP grade. 
* 모든데이터에 마스크가 있는 건 아님.
* mask에는 다양한 이유로 FP, FN이 존재할  수있음.
* 이 마스크는 wsi에서 유용한 subsample을 선택하도록 제공.
* data provider에따라 mask 값은 다름.:
* Radboud: Prostate glands are individually labelled. Valid values are:
  * 0: background \(non tissue\) or unknown
  * 1: stroma \(connective tissue, non-epithelium tissue\)
  * 2: healthy \(benign\) epithelium
  * 3: cancerous epithelium \(Gleason 3\)
  * 4: cancerous epithelium \(Gleason 4\)
  * 5: cancerous epithelium \(Gleason 5\)
* Karolinska: Regions are labelled. Valid values are:
  * 0: background \(non tissue\) or unknown
  * 1: benign tissue \(stroma and epithelium combined\)
  * 2: cancerous tissue \(stroma and epithelium combined\) 

**sample\_submission.csv**: 

* A valid submission file. 
* This is a notebooks-only competition; the downloadable test.csv and sample\_submission.csv have been truncated. 
* The full versions will be available to your submitted notebooks.

## Evaluation:

* quadratic weighted kappa

## Timeline:

* **July 15, 2020** - Entry deadline. You must accept the competition rules before this date in order to compete.
* **July 15, 2020** - Team Merger deadline. This is the last day participants may join or merge teams.
* **July 22, 2020** - Final submission deadline.

All deadlines are at 11:59 PM UTC on the corresponding day unless otherwise noted. The competition organizers reserve the right to update the contest timeline if they deem it necessary.

## Rule:

* **This is a Code competition**
* \*\*\*\*
* External data, freely & publicly available, is allowed. This includes pre-trained models.
* No custom packages enabled in kernels
* Submission file must be named `submission.csv`









\*\*\*\*

