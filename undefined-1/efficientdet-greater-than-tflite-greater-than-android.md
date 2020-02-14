---
description: 삽질정리
---

# EfficientDet =&gt;tflite =&gt;Android

* base code: [https://github.com/xuannianz/EfficientDet.git](https://github.com/xuannianz/EfficientDet.git)
  * 2020-02-13 기준으로 tensorflow 버전이 1.15.0 이다.
* 1.코드 다운 받고
* 2. tensorflow==2.1.0에서 코드를 돌려보는데 에러가 나는 부분 수정해서 2.1.0에서 돌아가도록 함.
* 3. custom operator 제거\(변경\)
  * swish =&gt; relu \(learning rate를 조금 더 크게 바꿔주면 그래도 잘 학습된다.\)
  * FixedDropout =&gt; Dropout
  * PriorProbability=&gt; 대충찍어보면, -1.9955 정도의 값을 bias 초기화에 사용
* 4. 학습.
* 5. tflite 컨버팅
  * 저장된 h5 모델을 읽어 온다.
  * 하지만 loss가 커스텀이라...   get\_custom\_objects\(\)로 우선 loss 만 업데이트 해준다.
  * 그리고 loss 날리고 다시 저장한다. tf.keras.models.save\_model\(..., include\_optimizer=False, ...\)
  * 새로 저장한 모델을 읽어서 컨버팅을 수행하면 된다.
  * 입출력 포맷을 함 확인본다.
    * interpreter.get\_input\_details\(\)
    * interpreter.get\_output\_details\(\)
  * 참고: Quantization:
    * 쉽게 할 수 있는 방법 2개\(Weight, Float16\)가 있다.
    * Weight은 모델 사이즈가 Float16 방법 보다 작아진다. 근데 속도는 Float16이 더 빠르다.
* 6. Android 자바에서 후처리 함수 구현
  * 사실 prediction 모델은 train 모델과 다르게,  anchor 박스와 bbox 결과 합치는 과정 및 NMS 등의 후처리 과정이 커스텀 레이어로 구현되어 있다.
  * 우리는 이걸 컨버팅 못하였으니까, 그런 기능을 하는 코드를 java로 짜주면 된다.
  * 그리고 EfficientDet 모델 B넘버에 따라 필요한 anchor 가 다르므로, python에서 csv 파일로 저장해서, 자바에서 csv 함수를 읽어서 후처리 할때 사용하면 된다.
  * 이런 꼼수를 쓰지 않으면, anchor박스를 계산하는 코드도 java 로 짜야한다....

---------

* efficient\_det\_b0.weight.tflite : 3
* efficient\_det\_b0.float16.tflite: 1.3초
* efficient\_det\_b1.float16.tflite: 2.5초

