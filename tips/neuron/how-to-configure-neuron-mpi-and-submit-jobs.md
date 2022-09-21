---
description: 슈퍼컴퓨팅인프라센터 2019. 4. 30. 09:52
---

# MPI 환경설정 및 작업 제출 방법(2021.03)

뉴론 시스템에는 mvapich2와 openmpi가 설치되어 있습니다.  이 문서에서는 MPI 기반 작업들을 slurm 스케쥴러를 이용해 작업 제출하는 방법에 대해 기술하고 있습니다.&#x20;

&#x20;

## **1. mvapich2로 빌드된 애플리케이션의 작업 제출**&#x20;

뉴론 시스템에 설치되어 있는 mvapich2를 활용하기 위해서는 아래와 같은 module 명령으로 사용가능한 모듈 목록 및 모듈 사용법을 확인합니다.&#x20;

&#x20;2019년 5월 현재 뉴론 시스템에는 mvapich2-2.3이 설치되어 있으며,  이것은 gcc-4.8.5, intel-18.0.2, pgi-19.1 컴파일러로 빌드한 버전들이 존재합니다.   이를 사용하기 위해서는 위에 언급된 바와 같이 다음과 같이 module 명령어를 사용합니다.&#x20;

> $ module load intel/18.0.2 cuda/10.0 cudampi/mvapich2-2.3

&#x20;

작업제출 방법(배치 작업용)은 다음과 같습니다.&#x20;

> $ sbatch ./job\_script.sh&#x20;

&#x20;

\[작업스크립트 예제]

| <p>#!/bin/sh</p><p>#SBATCH -J test</p><p>#SBATCH -p ivy_v100_2</p><p>#SBATCH -N 2</p><p>#SBATCH -n 2</p><p>#SBATCH -o test.o%j</p><p>#SBATCH -e test.e%j</p><p>#SBATCH --time 00:30:00</p><p>#SBATCH --gres=gpu</p><p> </p><p>srun ./hello</p> | <p><br><mark style="color:orange;"><strong># 작업 이름</strong></mark><br><mark style="color:orange;"><strong># partition 이름</strong></mark><br><mark style="color:orange;"><strong># 총 필요한 컴퓨팅 노드 수</strong></mark><br><mark style="color:orange;"><strong># 총 필요한 프로세스 수</strong></mark><br><mark style="color:orange;"><strong># stdout 파일 명</strong></mark><br><mark style="color:orange;"><strong># stderr 파일 명</strong></mark><br><mark style="color:orange;"><strong># 최대 작업 시간 (Wall Time Clock Limit)</strong></mark></p><p><mark style="color:orange;"><strong># GPU를 사용하기 위한 옵션</strong></mark></p><p> <mark style="color:orange;"><strong></strong></mark> </p><p><mark style="color:orange;"><strong># hello는 MPI기반 어플리케이션</strong></mark> </p> |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |



인터랙티브 작업 제출 방법은 다음과 같습니다.



(1) 자원 할당

\* 설명 : ivy\_v100\_2 파티션의 gpu 2노드(각각 2core, 2gpu)를 interactive 용도로 사용

> $ salloc --partition=ivy\_v100\_2 -N 2 -n 4 --tasks-per-node=2 --gres=gpu:2 --comment={SBATCH 옵션이름}&#x20;

※ Application별 SBATCH 옵션 이름표 참고

<mark style="color:red;">**※ 2시간 이상 미사용시 타임아웃으로 작업이 종료되고 자원이 회수됨, 인터렉티브 작업의 walltime은 최대 12시간으로 고정됨**</mark>



(2) 작업 실행

> $ srun ./(실행파일) (실행옵션)&#x20;



(3) 헤드 노드 접속

> $ srun --pty bash&#x20;

<mark style="color:red;">**※ 2시간 이상 키보드 미입력시 타임아웃으로 작업이 종료되고 자원이 회수됨**</mark>\ <mark style="color:red;"></mark><mark style="color:red;">**※ 헤드 노드에 접속한 후에는 srun을 통한 작업 제출 불가능**</mark>



(4) 진입한 노드에서 나가기 또는 자원 할당 취소

> $ exit

&#x20;

(5) 커맨드를 통한 작업 삭제

> $ scancel \[Job\_ID]

**※ Job ID는 squeue 명령으로 확인 가능**



## 2. openmpi로 빌드된 애플리케이션의 작업 제출&#x20;

mvapich2와 마찬가지로 뉴론 시스템에 설치된 openmpi를 사용하기 위해서는 다음과 같은 module 명령을 이용해 사용가능한 목록 및 사용방법을 확인합니다.&#x20;

> **$ module av**
>
> &#x20;
>
> \----------------------------------------------- /apps/Modules/modulefiles/compilers -----------------------------------------------
>
> gcc/4.8.5    intel/18.0.2 pgi/19.1
>
> &#x20;
>
> \----------------------------------------------- /apps/Modules/modulefiles/libraries -----------------------------------------------
>
> hdf4/4.2.13  hdf5/1.10.2  lapack/3.7.0 netcdf/4.6.1
>
> &#x20;
>
> \-------------------------------------------------- /apps/Modules/modulefiles/mpi --------------------------------------------------
>
> cudampi/mvapich2-2.3  cudampi/openmpi-3.1.0 mpi/impi-18.0.2       mpi/mvapich2-2.3      mpi/openmpi-3.1.0
>
> &#x20;
>
> \------------------------------------------ /apps/Modules/modulefiles/libraries\_using\_mpi ------------------------------------------
>
> fftw\_mpi/2.1.5 fftw\_mpi/3.3.7
>
> &#x20;
>
> \--------------------------------------------- /apps/Modules/modulefiles/applications ----------------------------------------------
>
> cmake/3.12.3        gaussian/g16.b01    java/openjdk-11.0.1 python/2.7.15       qe/6.4.1\_v100       singularity/3.6.4
>
> cuda/10.0           gaussian/g16.c01    lammps/16Mar18      python/3.7.1        R/3.5.0
>
> gaussian/g16        gromacs/2016.4      namd/2.12           qe/6.4.1\_k40        singularity/3.1.0
>
> &#x20;
>
> \-------------------------------------------- /apps/Modules/modulefiles/conda\_packages ---------------------------------------------
>
> conda/caffe\_1.0       conda/pytorch\_1.0     conda/tensorflow\_1.13



2019년 5월 현재 뉴론 시스템에는 openmpi-3.1.0 가 설치되어 있으며, 이것은 gcc-4.8.5, intel-18.0.2, pgi-19.1 컴파일러로 빌드되어 있습니다. 이를 사용하기 위해서는 다음과 같은 module 명령어를 사용합니다.&#x20;

> $ module load intel/18.0.2 cuda/10.0 cudampi/openmpi-3.1.0

작업제출 방법(배치 작업용)은 다음과 같습니다.&#x20;

> $ sbatch ./job\_script.sh&#x20;

&#x20;

\[작업스크립트 예제]

| <p>#!/bin/sh</p><p>#SBATCH -J test</p><p>#SBATCH -p ivy_v100_2</p><p>#SBATCH -N 2</p><p>#SBATCH -n 2</p><p>#SBATCH -o test.o%j</p><p>#SBATCH -e test.e%j</p><p>#SBATCH --time 00:30:00</p><p>#SBATCH --gres=gpu</p><p> </p><p>srun ./hello</p> | <p><br><mark style="color:orange;"><strong># 작업 이름</strong></mark><br><mark style="color:orange;"><strong># partition 이름</strong></mark><br><mark style="color:orange;"><strong># 총 필요한 컴퓨팅 노드 수</strong></mark><br><mark style="color:orange;"><strong># 총 필요한 프로세스 수</strong></mark><br><mark style="color:orange;"><strong># stdout 파일 명</strong></mark><br><mark style="color:orange;"><strong># stderr 파일 명</strong></mark><br><mark style="color:orange;"><strong># 최대 작업 시간 (Wall Time Clock Limit)</strong></mark></p><p><mark style="color:orange;"><strong># GPU를 사용하기 위한 옵션</strong></mark></p><p> <mark style="color:orange;"><strong></strong></mark> </p><p><mark style="color:orange;"><strong># hello는 MPI기반 어플리케이션</strong></mark> </p> |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |



인터랙티브 작업 제출 방법은 다음과 같습니다.



(1) 자원 할당

\* 설명 : ivy\_v100\_2 파티션의 gpu 2노드(각각 2core, 2gpu)를 interactive 용도로 사용

> $ salloc --partition=ivy\_v100\_2 -N 2 -n 4 --tasks-per-node=2 --gres=gpu:2 --comment={SBATCH 옵션이름}&#x20;

※ Application별 SBATCH 옵션 이름표 참고

<mark style="color:red;">**※ 2시간 이상 미사용시 타임아웃으로 작업이 종료되고 자원이 회수됨, 인터렉티브 작업의 walltime은 최대 12시간으로 고정됨**</mark>



(2) 작업 실행

> $ srun ./(실행파일) (실행옵션)&#x20;



(3) 헤드 노드 접속

> $ srun --pty bash&#x20;

<mark style="color:red;">**※ 2시간 이상 키보드 미입력시 타임아웃으로 작업이 종료되고 자원이 회수됨**</mark>\ <mark style="color:red;"></mark><mark style="color:red;">**※ 헤드 노드에 접속한 후에는 srun을 통한 작업 제출 불가능**</mark>



(4) 진입한 노드에서 나가기 또는 자원 할당 취소

> $ exit



(5) 커맨드를 통한 작업 삭제

> $ scancel \[Job\_ID]

**※ Job ID는 squeue 명령으로 확인 가능**
