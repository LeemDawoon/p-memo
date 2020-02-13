---
description: 모바일에서 딥러닝 모델의 속도를 높이는 방법
---

# Quantization

* 참조 페이지링크:
  * [https://www.tensorflow.org/lite/performance/model\_optimization](https://www.tensorflow.org/lite/performance/model_optimization)



* DNN을 Quantizing 하는 것은 학습된 모델의 weight의 정밀도를 줄이는 것을 뜻한다. 이로 인해,  Latency\(interence 시 걸리는 시간\)을 줄일 수 있다. 다만 모델의 정확도는 조금 줄어들 수 있다
* Tensorflow Lite에서는 2가지 정도의 quantization 방법을 제공한다.
  * Post-training quantization: 정확도 손실이 너무 심하면 아래 방법 고려 해볼것.
  * Quantization-aware training:  학습할때부터 양자화 고려하는 방법인데,  documentation이 부족하다.

아래는 Post-training quantization 의 3가지 옵션에 대한 내용이다.

* **1.Weight quantization:**
  * 장점: 4x smaller, 2-3x speedup, accuracy
  * 아래는 코드
  * ```text
    import tensorflow as tf
    converter = tf.lite.TFLiteConverter.from_saved_model(saved_model_dir)
    converter.optimizations = [tf.lite.Optimize.OPTIMIZE_FOR_SIZE]
    tflite_quant_model = converter.convert()
    ```
  * 부동 소수점에서 8 비트 정밀도 \( "하이브리드"양자화라고도 함\)까지의 가중치 만 양자화한다.
  * 출력은 여전히 부동 소수점을 사용하여 저장 되므로, 속도는 전체 고정 소수점 계산보다 느리다.
* **2. Full integer quantization:**
  * 장점; 4x smaller, 3x+ speedup
  * 양자화가 구현되지 않은 연산은 부동소수점으로 남는다.
  * 요 모델도 출력은 여전히 부동 소수점을 사용한다.
  * ```text
    import tensorflow as tf

    def representative_dataset_gen():
      for _ in range(num_calibration_steps):
        # Get sample input data as a numpy array in a method of your choosing.
        yield [input]

    converter = tf.lite.TFLiteConverter.from_saved_model(saved_model_dir)
    converter.optimizations = [tf.lite.Optimize.DEFAULT]
    converter.representative_dataset = representative_dataset_gen
    tflite_quant_model = converter.convert()
    ```
  * 아래 코드를 사용하여 모든 연산자과 입출력을 integer quantization으로 강제할 수 있는데, 양자화 할 수 없으면,  수행시 오류를 뱉으므로, 한번 수행해볼만 하다.
  * ```text
    converter.target_spec.supported_ops = [tf.lite.OpsSet.TFLITE_BUILTINS_INT8]
    converter.inference_input_type = tf.uint8
    converter.inference_output_type = tf.uint8
    ```
* **Float16 quantization**

  * 장점: 2x smaller, potential GPU acceleration
  * ```text
    import tensorflow as tf
    converter = tf.lite.TFLiteConverter.from_saved_model(saved_model_dir)
    converter.optimizations = [tf.lite.Optimize.DEFAULT]
    converter.target_spec.supported_types = [tf.lite.constants.FLOAT16]
    tflite_quant_model = converter.convert()
    ```

* efficient\_det\_b0.weight.tflite : 3
* efficient\_det\_b0.float16.tflite: 1.3초
* efficient\_det\_b1.float16.tflite: 2.5초

