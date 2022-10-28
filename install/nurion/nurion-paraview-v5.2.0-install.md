---
description: 슈퍼컴퓨팅인프라센터 2020. 10. 28. 10:28
---

# ParaView-v5.2.0 설치 소개

KISTI 슈퍼컴퓨팅센터의 누리온 시스템에  ParaView-v5.2.0 Source 버전으로 설치 하는 방법에 대하여 소개 한다.



## **1. 설치 환경**

|  **구분**      | **내용**                      |
| ------------ | --------------------------- |
|  대상 시스템      |  누리온                        |
|  OS Version  |  리눅스 / CentOS 7.7           |
|  CPU         |  Intel(R) Xeon(R) Gold 6126 |
|  컴파일러        |  Intel 2018.3               |
|  MPI         |  Intel MPI 2018.3           |
|  기타          |                             |



## **2. 설치 전 환경 설정**

누리온 시스템은 PATH, LD\_LIBRARY\_PATH 등을 쉽게 설정하기 위하여 \
환경설정 툴인 Modules(http://modules.sourceforge.net)이 구성되어 있고,\
이하 설치 소개 에서는 module load를 이용한 환경 설정 방법을 이용한다.



{% code title="[ 환경 설정 ]" %}
```
 $ module load intel/18.0.3 impi/18.0.3
 $ module load cmake/3.17.4
```
{% endcode %}

## **3. ParaView-v5.2.0 버전 설치 과정**

&#x20;설치 과정 소개는 tar 를 이용한 압축 해제 방법과 설정 방법등 진행 절차를 위주로 설명하고,\
&#x20;소스 파일 다운로드 등은 생략한다. &#x20;

{% code title=" 설치 과정" %}
```
 $ tar -xzvf ParaView-v5.2.0.tar.gz
 $ cd ParaView-v5.2.0
 $ mkdir build
 $ cd build
 $ cmake .. -DVTK_RENDERING_BACKEND=OpenGL
 $ make


 $ ls bin/paraview
 bin/paraview
```
{% endcode %}

※ 의존 라이브러리 버전 등의 문제로 누리온 시스템에서 ParaView-v5.2.0 이후 버전은 사용 불가

※ 다만 하위 버전(4.x) 의 경우 사용이 가능할 것으로 보임
