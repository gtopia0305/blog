---
description: 슈퍼컴퓨팅인프라센터 2021. 4. 27. 09:17
---

# Quantum Espresso-6.6 설치 소개

KISTI 슈퍼컴퓨터센터의 장비에 Quantum Espresso-6.6 source 버전으로 설치하는 방법에 대하여 소개한다.

## **1. 설치 환경**

|   구분           | 내용                         |
| -------------- | -------------------------- |
|  대상 시스템        | 누리온                        |
|  OS Version    | 리눅스 / CentOS 7.7           |
|  CPU           | Intel(R) Xeon(R) Gold 6126 |
|  컴파일러          | Intel 2019.0.5 Version     |
|  MPI           | IntelMPI 2019.0.5 Version  |
| <p> 기타<br></p> | Intel MKL Math Library     |

## **2. 설치 전 환경 설정**

KISTI 시스템은 PATH, LD\_LIBRARY\_PATH 등을 쉽게 하기 위하여 OpenSource 인 Environment Modules(http://modules.sourceforge.net)이 구성되어 있고, 이하 설치 소개에서는 module load를 이용한 환경 설정 방법을 이용한다.

{% code title="[ 환경 설정 ]" %}
```
$ module load intel/19.0.5 impi/19.0.5
```
{% endcode %}

## **3. 설치 과정**

&#x20;설치 과정 소개는 tar 를 이용한 압축 해제 방법과 설정 방법등 진행 절차를 위주로 설명하고, 소스 파일 다운로드 등은 생략한다. &#x20;

{% code title="설치 과정" %}
```
$ tar -xvzf qe-6.6.tar.gz
$ cd q-e-qe-6.6
$ ./configure --prefix=${HOME}/QE/6.6 \
CC=mpiicc F90=mpiifort FC=mpiifort MPIF90=mpiifort \
CFLAGS="-O3 -fPIC -xCOMMON-AVX512" \
FFLAGS="-O3 -fPIC -xCOMMON-AVX512"  
$ make all
$ make install
```
{% endcode %}

<mark style="color:red;">**※위 설치 과정에서 파란색으로 표기된 설치 경로 ${HOME}/QE/6.6 은 예제이다. 실제 사용하는 위치로 변경해서 사용해야 한다.**</mark>

&#x20;

※  누리온 시스템 설치 예제는 SKL/KNL 계산 노드에서 공통적으로 사용을 위해 "-xCOMMON-AVX512" 로 작성\
\- SKL(skylake) 노드 전용 : -xCORE-AVX512\
\- KNL(Intel Xeon Phi Knights Landing) 전용 : -xMIC-AVX512\
\- SKL과 KNL 공통 적용 : -xCOMMON-AVX512

\- 참고 : https://software.intel.com/en-us/articles/compiling-for-the-intel-xeon-phi-processor-and-the-intel-avx-512-isa
