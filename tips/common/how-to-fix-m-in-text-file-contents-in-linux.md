---
description: 슈퍼컴퓨팅인프라센터 2018. 11. 29. 17:36
---

# Linux(Unix) 에서 Text 파일 내용에 ^M 이 붙어 있는 경우 해결 방법

\[오류 내용]

/bin/sh^M: bad interpreter: No such file or directory

&#x20;

\[원인]

dos 형식의 파일에서의 새줄 문자와 Unix 형식에서의 새줄 문자가 달라 ^M을 명령으로 인식해서 생기는 문제입니다.

&#x20;

\[조치사항]

dos 포멧으로 작성된 파일을 vi를 이용해 ^M을 제거 하거나&#x20;

dos2unix 명령어를 이용하여 Unix 파일 포멧으로 변경해주어야 합니다.

\
\[예제]

dos2unix {파일명}

&#x20;

\- 참고 : https://ko.wikipedia.org/wiki/새줄\_문자
