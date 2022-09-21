---
description: 슈퍼컴퓨팅인프라센터 2020. 6. 12. 10:05
---

# 인터렉티브 작업 제출 예제

KISTI 슈퍼컴퓨터 시스템에서는 기본적으로 스케줄러를 이용하여 작업을 제출하고 계산을 수행하도록 되어 있지만, 코드 개발이나 컴파일, 디버깅을 위해 자원을 할당받아 인터렉티브 모드로 작업을 수행할 수도 있다. 누리온과 뉴론은 각각 다른 스케줄러를 사용하고 있기 때문에 계산 자원을 할당받아 사용하는 명령은 다르다.&#x20;

&#x20;<mark style="color:red;">※ 2020년 6월 PM 이후, 2시간 이상 미사용시 타임아웃으로 작업이 종료되고 할당 받은 자원이 회수되며 인터렉티브 작업의 walltime은 최대 12시간으로 고정된다.</mark>

&#x20;<mark style="color:red;">※ 인터렉티브 작업을 포함한 모든 작업 제출은 scratch 디렉토리에서 제출되어야 한다.</mark>



## ■ 누리온 인터렉티브 작업 제출

&#x20;○ 인터렉티브 작업 제출

> $ qsub **-I** -l select=1:ncpus=64:ompthreads=1 -l walltime=02:00:00 -q {큐 이름} -A {PBS 옵션 이름}



○ 인터렉티브 작업 제출 시 그래픽 환경 사용 (-X)

> $ qsub -I **-X** -l select=1:ncpus=64:ompthreads=1 -l walltime=02:00:00 -q {큐 이름} -A {PBS 옵션 이름}



○ 인터렉티브 작업 제출 시 기존 환경변수 상속 (-V)

> $ qsub -I **-V** -l select=1:ncpus=64:ompthreads=1 -l walltime=02:00:00 -q {큐 이름} -A {PBS 옵션 이름}

&#x20;※ Application별 PBS 옵션 이름은 누리온 사용자 지침서를 참고하길 바람

&#x20;   (누리온 사용자 지침서 : [www.ksc.re.kr/gsjw/jcs/hd](https://www.ksc.re.kr/gsjw/jcs/hd))



## ■ 뉴론 인터렉티브 작업 제출

&#x20;○ 인터렉티브 작업 제출

(1) 자원 할당

\* 설명 : ivy\_v100\_2 파티션의 gpu 2노드(각각 2core, 2gpu)를 interactive 용도로 사용

> &#x20;$ salloc --partition=ivy\_v100\_2 -N 2 -n 4 --tasks-per-node=2 --gres=gpu:2 --comment={SBATCH 옵션이름}&#x20;

※ Application별 SBATCH 옵션 이름표 참고

<mark style="color:red;">**※ 2시간 이상 미사용시 타임아웃으로 작업이 종료되고 자원이 회수됨, 인터렉티브 작업의 walltime은 최대 12시간으로 고정됨**</mark>



(2) 작업 실행

> &#x20;$ srun ./(실행파일) (실행옵션)&#x20;



(3) 헤드 노드 접속

> &#x20;$ srun --pty bash&#x20;

<mark style="color:red;">**※ 2시간 이상 키보드 미입력시 타임아웃으로 작업이 종료되고 자원이 회수됨**</mark>\ <mark style="color:red;"></mark><mark style="color:red;">**※ 헤드 노드에 접속한 후에는 srun을 통한 작업 제출 불가능**</mark>



(4) 진입한 노드에서 나가기 또는 자원 할당 취소

> &#x20;$ exit



(5) 커맨드를 통한 작업 삭제

> &#x20;$ scancel \[Job\_ID]

**※ Job ID는 squeue 명령으로 확인 가능**



## ■ 인터렉티브 작업 예제

> &#x20;$ qsub -I -l select=1:ncpus=64:ompthreads=1 -l walltime=02:00:00 -q normal -A etc\
> qsub: waiting for job 5465046.pbs to start\
> qsub: job 5465046.pbs ready\
> \
> \[support@node4091 job\_examples]$ module purge\
> \[support@node4091 job\_examples]$ module load craype-mic-knl intel/18.0.3 impi/18.0.3\
> \[support@node4091 job\_examples]$ mpiicc -o test\_mpi.exe -O3 -fPIC -xCOMMON-AVX512 test\_mpi.c\
> \[support@node4091 job\_examples]$ mpirun -np 4 ./test\_mpi.exe\
> \
> Hello World from Node 0\
> Hello World from Node 1\
> Hello World from Node 2\
> Hello World from Node 3

&#x20;※ 뉴론의 경우, 작업 제출 방법과 module 이름이 다르니 자세한 내용은 뉴론 사용자 지침서 참고 바람

&#x20;   (뉴론 인터렉티브 작업제출 동영상 가이드 : [blog.ksc.re.kr/189](https://blog.ksc.re.kr/189))
