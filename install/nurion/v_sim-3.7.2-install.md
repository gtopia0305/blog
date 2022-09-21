---
description: 슈퍼컴퓨팅인프라센터 2020. 4. 23. 10:46
---

# v\_sim-3.7.2 설치 소개

KISTI 슈퍼컴퓨터센터의 장비에 v\_sim-3.7.2 source 버전으로 설치 하는 방법에 대하여 소개 한다.



## **1. 설치 환경**

|   **구분**    | **내용**                     |
| ----------- | -------------------------- |
|  대상 시스템     | 누리온                        |
|  OS Version | 리눅스 / CentOS 7.7           |
|  CPU        | Intel(R) Xeon(R) Gold 6126 |
|  컴파일러       | gcc 4.8.5 Version          |



## **2. 설치 과정**

&#x20;설치 과정 소개는 tar 를 이용한 압축 해제 방법과 설정 방법등 진행 절차를 위주로 설명하고, 소스 파일 다운로드 등은 생략한다.&#x20;

**v\_sim은 dependency로 인하여 설치 전 Intltool와 harfbuzz 라이브러리에 대한 설치가 사전에 완료되어야 한다.**

설치 경로는 <mark style="color:blue;">**${HOME}/apps/vsim**</mark>을 사용하였다. 이 위치는 사용자에게 맞는 위치로 변경하여야 한다.\
아래의 방법으로 설치 진행 시 바이너리와 라이브러리 파일은 각각 <mark style="color:blue;">**${HOME}/apps/vsim/bin, ${HOME}/apps/vsim/lib**</mark>** ** 디렉토리에 설치 된다.

\


### &#x20; **(1) intltool 설치**

|   **설치과정**                                                                                                                                                                                                 |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p>$ tar xzvf intltool-0.50.2.tar.gz</p><p>$ cd intltool-0.50.2</p><p>$ ./configure --prefix=<mark style="color:blue;"><strong>${HOME}/apps/intltool</strong></mark></p><p>$ make</p><p>$ make install</p> |



### &#x20; **(2) harfbuzz 설치**

|   **설치과정**                                                                                                                                                                                               |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p>$ tar xvf harfbuzz-1.7.5.tar.bz2</p><p>$ cd harfbuzz-1.7.5</p><p>$ ./configure --prefix=<mark style="color:blue;"><strong>${HOME}/apps/harfbuzz</strong></mark></p><p>$ make</p><p>$ make install</p> |



### &#x20; **(3) v\_sim 설치**

|   **설치과정**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p>$ export PATH=<mark style="color:blue;"><strong>${HOME}/apps/intltool/bin:$PATH</strong></mark></p><p>$ tar -xvf v_sim-3.7.2.tar.bz2</p><p>$ cd v_sim-3.7.2</p><p>$ ./configure --prefix=<mark style="color:blue;"><strong>${HOME}/apps/vsim</strong></mark> \</p><p>GTKS_CFLAGS='-I<mark style="color:blue;"><strong>${HOME}/apps/harfbuzz/include/harfbuzz</strong>'</mark> \</p><p>GTKS_LIBS='-L<mark style="color:blue;"><strong>${HOME}/apps/harfbuzz/lib -lharfbuzz</strong>'</mark> \</p><p>PKG_CONFIG_PATH='<mark style="color:blue;"><strong>${HOME}/apps/harfbuzz/lib/pkgconfig</strong></mark>' \</p><p>LDFLAGS='-L/usr/lib64 -lgmodule-2.0 -lgobject-2.0 -lgthread-2.0 -lglib-2.0 -lgtk-x11-2.0' \</p><p>CFLAGS='-I/usr/include/glib-2.0 -I/usr/lib64/glib-2.0/include -I/usr/include/cairo -I/usr/include/gdk-pixbuf-2.0 -I/usr/include/pango-1.0 -I/usr/include/gtk-2.0 -I/usr/lib64/gtk-2.0/include -I/usr/include/atk-1.0'</p><p>$ make</p><p>$ make install</p> |

