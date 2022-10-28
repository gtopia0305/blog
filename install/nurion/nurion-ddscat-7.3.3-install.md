---
description: 슈퍼컴퓨팅인프라센터 2021. 1. 27. 10:59
---

# DDSCAT-7.3.3 설치 소개

KISTI 슈퍼컴퓨팅센터의 누리온 시스템에 DDSCAT-7.3.3 Source 버전으로 설치하는 방법에 대하여 소개한다.

&#x20;

## **1. 설치 환경**

|  **구분**      | **내용**                      |
| ------------ | --------------------------- |
|  대상 시스템      |  누리온                        |
|  OS Version  |  리눅스 / CentOS 7.7           |
|  CPU         |  Intel(R) Xeon(R) Gold 6126 |
|  컴파일러        |  intel 2018.3               |
|  MPI         |  intel mpi 2018.3           |
|  기타          |                             |

&#x20;

## **2. 설치 전 환경 설정**

누리온 시스템은 PATH, LD\_LIBRARY\_PATH 등을 쉽게 설정하기 위하여&#x20;

환경설정 툴인 Modules(http://modules.sourceforge.net)이 구성되어 있고,

&#x20;

이하 설치 소개에서는 module load를 이용한 환경 설정 방법을 이용한다.

&#x20;

{% code title="[ 환경 설정 ]" %}
```
 $ module load intel/18.0.3 impi/18.0.3
```
{% endcode %}



## **3. DDSCAT-7.3.3 버전 설치 과정**

&#x20;설치 과정 소개는 tar 를 이용한 압축 해제 방법과 설정 방법등 진행 절차를 위주로 설명하고,

&#x20;소스 파일 다운로드 등은 생략한다.  &#x20;

{% code title="설치 과정" %}
```
 $ tar -xzvf ddscat7.3.3_200717.tgz
 $ cd src
 $ cp $MKLROOT/include/mkl_dfti.f90 .
 $ vi Makefile
 - - - - - [ Makefile 파일 수정 내용] 참고 - - - - -
 
 $ make ddscat
 
 $ ls ddscat
 ddscat
```
{% endcode %}

&#x20;

{% code title="[Makefile 파일 수정 내용]" %}
```
#### line 114 ~ 125 주석처리 ####
110 # 1.  gfortran compiler
111 #     sp + no MKL + no OpenMP + no MPI
112 #
113 # define the following:
114 # PRECISION   = sp
115 # CXFFTMKL.f  = cxfft3_mkl_fake.f90
116 # CXFFTMKL.o  = cxfft3_mkl_fake.o
117 # MKLM        =
118 # DOMP        =
119 # OPENMP      =
120 # MPI.f       = mpi_fake.f90
121 # MPI.o       = mpi_fake.o
122 # DMPI        =
123 # FC      = gfortran
124 # FFLAGS      = -O2
125 # LFLAGS      =




#### line 266 ~ 277 주석처리 해제 및 수정 ####
258 # 8.  ifort compiler (via mpif90)
259 #     sp + MKL + OpenMP + MPI
260 
261 # on some systems, before compiling, type
262 #   module purge
263 #   module load intel-mkl openmpi
264 
265 # define the following:
266 PRECISION  = sp
267 CXFFTMKL.f = $(MKL_f)
268 CXFFTMKL.o = $(MKL_o)
269 MKLM       = $(MKL_m)
270 DOMP       = -Dopenmp
271 OPENMP     = -qopenmp
272 MPI.f      = $(MPI_f)
273 MPI.o      = $(MPI_o)
274 DMPI       = -Dmpi
275 FC     = mpiifort
276 FFLAGS     = -O2
277 LFLAGS     = -traceback -mkl=parallel -lmpi
```
{% endcode %}

## **4. 실기영상**

{% embed url="https://youtu.be/GC1APApZuUE" %}
\[누리온] DDSCAT7.3.3 설
{% endembed %}

&#x20;
