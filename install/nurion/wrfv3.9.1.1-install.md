---
description: 슈퍼컴퓨팅인프라센터 2019. 5. 28. 10:47
---

# WRFV3.9.1.1 설치

KISTI 슈퍼컴퓨팅센터의 누리온 시스템에 WRFV3.9.1.1 Source 버전과 WPSV3.9.1 Source 버전을 설치 하는 방법에 대하여 소개한다.



## **1. 설치 환경**

|  **구분**     | **내용**                      |
| ----------- | --------------------------- |
|  대상 시스템     |  누리온                        |
| OS Version  |  리눅스 / CentOS 7.3           |
|  CPU        |  Intel(R) Xeon(R) Gold 6126 |
|  컴파일러       |  Intel 2018.3 Version       |
|  MPI        |  IntelMPI 2018.3 Version    |
|  기타         |                             |



## **2. 설치 전 환경 설정**

&#x20; 누리온 시스템은 PATH, LD\_LIBRARY\_PATH 등을 쉽게 하기 위하여 \
&#x20; 환경설정 툴인 Modules(http://modules.sourceforge.net)이 구성되어 있고,\
&#x20; 이하 설치 소개 에서는 module load를 이용한 환경 설정 방법을 이용한다.

{% code title="[ 환경 설정 ]" %}
```
 $ module load craype-mic-knl intel/18.0.3 impi/18.0.3
 $ module load hdf4/4.2.13 hdf5/1.10.2 netcdf/4.6.1 ncl/6.5.0
 $ export JASPERLIB=/apps/common/jasper/1.900.29/lib
 $ export JASPERINC=/apps/common/jasper/1.900.29/include
```
{% endcode %}

## **3. WRF 설치 과정**

&#x20;설치 과정 소개는 tar 를 이용한 압축 해제 방법과 설정 방법등 진행 절차를 위주로 설명하고, 소스 파일 다운로드 등은 생략한다. &#x20;

```
$ tar xvzf WRFV3.9.1.1.TAR.gz
$ cd WRFV3
$ ./configure 
 - - - - - 아래 [configure 과정 진행 내용] 참고
$ vi configure.wrf
 - - - - - 아래 [configure.wrf 파일 수정 내용] 참고
$ ./compile em_real
```

※ WRFV3.9.1.1 버전의 경우 "compile em\_real" "-j 2" 옵션이 기본으로 사용되어 "compile -j 2 em\_real" 으로 빌드 되고, **CentOS 7.3 버전에서 빌드 시 컴파일 오류가 1회 발생함. 이 경우 "compile em\_real" 과정을 한번더 진행.**

{% code title="[configure 과정 진행 내용]" %}
```
$ ./configure 
checking for perl5... no
checking for perl... found /usr/bin/perl (perl)
Will use NETCDF in dir: /apps/compiler/intel/18.0.3/applib1/mic-knl/netcdf/4.6.1
Will use HDF5 in dir: /apps/compiler/intel/18.0.3/applib1/mic-knl/hdf5/1.10.2
PHDF5 not set in environment. Will configure WRF for use without.
Will use 'time' to report timing information


If you REALLY want Grib2 output from WRF, modify the arch/Config_new.pl script.
Right now you are not getting the Jasper lib, from the environment, compiled into WRF.

------------------------------------------------------------------------
Please select from among the following Linux x86_64 options:

  1. (serial)   2. (smpar)   3. (dmpar)   4. (dm+sm)   PGI (pgf90/gcc)
  5. (serial)   6. (smpar)   7. (dmpar)   8. (dm+sm)   PGI (pgf90/pgcc): SGI MPT
  9. (serial)  10. (smpar)  11. (dmpar)  12. (dm+sm)   PGI (pgf90/gcc): PGI accelerator
 13. (serial)  14. (smpar)  15. (dmpar)  16. (dm+sm)   INTEL (ifort/icc)
                                         17. (dm+sm)   INTEL (ifort/icc): Xeon Phi (MIC architecture)
 18. (serial)  19. (smpar)  20. (dmpar)  21. (dm+sm)   INTEL (ifort/icc): Xeon (SNB with AVX mods)
 22. (serial)  23. (smpar)  24. (dmpar)  25. (dm+sm)   INTEL (ifort/icc): SGI MPT
 26. (serial)  27. (smpar)  28. (dmpar)  29. (dm+sm)   INTEL (ifort/icc): IBM POE
 30. (serial)               31. (dmpar)                PATHSCALE (pathf90/pathcc)
 32. (serial)  33. (smpar)  34. (dmpar)  35. (dm+sm)   GNU (gfortran/gcc)
 36. (serial)  37. (smpar)  38. (dmpar)  39. (dm+sm)   IBM (xlf90_r/cc_r)
 40. (serial)  41. (smpar)  42. (dmpar)  43. (dm+sm)   PGI (ftn/gcc): Cray XC CLE
 44. (serial)  45. (smpar)  46. (dmpar)  47. (dm+sm)   CRAY CCE (ftn $(NOOMP)/cc): Cray XE and XC
 48. (serial)  49. (smpar)  50. (dmpar)  51. (dm+sm)   INTEL (ftn/icc): Cray XC
 52. (serial)  53. (smpar)  54. (dmpar)  55. (dm+sm)   PGI (pgf90/pgcc)
 56. (serial)  57. (smpar)  58. (dmpar)  59. (dm+sm)   PGI (pgf90/gcc): -f90=pgf90
 60. (serial)  61. (smpar)  62. (dmpar)  63. (dm+sm)   PGI (pgf90/pgcc): -f90=pgf90
 64. (serial)  65. (smpar)  66. (dmpar)  67. (dm+sm)   INTEL (ifort/icc): HSW/BDW
 68. (serial)  69. (smpar)  70. (dmpar)  71. (dm+sm)   INTEL (ifort/icc): KNL MIC
 72. (serial)  73. (smpar)  74. (dmpar)  75. (dm+sm)   FUJITSU (frtpx/fccpx): FX10/FX100 SPARC64 IXfx/Xlfx

Enter selection [1-75] : 70
------------------------------------------------------------------------
Compile for nesting? (1=basic, 2=preset moves, 3=vortex following) [default 1]: 1

Configuration successful! 
------------------------------------------------------------------------
testing for MPI_Comm_f2c and MPI_Comm_c2f
   MPI_Comm_f2c and MPI_Comm_c2f are supported
testing for fseeko and fseeko64
fseeko64 is supported
------------------------------------------------------------------------

# Settings for    Linux KNL x86_64 ppc64le i486 i586 i686  (dmpar)
#
DESCRIPTION     =       INTEL ($SFC/$SCC): KNL MIC
DMPARALLEL      =        1
OMPCPP          =       # -D_OPENMP
OMP             =       # -openmp -fpp -auto
OMPCC           =       # -openmp -fpp -auto
SFC             =       ifort
SCC             =       icc
CCOMP           =       icc
DM_FC           =       mpif90 -f90=$(SFC)
DM_CC           =       mpicc -cc=$(SCC) -DMPI2_SUPPORT
FC              =       time $(DM_FC)
CC              =       $(DM_CC) -DFSEEKO64_OK 
LD              =       $(FC)
RWORDSIZE       =       $(NATIVE_RWORDSIZE)
PROMOTION       =       -real-size `expr 8 \* $(RWORDSIZE)` -i4
ARCH_LOCAL      =       -DNONSTANDARD_SYSTEM_FUNC -DWRF_USE_CLM
CFLAGS_LOCAL    =       -w -O3 -ip -fp-model fast=2 -no-prec-div -no-prec-sqrt -ftz -no-multibyte-chars -xMIC-AVX512
LDFLAGS_LOCAL   =       -ip -fp-model fast=2 -no-prec-div -no-prec-sqrt -ftz -align all -fno-alias -fno-common -xMIC-AVX512
CPLUSPLUSLIB    =       
ESMF_LDFLAG     =       $(CPLUSPLUSLIB)
FCOPTIM         =       -O3
FCREDUCEDOPT =       $(FCOPTIM)
FCNOOPT =       -O0 -fno-inline -no-ip
FCDEBUG         =       # -g $(FCNOOPT) -traceback # -fpe0 -check noarg_temp_created,bounds,format,output_conversion,pointers,uninit -ftrapuv -unroll0 -u
FORMAT_FIXED    =       -FI
FORMAT_FREE     =       -FR
FCSUFFIX        =
BYTESWAPIO      =       -convert big_endian
RECORDLENGTH    =       -assume byterecl
FCBASEOPTS_NO_G =       -ip -fp-model precise -w -ftz -align all -fno-alias $(FORMAT_FREE) $(BYTESWAPIO) -fp-model fast=2 -no-heap-arrays -no-prec-div -no-prec-sqrt -fno-common -xMIC-AVX512
FCBASEOPTS      =       $(FCBASEOPTS_NO_G) $(FCDEBUG)
MODULE_SRCH_FLAG =     
TRADFLAG        =      -traditional-cpp
CPP             =      /lib/cpp -P -nostdinc
AR              =      ar
ARFLAGS         =      ru
M4              =      m4
RANLIB          =      ranlib
RLFLAGS =
CC_TOOLS        =      $(SCC) 

#insert new stanza here

###########################################################
######################
# POSTAMBLE

FGREP = fgrep -iq

ARCHFLAGS       =    $(COREDEFS) -DIWORDSIZE=$(IWORDSIZE) -DDWORDSIZE=$(DWORDSIZE) -DRWORDSIZE=$(RWORDSIZE) -DLWORDSIZE=$(LWORDSIZE) \
                     $(ARCH_LOCAL) \
                     $(DA_ARCHFLAGS) \
                      -DDM_PARALLEL \
                       \
                      -DNETCDF \
                       \
                       \
                       \
                       \
                       \
                      -DHDF5 \
                       \
                       \
                       \
                       \
                      -DUSE_ALLOCATABLES \
                      -DGRIB1 \
                      -DINTIO \
                      -DKEEP_INT_AROUND \
                      -DLIMIT_ARGS \
                      -DCONFIG_BUF_LEN=$(CONFIG_BUF_LEN) \
                      -DMAX_DOMAINS_F=$(MAX_DOMAINS) \
                      -DMAX_HISTORY=$(MAX_HISTORY) \
      -DNMM_NEST=$(WRF_NMM_NEST)
CFLAGS          =    $(CFLAGS_LOCAL) -DDM_PARALLEL  \
                      -DMAX_HISTORY=$(MAX_HISTORY) -DNMM_CORE=$(WRF_NMM_CORE)
FCFLAGS         =    $(FCOPTIM) $(FCBASEOPTS)
ESMF_LIB_FLAGS  =    
# ESMF 5 -- these are defined in esmf.mk, included above
 ESMF_IO_LIB     =    -L$(WRF_SRC_ROOT_DIR)/external/esmf_time_f90 -lesmf_time
ESMF_IO_LIB_EXT =    -L$(WRF_SRC_ROOT_DIR)/external/esmf_time_f90 -lesmf_time
INCLUDE_MODULES =    $(MODULE_SRCH_FLAG) \
                     $(ESMF_MOD_INC) $(ESMF_LIB_FLAGS) \
                      -I$(WRF_SRC_ROOT_DIR)/main \
                      -I$(WRF_SRC_ROOT_DIR)/external/io_netcdf \
                      -I$(WRF_SRC_ROOT_DIR)/external/io_int \
                      -I$(WRF_SRC_ROOT_DIR)/frame \
                      -I$(WRF_SRC_ROOT_DIR)/share \
                      -I$(WRF_SRC_ROOT_DIR)/phys \
                      -I$(WRF_SRC_ROOT_DIR)/chem -I$(WRF_SRC_ROOT_DIR)/inc \
                      -I$(NETCDFPATH)/include \
                      
REGISTRY        =    Registry
CC_TOOLS_CFLAGS = -DNMM_CORE=$(WRF_NMM_CORE)

 LIB_BUNDLED     = \
                      $(WRF_SRC_ROOT_DIR)/external/fftpack/fftpack5/libfftpack.a \
                      $(WRF_SRC_ROOT_DIR)/external/io_grib1/libio_grib1.a \
                      $(WRF_SRC_ROOT_DIR)/external/io_grib_share/libio_grib_share.a \
                      $(WRF_SRC_ROOT_DIR)/external/io_int/libwrfio_int.a \
                      $(ESMF_IO_LIB) \
                      $(WRF_SRC_ROOT_DIR)/external/RSL_LITE/librsl_lite.a \
                      $(WRF_SRC_ROOT_DIR)/frame/module_internal_header_util.o \
                      $(WRF_SRC_ROOT_DIR)/frame/pack_utils.o 

 LIB_EXTERNAL    = \
                      -L$(WRF_SRC_ROOT_DIR)/external/io_netcdf -lwrfio_nf -L/apps/compiler/intel/18.0.3/applib1/mic-knl/netcdf/4.6.1/lib -lnetcdff -lnetcdf     -L/apps/compiler/intel/18.0.3/applib1/mic-knl/hdf5/1.10.2/lib -lhdf5_fortran -lhdf5 -lm -lz


LIB             =    $(LIB_BUNDLED) $(LIB_EXTERNAL) $(LIB_LOCAL) $(LIB_WRF_HYDRO)
LDFLAGS         =    $(OMP) $(FCFLAGS) $(LDFLAGS_LOCAL) 
ENVCOMPDEFS     =    
WRF_CHEM = 0 
CPPFLAGS        =    $(ARCHFLAGS) $(ENVCOMPDEFS) -I$(LIBINCLUDE) $(TRADFLAG) 
NETCDFPATH      =    /apps/compiler/intel/18.0.3/applib1/mic-knl/netcdf/4.6.1
HDF5PATH        =    /apps/compiler/intel/18.0.3/applib1/mic-knl/hdf5/1.10.2
WRFPLUSPATH     =    
RTTOVPATH       =    
PNETCDFPATH     =    

bundled:  io_only 
external: io_only $(WRF_SRC_ROOT_DIR)/external/RSL_LITE/librsl_lite.a gen_comms_rsllite module_dm_rsllite $(ESMF_TARGET)
io_only:  esmf_time wrfio_nf   \
  wrf_ioapi_includes wrfio_grib_share wrfio_grib1 wrfio_int fftpack


######################
------------------------------------------------------------------------
Settings listed above are written to configure.wrf.
If you wish to change settings, please edit that file.
If you wish to change the default options, edit the file:
     arch/configure_new.defaults
NetCDF users note:
 This installation of NetCDF supports large file support.  To DISABLE large file
 support in NetCDF, set the environment variable WRFIO_NCD_NO_LARGE_FILE_SUPPORT
 to 1 and run configure again. Set to any other value to avoid this message.
  

Testing for NetCDF, C and Fortran compiler

This installation of NetCDF is 64-bit
                 C compiler is 64-bit
           Fortran compiler is 64-bit
              It will build in 64-bit
```
{% endcode %}

{% code title="[configure.wrf 파일 수정 내용]" %}
```
DM_FC           =       mpiifort -f90=$(SFC)
DM_CC           =       mpiicc -cc=$(SCC) -DMPI2_SUPPORT
CFLAGS_LOCAL    =       -w -O3 -ip -fp-model fast=2 -no-prec-div -no-prec-sqrt -ftz -no-multibyte-chars -xCOMMON-AVX512
LDFLAGS_LOCAL   =       -ip -fp-model fast=2 -no-prec-div -no-prec-sqrt -ftz -align all -fno-alias -fno-common -xCOMMON-AVX512
FCBASEOPTS_NO_G =       -ip -fp-model precise -w -ftz -align all -fno-alias $(FORMAT_FREE) $(BYTESWAPIO) -fp-model fast=2 -no-heap-arrays -no-prec-div -no-prec-sqrt -fno-common -xCOMMON-AVX512
```
{% endcode %}

※ 설치를 진행한 CPU 타입과 계산을 수행할 CPU 타입이 다른 경우는 꼭 configure.wrf 파일의 "<mark style="color:red;">**-xHost**</mark><mark style="color:red;">"</mark> <mark style="color:red;"></mark><mark style="color:red;">**옵션을 삭제**</mark>하고 빌드를 진행한다.    (예 : login 노드(SKL CPU 타입)에서 설치를 진행 하고 KNL 계산노드 에서 수행하는 경우)

※ 누리온 시스템 설치 예제는 SKL/KNL 계산노드에서 공통적으로 사용을 위해 "-xCOMMON-AVX512" 로 작성

&#x20;\- SKL(skylake) 노드 전용 : -xCORE-AVX512\
&#x20;\- KNL(Intel Xeon Phi Knights Landing) 전용 : -xMIC-AVX512\
&#x20;\- SKL 과 KNL 공통 적용 : -xCOMMON-AVX512\
&#x20;\- 참고 :  https://software.intel.com/en-us/articles/compiling-for-the-intel-xeon-phi-processor-and-the-intel-avx-512-isa



## **4. WPS 설치 과정**

&#x20;설치 과정 소개는 tar 를 이용한 압축 해제 방법과 설정 방법등 진행 절차를 위주로 설명하고, 소스 파일 다운로드 등은 생략한다. &#x20;

{% code title="설치과정" %}
```
$ tar xvzf WPSV3.9.1.TAR.gz
$ cd WPS
$ ./configure 
 - - - - - 아래 [configure 과정 진행 내용] 참고
$ vi configure.wps
 - - - - - 아래 [configure.wps 파일 수정 내용] 참고
$ ./compile 
```
{% endcode %}

{% code title="[configure 과정 진행 내용]" %}
```
$ ./configure 
Will use NETCDF in dir: /apps/compiler/intel/18.0.3/applib1/mic-knl/netcdf/4.6.1
Found Jasper environment variables for GRIB2 support...
  $JASPERLIB = /apps/common/jasper/1.900.29/lib
  $JASPERINC = /apps/common/jasper/1.900.29/include
------------------------------------------------------------------------
Please select from among the following supported platforms.

   1.  Linux x86_64, gfortran    (serial)
   2.  Linux x86_64, gfortran    (serial_NO_GRIB2)
   3.  Linux x86_64, gfortran    (dmpar)
   4.  Linux x86_64, gfortran    (dmpar_NO_GRIB2)
   5.  Linux x86_64, PGI compiler   (serial)
   6.  Linux x86_64, PGI compiler   (serial_NO_GRIB2)
   7.  Linux x86_64, PGI compiler   (dmpar)
   8.  Linux x86_64, PGI compiler   (dmpar_NO_GRIB2)
   9.  Linux x86_64, PGI compiler, SGI MPT   (serial)
  10.  Linux x86_64, PGI compiler, SGI MPT   (serial_NO_GRIB2)
  11.  Linux x86_64, PGI compiler, SGI MPT   (dmpar)
  12.  Linux x86_64, PGI compiler, SGI MPT   (dmpar_NO_GRIB2)
  13.  Linux x86_64, IA64 and Opteron    (serial)
  14.  Linux x86_64, IA64 and Opteron    (serial_NO_GRIB2)
  15.  Linux x86_64, IA64 and Opteron    (dmpar)
  16.  Linux x86_64, IA64 and Opteron    (dmpar_NO_GRIB2)
  17.  Linux x86_64, Intel compiler    (serial)
  18.  Linux x86_64, Intel compiler    (serial_NO_GRIB2)
  19.  Linux x86_64, Intel compiler    (dmpar)
  20.  Linux x86_64, Intel compiler    (dmpar_NO_GRIB2)
  21.  Linux x86_64, Intel compiler, SGI MPT    (serial)
  22.  Linux x86_64, Intel compiler, SGI MPT    (serial_NO_GRIB2)
  23.  Linux x86_64, Intel compiler, SGI MPT    (dmpar)
  24.  Linux x86_64, Intel compiler, SGI MPT    (dmpar_NO_GRIB2)
  25.  Linux x86_64, Intel compiler, IBM POE    (serial)
  26.  Linux x86_64, Intel compiler, IBM POE    (serial_NO_GRIB2)
  27.  Linux x86_64, Intel compiler, IBM POE    (dmpar)
  28.  Linux x86_64, Intel compiler, IBM POE    (dmpar_NO_GRIB2)
  29.  Linux x86_64 g95 compiler     (serial)
  30.  Linux x86_64 g95 compiler     (serial_NO_GRIB2)
  31.  Linux x86_64 g95 compiler     (dmpar)
  32.  Linux x86_64 g95 compiler     (dmpar_NO_GRIB2)
  33.  Cray XE/XC CLE/Linux x86_64, Cray compiler   (serial)
  34.  Cray XE/XC CLE/Linux x86_64, Cray compiler   (serial_NO_GRIB2)
  35.  Cray XE/XC CLE/Linux x86_64, Cray compiler   (dmpar)
  36.  Cray XE/XC CLE/Linux x86_64, Cray compiler   (dmpar_NO_GRIB2)
  37.  Cray XC CLE/Linux x86_64, Intel compiler   (serial)
  38.  Cray XC CLE/Linux x86_64, Intel compiler   (serial_NO_GRIB2)
  39.  Cray XC CLE/Linux x86_64, Intel compiler   (dmpar)
  40.  Cray XC CLE/Linux x86_64, Intel compiler   (dmpar_NO_GRIB2)

Enter selection [1-40] : 19
------------------------------------------------------------------------
Configuration successful. To build the WPS, type: compile
------------------------------------------------------------------------

Testing for NetCDF, C and Fortran compiler

This installation NetCDF is 64-bit
C compiler is 64-bit
Fortran compiler is 64-bit
```
{% endcode %}

{% code title="[configure.wps 파일 수정 내용]" %}
```
DM_FC               = mpiifort -f90=ifort
DM_CC               = mpiicc -cc=icc
FFLAGS              = -FR -convert big_endian -O2 -xCOMMON-AVX512
F77FLAGS            = -FI -convert big_endian -O2 -xCOMMON-AVX512
CFLAGS              = -w -O2 -xCOMMON-AVX512
```
{% endcode %}

## **5. 기타**

&#x20;만약 WPS를 SKL CPU 노드(login노드, norm\_skl큐)에서 실행하는 경우에는 \
&#x20;craype-mic-knl 모듈 대신 craype-x86-skylake 모듈을 사용 하도록 환경설정을 해야 한다.

{% code title="[ 환경 설정 ]" %}
```
 $ module load craype-x86-skylake intel/18.0.3 impi/18.0.3
 $ module load hdf4/4.2.13 hdf5/1.10.2 netcdf/4.6.1 ncl/6.5.0
 $ export JASPERLIB=/apps/common/jasper/1.900.29/lib
 $ export JASPERINC=/apps/common/jasper/1.900.29/include
```
{% endcode %}
