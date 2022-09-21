---
description: 슈퍼컴퓨팅인프라센터 2017. 5. 8. 15:26
---

# 오픈 소스 빌드 시에 OpenMP 체크 오류 (Intel 컴파일러)

OpenMP 병렬 프로그래밍을 지원하는 Open Source 프로그램을 Intel 컴파일러로 설치해서

사용하고자 할때 configure 단계에서 OpenMP 체크 시 오류가 발생할 경우&#x20;



**\[오류내용]**

checking how to enable OpenMP... unknown

configure: error: don't know how to enable OpenMP



**\[원인]**

Intel 컴파일러에서 OpenMP 활성화 옵션이 "-qopenmp" 이지만 일부 Open Source 들에서 "-mp" 로만 지정되어 있어 오류 발생



**\[해결방법]**

파일 편집기(vi)로 configure 파일을 열어서 CFLAGS와 같은 환경변수에 "-mp" 로 지정된 부분을 "-qopenmp"로 수정해 준다



**\[예제 : fftw-2.1.5]**

변경 전 : CFLAGS="$save\_CFLAGS -mp"

변경 후 : CFLAGS="$save\_CFLAGS -qopenmp"
