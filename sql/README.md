### [1. SQL 기본](#1-sql-기본)

[관계형 데이터베이스 개요](#관계형-데이터베이스-개요) </br>
[SELECT 문](#select-문) </br>
[함수](#함수) </br>
[WHERE 절](#where-절) </br>
[GROUP BY, HAVING 절](#group-by-having-절) </br>
[ORDER BY 절](#order-by-절) </br>
[조인](#조인) </br>
[표준 조인](#표준-조인)

### [2. SQL 활용](#2-sql-활용)

[서브 쿼리](#서브-쿼리) </br>
[집합 연산자](#집합-연산자) </br>
[그룹 함수](#그룹-함수) </br>
[윈도우 함수](#윈도우-함수) </br>
[Top N 쿼리](#top-n-쿼리) </br>
[계층형 질의와 셀프 조인](#계층형-질의와-셀프-조인) </br>
[PIVOT 절과 UNPIVOT 절](#pivot-절과-unpivot-절) </br>
[정규 표현식](#정규-표현식)

### [3. 관리구문](#3-관리-구문)

[DML](#dml) </br>
[TCL](#tcl) </br>
[DDL](#ddl) </br>
[DCL](#dcl)

---

</br></br>

# 1. SQL 기본

## 관계형 데이터베이스 개요

|    명령어의 종류     |                    명령어                     |                                                                                                                                                     |
| :------------------: | :-------------------------------------------: | :-------------------------------------------------------------------------------------------------------------------------------------------------- |
|  DML(데이터 조작어)  |                    SELECT                     | 데이터베이스에 들어 있는 데이터를 조회하거나 검색하기 위한 명령어로 RETRIEVE 라고도 함                                                              |
|                      | SELECT </br> INSERT </br> UPDATE </br> DELETE | 데이터베이스의 테이블에 들어 있는 데이터에 변형을 가하는 종류의 명령어                                                                              |
|  DDL(데이터 정의어)  |  CREATE </br> ALTER </br> DROP </br> RENAME   | 테이블과 같은 데이터 구조를 정의하는데 사용되는 명령어들로 그러한 구조를 생성하고나 변경하거나 삭제하거나 이름을 바꾸는 데이터 구조와 관련된 명령어 |
|  DCL(데이터 제어어)  |              GRANT </br> REVOKE               | 데이터베이스에 접근하고 객체들을 사용하도록 권한을 주고 회수하는 명령어                                                                             |
| TCL(트랜젝션 제어어) |             COMMIT </br> ROLLBACK             | 논리적인 작업의 단위를 묶어서 DML에 의해 조작된 결과를 트랜잭션 별로 제어하는 명령어                                                                |

## SELECT 문

- WHERE 절은 필수가 아니므로 생략 가능
- ALL: Default 옵션. 중복된 데이터가 있어도 모두 출력
- DISTINCT: 중복된 데이터가 있는 경우 1건으로 처리해서 출력
- SELECT list에 서브쿼리가 사용될 수 있음

## 함수

#### ✅ 연산자의 종류

|      구분      |        연산자         |                                                     |
| :------------: | :-------------------: | :-------------------------------------------------- |
|   비교연산자   |          `=`          | 같다                                                |
|                |          `>`          | 보다 크다                                           |
|                |         `>=`          | 보다 크거나 같다                                    |
|                |          `<`          | 보다 작다                                           |
|                |         `<=`          | 보다 작거나 같다                                    |
|   SQL연산자    |   `BETWEEN a AND b`   | a와 b의 값 사이에 있으면 된다(a와 b값이 포함)       |
|                |      `IN (list)`      | 리스트에 있는 값 중에서 어느 하나라도 일치하면 된다 |
|                |  `LIKE '비교문자열'`  | 비교문자열과 형태가 일치하면된다(%, \_ 사용)        |
|                |       `IS NULL`       | NULL 값인 경우                                      |
|   논리연산자   |         `AND`         | 앞 조건과 뒤 조건을 동시에 만족                     |
|                |         `OR`          | 앞뒤 조건 중 하나라도 만족하면 됨                   |
|                |         `NOT`         | 뒤에 오는 조건에 반대되는 결과를 돌려줌             |
| 부정비교연산자 |         `!=`          | 같지 않다                                           |
|                |         `^=`          | 같지 않다                                           |
|                |         `<>`          | 같지 않다 (ISO 표준, 모든 운영체제에서 사용 가능)   |
|                |    `NOT 칼럼명 =`     | ~와 같지 않다                                       |
|                |    `NOT 칼럼명 >`     | ~보다 크지 않다                                     |
| 부정SQL연산자  | `NOT BETWEEN a AND b` | a와 b값 사이에 있지 않다(a와 b값을 포함하지 않음)   |
|                |    `NOT IN (list)`    | list 값과 일치하지 않는다                           |
|                |     `IS NOT NULL`     | NULL 값을 갖지 않는다                               |

#### ✅ 단일행 함수의 종류

|      종류      |                    내용                    |                                                                                       함수의 예                                                                                       |
| :------------: | :----------------------------------------: | :-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
|  문자형 함수   |   문자를 입력하면 문자나 숫자 값을 반환    |                                  `LOWER` </br> `UPPER` </br> `SUBSTR/SUBSTRING`</br> `LENGTH/LEN`</br> `LTRIM`</br> `RTRIM`</br> `TRIM`</br> `ASCII`                                  |
|  숫자형 함수   |       숫자를 입력하면 숫자 값을 반환       | `ABS`</br> `MOD`</br> `ROUND`</br> `TRUNC`</br> `SIGN`</br> `CHR/CHAR`</br> `CEIL/CEILING`</br> `FLOOR`</br> `EXP`</br> `LOG`</br> `LN`</br> `POWER`</br> `SIN`</br> `COS`</br> `TAN` |
|  날짜형 함수   |           DATE 타입의 값을 연산            |                                       `SYSDATE/GETDATE`</br> `EXTRACT/DATEPART`</br> `TO_NUMBER(TO_CHAR(d, 'YYYY'│'MM'│'DD'))/ YEAR│MONTH│DAY`                                        |
|  변환형 함수   | 문자, 숫자, 날짜형 값의 데이터 타입을 변환 |                                                             `TO_NUMNER`</br>`TO_CHAR`</br>`TO_DATE / CAST`</br>`CONVERT`                                                              |
| NULL 관련 함수 |         NULL을 처리하기 위한 함수          |                                                                       `NVL/ISNULL`</br>`NULLIF`</br>`COALESCE`                                                                        |

#### ✅ DUAL 테이블의 특성

- 사용자 SYS가 소유하며 모든 사용자가 액세스 가능한 테이블
- SELECT ~ FROM ~ 의 형식을 갖추기 위한 일종의 DUMMY 테이블
- DUMMY라는 문자열 유형의 칼럼에 'X'라는 값이 들어 있는 행을 1건 포함하고 있다

#### ✅ 오라클에서 날짜 연산

```sql
SELECT TO_CHAR(TO_DATE('2023.01.10 10', 'YYYY.MM.DD HH24') + 1/24/(60/10), 'YYYY.MM.DD HH24:MI:SS') FROM DUAL;
```

- 오라클에서 날짜 연산은 숫자 연산과 같음
- 1/24 = 1시간 (1일을 24시간으로 나눔)
- 1/24/60 = 1분 (1시간을 60분으로 나눔)
- 1/24/(60/10) = 10분 (1시간을 6분으로 나눔)

#### ✅ NULL의 연산

- NULL 값과의 연산(+,-,\*,/ 등)은 NULL 값을 리턴
- NULL 값과의 비교연산은 FALSE를 리턴
- 특정 값보다 크다, 적다고 표현할 수 없음
- NULL 값을 조건절에서 사용하는 경우 `IS NULL`, `IS NOT NULL` 키워드를 사용해야 함

#### ✅ 오라클, SQL Server에서 `NULL`

- 오라클에서는 ''과 같이 데이터를 입력하는 경우 해당 속성의 값은 NULL로 입력된다.
- SQL Server에서는 ''과 같이 데이터를 입력하는 경우 해당 속성의 값은 ''로 입력된다.
- 오라클, SQL Server에서 NULL 값을 조건절에서 사용하는 경우 `IS NULL`, `IS NOT NULL` 키워드를 사용해야 함
- 오라클과 SQL Server에서 `NULLIF(표현식1, 표현식2)`를 사용하여 표현식1과 표현식2와 같으면 NULL을 같지 않으면 표현식1을 리턴한다.
- 오라클에서는 `NVL(표현식1, 표현식2)` SQL Server에서는 `ISNULL(표현식1, 표현식2)`을 사용하여 표현식1의 결과값이 NULL이면 표현식 2의 값을 출력한다.
- 오라클과 SQL Server에서 `COALESCE(표현식1, 표현식2, ...)`를 사용하여 임의의 개수 표현식에서 NULL이 아닌 최초의 표현식을 나타내며 모든 표현식이 NULL이면 NULL을 리턴한다.

#### ✅ `CASE`, `DECODE`

```sql
SELECT LOC, CASE
             WHEN LOC = 'NEW YORK' THEN 'EAST'
             ELSE 'ETC'
          END AS AREA
FROM DEPT;
```

```sql
SELECT LOC,
          CASE LOC WHEN 'NEW YORK' THEN 'EAST' ELSE 'ETC' END AS AREA
FROM DEPT;
```

```sql
SELECT LOC,
          DECODE (LOC, 'NEW YORK', 'EAST', 'ETC') AS AREA
FROM DEPT;
```

## WHERE 절

- FROM 절 다음에 위치하며 조건식은 칼럼명, 비교연상자, 문자, 숫자, 표현식, 비교칼럼명으로 구성된다.
- 데이터베이스에서 조회되는 데이터에 대한 조건을 설정하여 원하는 데이터만을 검색하기 위해 사용하는 절

## GROUP BY, HAVING 절

- `GROUP BY` 절을 통해 소그룹별 기준을 정한 후, `SELECT` 절에 집계함수를 사용
- 집계 함수의 통계 정보는 NULL 값을 가진 행을 제외하고 수행
- `GROUP BY` 절에서는 `SELECT` 절과는 달리 ALIAS 명을 사용할 수 없음
- 집계 함수는 `WHERE` 절에는 올 수 없음 (`GROUP BY` 절보다 `WHERE` 절이 먼저 수행되기 때문)
- `WHERE` 절은 전체 데이터를 GROUP으로 나누기 전에 행들을 미리 제거
- `HAVING` 절은 `GROUP BY` 절의 기준 항목이나 소그룹의 집계 함수를 이용한 조건을 표시할 수 있음
- `GROUP BY` 절에 의한 소그룹별로 만들어진 집계 데이터 중 `HAVING` 절에서 제한 조건을 두어 조건을 만족하는 내용만 출력
- `HAVING` 절은 일반적으로 `GROUP BY` 절 뒤에 위치

## ORDER BY 절

- 쿼리 결과를 정렬하는 데 사용
- `ASC`, `DESC`를 사용하여 정렬 방향을 지정할 수 있다.

```sql
SELECT column1, column2
FROM table_name
ORDER BY column1 DESC;
```

- 다중 칼럼 정렬: 여러 칼럼을 기준으로 정렬 가능. 왼쪽에서 오른쪽 순으로 우선순위가 적용

```sql
SELECT column1, column2, column3
FROM table_name
ORDER BY column1 ASC, column2 DESC;
```

- 칼럼 위치로 정렬: 칼럼 이름 대신 SELECT 문에서의 위치를 사용 가능

```sql
SELECT column1, column2
FROM table_name
ORDER BY 2; -- column2로 정렬
```

- 표현식으로 정렬: 계산된 값이나 함수 결과로 정렬 가능

```sql
SELECT product_name, unit_price, units_in_stock
FROM products
ORDER BY unit_price * units_in_stock DESC;
```

- NULL값 처리: 오라클에서는 `NULLS FIRST`, `NULLS LAST` 사용, SQL Server에서는 NULL을 가장 작은 값으로 취급

```sql
-- Oracle
SELECT column1
FROM table_name
ORDER BY column1 NULLS LAST;

-- SQL Server (NULL을 마지막에 표시하려면)
SELECT column1
FROM table_name
ORDER BY CASE WHEN column1 IS NULL THEN 1 ELSE 0 END, column1;
```

- `CASE` 문을 이용한 사용자 정의 정렬

```sql
SELECT product_name, category
FROM products
ORDER BY
  CASE category
    WHEN 'Electronics' THEN 1
    WHEN 'Books' THEN 2
    WHEN 'Clothing' THEN 3
    ELSE 4
  END;
```

- `TOP/LIMIT`과 함께 사용: 정렬 후 상위 N개의 행만 가져올 때 사용

```sql
-- SQL Server
SELECT TOP 5 column1
FROM table_name
ORDER BY column1 DESC;

-- MySQL/PostgreSQL
SELECT column1
FROM table_name
ORDER BY column1 DESC
LIMIT 5;
```

- `GROUP BY`와 함께 사용: `GROUP BY` 절과 함께 사용할 때는 `GROUP BY`에 나온 열이나 집계 함수만 `ORDER BY`에서 사용 가능

```sql
SELECT category, COUNT(*) as count
FROM products
GROUP BY category
ORDER BY count DESC;
```

## 조인

- 두 개 이상의 테이블들을 연결 또는 결합하여 데이터를 출력하는 것
- 일반적인 경우 행들은 PK나 FK 값의 연관에 의해 조인이 성립
- 어떤한 경우에는 PK, FK의 관계가 없어도 논리적인 값들의 연관만으로 조인이 성립
- DBMS 옵티마이저는 FROM 절에 나열된 테이블들이 아무리 많아도 항상 2개의 테이블씩 짝을 지어 Join을 수행
- EQUI JOIN은 '=' 연산자에 의해서만 수행되며, 그 이외의 비교 연산자를 사용하는 경우에는 모두 NON EQUI JOIN이다
- 대부분 NON EQUI JOIN을 수행할 수 있지만, 때로는 설계상의 이유로 수행이 불가능한 경우도 있음

## 표준 조인

- 표준 조인은 SQL-92 표준에 정의된 조인 문법을 따름
- FROM 절에서 JOIN 키워드를 사용하여 조인 조건을 명시적으로 표현
- JOIN 키워드를 사용
- ON 절이나 USING 절로 조인 조건을 명시
- 조인 조건과 필터 조건을 명확히 구분

```sql
SELECT e.employee_name, d.department_name
FROM employees e
INNER JOIN departments d ON e.department_id = d.department_id
WHERE e.salary > 50000;
```

### ✅ 일반 조인

- 일반 조인은 SQL-92 표준 이전에 사용되던 방식으로, WHERE 절에서 조인 조건을 지정
- JOIN 키워드를 사용하지 않음
- WHERE 절에서 조인 조건과 필터 조건을 함께 명시
- 여러 테이블을 FROM 절에 콤마로 구분하여 나열

```sql
SELECT e.employee_name, d.department_name
FROM employees e, departments d
WHERE e.department_id = d.department_id
  AND e.salary > 50000;
```

### ✅ 표준 조인과 일반조인의 차이점

|      차이      |                               표준 조인                                |                                 일반 조인                                  |
| :------------: | :--------------------------------------------------------------------: | :------------------------------------------------------------------------: |
|     가독성     | 조인 의도가 명확하게 드러나며, 복잡한 다중 조인에서 특히 가독성이 좋음 |   간단한 조인에서는 간결할 수 있지만, 복잡한 조인에서는 가독성이 떨어짐    |
|   조건 분리    |            조인 조건(ON)과 필터 조건(WHERE)이 명확히 구분됨            |              조인 조건과 필터 조건이 WHERE 절에 혼재되어 있음              |
| 외부 조인 표현 |               LEFT JOIN, RIGHT JOIN 등으로 명확하게 표현               |   Oracle의 경우 (+) 연산자를 사용, 다른 DBMS에서는 지원하지 않을 수 있음   |
|   다중 조인    |              각 조인을 순차적으로 명시하여 이해하기 쉬움               | 여러 테이블을 한 번에 나열하고 WHERE 절에서 조건을 지정하여 복잡할 수 있음 |

```sql
-- 표준 조인
SELECT e.employee_name, d.department_name, l.location_name
FROM employees e
INNER JOIN departments d ON e.department_id = d.department_id
INNER JOIN locations l ON d.location_id = l.location_id
WHERE e.salary > 50000;

--  일반 조인
SELECT e.employee_name, d.department_name, l.location_name
FROM employees e, departments d, locations l
WHERE e.department_id = d.department_id
  AND d.location_id = l.location_id
  AND e.salary > 50000;
```

# 2. SQL 활용

## 서브 쿼리

## 집합 연산자

## 그룹 함수

## 윈도우 함수

## Top N 쿼리

## 계층형 질의와 셀프 조인

## PIVOT 절과 UNPIVOT 절

## 정규 표현식

# 3. 관리 구문

## DML

## TCL

## DDL

## DCL
