---
description: MULTI-SCALE CONTEXT AGGREGATION BY DILATED CONVOLUTIONS
---

# \(2016\) Dilated Convolution

* 논문링크: [https://arxiv.org/pdf/1511.07122.pdf](https://arxiv.org/pdf/1511.07122.pdf)
* 참조자: 
  * [http://blog.naver.com/laonple/220991967450](http://blog.naver.com/laonple/220991967450)
  * [https://m.blog.naver.com/PostView.nhn?blogId=sogangori&logNo=220952339643&proxyReferer=https%3A%2F%2Fwww.google.com%2F](https://m.blog.naver.com/PostView.nhn?blogId=sogangori&logNo=220952339643&proxyReferer=https%3A%2F%2Fwww.google.com%2F)

## Receptive Field ?!

![](../.gitbook/assets/image%20%2891%29.png)

* 위 그림 설명:
  * 일반적인 convolution에서 receptive field 는 좌측 입력 레이어의 중앙 진분홍색 공간이 된다.
  * convolution 연산의 필터\(커널\)의 크기와 같다.
* receptive field 는 **어떤 출력 레이어의 뉴런 하나에 영향을 미치는 입력 뉴런들의 공간 크기**이다.
* receptive field의 크기는 얼마나 많은 문맥정보를 사용하는지를 의미한다.
* receptive field의 크기가 작으면 지역 정보는 충분하게 추출될 수 있지만 전역적, 문맥정 정보는 부족할 것이다.
* Dilated Convolution은 이러한 문제를 해결하기 위한 방법 중 하나이다.

## Dilated Convolution ?!

![](../.gitbook/assets/image%20%2874%29.png)

* 위 그림 설명:
  * 좌측: 
    * 파란색: D=1이고 커널 크기가 3 x 3 인 경우에 사용 되는 입력 노드
    * receptive field 크기:  3x3 = 9
  * 중간:
    * 파란색:  D=2이고 커널 크기가 3 x 3 인 경우에 사용 되는 입력 노드
    * receptive field 크기:  5x5 = 25
  * 우측: 
    * 파란색: D=3이고 커널 크기가 3 x 3 인 경우에 사용 되는 입력 노드
    * receptive field 크기:  7x7 = 49







