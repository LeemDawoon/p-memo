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
* 바꾼담에 학
* efficient\_det\_b0.weight.tflite : 3
* efficient\_det\_b0.float16.tflite: 1.3초
* efficient\_det\_b1.float16.tflite: 2.5초

