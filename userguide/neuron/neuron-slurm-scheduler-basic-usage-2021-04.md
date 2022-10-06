---
description: 슈퍼컴퓨팅인프라센터 2019. 6. 14. 15:00
---

# 뉴론 slurm 스케쥴러 기본 사용법 (2021.04)

뉴론 시스템은 스케쥴러로 slurm 을 사용하고 있습니다. 이 문서에서는 사용자가 뉴론 시스템에서 기본적인 작업 제출을 할 수 있는데 목적을 두고 있으며, MPI 작업 제출 등은 별도의 문서에서 설명할 계획입니다. 그 밖의 자세한 정보는 아래를 참고해 주시기 바랍니다.

\[ slurm 온라인 문서 ]https://slurm.schedmd.com/documentation.html

## **1. 기본명령어 요약**

| **명령어**               | **내용**           |
| --------------------- | ---------------- |
| $ sbatch \[옵션..] 스크립트 | 작업 제출            |
| $ scancel 작업ID        | 작업 삭제            |
| $ squeue              | 작업 상태 확인         |
| $ smap                | 작업 상태 및 노드 상태 확인 |
| $ sinfo \[옵션..]       | 노드 정보 확인         |

## **2. sinfo**

slurm 노드 및 파티션 정보를 조회합니다. 사용방법 및 예제는 아래와 같습니다.

```
$ sinfo 
PARTITION        AVAIL  TIMELIMIT  NODES  STATE NODELIST
ivy_k40_2         up 5-00:00:00      4   idle gpu[01-03,07]
jupyter           up 1-02:00:00      3   idle gpu[04-06]
ivy_v100_2        up 5-00:00:00     21   idle gpu[08-28]
ivy_v100-16G_2    up 5-00:00:00     11   idle gpu[08-18]
ivy_v100-32G_2    up 5-00:00:00     10   idle gpu[19-28]
cas_v100_2        up 5-00:00:00     14   idle gpu[30-41,43-44]
cas_v100nv_4      up 5-00:00:00      4   idle gpu[45-48]
cas32c_v100_2     up 5-00:00:00      5   idle gpu[54-58]
skl                up 3-00:00:00    10   idle skl[01-10]
bigmem            up 3-00:00:00      2   idle bigmem[01-02]
amd               up 3-00:00:00      2   idle amd[01-02]
optane            up 3-00:00:00      1   idle optane01
```

**※ GPU 메모리 크기를 특정할 필요가 없다면 ivy\_v100\_2 파티션에 작업을 제출하고, GPU 메모리 크기를 특정하고자 할 경우 ivy\_v100-16G\_2 혹은 ivy\_v100-32G\_2 파티션을 선택하여 작업 제출**

**※ cas32c\_v100\_2 파티션 계산노드는 Xeon Gold 6242 CPU 2ea 탑재 (총 32코어)**

****

## **3. sbatch**

작업 제출을 위한 명령어입니다. 사용방법 및 예제는 아래와 같습니다.

```
$ sbatch ./job_script.sh
```

\[작업스크립트 예제]

```
#!/bin/sh
#SBATCH -J test          # 작업 이름         
#SBATCH -p cas_v100_2    # partition 이름
#SBATCH -N 2             # 총 필요한 프로세스 수
#SBATCH -n 2             # 총 필요한 프로세스 수
#SBATCH -o %x.o%j        # stdout 파일 명(.o)
#SBATCH -e %x.e%j        # stderr 파일 명(.e)
#SBATCH --comment        # Application별 SBATCH 옵션 아래 작업스크립트 작성 안내 참고)
#SBATCH --time 00:30:00  # 최대 작업 시간 (Wall Time Clock Limi
#SBATCH --gres=gpu:2     # GPU를 사용하기 위한 옵션

srun ./run.x             # srun 사용
```



\[작업스크립트 작성 안내]

slurm에게 전달해야 할 정보는 작업스크립트 내에 #SBATCH 지시자를 붙여 전달합니다.



(1) 작업 이름

작업 이름을 명시하지 않으면 slurm이 임의로 부여합니다..\
작업 이름을 test 로 지정할 경우 "--job-name=test" 또는 "-J test" 와 같이 사용합니다.



(2) 작업 파티션 이름

작업 수행할 파티션을 지정하는 것이며, 사용가능한 파티션 이름은 sinfo 명령어로 확인이 가능합니다.\
파티션을 ivy\_v100\_2으로 사용할 경우, "--partition=ivy\_v100\_2" 또는 "-p ivy\_v100\_2" 와 같이 사용합니다.



(3) 필요한 자원의 양

작업 수행에 필요한 총 프로세스 수는 "-n" 옵션으로, 작업 수행에 필요한 컴퓨팅 노드 개수는 "-N"으로 정의합니다.



(4) output

Standard output을 저장할 파일명을 정의합니다.\
파일명을 test.out으로 사용할 경우, "--output=test.out" 또는 "-o test.out" 과 같이 사용합니다.



(5) error

Standard error 를 저장할 파일명을 정의합니다.\
파일명을 test.err으로 사용할 경우, "--error=test.err" 또는 "-e test.err" 와 같이 사용합니다.



(6) comment

뉴론 시스템 사용자 편익 증대를 위한 자료 수집의 목적으로, 아래와 같이 SBATCH 옵션을 통한 사용 프로그램 정보 작성을 의무화한다.

| Application종류 | SBATCH 옵션 이름 | Application종류    | SBATCH 옵션 이름 |
| ------------- | ------------ | ---------------- | ------------ |
| PYTHON        | python       | LAMMPS           | lammps       |
| Charmm        | charmm       | NAMD             | namd         |
| Gaussian      | gaussian     | Quantum Espresso | qe           |
| OpenFoam      | openfoam     | SIESTA           | siesta       |
| WRF           | wrf          | Tensorflow       | tensorflow   |
| in-house code | inhouse      | Caffe            | caffe        |
| R             | R            | Pytorch          | pytorch      |
| VASP          | vasp         | Sklearn          | sklearn      |
| Gromacs       | gromacs      | 그 외 applications | etc          |



(7) Wall Time Clock Limit

예상되는 작업 소요 시간을 의미하며, 실제 예상되는 작업 소요 시간보다 약간 더 길게 설정해 주는 것이 안전합니다.\
만약 해당 작업이 설정해 둔 시간내에 종료 되지 않을 경우, Wall Time Clock Limit 시간을 초과하는 시점에서 slurm이 해당 작업을 강제로 종료시킵니다. 이 키워드는 작업 제출 스크립트 파일에 반드시 지정되어야 하며, 초단위 또는 시:분:초 형식으로 지정할 수 있습니다.\
Wall Time Clock Limit을 1시간을 지정할 경우, "--time=01:00:00: 또는 "-t 01:00:00" 과 같이 사용합니다.

****

## **4. squeue**

제출된 작업 목록 및 정보 조회 명령어입니다. 사용방법 및 예제는 아래와 같습니다.

```
$ squeue 
        JOBID  PARTITION     NAME     USER    ST TIME    NODES  NODELIST(REASON)
        1166   ivy_k40_2     gpu_burn userid  R  2:26:04 1        gpu02
        1167   ivy_v100_2    gpu_burn userid  R  2:25:54 1        gpu19
        1168   cas_v100_2    gpu_burn userid  R  2:25:44 1        gpu30
```



\[제출된 작업 상세 조회]

scontrol 명령어를 이용하면 제출된 작업의 상세내역을 조회할 수 있습니다. 사용방법 및 예제는 아래와 같습니다.

```
$ scontrol show job [작업 ID]
```

\- 예제 -

```
$ scontrol show job 3217
 JobId=3217 JobName=ssw_test
   UserId=moasys1(100001107) GroupId=in0011(1000011) MCS_label=N/A
   Priority=4294901630 Nice=0 Account=kat_user QOS=normal
   JobState=RUNNING Reason=None Dependency=(null)
   Requeue=1 Restarts=0 BatchFlag=1 Reboot=0 ExitCode=0:0
   RunTime=00:00:05 TimeLimit=01:00:00 TimeMin=N/A
   SubmitTime=2018-04-30T17:54:07 EligibleTime=2018-04-30T17:54:07
   StartTime=2018-04-30T17:54:07 EndTime=2018-04-30T18:54:08 Deadline=N/A
   PreemptTime=None SuspendTime=None SecsPreSuspend=0
   Partition=ivy_v100_2 AllocNode:Sid=login-tesla02:9203
   ReqNodeList=(null) ExcNodeList=(null)
   NodeList=tesla[03-04]
   BatchHost=tesla03
   NumNodes=2 NumCPUs=40 NumTasks=4 CPUs/Task=1 ReqB:S:C:T=0:0:*:*
   TRES=cpu=40,node=2,gres/gpu=2
   Socks/Node=* NtasksPerN:B:S:C=0:0:*:* CoreSpec=*
   MinCPUsNode=1 MinMemoryNode=0 MinTmpDiskNode=0
   Features=(null) DelayBoot=00:00:00
   Gres=gpu Reservation=(null)
   OverSubscribe=NO Contiguous=0 Licenses=(null) Network=(null)
   Command=./kat-2.sh
   WorkDir=/scratch2/moasys1/ssw/moasys1/kat_test
   StdErr=/scratch2/moasys1/ssw/moasys1/kat_test/ssw.e3217
   StdIn=/dev/null
   StdOut=/scratch2/moasys1/ssw/moasys1/kat_test/ssw.o3217
   Power=
```



## **5. scancel**

제출된 작업 수행을 취소합니다. 사용방법은 아래와 같습니다.

```
$ scancel [작업 ID]
```



## **6. smap**

sinfo와 squeue에서 보여주는 정보를 그래픽화면으로 보여줍니다.&#x20;



## **7. 인터렉티브 작업 제출**

(1) 자원 할당

\* 설명 : ivy\_v100\_2 파티션의 gpu 2노드(각각 2core, 2gpu)를 interactive 용도로 사용

```
$ salloc --partition=ivy_v100_2 -N 2 -n 4 --tasks-per-node=2 --gres=gpu:2 --comment={SBATCH 옵션이름} 
```

※ Application별 SBATCH 옵션 이름표 참고

<mark style="color:red;">**※ 2시간 이상 미사용시 타임아웃으로 작업이 종료되고 자원이 회수됨, 인터렉티브 작업의 walltime은 최대 12시간으로 고정됨**</mark>

<mark style="color:red;">****</mark>

(2) 작업 실행

```
$ srun ./(실행파일) (실행옵션) 
```



(3) 헤드 노드 접속

```
$ srun --pty bash 
```

<mark style="color:red;">**※ 2시간 이상 키보드 미입력시 타임아웃으로 작업이 종료되고 자원이 회수됨**</mark>\ <mark style="color:red;"></mark><mark style="color:red;">**※ 헤드 노드에 접속한 후에는 srun, mpirun을 통한 작업 제출 불가능, 헤드 노드에서 빠져나온 후(exit)에 작업 제출이 가능함**</mark>



(4) 진입한 노드에서 나가기 또는 자원 할당 취소

```
$ exit
```



(5) 커맨드를 통한 작업 삭제

```
$ scancel [Job_ID]
```

**※ Job ID는 squeue 명령으로 확인 가능**
