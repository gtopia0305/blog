---
description: 슈퍼컴퓨팅인프라센터 2019. 5. 30. 18:02
---

# NAMD 2.12 (GPU 버전) 설치

KISTI 슈퍼컴퓨터센터의 장비에 NAMD 2.12 소스를 (CUDA 기반의) GPU 기능을 사용하는 버전으로 설치하는 방법에 대하여 소개한다.

## **1. 설치 환경**

|   **구분**       | **내용**                            |
| -------------- | --------------------------------- |
|  대상 시스템        | 뉴론                                |
|  OS Version    | 리눅스 / CentOS 7.4                  |
|  CPU           | Intel Xeon E5-2670 v2             |
|  컴파일러          | Intel 2018 Version                |
|  MPI           | Mvapich2 2.3 Version              |
| <p> 기타<br></p> | Intel MKL Math Library, CUDA 10.0 |



## **2. 설치 전 환경 설정**

KISTI 시스템은 PATH, LD\_LIBRARY\_PATH 등을 쉽게 하기 위하여 OpenSource 인\
Environment Modules(http://modules.sourceforge.net)이 구성되어 있고,\
이하 설치 소개 에서는 module load를 이용한 환경 설정 방법을 이용한다.

****

**\[ 환경 설정 ]**

> &#x20;$ module load intel/18.0.2 cuda/10.0 cudampi/mvapich2-2.3

## **3. 설치 과정**

&#x20;설치 과정 소개는 tar 를 이용한 압축 해제 방법과 설정 방법등 진행 절차를 위주로 설명하고, 소스 파일 다운로드 등은 생략한다.&#x20;

****

### **(1) charm-6.7.1 설치**&#x20;

NAMD 소스는 charm을 이용하여 빌드한다. charm은 NAMD 소스내에 tar파일 형태로 포함되어 있으며 NAMD 소스를 압축 해제 후 NAMD 소스 파일안에서 설치를 진행한다.

|   **설치과정**                                                                                                                                                                                                                   |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p>$ tar xvf NAMD_2.12_Source.tar.gz</p><p>$ cd NAMD_2.12_Source  </p><p><br></p><p>$ tar xvf charm-6.7.1.tar</p><p>$ cd charm-6.7.1</p><p>$ ./build charm++ verbs-linux-x86_64 icc smp --with-production -static-intel </p> |

NAMD의 설치 과정에서 호출되는 cuda 식별자 이름이 CUDA 9.1 이후 버전부터 삭제되어 주석처리를 통하여 해당 식별자를 비활성화 한다.&#x20;

※ "**cufftCheck(cufftSetCompatibilityMode**" 이 포함된 10개 line 주석처리

|   **설치과정**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p>$ cd ..</p><p>$ vi src/CudaPmeSolverUtil.C </p><p><br>&#x3C;수정 전></p><p>cufftCheck(cufftSetCompatibilityMode(forwardPlan, CUFFT_COMPATIBILITY_FFTW_PADDING));</p><p>cufftCheck(cufftSetCompatibilityMode(backwardPlan, CUFFT_COMPATIBILITY_FFTW_PADDING));</p><p> </p><p>&#x3C;수정 후></p><p><strong>//cufftCheck(cufftSetCompatibilityMode(forwardPlan, CUFFT_COMPATIBILITY_FFTW_PADDING));</strong></p><p><strong>//cufftCheck(cufftSetCompatibilityMode(backwardPlan, CUFFT_COMPATIBILITY_FFTW_PADDING));</strong></p> |



### **(2) NAMD-2.12 설치** GPU를 사용하기 위해서는 아래와 같이 config 수행시에 --with-cuda, --cuda-prefix 옵션을 추가로 기재한다. &#x20;

|   **설치과정**                                                                                                                                                                                                                                                                                                                    |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p>$ cd ..</p><p>$ ./config Linux-x86_64-icc --charm-base ./charm-6.7.1 \</p><p>--charm-arch verbs-linux-x86_64-smp-icc \</p><p>--with-tcl --with-mkl --mkl-prefix /apps/compiler/intel/18.0.2/mkl \</p><p><strong>--with-cuda --cuda-prefix</strong> /apps/cuda/10.0</p><p><br></p><p>$ cd Linux-x86_64-icc</p><p>$ make</p> |



## **4. 뉴론 시스템에 설치된 NAMD-2.12 사용**

&#x20;위의 설치 과정에서 소개된 옵션들을 사용하여 설치된 NAMD의 사용방법  \
****실행 예제 파일은 apoa1 시뮬레이션 예제 데이터 파일을 다운받아 사용한다.&#x20;

|   **실행방법 예시**                                                                                                   |
| --------------------------------------------------------------------------------------------------------------- |
| <p>$ module load intel/18.0.2 cuda/10.0 cudampi/mvapich2-2.3 module load namd/2.12</p><p>$ namd2 apoa1.namd</p> |
