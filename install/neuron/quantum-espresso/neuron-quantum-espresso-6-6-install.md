---
description: 슈퍼컴퓨팅인프라센터 2021. 1. 27. 10:32
---

# Quantum Espresso-6.6 (GPU 버전) 설치

KISTI 슈퍼컴퓨터센터의 뉴론 시스템에 qe-6.6 버전을 설치하는 방법에 대하여 소개한다.

## **1. 설치 환경**

| **구분**        | **내용**                |
| ------------- | --------------------- |
| 대상 시스템        | 뉴론                    |
| OS Version    | 리눅스 / CentOS 7.4      |
| CPU           | Intel Xeon E5-2670 v2 |
| 컴파일러          | PGI 2019 Version      |
| MPI           | Openmpi 3.1.0 Version |
| <p>기타<br></p> | CUDA 10.0             |



## **2. 설치 전 환경 설정**

KISTI 시스템은 PATH, LD\_LIBRARY\_PATH 등을 쉽게 하기 위하여 OpenSource 인 Environment Modules(http://modules.sourceforge.net)이 구성되어 있고, 이하 설치 소개 에서는 module load를 이용한 환경 설정 방법을 이용한다.

****

{% code title="[ 환경 설정 ]" %}
```
 $ module load pgi/19.1 cuda/10.0 cudampi/openmpi-3.1.0
```
{% endcode %}

## **3. 설치 과정**

설치 과정 소개는 tar 를 이용한 압축 해제 방법과 설정 방법등 진행 절차를 위주로 설명하고, 소스 파일 다운로드 등은 생략한다. (다운로드 : https://gitlab.com/QEF/q-e-gpu/-/releases)

{% code title="설치과정" %}
```
$ tar xvf q-e-gpu-qe-gpu-6.6a2.tar.gz
$ cd q-e-gpu-qe-gpu-6.6a2
$ export F90FLAGS="-fast -Ktrap=fp -Mcache_align -Mpreprocess -Mlarge_arrays -mp -tp=px"
$ export FFLAGS="-fast -Ktrap=fp -mp -tp=px"
$ ./configure --prefix=$/QE/6.6 <br>--with-cuda=/apps/cuda/10.0 --with-cuda-cc=70 --with-cuda-runtime=10.0 --enable-openmp
$ vi make.inc

-----아래 내용으로 수정-----
CFLAGS = -fast -tp=px -Mpreprocess $(DFLAGS) $(IFLAGS)
FOX_FLAGS = -fast -tp=px -Mcache_align -Mpreprocess -Mlarge_arrays

$ make pw
```
{% endcode %}



## 4. 뉴론에서 QE 사용을 위한 SLURM 작업 스크립트 예제

위의 과정을 거처 설치된 QE는 뉴론 환경에서 다음과 같이 실행이 가능하다.\
****뉴론에서 작업을 제출하기 위해서는 SLURM 작업 스크립트를 사용하여야 한다.\
(작업 스크립트 예제 경로: /apps/applications/test\_samples/QE/)

{% code title="작업스크립트 예제(qe_job.sh)" %}
```
#!/bin/sh
#SBATCH -J "QE-GPU"
#SBATCH -p cas_v100_2
#SBATCH -N 1
#SBATCH -n 20
#SBATCH -o %x_%j.out
#SBATCH -e %x_%j.err
#SBATCH -t 02:30:00
#SBATCH --gres=gpu:2
#SBATCH --comment qe

ulimit -a unlimited

export NO_STOP_MESSAGE=yes

## Setting for gpu acceleration off
#### export USEGPU=no

# QE run parameters

NGPU=2
NPOOL=1

# Node-specific parameters
GPU_PER_SOCKET=1
CORES_PER_SOCKET=10

NCORE_PER_RANK=$((${CORES_PER_SOCKET}/${GPU_PER_SOCKET}))

export OMP_NUM_THREADS=${NCORE_PER_RANK}
export MKL_NUM_THREADS=${NCORE_PER_RANK}

srun --cpu_bind=none -n ${NGPU} -c ${CORES_PER_SOCKET} pw.x -input ./pw.in -npool ${NPOOL}
```
{% endcode %}
