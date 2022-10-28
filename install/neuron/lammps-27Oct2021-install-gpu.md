---
description: 슈퍼컴퓨팅인프라센터 2022. 9. 16. 14:33
---

# 뉴론 lammps-27Oct2021 (GPU 버전) 설치

KISTI 뉴론 시스템에 lammps-27Oct2021버전을 설치하는 방법에 대해 소개한다.설치 과정 소개는 tar 파일을 이용한 압축 해제 방법과 설정 방법 등의 진행 절차를 위주로 설명하고, 소스 파일 다운로드 등은 생략한다.

## **1. 설치 환경**

|   **구분**    | **내용**                                   |
| ----------- | ---------------------------------------- |
|  대상 시스템     | 뉴론                                       |
|  OS Version | 리눅스 / CentOS 7.9                         |
|  CPU        | Intel(R) Xeon(R) Gold 5217 CPU @ 3.00GHz |
|  컴파일러       | GCC 10.2.0                               |
|  MPI        | CUDA OpenMPI 4.1.1                       |
| 기타          | CUDA 11.4                                |

## **2. 설치 전 환경 설정**

KISTI 시스템은 PATH, LD\_LIBRARY\_PATH 등을 쉽게 하기 위하여 OpenSource 인 Environment Modules(http://modules.sourceforge.net)이 구성되어 있고, 이하 설치 소개 에서는 module load를 이용한 환경 설정 방법을 이용한다.&#x20;

****

```
$ module purge
$ module load gcc/10.2.0 cuda/11.4 cudampi/openmpi-4.1.1
```

## **3. 설치 과정**

설치 과정에서 설치 경로는 **${HOME}/lammps/27Oct2021** 을 사용하였다. 이 경로는 사용자에게 맞는 경로로 변경하여야 한다.

### &#x20;**(1)** VORO++ 설치 (다운로드 : [http://math.lbl.gov/voro++/download](http://math.lbl.gov/voro++/download))

VORONOI 패키지 설치를 위한 voro++을 우선 설치한다.

<pre><code><strong>$ tar xvf voro++-0.4.6.tar.gz
</strong>$ cd voro++-0.4.6
$ mkdir -p ${HOME}/build/library
$ vi config.mk</code></pre>

{% code title="[수정 사항]" %}
```
CXX=mpicxx
PREFIX=${HOME}/build/library
```
{% endcode %}

```
$ make
$ make install
```

### &#x20;**(2)** LATTE 설치 (다운로드 : [https://github.com/lanl/LATTE/releases)](https://github.com/lanl/LATTE/releases\))

LATTTE 패키지 설치를 위한 Latte 라이브러리를 우선 설치한다. 다운로드 받은 파일을 적당한 위치($HOME/build)에 올린 후 다음과 같은 명령으로 압축 묶음 파일을 푼다.

```
$ cd ${HOME}/build
$ tar xvf v1.2.1.tar.gz
$ cd LATTE-1.2.1
$ vi makefile.CHOICES
```

{% code title="[ 수정 사항]" %}
```
LIB = -L/apps/compiler/gcc/10.2.0/applib1/lapack/3.10.0 -llapack -lblas
 
GPU_CUDA_LIB = -L/apps/cuda/11.4/lib64 -lcublas -lcudart
GPU_ARCH = sm_70
※ a100 노드에서 작업 수행 시, GPU_ARCH = sm_70대신 GPU_ARCH = sm_80)
```
{% endcode %}

```
$ make
```



### &#x20;**(3) 라이브러리 패키지 설치**

LAMMPS 공식 홈페이지 [http://lammps.sandia.gov/index.html](http://lammps.sandia.gov/index.html) 나 [https://github.com/lammps/lammps/releases](https://github.com/lammps/lammps/releases) 로부터 다운로드 받은 파일을 적당한 위치($(HOME)/build)에 옮긴 뒤, 다음과 같은 명령으로 압축 해제 한다.

```
$ tar xvf lammps-27Oct2021.tar.gz
```



&#x20;(3-1) voronoi 설치

(1)에서 설치한 voro++ 설치 디렉터리를 지정해 준다. lammps 압축 해제 후 lammps-27Oct2021 디렉터리로 이동하여 아래의 작업을 진행한다.

```
$ cd lammps-27Oct2021
$ cd lib/voronoi
$ ln -s $(HOME)/build/library/include/voro++ includelink
$ ln -s $(HOME)/build/library/lib liblink
$ cd ../../
```



&#x20;(3-2) poems 설치

```
$ cd lib/poems
$ make -f Makefile.g++
```



&#x20;(3-3) awpmd 설치

```
$ cd lib/awpmd
$ vi Makefile.lammps.installed
```

{% code title="[수정 내용]" %}
```
awpmd_SYSLIB =
```
{% endcode %}

```
$ make -f Makefile.mpicc
$ cd ../../
```



&#x20;(3-4) atc 설치

```
$ cd lib/atc
$ vi Makefile.lammps.installed
```

{% code title="[수정 내용]" %}
```
atc_SYSLIB =
```
{% endcode %}

```
$ make -f Makefile.mpic++
$ cd ../../
```



&#x20;(3-5) linalg 설치

```
$ cd lib/linalg
$ make -f Makefile.gfortran
$ cd ../../
```



&#x20;(3-6) latte 설정

```
$ cd lib/latte

$ ln -s ${HOME}/build/LATTE-1.2.1/src includelink
$ ln -s ${HOME}/build/LATTE-1.2.1 liblink
$ ln -s ${HOME}/build/LATTE-1.2.1/src/latte_c_bind.o filelink.o

$ cd ../../
```



&#x20;(3-7) gpu 설치

```
$ cd lib/gpu
$ vi Makefile.linux
```

{% code title="[수정 내용]" %}
```
CUDA_HOME = /apps/cuda/11.4
 
# Maxwell hardware
#CUDA_ARCH = -arch=sm_50
 
# Volta hardware
CUDA_ARCH = -arch=sm_70
 
# Ampere hardware
#CUDA_ARCH = -arch=sm_80
※ a100 노드에서 작업 수행 시, CUDA_ARCH = -arch=sm_70는 주석처리(#)하고
 CUDA_ARCH = -arch=sm_80 주석해제)
```
{% endcode %}

```
$ vi Makefile.cuda
```

{% code title="[수정 내용]" %}
```
CUDA_HOME = /apps/cuda/11.4
```
{% endcode %}

```
$ make -f Makefile.linux
$ ./nvc_get_devices

$ vi Makefile.lammps
```

{% code title="[수정 내용]" %}
```
CUDA_HOME=/apps/cuda/11.4
 
$ cd ../../
```
{% endcode %}



&#x20;(3-8) LAMMPS 설치

lammps 설치 디렉터리 ($(HOME)/build/lammps-27Oct2021) 아래 src 디렉터리로 이동한다.

\- package 선택 및 설치

수행하는 연구 내용에 맞추어 필요한 package를 선택하여 설치한다. 예시에서는 기본적으로 많이 사용되는 package 위주로 구성하여 설치 진행하였다.

```
$ cd src

$ make package-status
$ make no-kim
$ make no-KOKKOS
$ make no-MSCG
$ make no-PYTHON
$ make yes-meam
$ make yes-reaxff
$ make yes-granular
$ make yes-peri
$ make yes-MANYBODY
$ make yes-ATC
$ make yes-AWPMD
$ make yes-MISC
$ make yes-POEMS
$ make yes-VORONOI
$ make yes-LATTE
$ make yes-GPU
$ make package-status
```



&#x20;\- makefile 수정

```
$ vi MAKE/Makefile.mpi
```

```
(수정 내용)
CC =            mpicxx
CCFLAGS =       -g -O3 -std=c++11
CCFLAGS +=      -I/apps/wa/build/applications/lammps-27Oct2021/build/library/include/voro++ -I/apps/cuda/11.4/include
SHFLAGS =       -fPIC
DEPFLAGS =      -M
 
FFT_INC = -I/apps/compiler/gcc/10.2.0/cudampi/11.4/openmpi/4.1.1/applib2/fftw_mpi/3.3.7/include
FFT_PATH =
FFT_LIB = -L/apps/compiler/gcc/10.2.0/cudampi/11.4/openmpi/4.1.1/applib2/fftw_mpi/3.3.7/lib
 


 
(수정 내용)
include ../../lib/gpu/Makefile.lammps
include ../../lib/latte/Makefile.lammps.mpi
include ../../lib/atc/Makefile.lammps
include ../../lib/awpmd/Makefile.lammps
include ../../lib/poems/Makefile.lammps
include ../../lib/voronoi/Makefile.lammps
```

```
$ make mpi 2>&1 |tee make.log
```

****

**4. 실행 파일 복사**

설치가 완료되면 편의를 위해 bin 경로를 만들어 실행 파일인 lmp\_mpi를 bin 경로에 복사하여 활용할 수 있다. (선택사항)

```
$ ls -l lmp_mpi
$ cd $(HOME)

$ mkdir bin
$ cp $(HOME)/build/lammps-27Oct2021/src/lmp_mpi .
```

****

**5. 뉴론에서 LAMMPS 사용을 위한 SLURM 작업 스크립트 예제**

&#x20;위의 과정을 거쳐 설치된 lammps는 뉴론 환경에서 다음과 같이 작업에 활용할 수 있다. 뉴론에서 작업을 제출하기 위해서는 SLURM 작업 스크립트를 사용하여야 한다.

실행 예제로는 /apps/applications/test\_samples/LAMMPS/peri 경로의 데이터를 이용하였다.

&#x20;\- 작업 스크립트 예시 (test.sh)

```
#!/bin/sh
#SBATCH -J test
#SBATCH -p cas_v100_4
#SBATCH -N 1
#SBATCH -n 4
#SBATCH -o %x_%j.out
#SBATCH -e %x_%j.err
#SBATCH -t 00:30:00
#SBATCH --gres=gpu:2
#SBATCH --comment lammps

module purge
module load gcc/10.2.0 cuda/11.4 cudampi/openmpi-4.1.1

export PATH=$(HOME)/bin:$PATH
(또는 lmp_mpi가 있는 경로를 PATH 경로로 지정)

srun lmp_mpi -sf gpu -pk gpu 2 -in [input 파일명]
```
