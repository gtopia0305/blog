---
description: 슈퍼컴퓨팅인프라센터 2021. 5. 11. 09:16
---

# 뉴론 FAQ (2021.05)

## **Neuron\[가속기] FAQ 모음**

### **# 배치작업 (11)**

Q. 계산 노드에서 작업 실행은 어떻게 하나요?

A.\
계산 노드에서의 작업 실행은 작업 스케줄러인 SLURM를 통해 수행하실 수 있습니다.\
작업에 따른 스크립트 예제는 뉴론 사용자 지침서를 참고바랍니다.\
\
뉴론 사용자 지침서는 KISTI 슈퍼컴퓨팅센터 홈페이지([www.ksc.re.kr](https://www.ksc.re.kr/))에서 기술지원 > 지침서 > 하드웨어 > 뉴론 > "스케줄러(SLURM)을 통한 작업 실행" 에서 확인 가능합니다.\
\
job\_script를 작성하시고 sbatch 명령을 통해 작업을 올리실 수 있습니다.\
\
$ sbatch mpi.sh



Q. 제출 한 작업의 진행 상태를 알고 싶어요.

A.\
작업 상태를 알려주는 명령은 squeue 입니다.\
$ squeue\
\
\- 사용자 ID로 확인 방법\
$ squeue -u {사용자 ID}\
\
\- 작업 ID로 확인 방법\
$ squeue -j {작업ID}



Q. 노드별 상세 정보를 알고 싶어요.

A.\
sinfo -Nel 명령을 통해 노드별 상세 정보를 출력할 수 있습니다.



Q. 작업 스크립트 예제에 #SBATCH 로 되어 있는데, #은 주석 아닌가요?

A.\
일반적으로 스크립트 파일에서는 #이 주석으로 인식하지만\
\#SBATCH 로 지정하고 스케줄러로 작업을 제출 하면 SLURM 스케줄러가 지시어로 인식 합니다.



Q. 작업을 제출했는데 작업이 멈추어 있어요.

A.\
squeue 명령으로 제출한 작업 상태를 확인해 주세요.\
\
"PD" 상태는 작업 제출 시 지정된 자원을 확보하는 과정에 대기 상태 입니다.\
"E" 상태로 대기 상태가 되어있으면 기술지원 > 기술지원(상담) > 상담으로 문의글을 남겨 주세요.



Q. 큐에 몇개까지 작업을 제출할 수 있나요?

A.\
작업 제출 정책은 사용자들의 사용량에 따라 변경될 수 있습니다.\
시스템을 접속할때 출력되는 공지사항에서 확인할 수 있고, 접속해 있는 상태 에서는 motd 명령으로 공지사항을 다시 확인 할 수 있습니다.



Q. GPU는 최대 몇 개까지 점유 가능한가요?

A.\
작업 제출 정책은 사용자들의 사용량에 따라 변경될 수 있습니다.\
시스템을 접속할때 뜨는 공지사항에서 확인할 수 있고,\
접속해 있는 상태 에서는 motd 명령으로 공지사항을 다시 확인 할 수 있습니다.



Q. GPU를 1개만 설정하는 법과 2개 모두 사용하도록 설정하는 방법을 알고 싶어요.

A.\
\#SBATCH --gres=gpu 또는 #SBATCH --gres=gpu:1로 작업스크립트를 작성하시는 경우 GPU 를 하나만 사용하며,\
\#SBATCH --gres=gpu:2로 작업스크립트에 작성하시는 경우 GPU를 2개 모두 사용하게 됩니다.



Q. tensorflow의 컨테이너 인스턴스를 띄우지 않고 배치 작업으로 수행하고 싶어요.

A.\
뉴론 시스템에서의 기계학습 프레임워크는 anaconda 환경을 사용하는 것을 권장합니다.\
다음과 같이 module 명령어를 이용하여 환경변수를 설정한 후 배치작업으로 수행이 가능합니다.\
\
$ module load(add) conda/tensorflow\_1.13\
\
사용자가 원하는 환경의 conda 패키지 활용방법은 블로그([https://blog.ksc.re.kr/127](https://blog.ksc.re.kr/127))를 통해 확인하실수 있습니다.



Q. 작업을 제출하면 "/bin/sh^M: bad interpreter: No such file or directory" 오류가 나와요.

A.\
"^M" 표시는 DOS(Windows) 형식 파일의 줄바꿈 표시 입니다.\
Linux(Unix) 에서 DOS 형식의 줄바꿈 표시를 문자로 인식해서 생기는 문제 입니다.\
\
dos2unix 명령을 이용해서 unix 형식으로 변경해서 사용해 주세요.\
\
(예) dos2unix mpi.sh



Q. Interactive Job 사용법을 알고 싶어요.

A.\
Interactive Job 작업의 수행 방법은 아래를 참고해주세요.

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

<mark style="color:red;">****</mark>

### **#시스템 접속 (7)**

Q. 뉴론에 접속하는 방법은 어떻게 되나요?

A.\ <mark style="color:red;">****</mark>ssh 클라이언트를 통해 호스트명 neuron.ksc.re.kr (neuron01 또는 neuron02.ksc.re.kr) 로 접속하실 수 있습니다.



Q. ssh 접속 방법은 어떻게 되나요?

A.\
o Unix/Linux 에서는 ssh 명령을 통해 접속하실 수 있고 X11 포워딩을 사용하시려면 -X 옵션을 추가하면 됩니다.\
\- 접속 방법 : ssh -l (사용자 ID)@neuron01.ksc.re.kr 또는 ssh -l (사용자 ID)@neuron02.ksc.re.kr\
\
o Windows에서는 ssh 접속 유틸리티(putty, xshell 등)를 사용하여 접속하실 수 있습니다.



Q. neuron01 노드와 neuron02 노드의 차이점은 무엇인가요?

A.\
neuron01 노드는 K40 GPU가 탑재되어 있고 neuron02 노드는 V100 GPU가 탑재되어 있습니다.



Q. 여러번 접속이 실패하여 접속이 차단 되었습니다. 어떻게 해야 하나요?

A.\
패스워드 관련 작업에서 일정횟수에 실패가 이루어 지게 되면 ID가 잠기게 됩니다.\
기술지원에서 계약/계정 항목으로 상담신청 하거나 계정담당자(account@ksc.re.kr)에게 연락 바랍니다.



Q. PC에 있는 데이터를 뉴론으로 옮기는 방법을 알고 싶습니다.

A.\
Neuron 에서 허용하는 file 전송 프로토콜로는 ftp, sftp 가 가능합니다.\
해당 기능을 지원하는 유틸리티 프로그램을 설치하여 서버에 접속하면 파일 전송이 가능합니다.



Q. 가장 최신의 사용자 가이드는 어디에서 구할 수 있나요?

A.\
"[www.ksc.re.kr](https://www.ksc.re.kr/) > 기술지원 > 지침서에 있는 뉴론 지침서를 참조하시기 바랍니다.



Q. 로그인시 공지사항이 올라가서 확인할 수가 없습니다. 방법이 없나요?

A.\
접속 shell 프로그램 설정을 확인하시면 라인버퍼를 지정하시는 부분이 있습니다.\
이부분이 작게 되어 있으시다면 2만 라인 정도로 해놓으시면 공지 확인이 가능합니다.\
또는, motd 명령을 통해서 공지사항 확인이 가능 합니다.

****

### **# 응용 S/W (9)**

Q. 가지고 있는 상용 프로그램을 뉴론 시스템에서 설치하여 사용하는 것이 가능한 가요?

A.\
****라이선스가 특정 machine이나 site에 대한 dependency가 없는 경우\
(즉, KISTI 슈퍼컴센터에서 설치하여 사용해도 법적으로 문제가 없는 경우)\
그리고 해당 SW가 현재의 시스템의 HW/SW와 compatible 한 경우에는 설치 후 사용할 수 있습니다.\
\
데몬형식의 라이선스 인증 방식을 사용하는 경우는 별도의 확인이 필요합니다.\
기술지원을 통해 상담신청해주시기 바랍니다.



Q. 뉴론에서 VASP 을 컴파일 하고 싶은데 어떻게 해야 하나요?

A.\
KISTI 슈퍼컴퓨팅센터 블로그([http://blog.ksc.re.kr)](https://github.com/gtopia0305/kisti/blob/gto01/blog/userguide/neuron/http:/blog.ksc.re.xn--kr\)-7r5v14v)에서 SW 설치 문서 창고" 페이지를 보시면 vasp 설치 문서가 등록되어 있습니다.



Q. 프로그램을 설치하여 사용하려고 하는데 make install을 하면 설치 권한이 없다는 오류가 생겨서 설치할 수가 없습니다. 어떻게 해야 하나요?

A.\
configure 할때 프로그램 기본 설정된 설치 위치가 /usr/local 로 되어서 그렇습니다.\
configure 할때는 "--prefix=설치위치"를 지정하면 설치가 가능 합니다.\
(예)\
$ ./configure --prefix=/home01/{사용자 ID}/build\
$ make\
$ make install



Q. 프로그램을 설치할때 sudo 명령을 사용할 수 없는데 어떻게 해야 하나요?

A.\
뉴론 시스템에서 sudo 명령어는 시스템 관리자만 사용할 수 있습니다.\
configure 할때 "--prefix=설치위치"를 홈 디렉토리로 지정해서 설치해 주세요.



Q. 기존에 사용하던 프로그램을 GPU를 이용하여 작업을 돌리고 싶어요.

A.\
기존에 사용하시던 프로그램이 CPU 기반으로 설치된 경우 GPU 옵션을 사용하여 다시 설치를 하셔야 합니다.\
\
KISTI 슈퍼컴퓨팅센터 블로그([http://blog.ksc.re.kr)](https://github.com/gtopia0305/kisti/blob/gto01/blog/userguide/neuron/http:/blog.ksc.re.xn--kr\)-7r5v14v)에서 "SW 설치 문서 창고/뉴론(Neuron) (GPU Cluster System)" 페이지에 GROMACS, VASP, lammps의 GPU 기반 설치 방법이 소개되어 있습니다.



Q. 뉴론에서 tensorflow 를 사용하고 싶은데, 어떻게 해야 하나요?

A.\
뉴론 시스템에서의 기계학습 프레임워크는 anaconda 환경을 사용하는 것을 권장합니다.\
다음과 같이 module 명령어를 이용하여 환경변수를 설정한 후 사용가능합니다.\
(예)\
$ module load gcc/8.3.0 cuda/10.0 cudampi/openmpi-3.1.0 conda/tensorflow\_1.13\
\
사용자가 원하는 환경의 conda 패키지 활용방법은 슈퍼컴퓨팅 블로그의 "Conda의 활용 소개" ([https://blog.ksc.re.kr/127)](https://blog.ksc.re.kr/127\)) 게시글을 통해 확인하실수 있습니다.



Q. 뉴론에서 gaussian 을 돌리려는데 권한이 없다고 나와요.

A.\
Gaussian을 사용하기 위해서는 라이센스 관련하여 계정을 Gaussian group으로 등록하여야 합니다.\
기술지원에서 상담신청 하거나 account@ksc.re.kr 로 사용하시는 계정을 알려주시면, Gaussian group으로 등록하여 드립니다.



Q. 뉴론에서 Python 라이브러리를 설치하고 싶어요.

A.\
뉴론 시스템에서는 pip 명령어를 이용하여 Python 라이브러리의 설치가 가능합니다.\
module을 이용한 환경설정 이후 pip 명령어를 이용해주시기 바랍니다.\
\
(예)\
$ module load python/3.7.1\
$ pip install --user --upgrade pip\
$ pip install --user pywindow\
$ pip list | grep pywindow\
pywindow 0.0.3



Q. 뉴론에서 pip 명령어를 사용하니 'pip install --upgrade pip' command. 오류가 발생합니다.

A.\
pip의 upgrade가 필요하여 발생하는 오류 입니다. pip를 upgrade 하신 이후 다시 Python 라이브러리를 설치해주시면 됩니다.\
pip를 이용한 설치를 하시는 경우 --user 옵션을 반드시 사용해주시야 설치가 가능합니다.\
\
\- pip 업그레이드 방법\
$ module load python/3.7.1\
$ pip install --user --upgrade pip

****

### **# 컴파일러 (10)**

Q. 뉴론에서 제공하는 컴파일러는 어떤 것이 있나요?

A.\
****Intel, GNU, PGI 컴파일러를 사용하실 수 있습니다.\
\
버전 정보는 "module avail" 명령으로 조회할 수 있습니다.\
\- 컴파일러 조회\
$ module avail



Q. 뉴론에서 제공하는 MPI 라이브러리는 어떤 것이 있나요?

A.\
일반적인 IMPI, OpenMPI, Mvapich MPI 와 CUDA 환경을 포함한 OpenMPI, Mvapich MPI를 사용하실 수 있습니다.\
버전 정보는 "module avail" 명령으로 조회할 수 있습니다.\
\- MPI 조회\
$ module avail



Q. 뉴론에서 컴파일러와 MPI는 어떻게 설정 하나요?

A.\
환경 모듈 파일 목록을 조회 (module avail) 해서 환경 설정(module load 모듈파일명) 후 사용해야 합니다.\
사용법은 뉴론 사용자 지침서 참조 바랍니다.\
\
\- 환경 설정 예제\
$ module load intel/18.0.2 cuda/10.0 cudampi/mvapich2-2.3



Q. MPI와 OpenMP의 차이점은 무엇인가요?

A.\
o OpenMP\
공유메모리를 이용한 하나의 노드에서 데이터를 공유하여, 하나의 프로세스가 멀티 스레드를 이용하여 병렬 계산을 수행하는 것입니다.\
\
o MPI\
MPI는 통신 라이브러리를 사용하여 여러 개의 프로세스간에 메시지를 주고 받는 방법으로 병렬 계산을 수행하는 것이고, 한 개의 노드 또는 여러 개의 노드에서 수행이 가능한 반면, OpenMP의 경우는 한 개의 노드안에서 여러 개의 스레드를 이용한 병렬화만 가능합니다.



Q. Fortran 90/95 문법의 소스코드(확장자 '.f')를 컴파일 했을 때 "Syntax error at or near end of line .." 문구와 함께 에러가 납니다.

A.\
확장자를 .f로 한 소스 코드는 fortran90/95 컴파일러로 컴파일 하더라도 fortran77 형식의 소스로 인식합니다.\
\
따라서, 완벽한 fortran90/95 형식의 소스코드는 확장자를 .f90(95)로 바꾸시고,\
라인의 열 개수만 확장하고 싶으실 때는 -free(Intel 컴파일러 사용시) 옵션을 통해 컴파일 하실 수 있습니다.



Q. Intel 컴파일러로 배열 크기가 2GB 이상일 경우 오류가 발생하며 컴파일이 되지 않아요. 어떻게 해야 하나요?

A.\
컴파일 옵션에 "-mcmodel=medium" 을 추가해서 컴파일해 보세요.



Q. test 에 위치한 module은 무엇인가요?

A.\
modulefiles/test에 위치한 module 들은 아직 test 진행중인 module 로 사용을 권해드리지는 않습니다.\
\
test에 위치한 module의 경우 시스템의 모든 컴파일러가 아닌 특정 컴파일러만 지원하거나\
시스템에 설치된 어플리케이션을 지원하지 않을 수 있습니다.



Q. Intel 컴파일러로 OpenMP 코드를 빌드하는데 "-mp" 옵션을 넣으니 오류가 나와요.

A.\
Intel 컴파일러에서 사용했던 "-mp" 옵션은 Intel 컴파일러 상위 버전에서는 "-qopenmp" 로 변경 되었습니다.\
"-mp" 를 "-qopenmp" 로 변경해서 사용하시면 OpenMP 병렬화 환경으로 빌드 할 수 있습니다.



Q. Makefile 에 GENCODE\_ARCH 라는게 있는데 어떤 옵션을 사용해야 하나요?

A.\
GPU 아키텍처의 종류에 따라 GENCODE\_ARCH의 옵션이 달라지게 됩니다.\
Tesla K40 GPU : SM\_35, compute\_35 (CUDA 5.0 이상)\
Tesla V100 GPU : SM\_70, compute\_70 (CUDA 9.0 이상)\
\
\- 사용 예시\
GENCODE\_ARCH :=\
\-gencode=arch=compute\_35,code=\\"sm\_35,compute\_35\\" \\\
\-gencode=arch=compute\_70,code=\\"sm\_70,compute\_70\\"



Q. module purge 명령어 사용 시 메시지가 출력됩니다.

A.\
"dependent modulefiles were removed" 메시지는 종속 모듈 파일이 제거되었다는 메시지로 오류 메시지가 아닙니다.



### **# 환경설정 (4)**

Q. 사용자 패스워드 변경은 어떻게 하나요?

A.\
passwd 명령을 통해 변경 가능합니다.\
\- 사용자 패스워드 길이는 최소 9자이며, 영문, 숫자, 특수문자의 조합으로 이뤄져야 합니다.\
\- 영문 사전 단어는 사용이 불가 합니다.\
\- 사용자 패스워드 변경 기간은 2개월로 설정(60일) 됩니다.\
\- 새로운 패스워드는 최근 5개의 패스워드와 유사한 것을 사용할 수 없습니다.



Q. 패스워드 변경 하려고 하다가 변경에 실패하여 접속이 차단 되었습니다. 어떻게 해야 하나요?

A.\
5회 이상 틀릴 경우, 이 계정의 ID는 잠기게 됩니다.\
기술지원에서 계약/계정 항목으로 상담신청 하거나 계정담당자(account@ksc.re.kr)에게 연락 바랍니다.



Q. 홈 디렉토리는 얼마의 용량까지 사용이 가능한가요?

A.\
구좌당 64GB까지 사용이 가능합니다.\
해당 용량을 초과하는 큰 작업의 경우는 스크래치 디렉토리로 옮겨서 작업을 수행하실 수 있습니다.



Q. 스크래치 디렉토리와 홈 디렉토리의 차이는 무엇인가요?

A.\
홈 디렉토리는 구좌당 64GB까지 사용이 가능하며, 스크래치 디렉토리의 경우 사용자당 최대 50TB까지 사용이 가능합니다.\
많은 용량이 필요한 작업의 경우 스크래치 디렉토리로 옮겨서 작업을 수행하시는 것을 권해드립니다.\
\
단, 스크래치 디렉토리의 경우는 15일 이상 액세스 하지 않은 자료는 삭제하도록 되어있으니 유의 바랍니다.
