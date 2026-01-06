# Chapter 10: View and Stored Procedure

---

## 📖 Course Overview

In this chapter, you will learn about Views that provide logical abstraction of databases and Stored Procedures that are reusable SQL routines. You will learn to simplify complex queries using views, control data access, and automate repetitive tasks with stored procedures, implementing application logic in the database. The goal is to enhance database maintainability and security. 

| 이 장에서는 데이터베이스의 논리적 추상화를 제공하는 뷰(View)와 재사용 가능한 SQL 루틴인 저장프로시저(Stored Procedure)를 학습합니다. 뷰를 사용하여 복잡한 쿼리를 단순화하고 데이터 접근을 제어하며, 저장프로시저로 반복적인 작업을 자동화하고 애플리케이션 로직을 데이터베이스에 구현하는 방법을 다룹니다. 데이터베이스의 유지보수성과 보안을 강화하는 것이 목표입니다.

---

## 📚 Part 1: Theoretical Learning

### What You'll Learn in This Section

- View concept and creation | 뷰의 개념과 생성
- View usage cases | 뷰의 활용 사례
- Advantages and disadvantages of views | 뷰의 장단점
- Stored procedure concept and syntax | 저장프로시저의 개념과 문법
- Stored procedure parameters | 저장프로시저의 매개변수
- Stored procedure execution and management | 저장프로시저 실행 및 관리

---

### 10.1 View Concept

A **view** is a virtual table based on one or more tables. | **뷰**는 하나 이상의 테이블을 기반으로 하는 가상 테이블입니다.

**Characteristics:**

- Does not store actual data (logical abstraction) | 실제 데이터를 저장하지 않음 (논리적 추상화)
- Defined by SELECT query | SELECT 쿼리로 정의됨
- Can be queried like a table with SELECT | 테이블처럼 SELECT로 조회 가능
- Simplifies complex joins or aggregations | 복잡한 조인이나 집계를 단순화

**Creating a View:**

```sql
CREATE VIEW view_name AS
SELECT column1, column2, ...
FROM table_name
WHERE condition;
```

**Querying a View:**

```sql
SELECT * FROM view_name;
```

---

### 10.2 View Usage Cases

**1. Simplifying Complex Queries:**

```sql
CREATE VIEW sales_summary AS
SELECT p.product_name, COUNT(*) AS sales_count, SUM(s.quantity) AS total_qty
FROM products p
JOIN sales s ON p.product_id = s.product_id
GROUP BY p.product_id, p.product_name;

-- Usage
SELECT * FROM sales_summary WHERE total_qty > 100;
```

**2. Data Security:**

```sql
CREATE VIEW employee_public AS
SELECT employee_id, name, hire_date
FROM employees;  -- Salary information excluded
```

**3. Data Abstraction:**

```sql
CREATE VIEW current_employees AS
SELECT * FROM employees
WHERE termination_date IS NULL;
```

---

### 10.3 Modifying and Deleting Views

**Modifying a View:**

```sql
ALTER VIEW view_name AS
SELECT column1, column2, ...
FROM table_name;
```

**Deleting a View:**

```sql
DROP VIEW view_name;
DROP VIEW IF EXISTS view_name;  -- Ignore if not exists
```

**Deleting Multiple Views:**

```sql
DROP VIEW view1, view2, view3;
```

---

### 10.4 Updatable View

If certain conditions are met, INSERT, UPDATE, DELETE are possible on views. | 특정 조건을 만족하면 뷰에 INSERT, UPDATE, DELETE가 가능합니다.

**Conditions:**

- Based on single table | 단일 테이블을 기반으로 함
- Does not include GROUP BY, DISTINCT, JOIN | GROUP BY, DISTINCT, JOIN을 포함하지 않음
- Does not include subquery, UNION | 서브쿼리, UNION을 포함하지 않음
- Does not include HAVING, LIMIT | HAVING, LIMIT을 포함하지 않음

**Example:**

```sql
CREATE VIEW employee_view AS
SELECT employee_id, name, salary FROM employees;

-- Can modify through view
UPDATE employee_view SET salary = 5000000 WHERE employee_id = 1;
```

---

### 10.5 Stored Procedure Concept

A **stored procedure** is a precompiled SQL routine stored in the database. | **저장프로시저**는 데이터베이스에 저장된 미리 컴파일된 SQL 루틴입니다.

**Characteristics:**

- Pre-compiled, so execution is faster | 미리 컴파일되어 실행이 빠름
- Can include control flow (IF, LOOP, etc.) | 제어문 (IF, LOOP 등) 포함 가능
- Can have parameters (IN, OUT, INOUT) | 매개변수 (IN, OUT, INOUT) 가능
- Can return results or values | 결과나 값을 반환할 수 있음

**Creating a Stored Procedure:**

```sql
DELIMITER //
CREATE PROCEDURE procedure_name (parameter1 INT, parameter2 VARCHAR(50))
BEGIN
  SELECT * FROM table_name WHERE column = parameter1;
END //
DELIMITER ;
```

**Calling a Stored Procedure:**

```sql
CALL procedure_name(value1, value2);
```

---

### 10.6 Stored Procedure Parameters

**IN Parameter:**

- Input parameter, passes value to procedure | 입력 매개변수, 프로시저에 값을 전달

**OUT Parameter:**

- Output parameter, returns value from procedure | 출력 매개변수, 프로시저에서 값을 반환

**INOUT Parameter:**

- Both input and output | 입력 및 출력 모두

**Example:**

```sql
DELIMITER //
CREATE PROCEDURE get_employee_salary (IN emp_id INT, OUT sal DECIMAL)
BEGIN
  SELECT salary INTO sal FROM employees WHERE employee_id = emp_id;
END //
DELIMITER ;

CALL get_employee_salary(1, @salary);
SELECT @salary;
```

---

### 10.7 Stored Procedure with Control Flow

**IF Statement:**

```sql
DELIMITER //
CREATE PROCEDURE check_salary (IN emp_id INT)
BEGIN
  DECLARE sal DECIMAL;
  SELECT salary INTO sal FROM employees WHERE employee_id = emp_id;
  
  IF sal > 4000000 THEN
    SELECT 'High Salary';
  ELSEIF sal > 3000000 THEN
    SELECT 'Medium Salary';
  ELSE
    SELECT 'Low Salary';
  END IF;
END //
DELIMITER ;
```

**LOOP Statement:**

```sql
DELIMITER //
CREATE PROCEDURE insert_employees (IN count INT)
BEGIN
  DECLARE i INT DEFAULT 1;
  
  WHILE i <= count DO
    INSERT INTO employees VALUES (i, CONCAT('Employee', i), 1, 3000000);
    SET i = i + 1;
  END WHILE;
END //
DELIMITER ;
```

---

### 10.8 Advantages and Disadvantages of Views and Stored Procedures

**Views Advantages:**

- Simplifies queries | 쿼리 단순화
- Provides security | 보안 제공
- Hides complexity | 복잡성 숨김

**Views Disadvantages:**

- May have performance overhead | 성능 오버헤드 가능
- Limited update capabilities | 수정 기능 제한
- Requires maintenance when base tables change | 기본 테이블 변경 시 유지보수 필요

**Stored Procedure Advantages:**

- Better performance | 더 나은 성능
- Code reusability | 코드 재사용
- Business logic in database | 데이터베이스에 비즈니스 로직 구현

**Stored Procedure Disadvantages:**

- Database-specific syntax | 데이터베이스별 문법 차이
- Harder to version control | 버전 관리 어려움
- Difficult to debug | 디버깅 어려움

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
(4, 'Choi Sunsin', 2, 3500000);
```

### departments Table

```sql
CREATE TABLE departments (
    dept_id INT PRIMARY KEY AUTO_INCREMENT,
    department_name VARCHAR(50) NOT NULL
);

INSERT INTO departments VALUES
(1, 'Sales'),
(2, 'Technology');
```

---

## 💻 Part 3: Practice

### What You'll Learn in This Section

- Creating and using views | 뷰 생성 및 사용
- View modification and deletion | 뷰 수정 및 삭제
- Creating stored procedures | 저장프로시저 생성
- Using parameters in procedures | 프로시저의 매개변수 사용
- Control flow in procedures | 프로시저의 제어 흐름
- Practical applications | 실무 응용

---

### 10-1. Creating Simple View

Create a view that shows employee name and salary. | 직원명과 급여를 표시하는 뷰를 만드세요.

```sql
CREATE VIEW employee_salary_view AS
SELECT employee_id, name, salary
FROM employees;

SELECT * FROM employee_salary_view;
```

---

### 10-2. View with Filter

Create a view showing only employees with salary above 4000000. | 급여가 4000000 이상인 직원만 표시하는 뷰를 만드세요.

```sql
CREATE VIEW high_salary_view AS
SELECT employee_id, name, salary
FROM employees
WHERE salary > 4000000;

SELECT * FROM high_salary_view;
```

---

### 10-3. View with JOIN

Create a view showing employee name and department name. | 직원명과 부서명을 표시하는 뷰를 만드세요.

```sql
CREATE VIEW employee_department_view AS
SELECT e.employee_id, e.name, d.department_name
FROM employees e
JOIN departments d ON e.dept_id = d.dept_id;

SELECT * FROM employee_department_view;
```

---

### 10-4. View with Aggregation

Create a view showing department average salary. | 부서별 평균 급여를 표시하는 뷰를 만드세요.

```sql
CREATE VIEW department_salary_avg AS
SELECT d.department_name, AVG(e.salary) AS avg_salary
FROM employees e
JOIN departments d ON e.dept_id = d.dept_id
GROUP BY d.department_name;

SELECT * FROM department_salary_avg;
```

---

### 10-5. Simple Stored Procedure

Create a stored procedure to get employee information by ID. | ID로 직원 정보를 조회하는 저장프로시저를 만드세요.

```sql
DELIMITER //
CREATE PROCEDURE get_employee (IN emp_id INT)
BEGIN
  SELECT * FROM employees WHERE employee_id = emp_id;
END //
DELIMITER ;

CALL get_employee(1);
```

---

### 10-6. Modifying a View

Modify the definition of an existing view. | 기존 뷰의 정의를 변경하세요.

```sql
ALTER VIEW high_salary_view AS
SELECT employee_id, name, salary, dept_id
FROM employees
WHERE salary > 4000000;
```

---

### 10-7. Deleting a View

Delete a view. | 뷰를 삭제하세요.

```sql
DROP VIEW IF EXISTS high_salary_view;
```

---

### 10-8. Updatable View

Create and modify a view that supports INSERT, UPDATE, DELETE. | 수정 가능한 뷰를 생성하고 수정하세요.

```sql
CREATE VIEW employee_view AS
SELECT employee_id, name, salary FROM employees;

-- Modify through view
UPDATE employee_view SET salary = 5000000 WHERE employee_id = 1;
```

---

### 10-9. INSERT Through View

Insert data through a view. | 뷰를 통해 데이터를 삽입하세요.

```sql
INSERT INTO employee_view (name, salary)
VALUES ('Park Sujeong', 4200000);
```

---

### 10-10. UPDATE Through View

Update data through a view. | 뷰를 통해 데이터를 수정하세요.

```sql
UPDATE employee_view SET salary = 4800000 WHERE employee_id = 2;
```

---

### 10-11. Basic Stored Procedure

Create a simple stored procedure with input parameter. | 입력 매개변수를 받는 간단한 저장프로시저를 생성하세요.

```sql
DELIMITER //
CREATE PROCEDURE GetEmployeeInfo (IN emp_id INT)
BEGIN
  SELECT employee_id, name, salary, dept_id
  FROM employees
  WHERE employee_id = emp_id;
END //
DELIMITER ;

CALL GetEmployeeInfo(1);
```

---

### 10-12. OUT Parameter

Create stored procedure using OUT parameter. | OUT 매개변수를 사용하는 저장프로시저를 생성하세요.

```sql
DELIMITER //
CREATE PROCEDURE GetEmployeeCount (OUT emp_count INT)
BEGIN
  SELECT COUNT(*) INTO emp_count FROM employees;
END //
DELIMITER ;

CALL GetEmployeeCount(@count);
SELECT @count;
```

---

### 10-13. INOUT Parameter

Create stored procedure using INOUT parameter. | INOUT 매개변수를 사용하는 저장프로시저를 생성하세요.

```sql
DELIMITER //
CREATE PROCEDURE IncreaseSalary (INOUT salary DECIMAL)
BEGIN
  SET salary = salary * 1.1;
END //
DELIMITER ;

SET @my_salary = 5000000;
CALL IncreaseSalary(@my_salary);
SELECT @my_salary;
```

---

### 10-14. IF-ELSE Statement

Create stored procedure with conditional statement. | 조건문을 포함하는 저장프로시저를 생성하세요.

```sql
DELIMITER //
CREATE PROCEDURE CheckSalaryLevel (IN emp_id INT)
BEGIN
  DECLARE emp_salary DECIMAL;
  SELECT salary INTO emp_salary FROM employees WHERE employee_id = emp_id;
  
  IF emp_salary > 5000000 THEN
    SELECT 'High' AS salary_level;
  ELSEIF emp_salary > 4000000 THEN
    SELECT 'Medium' AS salary_level;
  ELSE
    SELECT 'Low' AS salary_level;
  END IF;
END //
DELIMITER ;

CALL CheckSalaryLevel(1);
```

---

### 10-15. CASE Statement

Create stored procedure using CASE statement. | CASE 문을 사용하는 저장프로시저를 생성하세요.

```sql
DELIMITER //
CREATE PROCEDURE GetGrade (IN score INT, OUT grade CHAR(1))
BEGIN
  SET grade = CASE
    WHEN score >= 90 THEN 'A'
    WHEN score >= 80 THEN 'B'
    WHEN score >= 70 THEN 'C'
    ELSE 'F'
  END;
END //
DELIMITER ;

CALL GetGrade(85, @result);
SELECT @result;
```

---

### 10-16. WHILE Loop

Create stored procedure with loop. | 반복문을 포함하는 저장프로시저를 생성하세요.

```sql
DELIMITER //
CREATE PROCEDURE InsertSampleData (IN count INT)
BEGIN
  DECLARE i INT DEFAULT 1;
  WHILE i <= count DO
    INSERT INTO temp_table VALUES (i, CONCAT('Data', i));
    SET i = i + 1;
  END WHILE;
END //
DELIMITER ;

CALL InsertSampleData(5);
```

---

### 10-17. Execute Stored Procedure

Call stored procedure to execute. | 프로시저를 호출하여 실행하세요.

```sql
CALL GetEmployeeInfo(1);
```

---

### 10-18. Variable Declaration and Assignment

Declare and use variables in procedure. | 프로시저에서 변수를 선언하고 사용하세요.

```sql
DELIMITER //
CREATE PROCEDURE CalculateSalaryInfo ()
BEGIN
  DECLARE total_salary DECIMAL;
  DECLARE avg_salary DECIMAL;
  DECLARE emp_count INT;
  
  SELECT SUM(salary) INTO total_salary FROM employees;
  SELECT AVG(salary) INTO avg_salary FROM employees;
  SELECT COUNT(*) INTO emp_count FROM employees;
  
  SELECT total_salary, avg_salary, emp_count;
END //
DELIMITER ;

CALL CalculateSalaryInfo();
```

---

### 10-19. Procedure with Transaction

Create stored procedure with transaction control. | 트랜잭션 제어를 포함하는 저장프로시저를 생성하세요.

```sql
DELIMITER //
CREATE PROCEDURE TransferSalary (IN from_emp_id INT, IN to_emp_id INT, IN amount DECIMAL)
BEGIN
  START TRANSACTION;
  
  UPDATE employees SET salary = salary - amount WHERE employee_id = from_emp_id;
  UPDATE employees SET salary = salary + amount WHERE employee_id = to_emp_id;
  
  COMMIT;
END //
DELIMITER ;

CALL TransferSalary(1, 2, 100000);
```

---

### 10-20. Error Handling

Handle errors with DECLARE HANDLER. | DECLARE HANDLER로 에러를 처리하세요.

```sql
DELIMITER //
CREATE PROCEDURE SafeInsert (IN emp_name VARCHAR(50), IN dept_id INT)
BEGIN
  DECLARE CONTINUE HANDLER FOR SQLEXCEPTION
  BEGIN
    SELECT 'Error occurred' AS status;
  END;
  
  INSERT INTO employees (name, dept_id) VALUES (emp_name, dept_id);
  SELECT 'Success' AS status;
END //
DELIMITER ;

CALL SafeInsert('Test', 1);
```

---

### 10-21. Cursor

Process rows iteratively using cursor. | 커서를 사용하여 행을 반복 처리하세요.

```sql
DELIMITER //
CREATE PROCEDURE ProcessEmployees ()
BEGIN
  DECLARE done INT DEFAULT 0;
  DECLARE emp_name VARCHAR(50);
  DECLARE emp_cursor CURSOR FOR SELECT name FROM employees;
  DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = TRUE;
  
  OPEN emp_cursor;
  read_loop: LOOP
    FETCH emp_cursor INTO emp_name;
    IF done THEN LEAVE read_loop; END IF;
    INSERT INTO processed_employees VALUES (emp_name);
  END LOOP;
  CLOSE emp_cursor;
END //
DELIMITER ;

CALL ProcessEmployees();
```

---

### 10-22. Dynamic SQL

Use PREPARE and EXECUTE. | PREPARE와 EXECUTE를 사용하세요.

```sql
DELIMITER //
CREATE PROCEDURE DynamicQuery (IN table_name VARCHAR(50))
BEGIN
  SET @query = CONCAT('SELECT COUNT(*) FROM ', table_name);
  PREPARE stmt FROM @query;
  EXECUTE stmt;
  DEALLOCATE PREPARE stmt;
END //
DELIMITER ;

CALL DynamicQuery('employees');
```

---

### 10-23. Data Validation Procedure

Create procedure to validate data integrity. | 데이터 무결성을 검증하는 프로시저를 생성하세요.

```sql
DELIMITER //
CREATE PROCEDURE ValidateEmployee (IN emp_id INT, OUT is_valid INT)
BEGIN
  IF EXISTS(SELECT 1 FROM employees WHERE employee_id = emp_id) THEN
    SET is_valid = 1;
  ELSE
    SET is_valid = 0;
  END IF;
END //
DELIMITER ;

CALL ValidateEmployee(1, @valid);
SELECT @valid;
```

---

### 10-24. Data Conversion Procedure

Create procedure to convert data format. | 데이터 형식을 변환하는 프로시저를 생성하세요.

```sql
DELIMITER //
CREATE PROCEDURE ConvertSalaryData ()
BEGIN
  UPDATE employees
  SET salary = ROUND(salary / 1000) * 1000;
END //
DELIMITER ;

CALL ConvertSalaryData();
```

---

### 10-25. Batch Processing Procedure

Create procedure to process large amounts of data. | 대량의 데이터를 처리하는 프로시저를 생성하세요.

```sql
DELIMITER //
CREATE PROCEDURE BatchIncreaseSalary (IN percentage DECIMAL)
BEGIN
  UPDATE employees SET salary = salary * (1 + percentage / 100);
END //
DELIMITER ;

CALL BatchIncreaseSalary(10);
```

---

### 10-26. Statistics Calculation Procedure

Create procedure to calculate statistics. | 통계 정보를 계산하는 프로시저를 생성하세요.

```sql
DELIMITER //
CREATE PROCEDURE GetSalaryStatistics (OUT total DECIMAL, OUT average DECIMAL, OUT max DECIMAL)
BEGIN
  SELECT SUM(salary), AVG(salary), MAX(salary)
  INTO total, average, max FROM employees;
END //
DELIMITER ;

CALL GetSalaryStatistics(@t, @a, @m);
SELECT @t AS total, @a AS average, @m AS max;
```

---

### 10-27. Data Cleaning Procedure

Create procedure to clean data. | 데이터를 정제하는 프로시저를 생성하세요.

```sql
DELIMITER //
CREATE PROCEDURE CleanData ()
BEGIN
  DELETE FROM employees WHERE salary IS NULL;
  UPDATE employees SET name = TRIM(name);
END //
DELIMITER ;

CALL CleanData();
```

---

### 10-28. Logging Procedure

Create procedure to log actions. | 작업 내역을 로그에 기록하는 프로시저를 생성하세요.

```sql
DELIMITER //
CREATE PROCEDURE LogAction (IN action VARCHAR(50), IN details VARCHAR(255))
BEGIN
  INSERT INTO action_log (action, details, created_at)
  VALUES (action, details, NOW());
END //
DELIMITER ;

CALL LogAction('UPDATE', 'Employee salary updated');
```

---

### 10-29. Data Migration Procedure

Create procedure to move data. | 데이터를 이동하는 프로시저를 생성하세요.

```sql
DELIMITER //
CREATE PROCEDURE MigrateOldEmployees ()
BEGIN
  INSERT INTO employees_archive
  SELECT * FROM employees WHERE hire_date < '2020-01-01';
  
  DELETE FROM employees WHERE hire_date < '2020-01-01';
END //
DELIMITER ;

CALL MigrateOldEmployees();
```

---

### 10-30. Backup Procedure

Create procedure to backup data. | 데이터를 백업하는 프로시저를 생성하세요.

```sql
DELIMITER //
CREATE PROCEDURE BackupData ()
BEGIN
  INSERT INTO employees_backup SELECT * FROM employees;
END //
DELIMITER ;

CALL BackupData();
```

---

### 10-31. Complex Business Logic Procedure

Create procedure that modifies multiple tables. | 여러 테이블을 수정하는 복합 프로시저를 생성하세요.

```sql
DELIMITER //
CREATE PROCEDURE PromoteEmployee (IN emp_id INT, IN new_dept_id INT, IN new_salary DECIMAL)
BEGIN
  START TRANSACTION;
  UPDATE employees SET dept_id = new_dept_id, salary = new_salary WHERE employee_id = emp_id;
  INSERT INTO promotion_history VALUES (emp_id, new_dept_id, new_salary, NOW());
  COMMIT;
END //
DELIMITER ;

CALL PromoteEmployee(1, 2, 5500000);
```

---

### 10-32. Recursive Procedure

Create recursive procedure to handle hierarchy. | 계층 구조를 처리하는 재귀 프로시저를 생성하세요.

```sql
DELIMITER //
CREATE PROCEDURE PrintHierarchy (IN emp_id INT, IN level INT)
BEGIN
  DECLARE manager_id INT;
  SELECT manager_id INTO manager_id FROM employees WHERE employee_id = emp_id;
  
  IF manager_id IS NULL THEN
    SELECT REPEAT('  ', level), name FROM employees WHERE employee_id = emp_id;
  ELSE
    CALL PrintHierarchy(manager_id, level + 1);
    SELECT REPEAT('  ', level), name FROM employees WHERE employee_id = emp_id;
  END IF;
END //
DELIMITER ;

CALL PrintHierarchy(1, 0);
```

---

### 10-33. Performance Monitoring Procedure

Create procedure to collect performance information. | 성능 정보를 수집하는 프로시저를 생성하세요.

```sql
DELIMITER //
CREATE PROCEDURE MonitorPerformance ()
BEGIN
  SELECT 
    (SELECT COUNT(*) FROM employees) AS emp_count,
    (SELECT COUNT(*) FROM departments) AS dept_count,
    (SELECT AVG(salary) FROM employees) AS avg_salary;
END //
DELIMITER ;

CALL MonitorPerformance();
```

---

### 10-34. View Procedure List

Query all created procedures. | 생성된 모든 프로시저를 조회하세요.

```sql
SHOW PROCEDURE STATUS WHERE Db = 'database_name';
```

---

### 10-35. Procedure Modification

Modify existing procedure. | 기존 프로시저를 수정하세요.

```sql
DROP PROCEDURE GetEmployeeInfo;

DELIMITER //
CREATE PROCEDURE GetEmployeeInfo (IN emp_id INT)
BEGIN
  SELECT employee_id, name, salary, dept_id, hire_date
  FROM employees
  WHERE employee_id = emp_id;
END //
DELIMITER ;
```

---

### 10-36. Procedure Deletion

Delete a procedure. | 프로시저를 삭제하세요.

```sql
DROP PROCEDURE IF EXISTS GetEmployeeInfo;
```

---

### 10-37. View List Query

Query all views in database. | 데이터베이스의 모든 뷰를 조회하세요.

```sql
SHOW TABLES WHERE Table_type = 'VIEW';
```

---

### 10-38. View Definition Query

View the definition of a view. | 뷰의 정의를 확인하세요.

```sql
SHOW CREATE VIEW employee_department_view;
```

---

### 10-39. Procedure and View Performance Comparison

Compare performance of procedure and view. | 프로시저와 뷰의 성능을 비교하세요.

```sql
-- View method
SELECT * FROM employee_department_view LIMIT 10;

-- Procedure method
CALL GetEmployeeInfo(1);
```

---

### 10-40. Real-World Scenario Procedure

Create procedure for real-world scenarios like salary calculation and bonus payment. | 급여 계산, 성과급 지급 등 실무 시나리오의 프로시저를 생성하세요.

```sql
DELIMITER //
CREATE PROCEDURE CalculateBonus (IN dept_id INT, IN bonus_percentage DECIMAL)
BEGIN
  START TRANSACTION;
  
  UPDATE employees
  SET salary = salary + (salary * bonus_percentage / 100)
  WHERE dept_id = dept_id;
  
  INSERT INTO bonus_history (dept_id, amount, paid_date)
  SELECT dept_id, SUM(salary), NOW()
  FROM employees
  WHERE dept_id = dept_id;
  
  COMMIT;
END //
DELIMITER ;

CALL CalculateBonus(1, 10);
```

---

## 📝 Part 4: Assignment Instructions

### Theoretical Assignments

**Assignment 1**: Explain views concept and advantages. When should views be used? Provide practical examples. | 뷰의 개념과 장점을 설명하세요. 뷰를 언제 사용해야 하는지 설명하고 실제 사례를 제시하세요.

**Assignment 2**: Explain stored procedures concept and advantages. When should stored procedures be used? | 저장프로시저의 개념과 장점을 설명하세요. 저장프로시저를 언제 사용해야 하는지 설명하세요.

**Assignment 3**: Explain view modification and deletion. Discuss considerations when changing existing views. | 뷰의 수정과 삭제를 설명하세요. 기존 뷰를 변경할 때의 고려사항을 논의하세요.

**Assignment 4**: Explain stored procedure parameters (IN, OUT, INOUT). Provide examples of each. | 저장프로시저 매개변수 (IN, OUT, INOUT)를 설명하세요. 각각의 예시를 제시하세요.

**Assignment 5**: Compare views and stored procedures. Discuss when to use each and their impact on performance. | 뷰와 저장프로시저를 비교하세요. 각각을 언제 사용할지와 성능 영향을 논의하세요.

Submission Format: Word or PDF document (2-3 pages)

---

### Practical Assignments

**Assignment 1**: Create 5 views for different purposes: simple filtering, JOIN, aggregation, complex calculations, data security. | 5개의 뷰를 다양한 목적으로 만드세요: 단순 필터링, JOIN, 집계, 복잡한 계산, 데이터 보안.

**Assignment 2**: Modify and test views: update view definition, delete views, test updatable views. | 뷰를 수정하고 테스트하세요: 뷰 정의 변경, 뷰 삭제, 수정 가능한 뷰 테스트.

**Assignment 3**: Create 5 stored procedures with parameters: get employee, update salary, insert employee, delete employee, count employees. | 5개의 저장프로시저를 매개변수와 함께 만드세요: 직원 조회, 급여 수정, 직원 추가, 직원 삭제, 직원 수 세기.

**Assignment 4**: Create stored procedures with control flow: IF statements for decision making, WHILE loops for repetition, calculation procedures. | 제어 흐름을 포함한 저장프로시저를 만드세요: 의사결정을 위한 IF, 반복을 위한 WHILE, 계산 프로시저.

**Assignment 5**: Execute all Practice 10-1 to 10-40 queries with full functionality. Attach screenshots. Create 5 business scenarios using views and procedures. | 실습 10-1부터 10-40까지의 모든 쿼리를 실행하세요. 스크린샷을 첨부하세요. 뷰와 프로시저를 사용한 비즈니스 시나리오 5개를 작성하세요.

Submission Format: SQL file (Ch10_View_Procedure_[StudentID].sql) and result screenshots

---

Thank you for your hard work.

Prof. Cho Jeong-Hyun (peterchokr@gmail.com). Yeungnam University College
