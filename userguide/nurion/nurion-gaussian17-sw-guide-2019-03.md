---
description: 슈퍼컴퓨팅인프라센터 2019. 3. 4. 10:04
---

# 누리온 가우시안16(Gaussian16) S/W 사용 안내 (2019.03)

본 문서는 누리온 시스템에서 가우시안 소프트웨어 사용을 위한 기초적인 정보를 제공하고 있습니다. 따라서, 가우시안 소프트웨어 사용법 및 누리온/리눅스 사용법 등은 포함되어 있지 않습니다. 누리온/리눅스 사용법에 대한 정보는 KISTI 홈페이지 (https://www.ksc.re.kr)의 기술지원 > 지침서 내 누리온 사용자 지침서 등을 참고하시기 바랍니다.

****

## **1. 가우시안 소개**

가우시안은 에너지, 분자구조 및 진동주파수를 예측하는 분자 모델링 패키지이며, 화학, 물리, 생명과학, 공학 분야 연구자를 위한 프로그램입니다.\
자세한 사항은 가우시안 사의 홈페이지를 통해 얻을 수 있습니다.

홈페이지 주소: [http://gaussian.com](http://www.gaussian.com/)

****

## **2. 설치 버전 및 라이선스**

\- KISTI 슈퍼컴퓨팅센터는 가우시안 16의 사이트 라이선스를 보유하고 있으며, 누리온 시스템에는 가우시안16 Rev. A03이 설치되어 있습니다.

\- 가우시안16를 사용하기 위해서는 사용자의 계정이 가우시안 그룹(gauss group)에 등록되어야 합니다. 가우시안 그룹 등록은 KISTI 홈페이지 또는 account@ksc.re.kr로 문의하시기 바랍니다.

\- 내 계정이 가우시안 그룹에 속해있는지 확인하는 방법은 다음과 같습니다.



```
$ id 사용자ID
```

※ 가우시안 그룹에 포함되어 있으면 출력 결과에 "1000009(gauss)" 이 포함되어 있어야 합니다.





\- 보안 문제로 사용자는 프로그램의 소스 코드에는 접근할 수 없고, 실행 파일과 기저함수(basis function)에만 접근할 수 있습니다. 실제로 프로그램을 사용하는 데는 아무런 지장이 없습니다.

\- 가우시안에 연동하여 사용하는 프로그램을 사용하기 위해서는 사전에 일부 소스 코드 혹은 쉘 파일에 대한 접근권한이 필요하며 (예, Gaussrate) 이 경우 KISTI 홈페이지 또는 account@ksc.re.kr 메일을 통해 요청하셔야 합니다.

\- HF 계산과 DFT 계산은 병렬로 수행할 수 있습니다.

<mark style="color:red;">- 2019년 3월 PM 이후</mark>

<mark style="color:red;">(3월14일)</mark>

<mark style="color:red;">에는 작업제출 스크립트에 "#PBS -A gaussian" 옵션을 사용해야 합니다.</mark>

<mark style="color:red;"></mark>

## **3. 소프트웨어 실행 방법**

### **(1) 환경설정**

가우시안16은 module 명령을 통하여 환경을 로드할 수 있습니다.

```
$ module load gaussian/g16.a03
```

****

### **(2) 스케쥴러 작업 스크립트 파일 작성**

누리온 시스템에서는 로그인 노드에서 PBS Pro라는 스케쥴러를 사용하여 작업을 제출해야 합니다.

누리온 시스템에서 PBS를 사용하는 예제 파일들이 아래의 경로에 존재하므로 사용자 작업용 파일을 만들 때 이를 참고하시기 바랍니다.



※ 아래 예제는 누리온 시스템 에서의 가우시안16에 대한 예제입니다.

파일 위치: /apps/commercial/test\_samples/G16/g16.sh

| <p>#!/bin/sh<br>#PBS -V<br>#PBS -N gaussian_test<br>#PBS -q normal <mark style="color:orange;"><mark style="color:yellow;"># PBS의 queue를 지정</mark></mark><br>#PBS -l select=1:ncpus=64:mpiprocs=1:ompthreads=64<br>#PBS -l walltime=01:00:00 <mark style="color:orange;"><mark style="color:yellow;"># 예상 작업소요시간 지정 (시:분:초)</mark></mark><br><mark style="color:red;">#PBS -A gaussian</mark><br><br>cd $PBS_O_WORKDIR<br><br>module purge<br>module load gaussian/g16.a03<br><br>export GAUSS_SCRDIR="/scratch/${USER}"<br>export GAUSS_PDEF=$NCPUS<br><br>g16 test.com<br><br>exit 0</p> |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |

<mark style="color:red;">- 2019년 3월 PM 이후</mark>\ <mark style="color:red;">(3월14일)</mark>\ <mark style="color:red;">부터는 "#PBS -A gaussian" 옵션이 없는 경우 작업제출이 되지 않습니다.</mark>

<mark style="color:red;"></mark>

\- GAUSS\_PDEF 변수는 %NProcShared 옵션과 동일하며, 입력파일에 %NProcShared 값이 있을 경우 해당 값이 적용 됩니다.\ <mark style="color:red;"></mark>이 때 GAUSS\_PDEF 또는 %NProcShared 옵션의 값은 누리온 KNL 계산노드는 68개 코어, SKL 계산노드는 40개 코어가 장착되어져 있기 때문에 계산노드에 맞게 기입하는것이 안정적인 계산 성능이 발휘 됩니다.

**- 가우시안16 Rev. A03 버전에서 지원하는 최대 threads 수는 64개 입니다. KNL 계산노드를 이용하는 경우 64개 까지만 사용 바랍니다.**

****

\- **작업 제출은 스크래치 디렉토리에서만 가능** 합니다.

\- 사용자별 스크래치 디렉토리는 /scratch/$USER입니다.



\- 큐 이름은 누리온 사용자 지침서를 참조하여 설정하며, 일반적으로 normal로 설정해야 합니다.

<mark style="color:red;">※ 가우시안 입력 파일을 PC에서 작성 후 FTP로 전송한다면, 반드시 ascii mode로 전송해야만 합니다.</mark>

\- 기타 PBS에 관련된 명령어 및 사용법은 누리온 사용자 지침서를 참조하시면 됩니다.

\- Gaussian Multi-Node 사용을 위해서는 Gaussian16 LINDA 버전을 사용하세요.

****

## **4. 참고자료**

\- 가우시안을 처음으로 사용하고자 하는 사람은 다음의 책의 일독을 권합니다.\
****James B. Foresman and Aeleen Frisch, "Exploring Chemistry with Electronic Structure Methods: A Guide to Using Gaussian", www.amazon.com, www.bn.com 등의 온라인 서점에서 구매할 수 있고, http://gaussian.com에서도 직접 구매가 가능합니다.

\- 가우시안에 관한 모든 정보는 Gaussian사의 홈페이지(http://gaussian.com)를 통해 얻을 수 있습니다.
