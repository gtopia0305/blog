---
description: 슈퍼컴퓨팅인프라센터 2018. 12. 4. 09:56
---

# RASPA-2.0 설치 소개

KISTI 슈퍼컴퓨터센터의 장비에 RASPA-2.0 버전으로 설치 하는 방법에 대하여 소개 한다.\
설치 과정 소개는 zip 을 이용한 압축 해제 방법과 설정 방법등 진행 절차를 위주로 설명하고, 소스 파일 다운로드 등은 생략한다.&#x20;



## **1. 설치 환경**

|   **구분**    | **내용**                     |
| ----------- | -------------------------- |
|  대상 시스템     | 누리온                        |
|  OS Version | 리눅스 / CentOS 7.3           |
|  CPU        | Intel(R) Xeon(R) Gold 6126 |
|  컴파일러       | Intel  2018.3 version      |
|  MPI        | IntelMPI 2018.3 version    |
|  기타         | Intel MKL Math Library     |



## **2. 설치 전 환경 설정**

KISTI 시스템은 PATH, LD\_LIBRARY\_PATH 등을 쉽게 하기 위하여 OpenSource 인 Environment Modules(http://modules.sourceforge.net)이 구성되어 있고, 이하 설치 소개 에서는 module load를 이용한 환경 설정 방법을 이용한다.



{% code title="[ 환경 설정 ]" %}
```
 $ module load intel/18.0.3 impi/18.0.3
```
{% endcode %}

## **3. RASPA 2.0 설치 과정**

&#x20;\- 설치 과정에서의 설치 경로는 <mark style="color:blue;">**${HOME}/RASPA2/2.0**</mark>를 사용하였다. 이 위치는 사용자에게 맞는 위치로 변경하여야 한다. &#x20;

{% code title=" 설치과정" %}
```
$ unzip RASPA2-master.zip
$ cd RASPA2-master
$ mkdir -p m4
$ aclocal
$ autoreconf -i
$ automake --add-missing
$ autoconf
$ ./configure --prefix=${HOME}/RASPA2/2.0 \
 CPPFLAGS="-I/apps/compiler/intel/18.0.3/mkl/include/fftw" \
 LDFLAGS="-L/apps/compiler/intel/18.0.3/mkl/lib/intel64" \
 LIBS="-mkl=cluster" CC=mpiicc CXX=mpiicpc F77=mpiifort \
 CFLAGS="-O3 -fPIC -m64 -xCOMMON-AVX512" \
 CXXFLAGS="-O3 -fPIC -m64 -xCOMMON-AVX512" \
 FFLAGS="-O3 -fPIC -m64 -xCOMMON-AVX512"
$ make
$ make install
```
{% endcode %}
