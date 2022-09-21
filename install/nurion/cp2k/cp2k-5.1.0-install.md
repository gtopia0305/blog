---
description: 슈퍼컴퓨팅인프라센터 2019. 7. 23. 14:51
---

# CP2K-5.1.0 버전 설치 소개

KISTI 슈퍼컴퓨팅센터의 누리온 시스템에 CP2K-v5.1.0 Source 버전으로 공용 파일시스템인 /apps에 설치했던 내용을 정리하여 사용자들이 설치하는 방법에 대하여 참고할 수 있도록 내용을 소개 한다.



## **1. 설치 환경**&#x20;

|  **구분**     | **내용**                          |
| ----------- | ------------------------------- |
|  대상 시스템     |  누리온                            |
| OS Version  |  리눅스 / CentOS 7.3               |
|  CPU        |  Intel(R) Xeon Phi(TM) CPU 7250 |
|  컴파일러       |  Gcc 6.1.0                      |
|  MPI        |  OpenMPI 3.1.0                  |
|  기타         |  Cmake 3.12.3                   |



## **2. 설치 전 환경 설정**

&#x20;CP2K 설치에는 여러 의존성 패키지들이 필요하다. 본 내용은 컴파일러와 MPI, CMAKE를 제외한 의존 라이브러리들을 CP2K 의 toolchain 을 이용하는 방법으로 소개한다.

&#x20;\[ 환경 설정 ]

> &#x20;$ module load gcc/6.1.0 openmpi/3.1.0 cmake/3.12.3

## **3. CP2K-v5.1.0 버전 설치 과정**

&#x20;설치 과정 소개는 진행 절차를 위주로 설명하고, 소스 파일 다운로드와 tar 를 이용한 압축 해제 방법과 설정 방법등은 생략한다. &#x20;

설치 소개 시 사용된 경로 <mark style="color:blue;">/$/CP2K-5.1.0/</mark> 는 설치 안내를 위한 경로이므로, 사용자는 실제 사용할 경로를 지정하여 설치하면 된다. &#x20;

|  **설치 과정**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p> $ cd <mark style="color:blue;">/$/CP2K-5.1.0/</mark><br> $ cd tools/toolchain<br> $ vi install_cp2k_toolchain.sh<br>  <em><mark style="color:orange;"><strong>- - - [install_cp2k_toolchain.sh 수정 사항] 참고 - - -</strong></mark></em><br> $ vi scripts/install_ptscotch.sh <br>  <em><mark style="color:orange;"><strong>- - - [install_ptscotch.sh 수정 사항] 참고 - - -</strong></mark></em><br> $ vi scripts/install_pexsi.sh<br>  <em><strong>  </strong><mark style="color:orange;"><strong>- - - [install_pexsi.sh 수정 사항] 참고 - - -</strong></mark></em><br> $ ./install_cp2k_toolchain.sh -j 2 --install-all \<br>--mpi-mode=openmpi --math-mode=openblas --enable-omp \<br>--with-binutils=system --with-openblas --with-gcc=system \<br>--with-openmpi=system --with-cmake=system<br> $ cp install/arch/* ../../arch<br> $ cd ../../makefiles<br> $ vi ../arch/local.popt<br> <em><strong>  </strong><mark style="color:orange;"><strong>- - - [local.popt 수정 사항] 참고 - - -</strong></mark></em><br> $ vi ../arch/local.psmp<br> <em><strong>  </strong><mark style="color:orange;"><strong>- - - [local.psmp 수정 사항] 참고 - - -</strong></mark></em><br>  $ make -j 2 ARCH=local VERSION="popt psmp"</p> |

※ popt : MPI 버전

※ psmp : MPI+OpenMP의 Hybrid 버전

※ 참고 :&#x20;

[https://www.cp2k.org/howto:compile](https://www.cp2k.org/howto:compile)



**\[install\_cp2k\_toolchain.sh 수정 사항]**&#x20;

> &#x20;760라인 : CFLAGS="-O2 -ftree-vectorize -g -fno-omit-frame-pointer <mark style="color:blue;">-mfma -mavx512f -mavx512cd</mark> -ffast-math $TSANFLAGS"\
> &#x20;761라인 : FFLAGS="-O2 -ftree-vectorize -g -fno-omit-frame-pointer <mark style="color:blue;">-mfma -mavx512f -mavx512cd</mark> -ffast-math $TSANFLAGS"\
> &#x20;762라인 : F77FLAGS="-O2 -ftree-vectorize -g -fno-omit-frame-pointer <mark style="color:blue;">-mfma -mavx512f -mavx512cd</mark> -ffast-math $TSANFLAGS"\
> &#x20;763라인 : F90FLAGS="-O2 -ftree-vectorize -g -fno-omit-frame-pointer <mark style="color:blue;">-mfma -mavx512f -mavx512cd</mark> -ffast-math $TSANFLAGS"\
> &#x20;764라인 : FCFLAGS="-O2 -ftree-vectorize -g -fno-omit-frame-pointer <mark style="color:blue;">-mfma -mavx512f -mavx512cd</mark> -ffast-math $TSANFLAGS"\
> &#x20;765라인 : CXXFLAGS="-O2 -ftree-vectorize -g -fno-omit-frame-pointer <mark style="color:blue;">-mfma -mavx512f -mavx512cd</mark> -ffast-math $TSANFLAGS"\
> &#x20;\
> &#x20;859라인 : BASEFLAGS="-mfma -mavx512f -mavx512cd -fno-omit-frame-pointer -g $"

※ 위 수정 예제는 <mark style="color:blue;">"-march=native" 를 "-mfma -mavx512f -mavx512cd"</mark> 로 변경 하였다.   "-march=native"는 설치 시스템에 장착된 CPU 의 최적 옵션을 찾아 컴파일하는 옵션으로,   소개 내용은 누리온의 skylake 와 knl 계산노드에서 모두 사용 가능하도록 "-mfma -mavx512f -mavx512cd" 로 소개한다.※ 참고 : [https://colfaxresearch.com/knl-avx512](https://colfaxresearch.com/knl-avx512)&#x20;



**\[install\_ptscotch.sh 수정 사항]**&#x20;

> &#x20;38라인 :  sed -e "s|\\(^CCS\\).\*|\1 = <mark style="color:blue;">$MPICC</mark>|g" \\\
> &#x20;40라인 :      -e "s|\\(^CCD\\).\*|\1 = <mark style="color:blue;">$MPICC</mark>|g" \\

※ "<mark style="color:blue;">$CC</mark>" 대신 "<mark style="color:blue;">$MPICC</mark>" 를 사용한다.



**\[install\_pexsi.sh수정 사항]**&#x20;

> &#x20;55라인 : -e "s|\\(CPP\_LIB \*=\\).\*|\1 -lstdc++ $ $ <mark style="color:blue;">-lmpi\_cxx</mark> |g" \\

※ "<mark style="color:blue;">-lmpi\_cxx</mark>" 옵션을 추가 한다.



**\[local.popt 수정 사항]**&#x20;



※ 17라인 LIBS 항목에 <mark style="color:blue;">"-lmpi\_cxx" 가 없는 경우</mark> <mark style="color:blue;"></mark><mark style="color:blue;">****</mark> <mark style="color:blue;"></mark><mark style="color:blue;">"-lmpi -lmpi\_cxx"</mark> 와 같이 "<mark style="color:blue;">-lmpi\_cxx</mark>"를 추가한다.

&#x20;

&#x20;

\
**\[local.psmp 수정 사항]**&#x20;

&#x20;

&#x20;

※ 17라인 LIBS 항목에 <mark style="color:blue;">"-lmpi\_cxx" 가 없는 경우 "-lmpi -lmpi\_cxx"</mark> 와 같이 "<mark style="color:blue;">-lmpi\_cxx</mark>"를 추가한다.



## **4. 테스트**

> &#x20;\#!/bin/sh\
> &#x20;\#PBS -V\
> &#x20;\#PBS -N cp2k\_sample\
> &#x20;\#PBS -q normal\
> &#x20;\#PBS -A etc\
> &#x20;\#PBS -l select=1:ncpus=64:mpiprocs=32:ompthreads=2\
> &#x20;\#PBS -l walltime=04:00:00\
> &#x20;\
> &#x20;cd $PBS\_O\_WORKDIR\
> &#x20;\
> &#x20;module purge\
> &#x20;module load gcc/6.1.0 openmpi/3.1.0 cmake/3.12.3\
> &#x20;export PATH=$PATH:$/cp2k-5.1.0/exe/local\
> \
> \
> &#x20;mpirun cp2k.psmp -i H2O-32.inp

※ <mark style="color:blue;">/$/CP2K-5.1.0/tests</mark> 경로에 예제가 들어 있으니 필요한 경우 본인의 스크레치(/scratch/$USER) 경로로 복사해서 테스트 진행해 볼 수 있다.



## **5. 기타**

CP2K toolchain 이용하여 설치 시 "-march=native" 옵션이 포함되어져 있어 KNL 계산노드 전용으로 설치하는 경우 KNL 계산노드에서 설치를 진행해야 오류가 발생되지 않는다.\
KNL 계산노드 전용으로 설치하는 경우 PBS 스케줄러의 Interactive 기능을 이용하여 KNL 계산노드로 접속하여 빌드를 진행해야 한다.아래는 누리온 KNL 계산노드(debug 큐) 로 접속하는 예제이다.&#x20;

> &#x20;$ qsub -I -V -q debug -l select=1:ncpus=68:mpiprocs=68:ompthreads=1 -l walltime=24:00:00 -A cp2k

**※ qsub 명령 뒤 문자는 I(대문자 아이)  이고, select 와 walltime 앞에 문자는 l(소문자 엘) 이다.**
