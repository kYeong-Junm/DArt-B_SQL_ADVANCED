# SQL_ADVANCED 3주차 정규 과제 

📌SQL_ADVANCED 정규과제는 매주 정해진 분량의 『*혼자 공부하는 SQL*』 을 읽고 학습하는 것입니다. 이번주는 아래의 **SQL_ADVANCED_3rd_TIL**에 나열된 분량을 읽고 공부하시면 됩니다.

아래의 문제를 풀어보며 학습 내용을 점검하세요. 문제를 해결하는 과정에서 개념을 스스로 정리하고, 필요한 경우 제시된 강의를 참고하여 보완하는 것이 좋습니다.

<!-- 강의 링크는 아래와 같습니다.
https://www.youtube.com/watch?v=1YmWy-7-OhQ&list=PLVsNizTWUw7GCfy5RH27cQL5MeKYnl8Pm&index=10
https://www.youtube.com/watch?v=tuQFkzjqEGw&list=PLVsNizTWUw7GCfy5RH27cQL5MeKYnl8Pm&index=11
https://www.youtube.com/watch?v=IOCsreDYqFE&list=PLVsNizTWUw7GCfy5RH27cQL5MeKYnl8Pm&index=12
-->

**교재 실습 예제 파일은 08_SQL_ADVANCED_Template 레포지토리의 src 폴더에 업로드되어 있습니다. market_db 파일도 해당 폴더에 함께 포함되어 있으니 참고하시기 바랍니다.**

**👀(수행 인증샷은 필수입니다.)** 

## SQL_ADVANCED_3rd_TIL

### 4장 SQL 고급 문법
#### 01. MySQL의 데이터 형식
#### 02. 두 테이블을 묶는 조인
#### 03. SQL 프로그래밍 


## Study Schedule

| 주차  | 공부 범위     | 완료 여부 |
| ----- | ------------- | --------- |
| 1주차 | p.24~99    | ✅         |
| 2주차 | p.102~155   | ✅         |
| 3주차 | p.158~213  | ✅         |
| 4주차 | p.216~271 | 🍽️         |
| 5주차 | p.274~327 | 🍽️         |
| 6주차 | p.330~369 | 🍽️         |
| 7주차 | p.372~407 | 🍽️         |


<br>

<!-- 여기까진 그대로 둬 주세요-->

---

# 1️⃣ 학습 내용 정리

## 1. MySQL의 데이터 형식

## 📌 학습 목표

- 테이블을 만들 때 **적절한 데이터 형식**을 고르는 이유를 이해한다.
- 정수형 / 문자형 / 대량 데이터 / 실수형 / 날짜형의 종류와 범위를 안다.
- **변수**를 선언하고 사용한다.
- **형 변환**(명시적 / 암시적)을 이해한다.

**핵심 키워드**: 정수형 · 문자형 · 실수형 · 날짜형 · 변수 · 형 변환

---

## 1. 데이터 형식이 필요한 이유

데이터에 맞는 크기의 형식을 지정해야 저장 공간을 낭비하지 않는다.
예) 이름을 저장하려고 100칸을 잡으면 낭비 → 5칸이면 충분.

---

## 2. 정수형 (소수점 없는 숫자)

| 데이터 형식 | 바이트 수 | 숫자 범위 |
|---|---|---|
| TINYINT | 1 | -128 ~ 127 |
| SMALLINT | 2 | -32,768 ~ 32,767 |
| INT | 4 | 약 -21억 ~ +21억 |
| BIGINT | 8 | 약 -900경 ~ +900경 |

### 범위를 벗어나면?

```sql
INSERT INTO hongong4 VALUES(128, 32768, 2147483648, 90000000000000000000);
-- Error Code: 1264. Out of range value for column 'tinyint_col' at row 1
```

### UNSIGNED
- 붙이면 **0부터 시작**하는 범위가 된다 (음수 없음).
- `TINYINT UNSIGNED` : 0 ~ 255 (같은 1바이트로 범위가 2배)
- `SMALLINT UNSIGNED` : 0 ~ 65535
- 예) 키(height)는 `TINYINT UNSIGNED`가 적합.

---

## 3. 문자형

| 데이터 형식 | 설명 | 범위 |
|---|---|---|
| CHAR(개수) | **고정길이** 문자형 | 1 ~ 255 |
| VARCHAR(개수) | **가변길이** 문자형 | 1 ~ 16383 |

- `CHAR(10)`에 '가나다' 저장 → 10자리 확보 (7자리 낭비)
- `VARCHAR(10)`에 '가나다' 저장 → 3자리만 사용
- 글자 수가 **고정**이면 CHAR (성능상 약간 유리), **변동**이면 VARCHAR
- 예) 시도(서울/경기) → `CHAR(2)`, 그룹 이름 → `VARCHAR(10)`

### 숫자인데 문자형으로 저장하는 경우
- 국번 `02`, `031`처럼 **앞의 0**이 필요한 값
- 전화번호처럼 **연산/크기 비교에 의미가 없는** 값

> 숫자로서 의미가 있으려면 ① 더하기/빼기 연산 ② 크다/작다, 순서 중 하나는 해당돼야 한다.

---

## 4. 대량의 데이터 형식

`CHAR(256)`, `VARCHAR(16384)`처럼 너무 크게 지정하면 오류가 난다.

```
Error Code: 1074. Column length too big for column 'data1' (max = 255); use BLOB or TEXT instead
```

| 구분 | 데이터 형식 | 바이트 수 |
|---|---|---|
| TEXT 형식 | TEXT | 1 ~ 65535 |
| | LONGTEXT | 1 ~ 4294967295 (약 4GB) |
| BLOB 형식 | BLOB | 1 ~ 65535 |
| | LONGBLOB | 1 ~ 4294967295 (약 4GB) |

- **LONGTEXT** : 소설, 영화 자막 같은 대량 텍스트
- **LONGBLOB** : 이미지, 동영상 같은 이진(Binary) 데이터

```sql
CREATE DATABASE netflix_db;
USE netflix_db;
CREATE TABLE movie
  (movie_id       INT,
   movie_title    VARCHAR(30),
   movie_director VARCHAR(20),
   movie_star     VARCHAR(20),
   movie_script   LONGTEXT,
   movie_film     LONGBLOB
);
```

---

## 5. 실수형 (소수점 있는 숫자)

| 데이터 형식 | 바이트 수 | 설명 |
|---|---|---|
| FLOAT | 4 | 소수점 아래 7자리까지 |
| DOUBLE | 8 | 소수점 아래 15자리까지 |

과학 기술용이 아니면 FLOAT로 충분. 예) 시력 2.0, 1.5, 0.7

---

## 6. 날짜형

| 데이터 형식 | 바이트 수 | 설명 |
|---|---|---|
| DATE | 3 | 날짜만. `YYYY-MM-DD` |
| TIME | 3 | 시간만. `HH:MM:SS` |
| DATETIME | 8 | 날짜+시간. `YYYY-MM-DD HH:MM:SS` |

- 날짜/시간을 입력할 때는 문자처럼 **작은따옴표**로 묶는다.
- 데뷔 일자 → `DATE`, 구매 기록(시각까지 필요) → `DATETIME`

---

## 7. 변수

- 선언/대입: `SET @변수이름 = 값;`
- 출력: `SELECT @변수이름;`
- MySQL 워크벤치를 **종료하면 사라지는 임시 값**

```sql
USE market_db;
SET @myVar1 = 5;
SET @myVar2 = 4.25;

SELECT @myVar1;              -- 5
SELECT @myVar1 + @myVar2;    -- 9.25

SET @txt = '가수 이름==> ';
SET @height = 166;
SELECT @txt, mem_name FROM member WHERE height > @height;
-- 소녀시대, 잇지, 트와이스
```

### LIMIT에는 변수를 직접 쓸 수 없다 → PREPARE / EXECUTE

```sql
SET @count = 3;
PREPARE mySQL FROM 'SELECT mem_name, height FROM member ORDER BY height LIMIT ?';
EXECUTE mySQL USING @count;
-- 오마이걸 160, 레드벨벳 161, 우주소녀 162
```

- `PREPARE` : SQL을 실행하지 않고 준비만 (`?`는 나중에 채워질 자리)
- `EXECUTE ... USING` : 변수 값을 `?`에 넣어 실행

---

## 8. 데이터 형 변환

### 8-1. 명시적 변환 : `CAST()` / `CONVERT()`

```sql
CAST(값 AS 데이터_형식 [(길이)])
CONVERT(값, 데이터_형식 [(길이)])
```

- 변환 대상으로 쓸 수 있는 형식: `CHAR`, `SIGNED`, `UNSIGNED`, `DATE`, `TIME`, `DATETIME`
- `INT`, `TINYINT` 같은 이름은 쓸 수 없어서 정수 변환은 **SIGNED / UNSIGNED**를 사용
- `SIGNED` = 부호 있는 정수, `UNSIGNED` = 부호 없는 정수 (`SIGNED INTEGER`로 써도 됨)

```sql
SELECT AVG(price) AS '평균 가격' FROM buy;                 -- 142.9167
SELECT CAST(AVG(price) AS SIGNED) '평균 가격' FROM buy;    -- 143 (반올림)
SELECT CONVERT(AVG(price), SIGNED) '평균 가격' FROM buy;   -- 143
```

**다양한 구분자를 날짜로 변환**

```sql
SELECT CAST('2022$12$12' AS DATE);
SELECT CAST('2022/12/12' AS DATE);
SELECT CAST('2022%12%12' AS DATE);
SELECT CAST('2022@12@12' AS DATE);   -- 모두 2022-12-12
```

**숫자를 문자로 바꿔 이어 붙이기 (`CONCAT`)**

```sql
SELECT num, CONCAT(CAST(price AS CHAR), 'X', CAST(amount AS CHAR), '=')
       '가격X수량', price*amount '구매액'
FROM buy;
```

### 8-2. 암시적 변환 (함수 없이 자동 변환)

```sql
SELECT '100' + '200';          -- 300     (문자 → 숫자로 자동 변환 후 덧셈)
SELECT CONCAT('100', '200');   -- 100200  (문자 연결)
SELECT CONCAT(100, '200');     -- 100200  (숫자 100이 문자로 변환)
SELECT 100 + '200';            -- 300     (문자 '200'이 숫자로 변환)
```

> `CONCAT()`을 쓰면 숫자가 문자로, `+`만 쓰면 문자가 숫자로 변환되어 연산된다.

---

## 9. 확인문제 정답

| 번호 | 정답 | 비고 |
|---|---|---|
| 1 | TINYINT, SMALLINT, INT, BIGINT | 크기 순 |
| 2 | ② Out of range | |
| 3 | ② 데이터가 양수만 저장됨 | 범위가 0부터 시작 |
| 4 | ③ CHAR는 최대 4GB까지 저장됨 | CHAR는 최대 255자 |
| 5 | ① 전화번호 국번, ② 전화번호 뒷자리 | 연산 의미 없는 숫자 |
| 6 | ① LONGTEXT, LONGBLOB | 자막 / 동영상 |
| 7 | CONVERT(), CAST() | 형 변환 함수 |

---

## 🧠 핵심 요약

- 정수형 4종: TINYINT < SMALLINT < INT < BIGINT, `UNSIGNED`를 붙이면 0부터 시작
- 문자형: 고정 `CHAR`, 가변 `VARCHAR`
- 대량 데이터: 텍스트 `LONGTEXT`, 이진 `LONGBLOB`
- 실수형: `FLOAT`(7자리), `DOUBLE`(15자리)
- 날짜형: `DATE`, `TIME`, `DATETIME`
- 변수는 `@`로 시작하고 `SET`으로 대입, `LIMIT`에는 `PREPARE`/`EXECUTE` 사용
- 형 변환 함수: `CAST()`, `CONVERT()`

---

## 🛠️ 실습하며 겪은 문제와 해결 (Workbench 팁)

| 증상 | 원인 | 해결 |
|---|---|---|
| 오류가 안 남 / 코드가 안 돌아감 | 오타(`TIMYINT`) 또는 값이 책과 다름 | 오타·숫자(0의 개수) 확인 |
| `Error 1146: Table ... doesn't exist` | `CREATE TABLE`을 실행하지 않았거나 다른 DB가 선택됨 | `USE market_db;` 함께 실행, `SHOW TABLES;`로 확인 |
| `Error 1049: Unknown database` | 해당 DB가 없음 | DB와 테이블부터 다시 생성 |
| `Error 1064` | 문법 오류 (열 이름과 자료형 사이 공백 없음, `;` 누락 등) | 공백, 쉼표, 세미콜론 확인 |
| `Error 1452` | 외래 키가 참조하는 member 데이터가 없음 | **member 먼저** INSERT 후 buy INSERT |
| 여러 줄 선택했는데 한 줄만 실행됨 | 커서 모양의 번개(⚡)나 `Ctrl+Enter`는 **문장 1개**만 실행 | **맨 왼쪽 번개(⚡)** 또는 `Ctrl+Shift+Enter` |
| SELECT 결과가 안 보임 | 결과는 Output이 아니라 **Result Grid**에 표시 | Result Grid 탭 확인 |
| `USE`가 `0 row(s) affected` | 정상 (DB만 바꾸는 명령) | 문제 아님 |
| buy의 num이 1이 아닌 2부터 시작 | 실패한 INSERT도 AUTO_INCREMENT 번호를 소모 | 필요하면 `DELETE FROM buy; ALTER TABLE buy AUTO_INCREMENT = 1;` 후 재삽입 |

### 실행 규칙 정리
- 선택(드래그)이 있으면 **선택한 부분만** 실행된다.
- 선택이 없을 때 첫 번째 번개는 전체, 두 번째 번개는 커서가 있는 문장 1개를 실행한다.
- 데이터는 DB(서버)에 저장되므로 **코드를 지워도 데이터는 남는다.**
- `DROP DATABASE`가 들어간 줄은 실수로 전체 실행되지 않게 지워 둔다.


<img width="1247" height="912" alt="image" src="https://github.com/user-attachments/assets/db4f8830-e735-4d59-9dcc-e44b02e472b2" />
<img width="1211" height="922" alt="image" src="https://github.com/user-attachments/assets/e05699f7-9a74-4328-b5d5-302becc7c35d" />
<img width="1226" height="930" alt="image" src="https://github.com/user-attachments/assets/47e49af3-1aa3-42d7-892f-1bc2b0a1fe8b" />
<img width="1157" height="917" alt="image" src="https://github.com/user-attachments/assets/e63dc8f0-65f2-46c5-bcde-daed3ca1c77b" />


> **확인문제: 다음 보기에서 데이터 형식의 변환에 사용되는 함수를 2개 고르세요.**

보기는 아래와 같습니다.
```
CONVERT() / DATA() / CAST() / MOVE() / TYPE() / SUM() / AVG() / CURRENT_DATE()
```

```
CONVERT(), CAST()
```


## 2. 두 테이블을 묶는 조인

<!-- 두 테이블을 묶는 조인에 관해 배우게 된 점을 적어주세요. -->
<!-- 과제 설명 예시처럼 직접 실습 후 인증 사진 4장 이상을 첨부해주세요. -->

> **확인문제: 다음 SQL은 회원으로 가입만 하고, 한 번도 구매한 적이 없는 회원의 목록을 조회하는 쿼리입니다. 빈칸에 들어갈 가장 적절한 구문을 고르세요..**

```sql
SELECT DISTINCT M.mem_id, B.prod_name, M.mem_name, M.addr
  FROM member M
    LEFT OUTER JOIN buy B
    ON M.mem_id = B.mem_id
  __________
  ORDER BY M.mem_id;
```
보기는 아래와 같습니다.
```
1. JOIN B.prod_name IS NULL
2. LIMIT B.prod_name IS NULL
3. HAVING B.prod_name IS NULL
4. WHERE B.prod_name IS NULL
```
```
여기에 답과 그 이유를 적어주세요!
```

## 3. SQL 프로그래밍 

<!-- IF문, CASE문, WHILE문에 관해 배우게 된 점을 적어주세요. -->

> **확인문제: 다음은 CASE 문의 형식입니다. 빈칸에 들어갈 가장 적절한 명령어를 보기에서 고르세요..**

```sql
CASE
    (1) 조건 THEN
        SQL문장들1
    ELSE
        SQL문장들4
END (2);
```

보기는 아래와 같습니다.
```
WHEN / THEN / CURRENT / DATE / TIME / IF / END IF / CASE
```

```
여기에 답을 적어주세요!
(1)
(2) 
```


---

# 2️⃣ 실습과제

## 1. 데이터베이스 구축

아래 코드를 MySQL Workbench에 붙여넣은 후,  
**전체 드래그 → 실행 (Ctrl + shift + Enter)** 하여 데이터베이스를 구축하세요.

```sql
-- 1. 데이터베이스 생성
CREATE DATABASE IF NOT EXISTS week3_db;

-- 2. 사용할 데이터베이스 선택
USE week3_db;

-- 3. 기존 테이블 삭제 (초기화용)
DROP TABLE IF EXISTS orders;
DROP TABLE IF EXISTS customers;

-- 4. 테이블 생성 (조인 실습용)
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    name VARCHAR(20),
    signup_date_str VARCHAR(8) 
);

CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    customer_id INT,           
    order_date_str VARCHAR(8), 
    amount_str VARCHAR(10)     
);

-- 5. 데이터 삽입
INSERT INTO customers VALUES
(1, '신영', '20241528'),
(2, '경모', '20220261'),
(3, '세원', '20203401'),
(4, '진우', '20221024'),
(5, '성환', '20225100'),
(6, '혜준', '20244946'),
(7, '채은', '20250412'),
(8, '다나', '20212774'); -- 주문 없는 고객(외부 조인용)

INSERT INTO orders VALUES
(101, 1, '20240220', '12000'),
(102, 1, '20240303', '30000'),
(103, 2, '20240111', '15000'),
(104, 3, '20221201', '9000'),
(105, 5, '20231111', '20000'),
(106, 7, '20220707', '5000'),
(107, 99, '20240210', '7000'); -- 고객 테이블에 없는 customer_id (외부 조인용)
```

## 2. 실습 문제

다음 SQL 문을 작성하고 실행 결과를 확인 후 인증 사진을 아래에 업로드하세요.

1. **데이터 형식 변환**
   - orders 테이블의 `order_date_str`을 DATE 형식으로 변환하여 조회하시오.
   (힌트: STR_TO_DATE 사용)

2. **데이터 형식 변환**
   - orders 테이블의 `amount_str`을 숫자형으로 변환하여 조회하시오.

3. **내부 조인 (INNER JOIN)**
   - customers와 orders를 customer_id 기준으로 내부 조인하여
     고객 이름(name)과 주문 번호(order_id)를 함께 조회하시오.

4. **외부 조인 (LEFT JOIN)**
   - customers를 기준으로 LEFT JOIN을 수행하여,
     주문이 없는 고객도 함께 조회하시오.

5. **스토어드 프로시저 (IF문 사용)**
   - 입력받은 금액이 10000 이상이면 '고액 주문',
     그렇지 않으면 '일반 주문'을 출력하는
     프로시저를 생성하시오.
   - 생성 후 CALL로 실행 결과를 확인하시오.


<!-- 이 부분을 지우고 인증사진을 제출해주세요.-->


### 🎉 수고하셨습니다.






