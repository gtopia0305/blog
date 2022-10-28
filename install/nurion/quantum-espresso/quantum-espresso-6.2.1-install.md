---
description: 슈퍼컴퓨팅인프라센터 2019. 1. 10. 11:23
---

# Quantum Espresso-6.2.1설치 소개

KISTI 슈퍼컴퓨터센터의 장비에 espresso 6.2.1 source 버전으로 설치 하는 방법에 대하여 소개 한다.

## **1. 설치 환경**

|   **구분**       | **내용**                     |
| -------------- | -------------------------- |
|  대상 시스템        | 누리온                        |
|  OS Version    | 리눅스 / CentOS 7.3           |
|  CPU           | Intel(R) Xeon(R) Gold 6126 |
|  컴파일러          | Intel 2018.3 Version       |
|  MPI           | IntelMPI 2018.3 Version    |
| <p> 기타<br></p> | Intel MKL Math Library     |

## **2. 설치 전 환경 설정**

KISTI 시스템은 PATH, LD\_LIBRARY\_PATH 등을 쉽게 하기 위하여 OpenSource 인 Environment Modules(http://modules.sourceforge.net)이 구성되어 있고, 이하 설치 소개 에서는 module load를 이용한 환경 설정 방법을 이용한다.

{% code title="[ 환경 설정 ]" %}
```
 $ module load intel/18.0.3 impi/18.0.3
```
{% endcode %}

## **3. 설치 과정**

&#x20;설치 과정 소개는 소스 파일 다운로드 등은 생략하고, 설정 방법등 진행 절차를 위주로 설명한다. &#x20;

{% code title="  설치과정" %}
```
 $ tar xvf qe-6.2.1.tar.gz 
 $ cd qe-6.2.1
 $ ./configure --prefix=${HOME}/QE/6.2.1 \
 CC=mpiicc F90=mpiifort FC=mpiifort MPIF90=mpiifort \
 CFLAGS="-O3 -fPIC -xCOMMON-AVX512" \
 FFLAGS="-O3 -fPIC -xCOMMON-AVX512"
 $ make all
 $ make install  
```
{% endcode %}

<mark style="color:red;">**※ 위 설치 과정에서 파란색으로 표기된 설치 경로(${HOME}/QE/6.2.1)는 예제 이다. 실제 사용하는 위치로 변경해서 사용 해야 한다.**</mark>

※ 누리온 시스템 설치 예제는 SKL/KNL 계산노드에서 공통적으로 사용을 위해 "-xCOMMON-AVX512" 로 작성\
****- SKL(skylake) 노드 전용 : -xCORE-AVX512\
\- KNL(Intel Xeon Phi Knights Landing) 전용 : -xMIC-AVX512\
\- SKL 과 KNL 공통 적용 : -xCOMMON-AVX512\
\- 참고 :  https://software.intel.com/en-us/articles/compiling-for-the-intel-xeon-phi-processor-and-the-intel-avx-512-isa
