---
description: 슈퍼컴퓨팅인프라센터 2017. 5. 12. 16:34
---

# vasp.5.4.1.24Jun15 Version Bug 이슈

vasp.5.4.1.24Jun15 버전은 버그가 있어 패치가 필요한것으로 알려져 있다.

아래 내용들을 참고 하여 vasp 설치 전 패치를 진행해야 오류가 발생하지 않는다.

&#x20;

**\[참고 주소]**

\- vasp site : https://www.vasp.at/index.php/news/archive

\- vasp wiki site : http://cms.mpi.univie.ac.at/wiki/index.php/Installing\_VASP

&#x20;

**\[패치 파일 목록]**

vasp download portal 또는 vasp wiki 에서 다운 로드 가능.

\- patch.5.4.1.08072015.gz

\- patch.5.4.1.27082015.gz

\- patch.5.4.1.06112015.gz

&#x20;

**\[패치 방법]**

각 압축 파일을 Gunzip 후 vasp.5.4.1 root directory 에서 패치 진행(압축해제 방법은 생략한다)

> $ patch -p1 < patch.5.4.1.08072015
>
> $ patch -p1 < patch.5.4.1.27082015
>
> $ patch -p1 < patch.5.4.1.06112015

\


\


\
