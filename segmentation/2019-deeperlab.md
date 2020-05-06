---
description: 'DeeperLab: Single-Shot Image Parser'
---

# \(2019\) DeeperLab

* 논문링크:  [https://arxiv.org/pdf/1902.05093.pdf](https://arxiv.org/pdf/1902.05093.pdf)



Abstract 

* whole image parsing:
  * = Panoptic Segmentation
  * "stuff" 클래스\(uncountable\)에 대한 semantic segmentation 
  * "thing" 클래스\(countable\)에 대한 instance segmentation 
* 최근 접근법은 semantic 과 instance segmentation tasks 를 분리하여 수행하는 것이었음.
* 본 논문에서,  single-shot, bottom-up 접근법인 DeeperLab을 제안.
* DeeperLab은 semantic 과 instance segmentation tasks 를 합쳐서 다룸.
* 정량적 평가 지:
  * the instance-based Panoptic Quality \(PQ\) metric:
  * the region-based Parsing Covering \(PC\) metric:



Introduction

Related Work

3. Methodology

* encoder-decoder paradigm 채택.
* shared decoder output 로 부터 semantic segmentation 과 instance segmentation 을 생성하고, 최종 parsing image를 합성한다.

3.1. Encoder

3.2. Decoder

![](../.gitbook/assets/image%20%2856%29.png)



3.3. Image Parsing Prediction Heads

* The proposed network contains five prediction heads, each of which is directly attached to the shared decoder output and consists of two convolution layers with kernel sizes of 7×7 and 1×1 respectively. 
* One head \(with 256 filters for the first 7 × 7 layer\) is specific for semantic segmentation, while the other four \(each with 64 filters for the first 7 × 7 layer\) are used for class-agnostic instance segmentation.

3.3.1 Semantic Segmentation Head

* We only backpropagate the errors in the top-K positions \(hard example mining\).
* We set K = 0.15 · N, where N is the total number of pixels in the image.
* Moreover, we weigh the pixel loss based on instance sizes, putting more emphasis on small instances.
* Our proposed weighted bootstrapped cross-entropy loss is defined by:

![](../.gitbook/assets/image%20%2866%29.png)

* $$y_i$$ is the target class label for pixel $$i$$.
* $$p_{i,j}$$ is the predicted posterior probability for pixel $$i$$ and class $$j$$.
* 1\[x\] = 1 if x is true and 0 otherwise.
* The threshold$$t_K$$ is set in a way that only the pixels with top-K highest losses are selected.
* We set the weight $$w_i$$ = 3 for pixels that belong to instances with an area smaller than 64 × 64 and $$w_i$$ = 1 everywhere else.
* By doing so, the network is trained to focus on both hard pixels and small instances.







