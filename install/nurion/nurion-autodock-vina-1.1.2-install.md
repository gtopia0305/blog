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

```
 $ module load intel/18.0.3
```

## **3. AutoDock Vina-1.1.2 버전 설치 과정**

&#x20;설치 과정 소개는 tar 를 이용한 압축 해제 방법과 설정 방법등 진행 절차를 위주로 설명하고,

&#x20;소스 파일 다운로드 등은 생략한다.  \


{% code title=" 설치 과정" %}
```
 $ tar -xzvf autodock_vina_1_1_2.tgz
 $ cd autodock_vina_1_1_2
 $ vi src/main/main.cpp
 - - - - - [ main.cpp 파일 수정 내용] 참고 - - - - -


 $ vi src/split/split.cpp
 - - - - - [ split.cpp 파일 수정 내용] 참고 - - - - -


 $ cd build/linux/release
 $ vi Makefile
 - - - - - [ Makefile 파일 수정 내용] 참고 - - - - -


 $ make
 $ ls vina*
 vina  vina_split
```
{% endcode %}

**※ 컴파일러 버전 호환성 문제로 GNU 컴파일러는 반드시 시스템 기본 컴파일러(4.8.5)를 사용해야 한다.**



{% code title="[main.cpp 파일 수정 내용]" %}
```
line 50:
<수정 전>
return path(str, boost::filesystem::native);
<수정 후>
return path(str);


line 664:
<수정 전>
std::cerr <"\n\nError: could not open \"" <e.name.native_file_string() <"\" for " <(e.in ? "reading" : "writing") <".\n";
<수정 후>
std::cerr <"\n\nError: could not open \"" <e.name.filename() <"\" for " <(e.in ? "reading" : "writing") <".\n";


line 676:
<수정 전>
std::cerr <"\n\nParse error on line " <e.line <" in file \"" <e.file.native_file_string() <"\": " <e.reason <'\n';
<수정 후>
std::cerr <"\n\nParse error on line " <e.line <" in file \"" <e.file.filename() <"\": " <e.reason <'\n';
```
{% endcode %}

{% code title=" [split.cpp 파일 수정 내용]" %}
```
line 41:
<수정 전>
return path(str, boost::filesystem::native);
<수정 후>
return path(str);


line 211:
<수정 전>
std::cerr <"\n\nError: could not open \"" <e.name.native_file_string() <"\" for " <(e.in ? "reading" : "writing") <".\n";
<수정 후>
std::cerr <"\n\nError: could not open \"" <e.name.filename() <"\" for " <(e.in ? "reading" : "writing") <".\n";


line 223:
<수정 전>
std::cerr <"\n\nParse error on line " <e.line <" in file \"" <e.file.native_file_string() <"\": " <e.reason <'\n';
<수정 후>
std::cerr <"\n\nParse error on line " <e.line <" in file \"" <e.file.filename() <"\": " <e.reason <'\n';

```
{% endcode %}



\[Makefile 파일 수정 내용]

{% code title="- GNU 컴파일러 사용 시" %}
```

BASE=/apps/common/boost/1.68.0
BOOST_VERSION=1_68
BOOST_INCLUDE = $(BASE)/include
C_PLATFORM= -static -pthread -L/apps/compiler/gcc/7.2.0/lib64
GPP=g++
C_OPTIONS= -O3 -DNDEBUG
BOOST_LIB_VERSION=

include ../../makefile_common
```
{% endcode %}

{% code title="- intel 컴파일러 사용 시" %}
```
BASE=/apps/common/boost/1.68.0
BOOST_VERSION=1_68
BOOST_INCLUDE = $(BASE)/include
C_PLATFORM= -static -pthread -L/apps/compiler/gcc/7.2.0/lib64
GPP=icpc
C_OPTIONS= -O3 -DNDEBUG
BOOST_LIB_VERSION=

include ../../makefile_common
```
{% endcode %}
