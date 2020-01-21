---
description: automatic lung cancer patient management
---

# LND

링크: [https://lndb.grand-challenge.org/Home/](https://lndb.grand-challenge.org/Home/)

## Motivation

* lung cancer는 조기 발견이 어려워서 다른 암들에 비해 생존률이 더디게 나아졌다.
* Low-dose computed tomography \(CT\)는 조기 발견을 위한 스크리닝 툴로서 제안되었는데, lung cancer 위험군의 사망률을 20% 줄여준 것이 입증되었다.
* 그럼에도 불구하고 Low-dose CT를 환자에게 설명하는 것은\(진단하는 것은\) 쉽지않고, 인력비용 등으로 어려움을 겪고 있다.
* 특히  lung nodules 같은 경우, 그 특성이 너무 다양해서 판독자에 따라 그 차이가 크다.
* Computer-aided diagnosis \(CAD\)시스템은 임상의의 부담을 줄이고 독립적 인 2 차 의견을 제공함으로써 도움을 줄 수 있다.



## Aims

* The main goal of this challenge is the **automatic classification of chest CT scans according to the** [**2017 Fleischner society pulmonary nodule guidelines**](https://pubs.rsna.org/doi/full/10.1148/radiol.2017161659) for patient follow-up recommendation. 
* The Fleischner guidelines are widely used for patient management in the case of nodule findings, and are composed of 4 classes, taking into account the number of nodules \(single or multiple\), their volume \(&lt;100mm³, 100-250mm³ and ⩾250mm³\) and texture \(solid, part solid and ground glass opacities \(GGO\)\). Furthermore, **three additional sub-challenges** will be held related to the different tasks needed to calculate a Fleischner score. 
  * 수.
  * 크기.
  * 텍스쳐.
* The challenge is thus made up of four different parts:
  * **Main Challenge - Fleischner Classification:** From chest CT scans, participants must predict the correct follow-up according to the 2017 Fleischner guidelines;
  * **Sub-Challenge A - Nodule Detection:** From chest CT scans, participants must detect pulmonary nodules;
  * **Sub-Challenge B - Nodule Segmentation:** Given a list of &gt;3mm nodule centroids, participants must segment the nodules in the corresponding chest CT scans;
  * **Sub-Challenge C - Nodule Texture Characterization:** Given a list of nodule centroids, participants must classify nodules into three texture classes - **solid**, **sub-solid** and **GGO**.



