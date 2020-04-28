---
description: Context Encoding for Semantic Segmentation
---

# \(2018\) EncNet

* 논문링크: [https://arxiv.org/pdf/1803.08904.pdf](https://arxiv.org/pdf/1803.08904.pdf)
* 코드링크: [https://github.com/zhanghang1989/PyTorch-Encoding](https://github.com/zhanghang1989/PyTorch-Encoding)

​

* Context Encoding Network \(EncNet\)
* Semantic Encoding Loss \(SE-loss\)



## 2. Context Encoding Module

![](../.gitbook/assets/image%20%2834%29.png)



Context Encoding:

* We employ the **Encoding Layer \[58\]** to capture the feature statistics as a global semantic context.
  * \[58\] Deep TEN: Texture Encoding Network \([https://arxiv.org/pdf/1612.02844.pdf](https://arxiv.org/pdf/1612.02844.pdf)\)
  * ![](../.gitbook/assets/image%20%2837%29.png)
* We refer to the output of Encoding Layer as _encoded semantics_
* 
