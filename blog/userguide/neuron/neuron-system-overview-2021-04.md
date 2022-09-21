---
description: 슈퍼컴퓨팅인프라센터 2019. 4. 30. 09:49
---

# 뉴론 시스템 개요(2021.04)

2019년 05월 서비스 오픈하는

뉴론 시스템의 제원은 다음과 같습니다.



## **1. GPU 노드 제원**



**(1) ivy\_k40\_2**

| **구분**           | **내용**                                                         |
| ---------------- | -------------------------------------------------------------- |
| CPU              | Intel(R) Xeon(R) CPU E5-2670 v2 @ 2.5GHz ( 노드 당 2 소켓, 총 20코어 ) |
| GPU              | NVIDIA Tesla K40m (1 노드당 2 K40 카드 탑재)                          |
| CPU 메모리          | DDR3/1866MHz (노드당 128GB, CPU 코어 당 6.4GB)                       |
| GPU 메모리          | K100 카드당 12GB                                                  |
| 운영체제             | CentOS 7.4                                                     |
| 할당 노드 수          | 4                                                              |
| Total CPU core 수 | 80                                                             |
| 최대제출/실행 작업개수     | 2                                                              |
| 사용자별 최대 GPU 점유개수 | 6                                                              |



**(2) ivy\_v100**

| **구분**           | **내용**                                                         |
| ---------------- | -------------------------------------------------------------- |
| CPU              | Intel(R) Xeon(R) CPU E5-2670 v2 @ 2.5GHz ( 노드 당 2 소켓, 총 20코어 ) |
| GPU              | NVIDIA Tesla V100 (1 노드당 2 V100 카드 탑재)                         |
| CPU 메모리          | DDR3/1866MHz (노드당 128GB, CPU 코어 당 6.4GB)                       |
| GPU 메모리          | V100 카드당 HBM2 16GB/32GB                                        |
| 운영체제             | CentOS 7.4                                                     |
| 할당 노드 수          | 19                                                             |
| Total CPU core 수 | 380                                                            |
| 최대제출/실행 작업개수     | 10                                                             |
| 사용자별 최대 GPU 점유개수 | 20                                                             |

****

**(2-1) ivy\_v100-16G\_2**

| **구분**           | **내용**                                                                        |
| ---------------- | ----------------------------------------------------------------------------- |
| CPU              | <p>Intel(R) Xeon(R) CPU E5-2670 v2 @ 2.5GHz ( 노드 당 2 소켓, 총 20코어 )<br><br></p> |
| GPU              | <p>NVIDIA Tesla V100 (1 노드당 2 V100 카드 탑재)<br><br></p>                         |
| CPU 메모리          | DDR3/1866MHz (노드당 128GB, CPU 코어 당 6.4GB)                                      |
| GPU 메모리          | V100 카드당 HBM2 16GB                                                            |
| 운영체제             | CentOS 7.4                                                                    |
| 할당 노드 수          | 11                                                                            |
| Total CPU core 수 | <p>220<br><br></p>                                                            |
| 최대제출/실행 작업개수     | 10                                                                            |
| 사용자별 최대 GPU 점유개수 | 20                                                                            |

\*\*\*\*

**(2-2) ivy\_v100-32G\_2**

\*\*\*\*

| **구분**           | **내용**                                                                        |
| ---------------- | ----------------------------------------------------------------------------- |
| CPU              | <p>Intel(R) Xeon(R) CPU E5-2670 v2 @ 2.5GHz ( 노드 당 2 소켓, 총 20코어 )<br><br></p> |
| CPU              | <p>NVIDIA Tesla V100 (1 노드당 1 V100 카드 탑재)<br><br></p>                         |
| CPU 메모리          | DDR3/1866MHz (노드당 128GB, CPU 코어 당 6.4GB)                                      |
| GPU 메모리          | V100 카드당 HBM2 32GB                                                            |
| 운영체제             | CentOS 7.4                                                                    |
| 할당 노드 수          | 8                                                                             |
| Total CPU core 수 | <p>160<br><br></p>                                                            |
| 최대제출/실행 작업개수     | 10                                                                            |
| 사용자별 최대 GPU 점유개수 | 20                                                                            |

\*\*\*\*

\*\*\*\*

**(3) cas\_v100\_2**

\*\*\*\*

| **구분**           | **내용**                                                                       |
| ---------------- | ---------------------------------------------------------------------------- |
| CPU              | <p>Intel(R) Xeon(R) Gold 6230 CPU @ 2.1GHz ( 노드 당 2 소켓, 총 40코어 )<br><br></p> |
| GPU              | <p>NVIDIA Tesla V100 (1 노드당 2 V100 카드 탑재)<br><br></p>                        |
| CPU 메모리          | DDR4 (노드당 384GB, CPU 코어 당 9.6GB)                                             |
| GPU 메모리          | V100 카드당 HBM2 32GB                                                           |
| 운영체제             | CentOS 7.4                                                                   |
| 할당 노드 수          | 15                                                                           |
| Total CPU core 수 | <p>600<br><br></p>                                                           |
| 최대제출/실행 작업개수     | 10                                                                           |
| 사용자별 최대 GPU 점유개수 | 30                                                                           |



**(4) cas\_v100nv\_4**

| **구분**           | **내용**                                                            |
| ---------------- | ----------------------------------------------------------------- |
| CPU              | Intel(R) Xeon(R) CPU Gold 6230 CPU @ 2.1GHz ( 노드 당 2 소켓, 총 40코어 ) |
| GPU              | NVIDIA Tesla V100 (1 노드당 4 V100 카드 탑재) (NVlink)                   |
| CPU 메모리          | DDR4 (노드당 384GB, CPU 코어 당 9.6GB)                                  |
| GPU 메모리          | V100 카드당 HBM2 32GB                                                |
| 운영체제             | CentOS 7.4                                                        |
| 할당 노드 수          | 4                                                                 |
| Total CPU core 수 | 160                                                               |
| 최대제출/실행 작업개수     | 3                                                                 |
| 사용자별 최대 GPU 점유개수 | 16                                                                |



**(5) cas\_v100nv\_8**

| **구분**           | **내용**                                                             |
| ---------------- | ------------------------------------------------------------------ |
| CPU              | Intel(R) Xeon(R) CPU Gold 6226R CPU @ 2.9GHz ( 노드 당 2 소켓, 총 32코어 ) |
| GPU              | NVIDIA Tesla V100 (1 노드당 8 V100 카드 탑재) (NVlink)                    |
| CPU 메모리          | DDR4 (노드당 384GB, CPU 코어 당 12GB)                                    |
| GPU 메모리          | V100 카드당 HBM2 32GB                                                 |
| 운영체제             | CentOS 7.4                                                         |
| 할당 노드 수          | 5                                                                  |
| Total CPU core 수 | 160                                                                |
| 최대제출/실행 작업개수     | 10/5                                                               |
| 사용자별 최대 GPU 점유개수 | 40                                                                 |



**(6) cas32c\_v100\_2**

| **구분**           | **내용**                                                            |
| ---------------- | ----------------------------------------------------------------- |
| CPU              | Intel(R) Xeon(R) CPU Gold 6242 CPU @ 2.8GHz ( 노드 당 2 소켓, 총 32코어 ) |
| GPU              | NVIDIA Tesla V100 (1 노드당 2 V100 카드 탑재)                            |
| CPU 메모리          | DDR4 (노드당 384GB, CPU 코어 당 12GB)                                   |
| GPU 메모리          | V100 카드당 HBM2 32GB                                                |
| 운영체제             | CentOS 7.4                                                        |
| 할당 노드 수          | 3                                                                 |
| Total CPU core 수 | 96                                                                |
| 최대제출/실행 작업개수     | 2                                                                 |
| 사용자별 최대 GPU 점유개수 | 6                                                                 |



## **2. CPU\_only 노드 제원**

**(1) skl**

| **구분**           | **내용**                                                              |
| ---------------- | ------------------------------------------------------------------- |
| CPU              | Intel(R) Xeon(R) Skylake (Gold 6140) @ 2.3GHz ( 노드 당 2 소켓, 총 36코어 ) |
| 메모리              | DDR4 (노드당 192GB)                                                    |
| 운영체제             | CentOS 7.4                                                          |
| 할당 노드 수          | 10                                                                  |
| Total CPU core 수 | 360                                                                 |
| 최대제출 작업개수        | 2                                                                   |
| 최대실행 작업개수        | 2                                                                   |
| 작업별 최대노드 점유개수    | 2                                                                   |



**(2) amd**

| **구분**           | **내용**                                       |
| ---------------- | -------------------------------------------- |
| CPU              | AMD EPYC 7542 @ 2.9GHz ( 노드 당 2 소켓, 총 64코어 ) |
| 메모리              | DDR4 (노드당 256GB)                             |
| 운영체제             | CentOS 7.7                                   |
| 할당 노드 수          | 2                                            |
| Total CPU core 수 | 128                                          |
| 최대제출 작업개수        | 1                                            |
| 최대실행 작업개수        | 1                                            |
| 작업별 최대노드 점유개수    | 1                                            |



**(3) optane**

| **구분**           | **내용**                                                            |
| ---------------- | ----------------------------------------------------------------- |
| CPU              | Intel(R) Xeon(R) CPU Gold 6246 CPU @ 3.3GHz ( 노드 당 2 소켓, 총 24코어 ) |
| 메모리              | DDR4 (노드당 1.5TB)                                                  |
| 운영체제             | CentOS 7.7                                                        |
| 할당 노드 수          | 1                                                                 |
| Total CPU core 수 | 24                                                                |
| 최대제출 작업개수        | 1                                                                 |
| 최대실행 작업개수        | 1                                                                 |
| 작업별 최대노드 점유개수    | <p>1<br></p>                                                      |



## **3. 대용량 메모리 노드 제원**

**(1) bigmem**

| **구분**           | **노드1**                                                          | **노드2**                                                           |
| ---------------- | ---------------------------------------------------------------- | ----------------------------------------------------------------- |
| CPU              | Intel(R) Xeon(R) Westmere E7-4870 / 2.4GHz ( 노드 당 4 소켓, 총 40코어 ) | Intel(R) Xeon(R) Broadwell E7-4830 / 2.0GHz ( 노드 당 4 소켓, 총 56코어 ) |
| 메모리              | DDR3 (노드당 512GB)                                                 | DDR4 (노드 당 768GB)                                                 |
| 운영체제             | CentOS 7.4 (Linux, 64 bit)                                       |                                                                   |
| 할당 노드 수          | 1                                                                | 1                                                                 |
| Total CPU core 수 | 40                                                               | 56                                                                |
| 최대제출 작업개수        | 2                                                                |                                                                   |
| 최대실행 작업개수        | 1                                                                |                                                                   |
| 작업별 최대노드 점유개수    | -                                                                |                                                                   |

****

## **4. 접속 방법**

뉴론 시스템 서비스는 2019년 05월 사용신청한 사용자들에게 제한적으로 허가가 되었으며, 사용허가를 받은 사용자들은 아래와 같은 방법으로 접속가능합니다.

****

**\[ 리눅스/맥 환경에서 접속방법 ]**

```
$ ssh -l <user id> neuron01.ksc.re.kr (or 150.183.150.99)
$ ssh -l <user id> neuron02.ksc.re.kr (or 150.183.150.100)
```

****

## **5. 파일 전송방법 (업로드/다운로드)**

뉴론 시스템에서는 다음과 같은 노드들을 통하여 사용자의 데이터를 업로드/다운로드할 수 있습니다. 아래에 명시되어 있지 않은 프로토콜에 대해서는 기본적으로 서비스하고 있지 않은 것이니, 참고하시기 바랍니다.

| **노드**                                          | **내용**                          |
| ----------------------------------------------- | ------------------------------- |
| <p>neuron01.ksc.re.kr<br>neuron02.ksc.re.kr</p> | sftp, scp 전송 지원 ( ftp은 지원하지 않음) |

****

## **6. module 사용법 (애플리케이션 사용을 위한 환경 설정 툴)**

뉴론 시스템에 설치된 컴파일러, 라이브러리, 주요 애플리케이션에 대해 module을 활용하여 쉽게 환경설정을 할수 있도록 하였습니다.

사용가능한 모듈 목록은 다음과 같습니다. (2021년 4월 현재)

```
 $ module av  

--------------------------------------- /apps/Modules/modulefiles/compilers ---------------------------------------
gcc/4.8.5    gcc/8.3.0    intel/18.0.2 pgi/19.1

--------------------------------------- /apps/Modules/modulefiles/libraries ---------------------------------------
hdf4/4.2.13  hdf5/1.10.2  lapack/3.7.0 ncl/6.5.0    netcdf/4.6.1

------------------------------------------ /apps/Modules/modulefiles/mpi ------------------------------------------
cudampi/mvapich2-2.3  cudampi/openmpi-3.1.0  cudampi/openmpi-3.1.5  mpi/impi-18.0.2       
mpi/mvapich2-2.3      mpi/openmpi-3.1.0     mpi/openmpi-3.1.5

---------------------------------- /apps/Modules/modulefiles/libraries_using_mpi ----------------------------------
fftw_mpi/2.1.5 fftw_mpi/3.3.7

------------------------------------- /apps/Modules/modulefiles/applications --------------------------------------
cmake/3.12.3        gaussian/g16.b01    htop/3.0.5          namd/2.12           python/3.7.1        R/3.5.0
cuda/10.0           gaussian/g16.c01    java/openjdk-11.0.1 nvtop/1.1.0         qe/6.4.1_k40        singularity/3.1.0
gaussian/g16        gromacs/2016.4      lammps/16Mar18      python/2.7.15       qe/6.4.1_v100       singularity/3.6.4

------------------------------------ /apps/Modules/modulefiles/conda_packages -------------------------------------
conda/caffe_gpu_1.0   conda/pytorch_1.0     conda/tensorflow_1.13

```

\[module av 목록의 주요 카테고리 설명]

\>> /apps/Modules/modulefiles/applications : 주요 애플리케이션들의 모음

\>> /apps/Modules/modulefiles/compilers : 사용 가능한 컴파일러들의 모음

\>> /apps/Modules/modulefiles/libraries : 주요 라이브러리들의 모음

\>> /apps/Modules/modulefiles/mpi : 사용 가능한 MPI 들의 모음(cudampi 는 cuda sdk에 의존성이 있는 MPI)

\>> /apps/Modules/modulefiles/libraries\_using\_mpi : MPI 의존 라이브러리들의 모음



각 모듈들의 사용방법은 **module help** 명령을 사용하시면 간단한 사용 예제를 보실 수 있습니다.

```
 $ module help intel/18.0.2 
 
----------- Module Specific Help for 'intel/18.0.2' ---------------
 
 This module is for use of Intel Compiler 2018.
 It needs module(s):
                   None
 Use example:
         $ module load intel/18.0.2


 Additional info:
     1. We can use Intel Advisor, Vtune, Inspector, TBB and MKL.
     2. Major environment variables is set up like these:
           CC=icc   CXX=icpc   FC=ifort    F77=ifort    F90=ifort
```

module help 결과를 참고하여 다음과 같이 module load 명령어를 이용하여 intel-2018 컴파일러 환경을 설정합니다.

```
$ module load intel/18.0.2
```

현재 모듈을 적재한 상황은 module list를 이용하여 확인이 가능합니다.

```
$ module list
Currently Loaded Modulefiles:
1) intel/18.0.2
```

또한, 아래와 같은 방법으로 모듈에 적재한 애플리케이션 명령어가 PATH 환경 변수에 제대로 설정되어 있는지 확인합니다.

```
$ which icc
/apps/compiler/intel/18.0.2/bin/icc
```

추가로 cuda/10.0 툴킷 환경을 적재하고 다시 한번 module list로 확인합니다.

```
$ module load cuda/10.0
$ module list
Currently Loaded Modulefiles:
1) intel/18.0.2 2) cuda/10.0
```

이번에는 더 이상 필요하지 않는 module 환경을 해제하도록 하겠습니다. 아래의 예에서는 cuda/10.0의 환경설정을 삭제하고 그 결과를 module list로 확인합니다.

```
$ module rm cuda/10.0
$ module list
Currently Loaded Modulefiles:
1) intel/18.0.2
```

혹은 module purge 명령어로 적재되었던 모든 module 들을 삭제해 버릴수도 있습니다.

```
$ module purge
$ module list
No Modulefiles Currently Loaded.
```
