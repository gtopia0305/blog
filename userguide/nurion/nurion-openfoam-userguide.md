---
description: 슈퍼컴퓨팅인프라센터 2019. 5. 28. 15:17
---

# 누리온 OpenFOAM 사용자 지침서

본 문서는 누리온 시스템에서 OpenFOAM 소프트웨어 사용을 위한 기초적인 정보를 제공하고 있습니다.

따라서, OpenFOAM 소프트웨어 사용법 및 누리온/리눅스 사용법 등은 포함되어 있지 않습니다.

누리온/리눅스 사용법에 대한 정보는 KISTI 홈페이지 (https://www.ksc.re.kr)의 기술지원 > 지침서 내 누리온 사용자 지침서 등을 참고하시기 바랍니다.

OpenFOAM 사용법에 대한 정보는 OpenFOAM 사이트([https://www.openfoam.com](https://www.openfoam.com/), [https://openfoam.org](https://openfoam.org/), [http://openfoamwiki.net](http://openfoamwiki.net/))를 참고하시기 바랍니다.



### **1. 소프트웨어 설치 정보**

**(1) 설치 버전**

\- 4.1(KNL, SKL), 5.0(KNL, SKL), 7.0(KNL, SKL), v1912(KNL, SKL)



**(2) 설치 위치**

![](../../../.gitbook/assets/install\_location.png)



### **3. 소프트웨어 환경 설정 방법**

**(1) OpenFOAM-4.1 KNL 버전**

| <p>$ module load intel/17.0.5 impi/17.0.5</p><p>$ source /apps/applications/OpenFOAM/4.1/intel/17.0.5/impi/17.0.5/mic-knl/OpenFOAM/OpenFOAM-4.1/etc/bashrc WM_MPLIB=MPI</p> |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |



**(2) OpenFOAM-4.1 SKL 버전**

| <p>$ module load intel/17.0.5 impi/17.0.5</p><p>$ source /apps/applications/OpenFOAM/4.1/intel/17.0.5/impi/17.0.5/x86-skylake/OpenFOAM/OpenFOAM-4.1/etc/bashrc WM_MPLIB=MPI</p> |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |



**(3) OpenFOAM-5.0 KNL 버전**

| <p>$ module load intel/18.0.3 impi/18.0.3</p><p>$ source /apps/applications/OpenFOAM/5.0/intel/18.0.3/impi/18.0.3/mic-knl/OpenFOAM/OpenFOAM-5.0/etc/bashrc</p> |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------- |



**(4) OpenFOAM-5.0 SKL 버전**

| <p>$ module load intel/18.0.3 impi/18.0.3</p><p>$ source /apps/applications/OpenFOAM/5.0/intel/18.0.3/impi/18.0.3/x86-skylake/OpenFOAM/OpenFOAM-5.0/etc/bashrc</p> |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |



**(5) OpenFOAM-7.0 KNL 버전**

| <p>$ module load intel/18.0.3 impi/18.0.3</p><p>$ source /apps/applications/OpenFOAM/7.0/intel/18.0.3/impi/18.0.3/mic-knl/OpenFOAM/OpenFOAM-7/etc/bashrc</p> |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------ |



**(6) OpenFOAM-7.0 SKL 버전**

| <p>$ module load intel/18.0.3 impi/18.0.3</p><p>$ source /apps/applications/OpenFOAM/7.0/intel/18.0.3/impi/18.0.3/x86-skylake/OpenFOAM/OpenFOAM-7/etc/bashrc</p> |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |



**(7) OpenFOAM-v1912 KNL 버전**

| <p>$ module load intel/18.0.3 impi/18.0.3</p><p>$ source /apps/applications/OpenFOAM/v1912/intel/18.0.3/impi/18.0.3/mic-knl/OpenFOAM/OpenFOAM-v1912/etc/bashrc</p> |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |



**(8) OpenFOAM-v1912 SKL 버전**

| <p>$ module load intel/18.0.3 impi/18.0.3</p><p>$ source /apps/applications/OpenFOAM/v1912/intel/18.0.3/impi/18.0.3/x86-skylake/OpenFOAM/OpenFOAM-v1912/etc/bashrc</p> |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |



### **4. 스케쥴러 작업 스크립트 파일 작성**

누리온 시스템에서는 로그인 노드에서 PBS 라는 스케쥴러를 사용하여 작업을 제출해야 합니다.



※ 아래 예제는 OpenFOAM 의 simpleFoam 에 대한 예제입니다.

| <p>#!/bin/sh</p><p>#PBS -V</p><p>#PBS -N <mark style="color:blue;">OpenFOAM_job</mark></p><p>#PBS -q <mark style="color:blue;">normal</mark></p><p>#PBS -l select=2:ncpus=<mark style="color:blue;">32</mark>:mpiprocs=<mark style="color:blue;">32</mark>:ompthreads=<mark style="color:blue;">1</mark></p><p>#PBS -l walltime=<mark style="color:blue;">04:00:00</mark></p><p><mark style="color:red;">#PBS -A openfoam</mark></p><p></p><p>cd $PBS_O_WORKDIR</p><p></p><p>mpirun <mark style="color:blue;">simpleFoam -parallel</mark></p> |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

\- 위에서 파란색으로 표기된 부분은 사용자가 적절히 수정해야 합니다.

<mark style="color:red;">- "#PBS -A openfoam" 옵션이 없는 경우 작업제출이 되지 않습니다.</mark>

\- **작업 제출은 스크래치 디렉토리에서만 가능** 합니다.

\- 사용자별 스크래치 디렉토리는 /scratch/$USER입니다.

<mark style="color:red;">- 스크래치 디스크는 작업 종료 후 일정 시간(2020년 4월 현재 정책: 15일)이 지나면 삭제되기 때문에, 작업이 완료 된경우 빠른 시일 내에 백업하시길 권장합니다.</mark>

\- 기타 PBS에 관련된 명령어 및 사용법은 누리온 사용자 지침서를 참조하시면 됩니다.



### **5. 작업 제출 방법**

\- 예제 : 스크립트 파일 이름이 run.sh 인 경우

```
$ qsub run.sh
```



### **6. 작업 상태 확인**

```
$ qstat (또는 qstat -u $USER) 
```



### **7. 제출된 작업을 강제로 종료**

\- 사용 방법 : qdel {작업ID}

\- 작업ID는 qstat 명령어 실행 시 제일 왼쪽에 표시 되는 정보입니다.(ex. 1771476.pbs)

\- 예제 : 작업ID 가 1771476.pbs 인 경우

```
$ qdel 1771476.pbs
```

### **8. PBS 상에서 사용 가능한 자원 확인**

```
$ pbs_status
```
