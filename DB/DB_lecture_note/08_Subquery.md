# Chapter 8: Subquery

---

## 📖 Course Overview

In this chapter, you will learn about subqueries, which are queries contained within other queries. Subqueries are powerful tools in SQL that allow you to solve complex data retrieval requirements step by step. This chapter covers various forms of subqueries including scalar subqueries, inline views, correlated subqueries, EXISTS, and performance comparisons with JOIN. 

| 이 장에서는 다른 쿼리 내에 포함된 쿼리인 서브쿼리(Subquery)를 학습합니다. 서브쿼리는 SQL의 강력한 도구로, 복잡한 데이터 검색 요구사항을 단계적으로 해결할 수 있게 합니다. 스칼라 서브쿼리, 인라인 뷰, 상관 서브쿼리, EXISTS 등 다양한 형태의 서브쿼리와 JOIN과의 성능 비교를 다룹니다.

---

## 📚 Part 1: Theoretical Learning

### What You'll Learn in This Section

- Concept and classification of subqueries | 서브쿼리의 개념과 분류
- Single-row and multi-row subqueries | 단일 행 서브쿼리와 다중 행 서브쿼리
- Scalar subqueries and inline views | 스칼라 서브쿼리와 인라인 뷰
- Concept and usage of correlated subqueries | 상관 서브쿼리의 개념과 활용
- Difference between EXISTS and IN | EXISTS와 IN의 차이점
- Subquery performance optimization | 서브쿼리 성능 최적화

---

### 8.1 Basic Concept of Subquery

A **subquery** is a SELECT statement contained within another query. | **서브쿼리**는 다른 쿼리 내에 포함된 SELECT 문입니다.

**Characteristics:**

- Written inside parentheses () | 괄호 () 안에 작성
- Often executed before the main query (inner query) | 메인 쿼리보다 먼저 실행되는 경우가 많음 (내부 쿼리)
- Results used by the main query | 결과를 메인 쿼리가 사용

**Usage Locations:**

```sql
SELECT ... (SELECT ... ) FROM ...    -- SELECT clause
FROM (SELECT ... ) AS alias_name     -- FROM clause
WHERE column IN (SELECT ... )        -- WHERE clause
```

---

### 8.2 Single-Row Subquery

A **single-row subquery** returns exactly one row. | **단일 행 서브쿼리**는 정확히 하나의 행을 반환하는 서브쿼리입니다.

**Syntax:**

```sql
SELECT * FROM employees
WHERE salary = (SELECT MAX(salary) FROM employees);
```

**Characteristics:**

- Comparison operators can be used (=, >, <, >=, <=, !=) | 비교 연산자 사용 가능 (=, >, <, >=, <=, !=)
- Usually includes aggregate functions | 집계함수를 주로 포함
- Better performance than multi-row subqueries | 다중 행 서브쿼리보다 성능이 좋음

**Example:**

```sql
-- Find employees with salary higher than average
SELECT * FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);

-- Find employees with same salary as maximum in department 1
SELECT * FROM employees
WHERE salary = (SELECT MAX(salary) FROM employees WHERE dept_id = 1);
```

---

### 8.3 Multi-Row Subquery

A **multi-row subquery** returns one or more rows. | **다중 행 서브쿼리**는 하나 이상의 행을 반환하는 서브쿼리입니다.

**Required Operators:**

- IN: Matches any value in subquery result | 서브쿼리 결과 중 하나와 일치
- NOT IN: Does not match any value in subquery result | 서브쿼리 결과 중 하나와도 일치하지 않음
- ANY: Compares with any value in subquery (=ANY is same as IN) | 서브쿼리 결과 중 하나와 비교 (=ANY는 IN과 같음)
- ALL: Compares with all values in subquery result | 서브쿼리 결과의 모든 값과 비교

**Example:**

```sql
-- Find all employees with same salary as employees in department 1
SELECT * FROM employees
WHERE salary IN (SELECT salary FROM employees WHERE dept_id = 1);

-- Find employees with salary higher than any department average
SELECT * FROM employees
WHERE salary > ANY (SELECT AVG(salary) FROM employees GROUP BY dept_id);
```

---

### 8.4 Scalar Subquery

A **scalar subquery** in the SELECT clause returns a single value. | **스칼라 서브쿼리**는 SELECT 절에서 단일 값을 반환하는 서브쿼리입니다.

**Syntax:**

```sql
SELECT column, (SELECT ... FROM ...) AS alias
FROM table_name;
```

**Characteristics:**

- Can be executed for each row | 각 행마다 실행될 수 있음
- Performance may decrease if it's a correlated subquery | 상관 서브쿼리일 경우 성능이 저하될 수 있음
- Can be replaced with JOIN for better performance | JOIN으로 대체하면 성능 향상 가능

**Example:**

```sql
SELECT employee_id, name, 
       (SELECT department_name FROM departments d 
        WHERE d.dept_id = e.dept_id) AS dept_name
FROM employees e;
```

---

### 8.5 Inline View

An **inline view** is a subquery used in the FROM clause. | **인라인 뷰**는 FROM 절에 사용되는 서브쿼리입니다.

**Syntax:**

```sql
SELECT * FROM (
  SELECT column FROM table_name WHERE condition
) AS alias_name;
```

**Characteristics:**

- Acts like a temporary table | 임시 테이블처럼 동작
- Alias is required | 별칭 필수
- Handles complex queries step by step | 복잡한 쿼리를 단계적으로 처리

**Example:**

```sql
-- Find department average salary, then find employees above average
SELECT e.name, dept_avg.avg_salary
FROM employees e
JOIN (SELECT dept_id, AVG(salary) AS avg_salary 
      FROM employees GROUP BY dept_id) AS dept_avg
ON e.dept_id = dept_avg.dept_id
WHERE e.salary >= dept_avg.avg_salary;
```

---

### 8.6 Correlated Subquery

A **correlated subquery** references values from the outer query. | **상관 서브쿼리**는 외부 쿼리의 값을 참조하는 서브쿼리입니다.

**Characteristics:**

- Subquery executes for each row of outer query | 외부 쿼리의 각 행에 대해 서브쿼리가 실행
- Performance may decrease | 성능이 저하될 수 있음
- Logic can be complex | 로직이 복잡할 수 있음

**Example:**

```sql
-- Check if employee's salary is higher than department average
SELECT name, salary
FROM employees e1
WHERE salary > (SELECT AVG(salary) FROM employees e2 
                WHERE e2.dept_id = e1.dept_id);
```

---

### 8.7 EXISTS and NOT EXISTS

**EXISTS** checks whether subquery returns any rows. | **EXISTS**는 서브쿼리가 행을 반환하는지 확인합니다.

**Syntax:**

```sql
SELECT * FROM table1
WHERE EXISTS (SELECT 1 FROM table2 WHERE condition);
```

**Characteristics:**

- Checks only existence, not actual data | 실제 데이터보다 존재 여부만 확인
- Can have better performance than IN | IN보다 성능이 좋을 수 있음
- Different NULL handling than IN | NULL 값 처리가 다름

**Example:**

```sql
-- Find customers with at least one order
SELECT * FROM customers c
WHERE EXISTS (SELECT 1 FROM orders o WHERE o.customer_id = c.customer_id);

-- Find customers without any orders
SELECT * FROM customers c
WHERE NOT EXISTS (SELECT 1 FROM orders o WHERE o.customer_id = c.customer_id);
```

---

### 8.8 Subquery vs JOIN

The same result can be achieved using subqueries or JOINs. | 동일한 결과를 서브쿼리와 JOIN으로 구현할 수 있습니다.

**Subquery Method:**

```sql
SELECT * FROM employees
WHERE dept_id IN (SELECT dept_id FROM departments WHERE location = 'Seoul');
```

**JOIN Method:**

```sql
SELECT e.* FROM employees e
JOIN departments d ON e.dept_id = d.dept_id
WHERE d.location = 'Seoul';
```

**Performance Considerations:**

- Generally JOINs perform better for large datasets | 일반적으로 큰 데이터셋에서는 JOIN이 성능이 좋음
- Subqueries can be more readable for complex logic | 복잡한 로직에서는 서브쿼리가 더 읽기 쉬울 수 있음
- Use EXPLAIN to analyze performance | EXPLAIN으로 성능을 분석하세요

---

## 📚 Part 2: Sample Data

### employees Table

```sql
CREATE TABLE employees (
    employee_id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(50) NOT NULL,
    dept_id INT,
    salary DECIMAL(10, 2)
);

INSERT INTO employees VALUES
(1, 'Kim Chulsu', 1, 5000000),
(2, 'Lee Younghee', 1, 4000000),
(3, 'Park Minjun', 2, 4500000),
(4, 'Choi Sunsin', 2, 3500000),
(5, 'Kang Gamchan', 3, 4200000),
(6, 'Lee Sunsin', 3, 3800000);
```

### departments Table

```sql
CREATE TABLE departments (
    dept_id INT PRIMARY KEY AUTO_INCREMENT,
    department_name VARCHAR(50) NOT NULL
);

INSERT INTO departments VALUES
(1, 'Sales'),
(2, 'Technology'),
(3, 'HR');
```

---

## 💻 Part 3: Practice

### What You'll Learn in This Section

- Writing various forms of subqueries | 다양한 서브쿼리 형태의 작성
- Subquery optimization techniques | 서브쿼리 최적화 기법
- Step-by-step resolution of complex queries | 복잡한 쿼리의 단계적 해결
- Performance comparison and selection | 성능 비교 및 선택

---

### 8-1. Single-Row Subquery - Maximum Value

Query the employee with the highest salary among all employees. | 전체 직원 중 최고 급여를 받는 직원을 조회하세요.

```sql
SELECT * FROM employees
WHERE salary = (SELECT MAX(salary) FROM employees);
```

---

### 8-2. Single-Row Subquery - Average Value

Query employees with salary higher than average. | 평균 급여보다 높은 급여를 받는 직원을 조회하세요.

```sql
SELECT name, salary FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);
```

---

### 8-3. Single-Row Subquery - Specific Department

Query all employees with same salary as maximum in department 1. | 부서 1의 최대 급여와 같은 급여를 받는 모든 직원을 조회하세요.

```sql
SELECT * FROM employees
WHERE salary = (SELECT MAX(salary) FROM employees WHERE dept_id = 1);
```

---

### 8-4. Multi-Row Subquery - IN

Query employees with same salary as employees in department 1 or 2. | 부서 1 또는 부서 2의 급여와 같은 급여를 받는 직원을 조회하세요.

```sql
SELECT * FROM employees
WHERE salary IN (SELECT salary FROM employees WHERE dept_id IN (1, 2));
```

---

### 8-5. Multi-Row Subquery - NOT IN

Query employees with different salary than employees in department 1. | 부서 1의 급여와 다른 급여를 받는 직원을 조회하세요.

```sql
SELECT * FROM employees
WHERE salary NOT IN (SELECT salary FROM employees WHERE dept_id = 1);
```

---

### 8-6. Multi-Row Subquery - ANY

Query employees with salary higher than any department average. | 각 부서의 평균 급여보다 높은 급여를 받는 직원을 조회하세요.

```sql
SELECT name, salary, dept_id FROM employees
WHERE salary > ANY (SELECT AVG(salary) FROM employees GROUP BY dept_id);
```

---

### 8-7. Multi-Row Subquery - ALL

Query employees with salary lower than maximum salary of all departments. | 모든 부서의 최대 급여보다 낮은 급여를 받는 직원을 조회하세요.

```sql
SELECT name, salary FROM employees
WHERE salary < ALL (SELECT MAX(salary) FROM employees GROUP BY dept_id);
```

---

### 8-8. Scalar Subquery

Display employee name with the average salary of their department. | 각 직원의 이름과 함께 해당 부서의 평균 급여를 표시하세요.

```sql
SELECT name, salary,
       (SELECT AVG(salary) FROM employees e2 WHERE e2.dept_id = e1.dept_id) AS dept_avg
FROM employees e1;
```

---

### 8-9. Scalar Subquery - Department Name

Query employee name and department name. | 각 직원의 이름과 부서명을 조회하세요.

```sql
SELECT name,
       (SELECT department_name FROM departments WHERE dept_id = e.dept_id) AS dept_name
FROM employees e;
```

---

### 8-10. Inline View - Basics

Query department average salary and then query the result again. | 부서별 평균 급여를 조회하고, 그 결과를 다시 쿼리하세요.

```sql
SELECT dept_id, dept_avg FROM (
    SELECT dept_id, AVG(salary) AS dept_avg
    FROM employees
    GROUP BY dept_id
) AS dept_salary
WHERE dept_avg > 4000000;
```

---

### 8-11 through 8-40: Advanced Subquery Techniques

The practice includes 30 more comprehensive subquery examples covering:

- Inline views with JOINs | 인라인 뷰와 JOIN
- Correlated subqueries | 상관 서브쿼리
- EXISTS and NOT EXISTS | EXISTS와 NOT EXISTS
- Subqueries in UPDATE/DELETE | UPDATE/DELETE의 서브쿼리
- Performance optimization | 성능 최적화
- Complex nested subqueries | 복잡한 중첩 서브쿼리

---

## 📝 Part 4: Assignment Instructions

### Theoretical Assignments

**Assignment 1**: Explain the difference between single-row and multi-row subqueries. When should each type be used? Provide examples of both. | 단일 행 서브쿼리와 다중 행 서브쿼리의 차이를 설명하세요. 각각이 언제 사용되어야 하는지 설명하고 예시를 제시하세요.

**Assignment 2**: Explain scalar subqueries and inline views. When is it better to replace them with JOINs? | 스칼라 서브쿼리와 인라인 뷰를 설명하세요. JOIN으로 대체하는 것이 더 나은 경우는 언제인지 설명하세요.

**Assignment 3**: Explain correlated subqueries with examples. Discuss performance implications. | 상관 서브쿼리를 예시와 함께 설명하세요. 성능 영향에 대해 논의하세요.

**Assignment 4**: Explain the difference between EXISTS and IN. When should each be used? | EXISTS와 IN의 차이를 설명하세요. 각각이 언제 사용되어야 하는지 설명하세요.

**Assignment 5**: Compare subqueries and JOINs for the same task. Analyze performance differences and provide recommendations. | 같은 작업을 서브쿼리와 JOIN으로 구현하고 비교하세요. 성능 차이를 분석하고 권장사항을 제시하세요.

Submission Format: Word or PDF document (2-3 pages)

---

### Practical Assignments

**Assignment 1**: Write subqueries to answer these questions: highest salary employee, employees above average salary, employees from each department above their department average. | 다음 질문에 대한 서브쿼리를 작성하세요: 최고 급여 직원, 평균 이상 급여 직원, 각 부서의 평균 이상 직원.

**Assignment 2**: Write inline views for complex queries such as department-based aggregation followed by filtering, ranking within groups. | 부서별 집계 후 필터링, 그룹 내 순위 매기기 등 복잡한 쿼리에 대한 인라인 뷰를 작성하세요.

**Assignment 3**: Write correlated subqueries to solve employee comparison within departments, find employees above their department average. | 부서 내 직원 비교, 부서 평균 이상 직원 찾기 등의 상관 서브쿼리를 작성하세요.

**Assignment 4**: Rewrite 5 JOIN queries from Chapter 5-6 using subqueries. Analyze which performs better. | Chapter 5-6의 JOIN 쿼리 5개를 서브쿼리로 다시 작성하세요. 어느 것이 더 성능이 좋은지 분석하세요.

**Assignment 5**: Execute all Practice 8-1 to 8-40 queries and attach screenshots. Additionally, create 5 creative subqueries for business problems and explain their purpose. | 실습 8-1부터 8-40까지의 모든 쿼리를 실행하고 스크린샷을 첨부하세요. 추가로 비즈니스 문제에 대한 창의적인 서브쿼리 5개를 작성하고 목적을 설명하세요.

Submission Format: SQL file (Ch8_Subquery_[StudentID].sql) and result screenshots

---

Thank you for your hard work.

Prof. Cho Jeong-Hyun (peterchokr@gmail.com). Yeungnam University College
