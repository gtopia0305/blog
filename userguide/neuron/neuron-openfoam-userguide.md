---
description: 슈퍼컴퓨팅인프라센터 2020. 7. 9. 09:19
---

# 뉴론 OpenFOAM 사용자 지침서

본 문서는 뉴론 시스템에서 OpenFOAM 소프트웨어 사용을 위한 기초적인 정보를 제공하고 있습니다.\
따라서, OpenFOAM 소프트웨어 사용법 및 뉴론/리눅스 사용법 등은 포함되어 있지 않습니다.\
뉴론/리눅스 사용법에 대한 정보는 KISTI 홈페이지 (https://www.ksc.re.kr)의 기술지원 > 지침서 내 뉴론 사용자 지침서 등을 참고하시기 바랍니다.\
OpenFOAM 사용법에 대한 정보는 OpenFOAM 사이트([https://www.openfoam.com](https://www.openfoam.com/), [https://openfoam.org](https://openfoam.org/), [http://openfoamwiki.net](http://openfoamwiki.net/))를 참고하시기 바랍니다.



### **1. 소프트웨어 설치 정보**

**(1) 설치 버전**

\- v1912



**(2) 설치 위치**

| **Version** | **CPU 구분** | **설치 경로**                                                           | **비고** |
| ----------- | ---------- | ------------------------------------------------------------------- | ------ |
| v1912       | SKL        | /apps/applications/OpenFOAM/v1920/intel/18.0.2/impi/18.0.2/OpenFOAM |        |



### **3. 소프트웨어 환경 설정 방법**

**(1) OpenFOAM-v1912 버전**

| <p>$ module load intel/18.0.2 mpi/impi-18.0.2</p><p>$ source /apps/applications/OpenFOAM/v1920/intel/18.0.2/impi/18.0.2/OpenFOAM/OpenFOAM-v1912/etc/bashrc</p> |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------- |



### **4. 스케쥴러 작업 스크립트 파일 작성**

뉴론 시스템에서는 로그인 노드에서 Slurm 스케쥴러를 사용하여 작업을 제출해야 합니다.



※ 아래 예제는 OpenFOAM 의 simpleFoam 에 대한 예제입니다.

| <p>#!/bin/sh</p><p>#SBATCH -J <mark style="color:blue;">OpenFOAM_job</mark></p><p>#SBATCH -p <mark style="color:blue;">skl</mark></p><p>#SBATCH -N <mark style="color:blue;">2</mark> # number of nodes</p><p>#SBATCH -n <mark style="color:blue;">8</mark> # total process</p><p>#SBATCH -o %x_%j.out</p><p>#SBATCH -e %x_%j.err</p><p>#SBATCH --time=<mark style="color:blue;">24:00:00</mark></p><p>#SBATCH --comment <mark style="color:red;">openfoam</mark></p><p><mark style="color:red;"></mark></p><p>srun <mark style="color:blue;">simpleFoam -parallel</mark></p> |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

\- 위에서 파란색으로 표기된 부분은 사용자가 적절히 수정해야 합니다.

<mark style="color:red;">- "#SBATCH --comment openfoam" 옵션이 없는 경우 작업제출이 되지 않습니다.</mark>

\- **작업 제출은 스크래치 디렉토리에서만 가능** 합니다.

<mark style="color:red;">- 스크래치 디스크는 작업 종료 후 일정 시간(2020년 7월 현재 정책: 15일)이 지나면 삭제되기 때문에, 작업이 완료 된경우 빠른 시일 내에 백업하시길 권장합니다.</mark>

\- 기타 Slurm에 관련된 명령어 및 사용법은 뉴론 사용자 지침서를 참조하시면 됩니다.



### **5. 작업 제출 방법**

\- 예제 : 스크립트 파일 이름이 run.sh 인 경우

```
$ sbatch run.sh
```



### **6. 작업 상태 확인**

```
$ squeue (또는 squeue -u $USER) 
```

****

### **7. 제출된 작업을 강제로 종료**

\- 사용 방법 : scancel {작업ID}

\- 작업ID는 qstat 명령어 실행 시 제일 왼쪽에 표시 되는 정보입니다.(ex. 12721)

\- 예제 : 작업ID 가 12721 인 경우

```
$ scancel 12721
```

****

### **8. Slurm 상에서 사용 가능한 자원 확인**

```
$ sinfo
```
