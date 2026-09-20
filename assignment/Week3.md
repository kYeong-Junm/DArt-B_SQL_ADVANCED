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

---

## 조인이란

**조인(join)이란 두 개의 테이블을 서로 묶어서 하나의 결과를 만들어 내는 것**을 말함. 두 테이블을 엮어야만 원하는 형태가 나오는 경우가 많음.

예: 회원 테이블(이름, 연락처)과 구매 테이블(구매한 물건) → 물건을 배송하려면 **회원 이름·연락처 + 구매한 물건 정보**가 함께 필요 → 이렇게 두 테이블을 엮어서 하나의 배송 정보를 추출하는 것이 대표적인 조인.

---

## 일대다 관계의 이해

두 테이블의 조인을 위해서는 테이블이 **일대다(one to many) 관계**로 연결되어야 함.

- 데이터베이스의 테이블은 하나로 구성되기보다는 여러 정보를 주제에 따라 분리해서 저장하는 것이 효율적. 이 분리된 테이블은 서로 **관계(relation)**를 맺고 있음
- **일대다 관계란 한쪽 테이블에는 하나의 값만 존재해야 하지만, 연결된 다른 테이블에는 여러 개의 값이 존재할 수 있는 관계**를 말함

### 예시: 회원 테이블 ↔ 구매 테이블

- 회원 테이블에서 블랙핑크의 아이디는 'BLK'로 1명(1, one)밖에 없음 → 회원 테이블의 아이디를 **기본 키(Primary Key, PK)**로 지정
- 구매 테이블의 아이디에서는 3개의 BLK를 찾을 수 있음 → 회원은 1명이지만 이 회원이 구매를 여러 번(다, many) 할 수 있음 → 구매 테이블의 아이디는 기본 키가 아닌 **외래 키(Foreign Key, FK)**로 설정

> 일대다 관계는 주로 **기본 키(PK)와 외래 키(FK) 관계**로 맺어져 있음. 그래서 일대다 관계를 **'PK-FK 관계'**라고 부르기도 함

- 회사원-급여 테이블, 학생-학점 테이블도 마찬가지로 1명이 여러 급여/학점을 받는 일대다 관계
- 꼭 기본 키-외래 키 관계가 아니어도 가능한 조인도 있음 (예: 상호 조인). 상호 조인 외의 조인은 기본 키-외래 키 관계가 핵심 요소임

---

## 내부 조인

일반적으로 조인이라고 부르는 것은 **내부 조인(inner join)**을 말하는 것으로, **조인 중에서 가장 많이 사용**됨. 조인은 3개 이상의 테이블로도 할 수 있지만 대부분 2개로 조인함.

### 내부 조인의 형식

```sql
SELECT <열 목록>
FROM <첫 번째 테이블>
    INNER JOIN <두 번째 테이블>
    ON <조인될 조건>
[WHERE 검색 조건]
```

> `INNER JOIN`을 그냥 `JOIN`이라고만 써도 `INNER JOIN`으로 인식함

### 실습: 구매 테이블 기준으로 회원 정보 조인하기

```sql
USE market_db;
SELECT *
    FROM buy
    INNER JOIN member
    ON buy.mem_id = member.mem_id
    WHERE buy.mem_id = 'GRL';
```

> 두 개의 테이블(buy, member)을 조인하는 경우 동일한 열 이름이 존재한다면 꼭 **테이블_이름.열_이름 형식**으로 표기해야 함. 여기서는 `buy.mem_id`와 `member.mem_id`를 사용함

### 내부 조인의 처리 과정

1. 구매 테이블의 `mem_id`(`buy.mem_id`)인 'GRL'을 추출합니다.
2. 'GRL'과 동일한 값을 회원 테이블의 `mem_id`(`member.mem_id`) 열에서 검색합니다.
3. 'GRL'이라는 아이디를 찾으면 구매 테이블과 회원 테이블의 두 행을 결합(JOIN)합니다.

### WHERE 절을 생략하면?

`WHERE buy.mem_id = 'GRL'`을 생략하면, 원래는 구매 테이블의 7번째(GRL)에 대해서만 결합했지만, WHERE 절을 생략하면 **1번째 BLK부터 12번째 MMU까지 구매 테이블의 모든 행이 회원 테이블과 결합**함.

```sql
SELECT *
    FROM buy
    INNER JOIN member
    ON buy.mem_id = member.mem_id;
```

> MySQL 버전에 따라 실행 결과의 차례는 다를 수 있음

---

## 내부 조인의 간결한 표현

열이 너무 많아 복잡해 보이므로 필요한 아이디/이름/구매 물품/주소/연락처만 추출:

```sql
SELECT mem_id, mem_name, prod_name, addr, CONCAT(phone1, phone2) '연락처'
    FROM buy
    INNER JOIN member
    ON buy.mem_id = member.mem_id;
```

### 열 이름이 불확실한 경우 오류 발생

```
Error Code: 1052. Column 'mem_id' in field list is ambiguous
```
- 회원 아이디(`mem_id`)는 회원 테이블, 구매 테이블에 모두 들어 있어서 **어느 테이블의 mem_id인지 헷갈린다**는 뜻
- 이럴 때는 어느 테이블의 mem_id를 추출할지 정확하게 작성해야 함 (`buy.mem_id`처럼)

```sql
SELECT buy.mem_id, mem_name, prod_name, addr, CONCAT(phone1, phone2) '연락처'
    FROM buy
    INNER JOIN member
    ON buy.mem_id = member.mem_id;
```

### 모든 열 이름을 명확히 표기

SQL을 좀 더 명확히 하기 위해서 SELECT 다음의 열 이름(컬럼 이름)에도 모두 **테이블_이름.열_이름 형식**으로 작성 가능 (결과는 동일):

```sql
SELECT buy.mem_id, member.mem_name, buy.prod_name, member.addr,
       CONCAT(member.phone1, member.phone2) '연락처'
    FROM buy
    INNER JOIN member
    ON buy.mem_id = member.mem_id;
```

### 별칭(alias) 사용하기

각 열이 어느 테이블에 속한 것인지 명확해졌지만 코드가 너무 길어져서 오히려 복잡해 보임. 이를 간결하게 표현하기 위해서는 **FROM 절에 나오는 테이블의 이름 뒤에 별칭(alias)을 줄 수 있음**. 여러 개의 테이블이 관련된 조인에서는 이 방식을 사용할 것을 적극 권장.

```sql
SELECT B.mem_id, M.mem_name, B.prod_name, M.addr,
       CONCAT(M.phone1, M.phone2) '연락처'
    FROM buy B
    INNER JOIN member M
    ON B.mem_id = M.mem_id;
```

---

## 내부 조인의 활용

**전체 회원**의 아이디/이름/구매한 제품/주소를 회원 아이디 순으로 정렬하여 출력:

```sql
SELECT M.mem_id, M.mem_name, B.prod_name, M.addr
    FROM buy B
    INNER JOIN member M
    ON B.mem_id = M.mem_id
    ORDER BY M.mem_id;
```

구매 테이블의 목록이 12건이었으므로 이상 없이 잘 나옴. 하지만 결과는 **'전체 회원'이 아닌 '구매한 기록이 있는 회원들'의 목록**임 (한 번도 구매하지 않은 회원의 정보는 없음).

> **내부 조인은 두 테이블에 모두 있는 내용만 출력됨.** 만약 양쪽 중에 한곳이라도 내용이 있을 때 조인하려면 **외부 조인**을 사용해야 함

### 중복된 결과 1개만 출력하기 (DISTINCT 활용)

"한 번이라도 구매한 기록이 있는 회원들에게 감사 안내문을 발송"하려는 경우, 내부 조인 결과에서 중복된 이름은 필요 없으므로 **DISTINCT 문**을 활용해 회원의 주소를 조회 가능:

```sql
SELECT DISTINCT M.mem_id, M.mem_name, M.addr
    FROM buy B
    INNER JOIN member M
    ON B.mem_id = M.mem_id
    ORDER BY M.mem_id;
```

---

## 외부 조인

**내부 조인은 두 테이블에 모두 데이터가 있어야만 결과가 나옴. 이와 달리 외부 조인은 한쪽에만 데이터가 있어도 결과가 나옴.**

### 외부 조인의 기본

**외부 조인(outer join)은 두 테이블을 조인할 때 필요한 내용이 한쪽 테이블에만 있어도 결과를 추출**할 수 있음. 자주 사용되지는 않지만, 가끔 사용되는 방식이므로 알아두면 유용함.

### 외부 조인의 형식

```sql
SELECT <열 목록>
FROM <첫 번째 테이블(LEFT 테이블)>
    <LEFT | RIGHT | FULL> OUTER JOIN <두 번째 테이블(RIGHT 테이블)>
    ON <조인될 조건>
[WHERE 검색 조건];
```

내부 조인보다는 조금 복잡해 보이지만 사용 방법은 거의 비슷함.

### LEFT OUTER JOIN

내부 조인에서 해결하지 못한 '전체 회원의 구매 기록(구매 기록이 없는 회원의 정보도 함께) 출력'을 외부 조인으로 만들기:

```sql
SELECT M.mem_id, M.mem_name, B.prod_name, M.addr
    FROM member M
    LEFT OUTER JOIN buy B
    ON M.mem_id = B.mem_id
    ORDER BY M.mem_id;
```
- **왼쪽에 있는 회원 테이블을 기준으로 외부 조인**함
- `LEFT OUTER JOIN`을 줄여서 `LEFT JOIN`이라고만 써도 됨
- **`LEFT OUTER JOIN` 문의 의미를 '왼쪽 테이블(member)의 내용은 모두 출력되어야 한다' 정도로 해석**하면 기억하기 쉬움
- 결과: 구매 기록이 없는 회원(OMY, RED, SPC, TWC, WMN 등)도 `prod_name`이 NULL인 상태로 함께 출력됨 → **외부 조인은 한쪽 테이블에만 있는 내용도 출력됨**

### RIGHT OUTER JOIN

동일한 결과를 출력하려면 단순히 왼쪽과 오른쪽 테이블의 위치만 바꾸면 됨:

```sql
SELECT M.mem_id, M.mem_name, B.prod_name, M.addr
    FROM buy B
    RIGHT OUTER JOIN member M
    ON M.mem_id = B.mem_id
    ORDER BY M.mem_id;
```
- **오른쪽에 있는 회원 테이블을 기준으로 외부 조인**함

### 외부 조인의 활용: 구매 이력이 없는 회원 찾기

내부 조인으로는 구매 기록이 있는 회원 목록만 추출했었는데, 이번엔 반대로 **회원 가입만 하고 한 번도 구매한 적이 없는 회원의 목록**을 추출:

```sql
SELECT DISTINCT M.mem_id, B.prod_name, M.mem_name, M.addr
    FROM member M
    LEFT OUTER JOIN buy B
    ON M.mem_id = B.mem_id
    WHERE B.prod_name IS NULL
    ORDER BY M.mem_id;
```
- 한 번도 구매하지 않았으므로 조인된 결과의 물건 이름(`prod_name`)이 당연히 비어있을 것 → **`IS NULL` 구문은 널(NULL) 값인지 비교**함

### FULL OUTER JOIN

**FULL OUTER JOIN은 왼쪽 외부 조인과 오른쪽 외부 조인이 합쳐진 것**이라고 생각하면 됨. 왼쪽이든 오른쪽이든 한쪽에 들어 있는 내용이면 출력함. 자주 사용되지는 않으니 이 정도만 알아두면 됨.

---

## 핵심 개념 요약

| 개념 | 설명 |
|---|---|
| 일대다(one to many) 관계 | 한쪽 테이블에는 하나의 값만 존재하지만, 연결된 다른 테이블에는 여러 개의 값이 존재할 수 있는 관계. 주로 기본 키(PK)-외래 키(FK) 관계로 맺어짐 |
| 내부 조인(INNER JOIN) | 두 테이블에 모두 데이터가 있는 행만 결합. 가장 많이 사용되는 조인 (그냥 JOIN이라 쓰면 내부 조인) |
| 외부 조인(OUTER JOIN) | 한쪽 테이블에만 데이터가 있어도 결과를 출력. LEFT/RIGHT/FULL 세 종류 |
| LEFT OUTER JOIN | 왼쪽 테이블의 내용은 모두 출력 |
| RIGHT OUTER JOIN | 오른쪽 테이블의 내용은 모두 출력 |
| FULL OUTER JOIN | 왼쪽·오른쪽 외부 조인을 합친 것 (한쪽에라도 있으면 출력) |
| DISTINCT | 조인 결과에서 중복된 행을 제거하고 한 번만 출력 |
| 별칭(alias) | FROM 절의 테이블 이름 뒤에 짧은 별칭을 붙여 SQL을 간결하게 표현 |

<img width="1252" height="990" alt="image" src="https://github.com/user-attachments/assets/ff9e4d63-df2d-4ad6-90b2-8874aad044aa" />
<img width="1172" height="995" alt="image" src="https://github.com/user-attachments/assets/5a99c90c-c9ec-49a4-8127-c8c1f998f803" />
<img width="1212" height="987" alt="image" src="https://github.com/user-attachments/assets/25ae9e0b-7288-49f4-a853-f9ad5e42e016" />
<img width="1202" height="992" alt="image" src="https://github.com/user-attachments/assets/b85646ba-cc4d-41b1-a437-81c0799b55c7" />



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
답: WHERE B.prod_name IS NULL
이유: 조인 결과에서 특정 조건으로 행을 걸러낼 때는 WHERE 절을 사용한다. JOIN은 조건 필터링 키워드가 아니고, LIMIT은 결과 개수 제한, HAVING은 GROUP BY와 함께 쓰이므로 이 쿼리에는 맞지 않는다.
```

## 3. SQL 프로그래밍 

---

## 스토어드 프로시저

SQL은 SELECT, INSERT, UPDATE, DELETE 등을 사용해서 C, 자바, 파이썬 같은 프로그래밍 언어와는 많이 달라 보이지만, 필요하다면 **SQL만으로도 프로그램을 만들 수 있음**.

**스토어드 프로시저는 MySQL에서 프로그래밍 기능이 필요할 때 사용하는 데이터베이스 개체**임. **SQL 프로그래밍은 기본적으로 스토어드 프로시저 안에 만들어야 함**.

### 스토어드 프로시저의 구조

```sql
DELIMITER $$
CREATE PROCEDURE 스토어드_프로시저_이름()
BEGIN
    이 부분에 SQL 프로그래밍 코딩
END $$
DELIMITER ;
CALL 스토어드_프로시저_이름();
```

- `DELIMITER $$` : 종료 문자를 세미콜론(`;`)에서 `$$`로 변경 (스토어드 프로시저의 코딩 부분 시작)
- `END $$` : 스토어드 프로시저 종료
- `DELIMITER ;` : 종료 문자를 다시 세미콜론(`;`)으로 변경
- `CALL 스토어드_프로시저_이름();` : 스토어드 프로시저 실행

> 일반적으로 구분 문자(DELIMITER)는 `$$`를 많이 사용하지만, 원한다면 `/`, `&`, `@` 등을 사용해도 상관없음. 다른 기호와 중복될 수 있으므로 기호 2개를 연속해서 사용하는 것이 좋음

**스토어드 프로시저는 `DELIMITER $$ ~ END $$` 안에 작성하고 `CALL`로 호출**함.

---

## IF 문

IF 문은 **조건문으로 가장 많이 사용되는 프로그래밍 문법 중 하나**임. IF 문을 활용하면 다양한 조건을 처리할 수 있음.

### IF 문의 기본 형식

IF 문은 **조건식이 참이라면 'SQL문장들'을 실행하고, 그렇지 않으면 그냥 넘어감**.

```sql
IF <조건식> THEN
    SQL문장들
END IF;
```

- **'SQL문장들'이 한 문장이라면 그 문장만 써도 되지만, 두 문장 이상이 처리되어야 할 때는 BEGIN~END로 묶어줘야 함**. 현재는 한 문장이더라도 나중에 추가될 수 있으니 습관적으로 BEGIN~END로 묶어주는 것을 권장

```sql
DROP PROCEDURE IF EXISTS ifProc1;
DELIMITER $$
CREATE PROCEDURE ifProc1()
BEGIN
    IF 100 = 100 THEN
        SELECT '100은 100과 같습니다.';
    END IF;
END $$
DELIMITER ;
CALL ifProc1();
```

> 다른 프로그래밍 언어에서는 같다는 의미로 `==`을 사용하지만, SQL은 `=`을 사용함. 그리고 **SELECT 뒤에 문자가 나오면 그냥 화면에 출력**해줌 (다른 언어의 `print()`와 비슷한 기능)

### IF ~ ELSE 문

**IF ~ ELSE 문은 조건에 따라 다른 부분을 수행**함. 조건식이 참이라면 'SQL문장들1'을 실행하고, 그렇지 않으면 'SQL문장들2'를 실행함.

```sql
DROP PROCEDURE IF EXISTS ifProc2;
DELIMITER $$
CREATE PROCEDURE ifProc2()
BEGIN
    DECLARE myNum INT;
    SET myNum = 200;
    IF myNum = 100 THEN
        SELECT '100입니다.';
    ELSE
        SELECT '100이 아닙니다.';
    END IF;
END $$
DELIMITER ;
CALL ifProc2();
```
- **`DECLARE` 예약어를 사용해서 변수를 선언**함. 제일 뒤에는 변수의 데이터 형식을 지정
- **`SET` 예약어로 변수에 값을 대입**함

### IF 문의 활용 (실전 예제)

기존 테이블과 함께 IF 문을 활용해, 아이디가 APN(에이핑크)인 회원의 데뷔 일자가 5년이 넘었는지 확인해보고 축하 메시지를 출력하는 예제.

```sql
DROP PROCEDURE IF EXISTS ifProc3;
DELIMITER $$
CREATE PROCEDURE ifProc3()
BEGIN
    DECLARE debutDate DATE;   -- 데뷔 일자
    DECLARE curDate DATE;     -- 오늘
    DECLARE days INT;         -- 활동한 일수

    SELECT debut_date INTO debutDate
        FROM market_db.member
        WHERE mem_id = 'APN';

    SET curDate = CURRENT_DATE();               -- 현재 날짜
    SET days = DATEDIFF(curDate, debutDate);      -- 날짜의 차이, 일 단위

    IF (days/365) >= 5 THEN   -- 5년이 지났다면
        SELECT CONCAT('데뷔한 지 ', days, '일이나 지났습니다. 핑순이들 축하합니다!');
    ELSE
        SELECT '데뷔한 지 ' + days + '일밖에 안되었네요. 핑순이들 화이팅~';
    END IF;
END $$
DELIMITER ;
CALL ifProc3();
```
- `SELECT ... INTO 변수` : 그냥 SELECT와 달리 **`INTO 변수`가 붙으면 결과를 변수에 저장**함
- **`CURRENT_DATE()`**: 오늘 날짜를 알려줌
- **`CURRENT_TIMESTAMP()`**: 오늘 날짜 및 시간을 함께 알려줌
- **`DATEDIFF(날짜1, 날짜2)`**: 날짜2부터 날짜1까지 일수로 몇일인지 알려줌

---

## CASE 문

여러 가지 조건 중에서 선택해야 하는 경우도 있음. 이럴 때 **CASE 문을 사용해서 조건을 설정**할 수 있음.

### CASE 문의 기본 형식

**IF 문은 참 아니면 거짓 두 가지만 있기 때문에 '2중 분기'라는 용어를 사용**함. **CASE 문은 2가지 이상의 여러 가지 경우일 때 처리가 가능하므로 '다중 분기'라고 부름**.

```sql
CASE
    WHEN 조건1 THEN
        SQL문장들1
    WHEN 조건2 THEN
        SQL문장들2
    WHEN 조건3 THEN
        SQL문장들3
    ELSE
        SQL문장들4
END CASE;
```

- **CASE와 END CASE 사이에는 여러 조건을 넣을 수 있음**. `WHEN` 다음에 조건이 나오는데, 조건이 여러 개라면 `WHEN`을 여러 번 반복. 모든 조건에 해당하지 않으면 마지막 `ELSE` 부분을 수행
- SQL의 CASE 문은 다른 프로그래밍 언어의 SWITCH ~ CASE 문과 비슷한 기능을 함

```sql
DROP PROCEDURE IF EXISTS caseProc;
DELIMITER $$
CREATE PROCEDURE caseProc()
BEGIN
    DECLARE point INT;
    DECLARE credit CHAR(1);
    SET point = 88;

    CASE
        WHEN point >= 90 THEN
            SET credit = 'A';
        WHEN point >= 80 THEN
            SET credit = 'B';
        WHEN point >= 70 THEN
            SET credit = 'C';
        WHEN point >= 60 THEN
            SET credit = 'D';
        ELSE
            SET credit = 'F';
    END CASE;

    SELECT CONCAT('취득점수==>', point), CONCAT('학점==>', credit);
END $$
DELIMITER ;
CALL caseProc();
```

### CASE 문의 활용: 회원 등급 나누기

회원별 총 구매액을 계산해서 4단계 등급(최우수고객/우수고객/일반고객/유령고객)으로 나누는 실전 예제.

```sql
-- 1) 회원별 총 구매액 구하기 (GROUP BY)
SELECT mem_id, SUM(price*amount) "총구매액"
    FROM buy
    GROUP BY mem_id
    ORDER BY SUM(price*amount) DESC;

-- 2) 구매하지 않은 회원까지 모두 포함 (내부 조인 → 외부 조인으로 변경)
SELECT M.mem_id, M.mem_name, SUM(price*amount) "총구매액"
    FROM buy B
        RIGHT OUTER JOIN member M
        ON B.mem_id = M.mem_id
    GROUP BY M.mem_id
    ORDER BY SUM(price*amount) DESC;

-- 3) CASE 문으로 등급 열 추가
SELECT M.mem_id, M.mem_name, SUM(price*amount) "총구매액",
    CASE
        WHEN (SUM(price*amount)) >= 1500 THEN '최우수고객'
        WHEN (SUM(price*amount)) >= 1000 THEN '우수고객'
        WHEN (SUM(price*amount)) >= 1) THEN '일반고객'
        ELSE '유령고객'
    END "회원등급"
    FROM buy B
        RIGHT OUTER JOIN member M
        ON B.mem_id = M.mem_id
    GROUP BY M.mem_id
    ORDER BY SUM(price*amount) DESC;
```
- 구매 테이블에는 구매한 회원만 있어 정보가 없는 회원은 **`SELECT`에서 회원 테이블의 아이디(`M.mem_id`)를 조회하고 `GROUP BY`도 `M.mem_id`로 지정**해야 함
- CASE 문을 새로운 열로 추가할 때는 **콤마(`,`)로 구분해서 열의 마지막에 추가**하고, 열 이름에 별칭 지정 가능

---

## WHILE 문

프로그래밍에서 꼭 필요한 부분 중 하나가 **반복**임. **WHILE 문은 필요한 만큼 계속 같은 내용을 반복**할 수 있음.

### WHILE 문의 기본 형식

**WHILE 문은 조건식이 참인 동안에 'SQL문장들'을 계속 반복**함.

```sql
WHILE <조건식> DO
    SQL 문장들
END WHILE;
```
> SQL의 WHILE 문은 일반 프로그래밍 언어의 WHILE 문과 같은 개념

```sql
DROP PROCEDURE IF EXISTS whileProc;
DELIMITER $$
CREATE PROCEDURE whileProc()
BEGIN
    DECLARE i INT;     -- 1에서 100까지 증가할 변수
    DECLARE hap INT;   -- 더한 값을 누적할 변수
    SET i = 1;
    SET hap = 0;

    WHILE (i <= 100) DO
        SET hap = hap + i;   -- hap의 원래 값에 i를 더해서 다시 hap에 넣으라는 의미
        SET i = i + 1;       -- i의 원래 값에 1을 더해서 다시 i에 넣으라는 의미
    END WHILE;

    SELECT '1부터 100까지의 합 ==>', hap;
END $$
DELIMITER ;
CALL whileProc();
```

### WHILE 문의 응용: ITERATE, LEAVE

WHILE 문은 단순히 조건식이 참인 동안 반복하지만, 중간에 특정 조건에서 건너뛰거나 반복을 중단하고 싶을 때는 **ITERATE 문과 LEAVE 문**을 활용할 수 있음.

- **`ITERATE [레이블]`**: 지정한 레이블로 가서 계속 진행함
- **`LEAVE [레이블]`**: 지정한 레이블을 빠져나감. 즉 **WHILE 문이 종료**됨

> ITERATE 문은 프로그래밍 언어의 CONTINUE와, LEAVE 문은 BREAK 문과 비슷한 역할을 함

```sql
DROP PROCEDURE IF EXISTS whileProc2;
DELIMITER $$
CREATE PROCEDURE whileProc2()
BEGIN
    DECLARE i INT;
    DECLARE hap INT;
    SET i = 1;
    SET hap = 0;

    myWhile: -- WHILE 문에 레이블 지정
    WHILE (i <= 100) DO
        IF (i%4 = 0) THEN
            SET i = i + 1;
            ITERATE myWhile;   -- 지정한 label 문으로 가서 계속 진행 (4의 배수는 건너뜀)
        END IF;
        SET hap = hap + i;
        IF (hap > 1000) THEN
            LEAVE myWhile;     -- 지정한 label 문을 떠남. 즉 WHILE 종료
        END IF;
        SET i = i + 1;
    END WHILE;

    SELECT '1부터 100까지의 합(4의 배수 제외), 1000 넘으면 종료 ==>', hap;
END $$
DELIMITER ;
CALL whileProc2();
```

---

## 동적 SQL

SQL 문은 내용이 고정되어 있는 경우가 대부분이지만, 상황에 따라 내용 변경이 필요할 때 **동적 SQL을 사용하면 변경되는 내용을 실시간으로 적용시켜 사용**할 수 있음.

### PREPARE와 EXECUTE

**PREPARE는 SQL 문을 실행하지는 않고 미리 준비만 해놓고, EXECUTE는 준비한 SQL 문을 실행**함. 실행 후에는 **`DEALLOCATE PREPARE`로 문장을 해제**해주는 것이 바람직함.

```sql
USE market_db;
PREPARE myQuery FROM 'SELECT * FROM member WHERE mem_id = "BLK"';
EXECUTE myQuery;
DEALLOCATE PREPARE myQuery;
```

이렇게 **미리 SQL을 준비한 후에 나중에 실행하는 것을 동적 SQL**이라고 부름.

### 동적 SQL의 활용: ? 와 USING

**PREPARE 문에서는 `?`로 향후에 입력될 값을 비워 놓고, EXECUTE에서 `USING`으로 `?`에 값을 전달**할 수 있음. 그러면 실시간으로 필요한 값들을 전달해서 동적으로 SQL이 실행됨.

실전 예제: 출입문에서 출입한 내역을 기록하는 테이블에, 태그하는 순간의 날짜와 시간이 INSERT 문으로 자동 입력되도록 처리:

```sql
DROP TABLE IF EXISTS gate_table;
CREATE TABLE gate_table (id INT AUTO_INCREMENT PRIMARY KEY, entry_time DATETIME);

SET @curDate = CURRENT_TIMESTAMP();   -- 현재 날짜와 시간

PREPARE myQuery FROM 'INSERT INTO gate_table VALUES(NULL, ?)';
EXECUTE myQuery USING @curDate;
DEALLOCATE PREPARE myQuery;

SELECT * FROM gate_table;
```
- `?`를 사용해서 `entry_time`에 입력할 값을 비워 놓음
- `USING` 문으로 앞에서 준비한 `@curDate` 변수를 넣은 후 실행됨 → 실행한 시점의 날짜와 시간이 입력됨

> 일반 SQL에서 변수는 `@변수명`으로 지정하는데 별도의 선언은 없어도 됨. **스토어드 프로시저에서 변수는 `DECLARE`로 선언한 후에 사용**해야 함

**PREPARE와 EXECUTE로 동적 SQL 문을 만들 수 있음.**

---

## 핵심 개념 요약

| 개념/문법 | 설명 |
|---|---|
| 스토어드 프로시저 | MySQL에서 프로그래밍 기능이 필요할 때 사용하는 데이터베이스 개체. `DELIMITER $$ ~ END $$` 안에 작성하고 `CALL`로 호출 |
| `DECLARE` | 변수를 선언 (스토어드 프로시저 안에서 사용할 때 필수) |
| `SET` | 변수에 값을 대입 |
| `IF ~ THEN ~ END IF` | 조건이 참일 때만 실행하는 2중 분기 |
| `IF ~ ELSE ~ END IF` | 조건의 참/거짓에 따라 다른 문장을 실행 |
| `CASE ~ WHEN ~ END CASE` | 2가지 이상의 여러 조건을 처리하는 다중 분기 |
| `WHILE ~ DO ~ END WHILE` | 조건식이 참인 동안 반복 |
| `ITERATE` | 지정한 레이블로 가서 반복을 계속 진행 (CONTINUE와 유사) |
| `LEAVE` | 지정한 레이블(반복문)을 빠져나감 (BREAK와 유사) |
| `PREPARE` / `EXECUTE` | SQL 문을 미리 준비해두고(PREPARE), 필요할 때 실행(EXECUTE)하는 동적 SQL |
| `DEALLOCATE PREPARE` | 준비된 SQL 문(PREPARE)을 해제 |

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
(1) WHEN
(2) CASE
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


<img width="1292" height="985" alt="image" src="https://github.com/user-attachments/assets/cc4c675f-8f38-48d2-b3ea-d0f076546677" />
<img width="1197" height="991" alt="image" src="https://github.com/user-attachments/assets/f3a6c806-2a05-4396-b2be-de5501b19d28" />
<img width="1212" height="1001" alt="image" src="https://github.com/user-attachments/assets/93011f3e-5b26-4bfd-9d15-972c20b5e011" />
<img width="1192" height="972" alt="image" src="https://github.com/user-attachments/assets/7877dbac-4e58-4ebe-a091-96450775f560" />
<img width="1227" height="981" alt="image" src="https://github.com/user-attachments/assets/79d485a8-4110-4048-9544-dbb161eb4d1d" />



### 🎉 수고하셨습니다.






