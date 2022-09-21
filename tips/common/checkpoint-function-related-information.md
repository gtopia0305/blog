---
description: 슈퍼컴퓨팅인프라센터 2021. 5. 3. 14:44
---

# Checkpoint 기능 관련 안내

## 1. Checkpoint 기능의 정의

모종의 이유로 계산이 완료되기 전에 중단되었을 때(walltime limit 초과 및 에러 발생 등) 계산을 특정 지점부터 이어서 수행할 수 있도록 작업의 상태를 주기적으로 저장하는 기능



## 2. Checkpoint 기능 관련 문서

Checkpoint 기능의 명칭이나(Autosave 등) 활성화 여부는 애플리케이션의 종류 및 버전에 따라 다를 수 있으며, \
일부 애플리케이션의 경우 수동으로 활성화해야 할 수도 있다.\
Checkpoint 기능 관련 세부 사항은 다음과 같은 기술 문서를 통해 확인할 수 있다.



1\) Abaqus

\- Abaqus execution ([abaqus-docs.mit.edu/2017/English/SIMACAEEXCRefMap/simaexc-c-analysisproc.htm](https://abaqus-docs.mit.edu/2017/English/SIMACAEEXCRefMap/simaexc-c-analysisproc.htm))



2\) ANSYS Fluent

\- Autosave ([www.afs.enea.it/project/neptunius/docs/fluent/html/tuilist/node6.htm](https://www.afs.enea.it/project/neptunius/docs/fluent/html/tuilist/node6.htm))



3\) Gaussian

\- Using Gaussian Checkpoint Files ([www.ccl.net/cca/documents/dyoung/topics-orig/checkpoint.html](http://www.ccl.net/cca/documents/dyoung/topics-orig/checkpoint.html))

\- Restart ([gaussian.com/restart/](https://gaussian.com/restart/))

\- Link 0 Commands ([gaussian.com/link0/](https://gaussian.com/link0/))



4\) GROMACS

\- Managing long simulations ([manual.gromacs.org/documentation/current/user-guide/managing-simulations.html](https://manual.gromacs.org/documentation/current/user-guide/managing-simulations.html))



5\) LAMMPS

\- restart command ([lammps.sandia.gov/doc/restart.html](https://lammps.sandia.gov/doc/restart.html))

\- read\_restart command ([lammps.sandia.gov/doc/read\_restart.html](https://lammps.sandia.gov/doc/read\_restart.html))



6\) LS-DYNA

\- LS-DYNA Manual > RESTART ANALYSIS ([www.dynasupport.com/manuals](https://www.dynasupport.com/manuals))
