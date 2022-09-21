---
description: 슈퍼컴퓨팅인프라센터 2018. 11. 29. 17:58
---

# 컴파일러별 AVX-512 옵션 정리

각 컴파일러들에서 INTEL AVX-512(Advanced Vector Extensions 512) 를 활성화 시키는 옵션입니다.

INTEL AVX-512 참고 : https://en.wikipedia.org/wiki/AVX-512



\[GNU]

\- Skylake : -march=skylake-avx512

\- KNL : -march=knl

&#x20;

\[Intel]

\- Skylake : -xCORE-AVX512

\- KNL : -xMIC-AVX512

※ 참고 : https://software.intel.com/en-us/articles/compiling-for-the-intel-xeon-phi-processor-and-the-intel-avx-512-isa

&#x20;

\[PGI]

\- Skylake : -tp=skylake

\- KNL : -tp=knl

※ 참고 : https://www.pgroup.com/resources/docs/18.1/x86/pgi-release-notes/index.htm
