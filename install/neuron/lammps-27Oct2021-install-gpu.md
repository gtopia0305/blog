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



&#x20;(3-3) meam 설치

```
$ cd lib/awpmd
$ vi Makefile.lammps.installed
```

{% code title="[Makefile.lammps.ifort 수정 사항]" %}
```

meam_SYSINC=
meam_SYSLIB=
meam_SYSPATH=
```
{% endcode %}



&#x20;(3-4) awpmd 설치

{% code title="  설치과정" %}
```
$ cd lib/awpmd
$ vi Makefile.lammps.installed
----- 수정 사항은 아래의 내용 참고 -----
$ make -f Makefile.mpicc
$ cd ../../
```
{% endcode %}

{% code title="[Makefile.lammps.installed 수정 사항]" %}
```
user-awpmd_SYSINC =
user-awpmd_SYSLIB =
user-awpmd_SYSPATH =
```
{% endcode %}



&#x20;(3-5) atc 설치

{% code title="  설치과정" %}
```
$ cd lib/atc
$ vi Makefile.lammps.installed
----- 수정 사항은 아래의 내용 참고 -----
$ vi Makefile.lammps.linalg
----- 수정 사항은 아래의 내용 참고 -----
$ make -f Makefile.mpic++
$ cd ../../
```
{% endcode %}

{% code title="[Makefile.lammps.installed 수정 사항]" %}
```
user-atc_SYSINC =
user-atc_SYSLIB =
user-atc_SYSPATH =
```
{% endcode %}

{% code title="[Makefile.lammps.linalg 수정 사항]" %}
```
user-atc_SYSINC =
user-atc_SYSLIB = -llinalg
user-atc_SYSPATH = -L../../lib/linalg$(LIBOBJDIR)
```
{% endcode %}



&#x20;(3-6) linalg 설치

{% code title="  설치과정" %}
```
$ cd lib/linalg
$ cp -p Makefile.gfortran Makefile.ifort
$ vi Makefile.ifort
----- 수정 사항은 아래의 내용 참고 -----
$ make -f Makefile.ifort
$ cd ../../
```
{% endcode %}

{% code title="[Makefile.ifort 수정 사항]" %}
```
FC = ifort
FFLAGS = -O3 -fPIC
FFLAGS0 = -O0 -fPIC
```
{% endcode %}



&#x20;(3-7) reax 설치

{% code title="  설치과정" %}
```
$ cd lib/reax
$ make -f Makefile.ifort
$ cd ../../
```
{% endcode %}



&#x20;(3-8) latte 설치

{% code title="  설치과정" %}
```
$ cd lib/latte
$ ln -s ${HOME}/build/LATTE-1.2.1/src includelink
$ ln -s ${HOME}/build/LATTE-1.2.1 liblink
$ ln -s ${HOME}/build/LATTE-1.2.1/src/latte_c_bind.o filelink.o
$ vi Makefile.lammps.mpi
----- 수정 사항은 아래의 내용 참고 ----- 
$ cd ../../
```
{% endcode %}

{% code title="[Makefile.lammps.mpi 수정 사항]" %}
```
latte_SYSINC =
latte_SYSLIB = ../../lib/latte/filelink.o -llatte -llinalg -lifport
latte_SYSPATH = -L../../lib/linalg -qopenmp
```
{% endcode %}



&#x20;(3-9) gpu 설치

{% code title="  설치과정" %}
```
$ cd lib/gpu
$ vi Makefile.linux
----- 수정 사항은 아래의 내용 참고 ----- 
$ vi Makefile.mpi
----- 수정 사항은 아래의 내용 참고 ----- 
$ make -f Makefile.linux
$ ./nvc_get_devices
$ cd ../../
```
{% endcode %}

{% code title="[Makefile.linux 수정 사항]" %}
```
CUDA_HOME = /apps/cuda/10.0

# Tesla CUDA
CUDA_ARCH = -arch=sm_70

#CUDA_ARCH = -arch=sm_10 -DCUDA_PRE_THREE
#CUDA_ARCH = -arch=sm_35
```
{% endcode %}

{% code title="[Makefile.mpi 수정 사항]" %}
```
CUDA_HOME = /apps/cuda/10.0

# Tesla CUDA
CUDA_ARCH = -arch=sm_70

#CUDA_ARCH = -arch=sm_10 -DCUDA_PRE_THREE
#CUDA_ARCH = -arch=sm_35

include /home01/optpar05/build/lammps-16Mar18/lib/gpu/Makefile.mpi
```
{% endcode %}



&#x20;(4) LAMMPS 설치

lammps 설치 디렉토리(${HOME}/build/lammps-16Mar18) 아래 src 폴더로 이동한다.



&#x20;\- package 선택 및 설치

사용하는 사용자의 연구내용에 맞추어 필요한 package를 선택하여 설치한다. 여기서는 기본적으로 많이 사용되는 package를 위주로 설치하였다.

|   **설치과정**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |                                                                                                                                                                                                                                                                                                                                     |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p>$ cd src</p><p>$ make package-status</p><p>$ make yes-standard</p><p>$ make no-kim</p><p>$ make no-KOKKOS</p><p>$ make no-MSCG</p><p>$ make no-PYTHON</p><p>$ make yes-GPU</p><p>$ make yes-USER-ATC</p><p>$ make yes-USER-AWPMD</p><p>$ make yes-USER-MEAMC</p><p>$ make yes-USER-MISC</p><p>$ make yes-USER-OMP</p><p>$ make yes-USER-REAXC</p><p>$ make package-status</p><p></p><p>$ vi MAKE/Makefile.mpi</p><p>-- 수정 사항은 아래 내용 참고 --</p><p>$ vi Makefile.package.settings</p><p>-- 수정 사항은 아래 내용 참고 --</p><p>$ make mpi</p> | <p></p><p> package 선택 확인</p><p> standard package 선택</p><p> standard package 중 kim package 제외</p><p> standard package 중 KOKKOS package 제외</p><p> standard package 중 MSCG package 제외</p><p> standard package 중 PYTHON package 제외</p><p></p><p></p><p><br></p><p></p><p><br></p><p> package 선택 확인</p><p><br><br></p><p><br></p><p></p> |

{% code title="[MAKE/Makefile.mpi 수정 사항]" %}
```
CC =        mpicxx
OPTFLAGS = -O3 -fp-model fast=2 -no-prec-div -qoverride-limits
CCFLAGS =   -qopenmp -qno-offload -fno-alias -ansi-alias -restrict \
-DLMP_INTEL_USELRT -DLMP_USE_MKL_RNG $(OPTFLAGS)
CCFLAGS += -I/apps/compiler/intel/18.0.2/mkl/include/ -lmkl_rt -I/${HOME}/build/library/include/voro++ \
-I/apps/cuda/10.0/include
SHFLAGS =   -fPIC
DEPFLAGS =  -M

LINK =      mpicxx
LINKFLAGS = -qopenmp $(OPTFLAGS)
LIB = -L/apps/cuda/10.0/lib64 -lcudart -lcuda -L/apps/compiler/intel/18.0.2/lib/intel64 -lifport
SIZE =      size

ARCHIVE =   ar
ARFLAGS =   -rc
SHLIBFLAGS =    -shared

FFT_INC =       -DFFT_MKL -DFFT_SINGLE
FFT_PATH =
FFT_LIB =   -L${MKLROOT}/lib/intel64/ -lmkl_intel_ilp64 -lmkl_sequential -lmkl_core
```
{% endcode %}

{% code title="[Makefile.package.settings 수정 사항]" %}
```

include ../../lib/awpmd/Makefile.lammps
include ../../lib/atc/Makefile.lammps
include ../../lib/gpu/Makefile.mpi
include ../../lib/voronoi/Makefile.lammps
include ../../lib/reax/Makefile.lammps
include ../../lib/poems/Makefile.lammps
include ../../lib/meam/Makefile.lammps
include ../../lib/latte/Makefile.lammps.mpi
include ../../lib/compress/Makefile.lammps

```
{% endcode %}

****

**4. 실행 파일 복사**

&#x20;설치가 완료되면 사용에 편의를 위해 bin 경로를 만들어 실행 파일인 lmp\_mpi를 bin 경로에 복사한다.(선택사항)

```
$ ls -l lmp_mpi
$ cd ${HOME}/build/lammps-16Mar18/
$ mkdir bin
$ cp ${HOME}/build/lammps-16Mar18/src/lmp_mpi .
```

****

**5. 뉴론에서 LAMMPS 사용을 위한 SLURM 작업 스크립트 예제**

&#x20;위의 과정을 거처 설치된 lammps는 뉴론 환경에서 다음과 같이 실행이 가능하다.\
****뉴론에서 작업을 제출하기 위해서는 SLURM 작업 스크립트를 사용하여야 한다.

실행 예제로는 examples/meam 아래의 데이터를 이용하였다.

{% code title="  작업스크립트 예제(lammps_test-run.sh)" %}
```
#!/bin/sh
#SBATCH -J test
#SBATCH -p ivy_v100_2
#SBATCH -N 1
#SBATCH -n 10
#SBATCH -o %x_%j.out
#SBATCH -e %x_%j.err
#SBATCH -t 00:30:00
#SBATCH --gres=gpu:2
#SBATCH --comment lammps

module load intel/18.0.2 cuda/10.0 cudampi/mvapich2-2.3

ulimit -s unlimited

export PATH=${HOME}/build/lammps-16Mar18:$PATH

srun lmp_mpi -sf gpu -pk gpu 2 -in peri/in.peri
```
{% endcode %}
