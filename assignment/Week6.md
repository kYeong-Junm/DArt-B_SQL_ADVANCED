# SQL_ADVANCED 6주차 정규 과제 

📌SQL_ADVANCED 정규과제는 매주 정해진 분량의 『*혼자 공부하는 SQL*』 을 읽고 학습하는 것입니다. 이번주는 아래의 **SQL_ADVANCED_6th_TIL**에 나열된 분량을 읽고 공부하시면 됩니다.

아래의 문제를 풀어보며 학습 내용을 점검하세요. 문제를 해결하는 과정에서 개념을 스스로 정리하고, 필요한 경우 제시된 강의를 참고하여 보완하는 것이 좋습니다.

<!-- 강의 링크는 아래와 같습니다.
https://www.youtube.com/watch?v=cw1wGN0ZdFA&list=PLVsNizTWUw7GCfy5RH27cQL5MeKYnl8Pm&index=19
https://www.youtube.com/watch?v=bMQ_dAoaMzA&list=PLVsNizTWUw7GCfy5RH27cQL5MeKYnl8Pm&index=20
https://www.youtube.com/watch?v=bggWVsBmKag&list=PLVsNizTWUw7GCfy5RH27cQL5MeKYnl8Pm&index=21
-->

**교재 실습 예제 파일은 08_SQL_ADVANCED_Template 레포지토리의 src 폴더에 업로드되어 있습니다. market_db 파일도 해당 폴더에 함께 포함되어 있으니 참고하시기 바랍니다.**

**👀(수행 인증샷은 필수입니다.)** 

## SQL_ADVANCED_6th_TIL

### 7장 스토어드 프로시저
#### 01. 스토어드 프로시저 사용 방법
#### 02. 스토어드 함수와 커서
#### 03. 자동 실행되는 트리거 


## Study Schedule

| 주차  | 공부 범위     | 완료 여부 |
| ----- | ------------- | --------- |
| 1주차 | p.24~99    | ✅         |
| 2주차 | p.102~155   | ✅         |
| 3주차 | p.158~213  | ✅         |
| 4주차 | p.216~271 | ✅         |
| 5주차 | p.274~327 | ✅         |
| 6주차 | p.330~369 | ✅         |
| 7주차 | p.372~407 | 🍽️         |


<br>

<!-- 여기까진 그대로 둬 주세요-->

---

# 1️⃣ 학습 내용 정리

## 1. 스토어드 프로시저 사용 방법 

---

## 스토어드 프로시저란?

**스토어드 프로시저(stored procedure, 저장 프로시저)**는 MySQL에서 제공하는 **프로그래밍 기능**. SQL에 조건문·반복문 같은 프로그래밍 기능을 추가해서 일반 프로그래밍 언어와 비슷한 효과를 낼 수 있음.

- 쿼리 문의 집합으로도 볼 수 있으며, 어떤 동작을 일괄 처리하기 위한 용도로도 사용
- 자주 쓰는 쿼리를 스토어드 프로시저로 묶어두면 필요할 때마다 간단히 호출해서 운영 가능
- 데이터베이스의 개체 중 하나로, 테이블처럼 각 데이터베이스 내부에 저장됨

## 기본 형식

```sql
DELIMITER $$
CREATE PROCEDURE 스토어드_프로시저_이름( IN 또는 OUT 매개변수 )
BEGIN
    이 부분에 SQL 프로그래밍 코드 작성
END $$
DELIMITER ;
```

- `DELIMITER`: 구분자 변경. MySQL 기본 구분자는 세미콜론(;)인데, 프로시저 안에도 세미콜론이 많이 쓰여 끝 지점이 모호해짐 → `$$`로 바꿔 **`$$`가 나올 때까지**가 프로시저 끝임을 표시. 마지막에 다시 `;`로 되돌림
- `CREATE PROCEDURE`는 프로시저를 **만들기만** 함 (커피 자판기 만들기), 아직 실행된 게 아님
- `CALL 프로시저_이름();` 으로 **실행(호출)** (커피 뽑기)

```sql
CALL 스토어드_프로시저_이름();
```

## 생성과 삭제

```sql
USE market_db;
DROP PROCEDURE IF EXISTS user_proc;
DELIMITER $$
CREATE PROCEDURE user_proc()
BEGIN
    SELECT * FROM member;
END $$
DELIMITER ;

CALL user_proc();
```

```sql
DROP PROCEDURE user_proc;   -- 삭제 시 괄호 붙이지 않음 (CREATE 때는 괄호 붙임)
```

## 매개변수의 사용

- **입력 매개변수**: 동전을 넣고 버튼을 누르는 것과 비슷. `IN 입력_매개변수_이름 데이터_형식`
- **출력 매개변수**: 자판기에서 미리 준비된 빈 컵에 결과가 담겨 나오는 것과 비슷. `OUT 출력_매개변수_이름 데이터_형식`, `SELECT ~ INTO ~` 문으로 값 대입

```sql
-- 입력 매개변수 1개
CREATE PROCEDURE user_proc1(IN userName VARCHAR(10))
BEGIN
    SELECT * FROM member WHERE mem_name = userName;
END $$
CALL user_proc1('에이핑크');

-- 입력 매개변수 2개
CREATE PROCEDURE user_proc2(IN userNumber INT, IN userHeight INT)
BEGIN
    SELECT * FROM member WHERE mem_number > userNumber AND height > userHeight;
END $$
CALL user_proc2(6, 165);

-- 출력 매개변수
CREATE PROCEDURE user_proc3(IN txtValue CHAR(10), OUT outValue INT)
BEGIN
    INSERT INTO noTable VALUES(NULL, txtValue);
    SELECT MAX(id) INTO outValue FROM noTable;
END $$

CALL user_proc3('테스트1', @myValue);
SELECT CONCAT('입력된 ID 값 ==>', @myValue);
```

> 💡 스토어드 프로시저를 **만드는 시점**에는 사용한 테이블(noTable)이 존재하지 않아도 됨. 단, **CALL로 실행하는 시점**에는 그 테이블이 반드시 있어야 함.

## SQL 프로그래밍의 활용

**조건문 — IF ~ ELSE**

```sql
CREATE PROCEDURE ifelse_proc(IN memName VARCHAR(10))
BEGIN
    DECLARE debutYear INT;  -- 변수 선언
    SELECT YEAR(debut_date) INTO debutYear FROM member WHERE mem_name = memName;
    IF (debutYear >= 2015) THEN
        SELECT '신인 가수네요. 화이팅 하세요.' AS '메시지';
    ELSE
        SELECT '고참 가수네요. 그동안 수고하셨어요.' AS '메시지';
    END IF;
END $$

CALL ifelse_proc('오마이걸');
```

> 📝 YEAR(), MONTH(), DAY() — 날짜에서 연·월·일 추출. CURDATE() — 현재 날짜 반환.

**반복문 — WHILE** (1부터 100까지 합계)

```sql
CREATE PROCEDURE while_proc()
BEGIN
    DECLARE hap INT;
    DECLARE num INT;
    SET hap = 0;
    SET num = 1;
    WHILE (num <= 100) DO
        SET hap = hap + num;
        SET num = num + 1;
    END WHILE;
    SELECT hap AS '1~100 합계';
END $$

CALL while_proc();   -- 결과: 5050
```

**동적 SQL** — 이름 그대로 다이나믹하게 SQL이 변경됨 (테이블 이름을 매개변수로 전달받아 해당 테이블 조회)

```sql
CREATE PROCEDURE dynamic_proc(IN tableName VARCHAR(20))
BEGIN
    SET @sqlQuery = CONCAT('SELECT * FROM ', tableName);
    PREPARE myQuery FROM @sqlQuery;
    EXECUTE myQuery;
    DEALLOCATE PREPARE myQuery;
END $$

CALL dynamic_proc('member');
```

> 💡 동적 SQL은 실시간으로 SQL을 만든 후 실행함.

**스토어드 프로시저 삭제**: `DROP PROCEDURE 프로시저_이름;`

---

<!-- 이번 챕터에서는 확인문제를 실습 인증으로 대체하여 진행합니다. 제시된 실습을 흐름에 맞게 진행한 후, 실습 과정이 보일 수 있도록 인증 사진을 2장 이상 제출해 주세요. -->

<img width="1356" height="892" alt="image" src="https://github.com/user-attachments/assets/447dd60d-2ab9-49a9-8d03-0934a4bbc6fc" />
<img width="1232" height="920" alt="image" src="https://github.com/user-attachments/assets/b03020f2-5789-44e6-bde6-be1a22904e49" />
<img width="1265" height="912" alt="image" src="https://github.com/user-attachments/assets/44524c4c-5e60-4484-994a-6378453dbb1f" />
<img width="1272" height="967" alt="image" src="https://github.com/user-attachments/assets/34e0e7d5-b07b-4e0a-aa30-a362be8b59b2" />




## 2. 스토어드 함수와 커서 

---

## 스토어드 함수란?

**스토어드 함수(stored function)**는 MySQL이 제공하는 내장 함수(SUM, CAST, CONCAT 등) 외에 **사용자가 직접 함수를 만드는 기능**. 모양은 스토어드 프로시저와 비슷하지만 용도가 다름.

## 스토어드 함수 vs 스토어드 프로시저

| 구분 | 스토어드 함수 | 스토어드 프로시저 |
|---|---|---|
| 반환 | `RETURNS`로 형식 지정, `RETURN`으로 **반드시 하나의 값 반환** | 반환 의무 없음 |
| 매개변수 | 모두 **입력 매개변수** (IN 생략) | IN/OUT 모두 가능 |
| 호출 방법 | **SELECT 문 안에서** 호출 | **CALL**로 호출 |
| SELECT 사용 | 본문 안에서 **SELECT 사용 불가** | 본문 안에서 SELECT 사용 가능 |
| 주 용도 | 계산을 통해 **하나의 값을 반환**하는 데 사용 | 여러 SQL 문·로직 처리 등 다양한 용도 |

## 기본 형식

```sql
DELIMITER $$
CREATE FUNCTION 스토어드_함수_이름(매개변수)
    RETURNS 반환형식
BEGIN
    이 부분에 프로그래밍 코딩
    RETURN 반환값;
END $$
DELIMITER ;

SELECT 스토어드_함수_이름();
```

> 📝 내장 함수(built-in function): MySQL이 기본 제공하는 함수. 제어 흐름/문자열/수학/날짜·시간/전체 텍스트 검색/형 변환/XML/비트/보안·압축/정보/공간 분석 함수 등 수백 개가 넘음.

## 사용 전 설정

```sql
SET GLOBAL log_bin_trust_function_creators = 1;   -- 스토어드 함수 생성 권한 허용 (최초 1회만 설정)
```

**함수의 반환값을 변수에 저장 — SELECT ~ INTO ~**

```sql
SELECT calcYearFunc(2007) INTO @debut2007;
SELECT calcYearFunc(2013) INTO @debut2013;
SELECT @debut2007 - @debut2013 AS '2007과 2013 차이';
```

**함수를 테이블 조회와 결합** — 보통 테이블을 조회한 후 그 값을 계산할 때 사용

```sql
SELECT mem_id, mem_name, calcYearFunc(YEAR(debut_date)) AS '활동 햇수'
    FROM member;
```

**함수 내용 확인/삭제**

```sql
SHOW CREATE FUNCTION 함수_이름;   -- 코드 확인
DROP FUNCTION calcYearFunc;       -- 삭제
```

---

## 커서로 한 행씩 처리하기

**커서(cursor)**는 테이블에서 **한 행씩 처리**하기 위한 방식. 첫 번째 행을 처리한 후 마지막 행까지 한 행씩 접근해서 값을 처리하며, 대부분 **스토어드 프로시저와 함께** 사용됨.

### 커서의 작동 순서

1. 커서 선언하기
2. 반복 조건 선언하기
3. 커서 열기
4. (데이터 가져오기 → 데이터 처리하기) ← 반복
5. 커서 닫기

---


<!-- 이번 챕터에서는 확인문제를 실습 인증으로 대체하여 진행합니다. 제시된 실습을 흐름에 맞게 진행한 후, 실습 과정이 보일 수 있도록 인증 사진을 2장 이상 제출해 주세요. -->

<img width="1275" height="980" alt="image" src="https://github.com/user-attachments/assets/64997c52-8335-43d3-8134-b413dfa797f4" />
<img width="1277" height="972" alt="image" src="https://github.com/user-attachments/assets/f7f0025f-8fe0-47bd-abc2-f693067e17a0" />
<img width="1320" height="995" alt="image" src="https://github.com/user-attachments/assets/6b48014f-1e3d-41c5-b06f-22d0b1f69541" />



## 3. 자동 실행되는 트리거 

---

## 트리거란?

**트리거(trigger)**는 사전적으로 '방아쇠'를 뜻함. 총의 방아쇠를 당기면 자동으로 총알이 나가듯, 트리거는 테이블에 **INSERT, UPDATE, DELETE 문이 작동할 때 자동으로 실행**되는 프로그래밍 기능.

- 사용자가 추가 작업을 잊어버리는 실수를 방지해줌 (예: 데이터 삭제 시 다른 곳에 자동 백업)
- 데이터에 오류가 발생하는 것을 막을 수 있음 → **데이터의 무결성**이라고도 부름

## 트리거의 기본 작동

트리거는 테이블의 **DML(Data Manipulation Language) 문**(INSERT, UPDATE, DELETE 등)의 **이벤트**가 발생할 때 작동함. 테이블에 미리 **부착(attach)**되는 프로그램 코드라고 생각하면 됨.

> 📝 이 책에서 다루는 트리거는 **AFTER 트리거**임 (BEFORE 트리거는 작동 방식이 조금 다름). AFTER 트리거를 기준으로 설명.

**특징**

- 스토어드 프로시저와 문법은 비슷하지만, **CALL 문으로 직접 실행 불가** — 오직 테이블에 INSERT/UPDATE/DELETE 이벤트가 발생할 때만 자동 실행
- 스토어드 프로시저와 달리 **IN, OUT 매개변수 사용 불가**

## 기본 형식

```sql
DELIMITER $$
CREATE TRIGGER myTrigger       -- ① 트리거 이름
    AFTER DELETE                -- ② DELETE 문 발생 이후 작동
    ON trigger_table             -- ③ 트리거를 부착할 테이블
    FOR EACH ROW                 -- ④ 각 행마다 적용
BEGIN
    SET @msg = '가수 그룹이 삭제됨';  -- ⑤ 실행 시 작동되는 코드
END $$
DELIMITER ;
```

## 트리거 활용 — 변경 이력 백업

은행 계좌처럼 **누가 입력/수정/삭제했는지** 알아야 하는 중요한 정보는, 트리거로 변경 전 데이터와 변경한 사용자·시간을 자동 기록할 수 있음.

**백업 테이블** (수정·삭제 전 데이터 보관용 + 변경 타입/날짜/사용자 추가)

```sql
CREATE TABLE singer (SELECT mem_id, mem_name, mem_number, addr FROM member);

CREATE TABLE backup_singer
( mem_id      CHAR(8) NOT NULL,
  mem_name    VARCHAR(10) NOT NULL,
  mem_number  INT NOT NULL,
  addr        CHAR(2) NOT NULL,
  modType     CHAR(2),       -- 변경된 타입. '수정' 또는 '삭제'
  modDate     DATE,          -- 변경된 날짜
  modUser     VARCHAR(30)    -- 변경한 사용자
);
```

**UPDATE 트리거**

```sql
CREATE TRIGGER singer_updateTrg
    AFTER UPDATE
    ON singer
    FOR EACH ROW
BEGIN
    INSERT INTO backup_singer VALUES( OLD.mem_id, OLD.mem_name,
        OLD.mem_number, OLD.addr, '수정', CURDATE(), CURRENT_USER() );
END $$
```

> **OLD 테이블**: UPDATE나 DELETE가 수행될 때, **변경되기 전의 데이터가 잠깐 저장되는 임시 테이블**. CURDATE()는 현재 날짜, CURRENT_USER()는 현재 작업 중인 사용자를 알려줌.

**DELETE 트리거** — UPDATE 트리거와 거의 동일 (이벤트와 modType만 다름)

```sql
CREATE TRIGGER singer_deleteTrg
    AFTER DELETE
    ON singer
    FOR EACH ROW
BEGIN
    INSERT INTO backup_singer VALUES( OLD.mem_id, OLD.mem_name,
        OLD.mem_number, OLD.addr, '삭제', CURDATE(), CURRENT_USER() );
END $$
```

> 📝 하나의 테이블에 여러 개의 트리거를 부착시켜도 됨.

### 주의 — TRUNCATE TABLE은 트리거를 작동시키지 않음

```sql
TRUNCATE TABLE singer;   -- DELETE FROM 테이블_이름과 동일하게 모든 행 삭제
SELECT * FROM backup_singer;   -- 삭제된 내용이 들어가지 않음!
```

> ⚠️ **TRUNCATE TABLE**은 DELETE와 같은 효과(전체 행 삭제)를 내지만, **DELETE 트리거는 오직 DELETE 문에만 작동**하므로 TRUNCATE로 삭제하면 트리거가 작동하지 않음.

## 트리거가 사용하는 임시 테이블 — NEW / OLD

INSERT, UPDATE, DELETE 작업 시 MySQL이 내부적으로 관리하는 임시 시스템 테이블 2개.

| 테이블 | 시점 | 설명 |
|---|---|---|
| **NEW** | INSERT(새 값) | 새 값이 테이블에 들어가기 전, 잠깐 NEW 테이블에 저장됨 (많이 사용되지는 않음) |
| **OLD** | DELETE(예전 값) | 삭제될 예전 값이 삭제되기 전, 잠깐 OLD 테이블에 저장됨 → AFTER DELETE 트리거에서 `OLD.열이름`으로 예전 값 접근 가능 |
| **NEW + OLD** | UPDATE(새 값, 예전 값) | 새 값은 NEW에, 변경 전 값은 OLD에 모두 저장됨 |

---

<!-- 이번 챕터에서는 확인문제를 실습 인증으로 대체하여 진행합니다. 제시된 실습을 흐름에 맞게 진행한 후, 실습 과정이 보일 수 있도록 인증 사진을 2장 이상 제출해 주세요. -->

<img width="1325" height="1011" alt="image" src="https://github.com/user-attachments/assets/e3477fa9-0023-4896-a9d2-235f6f286ae1" />
<img width="1271" height="987" alt="image" src="https://github.com/user-attachments/assets/9bcff00c-ee8a-4864-9cac-650de167dd4c" />


---


### 🎉 수고하셨습니다.






