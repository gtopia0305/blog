---
description: 슈퍼컴퓨팅인프라센터 2018. 12. 3. 14:32
---

# GROMACS-5.1.4 설치

KISTI 슈퍼컴퓨팅센터의 누리온 시스템에 gromacs-5.1.4 Source 버전으로 설치 하는 방법에 대하여 소개 한다.



## **1. 설치 환경**

|  **구분**     | **내용**                      |
| ----------- | --------------------------- |
|  대상 시스템     |  누리온                        |
| OS Version  |  리눅스 / CentOS 7.3           |
|  CPU        |  Intel(R) Xeon(R) Gold 6148 |
|  컴파일러       |  Intel 2018.3 Version       |
|  MPI        |  IntelMPI 2018.3 Version    |
|  기타         |  Intel MKL Math Library     |



## **2. 설치 전 환경 설정**

&#x20; 누리온 시스템은 PATH, LD\_LIBRARY\_PATH 등을 쉽게 하기 위하여 \
&#x20; 환경설정 툴인 Modules(http://modules.sourceforge.net)이 구성되어 있고,\
&#x20; 이하 설치 소개 에서는 module load를 이용한 환경 설정 방법을 이용한다.



\[ 환경 설정 ]

> &#x20;$ module load intel/18.0.3 impi/18.0.3



## **3. gromacs-5.1.4 버전 설치 과정**

&#x20;설치 과정 소개는 tar 를 이용한 압축 해제 방법과 설정 방법등 진행 절차를 위주로 설명하고, 소스 파일 다운로드 등은 생략한다. &#x20;

|  **설치 과정**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p> $ export CC=mpiicc  </p><p> $ export CXX=mpiicpc</p><p> $ export CPATH=/apps/compiler/intel/18.0.3/mkl/include:$CPATH</p><p></p><p> $ tar xvzf gromacs-5.1.4.tar.gz</p><p> $ cd gromacs-5.1.4</p><p> $ mkdir build</p><p> $ cd build</p><p> $ cmake -DBUILD_SHARED_LIBS=OFF -DGMX_FFT_LIBRARY=mkl \</p><p> -DCMAKE_INSTALL_PREFIX=<mark style="color:blue;"><strong>${HOME}/GROMACS/5.1.4</strong></mark> \</p><p> -DGMX_MPI=ON -DGMX_OPENMP=ON -DGMX_CYCLE_SUBCOUNTERS=ON \</p><p> -DGMX_GPU=OFF -DGMX_BUILD_HELP=OFF -DGMX_HWLOC=OFF \</p><p> ..</p><p> $ make</p><p> $ make install</p> |

<mark style="color:blue;">※ "-DCMAKE\_INSTALL\_PREFIX=${HOME}/GROMACS/5.1.4"는 예시로 설치 희망하는 디렉토리로 명시한다.</mark>
