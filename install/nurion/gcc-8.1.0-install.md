---
description: 슈퍼컴퓨팅인프라센터 2018. 12. 3. 15:16
---

# gcc-8.1.0 설치

KISTI 슈퍼컴퓨팅센터의 누리온 시스템에 gcc-8.1.0 Source 버전으로 설치 하는 방법에 대하여 소개 한다.



## **1. 설치 환경**

|  **구분**     | **내용**                                   |
| ----------- | ---------------------------------------- |
|  대상 시스템     |  누리온                                     |
| OS Version  |  리눅스 / CentOS 7.3                        |
|  CPU        |  Intel(R) Xeon(R) Gold 6126              |
|  컴파일러       |  GCC 4.8.5 (CentOS 7.3 default compiler) |
|  MPI        |                                          |
|  기타         |                                          |



## **2. 설치 전 환경 설정**

&#x20;gcc-8.1.0 버전 설치에 필요한 gmp, mpfr, mpc는 누리온 시스템에 미리 설치된 /apps/common 라이브러리들을 사용한다.\
&#x20;만약 다른 버전의 gmp, mpfr, mpc 가 필요한 경우는 사용자의 홈 디렉토리(/home01/$USER)에 설치 후 환경설정을 해서 사용하면 된다.



{% code title="[ 환경 설정 ]" %}
```
$ export LD_LIBRARY_PATH=/apps/common/gmp/6.1.2/lib:$LD_LIBRARY_PATH
$ export LD_LIBRARY_PATH=/apps/common/mpfr/4.0.1/lib:$LD_LIBRARY_PATH
$ export LD_LIBRARY_PATH=/apps/common/mpc/1.1.0/lib:$LD_LIBRARY_PATH
```
{% endcode %}

## **3. gcc-8.1.0 버전 설치 과정**

&#x20;설치 과정 소개는 tar 를 이용한 압축 해제 방법과 설정 방법등 진행 절차를 위주로 설명하고, 소스 파일 다운로드 등은 생략한다.  \


{% code title=" 설치 과정" %}
```
 $ tar xvzf gcc-8.1.0.tar.gz 
 $ cd gcc-8.1.0
 $ mkdir build
 $ cd build
 $ ../configure --prefix=${HOME}/applications/gcc/8.1.0 \
 --with-mpc=/apps/common/mpc/1.1.0 \
 --with-mpfr=/apps/common/mpfr/4.0.1 \
 --with-gmp=/apps/common/gmp/6.1.2 \
 --disable-multilib --enable-languages=c,c++,fortran
 $ make
 $ make install
```
{% endcode %}

<mark style="color:blue;">※ "--prefix=${HOME}/applications/gcc/8.1.0"는 예시로 설치 희망하는 디렉토리로 명시한다.</mark>
