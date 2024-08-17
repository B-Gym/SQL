# SQL 종합 가이드 목차

1. [SQL 집합 연산자 가이드](#sql-집합-연산자-가이드)

   - [UNION과 UNION ALL](#1-union과-union-all)
   - [INTERSECT](#2-intersect)
   - [EXCEPT (SQL Server) / MINUS (Oracle)](#3-except-sql-server--minus-oracle)

2. [SQL 서브쿼리 연산자 가이드](#sql-서브쿼리-연산자-가이드)

   - [EXISTS와 NOT EXISTS](#1-exists와-not-exists)
   - [IN](#2-in)
   - [BETWEEN AND](#3-between-and)

3. [SQL 고급 그룹화 함수 가이드](#sql-고급-그룹화-함수-가이드)

   - [GROUPING SETS](#1-grouping-sets)
   - [ROLLUP](#2-rollup)
   - [CUBE](#3-cube)
   - [GROUPING 함수](#4-grouping-함수)

4. [SQL 윈도우 함수 가이드](#sql-윈도우-함수-가이드)

   - [OVER 절](#1-over-절)
   - [순위 함수](#2-순위-함수)
   - [오프셋 함수](#3-오프셋-함수)
   - [집계 함수](#4-집계-함수)
   - [분석 함수](#5-분석-함수)
   - [RATIO_TO_REPORT](#6-ratio_to_report)
   - [GROUPING](#7-grouping)
   - [CASE WHEN](#8-case-when)

5. [SQL 계층형 쿼리 가이드](#sql-계층형-쿼리-가이드)

   - [Oracle의 계층형 쿼리](#1-oracle의-계층형-쿼리)
   - [SQL Server의 재귀적 CTE](#2-sql-server의-재귀적-cte)

6. [SQL 조인 가이드](#sql-조인-가이드)

   - [INNER JOIN](#1-inner-join)
   - [LEFT OUTER JOIN](#2-left-outer-join)
   - [RIGHT OUTER JOIN](#3-right-outer-join)
   - [FULL OUTER JOIN](#4-full-outer-join)
   - [CROSS JOIN](#5-cross-join)
   - [SELF JOIN](#6-self-join)
   - [NATURAL JOIN](#7-natural-join)
   - [USING 절](#8-using-절)

7. [SQL 추가 함수 가이드](#sql-추가-함수-가이드)
   - [ROUND](#1-round)
   - [PERCENT_RANK](#2-percent_rank)
   - [CUME_DIST](#3-cume_dist)
   - [NTILE](#4-ntile)
   - [GROUPING](#5-grouping)

---

</br></br></br></br>

# SQL 집합 연산자 가이드

## 1. UNION과 UNION ALL

### 문법:

```sql
SELECT column1, column2, ... FROM table1
UNION [ALL]
SELECT column1, column2, ... FROM table2;
```

### Oracle과 SQL Server 예시:

예시 테이블 (Employees1):
| EmpID | EmpName | Department |
|-------|---------|------------|
| 1 | Alice | IT |
| 2 | Bob | HR |
| 3 | Charlie | IT |

예시 테이블 (Employees2):
| EmpID | EmpName | Department |
|-------|---------|------------|
| 3 | Charlie | IT |
| 4 | David | Finance |
| 5 | Eve | HR |

UNION 예시:

```sql
SELECT EmpID, EmpName, Department FROM Employees1
UNION
SELECT EmpID, EmpName, Department FROM Employees2;
```

결과:
| EmpID | EmpName | Department |
|-------|---------|------------|
| 1 | Alice | IT |
| 2 | Bob | HR |
| 3 | Charlie | IT |
| 4 | David | Finance |
| 5 | Eve | HR |

UNION ALL 예시:

```sql
SELECT EmpID, EmpName, Department FROM Employees1
UNION ALL
SELECT EmpID, EmpName, Department FROM Employees2;
```

결과:
| EmpID | EmpName | Department |
|-------|---------|------------|
| 1 | Alice | IT |
| 2 | Bob | HR |
| 3 | Charlie | IT |
| 3 | Charlie | IT |
| 4 | David | Finance |
| 5 | Eve | HR |

### 설명:

- UNION은 중복을 제거하고 결과를 반환한다.
- UNION ALL은 중복을 포함하여 모든 결과를 반환한다.
- 열의 개수와 데이터 타입이 일치해야 한다.
- ORDER BY는 전체 결과에 대해 마지막에 한 번만 사용할 수 있다.

## 2. INTERSECT

### 문법:

```sql
SELECT column1, column2, ... FROM table1
INTERSECT
SELECT column1, column2, ... FROM table2;
```

### Oracle과 SQL Server 예시:

```sql
SELECT EmpID, EmpName, Department FROM Employees1
INTERSECT
SELECT EmpID, EmpName, Department FROM Employees2;
```

결과:
| EmpID | EmpName | Department |
|-------|---------|------------|
| 3 | Charlie | IT |

### 설명:

- 두 쿼리 결과의 교집합을 반환한다.
- 중복은 자동으로 제거된다.
- SQL Server 2008 이상에서 지원된다.

## 3. EXCEPT (SQL Server) / MINUS (Oracle)

### 문법 (SQL Server):

```sql
SELECT column1, column2, ... FROM table1
EXCEPT
SELECT column1, column2, ... FROM table2;
```

### 문법 (Oracle):

```sql
SELECT column1, column2, ... FROM table1
MINUS
SELECT column1, column2, ... FROM table2;
```

### 예시:

SQL Server:

```sql
SELECT EmpID, EmpName, Department FROM Employees1
EXCEPT
SELECT EmpID, EmpName, Department FROM Employees2;
```

Oracle:

```sql
SELECT EmpID, EmpName, Department FROM Employees1
MINUS
SELECT EmpID, EmpName, Department FROM Employees2;
```

결과:
| EmpID | EmpName | Department |
|-------|---------|------------|
| 1 | Alice | IT |
| 2 | Bob | HR |

### 설명:

- 첫 번째 쿼리 결과에서 두 번째 쿼리 결과를 뺀 차집합을 반환한다.
- 중복은 자동으로 제거된다.
- Oracle은 MINUS를, SQL Server는 EXCEPT를 사용한다.

## 주의사항:

1. 모든 집합 연산자는 열의 개수와 데이터 타입이 일치해야 한다.
2. NULL 값 처리: UNION과 INTERSECT에서 NULL은 NULL과 같다고 간주된다.
3. 정렬: 결과를 정렬하려면 마지막에 ORDER BY를 사용해야 한다.
4. 성능: 대량의 데이터를 처리할 때는 UNION ALL이 UNION보다 일반적으로 더 빠르다.

집합 연산자는 복잡한 쿼리를 단순화하고 여러 테이블의 데이터를 효과적으로 결합하는 데 유용하다. 그러나 대용량 데이터 처리 시 성능에 주의해야 한다.

---

</br></br></br>

# SQL 서브쿼리 연산자 가이드

## 1. EXISTS와 NOT EXISTS

### 문법:

```sql
SELECT column1, column2, ...
FROM table1
WHERE [NOT] EXISTS (subquery);
```

### Oracle과 SQL Server 예시:

예시 테이블 (Employees):
| EmpID | EmpName | DepartmentID |
|-------|---------|--------------|
| 1 | Alice | 10 |
| 2 | Bob | 20 |
| 3 | Charlie | 10 |
| 4 | David | 30 |

예시 테이블 (Departments):
| DeptID | DeptName |
|--------|----------|
| 10 | IT |
| 20 | HR |
| 30 | Finance |

EXISTS 예시:

```sql
SELECT DeptName
FROM Departments d
WHERE EXISTS (
    SELECT 1
    FROM Employees e
    WHERE e.DepartmentID = d.DeptID
);
```

결과:
| DeptName |
|----------|
| IT |
| HR |
| Finance |

NOT EXISTS 예시:

```sql
SELECT DeptName
FROM Departments d
WHERE NOT EXISTS (
    SELECT 1
    FROM Employees e
    WHERE e.DepartmentID = d.DeptID
);
```

결과: (빈 결과 set)

### 설명:

- EXISTS는 서브쿼리가 하나 이상의 행을 반환하면 TRUE를 반환한다.
- NOT EXISTS는 서브쿼리가 어떤 행도 반환하지 않으면 TRUE를 반환한다.
- 성능상 EXISTS는 일반적으로 IN보다 빠르다.

## 2. IN

### 문법:

```sql
SELECT column1, column2, ...
FROM table1
WHERE column_name IN (value1, value2, ...) | (subquery);
```

### Oracle과 SQL Server 예시:

값 목록 사용:

```sql
SELECT EmpName, DepartmentID
FROM Employees
WHERE DepartmentID IN (10, 20);
```

결과:
| EmpName | DepartmentID |
|---------|--------------|
| Alice | 10 |
| Bob | 20 |
| Charlie | 10 |

서브쿼리 사용:

```sql
SELECT EmpName, DepartmentID
FROM Employees
WHERE DepartmentID IN (
    SELECT DeptID
    FROM Departments
    WHERE DeptName IN ('IT', 'HR')
);
```

결과:
| EmpName | DepartmentID |
|---------|--------------|
| Alice | 10 |
| Bob | 20 |
| Charlie | 10 |

### 설명:

- IN은 지정된 값 목록 또는 서브쿼리 결과에 값이 포함되어 있는지 확인한다.
- NOT IN을 사용하여 포함되지 않는 경우를 찾을 수 있다.
- NULL 값 주의: NOT IN 사용 시 서브쿼리 결과에 NULL이 포함되면 전체 결과가 빈 set이 될 수 있다.

## 3. BETWEEN AND

### 문법:

```sql
SELECT column1, column2, ...
FROM table
WHERE column_name BETWEEN value1 AND value2;
```

### Oracle과 SQL Server 예시:

예시 테이블 (Products):
| ProductID | ProductName | Price |
|-----------|-------------|-------|
| 1 | Apple | 0.50 |
| 2 | Banana | 0.75 |
| 3 | Orange | 0.60 |
| 4 | Mango | 1.00 |

```sql
SELECT ProductName, Price
FROM Products
WHERE Price BETWEEN 0.60 AND 0.90;
```

결과:
| ProductName | Price |
|-------------|-------|
| Banana | 0.75 |
| Orange | 0.60 |

### 설명:

- BETWEEN은 지정된 범위의 값을 선택한다 (양 끝값 포함).
- 날짜, 숫자, 텍스트 등 다양한 데이터 타입에 사용할 수 있다.
- NOT BETWEEN을 사용하여 범위 밖의 값을 선택할 수 있다.

## 주의사항:

1. NULL 처리: IN, EXISTS, BETWEEN 모두 NULL 값 처리에 주의해야 한다.
2. 성능: 대량 데이터에서 IN보다 EXISTS가 일반적으로 더 효율적이다.
3. 범위: BETWEEN은 경계값을 포함하므로, 필요에 따라 부등호를 사용해 정확한 범위를 지정해야 할 수 있다.
4. 데이터 타입: BETWEEN 사용 시 비교하는 값들의 데이터 타입이 일치해야 한다.

이러한 연산자들은 복잡한 조건을 간단하게 표현할 수 있게 해주며, 특히 서브쿼리와 함께 사용할 때 강력한 기능을 발휘한다. 그러나 대용량 데이터 처리 시 성능에 주의를 기울여야 한다.

# SQL 고급 그룹화 함수 가이드

## 1. GROUPING SETS

### 문법:

```sql
SELECT column1, column2, ..., aggregate_function(column)
FROM table
GROUP BY GROUPING SETS ((column1), (column2), ..., ());
```

### Oracle과 SQL Server 예시:

예시 테이블 (Sales):
| Product | Category | Year | Sales |
|----------|----------|------|-------|
| Apple | Fruit | 2021 | 1000 |
| Banana | Fruit | 2021 | 1500 |
| Carrot | Vegetable| 2021 | 800 |
| Apple | Fruit | 2022 | 1200 |
| Banana | Fruit | 2022 | 1700 |
| Carrot | Vegetable| 2022 | 1000 |

```sql
SELECT Product, Category, Year, SUM(Sales) as TotalSales
FROM Sales
GROUP BY GROUPING SETS ((Product, Category, Year), (Product, Category), (Category, Year), ());
```

결과:
| Product | Category | Year | TotalSales |
|----------|----------|------|------------|
| Apple | Fruit | 2021 | 1000 |
| Apple | Fruit | 2022 | 1200 |
| Banana | Fruit | 2021 | 1500 |
| Banana | Fruit | 2022 | 1700 |
| Carrot | Vegetable| 2021 | 800 |
| Carrot | Vegetable| 2022 | 1000 |
| Apple | Fruit | NULL | 2200 |
| Banana | Fruit | NULL | 3200 |
| Carrot | Vegetable| NULL | 1800 |
| NULL | Fruit | 2021 | 2500 |
| NULL | Fruit | 2022 | 2900 |
| NULL | Vegetable| 2021 | 800 |
| NULL | Vegetable| 2022 | 1000 |
| NULL | NULL | NULL | 7200 |

### 설명:

- GROUPING SETS는 여러 GROUP BY 쿼리를 UNION ALL로 결합한 것과 같은 결과를 제공한다.
- 각 그룹화 세트는 개별적으로 처리되며, 결과는 하나의 결과 집합으로 결합된다.
- 빈 괄호 ()는 전체 합계를 나타낸다.

## 2. ROLLUP

### 문법:

```sql
SELECT column1, column2, ..., aggregate_function(column)
FROM table
GROUP BY ROLLUP (column1, column2, ...);
```

### Oracle과 SQL Server 예시:

```sql
SELECT Category, Year, SUM(Sales) as TotalSales
FROM Sales
GROUP BY ROLLUP (Category, Year);
```

결과:
| Category | Year | TotalSales |
|----------|------|------------|
| Fruit | 2021 | 2500 |
| Fruit | 2022 | 2900 |
| Fruit | NULL | 5400 |
| Vegetable| 2021 | 800 |
| Vegetable| 2022 | 1000 |
| Vegetable| NULL | 1800 |
| NULL | NULL | 7200 |

### 설명:

- ROLLUP은 지정된 열의 계층적 소계를 생성한다.
- 오른쪽에서 왼쪽으로 열을 하나씩 제거하며 그룹화한다.
- 마지막 행은 전체 합계를 나타낸다.
- ROLLUP (A, B, C)는 (A, B, C), (A, B), (A), () 그룹화와 동일하다.

## 3. CUBE

### 문법:

```sql
SELECT column1, column2, ..., aggregate_function(column)
FROM table
GROUP BY CUBE (column1, column2, ...);
```

### Oracle과 SQL Server 예시:

```sql
SELECT Category, Year, SUM(Sales) as TotalSales
FROM Sales
GROUP BY CUBE (Category, Year);
```

결과:
| Category | Year | TotalSales |
|----------|------|------------|
| Fruit | 2021 | 2500 |
| Fruit | 2022 | 2900 |
| Fruit | NULL | 5400 |
| Vegetable| 2021 | 800 |
| Vegetable| 2022 | 1000 |
| Vegetable| NULL | 1800 |
| NULL | 2021 | 3300 |
| NULL | 2022 | 3900 |
| NULL | NULL | 7200 |

### 설명:

- CUBE는 지정된 열의 모든 가능한 조합에 대한 소계를 생성한다.
- CUBE (A, B, C)는 (A, B, C), (A, B), (A, C), (B, C), (A), (B), (C), () 그룹화와 동일하다.
- ROLLUP보다 더 많은 소계를 생성하므로 결과 집합이 더 크다.

## 주의사항:

1. 성능: CUBE는 ROLLUP보다 더 많은 연산을 수행하므로 대규모 데이터셋에서는 성능에 주의해야 한다.
2. NULL 처리: 결과에서 NULL은 해당 열이 그룹화에 포함되지 않았음을 나타낸다.
3. GROUPING 함수: GROUPING(column)을 사용하여 NULL이 그룹화 결과인지 실제 데이터의 NULL인지 구분할 수 있다.
4. 부분 ROLLUP/CUBE: Oracle에서는 ROLLUP/CUBE에 포함될 열을 선택적으로 지정할 수 있다.

## 4. GROUPING 함수

### 문법:

```sql
SELECT ..., GROUPING(column) as grouping_col
FROM table
GROUP BY ROLLUP/CUBE/GROUPING SETS (...);
```

### Oracle과 SQL Server 예시:

```sql
SELECT
    CASE
        WHEN GROUPING(Category) = 1 THEN 'All Categories'
        ELSE COALESCE(Category, 'Unknown')
    END AS Category,
    CASE
        WHEN GROUPING(Year) = 1 THEN 'All Years'
        ELSE COALESCE(CAST(Year AS VARCHAR), 'Unknown')
    END AS Year,
    SUM(Sales) as TotalSales
FROM Sales
GROUP BY CUBE (Category, Year);
```

결과:
| Category | Year | TotalSales |
|---------------|-----------|------------|
| Fruit | 2021 | 2500 |
| Fruit | 2022 | 2900 |
| Fruit | All Years | 5400 |
| Vegetable | 2021 | 800 |
| Vegetable | 2022 | 1000 |
| Vegetable | All Years | 1800 |
| All Categories| 2021 | 3300 |
| All Categories| 2022 | 3900 |
| All Categories| All Years | 7200 |

### 설명:

- GROUPING 함수는 지정된 열이 그룹화에 사용되었는지 여부를 반환한다.
- 반환값이 0이면 해당 열이 그룹화에 사용되었음을, 1이면 사용되지 않았음을 나타낸다.
- 이를 통해 소계, 총계 행을 식별하고 의미 있는 레이블을 부여할 수 있다.

이러한 고급 그룹화 함수들은 복잡한 분석 쿼리와 리포팅에 매우 유용하다. 데이터의 다양한 차원에서 집계를 수행하고, 계층적 결과를 생성할 수 있다. 그러나 대규모 데이터셋에서는 성능에 주의를 기울여야 하며, 결과 해석 시 NULL 값의 의미를 정확히 이해해야 한다.

다음으로 윈도우 함수와 관련된 내용을 설명하겠습니다.

---

</br></br></br>

# SQL 윈도우 함수 가이드

윈도우 함수는 행들의 집합(윈도우)에 대해 계산을 수행하는 함수입니다. 이들은 결과 집합의 각 행마다 계산을 수행하며, ORDER BY를 사용해 정렬된 결과에 대해 누적 계산도 가능합니다.

## 1. OVER 절

### 문법:

```sql
window_function (expression) OVER (
   [PARTITION BY partition_expression, ...]
   [ORDER BY sort_expression [ASC | DESC], ...]
   [ROWS | RANGE frame_extent]
)
```

## 2. 순위 함수

### ROW_NUMBER, RANK, DENSE_RANK

#### 문법:

```sql
ROW_NUMBER() OVER ([PARTITION BY column] ORDER BY column)
RANK() OVER ([PARTITION BY column] ORDER BY column)
DENSE_RANK() OVER ([PARTITION BY column] ORDER BY column)
```

#### Oracle과 SQL Server 예시:

예시 테이블 (Employees):
| EmpID | EmpName | Department | Salary |
|-------|---------|------------|--------|
| 1 | Alice | IT | 60000 |
| 2 | Bob | HR | 55000 |
| 3 | Charlie | IT | 65000 |
| 4 | David | Finance | 70000 |
| 5 | Eve | HR | 55000 |

```sql
SELECT
    EmpName,
    Department,
    Salary,
    ROW_NUMBER() OVER (ORDER BY Salary DESC) as RowNum,
    RANK() OVER (ORDER BY Salary DESC) as Rank,
    DENSE_RANK() OVER (ORDER BY Salary DESC) as DenseRank
FROM Employees;
```

결과:
| EmpName | Department | Salary | RowNum | Rank | DenseRank |
|---------|------------|--------|--------|------|-----------|
| David | Finance | 70000 | 1 | 1 | 1 |
| Charlie | IT | 65000 | 2 | 2 | 2 |
| Alice | IT | 60000 | 3 | 3 | 3 |
| Bob | HR | 55000 | 4 | 4 | 4 |
| Eve | HR | 55000 | 5 | 4 | 4 |

### 설명:

- ROW_NUMBER(): 각 행에 고유한 순위를 부여한다.
- RANK(): 동일한 값에 대해 같은 순위를 부여하고, 다음 순위를 건너뛴다.
- DENSE_RANK(): 동일한 값에 대해 같은 순위를 부여하고, 다음 순위를 연속적으로 부여한다.

## 3. 오프셋 함수

### LAG, LEAD

#### 문법:

```sql
LAG(column, offset, default) OVER ([PARTITION BY column] ORDER BY column)
LEAD(column, offset, default) OVER ([PARTITION BY column] ORDER BY column)
```

#### Oracle과 SQL Server 예시:

```sql
SELECT
    EmpName,
    Department,
    Salary,
    LAG(Salary, 1, 0) OVER (ORDER BY Salary) as PrevSalary,
    LEAD(Salary, 1, 0) OVER (ORDER BY Salary) as NextSalary
FROM Employees;
```

결과:
| EmpName | Department | Salary | PrevSalary | NextSalary |
|---------|------------|--------|------------|------------|
| Bob | HR | 55000 | 0 | 55000 |
| Eve | HR | 55000 | 55000 | 60000 |
| Alice | IT | 60000 | 55000 | 65000 |
| Charlie | IT | 65000 | 60000 | 70000 |
| David | Finance | 70000 | 65000 | 0 |

### 설명:

- LAG(): 현재 행을 기준으로 이전 행의 값을 반환한다.
- LEAD(): 현재 행을 기준으로 다음 행의 값을 반환한다.
- offset은 몇 번째 이전/다음 행을 참조할지 지정한다.
- default는 참조할 행이 없을 때 반환할 값을 지정한다.

## 4. 집계 함수

### SUM, AVG, COUNT, MAX, MIN

#### 문법:

```sql
aggregate_function(column) OVER (
   [PARTITION BY column]
   [ORDER BY column
    [ROWS | RANGE BETWEEN ... AND ...]]
)
```

#### Oracle과 SQL Server 예시:

```sql
SELECT
    EmpName,
    Department,
    Salary,
    SUM(Salary) OVER (PARTITION BY Department) as DeptTotalSalary,
    AVG(Salary) OVER (PARTITION BY Department) as DeptAvgSalary,
    COUNT(*) OVER (PARTITION BY Department) as DeptEmpCount
FROM Employees;
```

결과:
| EmpName | Department | Salary | DeptTotalSalary | DeptAvgSalary | DeptEmpCount |
|---------|------------|--------|-----------------|---------------|--------------|
| Alice | IT | 60000 | 125000 | 62500 | 2 |
| Charlie | IT | 65000 | 125000 | 62500 | 2 |
| Bob | HR | 55000 | 110000 | 55000 | 2 |
| Eve | HR | 55000 | 110000 | 55000 | 2 |
| David | Finance | 70000 | 70000 | 70000 | 1 |

### 설명:

- PARTITION BY: 지정된 열을 기준으로 데이터를 분할한다.
- ORDER BY: 윈도우 내에서 행의 순서를 정의한다.
- ROWS/RANGE: 윈도우의 범위를 정의한다.

## 5. 분석 함수

### PERCENT_RANK, CUME_DIST, NTILE

#### 문법:

```sql
PERCENT_RANK() OVER ([PARTITION BY column] ORDER BY column)
CUME_DIST() OVER ([PARTITION BY column] ORDER BY column)
NTILE(n) OVER ([PARTITION BY column] ORDER BY column)
```

#### Oracle과 SQL Server 예시:

```sql
SELECT
    EmpName,
    Department,
    Salary,
    PERCENT_RANK() OVER (ORDER BY Salary) as PercentRank,
    CUME_DIST() OVER (ORDER BY Salary) as CumeDist,
    NTILE(4) OVER (ORDER BY Salary) as Quartile
FROM Employees;
```

결과:
| EmpName | Department | Salary | PercentRank | CumeDist | Quartile |
|---------|------------|--------|-------------|----------|----------|
| Bob | HR | 55000 | 0 | 0.4 | 1 |
| Eve | HR | 55000 | 0 | 0.4 | 1 |
| Alice | IT | 60000 | 0.5 | 0.6 | 2 |
| Charlie | IT | 65000 | 0.75 | 0.8 | 3 |
| David | Finance | 70000 | 1 | 1 | 4 |

### 설명:

- PERCENT_RANK(): 백분율 순위를 계산한다 (0~1 사이의 값).
- CUME_DIST(): 누적 분포 값을 계산한다 (0~1 사이의 값).
- NTILE(n): 데이터를 n개의 그룹으로 나눈다.

## 주의사항:

1. 성능: 윈도우 함수는 대량의 데이터를 처리할 때 성능에 영향을 줄 수 있다.
2. 프레임 지정: ROWS/RANGE를 사용하여 정확한 윈도우 프레임을 지정해야 할 수 있다.
3. NULL 처리: 윈도우 함수에서 NULL 값 처리에 주의해야 한다.
4. 데이터베이스 버전: 일부 고급 윈도우 함수는 최신 버전의 데이터베이스에서만 지원될 수 있다.

윈도우 함수는 복잡한 분석 쿼리를 간단하게 만들어주며, 특히 데이터 웨어하우스나 비즈니스 인텔리전스 애플리케이션에서 자주 사용됩니다. 각 데이터베이스 시스템의 최신 버전에서는 더 다양한 윈도우 함수와 기능을 제공할 수 있으므로, 필요에 따라 공식 문서를 참조하는 것이 좋습니다.

## 6. RATIO_TO_REPORT

### 문법:

```sql
RATIO_TO_REPORT(numeric_expression) OVER ([PARTITION BY partition_expression])
```

### Oracle 예시:

```sql
SELECT
    EmpName,
    Department,
    Salary,
    RATIO_TO_REPORT(Salary) OVER () as SalaryRatio
FROM Employees;
```

결과:
| EmpName | Department | Salary | SalaryRatio |
|---------|------------|--------|-------------|
| Alice | IT | 60000 | 0.1967 |
| Bob | HR | 55000 | 0.1803 |
| Charlie | IT | 65000 | 0.2131 |
| David | Finance | 70000 | 0.2295 |
| Eve | HR | 55000 | 0.1803 |

### 설명:

- RATIO_TO_REPORT는 윈도우 내에서 특정 값의 비율을 계산합니다.
- Oracle에서만 사용 가능한 함수입니다.
- SQL Server에서는 SUM과 OVER 절을 조합하여 유사한 결과를 얻을 수 있습니다.

## 7. GROUPING

### 문법:

```sql
GROUPING(column)
```

### Oracle과 SQL Server 예시:

```sql
SELECT
    CASE
        WHEN GROUPING(Department) = 1 THEN 'All Departments'
        ELSE Department
    END AS Department,
    SUM(Salary) as TotalSalary
FROM Employees
GROUP BY ROLLUP(Department);
```

결과:
| Department | TotalSalary |
|-----------------|-------------|
| IT | 125000 |
| HR | 110000 |
| Finance | 70000 |
| All Departments | 305000 |

### 설명:

- GROUPING 함수는 ROLLUP, CUBE, GROUPING SETS와 함께 사용됩니다.
- 반환값이 1이면 해당 열이 그룹화에 사용되지 않았음을, 0이면 사용되었음을 나타냅니다.

## 8. CASE WHEN

### 문법:

```sql
CASE
    WHEN condition1 THEN result1
    WHEN condition2 THEN result2
    ...
    ELSE result
END
```

### Oracle과 SQL Server 예시:

```sql
SELECT
    EmpName,
    Salary,
    CASE
        WHEN Salary < 60000 THEN 'Low'
        WHEN Salary BETWEEN 60000 AND 65000 THEN 'Medium'
        ELSE 'High'
    END AS SalaryCategory
FROM Employees;
```

결과:
| EmpName | Salary | SalaryCategory |
|---------|--------|----------------|
| Alice | 60000 | Medium |
| Bob | 55000 | Low |
| Charlie | 65000 | Medium |
| David | 70000 | High |
| Eve | 55000 | Low |

### 설명:

- CASE WHEN은 조건부 표현식을 생성하는 데 사용됩니다.
- 윈도우 함수와 함께 사용하여 복잡한 조건부 계산을 수행할 수 있습니다.

이러한 윈도우 함수와 관련 기능들은 데이터 분석과 리포팅에서 강력한 도구로 사용됩니다. 각 함수의 특성과 사용 방법을 이해하고 적절히 활용하면, 복잡한 비즈니스 요구사항을 효율적으로 해결할 수 있습니다. 다만, 대용량 데이터 처리 시 성능에 주의를 기울여야 하며, 데이터베이스 시스템 간의 차이를 고려해야 합니다.

---

</br></br></br>

# SQL 계층형 쿼리 가이드

계층형 쿼리는 트리 구조의 데이터를 조회하고 처리하는 데 사용됩니다. 주로 조직도, 제품 카테고리, 파일 시스템 구조 등을 표현할 때 활용됩니다.

## 1. Oracle의 계층형 쿼리

### 문법:

```sql
SELECT column, LEVEL
FROM table
START WITH condition
CONNECT BY [NOCYCLE] PRIOR parent_column = child_column
[ORDER SIBLINGS BY column];
```

### 예시 테이블 (Employees):

| EmpID | EmpName | ManagerID |
| ----- | ------- | --------- |
| 1     | Alice   | NULL      |
| 2     | Bob     | 1         |
| 3     | Charlie | 1         |
| 4     | David   | 2         |
| 5     | Eve     | 2         |

### Oracle 예시:

```sql
SELECT
    LPAD(' ', 2 * (LEVEL - 1)) || EmpName AS Hierarchy,
    LEVEL,
    EmpID,
    ManagerID
FROM Employees
START WITH ManagerID IS NULL
CONNECT BY PRIOR EmpID = ManagerID
ORDER SIBLINGS BY EmpName;
```

결과:
| Hierarchy | LEVEL | EmpID | ManagerID |
|-----------|-------|-------|-----------|
| Alice | 1 | 1 | NULL |
| Bob | 2 | 2 | 1 |
| David | 3 | 4 | 2 |
| Eve | 3 | 5 | 2 |
| Charlie | 2 | 3 | 1 |

### 설명:

- START WITH: 계층 구조의 루트 노드를 지정합니다.
- CONNECT BY: 부모-자식 관계를 정의합니다.
- PRIOR: 이전 레벨의 열을 참조합니다.
- LEVEL: 현재 행의 깊이를 나타냅니다.
- ORDER SIBLINGS BY: 같은 레벨의 형제 노드를 정렬합니다.

## 2. SQL Server의 재귀적 CTE

### 문법:

```sql
WITH RECURSIVE cte_name AS (
    -- 앵커 멤버
    SELECT columns
    FROM table
    WHERE condition
    UNION ALL
    -- 재귀 멤버
    SELECT columns
    FROM table
    INNER JOIN cte_name ON condition
)
SELECT columns FROM cte_name;
```

### SQL Server 예시:

```sql
WITH EmployeeHierarchy AS (
    -- 앵커 멤버
    SELECT EmpID, EmpName, ManagerID, 1 AS Level,
           CAST(EmpName AS NVARCHAR(MAX)) AS Hierarchy
    FROM Employees
    WHERE ManagerID IS NULL

    UNION ALL

    -- 재귀 멤버
    SELECT e.EmpID, e.EmpName, e.ManagerID, eh.Level + 1,
           CAST(eh.Hierarchy + ' > ' + e.EmpName AS NVARCHAR(MAX))
    FROM Employees e
    INNER JOIN EmployeeHierarchy eh ON e.ManagerID = eh.EmpID
)
SELECT Hierarchy, Level, EmpID, ManagerID
FROM EmployeeHierarchy
ORDER BY Hierarchy;
```

결과:
| Hierarchy | Level | EmpID | ManagerID |
|-------------------|-------|-------|-----------|
| Alice | 1 | 1 | NULL |
| Alice > Bob | 2 | 2 | 1 |
| Alice > Bob > David| 3 | 4 | 2 |
| Alice > Bob > Eve | 3 | 5 | 2 |
| Alice > Charlie | 2 | 3 | 1 |

### 설명:

- WITH RECURSIVE: 재귀적 CTE를 정의합니다 (SQL Server에서는 RECURSIVE 키워드를 생략할 수 있습니다).
- 앵커 멤버: 재귀의 시작점을 정의합니다.
- 재귀 멤버: 재귀적으로 다음 레벨의 데이터를 검색합니다.
- UNION ALL: 앵커 멤버와 재귀 멤버의 결과를 결합합니다.

## 주의사항:

1. 무한 루프: CONNECT BY NOCYCLE (Oracle) 또는 적절한 종료 조건(SQL Server)을 사용하여 무한 루프를 방지해야 합니다.
2. 성능: 대규모 계층 구조에서는 성능에 주의해야 합니다.
3. 최대 깊이: SQL Server의 재귀적 CTE는 기본적으로 최대 100번의 재귀를 허용합니다. 필요한 경우 OPTION (MAXRECURSION n)을 사용하여 이 제한을 변경할 수 있습니다.
4. 데이터 정합성: 계층 구조의 데이터가 올바르게 구성되어 있는지 확인해야 합니다.

계층형 쿼리는 복잡한 트리 구조의 데이터를 효과적으로 조회하고 분석하는 데 매우 유용합니다. Oracle과 SQL Server에서 구문의 차이는 있지만, 기본적인 개념과 활용 방법은 유사합니다. 이러한 쿼리를 통해 조직 구조 분석, 제품 카테고리 관리, 계층적 데이터 리포팅 등 다양한 비즈니스 요구사항을 해결할 수 있습니다.

---

</br></br></br>

# SQL 조인 가이드

조인은 두 개 이상의 테이블에서 관련된 데이터를 결합하는 SQL 연산입니다. 다양한 유형의 조인을 통해 복잡한 데이터 관계를 표현하고 분석할 수 있습니다.

## 1. INNER JOIN

### 문법:

```sql
SELECT columns
FROM table1
INNER JOIN table2
ON table1.column = table2.column;
```

### 예시 테이블:

Employees:
| EmpID | EmpName | DeptID |
|-------|---------|--------|
| 1 | Alice | 10 |
| 2 | Bob | 20 |
| 3 | Charlie | 10 |
| 4 | David | 30 |

Departments:
| DeptID | DeptName |
|--------|----------|
| 10 | IT |
| 20 | HR |
| 30 | Finance |

### Oracle/SQL Server 예시:

```sql
SELECT e.EmpName, d.DeptName
FROM Employees e
INNER JOIN Departments d ON e.DeptID = d.DeptID;
```

결과:
| EmpName | DeptName |
|---------|----------|
| Alice | IT |
| Bob | HR |
| Charlie | IT |
| David | Finance |

### 설명:

- INNER JOIN은 두 테이블에서 조인 조건을 만족하는 행만 반환합니다.
- 양쪽 테이블에서 일치하는 데이터가 있는 경우에만 결과에 포함됩니다.

## 2. LEFT OUTER JOIN

### 문법:

```sql
SELECT columns
FROM table1
LEFT [OUTER] JOIN table2
ON table1.column = table2.column;
```

### Oracle/SQL Server 예시:

```sql
SELECT e.EmpName, d.DeptName
FROM Employees e
LEFT JOIN Departments d ON e.DeptID = d.DeptID;
```

결과:
| EmpName | DeptName |
|---------|----------|
| Alice | IT |
| Bob | HR |
| Charlie | IT |
| David | Finance |

### 설명:

- LEFT JOIN은 왼쪽 테이블(Employees)의 모든 행을 포함하고, 오른쪽 테이블(Departments)에서 일치하는 행을 가져옵니다.
- 일치하는 행이 없는 경우, 결과에 NULL이 포함됩니다.

## 3. RIGHT OUTER JOIN

### 문법:

```sql
SELECT columns
FROM table1
RIGHT [OUTER] JOIN table2
ON table1.column = table2.column;
```

### Oracle/SQL Server 예시:

```sql
SELECT e.EmpName, d.DeptName
FROM Employees e
RIGHT JOIN Departments d ON e.DeptID = d.DeptID;
```

결과:
| EmpName | DeptName |
|---------|----------|
| Alice | IT |
| Bob | HR |
| Charlie | IT |
| David | Finance |

### 설명:

- RIGHT JOIN은 오른쪽 테이블(Departments)의 모든 행을 포함하고, 왼쪽 테이블(Employees)에서 일치하는 행을 가져옵니다.
- 일치하는 행이 없는 경우, 결과에 NULL이 포함됩니다.

## 4. FULL OUTER JOIN

### 문법:

```sql
SELECT columns
FROM table1
FULL [OUTER] JOIN table2
ON table1.column = table2.column;
```

### Oracle/SQL Server 예시:

```sql
SELECT e.EmpName, d.DeptName
FROM Employees e
FULL OUTER JOIN Departments d ON e.DeptID = d.DeptID;
```

결과:
| EmpName | DeptName |
|---------|----------|
| Alice | IT |
| Bob | HR |
| Charlie | IT |
| David | Finance |

### 설명:

- FULL OUTER JOIN은 양쪽 테이블의 모든 행을 포함합니다.
- 일치하는 행이 없는 경우, 결과에 NULL이 포함됩니다.
- SQL Server는 FULL OUTER JOIN을 지원하지만, Oracle은 이를 UNION을 사용하여 구현해야 합니다.

## 5. CROSS JOIN

### 문법:

```sql
SELECT columns
FROM table1
CROSS JOIN table2;
```

### Oracle/SQL Server 예시:

```sql
SELECT e.EmpName, d.DeptName
FROM Employees e
CROSS JOIN Departments d;
```

결과:
| EmpName | DeptName |
|---------|----------|
| Alice | IT |
| Alice | HR |
| Alice | Finance |
| Bob | IT |
| Bob | HR |
| Bob | Finance |
| Charlie | IT |
| Charlie | HR |
| Charlie | Finance |
| David | IT |
| David | HR |
| David | Finance |

### 설명:

- CROSS JOIN은 두 테이블의 모든 가능한 조합을 반환합니다 (카테시안 곱).
- 조인 조건이 없으며, 결과 행의 수는 두 테이블 행 수의 곱과 같습니다.

## 6. SELF JOIN

### 문법:

```sql
SELECT columns
FROM table1 t1
JOIN table1 t2
ON t1.column = t2.column;
```

### Oracle/SQL Server 예시:

```sql
SELECT e1.EmpName as Employee, e2.EmpName as Manager
FROM Employees e1
LEFT JOIN Employees e2 ON e1.ManagerID = e2.EmpID;
```

결과:
| Employee | Manager |
|----------|---------|
| Alice | NULL |
| Bob | Alice |
| Charlie | Alice |
| David | Bob |

### 설명:

- SELF JOIN은 테이블을 자기 자신과 조인합니다.
- 주로 계층 구조나 관계를 표현할 때 사용됩니다.

## 7. NATURAL JOIN

### 문법:

```sql
SELECT columns
FROM table1
NATURAL JOIN table2;
```

### Oracle 예시:

```sql
SELECT e.EmpName, d.DeptName
FROM Employees e
NATURAL JOIN Departments d;
```

### 설명:

- NATURAL JOIN은 두 테이블에서 이름이 같은 모든 열을 기준으로 자동으로 조인합니다.
- Oracle에서 지원되지만, SQL Server에서는 직접 지원하지 않습니다.
- 예상치 못한 결과를 낼 수 있으므로 주의해서 사용해야 합니다.

## 8. USING 절

### 문법:

```sql
SELECT columns
FROM table1
JOIN table2
USING (column);
```

### Oracle 예시:

```sql
SELECT e.EmpName, d.DeptName
FROM Employees e
JOIN Departments d USING (DeptID);
```

### 설명:

- USING 절은 조인에 사용할 열을 명시적으로 지정합니다.
- Oracle에서 지원되지만, SQL Server에서는 직접 지원하지 않습니다.

## 주의사항:

1. 성능: 대량의 데이터를 조인할 때는 성能에 주의해야 합니다. 적절한 인덱스 사용이 중요합니다.
2. NULL 값: OUTER JOIN에서 NULL 값 처리에 주의해야 합니다.
3. 조인 조건: 적절한 조인 조건을 사용하지 않으면 예상치 못한 결과나 성능 저하가 발생할 수 있습니다.
4. 다중 테이블 조인: 여러 테이블을 조인할 때는 조인 순서와 조건을 신중히 고려해야 합니다.
5. NATURAL JOIN과 USING 절: 이들은 편리할 수 있지만, 예상치 못한 결과를 낼 수 있으므로 주의해서 사용해야 합니다.

조인은 관계형 데이터베이스에서 데이터를 효과적으로 결합하고 분석하는 데 필수적인 연산입니다. 각 조인 유형의 특성을 이해하고 적절히 사용하면, 복잡한 데이터 관계를 효과적으로 다룰 수 있습니다.

# SQL 추가 함수 가이드

## 1. ROUND

### 문법:

```sql
ROUND(number, decimal_places)
```

### Oracle/SQL Server 예시:

```sql
SELECT ROUND(123.456, 2) as Rounded;
```

결과:
| Rounded |
|---------|
| 123.46 |

### 설명:

- 지정된 소수점 자릿수로 숫자를 반올림합니다.
- 두 번째 인자가 음수이면 정수 부분을 반올림합니다.

## 2. PERCENT_RANK

### 문법:

```sql
PERCENT_RANK() OVER (ORDER BY column)
```

### Oracle/SQL Server 예시:

```sql
SELECT
    EmpName,
    Salary,
    PERCENT_RANK() OVER (ORDER BY Salary) as PercentRank
FROM Employees;
```

결과:
| EmpName | Salary | PercentRank |
|---------|--------|-------------|
| Bob | 55000 | 0 |
| Alice | 60000 | 0.33333 |
| Charlie | 65000 | 0.66667 |
| David | 70000 | 1 |

### 설명:

- 정렬된 결과 집합 내에서 각 행의 상대적 순위를 백분율로 계산합니다.
- 결과는 0에서 1 사이의 값입니다.

## 3. CUME_DIST

### 문법:

```sql
CUME_DIST() OVER (ORDER BY column)
```

### Oracle/SQL Server 예시:

```sql
SELECT
    EmpName,
    Salary,
    CUME_DIST() OVER (ORDER BY Salary) as CumeDist
FROM Employees;
```

결과:
| EmpName | Salary | CumeDist |
|---------|--------|----------|
| Bob | 55000 | 0.25 |
| Alice | 60000 | 0.5 |
| Charlie | 65000 | 0.75 |
| David | 70000 | 1 |

### 설명:

- 정렬된 결과 집합 내에서 각 행의 누적 분포를 계산합니다.
- 결과는 0보다 크고 1 이하의 값입니다.

## 4. NTILE

### 문법:

```sql
NTILE(n) OVER (ORDER BY column)
```

### Oracle/SQL Server 예시:

```sql
SELECT
    EmpName,
    Salary,
    NTILE(3) OVER (ORDER BY Salary) as Tertile
FROM Employees;
```

결과:
| EmpName | Salary | Tertile |
|---------|--------|---------|
| Bob | 55000 | 1 |
| Alice | 60000 | 1 |
| Charlie | 65000 | 2 |
| David | 70000 | 3 |

### 설명:

- 정렬된 결과 집합을 지정된 수의 그룹으로 나눕니다.
- 각 행에 1부터 n까지의 그룹 번호를 할당합니다.

## 5. GROUPING

### 문법:

```sql
GROUPING(column)
```

### Oracle/SQL Server 예시:

```sql
SELECT
    CASE WHEN GROUPING(Department) = 1 THEN 'All Departments' ELSE Department END as Department,
    SUM(Salary) as TotalSalary
FROM Employees
GROUP BY ROLLUP(Department);
```

결과:
| Department | TotalSalary |
|-----------------|-------------|
| IT | 125000 |
| HR | 55000 |
| Finance | 70000 |
| All Departments | 250000 |

### 설명:

- ROLLUP, CUBE, GROUPING SETS와 함께 사용됩니다.
- 반환값이 1이면 해당 열이 그룹화에 사용되지 않았음을, 0이면 사용되었음을 나타냅니다.

## 주의사항:

1. 데이터 타입: 함수에 전달되는 인자의 데이터 타입이 적절해야 합니다.
2. NULL 처리: 일부 함수는 NULL 값을 특별히 처리할 수 있으므로 주의가 필요합니다.
3. 성능: 윈도우 함수 (PERCENT_RANK, CUME_DIST, NTILE 등)는 대량의 데이터에서 성능에 영향을 줄 수 있습니다.
4. 데이터베이스 버전: 일부 함수는 특정 데이터베이스 버전에서만 사용 가능할 수 있습니다.

이러한 함수들은 데이터 분석과 리포팅에서 강력한 도구로 사용됩니다. 각 함수의 특성과 사용 방법을 이해하고 적절히 활용하면, 복잡한 데이터 처리 작업을 효과적으로 수행할 수 있습니다.
