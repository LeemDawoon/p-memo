---
description: >-
  Quantifying the effects of data augmentation and stain color normalization in
  convolutional neural networks for computational pathology
---

# \(2019\) Pathology에서 Augmentation과 Normalization 효과의 정량화

* 논문 링크: [https://arxiv.org/pdf/1902.06543.pdf](https://arxiv.org/pdf/1902.06543.pdf)
* 요약:
  * Network-based Normalization 후, HSV-light 하는게 좋았다.

## 1. Introduction

* augmentation: variation에 상관없게끔 Task 모델을 학습.
* normalization: 정규화를 통해 Train 셋과 Test 셋의 variation을 줄임.

1.4. Contributions 

Our contributions can be summarized as follows: 

* CNN 분류 성능에 미치는 영향을 정량화하기 위해 잘 알려진 여러 가지 stain color augmentation 및 stain color normalization 알고리즘을 체계적으로 평가했다.
* 우리는 유사 분열 검출, 림프절에서의 종양 전이 검출, 전립선 상피 검출, 및 멀티 클래스 결장 직장암 조직 분류의 4 가지 관련 분류 작업\(

  * mitosis detection, 
  * tumor metastasis detection in lymph nodes, 
  * prostate epithelium detection, 
  * and multiclass colorectal cancer tissue classification.\)

  에 걸쳐 총 9 개의 상이한 센터로부터의 데이터를 사용하여 앞선 평가를 수행 하였다.

* 우리는 unsupervised image-to-image translation으로 stain color normalization 문제를 공식화하고 이를 해결하기 위해 신경망을 훈련했다.

## 2. Materials\(데이터\)

* \(대략 작성하여, 자세한건 논문직접보기.\)
* Radboud University Medical Centre \(Radboudumc or rumc\)의 데이터만을 학습에 사용하고 나머지 센터의 데이터는 테스트 용으로 사용했다.
* RGB 패치는 128x128 로 추출했다.

2.1. Mitotic figure detection

* 분열을 하고 있는 세포를 검출하는 것으로 이진류 문제이다.
* TUPAC Challenge에서 받아올 수 있는 public 데이터이다.
* 0.25 µm/pixel resolution

2.2. Tumor metastasis detection

* metastatic tumor cell 을 포함하고 있는지 분류하는 이진분류 문제이다.
* Camelyon17 Challenge 에서 얻을 수 있는 public 데이터이다.
* t 0.25 µm/pixel resolution

2.3. Prostate epithelium detection

* prostate\(전립선\) 조직에 epithelial\(상피\) 세포가 있는지 분류하는 이진분류 문제이다.
* 요것도 공개되어 있고.

2.4. Colorectal cancer tissue type classification

* 9개의 colorectal cancer \(CRC\)를 분류하는 문제이다.
  * 1\) tumor, 2\) stroma, 3\) muscle, 4\) lymphocytes, 5\) healthy glands, 6\) fat, 7\) blood cells, 8\) necrosis and debris, and 9\) mucus.
* 0.5 µm/pixel resolution
* 요 테스트 에선 따로 2개의 데이터셋을 추가로 사용.

2.5. Multi-organ dataset

* stain color normalization 문제를 풀기위한 네트워크를 학습시기기 위해, 앞서 말한 데이터를 통합해서 만들었다.

## 3. Methods

3.1. Stain color augmentation

![](../.gitbook/assets/image%20%2836%29.png)

* **Basic**:  90 degree rotations / vertical and horizontal mirroring.
* **Morphology:**
* * basic augmentation\(위에 basic에서 확장\)
  * **scaling: \[0.8, 1.2\]**
    * 
  * **elastic deformation: α ∈ \[80, 120\] and σ ∈ \[9.0, 11.0\]**
  * **additive Gaussian noise\(perturbing the signal-to-noise ratio\):  σ ∈ \[0, 0.1\]**
  * **Gaussian blurring \(simulating out-of-focus artifacts\): σ ∈ \[0, 0.1\]**
  * tf.keras.layers.GaussianNoise\(stddev\)
    * mu, sigma = 0, 0.1 
    * noise = np.random.normal\(mu, sigma, \[2,2\]\) 
    * signal = clean\_signal + noise
  * cv2.GaussianBlur\(img, \(5,5\), sigmaa\)
* **Brightness & contrast \(BC\)**: 
  * 위에 Morphology에서 확장.
  *  **brightness intensity ratio: \[0.65, 1.35\]**
  *  **contrast intensity ratio: \[0.5, 1.5\]**
  * tf.keras.preprocessing.image.ImageDataGenerator\(
    * brightness\_range\)
  * 
* **Hue-Saturation-Value \(HSV\):**
  * 위에 BC에서 확장
  * randomly shifting the hue and saturation channels in the HSV color space
  * **HSV-light\(hue and saturation intensity ratios\): \[−0.1, 0.1\]**
  * **HSV-strong\(hue and saturation intensity ratios\):  \[−1, 1\]**
* **Hematoxylin-Eosin-DAB \(HED\)**
  * 위에 BC에서 확장
  * color variation routine specifically designed for H&E images 
  * This method followed three steps. 
    * First, it disentangled the hematoxylin and eosin color channels by means of color deconvolution using a fixed matrix. 
    * Second, it perturbed the hematoxylin and eosin stains independently. 
    * Third, it transformed the resulting stains into regular RGB color space.
  * **HED-light\(intensity ratios for all HED channels\): \[−0.05, 0.05\]**
  * **HED-strong\(intensity ratios for all HED channels\): \[−0.2, 0.2\]**

**basic**

`tf.keras.preprocessing.image.ImageDataGenerator(rotation_range, horizontal_flip, vertical_flip)`

scaling

tf~~.~~keras.preprocessing.image.ImageDataGenerator\(zoom\_range\)

\*\*\*\*

\*\*\*\*

3.2. Stain color normalization

![](../.gitbook/assets/image%20%2848%29.png)

* Identity: baseline. 아무것도 안한.
* Grayscale:
* LUT-based:
  * tissue morphology를 사용한 접근법.
  * It detects cell nuclei in order to precisely characterize the H&E\(Haematoxylin and eosin\) chromatic distribution and density histogram for a given WSI\(Whole Slide Image\).
  * First, it does so for a given template WSI, e.g. an image from the training set, and a target WSI. Second, the color distributions of the template and target WSIs are matched, and the color correspondence is stored in a lookup table \(LUT\). Finally, this LUT is used to normalize the color of the target WSI.
* **Network-based:**
  * **학습할때 Augmentation:**
    * **hue, saturation and value channel ratios between \[−1, 1\]** 
  * U-Net 구조 차용.
    * downward path of 5 layers of strided convolutions with 32, 64, 128, 256 and 512 3x3 filters, stride of 2, batch normalization \(BN\) and leaky-ReLU activation\(LRA\).
    * upward path consisted of 5 upsampling layers, each one composed of a pair of nearest-neighbor upsampling and a convolutional operation with 256, 128, 64, 32 and 3 3x3 filters, BN and LRA; except for the final convolutional layer that did not have BN and used the hyperbolic tangent \(tanh\) as activation function.
  * We used long skip connections to ease the synthesis upward path \(Ronneberger et al. \(2015\)\), and applied L2 regularization with a factor of 1 × 10−6 .
  * Loss: mean squared error \(MSE\) loss 
  * Optimizer: stochastic gradient descent with Adam optimization \(Kingma and Ba \(2014\)\) 
  * batch: 64
  * decreasing the learning rate by a factor of 10 starting from 1 × 10−2 every time the validation loss stopped improving for 4 consecutive epochs until 1 × 10−5

3.3. CNN Classifiers

* stain color augmentation 와 stain color normalization의 효과를 측정하기 위한 모델.

## 4. Experimental results

![](../.gitbook/assets/image%20%2815%29.png)

* best는 
  * Network-based Normalization 후, HSV-light
  * 

