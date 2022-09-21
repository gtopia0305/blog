---
description: 슈퍼컴퓨팅인프라센터 2019. 4. 30. 09:36
---

# Conda 의 활용 소개

아나콘다(Anaconda)는 PYTHON 과 R 프로그래밍 언어로 된 과학 컴퓨팅(데이터 과학, 기계 학습 응용 프로그램, 대규모 데이터 처리, 예측 분석 등)분야의 패키지들의 모음을 제공하는 배포판이다.\
Anaconda 배포판은 1,200 만 명이 넘는 사용자가 사용하며 Windows, Linux 및 MacOS에 적합한 1400 가지 이상의 인기있는 데이터 과학 패키지를 포함한다.\
Anaconda를 설치하기 위해서는 [https://www.anaconda.com](https://www.anaconda.com/) 웹사이트에서 자신의 OS에 맞는 배포판을 다운받아 설치하면 된다.\
(예) Windows, MacOS, Linux\
현재 Anaconda 는 Python 3.7 기반의 버전과 Python 2.7 기반의 버전을 제공한다.

conda 는 아나콘다에서 패키지 버전 관리를 위해 제공되는 어플리케이션이다.\
Python 사용자들이 패키지 설치 시 가장 어려움을 겪는 의존성 문제를 conda 를 활용함으로써 쉽게 해결할 수 있다.

본 문서는 KISTI 시스템에서 Python 사용자를 위하여 conda 패키지 활용하는 방법을 소개 한다.\
소개 페이지의 "/home01/optpar02" 는 테스트 계정 optpar02 의 홈디렉토리로 자신에 맞는 경로로 적절히 변경해서 사용해야 한다.

****

## **1. Conda 의 사용**

&#x20;\- Miniconda는 https://docs.conda.io/en/latest/miniconda.html 사이트 에서 각 OS 에 맞는 버전을 다운 받을 수 있고, Anaconda 는 [https://www.anaconda.com/distribution/#download-section](https://www.anaconda.com/distribution/#download-section) 사이트 에서 각 OS 에 맞는 버전을 다운 받을 수 있다.

| **명령어 모음** | **내용**                                                                                                                                                                           |
| ---------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|  clean     |  Remove unused packages and caches.                                                                                                                                              |
| config     | <p> Modify configuration values in .condarc.  This is modeled after the git config command. </p><p> Writes to the user .condarc file (/home01/optpar02/.condarc) by default.</p> |
|  create    |  Create a new conda environment from a list of specified packages.                                                                                                               |
|  help      |  Displays a list of available conda commands and their help strings.                                                                                                             |
|  info      |  Display information about current conda install.                                                                                                                                |
|  init      |  Initialize conda for shell interaction. \[Experimental]                                                                                                                         |
|  install   |  Installs a list of packages into a specified conda environment.                                                                                                                 |
|  list      |  List linked packages in a conda environment.                                                                                                                                    |
|  package   |  Low-level conda package utility. (EXPERIMENTAL)                                                                                                                                 |
|  remove    |  Remove a list of packages from a specified conda environment.                                                                                                                   |
|  uninstall |  Alias for conda remove.                                                                                                                                                         |
|  run       |  Run an executable in a conda environment. \[Experimental]                                                                                                                       |
|  search    | <p> Search for packages and display associated information. <br> The input is a MatchSpec, a query language for conda packages.</p><p> See examples below.</p>                   |
|  update    |  Updates conda packages to the latest compatible version.                                                                                                                        |
|  upgrade   |  Alias for conda update                                                                                                                                                          |



## **2. Conda Environment 생성**

\- conda environment 는 Python 의 독립적인 가상 실행환경을 만들어 패키지들의 버전 관리에 용이 하다.

\- "conda create -n \[ENVIRONMENT]" 을 이용하여 conda environment를 생성 할 수 있다.

\- 기본 값으로 conda path 의 envs 아래 경로에 지정한 environment 이름으로 생성된다.

\- "--use-local" 옵션을 사용하면 사용자 홈 디렉토리(**${HOME}/.conda/envs/\[environment\_name]**)에 생성 된다.



&#x20;\- 예제 -

> \[optpar02@login02 \~]$ **module load python/3.7.1**
>
> \[optpar02@login02 \~]$ **conda create -n scikit-learn\_0.21 --use-local**
>
> Collecting package metadata: done
>
> Solving environment: done
>
>
>
> \## Package Plan ##
>
>
>
> &#x20; environment location: /home01/optpar02/.conda/envs/scikit-learn\_0.21
>
>
>
> Proceed (\[y]/n)? y
>
>
>
> Preparing transaction: done
>
> Verifying transaction: done
>
> Executing transaction: done
>
> \#
>
> \# To activate this environment, use:
>
> \# > conda activate scikit-learn\_0.21
>
> \#
>
> \# To deactivate an active environment, use:
>
> \# > conda deactivate
>
> \#
>
>
>
> \[optpar02@login02 \~]$ **source activate scikit-learn\_0.21**
>
> (scikit-learn\_0.21) \[optpar02@login02 \~]$ ****&#x20;

## **3. Conda Environment에 패키지 설치 및 확인**

\- conda install \[패키지명] 으로 패키지를 설치할 수 있다.

\- conda 채널에 있는 패키지는 "conda install -c \[채널명] \[패키지명]" 와 같이 설치 할 수 있다.

\- 위 "2" 항목에서 생성한 conda environment 경로 아래에 패키지 들이 설치 된다.



&#x20;\- 예제 -

> \[optpar02@login02 \~]$ **module load python/3.7.1**
>
> \[optpar02@login02 \~]$ **source activate scikit-learn\_0.21**
>
> (scikit-learn\_0.21) \[optpar02@login02 \~]$ **conda install scikit-learn**
>
> Collecting package metadata: done
>
> Solving environment: done
>
>
>
> \## Package Plan ##
>
>
>
> &#x20; environment location: /home01/optpar02/.conda/envs/scikit-learn\_0.21
>
>
>
> &#x20; added / updated specs:
>
> &#x20;   \- scikit-learn
>
>
>
> The following packages will be downloaded:
>
>
>
> &#x20;   package                    |            build
>
> &#x20;   \---------------------------|-----------------
>
> &#x20;   ca-certificates-2019.1.23  |                0         126 KB
>
> &#x20;   certifi-2019.3.9           |           py37\_0         155 KB
>
> &#x20;   intel-openmp-2019.3        |              199         886 KB
>
> &#x20;   libedit-3.1.20181209       |       hc058e9b\_0         188 KB
>
> &#x20;   mkl-2019.3                 |              199       203.3 MB
>
> &#x20;   mkl\_fft-1.0.10             |   py37ha843d7b\_0         169 KB
>
> &#x20;   numpy-1.16.2               |   py37h7e9f1db\_0          49 KB
>
> &#x20;   numpy-base-1.16.2          |   py37hde5b4d6\_0         4.3 MB
>
> &#x20;   openssl-1.1.1b             |       h7b6447c\_1         4.0 MB
>
> &#x20;   pip-19.0.3                 |           py37\_0         1.8 MB
>
> &#x20;   python-3.7.3               |       h0371630\_0        36.7 MB
>
> &#x20;   scikit-learn-0.20.3        |   py37hd81dba3\_0         5.8 MB
>
> &#x20;   scipy-1.2.1                |   py37h7c811a0\_0        17.7 MB
>
> &#x20;   setuptools-40.8.0          |           py37\_0         643 KB
>
> &#x20;   sqlite-3.27.2              |       h7b6447c\_0         1.9 MB
>
> &#x20;   wheel-0.33.1               |           py37\_0          39 KB
>
> &#x20;   \------------------------------------------------------------
>
> &#x20;                                          Total:       277.6 MB
>
>
>
> The following NEW packages will be INSTALLED:
>
>
>
> &#x20; blas               pkgs/main/linux-64::blas-1.0-mkl
>
> &#x20; ca-certificates    pkgs/main/linux-64::ca-certificates-2019.1.23-0
>
> &#x20; certifi            pkgs/main/linux-64::certifi-2019.3.9-py37\_0
>
> &#x20; intel-openmp       pkgs/main/linux-64::intel-openmp-2019.3-199
>
> &#x20; libedit            pkgs/main/linux-64::libedit-3.1.20181209-hc058e9b\_0
>
> &#x20; libffi             pkgs/main/linux-64::libffi-3.2.1-hd88cf55\_4
>
> &#x20; libgcc-ng          pkgs/main/linux-64::libgcc-ng-8.2.0-hdf63c60\_1
>
> &#x20; libgfortran-ng     pkgs/main/linux-64::libgfortran-ng-7.3.0-hdf63c60\_0
>
> &#x20; libstdcxx-ng       pkgs/main/linux-64::libstdcxx-ng-8.2.0-hdf63c60\_1
>
> &#x20; mkl                pkgs/main/linux-64::mkl-2019.3-199
>
> &#x20; mkl\_fft            pkgs/main/linux-64::mkl\_fft-1.0.10-py37ha843d7b\_0
>
> &#x20; mkl\_random         pkgs/main/linux-64::mkl\_random-1.0.2-py37hd81dba3\_0
>
> &#x20; ncurses            pkgs/main/linux-64::ncurses-6.1-he6710b0\_1
>
> &#x20; numpy              pkgs/main/linux-64::numpy-1.16.2-py37h7e9f1db\_0
>
> &#x20; numpy-base         pkgs/main/linux-64::numpy-base-1.16.2-py37hde5b4d6\_0
>
> &#x20; openssl            pkgs/main/linux-64::openssl-1.1.1b-h7b6447c\_1
>
> &#x20; pip                pkgs/main/linux-64::pip-19.0.3-py37\_0
>
> &#x20; python             pkgs/main/linux-64::python-3.7.3-h0371630\_0
>
> &#x20; readline           pkgs/main/linux-64::readline-7.0-h7b6447c\_5
>
> &#x20; scikit-learn       pkgs/main/linux-64::scikit-learn-0.20.3-py37hd81dba3\_0
>
> &#x20; scipy              pkgs/main/linux-64::scipy-1.2.1-py37h7c811a0\_0
>
> &#x20; setuptools         pkgs/main/linux-64::setuptools-40.8.0-py37\_0
>
> &#x20; sqlite             pkgs/main/linux-64::sqlite-3.27.2-h7b6447c\_0
>
> &#x20; tk                 pkgs/main/linux-64::tk-8.6.8-hbc83047\_0
>
> &#x20; wheel              pkgs/main/linux-64::wheel-0.33.1-py37\_0
>
> &#x20; xz                 pkgs/main/linux-64::xz-5.2.4-h14c3975\_4
>
> &#x20; zlib               pkgs/main/linux-64::zlib-1.2.11-h7b6447c\_3
>
>
>
> Proceed (\[y]/n)? y
>
>
>
> Downloading and Extracting Packages
>
> setuptools-40.8.0    | 643 KB    | ##################################### | 100%&#x20;
>
> mkl\_fft-1.0.10       | 169 KB    | ##################################### | 100%&#x20;
>
> pip-19.0.3           | 1.8 MB    | ##################################### | 100%&#x20;
>
> mkl-2019.3           | 203.3 MB  | ##################################### | 100%&#x20;
>
> ca-certificates-2019 | 126 KB    | ##################################### | 100%&#x20;
>
> sqlite-3.27.2        | 1.9 MB    | ##################################### | 100%&#x20;
>
> wheel-0.33.1         | 39 KB     | ##################################### | 100%&#x20;
>
> python-3.7.3         | 36.7 MB   | ##################################### | 100%&#x20;
>
> intel-openmp-2019.3  | 886 KB    | ##################################### | 100%&#x20;
>
> libedit-3.1.20181209 | 188 KB    | ##################################### | 100%&#x20;
>
> numpy-base-1.16.2    | 4.3 MB    | ##################################### | 100%&#x20;
>
> scipy-1.2.1          | 17.7 MB   | ##################################### | 100%&#x20;
>
> scikit-learn-0.20.3  | 5.8 MB    | ##################################### | 100%&#x20;
>
> numpy-1.16.2         | 49 KB     | ##################################### | 100%&#x20;
>
> certifi-2019.3.9     | 155 KB    | ##################################### | 100%&#x20;
>
> openssl-1.1.1b       | 4.0 MB    | ##################################### | 100%&#x20;
>
> Preparing transaction: done
>
> Verifying transaction: done
>
> Executing transaction: done
>
> (scikit-learn\_0.21) \[optpar02@login02 \~]$ **python -c "import sklearn"**
>
> (scikit-learn\_0.21) \[optpar02@login02 \~]$



**4. Conda Environment 목록 확인**

\- "conda-env list" 또는 "conda env list" 를 이용하여 목록을 확인 할 수 있다.



\[예제]

> (scikit-learn\_0.21) \[optpar02@login02 \~]$ **conda env list**
>
> \# conda environments:
>
> \#
>
> base                     /apps/applications/PYTHON/3.7
>
> **scikit-learn\_0.21     \*  /home01/optpar02/.conda/envs/scikit-learn\_0.21**
>
>
>
> \[optpar02@login02 \~]$



**5. Conda Environment 삭제**

\- "conda-env remove -n \[ENVIRONMENT]" 또는 "conda env remove -n \[ENVIRONMENT]" 를 이용하여 삭제 할 수 있다.



\[예제]



> \[optpar02@login02 \~]$ **module load python/3.7.1**
>
> \[optpar02@login02 \~]$ **conda env remove -n scikit-learn\_0.21**
>
> \
> Remove all packages in environment /home01/optpar02/.conda/envs/scikit-learn\_0.21:
>
>
>
> \[optpar02@login02 \~]$ **conda env list**
>
> \# conda environments:
>
> \#
>
> base                  \*  /apps/applications/PYTHON/3.7
>
>
>
> \[optpar02@login02 \~]$



**6. Conda Environment 내보내기**

\- 내보내기 전 conda-pack 패키지 필요

&#x20; (참고) [https://conda.github.io/conda-pack](https://conda.github.io/conda-pack)

\- "conda pack -n \[ENVIRONMENT] -o \[파일명]" 을 이용하여 conda environment 를 다른 시스템에서 활용할 수 있다.

&#x20; (예) 외부 인터넷이 연결되지 않는 경우, 다른 시스템에서 동일한 conda 환경을 이용하는 경우



\[예제]

> \[optpar02@login02 \~]$ **module load python/3.7.1**
>
> \[optpar02@login02 \~]$ **source activate tensorflow\_1.12**
>
> (tensorflow\_1.12) \[optpar02@login02 \~]$ **conda install -c conda-forge -n tensorflow\_1.12**
>
> (tensorflow\_1.12) \[optpar02@login02 \~]$ **conda pack -n tensorflow\_1.12 -o conda\_tensorflow\_1.12.tar.gz**
>
> Collecting packages...
>
> Packing environment at '/home01/optpar02/.conda/envs/tensorflow\_1.12' to 'conda\_tensorflow\_1.12.tar.gz'
>
> \[########################################] | 100% Completed |  4min 18.8s
>
> (tensorflow\_1.12) \[optpar02@login02 \~]$ ls -l conda\_tensorflow\_1.12.tar.gz
>
> \-rw-------. 1 optpar02 in0162 1459826406 Mar 28 15:03 **conda\_tensorflow\_1.12.tar.gz**
>
> (tensorflow\_1.12) \[optpar02@login02 \~]$



**7. Conda Environment 가져오기**

\- conda pack 을 이용하여 생성했던 conda environment 를 아래 \[예제]와 같이 가져와 환경설정 후 사용 가능.



\[예제]

> \[optpar02@login02 \~]$ **module load python/3.7.1**
>
> \[optpar02@login02 \~]$ **conda env list**
>
> \# conda environments:
>
> \#
>
> base                  \*  /apps/applications/PYTHON/3.7
>
>
>
> \[optpar02@login02 \~]$ **mkdir -p $HOME/.conda/envs/tensorflow\_1.12**
>
> \[optpar02@login02 \~]$ **tar xvzf conda\_tensorflow\_1.12.tar.gz -C $HOME/.conda/envs/tensorflow\_1.12**
>
> \[optpar02@login02 \~]$ **cd $HOME/.conda/envs/tensorflow\_1.12/bin**
>
> \[optpar02@login02 \~]$ **./conda-unpack**
>
> \[optpar02@login02 \~]$ **conda env list**
>
> \# conda environments:
>
> \#
>
> base                  \*  /apps/applications/PYTHON/3.7
>
> tensorflow\_1.12          /home01/optpar02/.conda/envs/tensorflow\_1.12
>
>
>
> \[optpar02@login02 \~]$ **source activate tensorflow\_1.12**

\
