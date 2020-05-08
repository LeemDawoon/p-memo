# Digital Pathology

* \(2020\) Segmentation and Classification in Digital Pathology for Glioma Research: Challenges and Deep Learning Approaches
  * 논문링크: [https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7046596/pdf/fnins-14-00027.pdf](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7046596/pdf/fnins-14-00027.pdf)
  * 



* \(2018\) A Robust and Effective Approach Towards Accurate Metastasis Detection and pN-stage Classification in Breast Cancer: 
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





