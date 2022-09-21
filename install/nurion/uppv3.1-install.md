---
description: 슈퍼컴퓨팅인프라센터 2019. 1. 7. 13:59
---

# UPPV3.1 설치 소개

KISTI 슈퍼컴퓨팅센터의 누리온 시스템에 UPPV3.1 Source 버전으로 설치 하는 방법에 대하여 소개 한다.



## **1. 설치 환경**

|  **구분**     | **내용**                      |
| ----------- | --------------------------- |
|  대상 시스템     |  누리온                        |
| OS Version  |  리눅스 / CentOS 7.3           |
|  CPU        |  Intel(R) Xeon(R) Gold 6126 |
|  컴파일러       |  Intel 2018.3 Version       |
|  MPI        |  IntelMPI 2018.3 Version    |
|  기타         |  WRFV3.9.1.1                |



## **2. 설치 전 환경 설정**

&#x20; 누리온 시스템은 PATH, LD\_LIBRARY\_PATH 등을 쉽게 하기 위하여 \
&#x20; 환경설정 툴인 Modules(http://modules.sourceforge.net)이 구성되어 있고,\
&#x20; 이하 설치 소개 에서는 module load를 이용한 환경 설정 방법을 이용한다.



\[ 환경 설정 ]

> &#x20;$ module load craype-x86-skylake intel/18.0.3 impi/18.0.3
>
> &#x20;$ module load hdf4/4.2.13 hdf5/1.10.2 netcdf/4.6.1 ncl/6.5.0
>
> &#x20;$ export JASPERLIB=/apps/common/jasper/1.900.29/lib
>
> &#x20;$ export JASPERINC=/apps/common/jasper/1.900.29/include

## **3. 설치 과정**

&#x20;설치 과정 소개는 tar 를 이용한 압축 해제 방법과 설정 방법등 진행 절차를 위주로 설명하고, 소스 파일 다운로드 등은 생략한다. &#x20;

|  **설치 과정**                                                                                                                                                                                                         |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| <p>$ tar xvzf DTC_upp_v3.1.tar.gz</p><p>$ cd UPPV3.1</p><p>$ ./configure </p><p> - - - - - [configure 과정 진행 내용] 참고</p><p>$ vi configure.upp</p><p> - - - - - [configure.upp 파일 수정 수정 내용] 참고</p><p>$ ./compile </p> |

※ UPP는 WRF 경로 설정을 별도로 하지 않기 위하여 아래와 같이 WRF와 동일 경로 상에 설치 하였다.\
&#x20;  만약 다른 경로에 WRF 가 있다면 WRF\_DIR 설정이 필요함.



\[configure 과정 진행 내용]

> $ ./configure
>
> Will use NETCDF in dir:  /apps/compiler/intel/18.0.3/applib1/mic-knl/netcdf/4.6.1
>
> Will use WRF in dir: /scratch/optpar02/install\_test/WRF/UPPV3.1/../WRFV3
>
> configure: making ./bin
>
> bindir  /scratch/optpar02/install\_test/WRF/UPPV3.1/bin
>
> configure: making ./include
>
> incmod  /scratch/optpar02/install\_test/WRF/UPPV3.1/include
>
> configure: making ./lib
>
> libdir  /scratch/optpar02/install\_test/WRF/UPPV3.1/lib
>
> JASPER Environent found :: GRIB2 library ::
>
> grib2lib = -L/apps/common/jasper/1.900.29/lib -lpng -lz -ljasper
>
> grib2inc = -I/apps/common/jasper/1.900.29/include
>
> \-------------------------------------------------------------------------
>
> Please select from among the following supported platforms.
>
>
>
> &#x20;  1\.  Linux x86\_64, PGI compiler  (serial)
>
> &#x20;  2\.  Linux x86\_64, PGI compiler  (dmpar)
>
> &#x20;  3\.  Linux x86\_64, Intel compiler  (serial)
>
> &#x20;  <mark style="color:blue;">4.  Linux x86\_64, Intel compiler  (dmpar)</mark>
>
> &#x20;  5\.  Linux x86\_64, Intel compiler, SGI MPT  (serial)
>
> &#x20;  6\.  Linux x86\_64, Intel compiler, SGI MPT  (dmpar)
>
> &#x20;  7\.  Linux x86\_64, gfortran compiler  (serial)
>
> &#x20;  8\.  Linux x86\_64, gfortran compiler  (dmpar)
>
> &#x20;  9\.  Linux x86\_64, Intel compiler, IBM POE  (serial)
>
> &#x20; 10\.  Linux x86\_64, Intel compiler, IBM POE  (dmpar)
>
> &#x20; 11\.  Linux x86\_64, gfortran compiler: -f90=gfortran  (serial)
>
> &#x20; 12\.  Linux x86\_64, gfortran compiler: -f90=gfortran  (dmpar)
>
> &#x20; 13\.  Linux x86\_64, PGI compiler: -f90=pgf90  (serial)
>
> &#x20; 14\.  Linux x86\_64, PGI compiler: -f90=pgf90  (dmpar)
>
>
>
> Enter selection \[1-14] : <mark style="color:blue;">4</mark>
>
> \-------------------------------------------------------------------------
>
> Configuration successful. To build the UPP, type: compile&#x20;
>
> \-------------------------------------------------------------------------



\[configure.upp 파일 수정 수정 내용]

> DM\_FC  = <mark style="color:blue;">mpiifort</mark>
>
> DM\_F90 = <mark style="color:blue;">mpiifort</mark> -free
>
> DM\_CC  = <mark style="color:blue;">mpiicc</mark>
>
> &#x20;
>
> FOPT    = -O3 <mark style="color:blue;">-fPIC -xCOMMON-AVX512</mark>
>
> CRTMOPT = -O3 <mark style="color:blue;">-fPIC -xCOMMON-AVX512</mark>
>
> COPT    = -O3 -fPIC <mark style="color:blue;">-xCOMMON-AVX512</mark>
>
> &#x20;
>
> GRIB2SUPT\_LIB   =    -L/apps/common/jasper/1.900.29/lib <mark style="color:blue;">-L/apps/common/libpng/1.2.56/lib</mark> -lpng -lz -ljasper
>
> GRIB2SUPT\_INC   =    -I/apps/common/jasper/1.900.29/include <mark style="color:blue;">-I/apps/common/libpng/1.2.56/include</mark>

※ 누리온 시스템 설치 예제는 SKL/KNL 계산노드에서 공통적으로 사용을 위해 "-xCOMMON-AVX512" 로 작성

&#x20;\- SKL(skylake) 노드 전용 : -xCORE-AVX512\
&#x20;\- KNL(Intel Xeon Phi Knights Landing) 전용 : -xMIC-AVX512\
&#x20;\- SKL 과 KNL 공통 적용 : -xCOMMON-AVX512\
&#x20;\- 참고 :  https://software.intel.com/en-us/articles/compiling-for-the-intel-xeon-phi-processor-and-the-intel-avx-512-isa
