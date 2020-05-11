# Digital Pathology

## 적용해봤으면 하는 연구.

* multiple instance learning: 
  * 종양영역이 MSI-H와 정말 관련 있는지 확인해보는 실험.
  * 또는 classification에 사용.
* few-shot learning for classification:
  * 

## Digital Pathology 관련 연구.

* **\(2020\) Segmentation and Classification in Digital Pathology for Glioma Research: Challenges and Deep Learning Approaches**
  * 논문링크: [https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7046596/pdf/fnins-14-00027.pdf](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7046596/pdf/fnins-14-00027.pdf)
  * MICCAI 2018 CPM challenge 에서 2등.
  * Task1: Instance Segmentation of Nuclei in Brain Tissue Images
  * Task2: Methods for Classification of Brain Cancer Cases
    * Methods for Classification of Brain Cancer Cases: WSI와 뇌 MRI가 같이 주어지며, 악성 세포를 분류하는 문제 같음.
    * WSI 쪽만 봐보자.
  * **Histopathology classification model:**

![Analysis pipeline for whole slide tissue images](../.gitbook/assets/image%20%2874%29.png)

*  * **Histopathology classification model:**
    * multiple instance learning approach
    * pre-processing:
      * tissue detection: Otsu thresholding.
      * color normalization: histogram equalization
      * tiling: 랜덤 샘플링해서 20개의 448 × 448-pixel patches를 추출.
    * feature extraction:
    * classification: 

![](../.gitbook/assets/image%20%28171%29.png)



* **\(2019\) CAMEL: A Weakly Supervised Learning Framework for Histopathology Image Segmentation**

  *  논문링크: [https://arxiv.org/pdf/1908.10555.pdf](https://arxiv.org/pdf/1908.10555.pdf)
  * 코드링크: [https://github.com/ThoroughImages/CAMEL](https://github.com/ThoroughImages/CAMEL)
  * Task
    * WSI에 분류라벨만 있을 경우, segmentation 하는 방법.
    * ![](../.gitbook/assets/image%20%2830%29.png)
  * Method
    * Multiple instance 접근법
    * ![](../.gitbook/assets/image%20%28163%29.png)





  ![](blob:https://doaiacropolis.atlassian.net/43269653-a63c-450c-9889-61e972d2a8cf#media-blob-url=true&id=f6ec2f40-fa33-42f0-bc4a-07848e3a74a8&collection=contentId-425099485&contextId=425099485&mimeType=image%2Fpng&name=image-20200511-092507.png&size=343303&width=1203&height=705)

* **\(2019\) Accurate Gastric Cancer Segmentation in Digital Pathology Images Using Deformable Convolution and Multi-Scale Embedding Networks**
  * 논문링크:  [https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=8721664](https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=8721664)
  * contributions : 
    * 1\) We have created a clinical gastric cancer segmentation dataset for our research, which has been delicately annotated by medical specialists.
    *  2\) We have proposed multi-scale embedding networks for segmenting cancerous regions of various sizes, in which we have integrated **Atrous Spatial Pyramid Pooling module** and **encoder-decoder based semantic-level embedding networks.**
    *  3\) We have applied the **deformable convolutional module** for adapting to the non-rigid characters of pathological images, and we have utilized the **dense upsampling convolution** for boundary refinement at the end of our architecture.
  * Method: 
    * A. SELECTED BACKBONE RESNET-101
    * B. ENCODER WITH DEFORMABLE CONVOLUTION AND ATROUS CONVOLUTION
      * Deformable convolution integration:
      * Atrous Spatial Pyramid Pooling:
    * C. SEMANTIC-LEVEL EMBEDDING STRATEGY
    * D. PROPOSED DECODER AND DENSE UPSAMPLING
    * E. OVERALL FRAMEWORK
    * ![](../.gitbook/assets/image%20%2819%29.png)
* **\(2019\): Computational Nuclei Segmentation Methods in Digital Pathology: A Survey**
  * 논문링크: [https://www.researchgate.net/profile/Surya\_Prasath/publication/336347859\_Computational\_Nuclei\_Segmentation\_Methods\_in\_Digital\_Pathology\_A\_Survey/links/5da687da299bf1c1e4c37d41/Computational-Nuclei-Segmentation-Methods-in-Digital-Pathology-A-Survey.pdf](https://www.researchgate.net/profile/Surya_Prasath/publication/336347859_Computational_Nuclei_Segmentation_Methods_in_Digital_Pathology_A_Survey/links/5da687da299bf1c1e4c37d41/Computational-Nuclei-Segmentation-Methods-in-Digital-Pathology-A-Survey.pdf)
  * 세그멘테이션할때, HSV 컬러 스페이스가 좋다는 것 같음...
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
  * augmentation:





![](../.gitbook/assets/image%20%2818%29.png)

![](../.gitbook/assets/image%20%28115%29.png)

![](../.gitbook/assets/image%20%28149%29.png)







