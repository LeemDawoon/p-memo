---
description: 미니콘다를 이용한 python 개발 환경 설정
---

# Miniconda

**설치** 

* wget [https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86\_64.sh](https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh)
* sh Miniconda3-latest-Linux-x86\_64.sh -b -p $HOME/miniconda3
* rm -rf Miniconda3-latest-Linux-x86\_64.sh
* echo ". $HOME/miniconda3/etc/profile.d/conda.sh" &gt;&gt; ~/.bashrc

**conda 가상환경 실행**

* $ source ~/.bashrc

**가상환경 만들기**

* $ conda create -n dawoon python=3.6

**쥬피터에 가상환경 커널 추가하기**

* source activate dawoon
* pip install ipykernel
* python -m ipykernel install --user --name dawoon --display-name dawoon



* [https://github.com/inoryy/tensorflow-optimized-wheels](https://github.com/inoryy/tensorflow-optimized-wheels)

$ conda create -n dawoon2 python=3.7







