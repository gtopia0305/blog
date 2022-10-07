---
description: 슈퍼컴퓨팅인프라센터 2020. 6. 26. 13:44
---

# GROMACS-2020.2 버전 설치 소개 (KNL)

KISTI 슈퍼컴퓨팅센터의 누리온 시스템에 gromacs-2020.2 Source 버전으로 설치 하는 방법에 대하여 소개 한다.



## **1. 설치 환경**

|  **구분**      | **내용**                          |
| ------------ | ------------------------------- |
|  대상 시스템      |  누리온                            |
|  OS Version  |  리눅스 / CentOS 7.7               |
|  CPU         |  Intel(R) Xeon Phi(TM) CPU 7250 |
|  컴파일러        |  Intel 2019.5 Version           |
|  MPI         |  IntelMPI 2019.5 Version        |
|  기타          |  Intel MKL Math Library         |



## **2. 설치 전 환경 설정**

&#x20; 누리온 시스템은 PATH, LD\_LIBRARY\_PATH 등을 쉽게 하기 위하여 \
&#x20; 환경설정 툴인 Modules(http://modules.sourceforge.net)이 구성되어 있고,\
&#x20; 이하 설치 소개 에서는 module load를 이용한 환경 설정 방법을 이용한다.

{% code title="[ 환경 설정 ]" %}
```
 $ module load intel/19.0.5 impi/19.0.5 cmake/3.12.3
 $ export LD_LIBRARY_PATH=/apps/compiler/gcc/7.2.0/lib64:$LD_LIBRARY_PATH
```
{% endcode %}

## &#x20;**3. 설치 전 계산노드 접속**

Intel 컴파일러를 이용하여 KNL CPU 타입 전용 옵션인 **"-xMIC-AVX512"**를 사용하여 gromacs를 설치하게 될 경우 KNL 계산노드에서 설치를 진행해야 오류가 발생되지 않는다.

"-xMIC-AVX512" 옵션을 사용할 경우 PBS 스케줄러의 Interactive 기능을 이용하여 KNL 계산노드로 접속하여 빌드를 진행해야 한다.\
아래는 누리온 KNL 계산노드(debug 큐) 로 접속하는 예제이다.

```
$ cds
$ qsub -I -V -q debug -l select=1:ncpus=68:mpiprocs=68:ompthreads=1 -l walltime=12:00:00 -A gromacs
```

**※ qsub 명령 뒤 문자는 I(대문자 아이)  이고, select 와 walltime 앞에 문자는 l(소문자 엘) 이다.**

&#x20;

## **4. gromacs-2020.2 버전 설치 과정**

&#x20;설치 과정 소개는 tar 를 이용한 압축 해제 방법과 설정 방법등 진행 절차를 위주로 설명하고, 소스 파일 다운로드 등은 생략한다.

{% code title=" 설치 과정" %}
```
$ export FLAGS="-O2 -xMIC-AVX512 -g -static-intel"
$ export CFLAGS=$FLAGS 
$ export CXXFLAGS=$FLAGS 
$ export CC=mpiicc 
$ export CXX=mpiicpc 
$ export CPATH=/apps/compiler/intel/19.0.5/mkl/include:$CPATH


 $ tar xvzf gromacs-2020.2.tar.gz
 $ cd gromacs-2020.2
 $ mkdir build
 $ cd build


 $ cmake .. -DCMAKE_INSTALL_PREFIX=$HOME/GROMACS/2020.2 \
-DGMX_FFT_LIBRARY=mkl -DGMX_MPI=ON -DGMX_OPENMP=ON \
-DGMX_CYCLE_SUBCOUNTERS=ON -DGMX_GPU=OFF \
-DGMX_BUILD_HELP=OFF -DGMX_HWLOC=OFF \
-DGMX_SIMD=AVX_512_KNL -DGMX_OPENMP_MAX_THREADS=64 \
-DGMX_GPLUSPLUS_PATH=/apps/compiler/gcc/7.2.0/bin/g++


 $ make
 $ make install
```
{% endcode %}

※ <mark style="color:blue;">**"-DCMAKE\_INSTALL\_PREFIX=$HOME/GROMACS/2020.2**</mark><mark style="color:blue;">"</mark>는 예시로 설치 희망하는 디렉토리로 명시한다.

