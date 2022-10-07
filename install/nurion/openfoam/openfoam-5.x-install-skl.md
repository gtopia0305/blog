---
description: 슈퍼컴퓨팅인프라센터 2019. 6. 27. 11:33
---

# OpenFOAM-5.x 버전 설치 소개(SKL)

KISTI 슈퍼컴퓨팅센터의 누리온 시스템에 OpenFOAM-v5.0 Source 버전으로 공용 파일시스템인 /apps에 설치했던 내용을 정리하여 사용자들이 설치하는 방법에 대하여 참고할 수 있도록 내용을 소개 한다.



## **1. 설치 환경**

|  **구분**     | **내용**                      |
| ----------- | --------------------------- |
|  대상 시스템     |  누리온                        |
| OS Version  |  리눅스 / CentOS 7.3           |
|  CPU        |  Intel(R) Xeon(R) Gold 6126 |
|  컴파일러       |  Intel 2018.3               |
|  MPI        |  IntelMPI 2018.3            |
|  기타         |                             |



## **2. 설치 전 환경 설정**

&#x20;OpenFOAM-v5.x 버전 설치에 필요한 gmp, mpfr, mpc, boost, CGAL 는 누리온 시스템에 미리 설치된 /apps/common 라이브러리들을 사용한다.\
&#x20;만약 다른 버전의 gmp, mpfr, mpc, boost, CGAL  가 필요한 경우는 사용자의 홈 디렉토리(/home01/$USER)에 설치 후 환경설정을 해서 사용하면 된다.

{% code title="[ 환경 설정 ]" %}
```
 $ module load cmake/3.12.3
 $ module load intel/18.0.3 impi/18.0.3
```
{% endcode %}

## **3. OpenFOAM-v5.0 버전 설치 과정**

&#x20;설치 과정 소개는 tar 를 이용한 압축 해제 방법과 설정 방법등 진행 절차를 위주로 설명하고,

&#x20;소스 파일 다운로드 등은 생략한다.   설치 소개 시 사용된 경로 <mark style="color:blue;">${HOME}/OpenFOAM-5.0/SKL/</mark> 는 설치 안내를 위한 경로이므로, 사용자는 실제 사용할 경로를 지정하여 설치하면 된다. \


{% code title=" 설치 과정" %}
```
 $ cd ${HOME}/OpenFOAM-5.0/SKL/
 $ mkdir OpenFOAM
 $ cd OpenFOAM
 $ tar -xzf OpenFOAM-5.x-version-5.0.tar.gz 
 $ tar -xzf ThirdParty-5.x-version-5.0.tar.gz
 $ mv OpenFOAM-5.x-version-5.0 OpenFOAM-5.0
 $ mv ThirdParty-5.x-version-5.0 ThirdParty-5.0
 $ vi OpenFOAM-5.0/etc/config.sh/settings 
  - - - [settings 수정 사항] 참고 - - -
 $ vi OpenFOAM-5.0/etc/bashrc 
  - - - [bashrc 수정 사항] 참고 - - -
 $ vi ThirdParty-5.0/makeCGAL
  - - - [makeCGAL 수정 사항] 참고 - - -
 $ vi OpenFOAM-5.0/etc/config.sh/mpi
  - - - [mpi 수정 사항] 참고 - - -
 $ vi OpenFOAM-5.0/wmake/rules/linux64Icc/c++ 
  - - - [c++ 수정 사항] 참고 - - - 
$ vi ThirdParty-5.0/scotch_6.0.3/src/Make.inc/Makefile.inc.x86-64_pc_linux2.icc
  - - - [Makefile.inc.x86-64_pc_linux2.icc 수정 사항] 참고 - - -
 $ vi ThirdParty-5.0/etc/wmakeFiles/scotch/Makefile.inc.i686_pc_linux2.shlib-OpenFOAM
  - - - [Makefile.inc.i686_pc_linux2.shlib-OpenFOAM 수정 사항] 참고 - - -
 $ sed -i -e 's/\(boost_version=\)boost-system/\1boost_1_68_0/' OpenFOAM-5.0/etc/config.sh/CGAL
 $ sed -i -e 's/\(cgal_version=\)cgal-system/\1CGAL-4.9.1/' OpenFOAM-5.0/etc/config.sh/CGAL
 $ source OpenFOAM-5.0/etc/bashrc 
 $ mkdir -p $WM_THIRD_PARTY_DIR/platforms/$WM_ARCH$WM_COMPILER
 $ ln -s /apps/common/gmp/6.1.2          $WM_THIRD_PARTY_DIR/platforms/$WM_ARCH$WM_COMPILER/gmp-system
 $ ln -s /apps/common/mpfr/4.0.1         $WM_THIRD_PARTY_DIR/platforms/$WM_ARCH$WM_COMPILER/mpfr-system
 $ ln -s /apps/common/mpc/1.1.0          $WM_THIRD_PARTY_DIR/platforms/$WM_ARCH$WM_COMPILER/mpc-system
 $ ln -s /apps/common/boost/1.68.0       $WM_THIRD_PARTY_DIR/platforms/$WM_ARCH$WM_COMPILER/boost_1_68_0
 $ ln -s /apps/common/CGAL/4.9.1         $WM_THIRD_PARTY_DIR/platforms/$WM_ARCH$WM_COMPILER/CGAL-4.9.1
 $ ln -s /apps/applications/cmake/3.12.3  $WM_THIRD_PARTY_DIR/platforms/$WM_ARCH$WM_COMPILER/cmake-system


 $ cd ThirdParty-5.0
 $ ./Allwmake

 $ cd $WM_PROJECT_DIR
 $ ./Allwmake
```
{% endcode %}



**\[settings 수정 사항]**&#x20;

> &#x20;38line :      WM\_ARCH=linux64
>
> &#x20;61line :         64)
>
> &#x20;62line :             WM\_ARCH=linux64
>
> &#x20;63line :             export WM\_COMPILER\_LIB\_ARCH=64
>
> &#x20;64line :             export WM\_CC='icc'
>
> &#x20;65line :             export WM\_CXX='icpc'
>
> &#x20;66line :             export WM\_CFLAGS='-m64 -fPIC <mark style="color:blue;">-xCORE-AVX512</mark>'
>
> &#x20;67line :             export WM\_CXXFLAGS='-m64 -fPIC <mark style="color:blue;">-xCORE-AVX512</mark> -std=c++0x'
>
> &#x20;68line :             export WM\_LDFLAGS='-m64 -fPIC <mark style="color:blue;">-xCORE-AVX512</mark>'

※ "<mark style="color:blue;">-xCORE-AVX512</mark>" 옵션은 <mark style="color:blue;">SKL CPU 타입 전용 옵션</mark>으로, \
&#x20;SKL 계산노드와 KNL 계산노드 모두 실행을 희망하는 경우는 "-<mark style="color:blue;">xCOMMON-AVX512</mark>" 로 변경해서 사용



**\[bashrc 수정 사항]**&#x20;

> &#x20;47line : #export FOAM\_INST\_DIR=$HOME/$WM\_PROJECT
>
> &#x20;50line : export FOAM\_INST\_DIR=<mark style="color:blue;">${HOME}/OpenFOAM-5.0/SKL</mark>/$WM\_PROJECT
>
> &#x20;65line : export WM\_COMPILER=Icc&#x20;
>
> &#x20;89line : export WM\_MPLIB=INTELMPI

※ 위 수정 예제에서 **** <mark style="color:blue;">${HOME}/OpenFOAM-5.0/SKL</mark> **** 경로는 설치 시 사용된 경로이므로 변경 후 사용



**\[makeCGAL 수정 사항]**&#x20;

> &#x20;46line : cgalPACKAGE=${cgal\_version:-CGAL-4.8.1}
>
> &#x20;47line : boostPACKAGE=${boost\_version:-boost\_1\_68\_0}



**\[mpi 수정 사항]**&#x20;

> &#x20;46line :     libDir=\`mpiicc --showme:link | sed -e 's/.\*-L\\(\[^ ]\*\\).\*/\1/'\`



**\[c++ 수정 사항]**&#x20;

> &#x20;9line : CC          = icpc -std=c++11 -<mark style="color:blue;">xCORE-AVX512</mark> -fp-trap=common -fp-model precise



**\[Makefile.inc.x86-64\_pc\_linux2.icc 수정 사항]**&#x20;

> &#x20;10line : CCP             = mpiicc
>
> &#x20;12line : CFLAGS          = $(WM\_CFLAGS) -O3 -DCOMMON\_FILE\_COMPRESS\_GZ -DCOMMON\_PTHREAD -DCOMMON\_RANDOM\_FIXED\_SEED -DSCOTCH\_RENAME -DSCOTCH\_PTHREAD -restrict -DIDXSIZE64



**\[Makefile.inc.i686\_pc\_linux2.shlib-OpenFOAM 수정 사항]**&#x20;

> &#x20; 6line : AR              = icc
>
> &#x20; 9line : CCS             = icc
>
> &#x20;10line : CCP             = mpiicc
>
> &#x20;11line : CCD             = mpiicc

## **4. 테스트**

```
 $ module load intel/18.0.3 impi/18.0.3
 $ source ${HOME}/OpenFOAM-5.0/SKL/OpenFOAM/OpenFOAM-5.0/etc/bashrc 
 $ mkdir -p $FOAM_RUN 
 $ run 
 $ cp -r $FOAM_TUTORIALS/incompressible/simpleFoam/pitzDaily .
 $ cd pitzDaily 
 $ blockMesh 
 $ simpleFoam 
```

