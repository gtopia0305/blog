---
description: 슈퍼컴퓨팅인프라센터 2017. 5. 22. 15:02
---

# NAMD-2.12 설치 (Intel Xeon Phi KNL)

이 문서는 Intel Xeon Phi KNL (Knights Landing)이 장착되어 있는 머신에서 NAMD 2.12이 수행될 수 있도록 소스 버전을 빌드하는 방법에 대해 기술한다.

&#x20;

## **1. 설치 환경**

| 구분          | 내용                                                 |
| ----------- | -------------------------------------------------- |
|  대상 시스템     | 워크스테이션                                             |
|  OS Version | 리눅스 / CentOS 7.2                                   |
|  CPU        | Intel(R) Xeon Phi(TM) CPU 7210 @ 1.30GHz (64-core) |
|  컴파일러       | Intel Compiler 2017                                |
| 기타          | Intel MKL                                          |

&#x20;

## **2. 설치 정**

{% code title="  NAMD 소스 파일 압축 풀기 " %}
```
$ tar xzf NAMD_2.12_Source.tar.gz 
$ cd NAMD_2.12_Source   
```
{% endcode %}

### **2.1 charm 빌드**

NAMD 소스는 charm을 이용하여 빌드한다. charm은 NAMD 소스내에 tar파일 형태로 포함되어 있으며, charm 빌드는 아래와 같은 명령으로 이루어진다.

```
$ build charm++ {아키텍쳐} {C 컴파일러} {포트란 컴파일러} {기타 옵션}
```

\
리눅스 PC 등과 같은 단일 노드에서 사용할 경우에는 'multicore-linux64' 옵션을 기재하여 빌드한다.&#x20;

{% code title=" 단일노드용 charm 빌드  " %}
```
$ tar xf charm-6.7.1.tar 
$ cd charm-6.7.1
$./build charm++ multicore-linux64 icc --with-production   
```
{% endcode %}



### **2.2 NAMD 빌드**

NAMD를 빌드하기 위해서는 config을 이용하여 설정이 이루어지는데, 사용법은 다음과 같다**.**&#x20;

```
 $ ./config {NAMD-아키텍쳐} --charm-arch {charm-아키텍쳐} {기타 옵션}
```



Xeon Phi KNL용으로 빌드하기 위해서는 NAMD-아키텍쳐를 Linux-KNL-icc 지정하여 수행한다. &#x20;

{% code title="  Xeon Phi NAMD 빌드 " %}
```
# NAMD 소스 디렉토리에서 수행
$ ./config Linux-KNL-icc --charm-arch multicore-linux64-icc --with-mkl \
    --mkl-prefix /home/test01/intel/compilers_and_libraries_2017/linux/mkl \
    --without-tcl  
  # <---이 예제에서는 인텔 컴파일러를 사용자 홈디렉토리에 설치하였음을 가정 
$ cd Linux-KNL-icc
$ make -j 64
```
{% endcode %}

※ 이미 설치되어 있는 tcl를 이용하려고 하는 경우는 --with-tcl, --tcl-prefix 옵션을 추가로 기재한다. \
\


### **3. 테스트**&#x20;

아래 사이트에서 적당한 시뮬레이션 예제 데이터 파일을 다운 받는다.&#x20;

&#x20;[http://www.ks.uiuc.edu/Research/namd/utilities/](http://www.ks.uiuc.edu/Research/namd/utilities/)&#x20;

{% code title="  ApoA1 벤치마크를 이용한 테스트  " %}
```
$ wget http://www.ks.uiuc.edu/Research/namd/utilities/apoa1.zip
$ unzip apoa1.zip 
$ {NAMD_DIR}/namd2 +p 64 apoa1/apoa1.namd  +pemap 0-63  # 단일노드에서의 수행 
```
{% endcode %}

\
