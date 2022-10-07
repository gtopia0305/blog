---
description: 슈퍼컴퓨팅인프라센터 2018. 11. 29. 09:39
---

# VASP 5.4.4 설치 (KNL버전)

KISTI 슈퍼컴퓨팅센터의 누리온 시스템에 vasp 5.4.4 Source 버전으로 설치 하는 방법에 대하여 소개 한다.



## **1. 설치 환경**

|  **구분**     | **내용**                          |
| ----------- | ------------------------------- |
|  대상 시스템     |  누리온                            |
| OS Version  |  리눅스 / CentOS 7.3               |
|  CPU        |  Intel(R) Xeon Phi(TM) CPU 7250 |
|  컴파일러       |  Intel 2018.3 Version           |
|  MPI        |  IntelMPI 2018.3 Version        |
|  기타         |  Intel MKL Math Library         |



## **2. 설치 전 환경 설정**

&#x20; 누리온 시스템은 PATH, LD\_LIBRARY\_PATH 등을 쉽게 하기 위하여 \
&#x20; 환경설정 툴인 Modules(http://modules.sourceforge.net)이 구성되어 있고,\
&#x20; 이하 설치 소개 에서는 module load를 이용한 환경 설정 방법을 이용한다.



\[ 환경 설정 ]

> &#x20;$ module load intel/18.0.3 impi/18.0.3

## **3. vasp 5.4.4 버전 설치 과정**

&#x20;설치 과정 소개는 tar 를 이용한 압축 해제 방법과 설정 방법등 진행 절차를 위주로 설명하고, 소스 파일 다운로드 등은 생략한다. &#x20;

|  **설치 과정**                                                                                                                                                                                                            |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p>$ tar xvzf vasp.5.4.4.tar.gz</p><p>$ cd vasp.5.4.4</p><p>$ cp arch/makefile.include.linux_intel makefile.include</p><p>$ vi makefile.include</p><p> - - - - - [ makefile.include 파일 수정 내용] 참고</p><p>$ make all</p> |



\[make.include 파일 수정 내용]

> ADDITIONAL\_OPTS=-O3 -qopenmp -xMIC-AVX512 -align array64byte
>
> ADDITIONAL\_OPTS\_CC=-O3 -qopenmp -xMIC-AVX512
>
> \
> \# Precompiler options
>
> CPP\_OPTIONS= -DMPI -DHOST=\\"IFC15\_impi\\" \\
>
> \-DCACHE\_SIZE=12000 -Davoidalloc \\
>
> \-DMPI\_BLOCK=8000  -Duse\_collective \\
>
> \-DnoAugXCmeta -Duse\_bse\_te \\
>
> \-Dtbdyn \\
>
> \-D\_OPENMP -DPROFILING -Duse\_shmem \\
>
> \-Dshmem\_bcast\_buffer -Dsheme\_rproj
>
> CPP = fpp -f\_com=no -free -w0 $\*$(FUFFIX) $\*$(SUFFIX) $(CPP\_OPTIONS)
>
> FC = mpiifort -qopenmp -mkl=parallel ${ADDITIONAL\_OPTS}
>
> FCL = mpiifort -mkl=parallel ${ADDITIONAL\_OPTS}
>
> FREE = -free -names lowercase
>
> FFLAGS = -assume byterecl
>
> OFLAG = ${ADDITIONAL\_OPTS}
>
> OFLAG\_IN = $(OFLAG)
>
> DEBUG = -O0
>
> MKL\_PATH = $(MKLROOT)/lib/intel64
>
> BLAS =
>
> LAPACK =
>
> BLACS =&#x20;
>
> SCALAPACK =&#x20;
>
> OBJECTS = fftmpiw.o fftmpi\_map.o fftw3d.o fft3dlib.o
>
> INCS =-I$(MKLROOT)/include/fftw
>
> LLIBS = $(SCALAPACK) $(LAPACK) $(BLAS)
>
> OBJECTS\_O1 += fft3dfurth.o fftw3d.o fftmpi.o fftmpiw.o
>
> OBJECTS\_O2 += fft3dlib.o
>
>
>
> \# For what used to be vasp.5.lib
>
> CPP\_LIB = $(CPP)
>
> FC\_LIB = $(FC)
>
> CC\_LIB = icc
>
> CFLAGS\_LIB = -O
>
> FFLAGS\_LIB = ${ADDITIONAL\_OPTS}
>
> FREE\_LIB = $(FREE)
>
> OBJECTS\_LIB= linpack\_double.o getshmem.o
>
>
>
> \# For the parser library
>
> CXX\_PARS = icpc
>
> LIBS += parser
>
> LLIBS += -Lparser -lparser -lstdc++
>
>
>
> \# # Normally no need to change this
>
> SRCDIR = ../../src
>
> BINDIR = ../../bin
>
>
>
> \#================================================
>
> \# GPU Stuff
>
>
>
> CPP\_GPU    = -DCUDA\_GPU -DRPROMU\_CPROJ\_OVERLAP -DUSE\_PINNED\_MEMORY -DCUFFT\_MIN=28 -UscaLAPACK
>
>
>
> OBJECTS\_GPU = fftmpiw.o fftmpi\_map.o fft3dlib.o fftw3d\_gpu.o fftmpiw\_gpu.o
>
>
>
> CC         = icc
>
> CXX        = icpc
>
> CFLAGS     = -fPIC -DADD\_ -Wall -openmp -DMAGMA\_WITH\_MKL -DMAGMA\_SETAFFINITY -DGPUSHMEM=300 -DHAVE\_CUBLAS
>
>
>
> CUDA\_ROOT  ?= /usr/local/cuda/
>
> NVCC       := $(CUDA\_ROOT)/bin/nvcc -ccbin=icc
>
> CUDA\_LIB   := -L$(CUDA\_ROOT)/lib64 -lnvToolsExt -lcudart -lcuda -lcufft -lcublas
>
>
>
> GENCODE\_ARCH    := -gencode=arch=compute\_30,code=\\"sm\_30,compute\_30\\" \\
>
> &#x20;                  \-gencode=arch=compute\_35,code=\\"sm\_35,compute\_35\\" \\
>
> &#x20;                  \-gencode=arch=compute\_60,code=\\"sm\_60,compute\_60\\"
>
>
>
> MPI\_INC    = $(I\_MPI\_ROOT)/include64/
