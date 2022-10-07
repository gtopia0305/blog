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

{% code title="[ 환경 설정 ]" %}
```
$ module load intel/18.0.3 impi/18.0.3
```
{% endcode %}

## **3. vasp 5.4.4 버전 설치 과정**

&#x20;설치 과정 소개는 tar 를 이용한 압축 해제 방법과 설정 방법등 진행 절차를 위주로 설명하고, 소스 파일 다운로드 등은 생략한다. &#x20;

{% code title=" 설치 과정" %}
```
$ tar xvzf vasp.5.4.4.tar.gz
$ cd vasp.5.4.4
$ cp arch/makefile.include.linux_intel makefile.include
$ vi makefile.include
 - - - - - [ makefile.include 파일 수정 내용] 참고
$ make all
```
{% endcode %}

{% code title="[make.include 파일 수정 내용]" %}
```
ADDITIONAL_OPTS=-O3 -qopenmp -xMIC-AVX512 -align array64byte
ADDITIONAL_OPTS_CC=-O3 -qopenmp -xMIC-AVX512

# Precompiler options
CPP_OPTIONS= -DMPI -DHOST=\"IFC15_impi\" \
-DCACHE_SIZE=12000 -Davoidalloc \
-DMPI_BLOCK=8000  -Duse_collective \
-DnoAugXCmeta -Duse_bse_te \
-Dtbdyn \
-D_OPENMP -DPROFILING -Duse_shmem \
-Dshmem_bcast_buffer -Dsheme_rproj
CPP = fpp -f_com=no -free -w0 $*$(FUFFIX) $*$(SUFFIX) $(CPP_OPTIONS)
FC = mpiifort -qopenmp -mkl=parallel ${ADDITIONAL_OPTS}
FCL = mpiifort -mkl=parallel ${ADDITIONAL_OPTS}
FREE = -free -names lowercase
FFLAGS = -assume byterecl
OFLAG = ${ADDITIONAL_OPTS}
OFLAG_IN = $(OFLAG)
DEBUG = -O0
MKL_PATH = $(MKLROOT)/lib/intel64
BLAS =
LAPACK =
BLACS = 
SCALAPACK = 
OBJECTS = fftmpiw.o fftmpi_map.o fftw3d.o fft3dlib.o
INCS =-I$(MKLROOT)/include/fftw
LLIBS = $(SCALAPACK) $(LAPACK) $(BLAS)
OBJECTS_O1 += fft3dfurth.o fftw3d.o fftmpi.o fftmpiw.o
OBJECTS_O2 += fft3dlib.o

# For what used to be vasp.5.lib
CPP_LIB = $(CPP)
FC_LIB = $(FC)
CC_LIB = icc
CFLAGS_LIB = -O
FFLAGS_LIB = ${ADDITIONAL_OPTS}
FREE_LIB = $(FREE)
OBJECTS_LIB= linpack_double.o getshmem.o

# For the parser library
CXX_PARS = icpc
LIBS += parser
LLIBS += -Lparser -lparser -lstdc++

# # Normally no need to change this
SRCDIR = ../../src
BINDIR = ../../bin

#================================================
# GPU Stuff

CPP_GPU    = -DCUDA_GPU -DRPROMU_CPROJ_OVERLAP -DUSE_PINNED_MEMORY -DCUFFT_MIN=28 -UscaLAPACK

OBJECTS_GPU = fftmpiw.o fftmpi_map.o fft3dlib.o fftw3d_gpu.o fftmpiw_gpu.o

CC         = icc
CXX        = icpc
CFLAGS     = -fPIC -DADD_ -Wall -openmp -DMAGMA_WITH_MKL -DMAGMA_SETAFFINITY -DGPUSHMEM=300 -DHAVE_CUBLAS

CUDA_ROOT  ?= /usr/local/cuda/
NVCC       := $(CUDA_ROOT)/bin/nvcc -ccbin=icc
CUDA_LIB   := -L$(CUDA_ROOT)/lib64 -lnvToolsExt -lcudart -lcuda -lcufft -lcublas

GENCODE_ARCH    := -gencode=arch=compute_30,code=\"sm_30,compute_30\" \
                   -gencode=arch=compute_35,code=\"sm_35,compute_35\" \
                   -gencode=arch=compute_60,code=\"sm_60,compute_60\"

MPI_INC    = $(I_MPI_ROOT)/include64/
```
{% endcode %}
