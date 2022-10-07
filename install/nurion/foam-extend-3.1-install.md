---
description: 슈퍼컴퓨팅인프라센터 2020. 9. 11. 16:47
---

# foam-extend-3.1 설치 소개

KISTI 슈퍼컴퓨팅센터의 누리온 시스템에  foam-extend-3.1 Source 버전으로 설치 하는 방법에 대하여 소개 한다.

&#x20;

## **1. 설치 환경**

|  **구분**      | **내용**                      |
| ------------ | --------------------------- |
| 대상 시스템       | 누리온                         |
|  OS Version  |  리눅스 / CentOS 7.7           |
|  CPU         |  Intel(R) Xeon(R) Gold 6126 |
|  컴파일러        |  Gcc 7.2.0                  |
|  MPI         |  OpenMPI 3.1.0              |
|  기타          |                             |



## **2. 설치 전 환경 설정**

누리온 시스템은 PATH, LD\_LIBRARY\_PATH 등을 쉽게 설정하기 위하여 \
환경설정 툴인 Modules(http://modules.sourceforge.net)이 구성되어 있고,\
이하 설치 소개 에서는 module load를 이용한 환경 설정 방법을 이용한다.&#x20;



\[ 환경 설정 ]

> &#x20;$ module load gcc/7.2.0 openmpi/3.1.0
>
> &#x20;$ module load cmake/3.12.3 subversion/1.9.3 git/1.8.3.4

## **3. foam-extend-3.1 버전 설치 과정**

&#x20;설치 과정 소개는 tar 를 이용한 압축 해제 방법과 설정 방법등 진행 절차를 위주로 설명하고,\
&#x20;소스 파일 다운로드 등은 생략한다.  &#x20;

|  **설치 과정**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p> $ mkdir -p ~/foam/foam-extend-3.1</p><p> $ mv foam-extend-3.1_f77b480.tgz ~/foam/foam-extend-3.1</p><p> $ cd ~/foam/foam-extend-3.1</p><p> $ tar -xvzf foam-extend-3.1_f77b480.tgz</p><p> </p><p> $ source ~/foam/foam-extend-3.1/etc/bashrc</p><p> $ foam</p><p> </p><p> $ sed -iold -e "s=\-liberty==" wmake/rules/*/general</p><p> </p><p> $ vi ThirdParty/AllMake.stage5</p><p>  <strong>- - - [AllMake.stage5 수정 사항] 참고 - - -</strong></p><p> </p><p> $ ./Allwmake.firstInstall 2>&#x26;1 | tee make.log</p><p> </p><p> Proceed without compiling ParaView [Y/n] y </p><p> </p> |



**\[AllMake.stage5 수정 사항]**&#x20;

> &#x20;90line : (cd ./rpmBuild/BUILD; svn checkout <mark style="color:blue;">http</mark>://svn.code.sf.net/p/openfoam-extend/svn/trunk/Breeder\_2.0/libraries/swak4Foam swak4Foam-$SWAK\_RELEASE\_VERSION)

