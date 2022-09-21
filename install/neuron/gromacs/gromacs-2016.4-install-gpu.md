---
description: 슈퍼컴퓨팅인프라센터 2019. 4. 30. 10:13
---

# GROMACS-2016.4 (GPU 버전) 설치

이 문서는 GROMACS-2016.4 소스를 (CUDA 기반의) GPU 기능을 사용하는 버전으로 빌드하는 방법에 대해 소개한다.&#x20;



## **1. 설치 환경**

|   **구분**       | **내용**                                               |
| -------------- | ---------------------------------------------------- |
|  대상 시스템        | 뉴론 (GPU Cluster System)                              |
|  OS Version    | 리눅스 / CentOS 7.4                                     |
|  CPU           | Intel Xeon E5-2670 v2                                |
|  컴파일러          | intel compiler 2018.0.2                              |
|  GPU           | NVIDIA Tesla V100                                    |
| <p> 기타<br></p> | <p>intel mkl </p><p>mvapich2 2.3</p><p>CUDA 10.0</p> |



## **2. 설치 과정**

&#x20;소스 파일은 아래 사이트에서 다운로드 받을 수 있다.

&#x20;  [http://www.gromacs.org ](http://www.gromacs.org/)

|   **GROMACS 소스 압축 풀기 & 빌드할 디렉토리 생성**                                                                                        |
| --------------------------------------------------------------------------------------------------------------------------- |
| <p>$ tar xzf gromacs-2016.4.tar.gz </p><p>$ cd gromacs-2016.4  </p><p>$ <strong>mkdir build</strong> </p><p>$ cd build </p> |



&#x20;intel mkl 라이브러리를 사용하기 위해서는 아래의 cmake 명령을 참고하여 수행한다.&#x20;

|   **intel mkl 을 사용하기 위한 cmake 명령**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| <p>$ module load intel/18.0.2 cuda/10.0 cudampi/mvapich2-2.3 </p><p>$ module load cmake/3.12.3</p><p></p><p>$ export CC=mpicc </p><p>$ export CXX=mpicxx </p><p>$ cmake -DGMX_OPENMP=ON <mark style="color:blue;">-DGMX_GPU=ON -DGMX_MPI=ON</mark> \</p><p><mark style="color:blue;">-DGMX_FFT_LIBRARY=mkl</mark> \</p><p>-DGMX_PREFER_STATIC_LIBS=ON -DCMAKE_BUILD_TYPE=Release \</p><p>-DCMAKE_INSTALL_PREFIX=<mark style="color:red;">${HOME}/applications/GROMACS/2016.4</mark> \</p><p>-DGMX_HWLOC=OFF \</p><p>..</p><p>$ make</p><p>$ make install</p> |

&#x20;<mark style="color:red;">※ 옵션  - 설치할 디렉토리를 명시하고자 할때 : -DCMAKE\_INSTALL\_PREFIX=\<GROMACS\_INSTALL\_DIR> 를 추가한다.</mark>&#x20;

&#x20;&#x20;

**\[참고] 뉴론시스템 상에서 slurm 스케쥴러를 이용한 간단한 테스트**

|   **테스트 방법**                                                                                                                                                                                                                                                                                                                                                     |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p>$ module load intel/18.0.2 cuda/10.0 cudampi/mvapich2-2.3  gromacs/2016.4</p><p>$ cp /apps/applications/test_samples/Gromacs/* .</p><p>$ tar xvzf water_GMX50_bare.tar.gz</p><p>$ gmx_mpi grompp -f pme.mdp -c conf.gro -p topol.top -o topol_pme.tpr</p><p>$ gmx_mpi grompp -f rf.mdp -c conf.gro -p topol.top -o topol_rf.tpr</p><p>$ sbatch gromacs.sh</p> |



|   **작업 스크립트 (gromacs.sh)**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p>#!/bin/sh</p><p>#SBATCH -J “gromacs_test”</p><p>#SBATCH -p ivy_v100_2</p><p>#SBATCH -N 1</p><p>#SBATCH -n 4</p><p>#SBATCH -o %x_%j.out</p><p>#SBATCH -e %x_%j.err</p><p>#SBATCH -t 00:30:00</p><p><mark style="color:red;">#SBATCH --gres=gpu:2</mark></p><p><mark style="color:red;">#SBATCH --comment gromacs</mark></p><p></p><p>ulimit -s unlimited</p><p></p><p>srun gmx_mpi mdrun <mark style="color:red;">-ntomp</mark> 5 <mark style="color:red;">-dlb</mark> yes <mark style="color:red;">-nb gpu</mark> -nsteps 4000 -s ./topol_pme.tpr</p> |
