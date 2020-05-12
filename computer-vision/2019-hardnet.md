---
description: 'HarDNet: A Low Memory Traffic Network'
---

# \(2019\) HarDNet

* 논문링크: [https://arxiv.org/pdf/1909.00948.pdf](https://arxiv.org/pdf/1909.00948.pdf)
* 코드: [https://github.com/PingoLH/Pytorch-HarDNet](https://github.com/PingoLH/Pytorch-HarDNet)





3. Proposed Harmonic DenseNet

3.1. Sparsification and weighting

* 2^n이 k로 나눠지면, 레이어 k를 k–2^n 레이어에 연결한다.
* n은 non-negative integer여서 k–2^n  &gt;= 0  이다.
* 0번 레이어는 입력레이어 이다.
* 이러한 connection scheme에서,  2^n 번째 레이어가 처리되었을때, 1부터 2^n -1까지의 레이어는메모리로 부터 flush 될수 있다.
* The connections make the network appear as an overlapping of power-of-two-th harmonic waves\(아래 그림 참조\)
* 
![](../.gitbook/assets/image%20%28142%29.png)

* In the proposed network, layers with an index divided by a larger power of two are more influential than those that divided by a smaller power of two.
*  We amplify these key layers by increasing their channels, which can balance the channel ratio between the input and output of a layer to avoid a low MoC

































