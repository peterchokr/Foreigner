# Chapter 12: Triggers

---

## 📖 Course Overview

In this chapter, you will learn about **triggers**, which are database objects that automatically execute in response to specific events on a particular table. Triggers are used to enforce data integrity, automatically record audit logs, and maintain data consistency when INSERT, UPDATE, or DELETE operations occur. You will understand trigger timing (BEFORE/AFTER), operation types (INSERT/UPDATE/DELETE), and how to use NEW and OLD references. The goal is to develop the ability to implement complex business rules automatically at the database level while avoiding common trigger pitfalls.

| 이 장에서는 특정 사건이 발생했을 때 자동으로 실행되는 데이터베이스 객체인 **트리거(Trigger)**를 학습합니다. INSERT, UPDATE, DELETE 작업이 발생할 때 트리거를 사용하여 데이터 무결성을 보장하고, 감사 로그를 자동으로 기록하며, 데이터 일관성을 유지하는 방법을 다룹니다. 트리거의 시점(BEFORE/AFTER), 작업 유형(INSERT/UPDATE/DELETE), 그리고 NEW와 OLD 참조 방법을 이해하게 됩니다. 데이터베이스 수준에서 복잡한 비즈니스 규칙을 자동으로 구현하면서 트리거의 주요 함정을 피하는 능력을 개발하는 것이 목표입니다.

---

## 📚 Part 1: Theoretical Learning

### What You'll Learn in This Section

- Concept and purpose of triggers
- Trigger timing: BEFORE vs AFTER
- Operation types: INSERT, UPDATE, DELETE triggers
- NEW and OLD references
- Data validation and transformation with BEFORE triggers
- Audit logging and cascading operations with AFTER triggers
- Trigger creation, querying, and deletion
- Performance considerations and common pitfalls

---

## 12.1 What is a Trigger?

A **trigger** is a stored procedure that automatically executes in response to specific events (INSERT, UPDATE, or DELETE) on a particular table.

| **트리거(Trigger)**는 특정 테이블에서 INSERT, UPDATE, 또는 DELETE 작업이 발생할 때 자동으로 실행되는 저장 프로시저입니다.

### Real-World Analogy

Think of a trigger like an automated response system:

- **Fire alarm system**: When heat is detected, the alarm automatically sounds
- **Automatic door**: When motion is detected, the door opens automatically
- **Database trigger**: When data changes, specific actions execute automatically

| **일상생활의 비유**
|
| - 비상 경보 시스템: 열이 감지되면 경보가 자동으로 울림
| - 자동 문: 움직임이 감지되면 자동으로 열림
| - 데이터베이스 트리거: 데이터가 변경되면 특정 작업이 자동으로 실행됨

### Key Characteristics

**1. Automatic Execution**

- No explicit calling required
- Executes automatically when the trigger event occurs

| - 명시적인 호출이 불필요
| - 트리거 이벤트가 발생하면 자동으로 실행

**2. Data Integrity Guarantee**

- Prevents invalid data entry
- Automatically validates business rules
- Maintains data consistency

| - 잘못된 데이터 입력 방지
| - 비즈니스 규칙을 자동으로 검증
| - 데이터 일관성 유지

**3. Monitoring and Audit Functions**

- Automatically records all data changes
- Tracks who changed what and when
- Implements security auditing

| - 모든 데이터 변경 사항을 자동으로 기록
| - 누가 무엇을 언제 변경했는지 추적
| - 보안 감사 구현

**4. Complex Business Logic Automation**

- Eliminates user error possibilities
- Enforces business rules at database level
- Reduces application code complexity

| - 사용자 실수 가능성 제거
| - 데이터베이스 수준에서 비즈니스 규칙 강제
| - 애플리케이션 코드 복잡성 감소

### Main Use Cases

```
✓ Audit Logging
  → Record all data changes with timestamp and user info

✓ Data Validation & Constraint Enforcement
  → Salary cannot be negative
  → Price must be >= 0
  → Email must be unique

✓ Automatic Calculation & Updates
  → Decrease inventory automatically when order placed
  → Calculate total amount automatically
  → Update timestamps

✓ Data Synchronization
  → Reflect changes in one table to another automatically
  → Keep related tables in sync

✓ Cascading Operations
  → Delete related records when parent is deleted
  → Archive data before deletion
  → Notify systems about changes
```

---

## 12.2 Trigger Syntax and Components

### Basic Syntax

```sql
CREATE TRIGGER trigger_name
BEFORE/AFTER INSERT/UPDATE/DELETE ON table_name
FOR EACH ROW
BEGIN
  -- Trigger body
  trigger_statements;
END;
```

### Understanding Each Part

**Trigger Name**

- Should clearly indicate purpose   |  트리거의 목적을 명확히 나타내야 함
- Convention: `{TIMING}_{OPERATION}_{TABLE_NAME}   |  관례: `{시점}_{작업}_{테이블명}``
- Examples: `before_insert_employees`, `after_update_salary   `

**Timing: BEFORE vs AFTER**

|      Timing      | Execution Time                | Purpose                                                 |
| :--------------: | :---------------------------- | :------------------------------------------------------ |
| **BEFORE** | Before the actual data change | Validate data, transform values, reject invalid records |
| **AFTER** | After the actual data change  | Record logs, perform cascading updates, notify systems  |

|                  | 실행 시기      | 목적                                     |
| :--------------: | :------------- | :--------------------------------------- |
| **BEFORE** | 데이터 변경 전 | 데이터 검증, 값 변환, 잘못된 레코드 거부 |
| **AFTER** | 데이터 변경 후 | 로그 기록, 연쇄 업데이트, 시스템 알림    |

**Operation Type: INSERT, UPDATE, DELETE**

- **INSERT**: When new row is added to table   | 테이블에 새 행이 추가될 때
- **UPDATE**: When existing row data is modified   | 기존 행의 데이터가 수정될 때
- **DELETE**: When row is removed from table   |  테이블에서 행이 삭제될 때

**FOR EACH ROW**

- Trigger executes once for each affected row   | 트리거는 영향을 받는 각 행마다 한 번씩 실행
- If you modify 100 rows, trigger fires 100 times   |  100개 행을 수정하면 트리거도 100번 실행
- Critical for performance considerations   | 성능 고려 시 중요한 요소

### Trigger Execution Flow

```
1️⃣ User executes INSERT/UPDATE/DELETE command
   ↓
2️⃣ BEFORE Trigger executes (if exists)
   • Validate data
   • Transform/modify values
   • Can reject operation (raise error)
   ↓
3️⃣ Actual data modification occurs
   (Commit to database)
   ↓
4️⃣ AFTER Trigger executes (if exists)
   • Record audit logs
   • Update related tables
   • Perform cascading operations
   ↓
5️⃣ All changes confirmed (if no errors)
   (Original operation + BEFORE changes + AFTER actions all committed)
```

---

## 12.3 NEW and OLD References

### Understanding NEW and OLD

Inside a trigger, you can access data using special variables **NEW** and **OLD**:

- **NEW**: Contains the new/modified value
- **OLD**: Contains the previous/original value

| 트리거 내에서는 **NEW**와 **OLD**라는 특수 변수를 사용하여 데이터에 접근합니다:
| - **NEW**: 새로운/수정된 값 포함
| - **OLD**: 이전/원래 값 포함

### Availability by Operation Type

```
INSERT Operation:
  NEW.column_name   ✓ Available (newly inserted value)
  OLD.column_name   ✗ NOT available (no previous value)

UPDATE Operation:
  NEW.column_name   ✓ Available (modified value)
  OLD.column_name   ✓ Available (previous value)

DELETE Operation:
  NEW.column_name   ✗ NOT available (no new value)
  OLD.column_name   ✓ Available (deleted value)
```

### Practical Example: Tracking Changes

```sql
-- Trigger to track salary changes
-- 급여 변경을 추적하는 트리거`
CREATE TRIGGER salary_update_log
AFTER UPDATE ON employees
FOR EACH ROW
BEGIN
  -- OLD.salary: Previous salary (e.g., 5,000,000)
  -- NEW.salary: New salary (e.g., 5,500,000)
  
  IF NEW.salary != OLD.salary THEN
    INSERT INTO salary_history (emp_id, old_salary, new_salary, change_date)
    VALUES (NEW.employee_id, OLD.salary, NEW.salary, NOW());
    -- Result: (1, 5000000, 5500000, 2024-01-06 15:30:00)
  END IF;
END;
```

### How This Trigger Works

```
1. Salary column is updated (salary: 5000000 → 5500000)
2. AFTER UPDATE trigger fires
3. OLD.salary = 5000000 (previous value)
4. NEW.salary = 5500000 (new value)
5. If values differ, insert record into salary_history
6. Change history automatically saved
```

| ``| 1. 급여 칼럼이 업데이트됨 (급여: 5000000 → 5500000) | 2. AFTER UPDATE 트리거 실행 | 3. OLD.salary = 5000000 (이전 값) | 4. NEW.salary = 5500000 (새 값) | 5. 값이 다르면 salary_history에 레코드 삽입 | 6. 변경 이력이 자동으로 저장됨 |``

---

## 12.4 BEFORE Trigger: Data Validation & Transformation

### Purpose of BEFORE Trigger

A BEFORE trigger executes **before the actual data is saved to the database**. It's used to:

- Validate data against business rules
- Transform or auto-populate values
- Reject invalid records with an error

| BEFORE 트리거는 **데이터가 실제로 저장되기 전에** 실행됩니다. 다음 용도로 사용됩니다:
| - 데이터를 비즈니스 규칙에 따라 검증
| - 값 변환 또는 자동 채우기
| - 잘못된 레코드를 오류로 거부

### Capabilities

**✓ Can Do:**

- Validate NEW values
- Modify NEW values
- Reject operation (SIGNAL error)
- Auto-set values (current date, timestamps, defaults)

| **✓ 할 수 있는 것:**
| NEW 값 검증
| NEW 값 수정
| 작업 거부 (SIGNAL 오류 발생)
| 자동 값 설정 (현재 날짜, 타임스탬프, 기본값)

**✗ Cannot Do:**

- Modify other tables
- Record logs (data not yet committed)
- Guarantee the trigger will actually commit

| **✗ 할 수 없는 것:**
| 다른 테이블 수정
| 로그 기록 (데이터가 아직 커밋되지 않음)
| 트리거 실행이 실제로 커밋될 것을 보장

### Practical Example 1: Data Validation

```sql
-- Enforce rule: Salary must be positive
-- 규칙 강제: 급여는 양수여야 함`
CREATE TRIGGER validate_salary
BEFORE INSERT ON employees
FOR EACH ROW
BEGIN
  -- Check for negative salary
  -- 음수 급여 검사
  IF NEW.salary < 0 THEN
    SIGNAL SQLSTATE '45000'
    SET MESSAGE_TEXT = 'Error: Salary cannot be negative.';
  END IF;
  
  -- Check salary limit for non-CEO positions
  -- CEO 아닌 사람의 급여 한도 검사
  IF NEW.salary > 100000000 AND NEW.position != 'CEO' THEN
    SIGNAL SQLSTATE '45000'
    SET MESSAGE_TEXT = 'Error: Only CEOs can earn over 100 million.';
  END IF;
END;
```

### How It Works

```
User tries: INSERT INTO employees VALUES (..., -100000);
  ↓
BEFORE trigger fires
  ↓
Checks: NEW.salary < 0?
  ↓
If yes: SIGNAL error ❌ (Data NOT inserted)
If no: Continue to actual insert ✅ (Data inserted)
```

### Practical Example 2: Auto-populate Values  자동 값 채우기

```sql
-- Auto-set hire date if not provided
-- 제공되지 않은 입사일 자동 설정
CREATE TRIGGER set_hire_date_on_insert
BEFORE INSERT ON employees
FOR EACH ROW
BEGIN
  -- If hire_date is NULL, set to today
  -- hire_date가 NULL이면 오늘 날짜 설정
  IF NEW.hire_date IS NULL THEN
    SET NEW.hire_date = CURDATE();
  END IF;
  
  -- If emp_level not specified, set default
  -- emp_level이 지정되지 않으면 기본값 설정
  IF NEW.emp_level IS NULL THEN
    SET NEW.emp_level = 'Level 1';
  END IF;
  
  -- Auto-set creation timestamp
  -- 생성 타임스탬프 자동 설정
  SET NEW.created_at = NOW();
END;
```

---

## 12.5 AFTER Trigger: Logging & Cascading Operations

### Purpose of AFTER Trigger

An AFTER trigger executes **after the data has been saved to the database**. It's used to:

- Record audit logs
- Update related tables
- Perform cascading operations
- Notify external systems

| AFTER 트리거는 **데이터가 저장된 후에** 실행됩니다. 다음 용도로 사용됩니다:
| - 감사 로그 기록
| - 관련 테이블 업데이트
| - 연쇄 작업 수행
| - 외부 시스템 알림

### Capabilities

**✓ Can Do:**

- Record logs
- Update other tables
- Perform cascading operations
- Notify systems about changes

| **✓ 할 수 있는 것:**
| - 로그 기록
| - 다른 테이블 업데이트
| - 연쇄 작업 수행
| - 시스템 변경 알림

**✗ Cannot Do:**

- Modify NEW/OLD values (data already committed)
- Reject the current operation (already committed)
- Prevent the triggering statement

| **✗ 할 수 없는 것:**
| - NEW/OLD 값 수정 (데이터가 이미 커밋됨)
| - 현재 작업 거부 (이미 커밋됨)
| - 트리거 명령 방지

### Practical Example 1: Audit Logging   감사 로그 기록

```sql
-- Log every employee update with change details
-- 모든 직원 업데이트를 변경 세부사항과 함께 기록
CREATE TRIGGER audit_employee_update
AFTER UPDATE ON employees
FOR EACH ROW
BEGIN
  -- Log name changes
  -- 이름 변경 기록
  IF NEW.name != OLD.name THEN
    INSERT INTO audit_log (table_name, operation, column_name, old_value, new_value, changed_at)
    VALUES ('employees', 'UPDATE', 'name', OLD.name, NEW.name, NOW());
  END IF;
  
  -- Log salary changes
  -- 급여 변경 기록
  IF NEW.salary != OLD.salary THEN
    INSERT INTO audit_log (table_name, operation, column_name, old_value, new_value, changed_at)
    VALUES ('employees', 'UPDATE', 'salary', OLD.salary, NEW.salary, NOW());
  END IF;
  
  -- Log department changes
  -- 부서 변경 기록
  IF NEW.department != OLD.department THEN
    INSERT INTO audit_log (table_name, operation, column_name, old_value, new_value, changed_at)
    VALUES ('employees', 'UPDATE', 'department', OLD.department, NEW.department, NOW());
  END IF;
END;
```

### Practical Example 2: Cascading Operations   연쇄 작업

```sql
-- When product is deleted, archive it and update related records
-- 상품이 삭제되면 아카이브하고 관련 레코드 업데이트
CREATE TRIGGER archive_product_on_delete
AFTER DELETE ON products
FOR EACH ROW
BEGIN
  -- Back up deleted product info
  -- 삭제된 상품 정보 백업
  INSERT INTO product_archive (product_id, product_name, price, stock, deleted_at)
  VALUES (OLD.product_id, OLD.product_name, OLD.price, OLD.stock, NOW());
  
  -- Update related order items to mark product as deleted
  -- 관련 주문 항목을 상품이 삭제되었음을 표시하도록 업데이트
  UPDATE order_items 
  SET product_name = CONCAT('[DELETED] ', OLD.product_name)
  WHERE product_id = OLD.product_id;
END;
```

---

## 12.6 Querying and Dropping Triggers

### View All Triggers   모든 트리거 조회

```sql
-- View all triggers in current database
-- 현재 데이터베이스의 모든 트리거 조회
SHOW TRIGGERS;

-- View triggers matching pattern
-- 패턴과 일치하는 트리거 조회
SHOW TRIGGERS LIKE 'salary%';

-- View detailed trigger information
-- 자세한 트리거 정보 조회
SELECT TRIGGER_NAME, EVENT_MANIPULATION, TRIGGER_TIME, ACTION_STATEMENT
FROM INFORMATION_SCHEMA.TRIGGERS
WHERE TRIGGER_SCHEMA = DATABASE();
```

### Drop a Trigger   트리거 삭제

```sql
-- Basic drop (will error if trigger doesn't exist)
-- 기본 삭제 (트리거가 없으면 오류)
DROP TRIGGER trigger_name;

-- Safe drop (no error if doesn't exist)
-- 안전한 삭제 (없으면 오류 아님)
DROP TRIGGER IF EXISTS trigger_name;

-- Drop from specific database
-- 특정 데이터베이스의 트리거 삭제
DROP TRIGGER database_name.trigger_name;
```

### Important Warnings

**⚠️ Before deleting a trigger:**

- Verify no other components depend on it
- Understand what the trigger does
- Consider the impact of removing automatic behavior
- Backup critical triggers before deletion

| **⚠️ 트리거를 삭제하기 전에:**
| - 다른 컴포넌트가 이 트리거에 의존하는지 확인
| - 트리거가 하는 일을 명확히 이해
| - 자동 동작 제거의 영향 고려
| - 중요한 트리거는 삭제 전 백업

---

## 12.7 Trigger Precautions and Performance

### ⚠️ Performance Impact

Every INSERT, UPDATE, or DELETE triggers the associated triggers. With high-volume operations:

- Triggers add overhead to every data operation
- Multiple cascading triggers cause exponential slowdown
- Large triggers with complex logic hurt performance

| 모든 INSERT, UPDATE, DELETE 작업이 관련 트리거를 실행합니다. 대량 작업 시:
| - 트리거는 모든 데이터 작업에 오버헤드 추가
| - 여러 연쇄 트리거로 인해 기하급수적 성능 저하
| - 복잡한 로직의 큰 트리거는 성능 저하

### Cascading Reactions Problem

```
Trigger A fires on INSERT to Table A
  ↓
Trigger A updates Table B
  ↓
Trigger B fires (triggered by Table B update)
  ↓
Trigger B updates Table C
  ↓
Trigger C fires...
  ↓
Complex, unpredictable behavior! 💥
```

| **트리거가 다른 트리거를 연쇄적으로 유발할 수 있습니다:**
|
| ``| 테이블 A INSERT로 트리거 A 실행 |   ↓ | 트리거 A가 테이블 B 업데이트 |   ↓ | 트리거 B 실행 (테이블 B 업데이트로 인해) |   ↓ | 트리거 B가 테이블 C 업데이트 |   ↓ | 트리거 C 실행... |   ↓ | 복잡하고 예측 불가능한 동작! 💥 |``

### Solution: Debugging with Logs   로그를 통한 디버깅

```sql
-- Create a debug log table for trigger tracking
-- 트리거 추적 위한 디버그 로그 테이블 생성
CREATE TABLE trigger_debug_log (
  log_id INT AUTO_INCREMENT PRIMARY KEY,
  trigger_name VARCHAR(100),
  operation VARCHAR(50),
  record_id INT,
  debug_message TEXT,
  created_at DATETIME
);

-- Add logging to your trigger
-- 트리거에 로깅 추가
CREATE TRIGGER debug_trigger
AFTER INSERT ON employees
FOR EACH ROW
BEGIN
  INSERT INTO trigger_debug_log 
  (trigger_name, operation, record_id, debug_message, created_at)
  VALUES ('debug_trigger', 'INSERT', NEW.employee_id, 'Employee added', NOW());
END;
```

### Compatibility Across Databases

**Database-specific syntax:**

- **MySQL**: Different trigger syntax from PostgreSQL
- **PostgreSQL**: Uses FUNCTION instead of TRIGGER body
- **SQL Server**: INSTEAD OF triggers, different syntax
- **Oracle**: Different timing and reference syntax

Never assume trigger syntax works everywhere!

| **데이터베이스별 문법 차이:**
| - **MySQL**: PostgreSQL과 다른 트리거 문법
| - **PostgreSQL**: 트리거 본문 대신 FUNCTION 사용
| - **SQL Server**: INSTEAD OF 트리거, 다른 문법
| - **Oracle**: 다른 시점과 참조 문법
|
| 트리거 문법이 어디서나 작동한다고 가정하지 마세요!

### Common Constraints

```
❌ Cannot use OLD in BEFORE INSERT trigger   BEFORE INSERT 트리거에서 OLD 사용 불가
   (INSERT has no previous value)

❌ Cannot use NEW in AFTER DELETE trigger   AFTER DELETE 트리거에서 NEW 사용 불가
   (DELETE has no new value)

❌ Cannot use COMMIT/ROLLBACK inside trigger   트리거 내에서 COMMIT/ROLLBACK 사용 불가
   (Trigger is part of transaction)

❌ Cannot create table inside trigger   트리거 내에서 테이블 생성 불가
   (Database structure changes not allowed)

❌ Be careful with recursive triggers   재귀 트리거 주의
   (Trigger can accidentally trigger itself)
```

---

## 📚 Part 2: Sample Database and Tables

### Create Sample Database and Tables

```sql
CREATE DATABASE ch12_trigger CHARACTER SET utf8mb4;
USE ch12_trigger;

-- Create sample tables
CREATE TABLE employees (
    employee_id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(50) NOT NULL,
    salary DECIMAL(10, 2),
    department VARCHAR(50),
    position VARCHAR(50),
    hire_date DATE,
    last_modified TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE salary_history (
    history_id INT PRIMARY KEY AUTO_INCREMENT,
    emp_id INT,
    emp_name VARCHAR(50),
    old_salary DECIMAL(10, 2),
    new_salary DECIMAL(10, 2),
    change_reason VARCHAR(100),
    changed_at DATETIME,
    FOREIGN KEY (emp_id) REFERENCES employees(employee_id)
);

CREATE TABLE audit_log (
    log_id INT PRIMARY KEY AUTO_INCREMENT,
    table_name VARCHAR(50),
    operation VARCHAR(10),
    column_name VARCHAR(50),
    old_value VARCHAR(255),
    new_value VARCHAR(255),
    changed_at DATETIME
);

CREATE TABLE employee_archive (
    archive_id INT PRIMARY KEY AUTO_INCREMENT,
    emp_id INT,
    emp_name VARCHAR(50),
    salary DECIMAL(10, 2),
    department VARCHAR(50),
    hire_date DATE,
    archived_at DATETIME,
    archived_reason VARCHAR(100)
);

-- Insert sample data
INSERT INTO employees (name, salary, department, position, hire_date) VALUES
('Kim Chul-soo', 5000000, 'Development', 'Developer', '2020-01-15'),
('Lee Young-hee', 4000000, 'HR', 'HR Manager', '2020-06-20'),
('Park Min-jun', 4500000, 'Marketing', 'Marketer', '2021-03-10');
```

---

## 💻 Part 3: Practical Exercises

### 12-1. Basic INSERT Trigger: Audit Logging

Create a trigger that automatically logs when new employees are added.

| 새로운 직원이 추가될 때 자동으로 로그 기록하는 트리거를 작성하세요.

```sql
CREATE TRIGGER log_new_employee
AFTER INSERT ON employees
FOR EACH ROW
BEGIN
  INSERT INTO audit_log 
  (table_name, operation, column_name, new_value, changed_at)
  VALUES ('employees', 'INSERT', 'employee_id', 
          CONCAT('ID:', NEW.employee_id, ' Name:', NEW.name), 
          NOW());
END;

-- Test it
INSERT INTO employees (name, salary, department, position, hire_date)
VALUES ('Jung Su-jin', 3500000, 'Finance', 'Accountant', '2024-01-06');

-- Verify
SELECT * FROM audit_log ORDER BY log_id DESC LIMIT 1;
```

---

### 12-2. BEFORE INSERT Trigger: Data Validation

Create a trigger that validates employee data before insertion.

| 직원 데이터를 삽입하기 전에 검증하는 트리거를 작성하세요.

```sql
CREATE TRIGGER validate_employee_insert
BEFORE INSERT ON employees
FOR EACH ROW
BEGIN
  -- Salary cannot be negative
  IF NEW.salary < 0 THEN
    SIGNAL SQLSTATE '45000'
    SET MESSAGE_TEXT = 'Error: Salary cannot be negative.';
  END IF;
  
  -- Non-CEO positions limited to 100 million salary
  IF NEW.salary > 100000000 AND NEW.position NOT IN ('CEO', 'Executive') THEN
    SIGNAL SQLSTATE '45000'
    SET MESSAGE_TEXT = 'Error: Only CEO/Executive can earn over 100 million.';
  END IF;
  
  -- Department is required
  IF NEW.department IS NULL THEN
    SIGNAL SQLSTATE '45000'
    SET MESSAGE_TEXT = 'Error: Department is required.';
  END IF;
  
  -- Auto-set hire_date if not provided
  IF NEW.hire_date IS NULL THEN
    SET NEW.hire_date = CURDATE();
  END IF;
END;

-- Test success case
INSERT INTO employees (name, salary, department, position)
VALUES ('Hwang Su-jung', 4100000, 'Marketing', 'Manager');

-- Test failure case (negative salary)
INSERT INTO employees (name, salary, department, position)
VALUES ('Kim Chul-soo', -5000000, 'Development', 'Developer');
-- Error: Salary cannot be negative.
```

---

### 12-3. AFTER UPDATE Trigger: Change Tracking

Create a trigger that tracks all salary changes.

| 모든 급여 변경을 추적하는 트리거를 작성하세요.

```sql
CREATE TRIGGER track_salary_update
AFTER UPDATE ON employees
FOR EACH ROW
BEGIN
  -- Only record if salary actually changed
  IF NEW.salary != OLD.salary THEN
    INSERT INTO salary_history 
    (emp_id, emp_name, old_salary, new_salary, change_reason, changed_at)
    VALUES (
      NEW.employee_id,
      NEW.name,
      OLD.salary,
      NEW.salary,
      'Annual increase',
      NOW()
    );
  END IF;
END;

-- Test it
UPDATE employees 
SET salary = 5500000 
WHERE employee_id = 1;

-- Verify
SELECT * FROM salary_history ORDER BY history_id DESC LIMIT 1;
```

---

### 12-4. BEFORE UPDATE Trigger: Update Validation

Create a trigger that prevents excessive salary increases.

| 과도한 급여 인상을 방지하는 트리거를 작성하세요.

```sql
CREATE TRIGGER validate_salary_update
BEFORE UPDATE ON employees
FOR EACH ROW
BEGIN
  DECLARE raise_percent DECIMAL(5, 2);
  
  -- Calculate raise percentage
  SET raise_percent = ROUND(
    (NEW.salary - OLD.salary) / OLD.salary * 100, 2
  );
  
  -- Prevent raises over 50%
  IF raise_percent > 50 THEN
    SIGNAL SQLSTATE '45000'
    SET MESSAGE_TEXT = 'Error: Salary increase cannot exceed 50%.';
  END IF;
  
  -- Prevent decreases (unless position changed)
  IF NEW.salary < OLD.salary AND NEW.position = OLD.position THEN
    SIGNAL SQLSTATE '45000'
    SET MESSAGE_TEXT = 'Error: Salary decrease not allowed without position change.';
  END IF;
  
  -- Auto-update modification timestamp
  SET NEW.last_modified = NOW();
END;

-- Test success
UPDATE employees SET salary = 5500000 WHERE employee_id = 1;

-- Test failure (>50% increase)
UPDATE employees SET salary = 10000000 WHERE employee_id = 1;
-- Error: Salary increase cannot exceed 50%.
```

---

### 12-5. AFTER DELETE Trigger: Data Archiving

Create a trigger that archives deleted employees.

| 삭제된 직원을 아카이브하는 트리거를 작성하세요.

```sql
CREATE TRIGGER archive_deleted_employee
BEFORE DELETE ON employees
FOR EACH ROW
BEGIN
  -- Archive the deleted employee record
  INSERT INTO employee_archive 
  (emp_id, emp_name, salary, department, hire_date, archived_at, archived_reason)
  VALUES (
    OLD.employee_id,
    OLD.name,
    OLD.salary,
    OLD.department,
    OLD.hire_date,
    NOW(),
    'Retirement'
  );
  
  -- Also log to audit
  INSERT INTO audit_log 
  (table_name, operation, column_name, old_value, changed_at)
  VALUES ('employees', 'DELETE', 'employee_id', 
          CONCAT('ID:', OLD.employee_id, ' Name:', OLD.name), 
          NOW());
END;

-- Test it
DELETE FROM employees WHERE employee_id = 3;

-- Verify archiving
SELECT * FROM employee_archive;
```

---

## 📝 Part 4: Assignment Instructions

### Theoretical Assignments

**Assignment 1**: Explain the concept of triggers and provide 5 specific scenarios where triggers should be used.

| **과제 1**: 트리거의 개념을 설명하고 트리거를 사용해야 할 5가지 구체적인 상황을 제시하세요.

**Assignment 2**: Compare BEFORE and AFTER triggers, explaining when each should be used and why.

| **과제 2**: BEFORE 트리거와 AFTER 트리거를 비교하여 각각을 언제, 왜 사용해야 하는지 설명하세요.

**Assignment 3**: Create a table showing which NEW and OLD references are available for INSERT, UPDATE, and DELETE operations.

| **과제 3**: INSERT, UPDATE, DELETE 작업에서 NEW와 OLD 참조 가용성을 표로 정리하세요.

**Assignment 4**: Discuss performance impacts of triggers and propose optimization strategies.

| **과제 4**: 트리거의 성능 영향을 논의하고 최적화 전략을 제안하세요.

**Assignment 5**: Explain common trigger pitfalls and how to avoid them using real examples.

| **과제 5**: 일반적인 트리거 함정과 피하는 방법을 실제 사례를 들어 설명하세요.

Submit as: Word or PDF document (2-3 pages)

---

### Practical Assignments

**Assignment 1**: Create validation triggers for:

- Negative salary prevention
- Email uniqueness enforcement
- Department requirement validation

| **과제 1**: 다음 검증 트리거 작성:
| - 음수 급여 방지
| - 이메일 유일성 강제
| - 부서 필수 검증

**Assignment 2**: Create audit logging triggers for:

- All INSERT operations
- All UPDATE operations with before/after values
- All DELETE operations with archiving

| **과제 2**: 다음 감사 로깅 트리거 작성:
| - 모든 INSERT 작업
| - 모든 UPDATE 작업 (변경 전후 값)
| - 모든 DELETE 작업 (아카이빙 포함)

**Assignment 3**: Implement cascading operation triggers:

- Inventory updates when orders placed
- Balance updates when transactions occur
- Cascade deletion with archiving

| **과제 3**: 연쇄 작업 트리거 구현:
| - 주문 시 재고 업데이트
| - 거래 시 잔액 업데이트
| - 아카이빙을 통한 연쇄 삭제

**Assignment 4**: Create all triggers from exercises 12-1 through 12-5, test their functionality with sample data operations, and provide screenshots of successful execution.

| **과제 4**: 실습 12-1부터 12-5까지 모든 트리거 생성, 샘플 데이터로 기능 테스트, 성공 스크린샷 제출.

**Assignment 5**: Design and implement a comprehensive trigger system for a real business scenario (e-commerce, bank, hospital, etc.) with at least 8 triggers. Explain each trigger's purpose and demonstrate its functionality.

| **과제 5**: 실제 비즈니스 시나리오(전자상거래, 은행, 병원 등)에 대한 8개 이상의 트리거로 구성된 종합 트리거 시스템 설계 및 구현. 각 트리거의 목적을 설명하고 기능 입증.

Submit as: SQL file (Ch12_Trigger_[StudentID].sql) and screenshots

---

Congratulations on completing this chapter!

Prof. Cho Jeong-Hyun (peterchokr@gmail.com). Yeungnam University College
