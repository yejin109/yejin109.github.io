---
title: "[Lecture Note] MySQL 10.Cursor"
toc: true
use_math: true
categories:
  - RDBMS
tags:
  - [MySQL]
---

특정 테이블에 할당되어 DML문(**INSERT, UPDATE, DELETE**같은 작업들)이 발생되면 자동으로 실행되는 프로그래밍이라고 생각하면 될 것 같습니다.

트리거의 특성상 강제로 사용할 수 없고 이벤트가 발생해야 하며 파라미터도받을 수 없습니다.

또한 ALTER를 사용못하므로 변경시 지우고 생성해야 한다. 

# 기본 형태

```
DELIMITER // 
CREATE TRIGGER triger_name
    (AFTER| BEFORE) (INSERT | DELETE | UPDATE)
    ON target_table
    -- 그냥 무조건 쓰는 것, 각 행마다 작업하게 되는 것
    FOR EACH ROW 
    -- optional로 트리거의 순서를 결정
    (FOLLOW | PRECEDES) other_trigger_name
BEGIN
	(src)
END // 
DELIMITER ;
```

# 트리거 종류

## AFTER 트리거
이벤트 발생 후에 작동

## BEFORE 트리거
이벤트 발생 전에 작동

- 예시 1

```
DELIMITER //
CREATE TRIGGER trg_deletedMembertbl	AFTER DELETE ON membertbl FOR EACH ROW
BEGIN	
INSERT INTO deletedMemberTBL VALUES (OLD.memberID, OLD.memberName, OLD.memberAddress, CURDATE() );
END //
DELIMITER ;
```

이렇게 TRIGER를 작성하게 된다면 
- 이름: trg_deletedMebertbl
- 실행 조건 : DELETE 문
- 실행 내용 : INSERT문

으로 정리할 수 있게 됩니다.

위의 쿼리문으로 본다면 membertbl에서 DELETE문이 실행된 경우 지워지는 데이터를 deleteMemberTBL이란 테이블에 입력하게 됩니다.

이 때 OLD는 예약어로, TRIGGER가 시작하게 만든 data를 가리키고 있습니다.


- 예시 2

UPDATE 트리거 

```
DELIMITER $$
CREATE TRIGGER backUserTbl_UpdateTrg  
    AFTER UPDATE 
    ON userTBL 
    FOR EACH ROW 
BEGIN
    INSERT INTO backup_userTbl VALUES( OLD.userID, OLD.name, OLD.birthYear, 
        OLD.addr, OLD.mobile1, OLD.mobile2, OLD.height, OLD.mDate, 
        '수정', CURDATE(), CURRENT_USER() );
END $$
DELIMITER ;
```

DELETE 트리거
```
DELIMITER // 
CREATE TRIGGER backUserTbl_DeleteTrg  
    AFTER DELETE 
    ON userTBL 
    FOR EACH ROW 
BEGIN
    INSERT INTO backup_userTbl VALUES( OLD.userID, OLD.name, OLD.birthYear, 
        OLD.addr, OLD.mobile1, OLD.mobile2, OLD.height, OLD.mDate, 
        '삭제', CURDATE(), CURRENT_USER() );
END // 
DELIMITER ;
```
