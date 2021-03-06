---
description: 안저 이미지 관리 장치 및 안저 이미지의 적합성 판단 방법
---

# \(2018\) 특허-안저영상 적합성 판단

* 과제의 해결 수단
  * 대상 적합성 판단을 위해, 질병별 품질기준이 따로 있음
  * 대상 질병이 제1 질병인 경우 안저 이미지가 제1 품질 기준을 만족하는지 판단
  * 품질 기준별로 관련된 영상의 영역이 따로 있음
  * **이미지 획득부**
  * **품질 판단부:** 제1 영역과 관련된 제 1 품질 기준을 만족하는지 여부를 판단하고, 안저 이미지가 제1 영역과 적어도 일부 구별되는 제2 영역과 관련 되는 제2 품질 기준을 만족하는지 여부를 판단
  * **적합성 판단부:** 대상 질병이 제1 질병이고 안저 이미지가 제1 품질 기준을 만족하는 경우 안저 이미지가 제1 적합성을 만족하는 것으로 판단.



* 1. 안저 이미지를 이용한 진단 보조
* 1.1 진단 보조 시스템 및 프로세스
* 1.1.1 목적 및 정의
* 1.1.2 진단 보조 시스템 구성
  * 진단 보조 시스템 \(20\)
    * 진단 모델을 트레이닝하는 학습 장치\(1000\),
      * 학습부\(100\): 데이터 획득. 신경망 모델 학습.
        * 데이터 가공 모듈\(110\)
        * 큐 모듈\(130\)
        * 학습 모듈\(150\)
        * 학습 결과 획득 모듈\(170\)
    * 진단 모델을 이용하여 진단을 수행하는 진단 장치\(2000\)
      * 진단부\(200\): 진단 및 진단 보조 정보 획득.
        * 진단 요청 획득 모듈\(210\)
        * 데이터 가공 모듈\(230\)
        * 진단 모듈\(250\)
        * 출력 모듈\(270\)
    * 진단 요청을 획득하는 클라이언트 장치\(3000\)
      * 촬상부\(300\): 영상 촬영.
  * 1.1.2.1 학습 장치
    * 학습 장치\(1000\)
      * 제어부\(1200\) 
      * 메모리부\(1100\)
      * 통신부\(1300\)
  * 1.1.2.2 진단 장치
    * 진단 장치\(2000\)
      * 제어부\(2200\)
      * 메모리부\(2100\)
      * 통신부\(2300\)
      * 프로세서\(2050\)
        * 가공 모듈\(2051\)
        * 진단 모듈\(2053\)
      * 휘발성 메모리\(2030\),
      * 비휘발성 메모리\(2010\)
      * 대용량 저장 장치\(2070\) 
      * 통신 인터페이스\(2090\)
  * 1.1.2.3 서버 장치
    * 진단 서버\(4000\)
  * 1.1.2.4 클라이언트 장치
    * 클라이언트 장치\(3000\)
      * 촬상부\(3100\)
      * 제어부\(3200\)
      * 통신부\(3300\)
* 1.1.3 진단 보조 프로세스 개요
* 1.2 트레이닝 프로세스
* 1.2.1 학습부
* 1.2.2 데이터 가공 프로세스
  * 1.2.2.1 이미지 데이터 획득
  * 1.2.2.2 이미지 리사이징
  * 1.2.2.3 이미지 전처리
  * 1.2.2.4 이미지 증강\(augmentation\)
  * 1.2.2.5 이미지 직렬화\(serialization\)
  * 1.2.2.6 큐\(Queue\)
* 1.2.3 학습 프로세스
  * 1.2.3.1 데이터 입력
  * 1.2.3.2 모델의 설계
  * 1.2.3.3 모델의 학습
  * 1.2.3.4 모델의 검증\(validation\)
  * 1.2.3.5 모델의 테스트
  * 1.2.3.6 결과의 출력
  * 1.2.3.7 모델 앙상블\(ensemble\)
* 1.3 진단 보조 프로세스
* 1.3.1 진단부
* 1.3.2 데이터 획득 및 진단 요청
* 1.3.3 데이터 가공 프로세스
* 1.3.4 진단 프로세스
  * 1.3.4.1 데이터 입력
  * 1.3.4.2 데이터 분류
    * 진단 정보\(즉, 질병의 유무에 대한 정보\) 또는 소견 정보\(즉, 이상 소견의 유무에 대한 정보\)를 예측
    * CAM\(Class Activation Map\)이 획득
* 1.3.5 진단 보조 정보의 출력
  * 진단 보조 정보는, 진단 보조 신경망 모델로부터 예측된 라벨에 기초하여 결정
  * CAM
  * 적합성 정보
    * **제1 등급 정보:** 신경망 모델이 비정상이라고 예측한 경우
      * 해당 병의 처방
      * 전원 절차 안내
    * **제2 등급 정보:** 신경망 모델이 비정상이라고 예측하지 않은 경우
      * 다음 진료 시기
      * 다음 진료 과목
    * **제3 등급 정보:** 진단 대상 이미지의 품질이 기준 이하인 경우 

      * 흠결의 정보\(예컨대, bright artifact 인지 또는 dark artifact 인지, 혹은 그 정도\)
* 1.4 복수 라벨 대한 진단 보조 시스템
  * 복수의 진단 보조 정보를 획득하기 위한 복수의 신경망 모델을 학습시키고, 학습된 복수의 신경망 모델을 이용하여 복수의 진단 보조 정보를 획득
* 1.4.1 병렬 진단 보조 시스템 구성
* 1.4.2 병렬 트레이닝 프로세스
  * 1.4.2.1 병렬 학습부
  * 1.4.2.2 병렬 데이터 획득
  * 1.4.2.3 병렬 데이터 가공
  * 1.4.2.4 병렬 큐
  * 1.4.2.5 병렬 학습 프로세스
  * 1.4.2.6 병렬 앙상블 학습 프로세스
* 1.4.3 병렬 진단 프로세스
  * 1.4.3.1 병렬 진단부
  * 1.4.3.2 병렬 진단 프로세스
  * 1.4.3.3 진단 보조 정보의 출력
* 1.5 사용자 인터페이스
* 2. 안저 이미지의 품질 판단
* 2.1 서론
* 2.1.1 배경 및 목적
* 2.1.2 안저 이미지
* 2.2 이미지 관리 시스템 및 장치
* 2.2.1 시스템 및 동작
* 2.2.2 장치 및 동작
  * 품질 판단부
    * 이미지의 대상 영역으로부터 제1 픽셀을 검출하고, 제1 픽셀의 수를 고려하여 안저 이미지의 품질을 판단
    * 제1 픽셀 검출부
    * 비교부
    * 품질 정보 획득부
    * 품질 판단부는, 제1 영역과 관련된 제1 품질 기준을 만족하는지 여부를 판단하고,  안저 이미지가 제1 영역과 적어도 일부 구별되는 제2 영역과 관련되는 제2 품질 기준을 만족하는지 여부를 판단할 수 있다.
    * 품질 판단부는, 제1 영역을 고려하여 제1 아티팩트의 유무를 판단하고, 제2 영역을 고려하여 제2 아티팩트의 유무를 판단할 수 있다.
    * * 
  * 대상영역
    * ….
  * **아티팩트**
    * **제1 아티팩트:** 
      * 대상영역이 제1 영역
      * 기준 픽셀 값은 제1 픽셀 값
      * 기준 픽셀 값과 비교하여 제1 픽셀을 검출하는 단계는 대상 영역에 포함되고 기준 픽셀 값보다 큰 픽셀 값을 가지는 제1 픽셀을 검출
      * 기준 비율은 제1 기준 비율
      * 안저 이미지의 전체 픽셀 중 제1 픽셀이 차지하는 비율이 제1 기준 비율보다 큰 경우 안저 이미지는 불량이미지인 것으로 판단
* * * **제2 아티팩트:** 
      * 대상영역이 제2 영역
      * 기준 픽셀 값은 제2 픽셀 값
      * 기준 비율은 제2 기준 비율
      * 안저 이미지의 전체 픽셀 중 제1 픽셀이 차지하는 비율이 제2 기준 비율보다 작은 경우 안저 이미지는 불량 이미지인 것으로 판단
  * 적합성 판단부
    * 적합성 판단부는, 대상 질병이 제1 질병이고 안저 이미지가 제1 품질 기준을 만족하는 경우 안저 이미지가 제1적합성을 만족하는 것으로 판단
* 2.3 이미지 품질 판단 방법
* 2.3.1 일반
  * 브라이트 아티팩트\(a\), 다크 아티팩트\(b\), 혼탁\(c\)
* 2.3.2 판단 방법
  * 이미지를 회색조\(그레이스케일\) 처리
  * 안저 이미지는 RGB 채널을 포함하고, 픽셀 값은 R 채널 이미지로부터 획득
  * 이미지를 이진화
  * 안저 이미지의 히스토그램을 획득
  * 히스토그램 정규화 또는 평활화\(equalization\)
  * 이미지의 품질 정보는 품질 값, 품질 점수, 품질 정도 등을 포함할 수 있다. 
    * 품질 점수 등은 아티팩트에 대응되는 픽셀의 수, 비율 등을 고려하여 산출될 수 있다. 
    * 품질 점수 등은 포함하는 아티팩트의 종류 및/또는 아티팩트의 정도를 고려하여 산출될 수 있다. 
    * 품질 점수 등은 신경망 모델을 통해 획득될 수 있다.
  * 픽셀 값은 픽셀의 세기 값\(intensity value\), 강도 값, 밝기 값, 휘도 값, 색상 값, 색도 값, 채도 값 및 명도 값 중 어느 하나일 수 있다. 
  * 픽셀 값은 정수 또는 벡터 형태일 수 있다. 
  * 픽셀 값의 분포, 픽셀 값의 편차\(deviation\), 픽셀 값의 최대값 또는 최소값, 픽셀 값들 간의 차이 등\(이하, 픽셀 값의 가공 값\)을 이용하여 수행될 수 있다.
  * 안저 이미지가 품질 조건을 만족하는지 판단하는 것은, 픽셀 값 또는 픽셀 값의 가공 값 등이 **기준 범위**에 속하거나, 기준 값을 초과하거나, 기준 값에 못 미치는 경우, 해당 안저 이미지는 품질 조건을 만족하는 것으로 판단될 수 있다.
  * \[0729\]
* 2.3.3 검출 대상 종류별 알고리즘
* 2.3.3.1 제1 타입 아티팩트\(브라이트 아티팩트\)
* 2.3.3.2 제2 타입 아티팩트\(다크 아티팩트\)
* 2.3.3.3 제3 타입 아티팩트\(혼탁\)
* 2.3.4 복수의 안저 이미지
  * 복수의 안저 이미지 중 선택된 적어도 하나의 안저 이미지를 병합 또는 합성하여 생성된 하나의 안저 이미지가 진단 대상 안저 이미지로 결정될 수 있다
* 2.3.5 결과의 이용
* 2.3.5.1 라벨링
* 2.3.5.2 출력
* 2.4 질병 특이적 품질 판단 방법\(적합성 판단 방법\)
* 2.4.1 질병 특이적 특성
  * 대상 질병:
    * 제1 타입 질병: 대상 질병은 어떠한 아티팩트를 포함하는 안저 이미지에 기초하여서도 진단 가능
    * 제2 타입 질병: 어떠한 아티팩트를 포함하는 안저 이미지에 기초하여서도 진단이 곤란
    * 제 3타입 질병:  특정 종류의 아티팩트를 포함하는 안저 이미지에 기초하여서는 진단이 곤란
  * 아티팩트:
    * 제1 타입 아티팩트: 안저 이미지에 기초한 모든 질병의 진단을 곤란하게함
    * 제2 타입 아티팩트: 안저 이미지에 기초한 질병의 진단에 방해되지 않음
    * 제3 타입 아티팩트: 안저 이미지에 기초한 특정 질병 군의 진단에 방해
* 2.4.2 질병 적합성 판단 방법
* 2.5 품질 판단 결과의 적용
* 2.5.1 데이터 관리 단계
* 2.5.2 학습 단계
* 2.5.3 진단 단계

요약?!

* 안저 이미지를 이용한 진단 보조 시스템:
  * 구성 및 프로세스
*  안저 이미지 품질 판단 방법:
  * 구성및 프로세스
  *  판단을 위한 규칙 및 여러 요소들을 정의
    *  대상 영역\(제 1영역, 제 2영역\)
    *  대상 영역별 품질기준\(제1 품질 기준, 제2 품질 기준\)
    *  대상 영역별 아티팩트\(제1 아티팩트, 제2 아티팩트\)
    *  기준 픽셀\(제1 기준 픽셀, 제2 기준 픽셀\)
    *  기준 비율\(제1 기준 비율, 제2 기준 비율\)
    *  품질 픽셀값을 얻기 위한 방법
    *  대상 질병\(제1-3 타입 질병\) 







