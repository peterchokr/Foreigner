# Chapter 9: DML (Data Manipulation Language) and Transaction

---

## 📖 Course Overview

In this chapter, you will learn about DML commands (INSERT, UPDATE, DELETE) that manipulate database data and Transactions that ensure data integrity. You will learn methods to accurately and safely insert, modify, and delete data, and how to maintain data consistency by processing multiple operations as a single logical unit. The goal is to develop the ability to prevent and recover from data integrity errors in practice. 

| 이 장에서는 데이터베이스의 데이터를 조작하는 DML 명령어(INSERT, UPDATE, DELETE)와 데이터 무결성을 보장하는 트랜잭션(Transaction)을 학습합니다. 데이터를 정확하고 안전하게 입력, 수정, 삭제하는 방법과 여러 작업을 하나의 논리적 단위로 처리하여 데이터 일관성을 유지하는 방법을 다룹니다. 실무에서 데이터 무결성 오류를 방지하고 복구할 수 있는 능력을 개발하는 것이 목표입니다.

---

## 📚 Part 1: Theoretical Learning

### What You'll Learn in This Section

- Various forms of INSERT statements | INSERT 문의 다양한 형태
- UPDATE statements and condition handling | UPDATE 문과 조건 처리
- DELETE statements and data protection | DELETE 문과 데이터 보호
- Transaction concept and ACID properties | 트랜잭션의 개념과 특성 (ACID)
- Role of COMMIT and ROLLBACK | COMMIT과 ROLLBACK의 역할
- Concurrency control and locking | 동시성 제어 및 잠금

---

### 9.1 INSERT Statement

**INSERT** adds new data to a table. | **INSERT**는 테이블에 새로운 데이터를 추가합니다.

**Basic Syntax:**

```sql
INSERT INTO table_name (column1, column2, ...)
VALUES (value1, value2, ...);
```

**Omitting Column Specification:**

```sql
INSERT INTO table_name
VALUES (value1, value2, ...);  -- All column values in order
```

**Inserting Multiple Rows:**

```sql
INSERT INTO table_name (col1, col2)
VALUES (val1, val2), (val3, val4), (val5, val6);
```

**Inserting with Subquery:**

```sql
INSERT INTO table_name (col1, col2)
SELECT col1, col2 FROM other_table WHERE condition;
```

**Characteristics:**

- Checks NOT NULL constraints | NOT NULL 제약조건을 확인
- Can set default values (DEFAULT) | 기본값(DEFAULT)을 설정할 수 있음
- AUTO_INCREMENT columns increase automatically | AUTO_INCREMENT 열은 자동으로 증가

---

### 9.2 UPDATE Statement

**UPDATE** modifies existing data. | **UPDATE**는 기존 데이터를 수정합니다.

**Basic Syntax:**

```sql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
```

**Important Notes:**

- All rows are updated if WHERE condition is not specified | WHERE 조건을 명시하지 않으면 모든 행이 수정됨
- Always verify conditions first with SELECT | 조건을 항상 먼저 SELECT로 확인하기

**Using with Subquery:**

```sql
UPDATE employees
SET salary = (SELECT AVG(salary) FROM employees)
WHERE dept_id = 1;
```

**Using with JOIN:**

```sql
UPDATE employees e
JOIN departments d ON e.dept_id = d.dept_id
SET e.salary = e.salary * 1.1
WHERE d.location = 'Seoul';
```

**Characteristics:**

- Can modify multiple columns simultaneously | 여러 열을 동시에 수정 가능
- Can use expressions (salary = salary * 1.1) | 연산식 사용 가능 (salary = salary * 1.1)
- Checks foreign key constraints | 외래키 제약조건을 확인

---

### 9.3 DELETE Statement

**DELETE** removes data from a table. | **DELETE**는 테이블의 데이터를 삭제합니다.

**Basic Syntax:**

```sql
DELETE FROM table_name
WHERE condition;
```

**Important Notes:**

- All rows are deleted if WHERE condition is omitted | WHERE 조건이 없으면 모든 행이 삭제됨
- Verify conditions with SELECT before deletion | 삭제 전에 조건을 SELECT로 확인하기
- Deletion may fail due to foreign key constraints | 외래키 제약조건에 의해 삭제가 실패할 수 있음

**Using with JOIN:**

```sql
DELETE e FROM employees e
WHERE e.dept_id NOT IN (SELECT dept_id FROM departments);
```

**Characteristics:**

- Only data is deleted, table structure remains | 데이터만 삭제되고 테이블 구조는 유지
- TRUNCATE is faster but cannot be rolled back | TRUNCATE는 더 빠르지만 되돌릴 수 없음
- Can be protected with transaction | 트랜잭션으로 보호 가능

---

### 9.4 Transaction Concept

A **transaction** processes one or more SQL statements as a single logical unit. | **트랜잭션**은 하나 이상의 SQL 문을 하나의 논리적 단위로 처리합니다.

**Characteristics:**

- All succeed or all fail (All or Nothing) | 모두 성공하거나 모두 실패 (All or Nothing)
- Prevents unstable data in intermediate state | 중간 상태의 불안정한 데이터를 방지
- Guarantees data consistency | 데이터 일관성 보장

**Explicit Transaction:**

```sql
START TRANSACTION;  -- or BEGIN
  INSERT INTO accounts VALUES (1, 'Kim Chulsu', 1000);
  UPDATE accounts SET balance = balance - 100 WHERE account_id = 1;
  UPDATE accounts SET balance = balance + 100 WHERE account_id = 2;
COMMIT;  -- All succeeded
-- or
ROLLBACK;  -- All cancelled
```

---

### 9.5 ACID Properties

Transaction characteristics are defined as ACID. | 트랜잭션의 특성을 ACID로 정의합니다.

**A (Atomicity):**

- All operations in transaction are fully performed or completely cancelled | 트랜잭션의 모든 작업이 완전히 수행되거나 완전히 취소됨
- No intermediate state | 중간 상태가 없음

**C (Consistency):**

- Data remains in valid state before and after transaction | 트랜잭션 전후로 데이터가 유효한 상태 유지
- All constraints are checked | 모든 제약조건이 검사됨

**I (Isolation):**

- Transactions don't interfere with each other | 트랜잭션 간에 영향을 주지 않음
- Concurrent operations are isolated | 동시성 작업이 격리됨

**D (Durability):**

- Once committed, data persists permanently | COMMIT 후 데이터는 영구적으로 저장됨
- Not lost even if system fails | 시스템 장애 시에도 손실되지 않음

---

### 9.6 COMMIT and ROLLBACK

**COMMIT** makes transaction changes permanent. | **COMMIT**은 트랜잭션의 변경사항을 영구적으로 만듭니다.

**ROLLBACK** cancels transaction and reverts to previous state. | **ROLLBACK**은 트랜잭션을 취소하고 이전 상태로 되돌립니다.

```sql
START TRANSACTION;
INSERT INTO employees VALUES (10, 'New Employee', 1, 3000000);
-- If condition met
COMMIT;  -- Changes saved
-- If error
ROLLBACK;  -- Changes cancelled
```

---

### 9.7 Savepoint

**SAVEPOINT** creates a point within transaction to rollback to. | **SAVEPOINT**는 트랜잭션 내의 특정 지점을 표시합니다.

```sql
START TRANSACTION;
INSERT INTO employees VALUES (10, 'Employee1', 1, 3000000);
SAVEPOINT sp1;
INSERT INTO employees VALUES (11, 'Employee2', 1, 3500000);
ROLLBACK TO sp1;  -- Rolls back to sp1
COMMIT;  -- Commits first INSERT
```

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

- Various INSERT techniques | 다양한 INSERT 기법
- UPDATE with conditions | 조건을 포함한 UPDATE
- Safe DELETE practices | 안전한 DELETE 실습
- Transaction implementation | 트랜잭션 구현
- Error handling and recovery | 오류 처리 및 복구

---

### 9-1. Basic INSERT

Insert new employee information. | 새로운 직원 정보를 입력하세요.

```sql
INSERT INTO employees (name, dept_id, salary, hire_date)
VALUES ('Hwang Sujeong', 3, 4100000, CURDATE());
```

---

### 9-2. INSERT Omitting Columns

Insert values in order for all columns. | 모든 열에 값을 순서대로 입력하세요.

```sql
INSERT INTO employees
VALUES (NULL, 'Geum Sunmin', 2, NULL, 4300000, CURDATE());
```

---

### 9-3. INSERT Multiple Rows

Insert multiple employee records at once. | 여러 직원 정보를 한 번에 입력하세요.

```sql
INSERT INTO employees (name, dept_id, salary) VALUES
('Song Junki', 1, 3900000),
('Im Sejun', 2, 4100000),
('Park Junho', 3, 3700000);
```

---

### 9-4. INSERT with Subquery

Copy employees with above-average salary for specific department. | 특정 부서의 평균 급여 이상인 직원들을 아카이브 테이블에 복사하세요.

```sql
INSERT INTO employee_archive
SELECT * FROM employees
WHERE salary >= (SELECT AVG(salary) FROM employees WHERE dept_id = 1);
```

---

### 9-5. INSERT with DEFAULT

Use default values for data insertion. | 기본값을 이용하여 데이터를 입력하세요.

```sql
INSERT INTO employees (name, dept_id, salary)
VALUES ('Lee Soyoung', 1, DEFAULT);
```

---

### 9-6. Basic UPDATE

Modify specific employee's salary. | 특정 직원의 급여를 수정하세요.

```sql
UPDATE employees
SET salary = 5200000
WHERE employee_id = 1;
```

---

### 9-7. UPDATE Multiple Columns

Modify multiple columns simultaneously. | 여러 열을 동시에 수정하세요.

```sql
UPDATE employees
SET salary = 5500000, dept_id = 2
WHERE employee_id = 2;
```

---

### 9-8. UPDATE with Expression

Increase all employees' salaries by 10%. | 모든 직원의 급여를 10% 인상하세요.

```sql
UPDATE employees
SET salary = salary * 1.1;
```

---

### 9-9. UPDATE with CASE

Increase salaries differently by department. | 특정 부서의 직원들의 급여를 다르게 인상하세요.

```sql
UPDATE employees
SET salary = CASE 
    WHEN dept_id = 1 THEN salary * 1.15
    WHEN dept_id = 2 THEN salary * 1.12
    ELSE salary * 1.10
END;
```

---

### 9-10. UPDATE with Subquery

Adjust salaries to department averages. | 부서별 평균 급여로 모든 직원의 급여를 조정하세요.

```sql
UPDATE employees
SET salary = (SELECT AVG(salary) FROM employees e2 WHERE e2.dept_id = e1.dept_id)
WHERE (SELECT COUNT(*) FROM employees e3 WHERE e3.dept_id = employees.dept_id) > 0;
```

---

### 9-11. UPDATE with JOIN

Perform conditional UPDATE using JOIN. | JOIN을 사용하여 조건부 UPDATE를 수행하세요.

```sql
UPDATE employees e
JOIN departments d ON e.dept_id = d.dept_id
SET e.salary = e.salary * 1.1
WHERE d.location = 'Seoul';
```

---

### 9-12. UPDATE Safe Mode

Verify WHERE condition first with SELECT, then perform UPDATE. | WHERE 조건을 먼저 SELECT로 확인하고 UPDATE를 수행하세요.

```sql
-- Verify first
SELECT * FROM employees WHERE dept_id = 2;

-- Then UPDATE
UPDATE employees
SET salary = salary * 1.1
WHERE dept_id = 2;
```

---

### 9-13. Basic DELETE

Delete a specific employee. | 특정 직원을 삭제하세요.

```sql
DELETE FROM employees
WHERE employee_id = 7;
```

---

### 9-14. DELETE with Condition

Delete multiple rows with condition. | 특정 조건의 여러 행을 삭제하세요.

```sql
DELETE FROM employees
WHERE salary < 3500000;
```

---

### 9-15. DELETE with JOIN

Perform DELETE with JOIN condition. | JOIN 조건으로 DELETE를 수행하세요.

```sql
DELETE e FROM employees e
JOIN departments d ON e.dept_id = d.dept_id
WHERE d.location = 'Busan';
```

---

### 9-16. DELETE with Subquery

Perform DELETE with subquery condition. | 서브쿼리 조건으로 DELETE를 수행하세요.

```sql
DELETE FROM employees
WHERE dept_id IN (SELECT dept_id FROM departments WHERE location = 'Busan');
```

---

### 9-17. Simple Transaction

Process basic transaction with BEGIN-COMMIT. | BEGIN-COMMIT으로 기본 트랜잭션을 처리하세요.

```sql
START TRANSACTION;
INSERT INTO employees (name, dept_id, salary) VALUES ('Choi Yujeong', 1, 4300000);
UPDATE employees SET salary = salary * 1.05 WHERE dept_id = 1;
COMMIT;
```

---

### 9-18. Transaction Rollback

Start transaction then rollback. | 트랜잭션을 시작했다가 롤백하세요.

```sql
START TRANSACTION;
INSERT INTO employees (name, dept_id, salary) VALUES ('Lee Hojin', 2, 4400000);
ROLLBACK;
```

---

### 9-19. Money Transfer Transaction

Process money transfer as single transaction. | 송금자와 수취인의 잔액을 한 트랜잭션으로 처리하세요.

```sql
START TRANSACTION;
UPDATE accounts SET balance = balance - 100000 WHERE account_id = 1001;
UPDATE accounts SET balance = balance + 100000 WHERE account_id = 1002;
COMMIT;
```

---

### 9-20. Transaction Error Handling

Rollback transaction if error occurs. | 트랜잭션 중 오류 발생 시 롤백하세요.

```sql
START TRANSACTION;
INSERT INTO employees (name, dept_id, salary) VALUES ('Park Jun', 5, 4200000);
-- Error occurs (dept_id 5 does not exist)
ROLLBACK;
```

---

### 9-21. SAVEPOINT

Use SAVEPOINT for partial rollback. | SAVEPOINT를 사용하여 부분 롤백을 수행하세요.

```sql
START TRANSACTION;
INSERT INTO employees (name, dept_id, salary) VALUES ('Kim Nahyun', 1, 4100000);
SAVEPOINT sp1;
INSERT INTO employees (name, dept_id, salary) VALUES ('Lee Suho', 2, 4300000);
ROLLBACK TO sp1;
COMMIT;
```

---

### 9-22. Complex Transaction

Process multiple transactions as single transaction. | 여러 거래를 하나의 트랜잭션으로 처리하세요.

```sql
START TRANSACTION;
INSERT INTO orders (customer_id, order_date) VALUES (1, CURDATE());
INSERT INTO order_details (order_id, product_id, quantity) VALUES (1, 1, 5);
UPDATE products SET stock = stock - 5 WHERE product_id = 1;
COMMIT;
```

---

### 9-23. Autocommit Control

Disable autocommit, then explicitly commit. | 자동 커밋을 비활성화한 후 명시적으로 커밋하세요.

```sql
SET AUTOCOMMIT = 0;
UPDATE employees SET salary = 5000000 WHERE employee_id = 1;
COMMIT;
SET AUTOCOMMIT = 1;
```

---

### 9-24. Transaction Isolation Level

Set and test isolation level. | 격리 수준을 설정하고 테스트하세요.

```sql
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;
START TRANSACTION;
SELECT * FROM employees WHERE dept_id = 1;
COMMIT;
```

---

### 9-25. Data Validation

Validate data before INSERT. | INSERT 전에 데이터를 검증하세요.

```sql
SELECT IF((SELECT COUNT(*) FROM employees WHERE name = 'Kim Chulsu') = 0, 
          'OK to insert', 'Already exists');
```

---

### 9-26. Duplicate Check

Check for duplicates before INSERT. | 중복된 데이터가 없는지 확인한 후 INSERT하세요.

```sql
INSERT INTO employees (name, dept_id, salary)
SELECT 'New Employee', 1, 4100000
WHERE NOT EXISTS (SELECT 1 FROM employees WHERE name = 'New Employee');
```

---

### 9-27. Foreign Key Constraint

Handle INSERT failure due to foreign key constraint. | 외래키 제약조건으로 인한 INSERT 실패 처리하세요.

```sql
-- Fails: dept_id 99 does not exist
INSERT INTO employees (name, dept_id, salary) VALUES ('Test', 99, 4000000);
```

---

### 9-28. Foreign Key Constraint UPDATE

Handle UPDATE failure due to foreign key constraint. | 외래키 제약조건으로 인한 UPDATE 실패 처리하세요.

```sql
-- Fails: dept_id 99 does not exist
UPDATE employees SET dept_id = 99 WHERE employee_id = 1;
```

---

### 9-29. Foreign Key Constraint DELETE

Handle DELETE failure due to foreign key constraint. | 외래키 제약조건으로 인한 DELETE 실패 처리하세요.

```sql
-- Fails: child table has references
DELETE FROM departments WHERE dept_id = 1;
```

---

### 9-30. Data Migration

Safely migrate large amounts of data. | 대량의 데이터를 안전하게 마이그레이션하세요.

```sql
START TRANSACTION;
INSERT INTO new_employees SELECT * FROM employees WHERE created_date < '2024-01-01';
DELETE FROM employees WHERE created_date < '2024-01-01';
COMMIT;
```

---

### 9-31. Data Backup

Backup original data before changes. | 변경 전에 원본 데이터를 백업하세요.

```sql
INSERT INTO employees_backup SELECT * FROM employees;
UPDATE employees SET salary = salary * 1.1;
```

---

### 9-32. Concurrency Simulation

Simulate simultaneous updates from two sessions. | 두 개의 세션에서 동시에 같은 데이터를 수정하세요.

```sql
-- Session 1
START TRANSACTION;
UPDATE employees SET salary = 5000000 WHERE employee_id = 1;

-- Session 2 attempts same row update
```

---

### 9-33. Row Locking

Lock row using SELECT FOR UPDATE. | SELECT FOR UPDATE로 행을 잠그세요.

```sql
START TRANSACTION;
SELECT * FROM employees WHERE employee_id = 1 FOR UPDATE;
-- Other sessions cannot modify
COMMIT;
```

---

### 9-34. Table Locking

Lock table using LOCK TABLES. | LOCK TABLES로 테이블을 잠그세요.

```sql
LOCK TABLES employees WRITE;
INSERT INTO employees VALUES (...);
UNLOCK TABLES;
```

---

### 9-35. TRUNCATE vs DELETE

Compare TRUNCATE and DELETE differences. | TRUNCATE와 DELETE의 차이를 비교하세요.

```sql
-- DELETE (can be rolled back)
DELETE FROM employees WHERE dept_id = 4;

-- TRUNCATE (cannot be rolled back)
TRUNCATE TABLE employees;
```

---

### 9-36. Bulk INSERT

Efficiently insert many rows. | 많은 수의 행을 효율적으로 삽입하세요.

```sql
INSERT INTO employees (name, dept_id, salary) VALUES
('Employee1', 1, 4000000),
('Employee2', 1, 4100000),
('Employee3', 2, 4200000),
...
('Employee100', 3, 4300000);
```

---

### 9-37. Batch UPDATE

Update multiple rows as batch. | 배치로 여러 행을 UPDATE하세요.

```sql
UPDATE employees
SET salary = CASE 
    WHEN employee_id IN (1, 2, 3) THEN salary * 1.1
    WHEN employee_id IN (4, 5, 6) THEN salary * 1.05
    ELSE salary
END;
```

---

### 9-38. INSERT IGNORE

Ignore duplicate key errors and INSERT. | 중복 키 오류를 무시하고 INSERT하세요.

```sql
INSERT IGNORE INTO employees (name, dept_id, salary)
VALUES ('Kim Chulsu', 1, 5000000);
```

---

### 9-39. INSERT ON DUPLICATE KEY UPDATE

Conditional INSERT that UPDATEs on duplicate. | 중복 시 UPDATE하는 조건부 INSERT를 수행하세요.

```sql
INSERT INTO employees (employee_id, name, salary)
VALUES (1, 'Kim Chulsu', 5500000)
ON DUPLICATE KEY UPDATE salary = VALUES(salary);
```

---

### 9-40. Transaction Log Tracking

Track transaction processing through logs. | 트랜잭션 처리 과정을 로그로 추적하세요.

```sql
-- Enable transaction logging
SET SESSION binlog_format = 'ROW';
START TRANSACTION;
UPDATE employees SET salary = 5000000 WHERE employee_id = 1;
COMMIT;
```

---

---

## 📝 Part 4: Assignment Instructions

### Theoretical Assignments

**Assignment 1**: Explain INSERT, UPDATE, and DELETE statements with examples. When should each be used? | INSERT, UPDATE, DELETE 문을 설명하고 예시를 제시하세요. 각각이 언제 사용되어야 하는지 설명하세요.

**Assignment 2**: Explain transaction concept and ACID properties. Why are transactions important in databases? | 트랜잭션의 개념과 ACID 특성을 설명하세요. 데이터베이스에서 트랜잭션이 중요한 이유는 무엇인지 설명하세요.

**Assignment 3**: Explain COMMIT, ROLLBACK, and SAVEPOINT with examples. When should each be used? | COMMIT, ROLLBACK, SAVEPOINT를 설명하고 예시를 제시하세요. 각각이 언제 사용되어야 하는지 설명하세요.

**Assignment 4**: Discuss data consistency issues that can occur without transactions. Provide real-world examples. | 트랜잭션 없이 발생할 수 있는 데이터 일관성 문제를 논의하세요. 실제 사례를 제시하세요.

**Assignment 5**: Compare safe and unsafe DML practices. Explain how to protect data integrity. | 안전한 DML 실습과 위험한 실습을 비교하세요. 데이터 무결성을 보호하는 방법을 설명하세요.

Submission Format: Word or PDF document (2-3 pages)

---

### Practical Assignments

**Assignment 1**: Write INSERT statements to add employees and departments. Include single and multiple row insertion, and insertion from SELECT. | INSERT 문으로 직원과 부서를 추가하세요. 단일 행 및 다중 행 삽입, SELECT로부터 삽입을 포함하세요.

**Assignment 2**: Write UPDATE statements: increase salary for specific department, update with conditions, update using JOIN. | UPDATE 문으로 특정 부서의 급여 인상, 조건부 수정, JOIN을 사용한 수정을 작성하세요.

**Assignment 3**: Write DELETE statements: safe deletion with WHERE condition, verify before deletion with SELECT. | DELETE 문으로 WHERE 조건을 포함한 안전한 삭제, SELECT로 먼저 확인을 작성하세요.

**Assignment 4**: Write transaction examples: successful transaction with COMMIT, failed transaction with ROLLBACK, multiple operations in transaction. | 트랜잭션 예시를 작성하세요: COMMIT으로 성공하는 경우, ROLLBACK으로 실패하는 경우, 여러 작업을 포함한 트랜잭션.

**Assignment 5**: Execute all Practice 9-1 to 9-40 queries with transaction management. Attach screenshots of results. Create 5 business scenarios requiring transactions and implement them. | 실습 9-1부터 9-40까지의 모든 쿼리를 트랜잭션으로 관리하며 실행하세요. 결과 스크린샷을 첨부하세요. 트랜잭션이 필요한 비즈니스 시나리오 5개를 작성하고 구현하세요.

Submission Format: SQL file (Ch9_DML_Transaction_[StudentID].sql) and result screenshots

---

Thank you for your hard work.

Prof. Cho Jeong-Hyun (peterchokr@gmail.com). Yeungnam University College
