---
title: "[Lecture Note] MySQL 06. Stored Procedure"
toc: true
use_math: true
categories:
  - RDBMS
tags:
  - [MySQL]
---


# 생성할 때 
SQL에서 프로그래밍하여 사용하는 것으로 일종의 함수로 생각할 수 있다고 생각합니다.

다음과 같은 방식으로 생성합니다. 
```
DELIMITER $$
CREATE PROCEDURE procedure_name()
BEGIN
  (소스코드)
END $$
DELIMITER;
```
- 예시

```
DELIMITER //
CREATE PROCEDURE myProc()
BEGIN	SELECT * FROM memberTBL WHERE memberName = '당탕이';	
SELECT * FROM productTBL WHERE productName = '냉장고';
END //
DELIMITER ; 

CALL myProc() ;
```

이렇게 할 경우 stored procedure의 이름은 *myProc*이 되며 2개의 SELECT문이 실행됩니다. 

선언할 때 주의할 점은 다음과 같습니다.
- 쿼리가 여러개라면 BEGIN-END를 사용합니다
- DELIMITER로 전체 stored procedure문을 다른 쿼리문과 감싸 구분하게 됩니다.


# IF ELSE를 이용한 stored procedure

조건문의 형식은 다음과 같습니다.
```
IF condition THEN
  query
ELSE
  query
END IF;
```

- 예시

```
DELIMITER $$
CREATE PROCEDURE ifProc()
BEGIN

  DECLARE hireDATE DATE;
  DECLARE curDATE DATE;
  DECLARE days INT;
  
  SELECT hire_date INTO hireDATE 
    FROM employees.employees
    WHERE emp_no = 10001;
    
  SET curDATE = CURRENT_DATE();
  SET days = DATEDIFF(curDATE, hireDATE);

  IF (days/365) >=5  THEN
    SELECT CONCAT('입사한지', days, '일이나 지났습니다.');
  ELSE
    SELECT CONCAT('입사한지', days, '일밖에 안지났습니다.');
  END IF;  
END $$
DELIMITER;
```

# CASE ~ WHEN 으로 다중 분기

- 예시 1

```
DELIMITER $$
CREATE PROCEDURE caseProc()
BEGIN

  DECLARE point INT;
  DECLARE credit CHAR(1);
  
  SET point = 77;
  
  CASE
    WHEN point >= 90 THEN
      SET CREDIT = 'A';
    WHEN point >= 80 THEN
      SET CREDIT = 'B';
    WHEN point >= 70 THEN
      SET CREDIT = 'C';
    WHEN point >= 60 THEN
      SET CREDIT = 'D';
    ELSE
      SET credit = 'F';
  END CASE;
  SELECT CONCAT('취득점수:', point), CONCAT('학점:', credit);
END $$
DELIMITER;
```


# 반복문 WHILE

```
DELIMITER $$
CREATE PROCEDURE whileProc()
BEGIN
	DECLARE i INT; -- 1에서 100까지 증가할 변수
	DECLARE hap INT; -- 더한 값을 누적할 변수
    SET i = 1;
    SET hap = 0;

	WHILE (i <= 100) DO
		SET hap = hap + i;  -- hap의 원래의 값에 i를 더해서 다시 hap에 넣으라는 의미
		SET i = i + 1;      -- i의 원래의 값에 1을 더해서 다시 i에 넣으라는 의미
	END WHILE;

	SELECT hap;   
END $$
```

# ITERATIVE를 이용한 procedure

ITERATIVE란 예약어를 일종의 continue로,

LEAVE란 예악어는 일종의 break으로 생각하면 됩니다. 

```
DELIMITER $$
CREATE PROCEDURE whileProc2()
BEGIN
    DECLARE i INT; -- 1에서 100까지 증가할 변수
    DECLARE hap INT; -- 더한 값을 누적할 변수
    SET i = 1;
    SET hap = 0;

    myWhile: WHILE (i <= 100) DO  -- While문에 label을 지정
	IF (i%7 = 0) THEN
		SET i = i + 1;     
		ITERATE myWhile; -- 지정한 label문으로 가서 계속 진행
	END IF;
        
        SET hap = hap + i; 
        IF (hap > 1000) THEN 
		LEAVE myWhile; -- 지정한 label문을 떠남. 즉, While 종료.
	END IF;
        SET i = i + 1;
    END WHILE;

    SELECT hap;   
END $$
DELIMITER ;
```

# 오류 처리

기본적인 구조는 다음과 같습니다.

```
DECLARE action HANDLER FOR error_code query
```

action에는 크게 CONTINUE와 EXIT가 사용되며 CONTINUE는 뒤의 query를 실행시키며 EXIT는 넘어가게 됩니다.

- 예시 1

아래의 예시에서 에러코드 1146은 없는 테이블을 조회시 발생되고, 이 때의 action이 CONTINUE이므로 뒤의 query인 SELECT문이 실행됩니다.

```
DELIMITER $$
CREATE PROCEDURE errorProc()
BEGIN
    DECLARE CONTINUE HANDLER FOR 1146 SELECT '테이블이 없어요ㅠㅠ' AS '메시지';
    SELECT * FROM noTable;  -- noTable은 없음.  
END $$
DELIMITER ;
```

- 예시 2

에러코드와 상태코드를 둘 다 사용할 수 있으며 아래의 코드 상 에러가 생기면 BEGIN END; 가 실행됩니다.

```
DELIMITER $$
CREATE PROCEDURE errorProc2()
BEGIN
    DECLARE CONTINUE HANDLER FOR SQLEXCEPTION 
    BEGIN
	SHOW ERRORS; -- 오류 메시지를 보여 준다.
	SELECT '오류가 발생했네요. 작업은 취소시켰습니다.' AS '메시지'; 
	ROLLBACK; -- 오류 발생시 작업을 롤백시킨다.
    END;
    INSERT INTO usertbl VALUES('LSG', '이상구', 1988, '서울', NULL, 
		NULL, 170, CURRENT_DATE()); -- 중복되는 아이디이므로 오류 발생
END $$
DELIMITER ;
```

# 동적 쿼리

PREPARE로 쿼리만 준비해놓고 뒤에 EXECURE가 실행되면 그 때 실제로 시작되는 방식으로 작동됩니다. 

```
PREPARE myQuery FROM 'SELECT * FROM usertbl WHERE userID = "EJW"';
EXECUTE myQuery;
DEALLOCATE PREPARE myQuery;
```