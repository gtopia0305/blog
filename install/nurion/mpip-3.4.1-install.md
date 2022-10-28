---
description: 슈퍼컴퓨팅인프라센터 2019. 6. 12. 10:16
---

# mpiP-3.4.1 설치 소개

KISTI 슈퍼컴퓨터센터의 장비에 mpiP-3.4.1 source 버전으로 설치 하는 방법에 대하여 소개 한다.



## **1. 설치 환경**

|   **구분**    | **내용**                     |
| ----------- | -------------------------- |
|  대상 시스템     | 누리온                        |
|  OS Version | 리눅스 / CentOS 7.3           |
|  CPU        | Intel(R) Xeon(R) Gold 6126 |
|  컴파일러       | GNU 7.2.0 version          |
|  MPI        | Openmpi 3.1.0 version      |
|  기타         |                            |



## **2. 설치 전 환경 설정**

KISTI 시스템은 PATH, LD\_LIBRARY\_PATH 등을 쉽게 하기 위하여 OpenSource 인 Environment Modules(http://modules.sourceforge.net)로 구성되어 있고, 이하 설치 소개 에서는 module load를 이용한 환경 설정 방법을 이용한다.



{% code title="[ 환경 설정 ]" %}
```
$ module load gcc/7.2.0 openmpi/3.1.0
```
{% endcode %}

## **3. 설치 과정**

&#x20;설치 과정 소개는 tar 를 이용한 압축 해제 방법과 설정 방법등 진행 절차를 위주로 설명하고, 소스 파일 다운로드 등은 생략한다.&#x20;

**mpiP는 dependency로 인하여 설치 전 binutils와 libunwind 라이브러리에 대한 설치가 사전에 완료되어야 한다.**

설치 경로는 <mark style="color:blue;">**${HOME}/apps/mpiP/3.4.1**</mark>을 사용하였다. 이 위치는 사용자에게 맞는 위치로 변경하여야 한다. 아래의 방법으로 설치 진행 시 라이브러리와 헤더 파일은 각각 <mark style="color:blue;">**my\_lib, my\_include**</mark>** ** 디렉토리에 설치 된다.&#x20;

### &#x20; **(1) binutils 설치**

{% code title="  설치과정" %}
```
$ tar xzvf binutils-2.32.tar.gz
$ cd binutils-2.32 
$ ./configure --prefix=${HOME}/apps/binutils \
--libdir=${HOME}/apps/my_lib \
--includedir=${HOME}/apps/my_include \
--enable-ld=yes --with-system-zlib \
--with-mpc=/apps/common/mpc/1.1.0 --with-mpfr=/apps/common/mpfr/4.0.1 \
--with-gmp=/apps/common/gmp/6.1.2 --enable-install-libiberty
$ make
$ make install
```
{% endcode %}

###

### &#x20; **(2) libunwind 설치**

{% code title="  설치과정" %}
```
$ tar xzvf libunwind-1.2.tar.gz
$ cd libunwind-1.2
$ ./configure --prefix=${HOME}/apps/libunwind \
--libdir=${HOME}/apps/my_lib \
--includedir=${HOME}/apps/my_include \
--enable-shared --enable-static
$ make
$ make install
```
{% endcode %}

###

### &#x20; **(3) mpiP 설치**

{% code title="  설치과정" %}
```
$ tar xzvf mpiP-3.4.1.tar.gz
$ cd mpiP-3.4.1
$ ./configure --prefix=${HOME}/apps/mpiP \
--libdir=${HOME}/apps/my_lib \
--includedir=${HOME}/apps/my_include \
LDFLAGS="-L/apps/compiler/gcc/7.2.0/openmpi/3.1.0/lib \
-L${HOME}/apps/my_lib" \
CPPFLAGS="-I/apps/compiler/gcc/7.2.0/openmpi/3.1.0/include \
-I${HOME}/apps/my_include"
$ vi mpiPi.h
----- 수정 사항은 아래의 내용 참고 -----
$ make
$ make install
```
{% endcode %}

{% code title="[vi mpiPi.h수정 사항]" %}
```
<수정 전>
#ifndef _MPIPI_H
#define _MPIPI_H

<수정 후>
#ifndef _MPIPI_H
#define _MPIPI_H
#define PACKAGE 1
#define PACKAGE_VERSION 1

```
{% endcode %}

