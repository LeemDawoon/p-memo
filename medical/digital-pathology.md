# Digital Pathology

* **\(2020\) Segmentation and Classification in Digital Pathology for Glioma Research: Challenges and Deep Learning Approaches**
  * 논문링크: [https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7046596/pdf/fnins-14-00027.pdf](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7046596/pdf/fnins-14-00027.pdf)
  * MICCAI 2018 CPM challenge 에서 2등.
    * WSI와 뇌 MRI가 같이 주어지며, 악성 세포를 분류하는 문제 같음.
    * WSI 쪽만 봐보자.
  * **Instance Segmentation of Nuclei in Brain Tissue Images Method:**
    * Mask-RCNN 적용.
    * MASK non-maximum suppression \(MASK-NMS\) 적용.
      * MASK-NMS는 최대 중첩을 가진 마스크의 결합을 취하고 작은 중첩으로 FP 마스크를 제거합니다.
    * 백본: ResNet-101
    * input tissue images were cropped to 600 × 600
    * 세그멘테이션 결과 세트를 I이라고합니다. 
    * 세트 I의 각 결과에는 점수 S가 할당됩니다.
    * S는 Mask-RCNN 모듈의 분류 확률 값이며 분할 결과의 confidence level 에 해당합니다.
    * 최대 점수 M \(점수 S 중 최대 점수\)으로 세분화를 선택한 후 MASK-NMS는이를 세트 I에서 제거하고이를 final segmentation 세트 D에 추가합니다.
    * D는 빈 세트로 초기화됩니다. 
    * 또한 세트 I에서 임계 값 N보다 큰 오버랩이있는 세그먼트를 제거합니다. 여기서 교차 합집합 \(IOU\)이 오버랩 메트릭으로 사용됩니다.
    * I 가 빌때 까지 반복.

![](../.gitbook/assets/image%20%28161%29.png)

*  * **Analysis pipeline for whole slide tissue images:**
    * d

![](../.gitbook/assets/image%20%2871%29.png)

*  * **Histopathology classification model:**
    * multiple instance learning approach
    * pre-processing:
      * tissue detection: Otsu thresholding.
      * color normalization: histogram equalization
      * tiling: 랜덤 샘플링해서 20개의 448 × 448-pixel patches를 추출.

![](../.gitbook/assets/image%20%28165%29.png)



*  * *  * d
* **\(2018\) A Robust and Effective Approach Towards Accurate Metastasis Detection and pN-stage Classification in Breast Cancer** 
  * 원본 링크: 
    * [https://blog.lunit.io/2018/09/01/a-robust-and-effective-approach-towards-accurate-metastasis-detection-and-pn-stage-classification-in-breast-cancer/](https://blog.lunit.io/2018/09/01/a-robust-and-effective-approach-towards-accurate-metastasis-detection-and-pn-stage-classification-in-breast-cancer/)
  *  [CAMELYON17](https://camelyon17.grand-challenge.org/)에서 1위
  * **TASK:** 
    * 유방암 환자의 예후를 예측하는 인자들 중 가장 중요하게 다루어지는 병기\(stage\) 분류.
    * CAMELYON17 데이터에서는 한 환자당 5장의 림프절 조직이 주어졌다고 가정하고 있으며, 5장의 조직을 분석 후 종합하여 해당 환자에 대한 pN-stage를 예측.
  * **Method:** 
    1. **관심영역\(조직 영역\) 추출** 슬라이드에서 조직에 해당하는 부분만을 추출하기 위해, whole slide image를 grayscale로 변환한 후, 특정 값을 기준으로 thresholding하여 조직 영역을 검출합니다.
    2. **전이영역 검출** 일정 크기의 patch에 대해 normal/tumor를 분류할 수 있는 Convolutional neural network \(CNN\)를 활용해 조직 영역 중 전이가 된 부분들을 검출합니다.
    3. **림프절 분류** 전이된 부분을 검출한 림프절 슬라이드에 대해, 여러 특징들을 추출해 낸 뒤 [Random forest classifier](https://en.wikipedia.org/wiki/Random_forest)를 활용해 림프절의 전이 상태 \(Normal / ITC / Micro / Macro\)를 분류합니다.
    4. **결과 종합하여 pN-stage 결**

       한 환자로부터 채취한 5개의 림프절에 대해 전이 상태를 모두 분류한 뒤, 이를 종합하여 해당 환자의 pN-stage를 아래와 같이 결정.

       * **pN0**: 모든 림프절 전이상태가 normal일 경우
       * **pN0\(i+\)**: Micro와 macro 전이상태는 없지만, ITC 전이상태가 존재할 경우
       * **pN1mi**: Macro 전이상태는 없지만, Micro 전이상태가 존재할 경우
       * **pN1**: 1~3개의 전이된 림프절이 존재하면서, 적어도 1개 이상이 Macro 전이상태일 경우
       * **pN2**: 4~9개의 전이된 림프절이 존재하면서, 적어도 1개 이상이 Macro 전이상태일 경우





