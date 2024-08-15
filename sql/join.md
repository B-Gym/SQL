# SQL 조인(Join)

## 목차

1. [조인(Join)이란?](#조인join이란)
2. [조인의 종류](#조인의-종류)
3. [표준 조인 문법](#표준-조인-문법)
4. [각 조인 유형별 예시 및 테이블 변화](#각-조인-유형별-예시-및-테이블-변화)
5. [조인 사용 시 주의사항](#조인-사용-시-주의사항)

## 조인(Join)이란?

조인은 두 개 이상의 테이블에서 관련된 행들을 결합하는 연산입니다. 이를 통해 여러 테이블에 분산된 데이터를 하나의 결과 집합으로 만들 수 있습니다.

## 조인의 종류

1. INNER JOIN (내부 조인)
2. LEFT OUTER JOIN (왼쪽 외부 조인)
3. RIGHT OUTER JOIN (오른쪽 외부 조인)
4. FULL OUTER JOIN (전체 외부 조인)
5. CROSS JOIN (교차 조인)
6. SELF JOIN (자체 조인)
7. NETURAL JOIN

## 표준 조인 문법

SQL-92 표준에서는 다음과 같은 조인 문법을 정의하고 있습니다:

```sql
SELECT columns
FROM table1
[INNER | LEFT | RIGHT | FULL] JOIN table2
ON table1.column = table2.column;
```

## 각 조인 유형별 예시

예시를 위해 다음과 같은 두 테이블을 사용하겠습니다:

Employees 테이블:

```
| emp_id | emp_name | dept_id |
|--------|----------|---------|
| 1      | Alice    | 1       |
| 2      | Bob      | 2       |
| 3      | Charlie  | 1       |
| 4      | David    | NULL    |
```

Departments 테이블:

```
| dept_id | dept_name |
|---------|-----------|
| 1       | HR        |
| 2       | IT        |
| 3       | Sales     |
```

### 1. INNER JOIN

```sql
SELECT e.emp_name, d.dept_name
FROM Employees e
INNER JOIN Departments d ON e.dept_id = d.dept_id;
```

결과:

```
| emp_name | dept_name |
|----------|-----------|
| Alice    | HR        |
| Bob      | IT        |
| Charlie  | HR        |
```

INNER JOIN은 두 테이블에서 조인 조건을 만족하는 행만 반환합니다. David는 dept_id가 NULL이므로 결과에 포함되지 않습니다.

### 2. LEFT OUTER JOIN

```sql
SELECT e.emp_name, d.dept_name
FROM Employees e
LEFT JOIN Departments d ON e.dept_id = d.dept_id;
```

결과:

```
| emp_name | dept_name |
|----------|-----------|
| Alice    | HR        |
| Bob      | IT        |
| Charlie  | HR        |
| David    | NULL      |
```

LEFT JOIN은 왼쪽 테이블(Employees)의 모든 행을 포함하고, 오른쪽 테이블(Departments)에서 매칭되는 행이 없으면 NULL을 반환합니다.

### 3. RIGHT OUTER JOIN

```sql
SELECT e.emp_name, d.dept_name
FROM Employees e
RIGHT JOIN Departments d ON e.dept_id = d.dept_id;
```

결과:

```
| emp_name | dept_name |
|----------|-----------|
| Alice    | HR        |
| Bob      | IT        |
| Charlie  | HR        |
| NULL     | Sales     |
```

RIGHT JOIN은 오른쪽 테이블(Departments)의 모든 행을 포함하고, 왼쪽 테이블(Employees)에서 매칭되는 행이 없으면 NULL을 반환합니다.

### 4. FULL OUTER JOIN

```sql
SELECT e.emp_name, d.dept_name
FROM Employees e
FULL OUTER JOIN Departments d ON e.dept_id = d.dept_id;
```

결과:

```
| emp_name | dept_name |
|----------|-----------|
| Alice    | HR        |
| Bob      | IT        |
| Charlie  | HR        |
| David    | NULL      |
| NULL     | Sales     |
```

FULL OUTER JOIN은 양쪽 테이블의 모든 행을 포함하고, 매칭되는 행이 없는 경우 NULL을 반환합니다.

### 5. CROSS JOIN

```sql
SELECT e.emp_name, d.dept_name
FROM Employees e
CROSS JOIN Departments d;
```

결과:

```
| emp_name | dept_name |
|----------|-----------|
| Alice    | HR        |
| Alice    | IT        |
| Alice    | Sales     |
| Bob      | HR        |
| Bob      | IT        |
| Bob      | Sales     |
| Charlie  | HR        |
| Charlie  | IT        |
| Charlie  | Sales     |
| David    | HR        |
| David    | IT        |
| David    | Sales     |
```

CROSS JOIN은 두 테이블의 모든 가능한 조합을 반환합니다. 결과 행의 수는 Employees 테이블의 행 수(4) \* Departments 테이블의 행 수(3) = 12입니다.

### 6. SELF JOIN

Employees 테이블에 manager_id 열을 추가한 예:

```
| emp_id | emp_name | dept_id | manager_id |
|--------|----------|---------|------------|
| 1      | Alice    | 1       | NULL       |
| 2      | Bob      | 2       | 1          |
| 3      | Charlie  | 1       | 1          |
| 4      | David    | NULL    | 2          |
```

```sql
SELECT e1.emp_name AS employee, e2.emp_name AS manager
FROM Employees e1
LEFT JOIN Employees e2 ON e1.manager_id = e2.emp_id;
```

결과:

```
| employee | manager |
|----------|---------|
| Alice    | NULL    |
| Bob      | Alice   |
| Charlie  | Alice   |
| David    | Bob     |
```

SELF JOIN은 같은 테이블을 자기 자신과 조인합니다. 여기서는 각 직원의 매니저를 찾는데 사용되었습니다.

### 7. NATURAL JOIN

NATURAL JOIN은 두 테이블에서 이름이 같은 모든 열을 기준으로 자동으로 조인을 수행합니다.

예시:

```sql
SELECT e.emp_name, d.dept_name
FROM Employees e
NATURAL JOIN Departments d;
```

이 쿼리는 Employees와 Departments 테이블에서 이름이 같은 열(dept_id)을 자동으로 찾아 조인합니다.

결과:

```
| emp_name | dept_name |
|----------|-----------|
| Alice    | HR        |
| Bob      | IT        |
| Charlie  | HR        |
```

주의: NATURAL JOIN은 편리하지만 예상치 못한 결과를 낼 수 있으므로 주의해서 사용해야 합니다.

### USING 조건절

USING 조건절은 조인에 사용할 열을 명시적으로 지정합니다. 이는 NATURAL JOIN보다 더 명확하고 안전한 방법입니다.

예시:

```sql
SELECT e.emp_name, d.dept_name
FROM Employees e
JOIN Departments d USING (dept_id);
```

이 쿼리는 dept_id 열을 사용하여 두 테이블을 조인합니다.

결과:

```
| emp_name | dept_name |
|----------|-----------|
| Alice    | HR        |
| Bob      | IT        |
| Charlie  | HR        |
```

### ON 조건절

ON 조건절은 가장 일반적인 조인 조건 지정 방법으로, 복잡한 조인 조건을 표현할 수 있습니다.

예시:

```sql
SELECT e.emp_name, d.dept_name
FROM Employees e
JOIN Departments d ON e.dept_id = d.dept_id AND e.emp_id > 1;
```

이 쿼리는 dept_id로 조인하면서 추가로 emp_id가 1보다 큰 직원만 선택합니다.

결과:

```
| emp_name | dept_name |
|----------|-----------|
| Bob      | IT        |
| Charlie  | HR        |
```

USING과 ON의 주요 차이점:

1. USING은 같은 이름의 열을 간단히 지정할 수 있지만, ON은 더 복잡한 조건을 지정할 수 있습니다.
2. USING을 사용하면 지정된 열이 결과에 한 번만 나타나지만, ON을 사용하면 양쪽 테이블의 열이 모두 결과에 포함됩니다.
3. ON은 이름이 다른 열 간의 조인에도 사용할 수 있습니다.

각 방식의 적절한 사용:

- NATURAL JOIN: 두 테이블의 구조를 잘 알고 있고, 의도치 않은 조인이 발생하지 않을 것이 확실할 때 사용.
- USING: 조인 키가 양쪽 테이블에서 같은 이름을 가질 때 간단히 사용.
- ON: 가장 안전하고 명시적인 방법으로, 복잡한 조인 조건이나 서로 다른 이름의 열을 조인할 때 사용.

## 조인 사용 시 주의사항

1. 성능: 대용량 테이블 조인 시 성능에 주의해야 합니다. 적절한 인덱스 사용이 중요합니다.
2. 데이터 정합성: 조인 조건을 정확히 지정하여 원하는 결과만 얻도록 해야 합니다.
3. NULL 값 처리: OUTER JOIN 사용 시 NULL 값 처리에 주의해야 합니다.
4. 다중 테이블 조인: 여러 테이블을 조인할 때는 조인 순서와 조건을 신중히 고려해야 합니다.
5. CROSS JOIN 사용: 데이터 양이 급격히 증가할 수 있으므로 주의해서 사용해야 합니다.
