---
description: >-
  Active learning for accuracy enhancement of semantic segmentation with
  CNN-corrected label curations: Evaluation on kidney segmentation in abdominal
  CT
---

# \(2020\) Segmentation 정확도를 향상시키는 Active Learning 연구

* 논문링크: [https://www.nature.com/articles/s41598-019-57242-9?fbclid=IwAR3Jow0rOGgo451hozjP3kvYUXC1Ty\_mHlDB8b9UqNrvgNGgkfYI8q-Qgr8](https://www.nature.com/articles/s41598-019-57242-9?fbclid=IwAR3Jow0rOGgo451hozjP3kvYUXC1Ty_mHlDB8b9UqNrvgNGgkfYI8q-Qgr8)
* 요약:
  * 초기에서 적은 수의 데이터로 학습한 모델을 가지고 새로운 데이터에 대해 예측한 다음 그 결과를 확인하여, 수정하여 새로운 데이터에 대한 라벨을 만들면, annotation에 대한 노동력을 줄일 수 있다. 
* Active Learning?
  * 많은 데이터셋 필요 =&gt; 많은 annotation 필요.
  * 딥러닝에 효과적인 데이터를 샘플링해서 적은 수의 데이터만 annotation 하여 학습해도, 전체 셋을 학습했을 때와 유사한 성능을 낼 수 있도록하는 방법.
* Cascaded 3D U-Net:
  * 3D U-Net을 2번 학습.
  * 모델1: ROI BBox\(Voxel\)를 예측하도록 학습.
  * 모델2: BBox 로 부터 Segmentation을 예측하도록 학습.

![Figure 2 Workflow of active learning framework.](.gitbook/assets/image%20%2817%29.png)

![Figure 3 Data numbers in each stage of active learning.](.gitbook/assets/image%20%284%29.png)

* 실험 절차 \(Figure 2 & Figure 3 설명\).
  * \(Figure 3의 데이터의 흐름이 잘 매칭이 안된다\).
  * Stage1: Cascaded 3D U-Net을 적은 데이터\(manual annotation\)로 학습.
    * 20개 데이터\(16학습 / 4테스트.\)
  * Stage2: CNN-corrected segmentation
    * 새로운 데이터를 Stage1에서 학습한 모델로 예측.
    * 모델이 예측한 Segmentation을 매뉴얼하게 조정한다\(Mimics Software 라는 프로그램 사용\).
      * \(요게 처음부터 직접 하는 것 보다 노동력을 줄여준다는 것 같음\)
    * 20개 데이터 추가\(16학습 / 4테스트.\)
  * Stage3:
    * Stage1의 데이터와 Stage2에서 추가한 데이터로 학습\(총 40개 데이터\)
    * 테스트 데이터\(10개\)로 평가.
* 실험 세팅.
  * epoch
    * Stage1: 150
    * Stage2 & Stage3: 300
  * Adam optimizer with learning rate of 10−5.
  * weight decay: 0.0005
  * momentum:  0.9
  * loss: average dice coefficient loss
  * batch size: 1

