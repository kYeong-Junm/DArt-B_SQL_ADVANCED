# SQL_ADVANCED 4주차 정규 과제 

📌SQL_ADVANCED 정규과제는 매주 정해진 분량의 『*혼자 공부하는 SQL*』 을 읽고 학습하는 것입니다. 이번주는 아래의 **SQL_ADVANCED_4th_TIL**에 나열된 분량을 읽고 공부하시면 됩니다.

아래의 문제를 풀어보며 학습 내용을 점검하세요. 문제를 해결하는 과정에서 개념을 스스로 정리하고, 필요한 경우 제시된 강의를 참고하여 보완하는 것이 좋습니다.

<!-- 강의 링크는 아래와 같습니다.
https://www.youtube.com/watch?v=DMNpkj_bZIs&list=PLVsNizTWUw7GCfy5RH27cQL5MeKYnl8Pm&index=13
https://www.youtube.com/watch?v=BUHj-behLyc&list=PLVsNizTWUw7GCfy5RH27cQL5MeKYnl8Pm&index=14
https://www.youtube.com/watch?v=JrXWxku7ZIM&list=PLVsNizTWUw7GCfy5RH27cQL5MeKYnl8Pm&index=15
-->

**교재 실습 예제 파일은 08_SQL_ADVANCED_Template 레포지토리의 src 폴더에 업로드되어 있습니다. market_db 파일도 해당 폴더에 함께 포함되어 있으니 참고하시기 바랍니다.**

**👀(수행 인증샷은 필수입니다.)** 

## SQL_ADVANCED_4th_TIL

### 5장 테이블과 뷰
#### 01. 테이블 만들기
#### 02. 제약조건으로 테이블을 견고하게
#### 03. SQL 가상의 테이블: 뷰 


## Study Schedule

| 주차  | 공부 범위     | 완료 여부 |
| ----- | ------------- | --------- |
| 1주차 | p.24~99    | ✅         |
| 2주차 | p.102~155   | ✅         |
| 3주차 | p.158~213  | ✅         |
| 4주차 | p.216~271 | ✅         |
| 5주차 | p.274~327 | 🍽️         |
| 6주차 | p.330~369 | 🍽️         |
| 7주차 | p.372~407 | 🍽️         |


<br>

<!-- 여기까진 그대로 둬 주세요-->

---

# 1️⃣ 학습 내용 정리

## 1. 테이블 만들기 

# 05-1 테이블 만들기

> 핵심 키워드: `CREATE TABLE` `AUTO_INCREMENT` `NOT NULL` `PRIMARY KEY` `FOREIGN KEY`

## 1. 테이블이란?
- 행(row)과 열(column)로 구성된 2차원 구조 (엑셀 시트와 비슷)
- **행** = 로우(row) = 레코드(record)
- **열** = 컬럼(column) = 필드(field)

## 2. 테이블 설계
테이블을 만들기 전에 **테이블 이름, 열 이름, 데이터 형식, 기본 키** 등을 먼저 정한다.

### 회원 테이블 (member)
| 열 이름 | 데이터 형식 | NOT NULL | 기타 |
|---|---|---|---|
| mem_id | CHAR(8) | Yes | PK |
| mem_name | VARCHAR(10) | Yes | |
| mem_number | TINYINT | Yes | |
| addr | CHAR(2) | Yes | |
| phone1 | CHAR(3) | No | |
| phone2 | CHAR(8) | No | |
| height | TINYINT | No | UNSIGNED |
| debut_date | DATE | No | |

### 구매 테이블 (buy)
| 열 이름 | 데이터 형식 | NOT NULL | 기타 |
|---|---|---|---|
| num | INT | Yes | PK, 자동 증가 |
| mem_id | CHAR(8) | Yes | FK |
| prod_name | CHAR(6) | Yes | |
| group_name | CHAR(4) | No | |
| price | INT | Yes | UNSIGNED |
| amount | SMALLINT | Yes | UNSIGNED |

## 3. GUI(MySQL Workbench)로 테이블 만들기
- `Tables` 우클릭 → `Create Table`
- 체크박스 의미

| 체크 | 의미 |
|---|---|
| PK | 기본 키 (Primary Key) |
| NN | NOT NULL |
| UQ | UNIQUE |
| UN | UNSIGNED |
| AI | AUTO_INCREMENT |

- GUI에서는 외래 키 관계를 바로 지정할 수 없으므로, `Apply`를 누른 뒤 뜨는 **Apply SQL Script to Database 창에서 코드를 직접 수정**한다.

```sql
  PRIMARY KEY (`num`),
  FOREIGN KEY(mem_id) REFERENCES member(mem_id)
);
```

## 4. SQL로 테이블 만들기

### 데이터베이스 생성
```sql
DROP DATABASE IF EXISTS naver_db;
CREATE DATABASE naver_db;
USE naver_db;
```

### 회원 테이블
```sql
DROP TABLE IF EXISTS member;
CREATE TABLE member
( mem_id      CHAR(8) NOT NULL PRIMARY KEY,
  mem_name    VARCHAR(10) NOT NULL,
  mem_number  TINYINT NOT NULL,
  addr        CHAR(2) NOT NULL,
  phone1      CHAR(3) NULL,
  phone2      CHAR(8) NULL,
  height      TINYINT UNSIGNED NULL,
  debut_date  DATE NULL
);
```

### 구매 테이블
```sql
DROP TABLE IF EXISTS buy;
CREATE TABLE buy
( num         INT AUTO_INCREMENT NOT NULL PRIMARY KEY,
  mem_id      CHAR(8) NOT NULL,
  prod_name   CHAR(6) NOT NULL,
  group_name  CHAR(4) NULL,
  price       INT UNSIGNED NOT NULL,
  amount      SMALLINT UNSIGNED NOT NULL,
  FOREIGN KEY(mem_id) REFERENCES member(mem_id)
);
```

## 5. 주요 개념

### NULL / NOT NULL
- `NULL`: 빈 값 허용 (아무것도 지정하지 않으면 기본값은 NULL)
- `NOT NULL`: 반드시 값을 입력해야 함
- PRIMARY KEY로 지정한 열은 생략해도 자동으로 NOT NULL

### AUTO_INCREMENT
- 1부터 자동으로 1씩 증가
- 입력할 때 해당 열은 `NULL`로 두면 자동으로 채워짐
- **AUTO_INCREMENT 열은 반드시 PRIMARY KEY 또는 UNIQUE로 지정해야 함**
  - MySQL은 자동 증가 열이 키(인덱스)여야 다음 번호를 빠르게 찾을 수 있음
  - 키가 없으면 `Error 1075` 발생

### FOREIGN KEY
- 형식: `FOREIGN KEY(열_이름) REFERENCES 기준_테이블(열_이름)`
- buy의 mem_id에는 member의 mem_id에 **존재하는 값만** 입력 가능
- 목적: 주인 없는 구매 기록 같은 잘못된 데이터를 DB가 스스로 막게 함 (데이터 무결성)


## 6. 데이터 입력
```sql
INSERT INTO member VALUES('TWC', '트와이스', 9, '서울', '02', '11111111', 167, '2015-10-19');
INSERT INTO member VALUES('BLK', '블랙핑크', 4, '경남', '055', '22222222', 163, '2016-8-8');
INSERT INTO member VALUES('WMN', '여자친구', 6, '경기', '031', '33333333', 166, '2015-1-15');

INSERT INTO buy VALUES(NULL, 'BLK', '지갑', NULL, 30, 2);
INSERT INTO buy VALUES(NULL, 'BLK', '맥북프로', '디지털', 1000, 1);
INSERT INTO buy VALUES(NULL, 'APN', '아이폰', '디지털', 200, 1);  -- 오류 발생
```
- APN은 member에 없으므로 외래 키 제약조건 위반 → **Error 1452** (의도된 오류)
- 해결하려면 member에 APN을 먼저 입력(회원가입)해야 함



## 2. 제약조건으로 테이블을 견고하게 

# 05-2 제약조건으로 테이블을 견고하게

> 핵심 키워드: `기본 키` `외래 키` `고유 키` `체크` `기본값` `NOT NULL`

## 1. 제약조건(Constraint)이란?
- **데이터의 무결성**을 지키기 위해 제한하는 조건
- 무결성 = 데이터에 결함이 없음 (예: 회원 아이디 중복이 없음)

### MySQL의 대표 제약조건
- PRIMARY KEY
- FOREIGN KEY
- UNIQUE
- CHECK
- DEFAULT
- NULL 값 허용

---

## 2. 기본 키 (PRIMARY KEY)
- 각 행을 구분하는 **식별자** (예: 아이디, 학번, 사번)
- **중복 불가, NULL 불가**
- 테이블당 **1개만** 지정 가능
- 기본 키를 지정하면 **클러스터형 인덱스**가 자동 생성됨

### 지정 방법 3가지
```sql
-- ① 열 이름 뒤에 지정
CREATE TABLE member
( mem_id   CHAR(8) NOT NULL PRIMARY KEY,
  mem_name VARCHAR(10) NOT NULL,
  height   TINYINT UNSIGNED NULL
);

-- ② 마지막 줄에 지정
CREATE TABLE member
( mem_id   CHAR(8) NOT NULL,
  mem_name VARCHAR(10) NOT NULL,
  height   TINYINT UNSIGNED NULL,
  PRIMARY KEY (mem_id)
);

-- ③ ALTER TABLE로 지정
ALTER TABLE member
    ADD CONSTRAINT
    PRIMARY KEY (mem_id);
```

### 기본 키에 이름 붙이기
```sql
CONSTRAINT PRIMARY KEY PK_member_mem_id (mem_id)
```

### 테이블 정보 확인
```sql
DESCRIBE member;   -- DESC로 줄여 써도 됨, Key 열에 PRI 표시
```

---

## 3. 외래 키 (FOREIGN KEY)
- 두 테이블 사이의 관계를 연결하고 데이터 무결성을 보장
- **기준 테이블**: 기본 키가 있는 테이블 (member)
- **참조 테이블**: 외래 키가 있는 테이블 (buy)
- 참조하는 기준 테이블의 열은 반드시 **PRIMARY KEY 또는 UNIQUE**여야 함
- 두 테이블의 열 이름은 달라도 됨 (예: buy의 `user_id` → member의 `mem_id`)

### 지정 방법
```sql
-- ① CREATE TABLE에서
CREATE TABLE buy
( num       INT AUTO_INCREMENT NOT NULL PRIMARY KEY,
  mem_id    CHAR(8) NOT NULL,
  prod_name CHAR(6) NOT NULL,
  FOREIGN KEY(mem_id) REFERENCES member(mem_id)
);

-- ② ALTER TABLE로
ALTER TABLE buy
    ADD CONSTRAINT
    FOREIGN KEY(mem_id) REFERENCES member(mem_id);
```

### 테이블 삭제 순서
- 외래 키로 연결된 경우 **참조 테이블(buy)을 먼저**, 기준 테이블(member)을 나중에 삭제
```sql
DROP TABLE IF EXISTS buy, member;
```

### 기준 테이블의 값을 변경/삭제하면?
- 참조 테이블에 해당 데이터가 있으면 기준 테이블 값을 **변경·삭제할 수 없음**
```sql
UPDATE member SET mem_id = 'PINK' WHERE mem_id = 'BLK';  -- Error 1451
DELETE FROM member WHERE mem_id = 'BLK';                 -- Error 1451
```

### ON UPDATE CASCADE / ON DELETE CASCADE
- 기준 테이블이 바뀌면 참조 테이블도 **자동으로 함께** 변경·삭제
```sql
ALTER TABLE buy
    ADD CONSTRAINT
    FOREIGN KEY(mem_id) REFERENCES member(mem_id)
    ON UPDATE CASCADE
    ON DELETE CASCADE;
```
| 구문 | 동작 |
|---|---|
| ON UPDATE CASCADE | member의 BLK → PINK 변경 시 buy의 BLK도 PINK로 변경 |
| ON DELETE CASCADE | member의 PINK 삭제 시 buy의 PINK 구매 기록도 삭제 |

---

## 4. 고유 키 (UNIQUE)
- **중복 불가**, 하지만 **NULL은 허용** (NULL은 여러 개 가능)
- 테이블에 **여러 개** 지정 가능
- 예: 이메일, 휴대폰 번호
```sql
CREATE TABLE member
( mem_id   CHAR(8) NOT NULL PRIMARY KEY,
  mem_name VARCHAR(10) NOT NULL,
  height   TINYINT UNSIGNED NULL,
  email    CHAR(30) NULL UNIQUE
);
```
- 중복 입력 시 `Error 1062: Duplicate entry`
- UNIQUE + NOT NULL이면 기본 키와 동일하게 동작

### 기본 키 vs 고유 키
| 구분 | 기본 키 | 고유 키 |
|---|---|---|
| 중복 | 불가 | 불가 |
| NULL | 불가 | 허용 |
| 개수 | 테이블당 1개 | 여러 개 가능 |

---

## 5. 체크 (CHECK)
- 입력되는 데이터가 **조건에 맞는지 검사**
```sql
-- 열 정의에서 지정
height TINYINT UNSIGNED NULL CHECK (height >= 100)

-- ALTER TABLE로 지정
ALTER TABLE member
    ADD CONSTRAINT
    CHECK (phone1 IN ('02', '031', '032', '054', '055', '061'));
```
- 조건 위반 시 `Error 3819: Check constraint ... is violated`

---

## 6. 기본값 (DEFAULT)
- 값을 입력하지 않으면 **자동으로 들어갈 값**을 미리 지정
```sql
-- CREATE TABLE에서
height TINYINT UNSIGNED NULL DEFAULT 160

-- ALTER TABLE에서 (ALTER COLUMN 사용)
ALTER TABLE member
    ALTER COLUMN phone1 SET DEFAULT '02';

-- 입력 시 default라고 쓰면 기본값이 들어감
INSERT INTO member VALUES('SPC', '우주소녀', default, default);
```

---

## 7. NULL 값 허용
- 허용: 생략하거나 `NULL`
- 허용 안 함: `NOT NULL`
- PRIMARY KEY 열은 생략해도 자동으로 NOT NULL
- **NULL ≠ 공백('') ≠ 0** → NULL은 '아무것도 없음'

---

## 8. 정리
| 용어 | 설명 |
|---|---|
| 제약조건 | 데이터 무결성을 지키기 위한 제한 조건 |
| ALTER TABLE | 이미 만든 테이블을 수정하는 SQL |
| ADD CONSTRAINT | 제약조건을 추가하는 SQL |
| 기준 테이블 | 기본 키가 설정된 테이블 |
| 참조 테이블 | 외래 키가 설정된 테이블 |
| ON UPDATE CASCADE | 기준 테이블 변경 시 참조 테이블도 변경 |
| ON DELETE CASCADE | 기준 테이블 삭제 시 참조 테이블도 삭제 |


> **확인문제: 다음 보기 중에서 각 문항이 설명하는 것을 고르세요.**

보기는 아래와 같습니다.
```
CHECK / DEFAULT / PRIMAY KEY / UNIQUE / NOT NULL / FOREIGN KEY
```

```
여기에 답과 그 이유를 적어주세요!
1. 입력되는 데이터가 조건에 맞는지 검사하는 기능: **CHECK**
   - CHECK는 열에 `CHECK (height >= 100)`처럼 조건을 걸어두고, 입력되는 값이 조건을 만족하는지 검사한다.

2. 값을 입력하지 않으면 자동으로 들어갈 값: **DEFAULT**
   - DEFAULT는 `DEFAULT 160`처럼 기본값을 미리 지정해두는 것이다.
   - 값을 생략하거나 `default`라고 입력하면 지정해둔 기본값이 자동으로 들어간다.

3. 빈 값을 입력하는 것을 허용하지 않음: **NOT NULL**
   - NOT NULL로 지정한 열은 반드시 값을 입력해야 하며, NULL(빈 값)을 넣을 수 없다.
```


## 3. 가상의 테이블: 뷰 

<!-- 뷰에 관해 배우게 된 점을 적어주세요. -->

> **확인문제: 다음은 뷰의 특징입니다. 거리가 먼 것을 하나 고르세요.**

보기는 아래와 같습니다.
```
1️⃣ 뷰에는 테이블의 모든 열을 포함시켜야 합니다.
2️⃣ 뷰는 복잡한 SQL을 단순하게 만드는 효과가 있습니다.
3️⃣ 뷰는 보안에 도움이 됩니다.
4️⃣ 일부 사용자가 테이블에는 접근하지 못하게 하고, 뷰에만 접근하도록 설정할 수 있습니다.
```

```
여기에 답과 그 이유를 적어주세요!
```


---

# 2️⃣ 실습과제

## 1. 데이터베이스 구축

아래 코드를 MySQL Workbench에 붙여넣은 후,  
**전체 드래그 → 실행 (Ctrl + Shift + Enter)** 하여 데이터베이스를 생성하세요.

```sql
CREATE DATABASE IF NOT EXISTS week4_db;
USE week4_db;
```

## 2. 실습문제

1. 다음 조건을 만족하는 `users` 테이블을 생성하시오.
```
- user_id는 INT이며 **기본키(Primary Key)**로 설정합니다.
- name은 VARCHAR(20)이며 NULL을 허용하지 않습니다.
- email은 VARCHAR(50)이며 중복을 허용하지 않습니다.
- signup_date는 DATE 타입으로 설정합니다.
- grade는 INT이며 기본값(Default)을 1로 설정합니다.
```

2. 다음 조건을 만족하는 `orders` 테이블을 생성하시오.
```
- order_id는 INT이며 기본키(Primary Key)로 설정합니다.
- user_id는 INT이며 NULL을 허용하지 않습니다.
- amount는 INT이며 0보다 커야 합니다.
- order_date는 DATE 타입으로 설정합니다.
```

3. 다음 조건을 만족하여 데이터를 삽입하시오.
```
- users 테이블에 3명 이상의 데이터를 직접 INSERT 하시오. (단, user 중 본인이 포함돼야 함)
- orders 테이블에 3건 이상의 데이터를 직접 INSERT 하시오.
```

4. users와 orders 테이블을 활용하여 다음 컬럼을 보여주는 뷰 user_order_view를 생성하시오.
```
- user_id
- name
- amount
```

5. 생성한 user_order_view를 조회하시오.

## 3. 제출 방법

1. 각 문제의 실행 결과가 보이도록 화면을 캡처합니다.
2. 테이블 생성 결과, 데이터 삽입 결과, 뷰 생성 및 조회 결과가 모두 보이도록 제출합니다.

<!-- 이 부분을 지우고 인증사진을 제출해주세요.-->

### 🎉 수고하셨습니다.






