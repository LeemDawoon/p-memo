---
description: 'Noise2Noise: Learning Image Restoration without Clean Data'
---

# \_\(2018\) Noise2Noise

* 논문링크: [https://arxiv.org/pdf/1803.04189v3.pdf](https://arxiv.org/pdf/1803.04189v3.pdf)
* 참조 블로그 및 페이지:
  * [https://nuguziii.github.io/paper-review/PR-002](https://nuguziii.github.io/paper-review/PR-002/)
  * [https://github.com/joeylitalien/noise2noise-pytorch](https://github.com/joeylitalien/noise2noise-pytorch)





```text
python3 train.py \
  --train-dir ../data/train --train-size 1000 \
  --valid-dir ../data/valid --valid-size 200 \
  --ckpt-save-path ../ckpts \
  --nb-epochs 10 \
  --batch-size 4 \
  --loss l2 \
  --noise-type gaussian \
  --noise-param 50 \
  --crop-size 64 \
  --plot-stats \
  --cuda
```

