---
title: "[Lecture Note] MySQL 08.Cursor"
toc: true
use_math: true
categories:
  - RDBMS
tags:
  - [MySQL]
---

stored procedure 내에서 사용하는 cursor에 대한 내용입니다.

커서의 핵심은 파일을 열어서 한 행식 값을 처리하는 것을 말하고 이 때 '파일 포인터'는 자동으로 다음 줄을 가리켜 진행됩니다.

# 커서의 처리 순서

1. 커서의 선언
 - DECLARE CURSOR
2. 반족 조건 선언
 - DECLARE CONTINUE HANDLER
 - 더이상 읽을 것이 없을 때 실행할 내용 지정
3. 커서 열기
 - OPEN
4. 커서에서 데이터 가져오기
 - FETCH
5. 데이터 처리
 - 4,5번 단계를 LOOP END LOOP 문으로 반복실행
6. 커서 닫기
 - CLOSE