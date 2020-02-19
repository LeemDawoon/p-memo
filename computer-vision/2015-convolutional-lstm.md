---
description: >-
  Convolutional LSTM Network: A Machine LearningApproach for Precipitation
  Nowcasting
---

# \(2015\) Convolutional LSTM

## 2 Preliminaries 

### 2.1 Formulation of Precipitation Nowcasting Problem

* 강우량 실시간 예측문제.
  * 공간 영역은 M개의 row와 N개의 column의 로 구성되는 grid cell 로 표현.
  * P는 시간에 따른 측정값.
  * 시간에 따른 관찰값은 아래와 같이 X로 표현.

![](../.gitbook/assets/image%20%2830%29.png)

* 일정시간 간격으로 관찰하면, 다음과 같은 sequence 데이터를 얻을 수 있다.
  * 

![](../.gitbook/assets/image%20%2871%29.png)

* 시공간적 예측문제\(spatiotemporal sequence forecasting problem\)는. 아래과 같은 식으로 표현될 수있다. J개의 관찰값이 주어졌을 때, K개의 값을 예측.
  * 

![](../.gitbook/assets/image%20%2836%29.png)

## 3 The Model

* 기존 FC-LSTM 도 좋은 성능을 내었지만, 공간적 정보가 너무 중복된다.

### 3.1 Convolutional LSTM

![LSTM](../.gitbook/assets/image%20%286%29.png)

![Inputs](../.gitbook/assets/image%20%2884%29.png)

![cell outputs](../.gitbook/assets/image%20%2838%29.png)

![hidden states](../.gitbook/assets/image%20%2847%29.png)

![gates](../.gitbook/assets/image%20%2886%29.png)

![ConvLSTM](../.gitbook/assets/image%20%2864%29.png)

* o 은 element-wise 곱.
* 별표는 컨볼루션 연산.

![](../.gitbook/assets/image%20%2860%29.png)

### 3.2 Encoding-Forecasting Structure

![](../.gitbook/assets/image%20%2814%29.png)

* encoding network와  forecasting network 2가지 네트워크로 구성된다.
* 두 네트워크는 ConvLSTM 가 쌓여서 구성된다.
* encoding LSTM:
  * 전체 input sequence를 hidden state로 압축한다.
* forecasting LSTM:
  * encoding LSTM에서 얻은 hidden state를 unfold 하여 최종 prediction을 만든다.
  * forecasting network 의 모든 state를 concat하여 1x1 conv에 통과하여 최종 예측 output을 얻는다.
  * 최종 prediction은 input과 차원이 같다.

![](../.gitbook/assets/image%20%287%29.png)





