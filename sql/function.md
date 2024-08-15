# SQL 함수

## 목차

1. [문자열 함수](#문자열-함수)
2. [숫자 함수](#숫자-함수)
3. [날짜 함수](#날짜-함수)
4. [변환 함수](#변환-함수)
5. [NULL 관련 함수](#null-관련-함수)
6. [집계 함수](#집계-함수)
7. [윈도우 함수](#윈도우-함수)

## 문자열 함수

### LOWER, UPPER

- 설명: 문자열을 소문자 또는 대문자로 변환
- Oracle/SQL Server:
  ```sql
  SELECT LOWER('HELLO') AS lower_result, UPPER('hello') AS upper_result FROM DUAL;
  -- 결과: lower_result = 'hello', upper_result = 'HELLO'
  ```

### SUBSTR (Oracle) / SUBSTRING (SQL Server)

- 설명: 문자열의 일부를 추출
- Oracle:
  ```sql
  SELECT SUBSTR('ABCDEF', 2, 3) AS substr_result FROM DUAL;
  -- 결과: substr_result = 'BCD'
  ```
- SQL Server:
  ```sql
  SELECT SUBSTRING('ABCDEF', 2, 3) AS substring_result;
  -- 결과: substring_result = 'BCD'
  ```

### LENGTH (Oracle) / LEN (SQL Server)

- 설명: 문자열의 길이를 반환
- Oracle:
  ```sql
  SELECT LENGTH('Hello World') AS length_result FROM DUAL;
  -- 결과: length_result = 11
  ```
- SQL Server:
  ```sql
  SELECT LEN('Hello World') AS len_result;
  -- 결과: len_result = 11
  ```

### CONCAT

- 설명: 두 개 이상의 문자열을 연결
- Oracle/SQL Server:
  ```sql
  SELECT CONCAT('Hello', ' ', 'World') AS concat_result FROM DUAL;
  -- 결과: concat_result = 'Hello World'
  ```

### TRIM, LTRIM, RTRIM

- 설명: 문자열의 양쪽, 왼쪽, 오른쪽 공백을 제거
- Oracle/SQL Server:
  ```sql
  SELECT TRIM('  ABC  ') AS trim_result,
         LTRIM('  ABC  ') AS ltrim_result,
         RTRIM('  ABC  ') AS rtrim_result
  FROM DUAL;
  -- 결과: trim_result = 'ABC', ltrim_result = 'ABC  ', rtrim_result = '  ABC'
  ```

### INSTR (Oracle) / CHARINDEX (SQL Server)

- 설명: 문자열 내에서 특정 부분 문자열의 위치를 찾음
- Oracle:
  ```sql
  SELECT INSTR('Hello World', 'World') AS instr_result FROM DUAL;
  -- 결과: instr_result = 7
  ```
- SQL Server:
  ```sql
  SELECT CHARINDEX('World', 'Hello World') AS charindex_result;
  -- 결과: charindex_result = 7
  ```

### REPLACE

- 설명: 문자열 내의 특정 부분을 다른 문자열로 대체
- Oracle/SQL Server:
  ```sql
  SELECT REPLACE('Hello World', 'World', 'SQL') AS replace_result FROM DUAL;
  -- 결과: replace_result = 'Hello SQL'
  ```

### LPAD, RPAD (Oracle)

- 설명: 문자열의 왼쪽 또는 오른쪽을 특정 문자로 채움
- Oracle:
  ```sql
  SELECT LPAD('ABC', 5, '*') AS lpad_result,
         RPAD('ABC', 5, '*') AS rpad_result
  FROM DUAL;
  -- 결과: lpad_result = '**ABC', rpad_result = 'ABC**'
  ```

## 숫자 함수

### ROUND

- 설명: 숫자를 반올림
- Oracle/SQL Server:
  ```sql
  SELECT ROUND(3.14159, 2) AS round_result FROM DUAL;
  -- 결과: round_result = 3.14
  ```

### TRUNC (Oracle) / FLOOR (SQL Server)

- 설명: 숫자의 소수부를 잘라냄
- Oracle:
  ```sql
  SELECT TRUNC(3.14159, 2) AS trunc_result FROM DUAL;
  -- 결과: trunc_result = 3.14
  ```
- SQL Server:
  ```sql
  SELECT FLOOR(3.14159) AS floor_result;
  -- 결과: floor_result = 3
  ```

### MOD (Oracle) / % (SQL Server)

- 설명: 나눗셈의 나머지를 반환
- Oracle:
  ```sql
  SELECT MOD(10, 3) AS mod_result FROM DUAL;
  -- 결과: mod_result = 1
  ```
- SQL Server:
  ```sql
  SELECT 10 % 3 AS mod_result;
  -- 결과: mod_result = 1
  ```

### ABS

- 설명: 숫자의 절대값을 반환
- Oracle/SQL Server:
  ```sql
  SELECT ABS(-5) AS abs_result FROM DUAL;
  -- 결과: abs_result = 5
  ```

### CEIL (Oracle) / CEILING (SQL Server)

- 설명: 숫자보다 크거나 같은 최소 정수를 반환
- Oracle:
  ```sql
  SELECT CEIL(3.14) AS ceil_result FROM DUAL;
  -- 결과: ceil_result = 4
  ```
- SQL Server:
  ```sql
  SELECT CEILING(3.14) AS ceiling_result;
  -- 결과: ceiling_result = 4
  ```

### POWER

- 설명: 거듭제곱 값을 계산
- Oracle/SQL Server:
  ```sql
  SELECT POWER(2, 3) AS power_result FROM DUAL;
  -- 결과: power_result = 8
  ```

## 날짜 함수

### SYSDATE (Oracle) / GETDATE() (SQL Server)

- 설명: 현재 날짜와 시간을 반환
- Oracle:
  ```sql
  SELECT SYSDATE AS current_date FROM DUAL;
  ```
- SQL Server:
  ```sql
  SELECT GETDATE() AS current_date;
  ```

### ADD_MONTHS (Oracle) / DATEADD (SQL Server)

- 설명: 날짜에 개월 수를 더함
- Oracle:
  ```sql
  SELECT ADD_MONTHS(DATE '2023-01-01', 2) AS future_date FROM DUAL;
  -- 결과: future_date = '2023-03-01'
  ```
- SQL Server:
  ```sql
  SELECT DATEADD(MONTH, 2, '2023-01-01') AS future_date;
  -- 결과: future_date = '2023-03-01'
  ```

### MONTHS_BETWEEN (Oracle) / DATEDIFF (SQL Server)

- 설명: 두 날짜 사이의 개월 수를 계산
- Oracle:
  ```sql
  SELECT MONTHS_BETWEEN(DATE '2023-03-01', DATE '2023-01-01') AS month_diff FROM DUAL;
  -- 결과: month_diff = 2
  ```
- SQL Server:
  ```sql
  SELECT DATEDIFF(MONTH, '2023-01-01', '2023-03-01') AS month_diff;
  -- 결과: month_diff = 2
  ```

### LAST_DAY (Oracle)

- 설명: 해당 월의 마지막 날짜를 반환
- Oracle:
  ```sql
  SELECT LAST_DAY(DATE '2023-02-01') AS last_day FROM DUAL;
  -- 결과: last_day = '2023-02-28'
  ```

### TRUNC (Oracle) / DATETRUNC (SQL Server 2022+)

- 설명: 날짜의 시간 부분을 자름
- Oracle:
  ```sql
  SELECT TRUNC(SYSDATE) AS truncated_date FROM DUAL;
  ```
- SQL Server 2022+:
  ```sql
  SELECT DATETRUNC(DAY, GETDATE()) AS truncated_date;
  ```

## 변환 함수

### TO_CHAR (Oracle) / CONVERT (SQL Server)

- 설명: 다양한 데이터 타입을 문자열로 변환
- Oracle:
  ```sql
  SELECT TO_CHAR(SYSDATE, 'YYYY-MM-DD') AS formatted_date FROM DUAL;
  -- 결과: formatted_date = '2023-04-15' (현재 날짜에 따라 다름)
  ```
- SQL Server:
  ```sql
  SELECT CONVERT(VARCHAR, GETDATE(), 23) AS formatted_date;
  -- 결과: formatted_date = '2023-04-15' (현재 날짜에 따라 다름)
  ```

### TO_NUMBER (Oracle) / CAST (SQL Server)

- 설명: 문자열을 숫자로 변환
- Oracle:
  ```sql
  SELECT TO_NUMBER('123.45') AS converted_number FROM DUAL;
  -- 결과: converted_number = 123.45
  ```
- SQL Server:
  ```sql
  SELECT CAST('123.45' AS DECIMAL(5,2)) AS converted_number;
  -- 결과: converted_number = 123.45
  ```

### TO_DATE (Oracle) / CAST (SQL Server)

- 설명: 문자열을 날짜로 변환
- Oracle:
  ```sql
  SELECT TO_DATE('2023-04-15', 'YYYY-MM-DD') AS converted_date FROM DUAL;
  ```
- SQL Server:
  ```sql
  SELECT CAST('2023-04-15' AS DATE) AS converted_date;
  ```

## NULL 관련 함수

### NVL (Oracle) / ISNULL (SQL Server)

- 설명: NULL 값을 다른 값으로 대체
- Oracle:
  ```sql
  SELECT NVL(NULL, 'N/A') AS nvl_result FROM DUAL;
  -- 결과: nvl_result = 'N/A'
  ```
- SQL Server:
  ```sql
  SELECT ISNULL(NULL, 'N/A') AS isnull_result;
  -- 결과: isnull_result = 'N/A'
  ```

### COALESCE

- 설명: 여러 표현식 중 첫 번째 NULL이 아닌 값을 반환
- Oracle/SQL Server:
  ```sql
  SELECT COALESCE(NULL, NULL, 'Third', 'Fourth') AS coalesce_result FROM DUAL;
  -- 결과: coalesce_result = 'Third'
  ```

### NULLIF

- 설명: 두 표현식이 같으면 NULL을 반환, 다르면 첫 번째 표현식을 반환
- Oracle/SQL Server:
  ```sql
  SELECT NULLIF(5, 5) AS nullif_result1,
         NULLIF(5, 6) AS nullif_result2
  FROM DUAL;
  -- 결과: nullif_result1 = NULL, nullif_result2 = 5
  ```

## 집계 함수

### COUNT, SUM, AVG, MAX, MIN

- 설명: 행의 개수, 합계, 평균, 최대값, 최소값을 계산
- Oracle/SQL Server:
  ```sql
  SELECT
    COUNT(*) AS total_count,
    SUM(salary) AS total_salary,
    AVG(salary) AS avg_salary,
    MAX(salary) AS max_salary,
    MIN(salary) AS min_salary
  FROM employees;
  ```

### GROUP_CONCAT (Oracle) / STRING_AGG (SQL Server)

- 설명: 그룹 내의 문자열을 연결
- Oracle:
  ```sql
  SELECT LISTAGG(employee_name, ', ') WITHIN GROUP (ORDER BY employee_name) AS employee_list
  FROM employees;
  ```
- SQL Server:
  ```sql
  SELECT STRING_AGG(employee_name, ', ') WITHIN GROUP (ORDER BY employee_name) AS employee_list
  FROM employees;
  ```

## 윈도우 함수

### ROW_NUMBER, RANK, DENSE_RANK

- 설명: 결과 집합 내에서 행의 순위를 매김
- Oracle/SQL Server:
  ```sql
  SELECT
    employee_name,
    salary,
    ROW_NUMBER() OVER (ORDER BY salary DESC) AS row_num,
    RANK() OVER (ORDER BY salary DESC) AS rank,
    DENSE_RANK() OVER (ORDER BY salary DESC) AS dense_rank
  FROM employees;
  ```

### LAG, LEAD

- 설명: 현재 행을 기준으로 이전 행 또는 다음 행의 값을 참조
- Oracle/SQL Server:
  ```sql
  SELECT
    employee_name,
    hire_date,
    LAG(hire_date) OVER (ORDER BY hire_date) AS previous_hire_date,
    LEAD(hire_date) OVER (ORDER BY hire_date) AS next_hire_date
  FROM employees;
  ```

### FIRST_VALUE, LAST_VALUE

- 설명: 윈도우 프레임의 첫 번째 또는 마지막 값을 반환
- Oracle/SQL Server:
  ```sql
  SELECT
    employee_name,
    department,
    salary,
    FIRST_VALUE(salary) OVER (PARTITION BY department ORDER BY salary DESC) AS highest_salary,
    LAST_VALUE(salary) OVER (PARTITION BY department ORDER BY salary DESC
      ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) AS lowest_salary
  FROM employees;
  ```

### NTILE

- 설명: 결과 집합을 지정된 수의 그룹으로 균등하게 분할
- Oracle/SQL Server:
  ```sql
  SELECT
    employee_name,
    salary,
    NTILE(4) OVER (ORDER BY salary DESC) AS salary_quartile
  FROM employees;
  ```

### CUME_DIST

- 설명: 누적 분포 값을 계산 (0~1 사이의 값)
- Oracle/SQL Server:
  ```sql
  SELECT
    employee_name,
    salary,
    CUME_DIST() OVER (ORDER BY salary) AS cumulative_distribution
  FROM employees;
  ```

### PERCENT_RANK

- 설명: 백분율 순위를 계산 (0~1 사이의 값)
- Oracle/SQL Server:
  ```sql
  SELECT
    employee_name,
    salary,
    PERCENT_RANK() OVER (ORDER BY salary) AS percent_rank
  FROM employees;
  ```

## 기타 중요 함수

### CASE

- 설명: 조건에 따라 다른 결과를 반환
- Oracle/SQL Server:
  ```sql
  SELECT
    employee_name,
    salary,
    CASE
      WHEN salary < 30000 THEN 'Low'
      WHEN salary BETWEEN 30000 AND 50000 THEN 'Medium'
      ELSE 'High'
    END AS salary_category
  FROM employees;
  ```

### DECODE (Oracle) / IIF (SQL Server)

- 설명: 간단한 CASE 문과 유사한 기능
- Oracle:
  ```sql
  SELECT
    employee_name,
    DECODE(department, 'IT', 1.1, 'Sales', 1.2, 1.0) * salary AS adjusted_salary
  FROM employees;
  ```
- SQL Server:
  ```sql
  SELECT
    employee_name,
    IIF(department = 'IT', 1.1 * salary,
      IIF(department = 'Sales', 1.2 * salary, salary)) AS adjusted_salary
  FROM employees;
  ```

### PIVOT (SQL Server) / PIVOT 에뮬레이션 (Oracle)

- 설명: 행 데이터를 열 데이터로 변환
- SQL Server:
  ```sql
  SELECT *
  FROM (SELECT department, salary FROM employees) AS SourceTable
  PIVOT (
    AVG(salary)
    FOR department IN ([IT], [Sales], [HR])
  ) AS PivotTable;
  ```
- Oracle (에뮬레이션):
  ```sql
  SELECT
    AVG(CASE WHEN department = 'IT' THEN salary END) AS IT,
    AVG(CASE WHEN department = 'Sales' THEN salary END) AS Sales,
    AVG(CASE WHEN department = 'HR' THEN salary END) AS HR
  FROM employees;
  ```

### CONNECT BY (Oracle) / 재귀 CTE (SQL Server)

- 설명: 계층적 쿼리를 실행
- Oracle:
  ```sql
  SELECT employee_id, manager_id, LEVEL,
         SYS_CONNECT_BY_PATH(employee_name, '/') AS path
  FROM employees
  START WITH manager_id IS NULL
  CONNECT BY PRIOR employee_id = manager_id;
  ```
- SQL Server:
  ```sql
  WITH EmployeeHierarchy AS (
    SELECT employee_id, manager_id, employee_name, 0 AS LEVEL,
           CAST(employee_name AS VARCHAR(MAX)) AS path
    FROM employees
    WHERE manager_id IS NULL
    UNION ALL
    SELECT e.employee_id, e.manager_id, e.employee_name, eh.LEVEL + 1,
           CAST(eh.path + '/' + e.employee_name AS VARCHAR(MAX))
    FROM employees e
    INNER JOIN EmployeeHierarchy eh ON e.manager_id = eh.employee_id
  )
  SELECT * FROM EmployeeHierarchy;
  ```

### ROLLUP, CUBE (Oracle/SQL Server)

- 설명: 그룹화 집합을 생성
- Oracle/SQL Server:

  ```sql
  SELECT
    COALESCE(department, 'All Departments') AS department,
    COALESCE(job_title, 'All Jobs') AS job_title,
    SUM(salary) AS total_salary
  FROM employees
  GROUP BY ROLLUP(department, job_title);

  SELECT
    COALESCE(department, 'All Departments') AS department,
    COALESCE(job_title, 'All Jobs') AS job_title,
    SUM(salary) AS total_salary
  FROM employees
  GROUP BY CUBE(department, job_title);
  ```
