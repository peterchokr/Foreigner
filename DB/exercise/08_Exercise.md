# Ch8 Subquery - Exercise Problems

Dear students! After completing Chapter 8, please solve the following exercises to verify your understanding.
Check the difficulty level of each section and study step by step.

---

## 📌 Learning Objectives Verification

After completing Chapter 8, you should understand:

- Concept and necessity of subquery
- Subquery in SELECT clause (scalar subquery)
- Subquery in FROM clause (inline view)
- Subquery in WHERE clause (correlated subquery)
- IN, EXISTS operators
- Subquery vs JOIN comparison

---

# Multiple Choice Questions (10 Questions)

## Beginner Level (5 Questions) - Basic Concept Verification

**Question 1** Basic definition of subquery?

- ① Query executed before main query
- ② Query contained within another query
- ③ Query joining multiple tables
- ④ Query using GROUP BY

---

**Question 2** Where is scalar subquery used?

- ① Returns single value in SELECT clause
- ② Used as table in FROM clause
- ③ Comparison condition in WHERE clause
- ④ Sorting in ORDER BY clause

---

**Question 3** Invalid form of subquery result?

- ① Single row, single column (scalar value)
- ② Single row, multiple columns
- ③ Multiple rows, single column (list)
- ④ Multiple rows, multiple columns

---

**Question 4** Purpose of IN operator?

- ① Search when subquery returns multiple rows
- ② Check for NULL values
- ③ Range search
- ④ Pattern matching

---

**Question 5** Characteristic of EXISTS operator?

- ① Check if subquery result exists
- ② Check number of rows in subquery result
- ③ Compare subquery result values
- ④ Sort subquery result

---

## Intermediate Level (3 Questions) - Concept Application

**Question 6** Correctly used subqueries?

```sql
① SELECT name, (SELECT MAX(salary) FROM employees) AS max_salary
   FROM employees;

② SELECT name, salary
   FROM employees
   WHERE salary > (SELECT AVG(salary) FROM employees);

③ SELECT e.name
   FROM employees e
   WHERE e.dept_id IN (SELECT dept_id FROM departments);
```

- ① Correct (SELECT clause scalar subquery)
- ② Correct (WHERE clause subquery)
- ③ Correct (WHERE clause IN subquery)
- ④ All ①②③ correct

---

**Question 7** Why use subquery in FROM clause (inline view)?

- ① Perform complex calculation first then query results
- ② Improve performance
- ③ Simplify code
- ④ Join multiple tables

---

**Question 8** Characteristic of correlated subquery?

- ① References value from outer query
- ② Executes for each row
- ③ Can be converted to JOIN
- ④ All correct

---

## Advanced Level (2 Questions) - Critical Thinking

**Question 9** Performance difference between these queries?

```
Query A: Subquery
SELECT name FROM employees
WHERE dept_id IN (SELECT dept_id FROM departments);

Query B: JOIN
SELECT DISTINCT e.name FROM employees e
JOIN departments d ON e.dept_id = d.dept_id;
```

- ① Query A always faster
- ② Query B always faster
- ③ Depends on situation (data volume, indexes)
- ④ Performance exactly same

---

**Question 10** When to choose subquery over JOIN?

- ① When referencing same table multiple times
- ② When only simple values needed from another table
- ③ When using aggregate result as condition
- ④ Cases ②③

---

# Short Answer Questions (5 Questions)

## Beginner Level (3 Questions)

**Question 11** Define subquery and explain its necessity.

---

**Question 12** Explain 3 locations where subquery can be used and their characteristics.

---

**Question 13** Explain difference between IN and EXISTS and when to use each.

---

## Intermediate Level (1 Question)

**Question 14** Explain concept of correlated subquery, how it works, and difference from regular subquery.

---

## Advanced Level (1 Question)

**Question 15** Explain cases where subquery solves problem but JOIN can't and vice versa, analyzing advantages/disadvantages.

---

# Practical Problems (5 Questions)

## Beginner Level (2 Questions)

**Question 16** Execute following SQL and provide result screenshot.

```sql
CREATE DATABASE ch8_subquery CHARACTER SET utf8mb4;
USE ch8_subquery;

CREATE TABLE employees (
    employee_id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(30),
    salary INT,
    dept_id INT
);

CREATE TABLE departments (
    dept_id INT PRIMARY KEY AUTO_INCREMENT,
    dept_name VARCHAR(30)
);

INSERT INTO employees VALUES
(1, 'Kim Chulsu', 5000000, 1),
(2, 'Lee Younghee', 4000000, 1),
(3, 'Park Minjun', 4500000, 2),
(4, 'Choi Sunsin', 3500000, 2),
(5, 'Kang Gamchan', 4200000, 3);

INSERT INTO departments VALUES
(1, 'Sales'),
(2, 'Technology'),
(3, 'HR');

SELECT * FROM employees;
SELECT * FROM departments;
```

Submission: Screenshot showing both employees and departments tables

---

**Question 17** Perform following on employees table and verify results.

```sql
-- 1. Employees earning above average salary
SELECT name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);

-- 2. Average salary
SELECT AVG(salary) AS avg_salary FROM employees;
```

Submission: Screenshot showing both query results

---

## Intermediate Level (2 Questions)

**Question 18** Use subqueries to query following.

```sql
-- 1. Subquery with IN
SELECT name FROM employees
WHERE dept_id IN (SELECT dept_id FROM departments 
                  WHERE dept_name IN ('Sales', 'Technology'));

-- 2. Subquery in FROM (inline view)
SELECT * FROM (
    SELECT name, salary, dept_id
    FROM employees
    WHERE salary >= 4000000
) AS high_earners;
```

Submission: Screenshot showing both query results

---

**Question 19** Use correlated subquery to query following.

```sql
-- 1. Employees earning above department average
SELECT e.name, e.salary, e.dept_id
FROM employees e
WHERE e.salary > (
    SELECT AVG(salary) FROM employees
    WHERE dept_id = e.dept_id
);

-- 2. Correlated subquery with EXISTS
SELECT d.dept_name
FROM departments d
WHERE EXISTS (
    SELECT 1 FROM employees e
    WHERE e.dept_id = d.dept_id
);
```

Submission: Screenshot showing both query results

---

## Advanced Level (1 Question)

**Question 20** Write and execute complex subqueries.

```
Requirements:
1. Get department average salary and compare each employee

2. Use nested subqueries for complex conditions

3. Compare subquery vs JOIN for same result

4. Write 2+ free-form queries with aggregate + subquery

Submission:
   - SQL code for each query
   - Screenshot of each query result
```

---

---

# 📋 Answer Key and Model Answers

## Multiple Choice Answers (10 Questions)

| Question | Answer | Explanation                                        |
| :------: | :----: | :------------------------------------------------- |
|    1    |   ②   | Subquery is query contained within another query   |
|    2    |   ①   | Scalar subquery returns single value in SELECT     |
|    3    |   ④   | Multiple rows AND multiple columns invalid         |
|    4    |   ①   | IN used when subquery returns multiple rows        |
|    5    |   ①   | EXISTS checks if subquery result exists            |
|    6    |   ④   | All ①②③ are correctly used subqueries           |
|    7    |   ①   | FROM subquery for complex calculation then requery |
|    8    |   ④   | All characteristics correct                        |
|    9    |   ③   | Depends on data volume and indexes                 |
|    10    |   ④   | Cases ②③ select subquery approach                |

---

## Short Answer Model Answers (5 Questions)

### Question 11: Subquery Definition and Necessity

**Model Answer**:

```
Definition:
- Query contained within another query
- Also called inner query or partial query
- Works with outer (main) query

Necessity:
1. Simplify complex conditions
2. Use aggregate results as conditions
3. Reuse intermediate results
4. Easier to read complex queries

Example: "Find employees earning above average"
- Inner query: Calculate average
- Outer query: Compare against average
```

---

### Question 12: Three Subquery Locations

**Model Answer**:

```
1. SELECT Clause - Scalar Subquery
   - Returns single row, single column
   - Executes for each outer row
   SELECT name, (SELECT MAX(salary) ...) FROM ...

2. FROM Clause - Inline View
   - Uses subquery result as table
   - Complex calculation before requerying
   FROM (SELECT ... WHERE ...) AS subquery

3. WHERE Clause - Conditional Subquery
   - Used in conditions
   - Works with IN, EXISTS
   WHERE salary > (SELECT AVG(salary) ...)
```

---

### Question 13: IN vs EXISTS

**Model Answer**:

```
IN operator:
- Subquery returns multiple values
- Checks if value matches any in list
- Example: WHERE dept_id IN (1, 2, 3)

EXISTS operator:
- Checks if subquery result exists
- Returns TRUE/FALSE
- Ignores NULL

When to use IN:
SELECT name FROM employees
WHERE dept_id IN (SELECT dept_id FROM departments);

When to use EXISTS:
SELECT d.name FROM departments d
WHERE EXISTS (SELECT 1 FROM employees e 
              WHERE e.dept_id = d.id);

Performance:
- EXISTS typically faster (checks existence only)
- IN compares against full result set
```

---

### Question 14: Correlated Subquery Explanation

**Model Answer**:

```
Definition:
- Outer query values referenced in subquery
- Subquery executes for each outer row

How it works:
1. Outer query selects row
2. Pass that row values to subquery
3. Subquery executes using those values
4. Return result to outer query
5. Move to next outer row
6. Repeat

Example:
SELECT e.name FROM employees e
WHERE e.salary > (
    SELECT AVG(salary) FROM employees
    WHERE dept_id = e.dept_id  -- References outer e.dept_id
);

Difference from regular subquery:
- Regular: Executes once, returns static result
- Correlated: Executes once per outer row
- Performance: Correlated slower (more executions)
```

---

### Question 15: Subquery vs JOIN

**Model Answer**:

```
Subquery only:
- Aggregate results in conditions
- Example: WHERE salary > (SELECT AVG(...))
- Some correlated subqueries harder to convert

JOIN only:
- Self-join (same table with itself)
- Example: emp and manager from same table
- Multiple table simultaneous joining

Both possible:
SELECT e.name FROM employees e
WHERE dept_id IN (1, 2, 3);  -- Subquery

SELECT DISTINCT e.name FROM employees e
WHERE e.dept_id IN (1, 2, 3);  -- JOIN approach possible

Advantages:
Subquery:
- More readable in some cases
- Natural for aggregate conditions

JOIN:
- Generally better performance
- Database optimizer handles well
- Can be complex to read
```

---

## Practical Problems Model Answers (5 Questions)

### Question 16: Create Tables

**Completion Criteria**:
✅ ch8_subquery database created
✅ employees table: 5 employees
✅ departments table: 3 departments

---

### Question 17: Scalar Subquery Results

**Expected Results**:

```
Average salary: 4,240,000

Employees above average:
name        | salary
Kim Chulsu  | 5,000,000
Park Minjun | 4,500,000
Kang Gamchan| 4,200,000
```

---

### Question 18: IN and FROM Subqueries

**Model Results**:

```
Query 1 - IN subquery:
name
Kim Chulsu
Lee Younghee
Park Minjun
Choi Sunsin

Query 2 - FROM subquery (inline view):
High earners >= 4,000,000:
name         | salary
Kim Chulsu   | 5,000,000
Lee Younghee | 4,000,000
Park Minjun  | 4,500,000
Kang Gamchan | 4,200,000
```

---

### Question 19: Correlated Subquery Results

**Model Answer**:

```sql
-- Query 1: Above department average
Result:
name       | salary | dept_id
Kim Chulsu | 5,000,000 | 1 (Sales avg: 4,500,000)
Park Minjun| 4,500,000 | 2 (Tech avg: 4,000,000)

-- Query 2: Departments with employees
Result:
dept_name
Sales
Technology
HR
```

---

### Question 20: Complex Subqueries

**Model Answer**:

```sql
-- 1. Department average comparison
SELECT e.name, e.salary,
       (SELECT AVG(salary) FROM employees 
        WHERE dept_id = e.dept_id) AS dept_avg
FROM employees e;

-- 2. Nested subquery
SELECT name FROM employees
WHERE salary > (
    SELECT AVG(salary) FROM employees
    WHERE dept_id IN (
        SELECT dept_id FROM departments
        WHERE dept_name IN ('Sales', 'Technology')
    )
);

-- 3. Subquery approach
SELECT DISTINCT e.name FROM employees e
WHERE e.dept_id IN (
    SELECT dept_id FROM departments
    WHERE dept_name IN ('Sales', 'Technology')
);

-- 3. JOIN approach (same result)
SELECT DISTINCT e.name FROM employees e
JOIN departments d ON e.dept_id = d.dept_id
WHERE d.dept_name IN ('Sales', 'Technology');

-- 4. Aggregate + Subquery
SELECT dept_id, COUNT(*) AS cnt
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees)
GROUP BY dept_id;

-- 5. Multiple conditions
SELECT name FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees)
AND dept_id IN (
    SELECT dept_id FROM departments
    WHERE dept_name NOT IN ('HR')
);
```

---

Prof. Jeong-Hyun Cho (peterchokr@gmail.com)
Yeungnam University College
