---
description: 슈퍼컴퓨팅인프라센터 2019. 4. 12. 15:40
---

# SIESTA 4.1-b3 설치(KNL)

KISTI 슈퍼컴퓨터센터의 누리온 시스템에 siesta-4.1-b3 source 버전으로 설치 하는 방법에 대하여 소개 한다.



## **1. 설치 환경**

|   **구분**       | **내용**                                 |
| -------------- | -------------------------------------- |
|  대상 시스템        | 누리온                                    |
|  OS Version    | 리눅스 / CentOS 7.3                       |
|  CPU           | Intel(R) Xeon Phi(TM) CPU 7250         |
|  컴파일러          | Intel 2017.5 Version                   |
|  MPI           | <p>IntelMPI 2017.5 Version<br><br></p> |
| <p> 기타<br></p> | Intel MKL Math Library                 |



## **2. 설치 전 환경 설정**

KISTI 시스템은 PATH, LD\_LIBRARY\_PATH 등을 쉽게 하기 위하여 OpenSource 인 Environment Modules(http://modules.sourceforge.net)이 구성되어 있고, 이하 설치 소개 에서는 module load를 이용한 환경 설정 방법을 이용한다.



{% code title="[ 환경 설정 ]" %}
```
$ module load craype-mic-knl intel/17.0.5 impi/17.0.5 netcdf-hdf5-parallel/4.6.1
```
{% endcode %}

SIESTA 는 check 과정을 포함하고 있어 KNL CPU 타입 전용 옵션(-xMIC-AVX512) 사용을 위해서는 KNL 계산노드에서 빌드 해야 한다.

소개 문서는 KNL CPU 가 장착된 디버깅 노드에서 빌드 하는 방법으로 안내한다.

****

{% code title="[ 디버깅 노드 접속 ]" %}
```
$ qsub -I -q debug -l select=1:ncpus=1:mpiprocs=1:ompthreads=1 -l walltime=12:00:00 -A siesta
```
{% endcode %}

## **3. 설치 과정**

&#x20;설치 과정 소개는 tar 를 이용한 압축 해제 방법과 설정 방법등 진행 절차를 위주로 설명하고, 소스 파일 다운로드 등은 생략한다. &#x20;

{% code title="  설치과정" %}
```
$ tar -xvf siesta-4.1-b3.tar.gz
$ cd siesta-4.1-b3
$ cd Obj
$ sh ../Src/obj_setup.sh
$ cp intel.make arch.make
$ vi arch.make 수정
    - - - [arch.make 파일 수정 내용] 참고 - - -
$ make
```
{% endcode %}

{% code title="[arch.make 파일 수정 내용]" %}
```
# 
# Copyright (C) 1996-2016 The SIESTA group
#  This file is distributed under the terms of the
#  GNU General Public License: see COPYING in the top directory
#  or http://www.gnu.org/copyleft/gpl.txt.
# See Docs/Contributors.txt for a list of contributors.
#
#-------------------------------------------------------------------
# arch.make file for gfortran compiler.
# To use this arch.make file you should rename it to
#   arch.make
# or make a sym-link.
# For an explanation of the flags see DOCUMENTED-TEMPLATE.make


.SUFFIXES:
.SUFFIXES: .f .F .o .c .a .f90 .F90


SIESTA_ARCH = unknown


CC = mpiicc
FPP = $(FC) -E -P
FC = mpiifort
FC_SERIAL = ifort


FFLAGS = -O2 -fPIC


AR = ar
RANLIB = ranlib


SYS = nag


SP_KIND = 4
DP_KIND = 8
KINDS = $(SP_KIND) $(DP_KIND)


LDFLAGS = -mkl=cluster


COMP_LIBS = libsiestaLAPACK.a libsiestaBLAS.a


FPPFLAGS = $(DEFS_PREFIX)-DFC_HAVE_ABORT


MPI_INTERFACE = libmpi_f90.a


MPI_INCLUDE = .


FPPFLAGS += -DMPI


LIBS = -mkl=cluster -L/apps/compiler/intel/17.0.5/impi/17.0.5/applib2/mic-knl/netcdf-hdf5-parallel/4.6.1/lib -lnetcdf


# Dependency rules ---------


FFLAGS_DEBUG = -g -O1   # your appropriate flags here...


# The atom.f code is very vulnerable. Particularly the Intel compiler
# will make an erroneous compilation of atom.f with high optimization
# levels.
atom.o: atom.F
$(FC) -c $(FFLAGS_DEBUG) $(INCFLAGS) $(FPPFLAGS) $(FPPFLAGS_fixed_F) $< 
pdos2k.o: pdos2k.F
$(FC) -c $(INCFLAGS) $(FPPFLAGS) $(FPPFLAGS_fixed_F) $<
pdos3k.o: pdos3k.F
$(FC) -c $(INCFLAGS) $(FPPFLAGS) $(FPPFLAGS_fixed_F) $<


.c.o:
$(CC) -c $(CFLAGS) $(INCFLAGS) $(CPPFLAGS) $< 
.F.o:
$(FC) -c $(FFLAGS) $(INCFLAGS) $(FPPFLAGS) $(FPPFLAGS_fixed_F)  $< 
.F90.o:
$(FC) -c $(FFLAGS) $(INCFLAGS) $(FPPFLAGS) $(FPPFLAGS_free_F90) $< 
.f.o:
$(FC) -c $(FFLAGS) $(INCFLAGS) $(FCFLAGS_fixed_f)  $<
.f90.o:
$(FC) -c $(FFLAGS) $(INCFLAGS) $(FCFLAGS_free_f90)  $<
```
{% endcode %}

