---
description: 슈퍼컴퓨팅인프라센터 2019. 10. 21. 10:42
---

# Gromacs 멀티노드 활용

다음은 뉴론을 활용한 Gromacs 테스트 샘플의 실행 방법 및 성능을 보여주는 예제이다.



## **가. 테스트 계산 모델**

Gromacs (2018.6 버전)의 실행 테스트를 위하여, 프로틴을 모델 시스템으로 사용하여 성능을 테스트하였다.

![](../../.gitbook/assets/99DD5E395DB6361031.png)

****

## **나. 실행 방법 및 성능 분석**

**\[Gromacs 실행 명령 부분]**

> $gmxBin grompp -f opls.mdp -c em20.gro -p topol.top -o md00.tpr
>
> mpirun $gmxBin mdrun -notunepme -ntomp 1 -dlb yes -v -nsteps 40000 -resethway -noconfout -s ${WorkloadPath}/md00.tpr



### **1) 작업 스크립트 예제**

> \#!/bin/sh
>
> \#SBATCH –J gromacs                          <mark style="color:blue;">#job의 이름을 지정</mark>
>
> \#SBATCH –p ivy\_v100\_2                        <mark style="color:blue;"># 사용하고자 하는 파티션을 지정(누리온의 큐와 동일한 개념)</mark>
>
> \#SBATCH –N 1                                   <mark style="color:blue;"># 작업을 할당할 노드의 수</mark>
>
> \#SBATCH –n 18                                  <mark style="color:blue;"># 작업을 위해 할당할 전체 프로세스의 수</mark>
>
> \#SBATCH –o %x.o%j                            <mark style="color:blue;"># 표준 출력을 지정</mark>
>
> \#SBATCH –e %x.e%j                            <mark style="color:blue;"># 표준 오류를 지정</mark>
>
> \#SBATCH --time 10:00:00                    <mark style="color:blue;"># wall time limit을 지정</mark>
>
> \#SBATCH --gres=gpu:2                        <mark style="color:blue;"># 사용할 GPU개수를 지정(현재는 2개 사용하도록 설정됨)</mark>
>
> \#SBATCH --comment gromacs              <mark style="color:blue;"># 사용하는 Application 지정(의무사항)</mark>
>
> &#x20;
>
> module purge
>
> module load intel/18.0.2 cuda/10.0 cudampi/mvapich2-2.3 cmake/3.12.3
>
> &#x20;
>
> ulimit -s unlimited
>
> WorkloadPath=<mark style="color:red;">{작업 경로}</mark>
>
> InstallDir=<mark style="color:red;">{설치 경로}</mark>/bin
>
> gmxBin="${InstallDir}/gmx\_mpi"
>
> &#x20;
>
> \#$gmxBin grompp -f opls.mdp -c em10.gro -p topol.top -o md0.tpr
>
> &#x20;
>
> time -p srun $gmxBin mdrun -notunepme -ntomp 1 -dlb yes -v -nsteps 40000 -resethway -noconfout -s ${WorkloadPath}/md0.tpr

뉴론 시스템은 SLURM을 사용한다. 사용상에서 PBS와 약간의 차이는 있지만, 전체적으로는 유사한 방식으로 작성을 한다. GPU를 이용하기 위해서는 다음과 같은 라인을 추가해야 한다.

\#SBATCH —gres=gpu:1

여기서 1은 GPU 카드를 1장을 사용하겠다는 의미이며, 2개를 사용하기 위해서는 2를 지정하면 된다.

Gromacs는 CPU 계산 부분과 GPU 계산 부분이 함께 존재하므로 전체 프로세스의 개수를 18로 지정하였다.

****

### **2) 계산 성능 결과**

뉴론 시스템은 노드당 Tesla V100카드가 2장 장착된 노드가 36개(gpu\[08-28]/ivy\_v100\_2, gpu\[30-44]/cas\_v100\_2) 있다.

![](../../.gitbook/assets/gromacs\_test\_cal\_perf\_result.png)

![](../../.gitbook/assets/99DFF4335DBF51C516.png)

GPU\_2는 2개의 카드가 장착된 노드를 의미하며, GPU\_1은 1개의 카드가 장착된 노드를 의미한다.

GPU\_2인 인 경우 노드를 3개 사용할 때 실행되지 않았으며, GPU\_1인 경우 1개의 노드를 사용할 때 프로그램이 실행되지 않았다. 2개의 노드를 사용하는 경우에만 비교가 가능한데, 2개의 GPU를 사용하는 경우 약간의 성능이득이 있음을 알 수 있다.

※ KNL, SKL 시스템과의 비교는 "누리온 Gromacs 멀티노드 활용 (KNL)" 참조 ([https://blog.ksc.re.kr/163?category=688349](https://blog.ksc.re.kr/163?category=688349))
