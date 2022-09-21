---
description: 슈퍼컴퓨팅인프라센터 2019. 3. 4. 09:48
---

# 누리온 LS-DYNA 사용자 지침서(2020.02)

본 문서는 누리온 시스템에서 LS-DYNA 소프트웨어 사용을 위한 기초적인 정보를 제공하고 있습니다.\
따라서, LS-DYNA 소프트웨어 사용법 및 누리온/리눅스 사용법 등은 포함되어 있지 않습니다.\
누리온/리눅스 사용법에 대한 정보는 KISTI 홈페이지 (https://www.ksc.re.kr)의 기술지원 > 지침서 내 누리온 사용자 지침서 등을 참고하시기 바랍니다.

### **1. 사용 정책**

\- 사용자별 **최대 40개 CPU 코어 수행 가능**합니다.

<mark style="color:red;">- 한정된 라이선스를 슈퍼컴 사용자들이 함께 사용하므로, 사용정책 기준을 초과하여 사용할 경우 해당 작업은 관리자가 강제 종료 합니다.</mark>

<mark style="color:red;">- 부득이하게 많은 라이선스가 필요할 경우, KISTI 홈페이지(https://www.ksc.re.kr)를 통해 사전에 관리자와 협의해야 합니다.</mark>

\- 작업 제출 전 "lic\_check" 명령 에서 메뉴 선택을 통해 라이선스 상태를 확인 한 다음 작업을 제출하시기 바랍니다.

<mark style="color:red;">- 누리온시스템 로그인 노드의 과부하 방지를 위해 pre/post 작업은 허가하지 않습니다.</mark>

<mark style="color:red;">- 2019년 3월 PM 이후(3월14일)에는 작업제출 스크립트에 "#PBS -A lsdyna" 옵션을 사용해야 합니다.</mark>

### **2. 소프트웨어 설치 정보**

**(1) 설치 버전**

\- LSDYNA R11.1.0(smp, mpp), R10.1.0(smp, mpp), R9.2.0(smp, mpp), R9.1.0(smp, mpp)



**(2) 설치 위치**

\- /apps/commercial/LSDYNA

### **3. 소프트웨어 실행 방법**

**(1) 실행 방법 설명 및 실행 예시**

\- 실행 명령 및 옵션

: 옵션 대소문자 및 위치 구분 없음. 해당 옵션 사용하지 않을 경우 default값으로 출력됨



(Options = Input file specification, Output file specification, Extra option)

```
[ls-dyna 명령어] [option]
```

※ **ls-dyna 명령어**는 **(2)실행 예제** 및 관련 표를 참고하시고, **option**에 대한 정보는 아래 표를 참고하여 사용하시길 바랍니다.

![](../../../.gitbook/assets/ls\_dyna\_command.png)

**(2) 실행 예제**

**1) SMP 버전 실행 예시**

\- input file 명 : airbag\_deploy.k

\- 사용 CPU 수 : 4개

\- 사용메모리 양 : 400MB (100MB X 4)

\- Output files

: information outputs :d3hsp, messag, status.out

: binary outputs : d3plot - d3plot##, d3dump01 - d3dump##,

: ascii outputs : abstat, glstat, matsum, rcforc, rwforc, etc.

****

**- 실행 예제**

```
$ [LS-DYNA 명령어] i=airbag.deploy.k memory=100m ncpu=4
```

<mark style="color:red;">※ 사용가능한 LS-DYNA 명령어는 아래 표를 참고하여 선택한다.</mark>

![](../../../.gitbook/assets/ls\_dyna\_command2.png)

****

**2) MPP 버전 실행 예시**

\- input file 명 : airbag\_deploy.k

\- 사용 CPU 수 : 4개

\- 사용메모리 양 : 400MB (100MB X 4)

\- Output files

: Information outputs : d3hsp, mes0000 - mes0003, status.out, adptmp, scr0000 - scr0003

: Binary outputs : d3plot - d3plot##, d3dump01.0000 - d3dump##.0003, d3full01 - d3full##

: Binary database (smp의 ascii output에 해당) : binout0000



: Ascii ouput extractions from binary database : l2a binout\* 실행



**- 실행 예제**

```
$ mpirun -np 4 [LS-DYNA 명령어] i=airbag.deploy.k memory=100m memory2=20m -procs 4
```

<mark style="color:red;">※ 사용가능한 LS-DYNA 명령어는 아래 표를 참고하여 선택한다.</mark>

![](../../../.gitbook/assets/ls\_dyna\_command3.png)

**(3) 실행 방법**

\- Interactive 방식의 실행은 CPU time이 10분으로 제한되어 있습니다.

\- 장시간의 계산 작업은 PBS 라는 스케줄러를 이용하여 작업을 제출해야 합니다.

<mark style="color:red;">- 작업 제출 전 "lic\_check" 명령을 실행하여 라이선스 상태를 확인 후 작업을 제출하시기 바랍니다.</mark>

****

**(4) 스케쥴러 작업 스크립트 파일 작성**

누리온 시스템에서는 로그인 노드에서 PBS 라는 스케쥴러를 사용하여 작업을 제출해야 합니다.



누리온 시스템에서 PBS 를 사용하는 예제 파일들이 아래의 경로에 존재하므로 사용자 작업용 파일을 만들 때 이를 참고하시기 바랍니다.

\- 예제 파일 : /apps/commercial/test\_samples/LSDYNA/lsdyna\_smp.sh **(단일 노드에서 수행)**\
&#x20;                        ****                         /apps/commercial/test\_samples/LSDYNA/lsdyna\_mpp.sh **(멀티 노드에서 수행)**

※ 아래 예제는 누리온 시스템 에서의 LS-DYNA에 대한 예제입니다. **(단일 노드에서 수행)**

| <p>#!/bin/sh</p><p>#PBS -V</p><p>#PBS -N <mark style="color:blue;">lsdyna_job</mark></p><p>#PBS -q commercial</p><p>#PBS -l select=1:ncpus=<mark style="color:blue;">40</mark>:mpiprocs=1:ompthreads=<mark style="color:blue;">40</mark></p><p>#PBS -l walltime=<mark style="color:blue;">04:00:00</mark></p><p><mark style="color:red;">#PBS -A lsdyna</mark></p><p><mark style="color:red;"></mark></p><p>cd $PBS_O_WORKDIR</p><p></p><p>module load lsdyna/smp</p><p></p><p></p><p><mark style="color:blue;">/apps/commercial/LSDYNA/SMP/R10.1.0/ls-dyna_smp_s_r1010_x64_redhat5_ifort160 <mark style="color:red;">\</mark> i=airbag.deploy.k</mark> memory=<mark style="color:blue;">1000m</mark> ncpu=$NCPUS</p> |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

\- 위에서 파란색으로 표기된 부분은 사용자가 적절히 수정해야 합니다.

<mark style="color:red;">- 2019년 3월 PM 이후(3월14일)부터는 "#PBS -A lsdyna" 옵션이 없는 경우 작업제출이 되지 않습니다.</mark>



※ 아래 예제는 누리온 시스템 에서의 LS-DYNA에 대한 예제입니다. **(멀티 노드에서 수행)**

| <p>#!/bin/sh</p><p>#PBS -V</p><p>#PBS -N <mark style="color:blue;">lsdyna_job</mark></p><p>#PBS -q commercial</p><p>#PBS -l select=<mark style="color:blue;">2</mark>:ncpus=<mark style="color:blue;">20</mark>:mpiprocs=<mark style="color:blue;">20</mark>:ompthreads=1</p><p>#PBS -l walltime=04:00:00</p><p><mark style="color:red;">#PBS -A lsdyna</mark></p><p><mark style="color:red;"></mark></p><p>cd $PBS_O_WORKDIR</p><p></p><p>module load lsdyna/mpp</p><p></p><p>mpirun -machinefile $PBS_NODEFILE <mark style="color:red;">\</mark></p><p><mark style="color:blue;">/apps/commercial/LSDYNA/MPP/R10.1.0/ls-dyna_mpp_s_r10_1_123355_x64_redhat54_ifort160_sse2_platformmpi</mark> \</p><p>i=<mark style="color:blue;">airbag.deploy.k</mark> memory=<mark style="color:blue;">1000m</mark></p> |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |

\- 위에서 파란색으로 표기된 부분은 사용자가 적절히 수정해야 합니다.

<mark style="color:red;">- 2019년 3월 PM 이후(3월14일)부터는 "#PBS -A lsdyna" 옵션이 없는 경우 작업제출이 되지 않습니다.</mark>

\- **작업 제출은 스크래치 디렉토리에서만 가능** 합니다.

\- 사용자별 스크래치 디렉토리는 /scratch/$USER입니다.

<mark style="color:red;">- 스크래치 디스크는 작업 종료 후 일정 시간(2020년 2월 현재 정책: 15일)이 지나면 삭제되기 때문에, 작업이 완료 된경우 빠른 시일 내에 백업하시길 권장합니다.</mark>

<mark style="color:red;"></mark>

\- 기타 PBS에 관련된 명령어 및 사용법은 누리온 사용자 지침서를 참조하시면 됩니다.

****

**(5) 작업 제출 방법**

\- 예제 : 스크립트 파일 이름이 lsdyna.sh 인 경우

```
$ qsub lsdyna.sh
```

****

**(6) 작업 상태 확인**

```
$ qstat (또는 qstat -u $USER) 
```



**(7) 제출된 작업을 강제로 종료**

\- 사용 방법 : qdel

\- 작업ID는 qstat 명령어 실행 시 제일 왼쪽에 표시 되는 정보입니다.(ex. 1771476.pbs)

\- 예제 : 작업ID 가 1771476.pbs 인 경우

```
$ qdel 1771476.pbs
```



**(8) PBS 상에서 사용 가능한 자원 확인**

```
$ pbs_status
```

****
