---
description: 슈퍼컴퓨팅인프라센터 2018. 12. 4. 11:39
---

# vasp.5.4.4.18Apr17 Version Bug 이슈

vasp.5.4.4.18Apr17 버전은 버그가 있어 패치가 필요한것으로 알려져 있다.

아래 내용들을 참고 하여 vasp 설치 전 패치를 진행해야 오류가 발생하지 않는다.

&#x20;

**\[참고 주소]**

\- vasp site : https://www.vasp.at/index.php/news/archive

\- vasp wiki site : http://cms.mpi.univie.ac.at/wiki/index.php/Installing\_VASP

&#x20;

**\[패치 파일 목록]**

vasp download portal 또는 vasp wiki 에서 다운 로드 가능.

\- patch.5.4.4.16052018.gz

&#x20;

**\[패치 방법]**

각 압축 파일을 Gunzip 후 vasp.5.4.4 root directory 에서 패치 진행(압축해제 방법은 생략한다)

> $ patch -p0 < patch.5.4.4.16052018



\[vasp site 공지 내용]&#x20;



Dear All,



This patch (patch.5.4.4.16052018) for vasp.5.4.4.18Apr17-6-g9f103f2a35 addresses several bugs that were found and fixed:



Fixes a bug in the stress term when using the SCAN functional in certain pathological cases.

Fixes a bug in the Thomas-Fermi potential.

Fixes a bug that affected the optB88 for some atoms and molecules.

Fixes some issues with the BSE at finite q.

and adds support for:



The CX13 vdW-DFT functional.

This patch can be downloaded from the download portal or our wiki.



Gunzip and apply this patch inside the vasp.5.4.4 root directory:



patch -p0 < patch.5.4.4.16052018



Our apologies for the inconvenience,



The VASP team.



\- 참고 : https://www.vasp.at/index.php/news/bugfixes/119-bugfix-patch-1-for-vasp-5-4-4-18apr17
