---
description: 슈퍼컴퓨팅인프라센터 2022. 8. 1. 09:23
---

# RASPA2-2.0.47 설치 소개

KISTI 누리온/뉴론 시스템에 RASPA2-2.0.47 버전을 설치하는 방법에 대해 소개한다.\
설치 과정 소개는 tar 파일을 이용한 압축 해제 방법과 설정 방법 등의 진행 절차를 위주로 설명하고, 소스 파일 다운로드 등은 생략한다.

## **1. 설치 환경**

|   **구분**    | **내용**                                                      |
| ----------- | ----------------------------------------------------------- |
|  대상 시스템     | 누리온                                                         |
|  OS Version | 리눅스 / CentOS 7.7                                            |
|  CPU        | Intel(R) Xeon Phi(TM) CPU 7250 / Intel(R) Xeon(R) Gold 6148 |
|  컴파일러       | Intel 2018.3 Version                                        |
|  MPI        | Intel MPI 2018.3 Version                                    |
|  기타         | Intel Math Kernel Library                                   |



## **2. 설치 전 환경 설정**

누리온/뉴론 시스템은 PATH, LD\_LIBRARY\_PATH 등을 손쉽게 설정하기 위하여, OpenSource 프로그램인 Environment Modules(https://modules.sourceforge.net)이 구성되어 있고, 이하 설치 소개에서는 module load를 이용한 환경설정 방법을 활용한다.

```
$ module purge
$ module load intel/18.0.3 impi/18.0.3
```

## **3. RASPA 2.0 설치 과정**

설치 과정에서 설치 경로는 **${HOME}/RASPA2/2.0.47** 을 사용하였다. 이 경로는 사용자에게 맞는 경로로 변경하여야 한다.

```
$ tar -xvzf v2.0.47.tar.gz
$ cd RASPA2-2.0.47

$ mkdir -p m4
$ aclocal
$ autoreconf -i
$ automake --add-missing

$ autoconf

$ ./configure --prefix=${HOME}/RASPA2/2.0.47 \
CPPFLAGS="-I/apps/compiler/intel/18.0.3/mkl/include/fftw" \
LDFLAGS="-L/apps/compiler/intel/18.0.3/mkl/lib/intel64" \
LIBS="-mkl=cluster" CC=mpiicc CXX=mpiicpc F77=mpiifort \
CFLAGS="-O3 -fPIC -m64 -xCOMMON-AVX512 -std=c99" \
CXXFLAGS="-O3 -fPIC -m64 -xCOMMON-AVX512" \
FFLAGS="-O3 -fPIC -m64 -xCOMMON-AVX512"

$ make 
$ make install
```
