---
description: 슈퍼컴퓨팅인프라센터 2021. 5. 10. 17:29
---

# 누리온 FAQ (2021.05)

**Nurion \[5호기] FAQ 모음**

## **# 배치작업 (14)**

Q. 계산 노드에서 작업 실행은 어떻게 하나요?

A.\
계산 노드에서의 작업 실행은 작업 스케쥴러인 PBS를 통해 수행하실 수 있습니다.\
작업에 따른 스크립트 예제는 홈폴더 \~/job\_examples 에 있습니다.\
job\_script를 작성하시고 qsub 명령을 통해 작업을 올리실 수 있습니다.\
****\
****$ qsub mpi.sh





Q. 제출한 작업의 진행 상태를 알고 싶어요.

A.\
작업 상태를 알려주는 명령은 qstat 입니다.\
\
$ qstat\
\- 내 작업만 확인하는 경우\
\
$ qstat -u $USER\
\- 작업ID 의 상세 정보\
\
$ qstat -f {작업ID}





Q. 사용할 수 있는 자원을 확인하는 방법은 어떻게 되나요?

A.\
pbs\_status 명령을 통해 현재 사용 가능한 자원 상황을 출력할 수 있습니다.\




Q. 작업 스크립트 예제에 #PBS로 되어있는데, #은 주석 아닌가요?

A.\
일반적으로 스크립트 파일에서는 #을 주석으로 인식하지만\
\#PBS 로 지정하고 스케줄러로 작업을 제출 하면 PBS 스케줄러가 지시어로 인식합니다.



Q. 작업을 제출했는데 작업이 멈추어 있어요.

A.\
"qstat" 명령으로 제출한 작업 상태를 확인해 주세요.\
"Q" 상태는 작업 제출 시 지정된 자원을 확보하면서 대기중인 상태 입니다.\
"E" 상태로 대기 상태가 되어있으면 [www.ksc.re.kr](http://www.ksc.re.kr/) > 기술지원 > 상담 으로 문의글을 남겨 주세요.





Q. normal 큐에 몇개까지 작업을 제출할 수 있나요?

A.\
2021년 5월 기준으로 normal 큐는 사용자 당 최대 120개 작업 제출 되고,\
제출된 작업 중에서 최대 100개의 작업을 실행할 수 있습니다.\
\
작업 제출 정책은 변경될 수 있습니다.\
\
시스템을 접속할때 뜨는 공지사항에서 확인할 수 있고,\
접속해 사용 중인 상태 에서는 motd 명령으로 공지사항을 다시 확인 할 수 있습니다.





Q. 작업을 제출하면 "Program Exception - illegal instruction" 오류가 나와요.

A.\
컴파일 할 때 지정한 CPU 최적 옵션과 계산할 CPU 타입이 다른지 확인해 주세요.\
Intel 컴파일러 사용 시 "-xMIC-AVX512" 는 KNL CPU 의 최적 옵션이고,\
"-xCORE-AVX512"는 SKL CPU 의 최적 옵션 입니다.





Q. 작업을 제출하면\
"Please verify that both the operationg system and the processor suppoert Intel(R) AVX512ER and AVX512PF instructions." 오류가 나와요.

A.\
컴파일 할 때 지정한 CPU 최적 옵션과 계산할 CPU 타입이 다른지 확인해 주세요.\
Intel 컴파일러 사용 시 "-xMIC-AVX512" 는 KNL CPU 의 최적 옵션이고,\
"-xCORE-AVX512"는 SKL CPU 의 최적 옵션 입니다.





Q. 작업을 제출하면\
"qsub: ERROR: You can't submit job in home01 directory, please try in scratch directory" 오류가 나와요

A.\
누리온 시스템은 홈 디렉토리에서 작업이 제출 되지 않습니다.\
스크래치 디렉토리에서 작업을 제출해 보세요.\
\
계정 별 스크래치 디렉토리는 /scratch/${USER} 경로 입니다.\
(예) ID가 optpar02 일 경우 /scratch/optpar02





Q. 작업을 제출하면 "/bin/sh^M: bad interpreter: No such file or directory" 오류가 나와요.

A.\
"^M" 표시는 DOS(Windows) 형식 파일의 줄바꿈 표시 입니다.\
Linux(Unix) 에서 DOS 형식의 줄바꿈 표시를 문자로 인식해서 생기는 문제 입니다.\
\
dos2unix 명령을 이용해서 unix 형식으로 변경해서 사용해 주세요.\
(예) dos2unix mpi.sh





Q. 작업을 제출하면 권한이 없다고 나와요.

A.\
(오류내용 예시)\
\[optpar02@dm4 optpar02]$ qsub mpi.sh\
\-bash: /opt/pbs/bin/qsub: Permission denied\
\
프롬프트 표시가 dm4(Datamover 4번 노드) 인 것으로 보아 [nurion-dm.ksc.re.kr](http://nurion-dm.ksc.re.kr/) 로 접속 하신것 같습니다.\
dm 노드의 경우 파일 업/다운 을 위해 준비된 노드로 작업 제출이 되지 않습니다.

[nurion.ksc.re.kr](http://nurion.ksc.re.kr/)(login노드)로 접속 하셔서 사용하셔야 작업 제출을 하실 수 있습니다.\
login 노드로 접속 하셔서 다시 작업을 제출해 보세요.







Q. 작업을 제출하면 ""terminated with signal 11" 오류가 나오는데, 오류의 원인에는 어떤 것들이 있습니까?

A.\
"terminated with signal 11(Segmentation Fault)"은 메모리 접근 오류가 발생했을 때 발생하는 실행 에러입니다.\
\
아래와 같은 원인으로 에러가 발생할 수 있습니다.\
\- 메모리 부족\
\- 배열 인덱스가 정의된 범위를 넘어 사용되는 경우\
\- 할당되지 않은 동적 배열로의 접근\
\- 기타





Q. 작업을 제출하면 "insufficient viertual memory" 에러 메시지가 나오는데 어떻게 해야 하나요?

A.\
"insufficient virtual memory" 오류의 대부분은 메모리 부족으로 발생합니다.\
MPI 작업인 경우 사용 노드 수를 증가시키고 노드 당 코어 할당을 줄여서 사용해 보세요.\
\
(예제)\
\- 변경 전 : #PBS -l select=2:ncpus=64:mpiprocs=64:ompthreads=1\
\- 변경 후 : #PBS -l select=4:ncpus=32:mpiprocs=32:ompthreads=1





Q. 시스템 월간 점검 시 수행중이던 작업은 어떻게 되나요?

A.\
시스템 월간 점검이 시작되면 스케줄러에 등록된 모든 작업은 종료 됩니다.\
작업을 제출 하실 경우 월간 점검 이전에 작업이 종료 될 수 있도록 사용 바랍니다.

#### ****

#### ****

## **# 시스템 접속 (7)**

Q. 누리온에 접속하는 방법은 어떻게 되나요?

A.\
ssh 클라이언트를 통해 호스트명 [nurion.ksc.re.kr](http://nurion.ksc.re.kr/)로 접속하실 수 있습니다.





Q. ssh 접속 방법은 어떻게 되나요?

A.\
\* Unix/Linux 에서는 ssh 명령을 통해 접속하실 수 있고 X11 포워딩을 사용하시려면 -X 옵션을 추가하면 됩니다.\
\- 접속 방법 : ssh -l (사용자ID) [nurion.ksc.re.kr](http://nurion.ksc.re.kr/) 또는 ssh (사용자ID)@[nurion.ksc.re.kr](http://nurion.ksc.re.kr/)\
\
\* Windows에서는 ssh 접속 유틸리티(putty, xshell 등)를 사용하여 접속하실 수 있습니다.





Q. 여러번 접속이 실패하여 접속이 차단되었습니다. 어떻게 해야하나요?

A.\
패스워드 관련 작업에서 일정횟수에 실패가 이루어 지게 되면 ID가 잠기게 됩니다.\
[www.ksc.re.kr](http://www.ksc.re.kr/) > 기술지원 > 상담 > 상담신청하거나\
계정담당자(account@[ksc.re.kr](http://ksc.re.kr/))에게 연락 바랍니다.





Q. PC에 있는 데이터를 누리온으로 옮기는 방법을 알고 싶습니다.

A.\
누리온 에는 데이터 전송 전용 노드로 Datamover([nurion-dm.ksc.re.kr](http://nurion-dm.ksc.re.kr/)) 노드가 있고,\
허용하는 file 전송 프로토콜로는 ftp, sftp 가 가능합니다.\
해당 기능을 지원하는 유틸리티를 설치하여 서버에 접속하면 파일 전송이 가능합니다.





Q. 가장 최신의 사용자 가이드는 어디에서 구할 수 있나요?

A.\
"[www.ksc.re.kr](http://www.ksc.re.kr/) -> 기술지원 -> 지침서 -> 하드웨어" 또는\
"[blog.ksc.re.kr](http://blog.ksc.re.kr/) -> 사용자 지침서/동영상 지침서 \[초보사용자]" 카테고리에 있는 동영상 가이드를 참조하시기 바랍니다.





Q. 로그인 시 공지사항이 올라가서 확인할 수가 없습니다. 방법이 없나요?

A.\
접속 shell 프로그램 설정을 확인하시면 라인버퍼를 지정하시는 부분이 있습니다.\
이부분이 작게 되어 있으시다면 2만 라인 정도로 해놓으시면 공지 확인이 가능합니다.\
또는, motd 명령을 통해서 공지사항 확인이 가능 합니다.



Q. 누리온에서 계산한 결과를 PC로 복사하고 싶은데 scp 명령을 실행하면 멈춰 있어요.

A.\
누리온 시스템에서 외부로 접속 하는것은 보안 정책으로 막혀 있습니다.\
PC 에서 누리온 시스템으로 접속해서 파일을 가져가는 방식으로 사용하셔야 합니다.\
\
(예)\
$ scp -r USERID@[nurion.ksc.re.kr](http://nurion.ksc.re.kr/):/home01/USERID/result.tar ./

****

****

## **# 응용 S/W (7)**

Q. 가지고 있는 상용 프로그램을 누리온 시스템에 설치하여 사용하는 것이 가능한가요?

A.\
****라이선스가 특정 machine이나 site에 대한 dependency가 없는 경우\
(즉, KISTI 슈퍼컴센터에서 설치하여 사용해도 법적으로 문제가 없는 경우)\
그리고 해당 SW가 현재의 시스템의 HW/SW와 compatible 한 경우에는 설치 후 사용할 수 있습니다.\
\
데몬형식의 라이선스 인증 방식을 사용하는 경우는 별도의 확인이 필요합니다.





Q. 누리온에서 VASP을 컴파일하고 싶은데 어떻게 해야 하나요?

A.\
KISTI 슈퍼컴퓨팅센터 블로그([blog.ksc.re.kr](http://blog.ksc.re.kr/))에서 좌측상단의 목록에서 "SW 설치 문서 창고" 페이지를 보시면 vasp 설치 소개 페이지가 있습니다.





Q. 프로그램을 설치하여 사용하려고 하는데 make install을 하면 설치 권한이 없다는 오류가 생겨서 설치할 수가 없습니다. 어떻게 해야하나요?

A.\
configure 시 프로그램 기본 설치 위치가 /usr/local 이기 때문입니다.\
configure 할때는 ""--prefix=설치위치""를 지정하면 설치가 가능 합니다.\
\
(예)\
$ ./configure --prefix=/home01/optpar02/build\
$ make\
$ make install





Q. 프로그램을 설치할 때 sudo 명령을 사용할 수 없는데 어떻게 해야 하나요?

A.\
누리온 시스템에서는 sudo 명령어는 시스템 관리자만 사용할 수 있습니다.\
configure 할때 ""--prefix=설치위치""를 홈 디렉토리로 지정해서 설치해 주세요.





Q. KISTI 슈퍼컴퓨에서 쓸 수 있는 상용SW 목록은 어디서 확인하나요?

A.\
누리온 시스템에서는 sudo 명령어는 시스템 관리자만 사용할 수 있습니다.\
configure 할때 ""--prefix=설치위치""를 홈 디렉토리로 지정해서 설치해 주세요.





Q. 누리온에서 Abaqus를 돌리고 싶은데, 어떻게 해야하나요?

A.\
[www.ksc.re.kr](http://www.ksc.re.kr/) 홈페이지 > 보유자원 > 소프트웨어 에서\
소프트웨어 목록의 Abaqus 지침서 링크를 누르거나\
직접 KISTI 슈퍼컴퓨팅센터 블로그([http://blog.ksc.re.kr](http://blog.ksc.re.kr/)) 에서 abaqus 를 검색해서 지침서를 확인할 수 있습니다.





Q. 누리온에서 gaussian을 돌리려는데 권한이 없다고 나와요.

A.\
Gaussian을 사용하기 위해서는 라이센스 관련하여 계정을 Gaussian group으로 등록하여야 합니다.\
[www.ksc.re.kr](http://www.ksc.re.kr/) > 기술지원 > 상담에서 삼담신청 하거나,\
account@ksc.re.kr 로 사용하시는 로그인 아이디를 알려주시면, Gaussian group으로 등록하여 드립니다.





## **# 컴파일러 (9)**

Q. 누리온에서 제공하는 컴파일러는 어떤 것이 있나요?

A.\
Intel, GNU, CRAY 컴파일러를 사용하실 수 있습니다.\
버전 정보는 "module avail" 명령으로 조회할 수 있습니다.\
\
\- 컴파일러 조회\
$ module avail





Q. 누리온에서 제공하는 MPI 라이브러리는 어떤 것이 있나요?

A.\
IntelMPI, OpenMPI, Mvapich MPI 를 사용하실 수 있습니다.\
버전 정보는 "module avail" 명령으로 조회할 수 있습니다.\
\
\- MPI 조회\
$ module avail





Q. 컴파일러와 MPI는 어떻게 설정 하나요?

A.\
환경 모듈 파일 목록을 조회 (module avail) 해서 환경 설정(module load 모듈파일명) 후 사용해야 합니다.\
사용법은 누리온 사용자 지침서([www.ksc.re.kr](http://www.ksc.re.kr/) > 기술지원 > 지침서 > 하드웨어) 참조 바랍니다.\
\
\- 환경 설정 예제\
$ module load intel/18.0.3 impi/18.0.3





Q. 누리온에서 컴파일 할 때 권장하는 최적화 옵션은 어떤 것이 있나요?

A.\
누리온에는 여러 종류의 컴파일러가 설치 되어 있습니다.\
누리온 사용자 지침서([www.ksc.re.kr](http://www.ksc.re.kr/) > 기술지원 > 지침서 > 하드웨어)에 있는 컴파일러 별 권장 옵션을 확인해 주세요.





Q. MPI와 OpenMP의 차이점은 무엇인가요?

A.\
\* OpenMP\
공유메모리를 이용한 하나의 노드에서 데이터를 공유하여,\
하나의 프로세스가 멀티 스레드를 이용하여 병렬 계산을 수행하는 것입니다.\
\
\* MPI\
MPI는 통신 라이브러리를 사용하여 여러 개의 프로세스간에 메시지를 주고 받는\
방법으로 병렬 계산을 수행하는 것입니다.\
\
따라서, MPI는 한 개의 노드 또는 여러 개의 노드에서 수행이 가능한 반면,\
OpenMP의 경우는 한 개의 노드안에서 여러 개의 스레드를 이용한 병렬화만 가능합니다.





Q. Fortran 90/95 문법의 소스코드(확장자 '.f')를 컴파일 했을 때 "Syntax error at or near end of line .." 문구와 함께 에러가 납니다.

A.\
확장자를 .f로 한 소스 코드는 fortran90/95 컴파일러로 컴파일 하더라도 fortran77 형식의 소스로 인식합니다.\
따라서, 완벽한 fortran90/95 형식의 소스코드는 확장자를 .f90(95)로 바꾸시고,\
라인의 열 개수만 확장하고 싶으실 때는 -free(Intel 컴파일러 사용시) 옵션을 통해 컴파일 하실 수 있습니다.





Q. Intel 컴파일러로 배열 크기가 2GB 이상일 경우 오류가 발생하며 컴파일이 되지 않아요. 어떻게 해야 하나요?

A.\
컴파일 옵션에 "-mcmodel=medium" 을 추가해서 컴파일해 보세요.





Q. CPU-only(SKL) 노드와 KNL 노드 모두에서 실행 가능하도록 컴파일 하려면 어떻게 해야 하나요?

A.\
Intel 컴파일러를 사용할 경우 SKL과 KNL 모두 사용할 수 있도록 컴파일 하려면 "-xCOMMON-AVX512" 옵션을 사용해서 컴파일해 보세요.\
\
GNU 컴파일러를 사용할 경우에는 "-mpku" 옵션을 사용해서 컴파일해 보세요.





Q. Intel 컴파일러로 OpenMP 코드를 빌드하는데 "-mp" 옵션을 넣으니 오류가 나와요.

A.\
Intel 컴파일러에서 사용했던 "-mp" 옵션은 Intel 컴파일러 상위 버전에서는 "-qopenmp" 로 변경 되었습니다.\
"-mp" 를 "-qopenmp" 로 변경해서 사용하시면 OpenMP 병렬화 환경으로 빌드 할 수 있습니다.

****

****

## **# 환경설정 (5)**

Q. 사용자 shell을 변경할 수 있나요?

A.\
****chsh 명령을 통해 bash, csh, ksh, tcsh 로 변경 가능합니다.





Q. 사용자 패스워드 변경은 어떻게 하나요?

A.\
passwd 명령을 통해 변경 가능합니다.\
\
\- 사용자 패스워드 길이는 최소 9자이며, 영문, 숫자, 특수문자의 조합으로 이뤄져야 합니다.\
\- 영문 사전 단어는 사용이 불가 합니다.\
\- 사용자 패스워드 변경 기간은 2개월로 설정(60일) 됩니다.\
\- 새로운 패스워드는 최근 5개의 패스워드와 유사한 것을 사용할 수 없습니다.





Q. 패스워드를 변경하려고 하다가 변경에 실패하여 접속이 차단되었습니다. 어떻게 해야 하나요?

A.\
5회 이상 틀릴 경우, 이 계정의 ID는 잠기게 됩니다.\
[www.ksc.re.kr](http://www.ksc.re.kr/) > 기술지원 > 상담에서 상담신청 하거나, 계정담당자(account@ksc.re.kr)에게 연락 바랍니다.





Q. 홈 디렉토리는 얼마의 용량까지 사용이 가능한가요?

A.\
구좌당 64GB까지 사용이 가능합니다.\
해당 용량을 초과하는 큰 작업의 경우는 스크래치 디렉토리로 옮겨서 작업을 수행하실 수 있습니다.\
\
\- 사용량 확인 방법\
$ lfs quota /home01\
$ lfs quota /scratch





Q. 스크래치 디렉토리와 홈 디렉토리의 차이는 무엇인가요?

A.\
홈 디렉토리는 구좌당 64GB까지 사용이 가능하며 주기적으로 백업을 하고 있습니다.\
\
큰 작업의 경우는 사용자당 최대 100TB까지 사용이 가능한 스크래치 디렉토리로 옮겨서 작업을 수행할 수 있습니다.\
단, 스크래치 디렉토리의 경우는 백업이 되지 않으며 15일 이상 액세스 하지 않은 자료는 삭제하도록 되어있으니 유의 바랍니다.



