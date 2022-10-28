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



{% code title="[ 환경 설정 ]" %}
```
 $ module load intel/18.0.2 cuda/10.0 cudampi/mvapich2-2.3
```
{% endcode %}

## **3. vasp 5.4.4 버전 설치 과정**

&#x20;설치 과정 소개는 tar 를 이용한 압축 해제 방법과 설정 방법등 진행 절차를 위주로 설명하고, 소스 파일 다운로드 등은 생략합니다. &#x20;

{% code title=" 설치 과정" %}
```
$ tar xvzf vasp.5.4.4.tar.gz
$ cd vasp.5.4.4
$ cp arch/makefile.include.linux_intel ./makefile.include
$ vi makefile.include
 - - - - - [ makefile.include 파일 수정 내용] 참고
$ make gpu
```
{% endcode %}

{% code title="[make.include 파일 수정 내용]" %}
```
# Precompiler options
CPP_OPTIONS= -DHOST=\"LinuxIFC\"\
             -DMPI -DMPI_BLOCK=8000 \
             -Duse_collective \
             -DCACHE_SIZE=16000 \
             -Davoidalloc \
             -Duse_bse_te \
             -Dtbdyn \
             -Duse_shmem

CPP        = fpp -f_com=no -free -w0  $*$(FUFFIX) $*$(SUFFIX) $(CPP_OPTIONS)

FC         = mpif90
FCL        = mpif90 -mkl -lstdc++

FREE       = -free -names lowercase

FFLAGS     = -assume byterecl -w
OFLAG      = -O2 -fPIC -xAVX
OFLAG_IN   = $(OFLAG)
DEBUG      = -O0
MKL_PATH   = $(MKLROOT)/lib/intel64
BLAS       =
LAPACK     =
OBJECTS    = fftmpiw.o fftmpi_map.o fft3dlib.o fftw3d.o
INCS       =-I$(MKLROOT)/include/fftw
LLIBS      = 

OBJECTS_O1 += fftw3d.o fftmpi.o fftmpiw.o
OBJECTS_O2 += fft3dlib.o

# For what used to be vasp.5.lib
CPP_LIB    = $(CPP)
FC_LIB     = $(FC)
CC_LIB     = icc
CFLAGS_LIB = -O
FFLAGS_LIB = -O1
FREE_LIB   = $(FREE)
OBJECTS_LIB= linpack_double.o getshmem.o

# For the parser library
CXX_PARS   = icpc
LIBS       += parser
LLIBS      += -Lparser -lparser -lstdc++

# Normally no need to change this
SRCDIR     = ../../src
BINDIR     = ../../bin

#================================================
# GPU Stuff

CPP_GPU    = -DCUDA_GPU -DRPROMU_CPROJ_OVERLAP -DUSE_PINNED_MEMORY -DCUFFT_MIN=28 -UscaLAPACK
OBJECTS_GPU = fftmpiw.o fftmpi_map.o fft3dlib.o fftw3d_gpu.o fftmpiw_gpu.o
CC         = icc
CXX        = icpc
CFLAGS     = -fPIC -DADD_ -Wall -qopenmp -DMAGMA_WITH_MKL -DMAGMA_SETAFFINITY -DGPUSHMEM=300 -DHAVE_CUBLAS

CUDA_ROOT  ?= /apps/cuda/10.0
NVCC       := $(CUDA_ROOT)/bin/nvcc -ccbin=icc -std=c++11
CUDA_LIB   := -L$(CUDA_ROOT)/lib64 -lnvToolsExt -lcudart -lcuda -lcufft -lcublas

GENCODE_ARCH    := -gencode=arch=compute_35,code=\"sm_35,compute_35\" \
-gencode=arch=compute_70,code=\"sm_70,compute_70\"

MPI_INC    = /apps/compiler/intel/18.0.2/cudampi/10.0/mvapich2/2.3/include
```
{% endcode %}



## **4. 기타**

vasp.5.4.4.18Apr17-6-g9f103f2a35 버전을 사용하는 경우 아래 참고 사이트를 확인하여 패치를 진행 해야 GPU 작업 오류가 발생하지 않습니다.



\- 참고 : [https://cms.mpi.univie.ac.at/wiki/index.php/Installing\_VASP](https://cms.mpi.univie.ac.at/wiki/index.php/Installing\_VASP)
