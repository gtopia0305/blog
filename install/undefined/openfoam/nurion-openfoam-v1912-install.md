---
description: 슈퍼컴퓨팅인프라센터 2020. 6. 29. 16:04
---

# OpenFOAM-v1912 설치 소개

KISTI 슈퍼컴퓨팅센터의 누리온 시스템에 OpenFOAM-v1912 Source 버전으로 설치 하는 방법에 대하여 소개 한다.



## **1. 설치 환경**

|  **구분**     | **내용**                      |
| ----------- | --------------------------- |
|  대상 시스템     |  누리온                        |
| OS Version  |  리눅스 / CentOS 7.7           |
|  CPU        |  Intel(R) Xeon(R) Gold 6126 |
|  컴파일러       |  Intel 2018.3               |
|  MPI        |  IntelMPI 2018.3            |
|  기타         |                             |



## **2. 설치 전 환경 설정**

&#x20;OpenFOAM-v1912 버전 설치에 필요한 gmp, mpfr, mpc, boost, CGAL 는 누리온 시스템에 미리 설치된 /apps/common 라이브러리들을 사용한다.\
&#x20;만약 다른 버전의 gmp, mpfr, mpc, boost, CGAL  가 필요한 경우는 사용자의 홈 디렉토리(/home01/$USER)에 설치 후 환경설정을 해서 사용하면 된다.



\[ 환경 설정 ]

> &#x20;$ module load cmake/3.12.3
>
> &#x20;$ module load intel/18.0.3 impi/18.0.3



## **3. OpenFOAM-v1912 버전 설치 과정**

&#x20;설치 과정 소개는 tar 를 이용한 압축 해제 방법과 설정 방법등 진행 절차를 위주로 설명하고,\
&#x20;소스 파일 다운로드 등은 생략한다.&#x20;

|  **설치 과정**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| <p> $ cd <mark style="color:blue;"><strong>/scratch/{USER_ID}</strong></mark></p><p> $ mkdir OpenFOAM</p><p> $ cd OpenFOAM</p><p> $ tar -xzf OpenFOAM-v1912.tgz</p><p> $ tar -xzf ThirdParty-v1912.tgz</p><p></p><p> $ sed -i -e 's/\(boost_version=\)boost_1_64_0/\1boost-system/' OpenFOAM-v1912/etc/config.sh/CGAL</p><p> $ sed -i -e 's/\(cgal_version=\)CGAL-4.9.1/\1cgal-system/' OpenFOAM-v1912/etc/config.sh/CGAL</p><p> $ vi OpenFOAM-v1912/etc/bashrc </p><p>  <strong>- - - [bashrc 수정 사항] 참고 - - -</strong></p><p></p><p> $ mkdir -p OpenFOAM-v1912/platforms/linux64IccDPInt64Opt/bin</p><p> $ source <mark style="color:blue;"><strong>/scratch/{USER_ID}</strong></mark>/OpenFOAM/OpenFOAM-v1912/etc/bashrc</p><p> $ mkdir -p $WM_THIRD_PARTY_DIR/platforms/$WM_ARCH$WM_COMPILER</p><p></p><p> $ ln -s /apps/common/gmp/6.1.2 $WM_THIRD_PARTY_DIR/platforms/$WM_ARCH$WM_COMPILER/gmp-system</p><p> $ ln -s /apps/common/mpfr/4.0.1 $WM_THIRD_PARTY_DIR/platforms/$WM_ARCH$WM_COMPILER/mpfr-system</p><p> $ ln -s /apps/common/mpc/1.1.0 $WM_THIRD_PARTY_DIR/platforms/$WM_ARCH$WM_COMPILER/mpc-system</p><p> $ ln -s /apps/common/boost/1.68.0 $WM_THIRD_PARTY_DIR/platforms/$WM_ARCH$WM_COMPILER/boost-system</p><p> $ ln -s /apps/common/CGAL/4.9.1 $WM_THIRD_PARTY_DIR/platforms/$WM_ARCH$WM_COMPILER/cgal-system</p><p> $ ln -s /apps/applications/cmake/3.12.3 $WM_THIRD_PARTY_DIR/platforms/$WM_ARCH$WM_COMPILER/cmake-system</p><p></p><p>$ cd $WM_PROJECT_DIR </p><p>$ ./Allwmake</p> |



**\[bashrc 수정 사항]**&#x20;

> &#x20;61line : export WM\_COMPILER=Icc
>
> &#x20;69line : export WM\_LABEL\_SIZE=64
>
> &#x20;80line : export WM\_MPLIB=INTELMPI



## **4. 테스트**

> &#x20;$ module load intel/18.0.3 impi/18.0.3
>
> &#x20;$ source <mark style="color:blue;">**/scratch/{USER\_ID)**</mark>/OpenFOAM/OpenFOAM-v1912/etc/bashrc&#x20;
>
> \
>
>
> &#x20;$ mkdir -p $FOAM\_RUN&#x20;
>
> &#x20;$ run&#x20;
>
> &#x20;$ cp -r $FOAM\_TUTORIALS/incompressible/simpleFoam/pitzDaily .
>
> &#x20;$ cd pitzDaily&#x20;
>
> &#x20;$ blockMesh&#x20;
>
> &#x20;$ simpleFoam&#x20;

<mark style="color:blue;">※ 위 예제에서</mark> <mark style="color:blue;"></mark><mark style="color:blue;">**/scratch/{USER\_ID}**</mark> <mark style="color:blue;"></mark><mark style="color:blue;">경로는 설치 테스트 시 사용된 경로이므로 변경 후 사용</mark>
