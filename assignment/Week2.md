# SQL_ADVANCED 2주차 정규 과제 

📌SQL_ADVANCED 정규과제는 매주 정해진 분량의 『*혼자 공부하는 SQL*』 을 읽고 학습하는 것입니다. 이번주는 아래의 **SQL_ADVANCED_2nd_TIL**에 나열된 분량을 읽고 공부하시면 됩니다.

아래의 문제를 풀어보며 학습 내용을 점검하세요. 문제를 해결하는 과정에서 개념을 스스로 정리하고, 필요한 경우 제시된 강의를 참고하여 보완하는 것이 좋습니다.

<!-- 강의 링크는 아래와 같습니다.
https://www.youtube.com/watch?v=_JURyg_KzHE&list=PLVsNizTWUw7GCfy5RH27cQL5MeKYnl8Pm&index=7
https://www.youtube.com/watch?v=6qkPy7RfLqQ&list=PLVsNizTWUw7GCfy5RH27cQL5MeKYnl8Pm&index=8
https://www.youtube.com/watch?v=WWAFAm9op2U&list=PLVsNizTWUw7GCfy5RH27cQL5MeKYnl8Pm&index=9
-->

**교재 실습 예제 파일은 08_SQL_ADVANCED_Template 레포지토리의 src 폴더에 업로드되어 있습니다. market_db 파일도 해당 폴더에 함께 포함되어 있으니 참고하시기 바랍니다.**

**👀(수행 인증샷은 필수입니다.)** 

## SQL_ADVANCED_2nd_TIL

### 3장 SQL 기본 문법
#### 01. 기본 중에 기본 SELECT ~ FROM ~ WHERE
#### 02. 좀 더 깊게 알아보는 SELECT문
#### 03. 데이터 변경을 위한 SQL문


## Study Schedule

| 주차  | 공부 범위     | 완료 여부 |
| ----- | ------------- | --------- |
| 1주차 | p.24~99    | ✅         |
| 2주차 | p.102~155   | ✅         |
| 3주차 | p.158~213  | 🍽️         |
| 4주차 | p.216~271 | 🍽️         |
| 5주차 | p.274~327 | 🍽️         |
| 6주차 | p.330~369 | 🍽️         |
| 7주차 | p.372~407 | 🍽️         |


<br>

<!-- 여기까진 그대로 둬 주세요-->

---

# 1️⃣ 학습 내용 정리

## 1. 기본 중에 기본 SELECT ~ FROM ~ WHERE

<!-- 기본적인 SQL 문법에 관해 배우게 된 점을 적어주세요. -->
# SELECT문 정리

## SELECT문
테이블에서 데이터를 추출하는 기능

- 기본 형식:
```sql
  SELECT ~
  FROM ~
  WHERE ~
  GROUP BY ~
  HAVING ~
  ORDER BY ~
  LIMIT
```

- 데이터베이스 지정:
```sql
  USE 데이터베이스_이름; -- 현재 사용하는 데이터베이스 지정
```

## WHERE절
조회하는 결과에 특정한 조건을 추가함

- **관계 연산자**: `>`, `<`, `>=`, `<=`, `=` 등
- **논리 연산자**: `AND`, `OR`
- **BETWEEN ~ AND**: 범위에 있는 값을 구함
- **IN()**: 조건 중 하나에 포함되는 값을 구함
- **LIKE**: 문자열의 일부 글자를 검색
  - `%`: 무엇이든 허용
  - `_`: 한 글자


<!-- 과제 페이지를 참조하여 인증 사진 2장을 아래의 부분을 지우고 제출해주세요. -->

<img width="1376" height="957" alt="image" src="https://github.com/user-attachments/assets/f57dde98-1282-42f9-97df-9521add2fb5b" />
???

> **확인문제: 주소의 지역이 서울, 경기인 회원을 추출하는 SQL 문입니다. 빈칸에 들어갈 수 있는 것을 모두 고르세요.**

```sql
SELECT *
FROM table
WHERE ________;
```

보기는 아래와 같습니다.
```
1. addr IN('서울', '경기')
2. addr BETWEEN '서울' AND '경기'
3. addr = '서울' OR addr = '경기'
4. addr = '서울' AND addr = '경기'
```

```
답: 1, 3
이유:
1번: IN은 괄호 안에 나열된 값들 중 하나라도 일치하면 조건을 만족시킴.
3번: OR 논리 연산자로 두 조건 중 하나라도 참이면 만족. IN문을 풀어쓴 것과 동일한 결과.

(오답 이유)
2번: BETWEEN은 연속된 범위를 구할 때 쓰는 연산자. '서울'~'경기' 사이의 다른 값까지 포함될 수 있음.
4번: 하나의 열이 동시에 '서울'이면서 '경기'일 수 없음. 결과가 항상 없음.
```


## 2. 좀 더 깊게 알아보는 SELECT문

<!-- ORDER BY절과 GROUP BY절 그리고 HAVING절에 관해 배우게 된 점을 적어주세요. -->

```
## ORDER BY절
결과가 출력되는 순서를 조절함

- `ASC`: 오름차순 (디폴트)
- `DESC`: 내림차순

## GROUP BY절
데이터를 그룹으로 묶음

- 함께 사용되는 집계 함수:
  - `SUM()`: 합계
  - `AVG()`: 평균
  - `MIN()`: 최솟값
  - `MAX()`: 최댓값
  - `COUNT()`: 개수
  - `COUNT(DISTINCT)`: 중복을 제외한 개수

## HAVING절
집계 함수에 대해 조건을 제한하는 절

- GROUP BY 다음에 나옴.
```

> **확인문제: 다음 표는 주요 집계함수를 정리한 것입니다. 각 설명에 해당하는 올바른 함수명을 기호에 맞게 작성하세요.**

| 함수명 | 설명 |
|--------|------|
| SUM() | 합계를 구합니다. |
| (ㄱ) | 평균을 구합니다. |
| (ㄴ) | 최소값을 구합니다. |
| MAX() | 최대값을 구합니다. |
| (ㄷ) | 행의 개수를 셉니다. |
| (ㄹ) | 행의 개수를 셉니다 (중복은 1개만 인정). |

```
(ㄱ) AVG()
(ㄴ) MIN()
(ㄷ) COUNT()
(ㄹ) COUNT(DISTINCT)
```


## 3. 데이터 변경을 위한 SQL문

<!-- INSERT문, UPDATE문, DELETE문에 관해 배우게 된 점을 적어주세요. -->

| 구분 | 기본 형식 | 세부 내용 |
|---|---|---|
| **INSERT문** | `INSERT INTO 테이블 [(열1, 열2, ...)] VALUES (값1, 값2, ...)` | - 테이블에 행 데이터를 입력<br>- **AUTO_INCREMENT**: 열을 정의할 때 1부터 증가하는 값을 자동으로 입력함<br>&nbsp;&nbsp;• 해당 열은 꼭 **PK(기본 키)**로 지정해야 함<br>&nbsp;&nbsp;• 자동 증가하는 부분은 값 대신 **NULL**로 채워 넣음<br>&nbsp;&nbsp;• `@@auto_increment_increment`: AUTO_INCREMENT의 증가값을 지정하는 시스템 변수<br>- **INSERT INTO ~ SELECT**: 다른 테이블의 데이터를 가져와서 한 번에 입력함 |
| **UPDATE문** | `UPDATE 테이블 이름 SET 열1=값1, ... WHERE 조건` | - 행 단위로 기존 값을 수정함<br>- 콤마(,)로 분리해서 **여러 개의 열을 한 번에 변경** 가능<br>- ⚠️ **주의**: WHERE 절을 생략하면 테이블의 **모든 행**의 값이 변경됨 |
| **DELETE문** | `DELETE FROM 테이블 WHERE 조건` | - 행 단위로 삭제<br>- ⚠️ **주의**: WHERE 절이 없으면 **전체 행 삭제**<br>- TRUNCATE와 기능은 비슷하지만, DELETE는 조건(WHERE)을 걸어 일부 행만 삭제할 수 있다는 차이가 있음 |

## 관련 용어 

| 용어 | 설명 |
|---|---|
| NULL | 아무 것도 없는 값. AUTO_INCREMENT 열에 값을 입력할 때는 NULL로 지정함 |
| PRIMARY KEY | 기본 키. AUTO_INCREMENT 열은 기본 키로 지정해야 함 |
| ALTER TABLE | 테이블의 구조를 변형하는 SQL |
| 시스템 변수 | MySQL에서 자체적으로 가지고 있는 설정값이 저장된 변수 |
| @@auto_increment_increment | AUTO_INCREMENT의 증가값을 지정하는 시스템 변수 |
| DESCRIBE | 테이블의 구조를 확인하는 SQL |
| TRUNCATE | DELETE와 비슷한 기능이지만 전체 행을 삭제할 때 사용 |

## 공통 주의사항

- 세 문법 모두 실행 전에 `SELECT` 문으로 대상 행을 먼저 확인하는 습관을 들이면 실수를 줄일 수 있음
- WHERE 절 누락은 UPDATE/DELETE에서 가장 흔한 실수 포인트
  


# 2️⃣ 실습과제

다음 SQL 문을 작성하고 실행 결과를 확인 후 인증 사진을 아래에 업로드하세요.(market_db를 그대로 사용합니다.)

1. 모든 그룹 멤버의 정보를 조회하시오.
2. 멤버의 수가 6명 이상인 그룹 정보를 조회하시오.
3. 현재 구매 테이블에 존재하는 서로 다른 상품(prod_name)이 어떤 것이 있는지 조회하시오.
4. 총 구매 금액이 1000미만인 prod_name 중 상위 2개만 조회하시오.


<img width="827" height="675" alt="image" src="https://github.com/user-attachments/assets/7451aeda-879b-48de-93aa-e441bb4c6a39" />
<img width="830" height="661" alt="image" src="https://github.com/user-attachments/assets/2431d090-1c18-4c6c-a1ac-cd48d1ab44f8" />
<img width="845" height="652" alt="image" src="https://github.com/user-attachments/assets/c114203c-5678-4881-9597-5a7139ba397b" />
<img width="837" height="697" alt="image" src="https://github.com/user-attachments/assets/987dd396-8bf8-4913-8388-2a3db6154801" />


### 🎉 수고하셨습니다.






