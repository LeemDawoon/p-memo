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

![](../.gitbook/assets/image%20%28104%29.png)



Introduction

Related Work

3. Methodology

![](../.gitbook/assets/image%20%2816%29.png)

* encoder-decoder paradigm 채택.
* shared decoder output 로 부터 semantic segmentation 과 instance segmentation 을 생성하고, 최종 parsing image를 합성한다.

3.1. Encoder

* Xception-71
* Wider variant of MobileNetV2

3.2. Decoder

![](../.gitbook/assets/image%20%2858%29.png)



3.3. Image Parsing Prediction Heads

* The proposed network contains five prediction heads, each of which is directly attached to the shared decoder output and consists of two convolution layers with kernel sizes of 7×7 and 1×1 respectively. 
* One head \(with 256 filters for the first 7 × 7 layer\) is specific for semantic segmentation, while the other four \(each with 64 filters for the first 7 × 7 layer\) are used for class-agnostic instance segmentation.

3.3.1 Semantic Segmentation Head

* We only backpropagate the errors in the top-K positions \(hard example mining\).
* We set K = 0.15 · N, where N is the total number of pixels in the image.
* Moreover, we weigh the pixel loss based on instance sizes, putting more emphasis on small instances.
* Our proposed weighted bootstrapped cross-entropy loss is defined by:

![](../.gitbook/assets/image%20%2868%29.png)

* $$y_i$$ is the target class label for pixel $$i$$.
* $$p_{i,j}$$ is the predicted posterior probability for pixel $$i$$ and class $$j$$.
* 1\[x\] = 1 if x is true and 0 otherwise.
* The threshold$$t_K$$ is set in a way that only the pixels with top-K highest losses are selected.
* We set the weight $$w_i$$ = 3 for pixels that belong to instances with an area smaller than 64 × 64 and $$w_i$$ = 1 everywhere else.
* By doing so, the network is trained to focus on both hard pixels and small instances.

3.3.2 Instance Segmentation Head

* keypoint-based representation for object instances 채택.
* we consider the four bounding box corners and the center of mass as our P = 5 object keypoints.

![](../.gitbook/assets/image%20%28144%29.png)

* **The keypoint heatmap \(Fig. 4a\)**
  * predicts whether a pixel is within a disk of radius R pixels centered in the corresponding keypoint.
  * The target activation is equal to 1 in the interior of the disks and 0 elsewhere**.**
  * We use the same disk radius R = 25 regardless of the size of an instance, so that the network pays the same attention to both large and small instances.
  * The predicted keypoint heatmap contains P channels, one for each keypoint. 
  * We penalize prediction errors by the standard sigmoid cross entropy loss.
* **The long-range offset map \(Fig. 4b\)** 
  * predicts the position offset from a pixel to all the corresponding keypoints, encoding the long-range information for each pixel. 
  * The predicted long-range offset map has 2P channels, where every two channels predict the offset in the horizontal and vertical directions for each keypoint. 
  * We employ L1 loss for long-range offset prediction, which is only activated at pixels belonging to object instances.
* **The short-range offset map \(Fig. 4c\)** 
  * is similar to the long-range offset map except that it only focuses on pixels within the disk of radius R = 25 pixels around the keypoints, i.e., the pixels having a value of one in the target heatmap \(the green disk in Fig. 4a\). The short range offset map also has 2P channels and are used to improve keypoint localization. We employ L1 loss, which is only activated at the interior of the disks.
* **The middle-range offset map \(Fig. 4d\)** 
  * predicts the offset among keypoint pairs, defined in a **directed keypoint relation graph \(DKRG\).** 
  * This map is used to group keypoints that belong to the same instance \(i.e., instance detection via keypoints\). 
  * As shown in Fig. 4d, we adopt the star graph \[99\], where the mass center is bi-directionally connected to the other four box corners. 
  * The predicted middle-range offset map has 2E channels, where E is the number of directed edges in the DKRG \(E = 8 in the star graph\). 
  * Similarly, we use two channels for each edge to predict the horizontal and vertical offsets and employ L1 loss during training, which is only activated at the interior of the disks.

3.4. Prediction Fusion

* merge the four predictions \(keypoint heatmap, long-range, short-range, and middle-range offset maps\) into a single class-agnostic instance segmentation map

3.4.1 Instance Prediction

* We generate the instance segmentation map from the four instance-related prediction maps
* **Recursive offset refinement:** 
  * We observe that the predictions that are closer to the corresponding keypoints are more accurate. 
  * Therefore, we recursively refine the offset maps by itself and/or each other as in **PersonLab \[85\].**
* **Keypoint localization:** 
  * For each keypoint, we perform **Hough-voting** on _the short-range offset map_ and use the corresponding value in the keypoint heatmap \(after sigmoid activation\) as the voting weight to generate the short-range score map. 
  * We propose to also perform Hough-voting on _the long-range offset map_ \(using a weight equal to one for every vote\) to generate the long-range score map. 
  * These two score maps are merged into one by taking per-pixel weighted sum. 
  * We then localize the keypoints by finding the local maxima in the resultant fused score map. 
  * Finally, we use the **Expected-OKS score \[85\]** to rescore all keypoints.
* **Instance detection:** 
  * We cluster the keypoints to detect instances by using a fast greedy algorithm. 
  * All the keypoints are first pushed into a priority queue and popped one at a time. 
  * If the popped keypoint is in the proximity of the corresponding keypoint of an already detected instance, we reject it and continue the process. 
  * Otherwise, we follow the predicted _middle-range offsets_ to identify the positions of the remaining four keypoints, thus forming a newly detected instance. 
  * The confidence score of the detected instance is defined as the average of its keypoint scores. 
  * After all the instances are detected, we use bounding box non-maximum suppression to remove overlapping instances.
* Assignment of pixels to instances: 
  * Finally, given the detected instances, we assign an instance label to each pixel by using the predicted _long-range offset map_, which encodes the pixel-wise offset to the keypoints. 
  * Specifically, we assign each pixel to the detected instance whose keypoints have the smallest L2-distance to the pixel’s predicted keypoints \(i.e., its image location plus its predicted long**-**range offset\).

3.4.2 Semantic and Instance Prediction Fusion

* We opt for a simple merging method without any other post-processing, such as removal of small isolated regions in the segmentation maps. 
* In particular, we start from the predicted semantic segmentation by considering ‘stuff’ \(e.g., sky\) and ‘thing’ \(e.g., person\) classes separately. 
* Pixels predicted to have a ‘stuff‘ class are assigned with a single unique instance label. 
* For the other pixels, their instance labels are determined from the instance segmentation result while their semantic labels are resolved by the majority vote of the corresponding predicted semantic labels.

3.5. Evaluation Metrics

* Panoptic Quality \(PQ\)
  * ![](../.gitbook/assets/image%20%2817%29.png)
  * R and R' are groundtruth regions and predicted regions respectively, and \|T P\|, \|F P\|, and \|F N\| are the number of true positives, false postives, and false negatives. 
  * The matching is determined by a threshold of 0.5 IntersectionOver-Union \(IOU\).
  * 인스턴스의 크기르 고려하지 않는 점이 문제.
* Parsing Covering \(PC\)
  * ![](../.gitbook/assets/image%20%2888%29.png)
  * Si and S'i are the groundtruth segmentation and predicted segmentation for the i-th semantic class respectively, 
  * Ni is the total number of pixels of groundtruth regions from Si . 
  * The Covering for class i, Covi , is computed in the same way as the original Covering metric except that only groundtruth regions from Si and predicted regions from S'i are considered. 
  * PC is then obtained by computing the average of Covi over C semantic classes.











