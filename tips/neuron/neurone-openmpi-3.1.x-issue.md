---
description: 슈퍼컴퓨팅인프라센터 2019. 4. 30. 10:02
---

# OpenMPI 3.1.X 이슈 사항

KISTI 슈퍼컴퓨터센터의 OpenMPI 3.1.X 이슈 사항 해결 팁에 대하여 소개한다.

&#x20;

## **1. 환경**

* 대상 시스템 : 뉴론
* OS Version : Linux / CentOS 7.4
* CPU : Intel Xeon E5-2670 v2
* Mellanox OFED : MLNX\_OFED\_LINUX-4.4-2.0.7.0 (OFED-4.4-2.0.7)
* MPI : OpenMPI-3.1.0

&#x20;

## **2. 오류 내용**

\[optpar02@login02 test]$ mpirun -np 2 ./host.x

ibv\_exp\_query\_device: invalid comp\_mask !!! (comp\_mask = 0xffffffffffffffff valid\_mask = 0x1)

\[login02]\[\[51376,1],0]\[btl\_openib\_component.c:1670:init\_one\_device] error obtaining device attributes for mlx4\_1 errno says Invalid argument

ibv\_exp\_query\_device: invalid comp\_mask !!! (comp\_mask = 0xffffffffffffffff valid\_mask = 0x1)

\[login02]\[\[51376,1],1]\[btl\_openib\_component.c:1670:init\_one\_device] error obtaining device attributes for mlx4\_1 errno says Invalid argument

\--------------------------------------------------------------------------

WARNING: There was an error initializing an OpenFabrics device.

&#x20; Local host:   login02

&#x20; Local device: mlx4\_1

\--------------------------------------------------------------------------

Hello, World! I am process 0 of size 2 on login02&#x20;

Hello, World! I am process 1 of size 2 on login02&#x20;

\[login02:22081] 1 more process has sent help message help-mpi-btl-openib.txt / error in device init

\[login02:22081] Set MCA parameter "orte\_base\_help\_aggregate" to 0 to see all help / error messages

&#x20;

## **3. 오류 해결 팁**

\- openmpi-3.1.0/opal/mca/btl/openib/btl\_openib\_component.c 파일 1666라인 수정

\- 참고 : [https://github.com/hppritcha/ompi/commit/8126779a354b3e0c720d3e1790f7b936dd5b93b2](https://github.com/hppritcha/ompi/commit/8126779a354b3e0c720d3e1790f7b936dd5b93b2)

&#x20;

\[수정 전]

\#if HAVE\_DECL\_IBV\_EXP\_QUERY\_DEVICE&#x20;

&#x20;   device->ib\_exp\_dev\_attr.comp\_mask = IBV\_EXP\_DEVICE\_ATTR\_RESERVED - 1;

&#x20;

\[수정 후]

\#if HAVE\_DECL\_IBV\_EXP\_QUERY\_DEVICE&#x20;

&#x20;   **memset(\&device->ib\_exp\_dev\_attr, 0, sizeof(device->ib\_exp\_dev\_attr));**

&#x20;   device->ib\_exp\_dev\_attr.comp\_mask = IBV\_EXP\_DEVICE\_ATTR\_RESERVED - 1;

&#x20;

\
