# 환경설정

설치 

* wget [https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86\_64.sh](https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh)
* sh Miniconda3-latest-Linux-x86\_64.sh -b -p $HOME/miniconda3
* rm -rf Miniconda3-latest-Linux-x86\_64.sh
* echo ". $HOME/miniconda3/etc/profile.d/conda.sh" &gt;&gt; ~/.bashrc

conda 가상환경 실행

* $ source ~/.bashrc

가상환경 만들기

* $ conda create -n dawoon python=3.6

