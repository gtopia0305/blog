---
description: 슈퍼컴퓨팅인프라센터 2021. 5. 28. 10:36
---

# lammps-3Mar20 설치 소개(KNL)

KISTI 슈퍼컴퓨터센터의 장비에 lammps-3Mar20 source 버전으로 설치 하는 방법에 대하여 소개 한다.

&#x20;

## **1. 설치 환경**

| 구분          | 내용                         |
| ----------- | -------------------------- |
|  대상 시스템     | 누리온                        |
|  OS Version | 리눅스 / CentOS 7.7           |
|  CPU        | Intel(R) Xeon(R) Gold 6126 |
|  컴파일러       | Intel 2019.5 Version       |
|  MPI        | IntelMPI 2019.5 Version    |
|  기타         | Intel MKL Math Library     |

&#x20;

## **2. 설치 전 환경 설정**

KISTI 시스템은 PATH, LD\_LIBRARY\_PATH 등을 쉽게 하기 위하여 OpenSource 인 Environment Modules이 구성되어 있고, 이하 설치 소개 에서는 module load를 이용한 환경 설정 방법을 이용한다.

{% code title="[ 환경 설정 ]" %}
```
 $ module load intel/19.0.5 impi/19.0.5
```
{% endcode %}



## **3. 설치 과정**

&#x20;설치 과정 소개는 tar를 이용한 압축 해제 방법과 설정 방법 등 진행 절차를 위주로 설명하고, 소스 파일 다운로드 등은 생략한다.

(다운로드 : [https://lammps.sandia.gov/tars/)](https://lammps.sandia.gov/tars/\))\
\
설치 경로는 **${HOME}/lammps/3Mar20**을 사용하였다. 이 위치는 사용자에게 맞는 위치로 변경하여야 한다.&#x20;

&#x20;

### &#x20;**(1) VORO++ 설치** (다운로드 : [http://math.lbl.gov/voro++/download/)](http://math.lbl.gov/voro++/download)

VORONOI 패키지 설치를 위한 voro++를 우선 설치한다.&#x20;

{% code title="설치과정" %}
```
$ tar xvf voro++-0.4.6.tar.gz
$ cd voro++-0.4.6
$ mkdir -p ${HOME}/build/library
$ vi config.mk
----- 수정 사항은 아래의 내용 참고 -----
$ make
$ make install
```
{% endcode %}

{% code title="[config.mk 수정 사항]" %}
```
CXX=mpiicpc
CFLAGS= -Wall -ansi -pedantic -O3 -fPIC
E_INC= -I../../src
E_LIB= -L../../src
PREFIX= ${HOME}/build/library
INSTALL= install
IFLAGS_EXEC= -m 0755
IFLAGS= -m 0644
```
{% endcode %}

&#x20;

### &#x20;**(2) LATTE 설치** (다운로드 : [https://github.com/lanl/LATTE/releases](https://github.com/lanl/LATTE/releases))

LATTTE 패키지 설치를 위한 Latte 라이브러리를 우선 설치한다.

다운로드 받은 파일을 적당한 위치($HOME/build)에 올린 후 다음과 같은 명령으로 압축 묶음 파일을 푼다.

{% code title="설치과정" %}
```
$ cd ${HOME}/build
$ tar xzvf LATTE-1.2.1.tar.gz
$ cd LATTE-1.2.1
$ vi makefile.CHOICES
----- 수정 사항은 아래의 내용 참고 -----
$ make
```
{% endcode %}

{% code title="[makefile.CHOICES 수정 사항]" %}
```
#
# CPU Fortran options
#
 
#For GNU compiler:
#FC = mpif90
#FC = gfortran
#FCL = $(FC)
#FFLAGS = -O3 -fopenmp -cpp
#FFLAGS = -fast -Mpreprocess -mp
#LINKFLAG = -fopenmp
 
#For intel compiler:
FC = ifort
FCL = $(FC)
FFLAGS = -O3 -fpp -qopenmp
LINKFLAG = -qopenmp
#LIB = -mkl=parallel
 
#GNU BLAS/LAPACK libraries:
#LIB = -llapack -lblas
 
#Intel MKL BLAS/LAPACK libraries:
LIB = -Wl,--no-as-needed -L${MKLROOT}/lib/intel64 \
-lmkl_lapack95_lp64 -lmkl_gf_lp64 -lmkl_gnu_thread -lmkl_core \
-lmkl_gnu_thread -lmkl_core -ldl -lpthread -lm

#Alternative flags for MKL:
LIB += -mkl=parallel 


#
# GPU options
#
 
#GPU_CUDA_LIB = -L/opt/cudatoolkit-5.5/lib64 -lcublas -lcudart


#GPU_ARCH = sm_20 
```
{% endcode %}

&#x20;

### &#x20;**(3) 라이브러리 패키지 설치**

LAMMPS 홈페이지([http://lammps.sandia.gov/index.html](http://lammps.sandia.gov/index.html\)))로부터 다운로드 받은 파일을 적당한 위치($HOME/build)에 올린 후 다음과 같은 명령으로 압축 묶음 파일을 푼다.

```
$ tar xzvf lammps-3Mar20.tar.gz
```

&#x20;

&#x20;(3-1) voronoi 설치

(1)에서 설치한 voro++ 설치 디렉토리를 지정해 준다.

lammps 압축 해제후 lammps-3Mar20 폴더로 이동하여 아래의 작업을 진행한다.

{% code title="설치과정" %}
```
$ cd lammps-3Mar20
$ cd lib/voronoi
$ ln -s ${HOME}/build/library/include/voro++ includelink
$ ln -s ${HOME}/build/library/lib liblink
$ cd ../../
```
{% endcode %}

&#x20;

&#x20;(3-2) poems 설치

{% code title="설치과정" %}
```
$ cd lib/poems
$ vi Makefile.mpi
----- 수정 사항은 아래의 내용 참고 -----
$ make -f Makefile.mpi
$ cd ../../
```
{% endcode %}

{% code title="[Makefile.mpi 수정 사항]" %}
```
CC =            mpiicpc
CCFLAGS =       -O3 -g -fPIC -Wall #-Wno-deprecated
ARCHIVE =       ar
ARCHFLAG =      -rc
DEPFLAGS =      -M
LINK =          mpiicpc
```
{% endcode %}

&#x20;

&#x20;(3-3) awpmd 설치

{% code title="설치과정" %}
```
$ cd lib/awpmd
$ vi Makefile.lammps.linalg
----- 수정 사항은 아래의 내용 참고 -----
$ vi Makefile.mpi
----- 수정 사항은 아래의 내용 참고 -----
$ make -f Makefile.mpi
$ cd ../../
```
{% endcode %}

{% code title="[Makefile.lammps.installed 수정 사항]" %}
```
user-atc_SYSINC =
user-atc_SYSLIB = -llinalg
user-atc_SYSPATH = -L../../lib/linalg$(LIBOBJDIR)
```
{% endcode %}

{% code title="[Makefile.mpi 수정 사항]" %}
```
CC =        mpiicpc
```
{% endcode %}

&#x20;

&#x20;(3-4) atc 설치

{% code title="설치과정" %}
```
$ cd lib/atc
$ vi Makefile.lammps.linalg
----- 수정 사항은 아래의 내용 참고 -----
$ vi Makefile.mpi
----- 수정 사항은 아래의 내용 참고 -----
$ make -f Makefile.mpi
$ cd ../../
```
{% endcode %}

{% code title="[Makefile.lammps.linalg 수정 사항]" %}
```
user-atc_SYSINC =
user-atc_SYSLIB = -llinalg
user-atc_SYSPATH = -L../../lib/linalg$(LIBOBJDIR)
```
{% endcode %}

{% code title="[Makefile.mpi 수정 사항]" %}
```
CC =            mpiicpc
```
{% endcode %}

&#x20;

&#x20;(3-5) linalg 설치

{% code title="설치과정" %}
```
$ cd lib/linalg
$ vi Makefile.mpi
----- 수정 사항은 아래의 내용 참고 -----
$ make -f Makefile.mpi
$ cd ../../
```
{% endcode %}

{% code title="[Makefile.mpi 수정 사항]" %}
```
FC = mpiifort
FFLAGS = -O3 -fPIC
FFLAGS0 = -O0 -fPIC
```
{% endcode %}

&#x20;

&#x20;(3-6) latte 설치

{% code title="설치과정" %}
```
$ cd lib/latte
$ ln -s ${HOME}/build/LATTE-1.2.1/src includelink
$ ln -s ${HOME}/build/LATTE-1.2.1 liblink
$ ln -s ${HOME}/build/LATTE-1.2.1/src/latte_c_bind.o filelink.o
$ vi Makefile.lammps.mpi
----- 수정 사항은 아래의 내용 참고 ----- 
$ cd ../../
```
{% endcode %}

{% code title="[Makefile.lammps.mpi 수정 사항]" %}
```
latte_SYSINC =
latte_SYSLIB = ../../lib/latte/filelink.o -llatte -llinalg -lifport -lifcore
latte_SYSPATH = -L../../lib/linalg -qopenmp
```
{% endcode %}

&#x20;

&#x20;(3-7) message 설치

{% code title="설치과정" %}
```
$ cd lib/message/cslib/src
$ vi Makefile
----- 수정 사항은 아래의 내용 참고 ----- 
$ make lib_parallel zmq=no
$ cp libcsmpi.a libmessage.a
$ cd ../../../../
```
{% endcode %}

{% code title="[Makefile 수정 사항]" %}
```
ifeq ($(MPI),YES)
  CC = mpiicpc
else
  CCFLAGS += -I./STUBS_MPI
  LIB = libcsnompi.a
  SHLIB = libcsnompi.so
endif
```
{% endcode %}

&#x20;

&#x20;(3-8) plumed 설치

{% code title="설치과정" %}
```
$ cd lib/plumed
$ vi Install.py
----- 수정 사항은 아래의 내용 참고 ----- 
$ cd ../../src
$ make lib-plumed args="-b"
$ cd ..
```
{% endcode %}

{% code title="[Install.py 수정 사항]" %}
```
cmd = 'cd %s/plumed-%s; ./configure --prefix=%s --enable-static-patch CXX=mpiicpc ; make -j%d ; make install' % (homepath,version,homedir,n_cpus)
```
{% endcode %}

&#x20;&#x20;

### &#x20;**(4) LAMMPS 설치**

lammps 설치 디렉토리(${HOME}/build/lammps-3Mar20) 아래 src 폴더로 이동한다.

&#x20;

&#x20;\- package 선택 및 설치

사용하는 사용자의 연구내용에 맞추어 필요한 package를 선택하여 설치한다. 여기서는 기본적으로 많이 사용되는 package를 위주로 설치하였다.

| 설치과정                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |                                                                                                                                                                                                                                                                                                                                       |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p>$ cd src<br>$ make package-status<br>$ make yes-standard<br>$ make yes-message<br>$ make no-GPU<br>$ make no-PYTHON<br>$ make no-kim<br>$ make no-KOKKOS<br>$ make no-MSCG<br>$ make yes-USER-ATC<br>$ make yes-USER-AWPMD<br>$ make yes-USER-MEAMC<br>$ make yes-USER-OMP<br>$ make yes-USER-REAXC<br>$ make yes-USER-PLUMED<br><br>$ make package-status<br>$ vi MAKE/Makefile.mpi<br>-- 수정 사항은 아래 내용 참고 --<br>$ vi Makefile.package.settings<br>-- 수정 사항은 아래 내용 참고 --<br><br><br>$ make mpi</p> | <p> package 선택 확인<br> standard package 선택<br> <br> standard package 중 gpu package 제외<br> standard package 중 PYTHON package 제외<br> standard package 중 kim package 제외<br> standard package 중 KOKKOS package 제외<br> standard package 중 MSCG package 제외<br> <br> <br><br><br><br> <br> <br> package 선택 확인<br> <br> <br> <br> <br><br></p> |



{% code title="[MAKE/Makefile.mpi 수정 사항]" %}
```
CC = mpiicpc
OPTFLAGS = -xMIC-AVX512 -O3 -fp-model fast=2 -no-prec-div -qoverride-limits
CCFLAGS = -qopenmp -qno-offload -fno-alias -ansi-alias -restrict \
-DLMP_INTEL_USELRT -DLMP_USE_MKL_RNG $(OPTFLAGS)
CCFLAGS += -I/apps/compiler/intel/19.0.5/mkl/include -lmkl_rt
SHFLAGS = -fPIC
DEPFLAGS = -M
 
LINK = mpiicpc
LINKFLAGS = -qopenmp $(OPTFLAGS)
LIB =
SIZE = size
 
ARCHIVE = ar
ARFLAGS = -rc
SHLIBFLAGS = -shared
 
FFT_INC = -DFFT_MKL -DFFT_SINGLE
FFT_PATH =
FFT_LIB = -L${MKLROOT}/lib/intel64/ -lmkl_intel_ilp64 -lmkl_sequential -lmkl_core
```
{% endcode %}

{% code title="[Makefile.package.settings 수정 사항]" %}
```
include ../../lib/plumed/Makefile.lammps
include ../../lib/awpmd/Makefile.lammps
include ../../lib/atc/Makefile.lammps
include ../../lib/message/Makefile.lammps.nozmq
include ../../lib/voronoi/Makefile.lammps
include ../../lib/poems/Makefile.lammps
include ../../lib/latte/Makefile.lammps.mpi
include ../../lib/compress/Makefile.lammps
```
{% endcode %}

&#x20;

## **4. 실행 파일 복사**

&#x20;설치가 완료되면 사용에 편의를 위해 bin 경로를 만들어 실행 파일인 lmp\_mpi를 bin 경로에 복사한다.(선택사항)

```
ls -l lmp_mpi
$ cd ${HOME}/build/lammps-3Mar20/
$ mkdir bin
$ cp ${HOME}/build/lammps-3Mar20/src/lmp_mpi .
```

&#x20;

## **5. 누리온에서 LAMMPS 사용을 위한 PBS 작업 스크립트 예제**

&#x20;위의 과정을 거처 설치된 lammps는 누리온 환경에서 다음과 같이 실행이 가능하다.

누리온에서 작업을 제출하기 위해서는 PBS 작업 스크립트를 사용하여야 한다.

&#x20;

&#x20;

실행 예제로는 examples/meam 아래의 데이터를 이용하였다.

**작업스크립트 예제**(lammps\_test-run.sh)

```
#!/bin/sh
#PBS -V
#PBS -N lammps_job_test
#PBS -q normal
#PBS -l select=2:ncpus=60:mpiprocs=60:ompthreads=1
#PBS -l walltime=04:00:00
#PBS -A lammps
 
cd $PBS_O_WORKDIR
 
module purge
module load intel/19.0.5 impi/19.0.5
export PATH=${HOME}/build/lammps-3Mar20/bin:$PATH
 
mpirun lmp_mpi -in in.meamc
 
exit 0
```

&#x20;
