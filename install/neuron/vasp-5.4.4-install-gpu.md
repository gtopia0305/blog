---
description: 슈퍼컴퓨팅인프라센터 2019. 4. 30. 10:01
---

# VASP 5.4.4 설치 (GPU 버전)

KISTI 슈퍼컴퓨팅센터의 뉴론 시스템에 vasp 5.4.4 Source 버전으로 설치 하는 방법에 대하여 소개 합니다.



## **1. 설치 환경**

|  **구분**     | **내용**                             |
| ----------- | ---------------------------------- |
|  대상 시스템     |  뉴론                                |
| OS Version  |  리눅스 / CentOS 7.4                  |
|  CPU        |  Intel Xeon E5-2670 v2             |
|  컴파일러       |  Intel 2018 Version                |
|  MPI        |  Mvapich2 2.3 Version              |
|  기타         |  Intel MKL Math Library, cuda 10.0 |



## **2. 설치 전 환경 설정**

&#x20; 뉴론 시스템은 PATH, LD\_LIBRARY\_PATH 등을 쉽게 하기 위하여 \
&#x20; 환경설정 툴인 Modules(http://modules.sourceforge.net)이 구성되어 있고,\
&#x20; 이하 설치 소개 에서는 module load를 이용한 환경 설정 방법을 이용합니다.



\[ 환경 설정 ]

> &#x20;$ module load intel/18.0.2 cuda/10.0 cudampi/mvapich2-2.3

## **3. vasp 5.4.4 버전 설치 과정**

&#x20;설치 과정 소개는 tar 를 이용한 압축 해제 방법과 설정 방법등 진행 절차를 위주로 설명하고, 소스 파일 다운로드 등은 생략합니다. &#x20;

|  **설치 과정**                                                                                                                                                                                                              |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p>$ tar xvzf vasp.5.4.4.tar.gz</p><p>$ cd vasp.5.4.4</p><p>$ cp arch/makefile.include.linux_intel ./makefile.include</p><p>$ vi makefile.include</p><p> - - - - - [ makefile.include 파일 수정 내용] 참고</p><p>$ make gpu</p> |



\[make.include 파일 수정 내용]

> \# Precompiler options\
> CPP\_OPTIONS= -DHOST=\\"LinuxIFC\\"\\\
> &#x20;            \-DMPI -DMPI\_BLOCK=8000 \\\
> &#x20;            \-Duse\_collective \\\
> &#x20;            \-DCACHE\_SIZE=16000 \\\
> &#x20;            \-Davoidalloc \\\
> &#x20;            \-Duse\_bse\_te \\\
> &#x20;            \-Dtbdyn \\\
> &#x20;            \-Duse\_shmem
>
>
>
> CPP        = fpp -f\_com=no -free -w0  $\*$(FUFFIX) $\*$(SUFFIX) $(CPP\_OPTIONS)
>
>
>
> FC         = mpif90\
> FCL        = mpif90 -mkl -lstdc++
>
>
>
> FREE       = -free -names lowercase
>
>
>
> FFLAGS     = -assume byterecl -w\
> OFLAG      = -O2 -fPIC -xAVX\
> OFLAG\_IN   = $(OFLAG)\
> DEBUG      = -O0
>
> MKL\_PATH   = $(MKLROOT)/lib/intel64\
> BLAS       =\
> LAPACK     =\
> OBJECTS    = fftmpiw.o fftmpi\_map.o fft3dlib.o fftw3d.o
>
> INCS       =-I$(MKLROOT)/include/fftw
>
> LLIBS      =&#x20;
>
> \
> OBJECTS\_O1 += fftw3d.o fftmpi.o fftmpiw.o\
> OBJECTS\_O2 += fft3dlib.o
>
>
>
> \# For what used to be vasp.5.lib\
> CPP\_LIB    = $(CPP)\
> FC\_LIB     = $(FC)\
> CC\_LIB     = icc\
> CFLAGS\_LIB = -O\
> FFLAGS\_LIB = -O1\
> FREE\_LIB   = $(FREE)
>
> OBJECTS\_LIB= linpack\_double.o getshmem.o
>
>
>
> \# For the parser library\
> CXX\_PARS   = icpc
>
> LIBS       += parser\
> LLIBS      += -Lparser -lparser -lstdc++
>
>
>
> \# Normally no need to change this\
> SRCDIR     = ../../src\
> BINDIR     = ../../bin
>
>
>
> \#================================================\
> \# GPU Stuff
>
>
>
> CPP\_GPU    = -DCUDA\_GPU -DRPROMU\_CPROJ\_OVERLAP -DUSE\_PINNED\_MEMORY -DCUFFT\_MIN=28 -UscaLAPACK
>
> OBJECTS\_GPU = fftmpiw.o fftmpi\_map.o fft3dlib.o fftw3d\_gpu.o fftmpiw\_gpu.o
>
> CC         = icc\
> CXX        = icpc\
> CFLAGS     = -fPIC -DADD\_ -Wall -qopenmp -DMAGMA\_WITH\_MKL -DMAGMA\_SETAFFINITY -DGPUSHMEM=300 -DHAVE\_CUBLAS
>
>
>
> CUDA\_ROOT  ?= /apps/cuda/10.0\
> NVCC       := $(CUDA\_ROOT)/bin/nvcc -ccbin=icc -std=c++11\
> CUDA\_LIB   := -L$(CUDA\_ROOT)/lib64 -lnvToolsExt -lcudart -lcuda -lcufft -lcublas
>
> \
> GENCODE\_ARCH    := -gencode=arch=compute\_35,code=\\"sm\_35,compute\_35\\" \\
>
> \-gencode=arch=compute\_70,code=\\"sm\_70,compute\_70\\"
>
>
>
> MPI\_INC    = /apps/compiler/intel/18.0.2/cudampi/10.0/mvapich2/2.3/include



## **4. 기타**

vasp.5.4.4.18Apr17-6-g9f103f2a35 버전을 사용하는 경우 아래 참고 사이트를 확인하여 패치를 진행 해야 GPU 작업 오류가 발생하지 않습니다.



\- 참고 : [https://cms.mpi.univie.ac.at/wiki/index.php/Installing\_VASP](https://cms.mpi.univie.ac.at/wiki/index.php/Installing\_VASP)
