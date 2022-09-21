---
description: 슈퍼컴퓨팅인프라센터 2021. 12. 9. 15:52
---

# GROMACS-2021.4 버전 설치 소개 (SKL)

KISTI 슈퍼컴퓨팅센터의 누리온 시스템의 SKL 사용 시 gromacs-2021.4 Source 버전으로 설치 하는 방법에 대하여 소개 한다.



## **1. 설치 환경**

| **구분**     | **내용**                     |
| ---------- | -------------------------- |
| 대상 시스템     | 누리온(SKL)                   |
| OS Version | 리눅스 / CentOS 7.7           |
| CPU        | Intel(R) Xeon(R) Gold 6148 |
| 컴파일러       | Intel 2021.4 Version       |
| MPI        | IntelMPI 2021.4 Version    |
| 기타         | Intel MKL Math Library     |

****

## **2. 설치 전 환경 설정**

누리온 시스템은 PATH, LD\_LIBRARY\_PATH 등을 쉽게 하기 위하여\
****환경설정 툴인 Modules([http://modules.sourceforge.net)](http://modules.sourceforge.net\)/)이 구성되어 있고,\
이하 설치 소개 에서는 module load를 이용한 환경 설정 방법을 이용한다.

\[ 환경 설정 ]

```
$ module intel/oneapi_21.2 impi/oneapi_21.2 cmake/3.12.7
```

****

## **3. gromacs-2021.4 버전 설치 과정**

설치 과정 소개는 tar 를 이용한 압축 해제 방법과 설정 방법 등 진행 절차를 위주로 설명하고,\
****소스 파일 다운로드 등은 생략한다.

| **설치 과정**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p>$ export FLAGS="-O2 -g -static-intel"<br>$ export CFLAGS=$FLAGS<br>$ export CXXFLAGS=$FLAGS<br>$ export CC=mpiicc<br>$ export CXX=mpiicpc<br><br><br>$ tar -xzvf gromacs-2021.4.tar.gz<br>$ cd gromacs-2021.4/<br>$ mkdir build<br>$ cd build<br><br><br>$ cmake .. -DCMAKE_INSTALL_PREFIX=<mark style="color:blue;"><strong>=$HOME/GROMACS/2021.4</strong></mark> \</p><p>- DGMX_BUILD_OWN_FFTW=ON -DGMX_MPI=ON -DGMX_OPENMP=ON \</p><p>- DGMX_CYCLE_SUBCOUNTERS=ON -DGMX_GPU=OFF &#x3C;br>-DGMX_HWLOC=OFF \</p><p>- DGMX_SIMD=AVX_512 \</p><p>- DGMX_OPENMP_MAX_THREADS=32<br><br><br>$ make<br>$ make install</p> |

※ <mark style="color:blue;">**"-DCMAKE\_INSTALL\_PREFIX=$HOME/GROMACS/2021.4**</mark><mark style="color:blue;">"</mark>는 예시로 설치 희망하는 디렉토리로 명시한다.
