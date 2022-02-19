---
title: "[Lecture Note] MySQL 10.Cursor"
toc: true
use_math: true
categories:
  - RDBMS
tags:
  - [MySQL]
---

특정 테이블에 할당되어 INSERT, UPDATE, DELETE같은 작업들이 발생되면 자동으로 실행되는 프로그래밍이라고 생각하면 될 것 같습니다.

# 실행 형태
```
DELIMITER //
CREATE TRIGGER trg_deletedMembertbl	AFTER DELETE ON membertbl FOR EACH ROW
BEGIN	
INSERT INTO deletedMemberTBL		VALUES (OLD.memberID, OLD.memberName, OLD.memberAddress, CURDATE() );
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

