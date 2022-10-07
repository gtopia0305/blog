---
description: 슈퍼컴퓨팅인프라센터 2020. 12. 24. 10:03
---

# AutoDock Vina-1.1.2 설치 소개

KISTI 슈퍼컴퓨팅센터의 누리온 시스템에 AutoDock Vina-1.1.2 Source 버전으로 설치하는 방법에 대하여 소개한다.

\


## **1. 설치 환경**

|  **구분**      | **내용**                      |
| ------------ | --------------------------- |
|  대상 시스템      |  누리온                        |
|  OS Version  |  리눅스 / CentOS 7.7           |
|  CPU         |  Intel(R) Xeon(R) Gold 6126 |
|  컴파일러        |  gcc 4.8.5 / intel 2018.3   |
|  MPI         |                             |
|  기타          |                             |



## **2. 설치 전 환경 설정**

누리온 시스템은 PATH, LD\_LIBRARY\_PATH 등을 쉽게 설정하기 위하여&#x20;

환경설정 툴인 Modules(http://modules.sourceforge.net)이 구성되어 있고,

이하 설치 소개에서는 module load를 이용한 환경 설정 방법을 이용한다.



\[ 환경 설정 ]

1\) GNU 컴파일러 사용 시

\- 별도의 환경설정 불필요



2\) intel 컴파일러 사용 시

> &#x20;$ module load intel/18.0.3

## **3. AutoDock Vina-1.1.2 버전 설치 과정**

&#x20;설치 과정 소개는 tar 를 이용한 압축 해제 방법과 설정 방법등 진행 절차를 위주로 설명하고,

&#x20;소스 파일 다운로드 등은 생략한다.  \


|  **설치 과정**                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p> $ tar -xzvf autodock_vina_1_1_2.tgz</p><p> $ cd autodock_vina_1_1_2</p><p> $ vi src/main/main.cpp</p><p> - - - - - [ main.cpp 파일 수정 내용] 참고 - - - - -</p><p><br></p><p> $ vi src/split/split.cpp</p><p> - - - - - [ split.cpp 파일 수정 내용] 참고 - - - - -</p><p><br></p><p> $ cd build/linux/release</p><p> $ vi Makefile</p><p> - - - - - [ Makefile 파일 수정 내용] 참고 - - - - -</p><p><br></p><p> $ make</p><p> $ ls vina*</p><p> vina  vina_split</p> |

**※ 컴파일러 버전 호환성 문제로 GNU 컴파일러는 반드시 시스템 기본 컴파일러(4.8.5)를 사용해야 한다.**



\[main.cpp 파일 수정 내용]

> line 50:
>
> <수정 전>
>
> return path(str, boost::filesystem::native);
>
> <수정 후>
>
> return path(str);
>
> \
>
>
> line 664:
>
> <수정 전>
>
> std::cerr <"\n\nError: could not open \\"" \<e.name.native\_file\_string() <"\\" for " <(e.in ? "reading" : "writing") <".\n";
>
> <수정 후>
>
> std::cerr <"\n\nError: could not open \\"" \<e.name.filename() <"\\" for " <(e.in ? "reading" : "writing") <".\n";
>
> \
>
>
> line 676:
>
> <수정 전>
>
> std::cerr <"\n\nParse error on line " \<e.line <" in file \\"" \<e.file.native\_file\_string() <"\\": " \<e.reason <'\n';
>
> <수정 후>
>
> std::cerr <"\n\nParse error on line " \<e.line <" in file \\"" \<e.file.filename() <"\\": " \<e.reason <'\n';



\[split.cpp 파일 수정 내용]

> line 41:
>
> <수정 전>
>
> return path(str, boost::filesystem::native);
>
> <수정 후>
>
> return path(str);
>
> \
>
>
> line 211:
>
> <수정 전>
>
> std::cerr <"\n\nError: could not open \\"" \<e.name.native\_file\_string() <"\\" for " <(e.in ? "reading" : "writing") <".\n";
>
> <수정 후>
>
> std::cerr <"\n\nError: could not open \\"" \<e.name.filename() <"\\" for " <(e.in ? "reading" : "writing") <".\n";
>
> \
>
>
> line 223:
>
> <수정 전>
>
> std::cerr <"\n\nParse error on line " \<e.line <" in file \\"" \<e.file.native\_file\_string() <"\\": " \<e.reason <'\n';
>
> <수정 후>
>
> std::cerr <"\n\nParse error on line " \<e.line <" in file \\"" \<e.file.filename() <"\\": " \<e.reason <'\n';



\[Makefile 파일 수정 내용]

\- GNU 컴파일러 사용 시

> BASE=<mark style="color:blue;">/apps/common/boost/1.68.0</mark>
>
> BOOST\_VERSION=<mark style="color:blue;">1\_68</mark>
>
> BOOST\_INCLUDE = $(BASE)/include
>
> C\_PLATFORM= -static -pthread <mark style="color:blue;">-L/apps/compiler/gcc/7.2.0/lib64</mark>
>
> GPP=<mark style="color:blue;">g++</mark>
>
> C\_OPTIONS= -O3 -DNDEBUG
>
> BOOST\_LIB\_VERSION=\
>
>
> include ../../makefile\_common



\- intel 컴파일러 사용 시

> BASE=<mark style="color:blue;">/apps/common/boost/1.68.0</mark>
>
> BOOST\_VERSION=<mark style="color:blue;">1\_68</mark>
>
> BOOST\_INCLUDE = $(BASE)/include
>
> C\_PLATFORM= -static -pthread <mark style="color:blue;">-L/apps/compiler/gcc/7.2.0/lib64</mark>
>
> GPP=<mark style="color:blue;">icpc</mark>
>
> C\_OPTIONS= -O3 -DNDEBUG
>
> BOOST\_LIB\_VERSION=
>
>
>
> include ../../makefile\_common
