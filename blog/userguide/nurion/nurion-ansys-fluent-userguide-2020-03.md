---
description: 슈퍼컴퓨팅인프라센터 2019. 3. 4. 09:58
---

# 누리온 ANSYS FLUENT 사용자 지침서(2020.03)

본 문서는 누리온 시스템에서 ANSYS FLUENT 사용을 위한 기초적인 정보를 제공하고 있습니다.\
따라서, ANSYS FLUENT 소프트웨어 사용법 및 5호기 시스템/리눅스 사용법 등은 포함되어 있지 않습니다.\
누리온/리눅스 사용법에 대한 정보는 KISTI 홈페이지 (https://www.ksc.re.kr)의 기술지원 > 지침서 내 누리온 사용자 지침서 등을 참고하시기 바랍니다.

****

## **1. 사용 정책**

\- 사용자별 **최대 40개 CPU 코어 수행 가능**합니다.

<mark style="color:red;">- 한정된 라이선스를 슈퍼컴 사용자들이 함께 사용하므로, 사용정책 기준을 초과하여 사용할 경우 해당 작업은 관리자가 강제 종료 합니다.</mark>

<mark style="color:red;">- 부득이하게 많은 라이선스가 필요할 경우,</mark>\ <mark style="color:red;">****</mark><mark style="color:red;">KISTI 홈페이지 (https://www.ksc.re.kr)를 통해 사전에 관리자와 협의해야 합니다.</mark>

<mark style="color:red;">- 로그인 노드의 과부하 방지를 위해 pre/post 작업은 허가하지 않습니다.</mark>

<mark style="color:red;"></mark>

<mark style="color:red;">- 2019년 3월 PM 이후(3월14일)</mark>\ <mark style="color:red;">에는 작업제출 스크립트에 "#PBS -A ansys " 옵션을 사용해야 합니다.</mark>



## **2. 소프트웨어 설치 정보**

**(1) 설치 버전**

\- v145, v170, v181, v191, v192, v195, v201



**(2) 설치 위치**

\- /apps/commercial/ANSYS/(<mark style="color:red;">version</mark>)/fluent

<mark style="color:red;">※ 위 version 의 위치에 사용하기 원하는 fluent 버전, 즉, v145, v170, v181, v191, v192, v195, v201 중의 하나로 교체하기 바랍니다.</mark>



**(3) 실행 파일 경로**

\- /apps/commercial/ANSYS/(<mark style="color:red;">version</mark>)/fluent/bin

<mark style="color:red;">※ 위 version 의 위치에 사용하기 원하는 fluent 버전, 즉, v145, v170, v181, v191, v192, v195, v201 중의 하나로 교체하기 바랍니다.</mark>

<mark style="color:red;"></mark>

## **3. 소프트웨어 실행 방법**

**(1) 실행 방법**

\- 명령어 실행 전 환경설정을 위한 스크립트를 먼저 실행합니다.

(예) fluent v181을 사용하려면, 아래와 같이 수행합니다.

```
$ module load fluent/v181
```

<mark style="color:red;">※ module 환경 설정 모듈 파일이 존재합니다. 위의 예제를 참고하여 알맞은 환경 설정을 하시면 됩니다.</mark>



\- 다음과 같이 batch mode로 job을 실행하기 위한 명령어 입력

| **형식**  | fluent \[**version**] \[-help] \[**option**]                                                                                                   |
| ------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| version | <p>2d (2차원, single precision)<br>3d (3차원, single precision)<br>2ddp (2차원, double precision)<br>3ddp (3차원, double precision)</p>                |
| option  | <p>-g (GUI 없이 실행)<br>-i journal (지정한 journal file을 읽음)<br>-g -i journal ( background 모드로 작업을 수행)<br>-t<strong>x</strong> (프로세서의 수를 x개로 지정)</p> |

\
\- 로그인노드에서 Interactive 방식의 실행은 CPU time이 10분으로 제한되어 있습니다.

\- 장시간의 계산 작업은 스케줄러를 이용하여 작업을 제출해야 합니다.



**(2) FLUENT input(journal) file 예제**

\- /apps/commercial/test\_samples/ANSYS/wst.in 파일을 참조하여 적절히 수정하시기 바랍니다.

```
 (set! *cx-exit-on-error* #t)
 rc wst.cas                                     # read case file
 /solve/init/init                               # initialize the solution
 it 100                                          # calculate 100 iterations
 wd wst.dat                                   # write data file
 exit                                            # exit FLUENT
 yes
```

**(3) 스케쥴러 작업 스크립트 파일 작성**

5호기 시스템에서는 로그인 노드에서 PBS Professional 이라는 스케쥴러를 사용하여 작업을 제출해야 합니다.

5호기 시스템에서 PBS Professional 을 사용하는 예제 파일들이 아래의 경로에 존재하므로 사용자 작업용 파일을 만들 때 이를 참고하시기 바랍니다.

\- 예제 파일 : /apps/commercial/test\_samples/ANSYS/fluent\_v181.sh **(단일노드에서 수행)**\
&#x20;                        ****                         /apps/commercial/test\_samples/ANSYS/fluent\_v181\_multinode.sh **(멀티 노드에서 수행)**



※ 아래는 누리온 시스템에서의 작업제출 예제입니다. **(단일노드에서 수행)**

```
#!/bin/sh
#PBS -V
#PBS -N fluent_job                                  # job 이름 지정
#PBS -q commercial                                  # 큐 지정
#PBS -l select=1:ncpus=40:mpiprocs=40:ompthreads=1  # MPI 태스크 및 Threads 수 지정
#PBS -l walltime=04:00:00                           # 예상 작업 소요 시간 지정
#PBS -A ansys 
 
cd $PBS_O_WORKDIR
 
cpus=`cat $PBS_NODEFILE | wc -l`
 

fluent 3d -pethernet -t${cpus} -g -i wst.in > wst.output
```

\- 위에서 내용을 사용자가 적절히 수정해야 합니다.

<mark style="color:red;">- 2019년 3월 PM 이후</mark>\ <mark style="color:red;">(3월14일)</mark>\ <mark style="color:red;">부터는 "#PBS -A ansys " 옵션이 없는 경우 작업제출이 되지 않습니다.</mark>

\- **작업 제출은 스크래치 디렉토리에서만 가능** 합니다.

\- 사용자별 스크래치 디렉토리는 /scratch/$USER입니다.

<mark style="color:red;">- 스크래치 디스크는 작업 종료 후 일정 시간이 지나면 삭제되기 때문에, 작업이 완료 된 경우 빠른 시일 내에 백업하시길 권장합니다.</mark>

\- 기타 스케쥴러에 관련된 명령어 및 사용법은 누리온 사용자 지침서를 참조하여 주십시오.



## **4. 작업 모니터링**

**(1) 큐 조회**

```
$ showq
```

****

**(2) 노드 상태 조회**

```
$ pbs_status
```

****

**(3) 작업 상태 확인**

```
$ qstat <-a, -n, -s, -H, -x, …>
ex> qstat
Job id Name User Time Use S Queue
-------------------------------------------------------------------
0001.pbcm test_01 user01 8245:43: R commercail
0002.pbcm test_03 user03 7078:45: R commercail
0003.pbcm test_04 user04 1983:11: Q commercail 
```



**(4) 작업 제출 방법**

\- 예제 : 스크립트 파일 이름이 fluent.sh 인 경우

```
$ qsub fluent.sh
```



**(5) 제출된 작업을 강제로 종료**

\- 사용 방법 : qdel \[작업ID]

\- 작업ID는 qsub 명령어 실행 시 제일 왼쪽에 표시 되는 정보입니다.(ex. 0001.pbcm test\_01 user01 8245:43: R norm\_cache)

\- 예제 : 작업ID 가 0001.pbcm 인 경우

```
$ qdel 0001.pbcm
```



**5. 기타**

ANSYS Fluent 버전별 Start Guide 문서

{% file src="../../../.gitbook/assets/ANSYS_Fluent_Getting_Started_Guide_v145.pdf" %}

{% file src="../../../.gitbook/assets/ANSYS_Fluent_Getting_Started_Guide_v170.pdf" %}

{% file src="../../../.gitbook/assets/ANSYS_Fluent_Getting_Started_Guide_v181.pdf" %}

{% file src="../../../.gitbook/assets/ANSYS_Fluent_Getting_Started_Guide_v191.pdf" %}
