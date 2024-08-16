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

    서브쿼리는 다른 SQL문 내부에 포함된 쿼리이다.
    중첩쿼리(Nested Query) 또는 내부쿼리(Inner Query)라고도 한다.

### ✅ 서브 쿼리의 유형

|       분류 기준       |        유형        |                   설명                    |
| :-------------------: | :----------------: | :---------------------------------------: |
| 반환되는 행과 열의 수 |  단일 행 서브쿼리  |             하나의 행만 반환              |
|                       |  다중 행 서브쿼리  |              여러 행을 반환               |
|                       | 단일 칼럼 서브쿼리 |            하나의 칼럼만 반환             |
|                       | 다중 칼럼 서브쿼리 |             여러 칼럼을 반환              |
|       사용 위치       |  스칼라 서브쿼리   |    SELECT 문에서 사용되어 단일 값 반환    |
|                       |     인라인 뷰      | FROM 절에서 사용되어 가상의 테이블을 생성 |
|                       |   중첩 서브쿼리    |      WHERE 절이나 HAVING 절에서 사용      |

## 집합 연산자

    집합 연산자는 두 개 이상의 쿼리 결과를 하나로 결합하는 데 사용된다.
    주로 사용되는 집합 연산:  UNION, UNION ALL, INTERSECT, MINUS(또는 EXCEPT)

### ✅ 집합 연산자의 종류와 특징

|     연산자     | 설명                                                | 중복 제거 | Oracle | SQL Server |
| :------------: | :-------------------------------------------------- | :-------: | :----: | :--------: |
|     UNION      | 두 쿼리 결과를 합치고 중복을 제거                   |     O     |   O    |     O      |
|   UNION ALL    | 두 쿼리 결과를 합치고 중복을 유지                   |     X     |   O    |     O      |
|   INTERSECT    | 두 쿼리 결과의 교집합                               |     O     |   O    |     O      |
| MINUS / EXCEPT | 첫 번째 쿼리 결과에서 두 번째 쿼리 결과를 뺀 차집합 |     O     | MINUS  |   EXCEPT   |

### ✅ 집합 연산자 사용 시 주의사항

1. 열의 개수와 데이터 타입: 연산 대상이 되는 쿼리들은 같은 수의 열을 반환해야 하며, 대응되는 열의 데이터 타입이 호환 가능해야 함
2. ORDER BY: 전체 결과에 대한 정렬은 마지막에 한 번만 사용 가능
3. 열 이름: 결과 집합의 열 이름은 첫 번째 쿼리의 열 이름을 따른다

```sql
-- UNION (Oracle/SQL Server 동일)
SELECT employee_id FROM current_employees
UNION
SELECT employee_id FROM former_employees;

-- INTERSECT (Oracle/SQL Server 동일)
SELECT product_id FROM products
INTERSECT
SELECT product_id FROM order_items;

-- MINUS (Oracle)
SELECT customer_id FROM all_customers
MINUS
SELECT customer_id FROM active_customers;

-- EXCEPT (SQL Server)
SELECT customer_id FROM all_customers
EXCEPT
SELECT customer_id FROM active_customers;
```

## 그룹 함수

    그룹 함수는 데이터 집합에 대해 계산을 수행하고 단일 결과를 반환하는 함수로 주로 GROUP BY 절과 함께 사용되어 데이터를 요약하고 분석하는 데 활용

### ✅ 주요 그룹 함수

1. COUNT()
2. SUM()
3. AVG()
4. MAX()
5. MIN()
6. STDDEV() (표준편차)
7. VARIANCE() (분산)

### ✅ 그룹 함수 사용 예시

| EmpID | EmpName | Department | Salary |
| ----- | ------- | ---------- | ------ |
| 1     | Alice   | IT         | 60000  |
| 2     | Bob     | HR         | 55000  |
| 3     | Charlie | IT         | 65000  |
| 4     | David   | Finance    | 70000  |
| 5     | Eve     | HR         | 50000  |

### 1. COUNT(): 행의 개수를 세는 함수

```sql
SELECT COUNT(*) as TotalEmployees FROM Employees;
SELECT Department, COUNT(*) as EmployeeCount
FROM Employees
GROUP BY Department;
```

결과:

```
TotalEmployees: 5

Department | EmployeeCount
-----------|---------------
IT         | 2
HR         | 2
Finance    | 1
```

### 2. SUM(): 숫자 열의 합계를 계산

```sql
SELECT SUM(Salary) as TotalSalary FROM Employees;
SELECT Department, SUM(Salary) as DepartmentSalary
FROM Employees
GROUP BY Department;
```

결과:

```
TotalSalary: 300000

Department | DepartmentSalary
-----------|------------------
IT         | 125000
HR         | 105000
Finance    | 70000
```

### 3. AVG(): 숫자 열의 평균을 계산

```sql
SELECT AVG(Salary) as AverageSalary FROM Employees;
SELECT Department, AVG(Salary) as AverageSalary
FROM Employees
GROUP BY Department;
```

결과:

```
AverageSalary: 60000

Department | AverageSalary
-----------|---------------
IT         | 62500
HR         | 52500
Finance    | 70000
```

### 4. MAX() 및 5. MIN(): 최대값과 최소값

```sql
SELECT MAX(Salary) as HighestSalary, MIN(Salary) as LowestSalary
FROM Employees;

SELECT Department, MAX(Salary) as HighestSalary, MIN(Salary) as LowestSalary
FROM Employees
GROUP BY Department;
```

결과:

```
HighestSalary: 70000, LowestSalary: 50000

Department | HighestSalary | LowestSalary
-----------|---------------|-------------
IT         | 65000         | 60000
HR         | 55000         | 50000
Finance    | 70000         | 70000
```

### 6. STDDEV() 및 7. VARIANCE(): 표준편차와 분산을 계산

Oracle:

```sql
SELECT STDDEV(Salary) as SalaryStdDev, VARIANCE(Salary) as SalaryVariance
FROM Employees;
```

SQL Server:

```sql
SELECT STDEV(Salary) as SalaryStdDev, VAR(Salary) as SalaryVariance
FROM Employees;
```

결과 (근사값):

```
SalaryStdDev: 7905.69, SalaryVariance: 62500000
```

### ✅ 그룹 함수 사용 시 주의사항

1. NULL 값 처리: 그룹 함수는 기본적으로 NULL 값을 무시햔다. COUNT(\*)를 제외한 모든 그룹 함수는 NULL 값을 계산에서 제외

2. DISTINCT 키워드: COUNT, SUM, AVG 함수에서 DISTINCT 키워드를 사용하여 중복 값을 제거할 수 있다
   예: `SELECT COUNT(DISTINCT Department) FROM Employees;`

3. GROUP BY 사용: 그룹 함수를 사용할 때, SELECT 문에 그룹 함수가 아닌 열이 포함되어 있다면 반드시 GROUP BY 절에 해당 열을 명시해야 함

4. HAVING 절: 그룹화된 결과에 조건을 적용하려면 WHERE 대신 HAVING 절을 사용
   예: `SELECT Department, AVG(Salary) FROM Employees GROUP BY Department HAVING AVG(Salary) > 60000;`

5. 중첩 그룹 함수: 대부분의 데이터베이스 시스템에서 그룹 함수를 중첩해서 사용할 수 없다. (예: AVG(SUM(Salary)))

## 윈도우 함수

    윈도우 함수는 행들의 집합(윈도우)에 대해 계산을 수행하는 함수이다. 이 함수들은 결과 집합의 각 행마다 계산을 수행하며, ORDER BY를 사용해 정렬된 결과에 대해 누적 계산도 가능하다.

### ✅ 윈도우 함수의 주요 특징

1. 결과 집합의 행 수를 유지한다.
2. 집계 함수와 달리 결과를 단일 행으로 줄이지 않는다.
3. OVER 절을 사용하여 윈도우를 정의한다.
4. 순위, 비율, 이동 평균 등 다양한 분석 작업에 사용된다.

### ✅ 주요 윈도우 함수 유형

1. 순위 함수: ROW_NUMBER(), RANK(), DENSE_RANK()
2. 오프셋 함수: LAG(), LEAD()
3. 집계 함수: SUM(), AVG(), COUNT(), MAX(), MIN()
4. 분석 함수: FIRST_VALUE(), LAST_VALUE(), NTH_VALUE()

### ✅ 윈도우 함수 사용 예시

| SalesID | Salesperson | Amount | SaleDate   |
| ------- | ----------- | ------ | ---------- |
| 1       | Alice       | 5000   | 2023-01-15 |
| 2       | Bob         | 6000   | 2023-01-20 |
| 3       | Charlie     | 4500   | 2023-02-05 |
| 4       | Alice       | 5500   | 2023-02-10 |
| 5       | Bob         | 6500   | 2023-03-01 |
| 6       | Charlie     | 5000   | 2023-03-15 |

### 1. 순위 함수: ROW_NUMBER(), RANK(), DENSE_RANK()

```sql
SELECT
    Salesperson,
    Amount,
    ROW_NUMBER() OVER (ORDER BY Amount DESC) as RowNum,
    RANK() OVER (ORDER BY Amount DESC) as Rank,
    DENSE_RANK() OVER (ORDER BY Amount DESC) as DenseRank
FROM Sales;
```

이 쿼리는 판매 금액에 따른 순위를 매긴다. ROW_NUMBER()는 고유한 순위를, RANK()는 동일 값에 대해 같은 순위를 부여하고 다음 순위를 건너뛰며, DENSE_RANK()는 동일 값에 대해 같은 순위를 부여하되 다음 순위를 건너뛰지 않는다.

결과:

| Salesperson | Amount | RowNum | Rank | DenseRank |
| ----------- | ------ | ------ | ---- | --------- |
| Bob         | 6500   | 1      | 1    | 1         |
| Bob         | 6000   | 2      | 2    | 2         |
| Alice       | 5500   | 3      | 3    | 3         |
| Alice       | 5000   | 4      | 4    | 4         |
| Charlie     | 5000   | 5      | 4    | 4         |
| Charlie     | 4500   | 6      | 6    | 5         |

### 2. 오프셋 함수: LAG(), LEAD()

```sql
SELECT
    Salesperson,
    SaleDate,
    Amount,
    LAG(Amount) OVER (PARTITION BY Salesperson ORDER BY SaleDate) as PrevAmount,
    LEAD(Amount) OVER (PARTITION BY Salesperson ORDER BY SaleDate) as NextAmount
FROM Sales;
```

이 쿼리는 각 판매원별로 이전 판매 금액과 다음 판매 금액을 보여준다.

결과:

| Salesperson | SaleDate   | Amount | PrevAmount | NextAmount |
| ----------- | ---------- | ------ | ---------- | ---------- |
| Alice       | 2023-01-15 | 5000   | NULL       | 5500       |
| Alice       | 2023-02-10 | 5500   | 5000       | NULL       |
| Bob         | 2023-01-20 | 6000   | NULL       | 6500       |
| Bob         | 2023-03-01 | 6500   | 6000       | NULL       |
| Charlie     | 2023-02-05 | 4500   | NULL       | 5000       |
| Charlie     | 2023-03-15 | 5000   | 4500       | NULL       |

### 3. 집계 함수: SUM(), AVG(), COUNT(), MAX(), MIN()

```sql
SELECT
    Salesperson,
    SaleDate,
    Amount,
    SUM(Amount) OVER (ORDER BY SaleDate) as CumulativeSum,
    AVG(Amount) OVER (ORDER BY SaleDate ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING) as MovingAvg
FROM Sales;
```

이 쿼리는 날짜순으로 누적 합계와 3일 이동 평균(현재 행, 이전 행, 다음 행의 평균)을 계산한다.

결과:

| Salesperson | SaleDate   | Amount | CumulativeSum | MovingAvg |
| ----------- | ---------- | ------ | ------------- | --------- |
| Alice       | 2023-01-15 | 5000   | 5000          | 5500      |
| Bob         | 2023-01-20 | 6000   | 11000         | 5166.67   |
| Charlie     | 2023-02-05 | 4500   | 15500         | 5333.33   |
| Alice       | 2023-02-10 | 5500   | 21000         | 5500      |
| Bob         | 2023-03-01 | 6500   | 27500         | 5666.67   |
| Charlie     | 2023-03-15 | 5000   | 32500         | 5750      |

### 4. 분석 함수: FIRST_VALUE(), LAST_VALUE(), NTH_VALUE()

```sql
SELECT
    Salesperson,
    SaleDate,
    Amount,
    FIRST_VALUE(Amount) OVER (PARTITION BY Salesperson ORDER BY SaleDate) as FirstSale,
    LAST_VALUE(Amount) OVER (PARTITION BY Salesperson ORDER BY SaleDate
                             ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) as LastSale
FROM Sales;
```

이 쿼리는 각 판매원의 첫 판매 금액과 마지막 판매 금액을 보여준다.

결과:

| Salesperson | SaleDate   | Amount | FirstSale | LastSale |
| ----------- | ---------- | ------ | --------- | -------- |
| Alice       | 2023-01-15 | 5000   | 5000      | 5500     |
| Alice       | 2023-02-10 | 5500   | 5000      | 5500     |
| Bob         | 2023-01-20 | 6000   | 6000      | 6500     |
| Bob         | 2023-03-01 | 6500   | 6000      | 6500     |
| Charlie     | 2023-02-05 | 4500   | 4500      | 5000     |
| Charlie     | 2023-03-15 | 5000   | 4500      | 5000     |

### ✅ 윈도우 함수 사용 시 주의사항

1. 윈도우 함수는 SELECT 문과 ORDER BY 절에서만 사용할 수 있다.
2. WHERE 절에서는 사용할 수 없다.
3. 윈도우 함수 결과를 다른 윈도우 함수의 입력으로 직접 사용할 수 없다.
4. OVER 절의 구문은 데이터베이스 시스템에 따라 약간의 차이가 있을 수 있다.

### ✅ 윈도우 절 (PARTITION BY)

용도: 데이터를 파티션으로 나누어 윈도우 함수 적용

예시 테이블 (Sales):

| SalesID | Salesperson | Amount | Year |
| ------- | ----------- | ------ | ---- |
| 1       | Alice       | 5000   | 2022 |
| 2       | Bob         | 6000   | 2022 |
| 3       | Charlie     | 4500   | 2022 |
| 4       | Alice       | 5500   | 2023 |
| 5       | Bob         | 6500   | 2023 |
| 6       | Charlie     | 5000   | 2023 |

쿼리:

```sql
SELECT
    Salesperson,
    Year,
    Amount,
    SUM(Amount) OVER (PARTITION BY Year) as YearlyTotal,
    Amount / SUM(Amount) OVER (PARTITION BY Year) * 100 as PercentOfYearlyTotal
FROM Sales;
```

이 쿼리는 각 연도별로 총 판매액과 각 판매원의 판매액 비율을 계산한다.

결과:

| Salesperson | Year | Amount | YearlyTotal | PercentOfYearlyTotal |
| ----------- | ---- | ------ | ----------- | -------------------- |
| Alice       | 2022 | 5000   | 15500       | 32.26                |
| Bob         | 2022 | 6000   | 15500       | 38.71                |
| Charlie     | 2022 | 4500   | 15500       | 29.03                |
| Alice       | 2023 | 5500   | 17000       | 32.35                |
| Bob         | 2023 | 6500   | 17000       | 38.24                |
| Charlie     | 2023 | 5000   | 17000       | 29.41                |

## Top N 쿼리

    조건을 만족하는 상위 N개의 결과만을 반환하는 쿼리
    이는 대용량 데이터에서 특정 조건에 따라 상위 몇 개의 레코드만 필요할 때 유용하게 사용

### ✅ 사용 목적

1. 성능 최적화: 전체 결과 대신 일부만 반환하여 처리 시간과 리소스를 절약
2. 데이터 분석: 최상위 또는 최하위 데이터를 빠르게 확인
3. 리포트 생성: 예를 들어, 매출 상위 10개 제품과 같은 보고서를 쉽게 생성

### ✅ 데이터베이스별 구현 방법

### Oracle

Oracle에서는 `ROWNUM`을 사용하여 Top N 쿼리를 구현합니다.

```sql
SELECT * FROM (
    SELECT column1, column2
    FROM table_name
    ORDER BY column1 DESC
)
WHERE ROWNUM <= N;
```

### SQL Server

SQL Server에서는 `TOP` 키워드를 사용합니다.

```sql
SELECT TOP N column1, column2
FROM table_name
ORDER BY column1 DESC;
```

### ✅ 예시

| SalesID | ProductName | Amount |
| ------- | ----------- | ------ |
| 1       | Product A   | 5000   |
| 2       | Product B   | 6000   |
| 3       | Product C   | 4500   |
| 4       | Product D   | 5500   |
| 5       | Product E   | 6500   |
| 6       | Product F   | 5000   |

### Oracle

판매 금액 기준 상위 3개 제품을 조회하는 쿼리:

```sql
SELECT * FROM (
    SELECT ProductName, Amount
    FROM Sales
    ORDER BY Amount DESC
)
WHERE ROWNUM <= 3;
```

결과:

| ProductName | Amount |
| ----------- | ------ |
| Product E   | 6500   |
| Product B   | 6000   |
| Product D   | 5500   |

### SQL Server

같은 쿼리를 SQL Server에서 실행:

```sql
SELECT TOP 3 ProductName, Amount
FROM Sales
ORDER BY Amount DESC;
```

결과:

| ProductName | Amount |
| ----------- | ------ |
| Product E   | 6500   |
| Product B   | 6000   |
| Product D   | 5500   |

### ✅ Top N 쿼리 사용 시 주의사항

1. 정렬 순서: Top N 쿼리에서는 ORDER BY 절이 중요
2. 동점 처리: 같은 값이 여러 개 있을 경우, 데이터베이스 시스템에 따라 결과가 달라진다. SQL Server에서는 `WITH TIES` 옵션을 사용하여 동점을 포함할 수 있다.

   ```sql
   SELECT TOP 3 WITH TIES ProductName, Amount
   FROM Sales
   ORDER BY Amount DESC;
   ```

3. 성능: 대용량 데이터에서 Top N 쿼리를 사용할 때는 적절한 인덱스를 사용하여 성능을 최적화해야 한다.

4. 페이징: Top N 쿼리는 페이징 구현의 기초가 되며, OFFSET-FETCH (SQL Server) 또는 OFFSET-LIMIT (Oracle 12c 이상) 구문과 함께 사용될 수 있다.

## 계층형 질의와 셀프 조인

    데이터베이스에서 계층적 관계를 가진 데이터를 조회하는 방법으로 주로 조직도, 제품 카테고리, 파일 시스템 구조 등을 표현할 때 사용한다.

### ✅ 데이터베이스별 구현 방법

### Oracle

Oracle에서는 `START WITH`와 `CONNECT BY` 절을 사용하여 계층형 질의를 구현한다.

예시 테이블 (Employees):

| EmpID | EmpName | ManagerID |
| ----- | ------- | --------- |
| 1     | Alice   | NULL      |
| 2     | Bob     | 1         |
| 3     | Charlie | 1         |
| 4     | David   | 2         |
| 5     | Eve     | 2         |

```sql
SELECT LEVEL, EmpID, EmpName, ManagerID
FROM Employees
START WITH ManagerID IS NULL
CONNECT BY PRIOR EmpID = ManagerID;
```

시작점 (LEVEL 1):Alice(EmpID 1)가 ManagerID가 NULL이므로 최상위 레벨로 시작한다.

두 번째 레벨 (LEVEL 2):Bob(EmpID 2)과 Charlie(EmpID 3)는 Alice의 직속 부하이므로 다음 레벨에 위치한다.

세 번째 레벨 (LEVEL 3):David(EmpID 4)와 Eve(EmpID 5)는 Bob의 직속 부하이므로 그 다음 레벨에 위치한다.

순서: Oracle의 CONNECT BY는 깊이 우선 탐색을 수행한다. 따라서 Alice -> Bob -> David -> Eve -> Charlie 순으로 결과가 나온다.

LEVEL 열:각 행의 LEVEL 값은 해당 직원이 계층 구조에서 몇 번째 깊이에 있는지를 나타냅니다.

결과:

| LEVEL | EmpID | EmpName | ManagerID |
| ----- | ----- | ------- | --------- |
| 1     | 1     | Alice   | NULL      |
| 2     | 2     | Bob     | 1         |
| 3     | 4     | David   | 2         |
| 3     | 5     | Eve     | 2         |
| 2     | 3     | Charlie | 1         |

### SQL Server

SQL Server에서는 CTE(Common Table Expression)와 재귀 쿼리를 사용하여 계층형 질의를 구현한다.

쿼리:

```sql
WITH EmployeeHierarchy AS (
    SELECT EmpID, EmpName, ManagerID, 1 AS LEVEL
    FROM Employees
    WHERE ManagerID IS NULL
    UNION ALL
    SELECT e.EmpID, e.EmpName, e.ManagerID, eh.LEVEL + 1
    FROM Employees e
    INNER JOIN EmployeeHierarchy eh ON e.ManagerID = eh.EmpID
)
SELECT * FROM EmployeeHierarchy;
```

결과:

| LEVEL | EmpID | EmpName | ManagerID |
| ----- | ----- | ------- | --------- |
| 1     | 1     | Alice   | NULL      |
| 2     | 2     | Bob     | 1         |
| 3     | 4     | David   | 2         |
| 3     | 5     | Eve     | 2         |
| 2     | 3     | Charlie | 1         |

## PIVOT 절과 UNPIVOT 절

    PIVOT 절은 행 데이터를 열 데이터로 변환하는 SQL 연산이다.
    UNPIVOT 절은 열 데이터를 행 데이터로 변환한다.

### ✅ 데이터베이스별 구현 방법

### SQL Server PIVOT 쿼리

예시 테이블 (Sales):

| SalesDate  | Product | Amount |
| ---------- | ------- | ------ |
| 2023-01-01 | A       | 1000   |
| 2023-01-01 | B       | 1500   |
| 2023-01-02 | A       | 2000   |
| 2023-01-02 | B       | 2500   |

```sql
SELECT SalesDate, A, B
FROM (
    SELECT SalesDate, Product, Amount
    FROM Sales
) AS SourceTable
PIVOT (
    SUM(Amount)
    FOR Product IN (A, B)
) AS PivotTable;
```

1. PIVOT 연산은 Product 열의 값(A와 B)을 새로운 열로 변환한다.
2. Amount 값은 SUM 함수로 집계되어 각 Product 열 아래에 표시된다.
3. SalesDate별로 각 Product의 판매 금액이 열로 펼쳐져 표시된다.

| SalesDate  | A    | B    |
| ---------- | ---- | ---- |
| 2023-01-01 | 1000 | 1500 |
| 2023-01-02 | 2000 | 2500 |

### SQL Server UNPIVOT 쿼리

예시 테이블 (ProductSales):

| SalesDate  | A    | B    |
| ---------- | ---- | ---- |
| 2023-01-01 | 1000 | 1500 |
| 2023-01-02 | 2000 | 2500 |

```sql
SELECT SalesDate, Product, Amount
FROM ProductSales
UNPIVOT (
    Amount
    FOR Product IN (A, B)
) AS UnpivotTable;
```

1. UNPIVOT 연산은 A와 B 열을 Product라는 새로운 열로 변환한다.
2. 각 제품의 판매 금액은 Amount 열에 표시된다.
3. 원래 테이블의 각 행은 제품 수만큼의 새로운 행으로 확장된다.

| SalesDate  | Product | Amount |
| ---------- | ------- | ------ |
| 2023-01-01 | A       | 1000   |
| 2023-01-01 | B       | 1500   |
| 2023-01-02 | A       | 2000   |
| 2023-01-02 | B       | 2500   |

### Oracle PIVOT

Oracle 11g 이상에서는 SQL Server와 유사한 PIVOT/UNPIVOT 구문을 지원한다. 이전 버전에서는 CASE 문과 집계 함수를 사용하여 유사한 결과를 얻을 수 있다.

```sql
SELECT *
FROM (
    SELECT SalesDate, Product, Amount
    FROM Sales
)
PIVOT (
    SUM(Amount)
    FOR Product IN ('A' AS A, 'B' AS B)
);
```

결과는 SQL Server의 PIVOT 결과와 동일하다.

## 정규 표현식

    정규 표현식(Regular Expression, 줄여서 Regex)은 문자열의 패턴을 정의하는 특별한 문자열이다. 이는 문자열 검색, 유효성 검사, 데이터 추출 등 다양한 작업에 사용된다.

### ✅ 기본 문법

1. `.` : 임의의 한 문자와 일치
2. `*` : 앞의 패턴이 0회 이상 반복
3. `+` : 앞의 패턴이 1회 이상 반복
4. `?` : 앞의 패턴이 0회 또는 1회 등장
5. `^` : 문자열의 시작
6. `$` : 문자열의 끝
7. `[]` : 문자 집합. 예: [a-z]는 모든 소문자
8. `[^]` : 부정 문자 집합. 예: [^0-9]는 숫자가 아닌 문자
9. `\d` : 숫자와 일치
10. `\w` : 단어 문자(알파벳, 숫자, 밑줄)와 일치

### ✅ 정규 표현식 사용 예시

### Oracle

Oracle에서는 REGEXP_LIKE, REGEXP_INSTR, REGEXP_SUBSTR, REGEXP_REPLACE 함수를 사용하여 정규 표현식을 다룬다.

예시 테이블 (Customers):

| CustomerID | Email              |
| ---------- | ------------------ |
| 1          | john@example.com   |
| 2          | jane@test.co.uk    |
| 3          | invalid-email      |
| 4          | bob123@company.org |

#### REGEXP_LIKE 사용 예:

```sql
SELECT * FROM Customers
WHERE REGEXP_LIKE(Email, '^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}$');
```

결과:

| CustomerID | Email              |
| ---------- | ------------------ |
| 1          | john@example.com   |
| 2          | jane@test.co.uk    |
| 4          | bob123@company.org |

#### REGEXP_REPLACE 사용 예:

```sql
SELECT
    Email,
    REGEXP_REPLACE(Email, '(.*)@(.*)', '\1@masked.com') as MaskedEmail
FROM Customers;
```

결과:

| Email              | MaskedEmail       |
| ------------------ | ----------------- |
| john@example.com   | john@masked.com   |
| jane@test.co.uk    | jane@masked.com   |
| invalid-email      | invalid-email     |
| bob123@company.org | bob123@masked.com |

### SQL Server

SQL Server 2016 이상에서는 LIKE 절에서 정규 표현식과 유사한 패턴 매칭을 지원한다. 완전한 정규 표현식 지원을 위해서는 CLR(Common Language Runtime) 함수를 사용해야 한다.

#### LIKE를 사용한 패턴 매칭 예:

```sql
SELECT * FROM Customers
WHERE Email LIKE '%@%.%'
  AND Email NOT LIKE '%@%@%'
  AND Email LIKE '%_@__%.__%';
```

### ✅ 정규 표현식 사용 시 주의사항

1. 성능: 복잡한 정규 표현식은 성능에 영향을 줄 수 있다. 대량의 데이터를 처리할 때는 주의가 필요하다.
2. 가독성: 복잡한 정규 표현식은 가독성이 떨어질 수 있다. 주석을 달아 설명하는 것이 좋다.
3. 데이터베이스 호환성: 정규 표현식 지원은 데이터베이스 시스템마다 다르다. 이식성을 고려해야 한다.

<br><br><br><br>
