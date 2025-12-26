# Ch10 Views and Stored Procedures - Exercise Problems

Dear students! After completing Chapter 10, please solve the following exercises to verify your understanding.
Check the difficulty level of each section and study step by step.

---

## 📌 Learning Objectives Verification

After completing Chapter 10, you should understand:

- View concept and creation
- View utilization and advantages/disadvantages
- View modification and deletion
- Stored Procedure concept and syntax
- Stored Procedure parameters (IN, OUT, INOUT)
- Stored Procedure execution and management

---

# Multiple Choice Questions (10 Questions)

## Beginner Level (5 Questions) - Basic Concept Verification

**Question 1** Most important characteristic of View?

- ① Stores actual data
- ② Virtual table that doesn't store actual data
- ③ Always faster than tables
- ④ Modifies original table

---

**Question 2** Basic syntax to create View?

- ① CREATE TABLE view_name AS SELECT ...;
- ② CREATE VIEW view_name AS SELECT ...;
- ③ CREATE VIEW view_name FROM SELECT ...;
- ④ MAKE VIEW view_name AS SELECT ...;

---

**Question 3** Correct definition of Stored Procedure?

- ① Reusable SQL routine stored in database
- ② SQL statement executed only once
- ③ Query that cannot include conditional statements
- ④ Can only be executed from client application

---

**Question 4** Role of IN parameter in Stored Procedure?

- ① Receive input values only
- ② Return output values only
- ③ Both input and output
- ④ Execute without parameters

---

**Question 5** How to execute Stored Procedure?

- ① SELECT procedure_name;
- ② RUN procedure_name;
- ③ CALL procedure_name(parameters);
- ④ EXECUTE procedure_name;

---

## Intermediate Level (3 Questions) - Concept Application

**Question 6** Condition for Updatable View that is NOT correct?

- ① Based on single table
- ② GROUP BY allowed
- ③ No JOIN included
- ④ No DISTINCT included

---

**Question 7** Example of using OUT parameter in Stored Procedure?

```sql
① CREATE PROCEDURE GetCount (IN dept_id INT)
② CREATE PROCEDURE GetCount (OUT count INT)
③ CREATE PROCEDURE GetCount (INOUT salary DECIMAL)
```

- ① Input only
- ② Output only (uses OUT)
- ③ Both input and output
- ④ Both ① and ② possible

---

**Question 8** Correct syntax to delete View?

- ① DELETE VIEW view_name;
- ② DROP VIEW view_name;
- ③ REMOVE VIEW view_name;
- ④ DROP TABLE view_name;

---

## Advanced Level (2 Questions) - Critical Thinking

**Question 9** Most important reason to use View?

- ① Always better performance
- ② Provides data security and abstraction
- ③ Saves storage space
- ④ UPDATE possible in all operations

---

**Question 10** Biggest difference between View and Stored Procedure?

- ① View for queries only, Procedure for logic
- ② Procedure for queries only, View for data modification
- ③ Both have same functionality
- ④ View faster, Procedure slower

---

# Short Answer Questions (5 Questions)

## Beginner Level (3 Questions)

**Question 11** Define View and explain why Views are needed.

---

**Question 12** Explain 3 main use cases for Views.

---

**Question 13** Define Stored Procedure and explain differences between IN, OUT, INOUT parameters.

---

## Intermediate Level (1 Question)

**Question 14** Explain conditions for Updatable View and provide examples of non-updatable Views.

---

## Advanced Level (1 Question)

**Question 15** Compare and analyze differences, advantages and disadvantages of View vs Stored Procedure.

---

# Practical Problems (5 Questions)

## Beginner Level (2 Questions)

**Question 16** Execute following SQL and provide result screenshot.

```sql
CREATE DATABASE ch10_view_procedure CHARACTER SET utf8mb4;
USE ch10_view_procedure;

CREATE TABLE employees (
    employee_id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(30),
    dept_id INT,
    salary INT,
    hire_date DATE
);

INSERT INTO employees VALUES
(1, 'Kim Chulsu', 1, 5000000, '2020-01-15'),
(2, 'Lee Younghee', 1, 4000000, '2020-06-20'),
(3, 'Park Minjun', 2, 4500000, '2019-03-10');

SELECT * FROM employees;
```

Submission: Screenshot showing employees table with 3 records

---

**Question 17** Create and query Views based on employees table.

```sql
-- 1. Simple View: Employee names and salaries
CREATE VIEW employee_salary_view AS
SELECT name, salary FROM employees;

SELECT * FROM employee_salary_view;

-- 2. Conditional View: Employees with salary >= 4000000
CREATE VIEW high_salary_view AS
SELECT name, salary FROM employees WHERE salary >= 4000000;

SELECT * FROM high_salary_view;
```

Submission: Screenshot showing results of both Views

---

## Intermediate Level (2 Questions)

**Question 18** Perform aggregation and modification using Views.

```sql
-- 1. Aggregation View: Employee count and average salary by department
CREATE VIEW dept_summary_view AS
SELECT dept_id, COUNT(*) AS emp_count, AVG(salary) AS avg_salary
FROM employees
GROUP BY dept_id;

SELECT * FROM dept_summary_view;

-- 2. Modify through View (updatable View)
CREATE VIEW emp_update_view AS
SELECT employee_id, name, salary FROM employees;

UPDATE emp_update_view SET salary = 5500000 WHERE employee_id = 1;
SELECT * FROM emp_update_view;
```

Submission: Screenshot showing aggregation View and updated results

---

**Question 19** Create and execute Stored Procedures.

```sql
-- 1. Procedure with IN parameter
CREATE PROCEDURE GetEmployeeInfo (IN emp_id INT)
BEGIN
  SELECT name, salary FROM employees WHERE employee_id = emp_id;
END;

CALL GetEmployeeInfo(1);

-- 2. Procedure with OUT parameter
CREATE PROCEDURE GetEmployeeCount (OUT count INT)
BEGIN
  SELECT COUNT(*) INTO count FROM employees;
END;

CALL GetEmployeeCount(@emp_count);
SELECT @emp_count AS employee_count;

-- 3. Procedure with conditional logic
CREATE PROCEDURE CheckSalary (IN emp_id INT)
BEGIN
  DECLARE emp_salary INT;
  SELECT salary INTO emp_salary FROM employees WHERE employee_id = emp_id;
  
  IF emp_salary > 4500000 THEN
    SELECT CONCAT('High salary: ', emp_salary);
  ELSE
    SELECT CONCAT('Regular salary: ', emp_salary);
  END IF;
END;

CALL CheckSalary(1);
```

Submission: Screenshot showing execution results of 3 procedures

---

## Advanced Level (1 Question)

**Question 20** Write and execute complex Stored Procedures.

```
Requirements:
1. Salary raise procedure
   - Raise salary by percentage for each department
   - Apply upper limit if exceeded
   - Return result using input/output parameters

2. Complex logic procedure
   - Determine salary grade using IF-ELSEIF-ELSE
   - Grade-based processing (A: bonus, B: normal, C: promotion pending)

3. Data validation procedure
   - Verify if employee ID exists
   - Return info if exists, error message if not

4. Procedure with WHILE loop
   - Batch process multiple employees
   - Process each employee in loop

Submission:
   - SQL code for each procedure
   - Execution result screenshot for each
   - Before/after data comparison
```

---

---

# 📋 Answer Key and Model Answers

## Multiple Choice Answers (10 Questions)

| Question | Answer | Explanation                                      |
| :------: | :----: | :----------------------------------------------- |
|    1    |   ②   | View is virtual table, doesn't store actual data |
|    2    |   ②   | CREATE VIEW creates View                         |
|    3    |   ①   | Stored Procedure is SQL routine stored in DB     |
|    4    |   ①   | IN is input parameter only                       |
|    5    |   ③   | CALL executes procedure                          |
|    6    |   ②   | GROUP BY makes View non-updatable                |
|    7    |   ②   | OUT is output-only parameter                     |
|    8    |   ②   | DROP VIEW deletes View                           |
|    9    |   ②   | View's purpose is security and abstraction       |
|    10    |   ①   | View for queries, Procedure for logic            |

---

## Short Answer Model Answers (5 Questions)

### Question 11: View Definition and Necessity

**Model Answer**:

```
Definition:
- Virtual table based on one or more tables
- Doesn't store actual data (logical abstraction)
- Defined by SELECT query

Necessity:
1. Simplify complex queries
   - Encapsulate complex JOINs and GROUP BYs
   - Users execute simple SELECT

2. Data security
   - Show only specific columns (exclude salary)
   - Control access to sensitive data

3. Data abstraction
   - Maintain compatibility if table structure changes
   - Minimize user query modifications
```

---

### Question 12: Three View Use Cases

**Model Answer**:

```
1. Simplify complex queries
   CREATE VIEW sales_summary AS
   SELECT p.name, COUNT(*) AS cnt, SUM(s.qty) AS total
   FROM products p
   JOIN sales s ON p.id = s.prod_id
   GROUP BY p.id;
   
   Users execute: SELECT * FROM sales_summary;

2. Data security
   CREATE VIEW emp_public AS
   SELECT emp_id, name, dept_id FROM employees;
   -- Salary, ID numbers excluded

3. Data abstraction
   CREATE VIEW active_employees AS
   SELECT * FROM employees WHERE termination_date IS NULL;
   -- Automatically exclude terminated employees
```

---

### Question 13: Stored Procedure and Parameters

**Model Answer**:

```
Definition:
- Reusable SQL routine stored in database
- Can include conditional statements, loops
- Called using CALL

Parameters:

1. IN (Input parameter)
   - Pass value to procedure
   - Read-only
   CREATE PROCEDURE get_emp (IN emp_id INT)

2. OUT (Output parameter)
   - Return result from procedure
   - Write-only
   CREATE PROCEDURE count_emp (OUT cnt INT)
   SELECT COUNT(*) INTO cnt FROM employees;
   
   Call: CALL count_emp(@c); SELECT @c;

3. INOUT (Input/Output parameter)
   - Receive value and return modified value
   - Read and write
   CREATE PROCEDURE adjust_salary (INOUT sal DECIMAL)
   SET sal = sal * 1.1;
```

---

### Question 14: Updatable View Conditions

**Model Answer**:

```
Conditions for Updatable View:
1. Based on single table
2. No GROUP BY
3. No DISTINCT
4. No JOIN
5. No HAVING
6. No LIMIT
7. No subqueries

Updatable View:
CREATE VIEW emp_update AS
SELECT emp_id, name, salary FROM employees;

UPDATE emp_update SET salary = 5000000 WHERE emp_id = 1;

Non-Updatable Views:
CREATE VIEW dept_summary AS
SELECT dept_id, COUNT(*) AS cnt, AVG(salary) AS avg_sal
FROM employees
GROUP BY dept_id;
-- Not updatable due to GROUP BY and AVG

CREATE VIEW high_salary AS
SELECT DISTINCT name FROM employees WHERE salary > 4000000;
-- Not updatable due to DISTINCT
```

---

### Question 15: View vs Stored Procedure Comparison

**Model Answer**:

```
Differences:

View:
- Virtual table based on SELECT
- Query capability only (mostly)
- Purpose: logical abstraction
- No parameters
- Simplifies complex queries

Stored Procedure:
- SQL routine with logic implementation
- Query, modify, delete, control all possible
- Purpose: business logic automation
- Supports parameters (IN, OUT, INOUT)
- Uses loops and conditionals

Advantages:

View:
✅ Simplifies queries
✅ Provides security
✅ Maintains compatibility
❌ Performance: recalculated each time

Procedure:
✅ Implements complex logic
✅ Performance: pre-compiled
✅ High reusability
❌ Complex management
```

---

## Practical Problems Model Answers (5 Questions)

### Question 16: Create employees Table

**Completion Criteria**:
✅ ch10_view_procedure database created
✅ employees table created
✅ 3 employees inserted

---

### Question 17: Basic Views

**Completion Criteria**:
✅ employee_salary_view: name and salary
✅ high_salary_view: salary >= 4000000

**Expected Results**:

```
employee_salary_view:
Lee Younghee, 4000000
Kim Chulsu, 5000000
Park Minjun, 4500000

high_salary_view:
Kim Chulsu, 5000000
Lee Younghee, 4000000
Park Minjun, 4500000
```

---

### Question 18: Aggregation View and Modification

**Completion Criteria**:
✅ dept_summary_view: department statistics
✅ UPDATE through View
✅ Verify modified data

---

### Question 19: Stored Procedures

**Completion Criteria**:
✅ IN procedure: get employee info
✅ OUT procedure: return employee count
✅ Conditional procedure: determine salary level

---

### Question 20: Complex Procedures

**Model Answer**:

```sql
-- 1. Salary raise procedure
CREATE PROCEDURE RaiseSalary (IN emp_id INT, IN raise_rate DECIMAL, OUT new_salary INT)
BEGIN
  DECLARE max_salary INT DEFAULT 6000000;
  DECLARE current_sal INT;
  
  SELECT salary INTO current_sal FROM employees WHERE employee_id = emp_id;
  SET new_salary = ROUND(current_sal * (1 + raise_rate/100));
  
  IF new_salary > max_salary THEN
    SET new_salary = max_salary;
  END IF;
  
  UPDATE employees SET salary = new_salary WHERE employee_id = emp_id;
END;

CALL RaiseSalary(1, 10, @new_sal);
SELECT @new_sal;

-- 2. Salary grade procedure
CREATE PROCEDURE AssignSalaryGrade (IN emp_id INT, OUT grade CHAR)
BEGIN
  DECLARE emp_salary INT;
  SELECT salary INTO emp_salary FROM employees WHERE employee_id = emp_id;
  
  IF emp_salary >= 5000000 THEN
    SET grade = 'A';
  ELSEIF emp_salary >= 4500000 THEN
    SET grade = 'B';
  ELSEIF emp_salary >= 4000000 THEN
    SET grade = 'C';
  ELSE
    SET grade = 'D';
  END IF;
END;

CALL AssignSalaryGrade(1, @g);
SELECT @g;

-- 3. Data validation procedure
CREATE PROCEDURE ValidateEmployee (IN emp_id INT, OUT result VARCHAR(100))
BEGIN
  DECLARE emp_exists INT;
  SELECT COUNT(*) INTO emp_exists FROM employees WHERE employee_id = emp_id;
  
  IF emp_exists > 0 THEN
    SELECT CONCAT('Employee exists: ', name) INTO result FROM employees WHERE employee_id = emp_id;
  ELSE
    SET result = 'Employee does not exist';
  END IF;
END;

CALL ValidateEmployee(1, @result);
SELECT @result;

-- 4. Batch processing with WHILE loop
CREATE TABLE salary_summary (
  emp_id INT,
  original_salary INT,
  adjusted_salary INT
);

CREATE PROCEDURE BatchProcessSalary (IN dept_id INT)
BEGIN
  DECLARE done INT DEFAULT FALSE;
  DECLARE emp_id INT;
  DECLARE emp_salary INT;
  DECLARE i INT DEFAULT 1;
  
  DECLARE emp_cursor CURSOR FOR 
    SELECT employee_id, salary FROM employees WHERE dept_id = dept_id;
  DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = TRUE;
  
  OPEN emp_cursor;
  FETCH emp_cursor INTO emp_id, emp_salary;
  
  emp_loop: WHILE NOT done DO
    INSERT INTO salary_summary VALUES (emp_id, emp_salary, ROUND(emp_salary * 1.05));
    FETCH emp_cursor INTO emp_id, emp_salary;
  END WHILE;
  
  CLOSE emp_cursor;
END;

CALL BatchProcessSalary(1);
SELECT * FROM salary_summary;
```

---

Prof. Jeong-Hyun Cho (peterchokr@gmail.com)
Yeungnam University College
