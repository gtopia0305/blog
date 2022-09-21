---
description: 슈퍼컴퓨팅인프라센터 2019. 5. 29. 17:56
---

# GROMACS-2018.6 (GPU 버전) 설치

KISTI 슈퍼컴퓨팅센터의 누리온 시스템에 gromacs-2018.6 Source 버전으로 설치 하는 방법에 대하여 소개 한다.

****

## **1. 설치 환경**

|   **구분**       | **내용**                                        |
| -------------- | --------------------------------------------- |
|  대상 시스템        | 뉴론 (GPU Cluster System)                       |
|  OS Version    | 리눅스 / CentOS 7.4                              |
|  CPU           | Intel Xeon E5-2670 v2                         |
|  GPU           | NVIDIA Tesla V100                             |
|  컴파일러          | Intel 2018.2 Version                          |
|  MPI           | mvapich2 2.3                                  |
| <p> 기타<br></p> | <p>Intel MKL Math Library</p><p>CUDA 10.0</p> |



## **2. 설치 전 환경 설정**

&#x20; 누리온 시스템은 PATH, LD\_LIBRARY\_PATH 등을 쉽게 하기 위하여 \
****  환경설정 툴인 Modules(http://modules.sourceforge.net)이 구성되어 있고,\
&#x20; 이하 설치 소개 에서는 module load를 이용한 환경 설정 방법을 이용한다.



\[ 환경 설정 ]

> &#x20;$ module load intel/18.0.2 cuda/10.0 cudampi/mvapich2-2.3 cmake/3.12.3

## **3. gromacs-2018.6 버전 설치 과정**

&#x20;설치 과정 소개는 tar 를 이용한 압축 해제 방법과 설정 방법등 진행 절차를 위주로 설명하고, 소스 파일 다운로드 등은 생략한다. &#x20;

|  **설치 과정**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p>$ export CC=mpicc </p><p>$ export CXX=mpicxx </p><p></p><p>$ tar xvzf gromacs-2018.6.tar.gz</p><p>$ cd gromacs-2018.6</p><p>$ <strong>mkdir build</strong></p><p>$ cd build</p><p>$ cmake -DGMX_OPENMP=ON <strong>-DGMX_GPU=ON</strong> -DGMX_MPI=ON  \</p><p>-DGMX_FFT_LIBRARY=mkl \</p><p>-DGMX_PREFER_STATIC_LIBS=ON -DCMAKE_BUILD_TYPE=Release \</p><p>-DCMAKE_INSTALL_PREFIX=<mark style="color:blue;"><strong>${HOME}/GROMACS/2018.6</strong></mark> \</p><p>-DGMX_HWLOC=OFF \</p><p>..</p><p> $ make</p><p> $ make install</p> |

※ <mark style="color:blue;">**"-DCMAKE\_INSTALL\_PREFIX=${HOME}/GROMACS/2018.6**</mark><mark style="color:blue;">"</mark>는 예시로 설치 희망하는 디렉토리로 명시한다.
