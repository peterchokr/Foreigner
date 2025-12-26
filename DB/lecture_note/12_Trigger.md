# Chapter 12: Trigger

---

## 📖 Course Overview

In this chapter, you will learn about Triggers, database objects that automatically execute when specific events occur. Using triggers that automatically execute before/after INSERT, UPDATE, and DELETE operations, you will learn methods to ensure data integrity, automatically record audit logs, and maintain data consistency. The goal is to understand the powerful features and precautions of triggers. 

| 이 장에서는 특정 사건이 발생했을 때 자동으로 실행되는 데이터베이스 객체인 트리거(Trigger)를 학습합니다. INSERT, UPDATE, DELETE 이전/이후에 자동으로 실행되는 트리거를 사용하여 데이터 무결성을 보장하고, 감사 로그를 자동으로 기록하며, 데이터 일관성을 유지하는 방법을 다룹니다. 트리거의 강력한 기능과 주의사항을 이해하는 것이 목표입니다.

---

## 📚 Part 1: Theoretical Learning

### What You'll Learn in This Section

- Trigger concept and operation | 트리거의 개념과 작동 원리
- BEFORE and AFTER triggers | BEFORE와 AFTER 트리거
- INSERT, UPDATE, DELETE triggers | INSERT, UPDATE, DELETE 트리거
- NEW and OLD references | NEW와 OLD 참조
- Trigger usage cases | 트리거의 활용 사례
- Trigger performance impact | 트리거의 성능 영향

---

### 12.1 Trigger Concept

A **trigger** is a stored procedure that automatically executes when INSERT, UPDATE, or DELETE operations occur on a specific table. | **트리거**는 특정 테이블의 INSERT, UPDATE, DELETE 작업이 발생했을 때 자동으로 실행되는 저장 프로시저입니다.

**Characteristics:**

- Automatically executes (no explicit call needed) | 자동으로 실행 (명시적 호출 불필요)
- Ensures data integrity | 데이터 무결성 보장
- Performs monitoring and auditing functions | 감시 및 감사 기능 수행
- Implements complex business rules | 복잡한 비즈니스 규칙 구현

**Usage Cases:**

- Audit log recording | 감사 로그(Audit Log) 기록
- Data validation | 데이터 검증
- Automatic calculation | 자동 계산
- Data synchronization | 데이터 동기화

---

### 12.2 Creating Triggers

**Basic Syntax:**

```sql
CREATE TRIGGER trigger_name
BEFORE/AFTER INSERT/UPDATE/DELETE ON table_name
FOR EACH ROW
BEGIN
  -- Trigger body
  trigger_statements;
END;
```

**Timing:**

- **BEFORE**: Executes before operation (data validation and conversion) | 작업 전에 실행 (데이터 검증 및 변경)
- **AFTER**: Executes after operation (log recording, cascading operations) | 작업 후에 실행 (로그 기록, 연쇄 작업)

**Operations:**

- **INSERT**: When new row is inserted | 새로운 행 삽입 시
- **UPDATE**: When row is modified | 행 수정 시
- **DELETE**: When row is deleted | 행 삭제 시

---

### 12.3 NEW and OLD References

**NEW and OLD** access row values in triggers. | **NEW와 OLD**는 트리거에서 행의 값에 접근합니다.

```sql
-- INSERT trigger
NEW.column_name  -- Newly inserted value

-- UPDATE trigger
OLD.column_name  -- Value before modification
NEW.column_name  -- Value after modification

-- DELETE trigger
OLD.column_name  -- Deleted value
```

---

### 12.4 BEFORE Trigger - Data Validation

**BEFORE triggers** are used for data validation and conversion before operations. | **BEFORE 트리거**는 작업 전에 데이터를 검증하고 변환하는 데 사용됩니다.

**Example: Validate salary before INSERT:**

```sql
CREATE TRIGGER validate_salary_before_insert
BEFORE INSERT ON employees
FOR EACH ROW
BEGIN
  IF NEW.salary < 0 THEN
    SET NEW.salary = 0;
  END IF;
END;
```

---

### 12.5 AFTER Trigger - Audit Logging

**AFTER triggers** are used for logging and cascading operations after successful operations. | **AFTER 트리거**는 작업 성공 후 로깅과 연쇄 작업을 수행하는 데 사용됩니다.

**Example: Record audit log after UPDATE:**

```sql
CREATE TRIGGER log_salary_update
AFTER UPDATE ON employees
FOR EACH ROW
BEGIN
  IF OLD.salary != NEW.salary THEN
    INSERT INTO salary_audit_log (employee_id, old_salary, new_salary, changed_at)
    VALUES (NEW.employee_id, OLD.salary, NEW.salary, NOW());
  END IF;
END;
```

---

### 12.6 UPDATE Trigger

**UPDATE triggers** can reference both OLD and NEW values to track changes. | **UPDATE 트리거**는 OLD와 NEW 값을 모두 참조하여 변경 사항을 추적할 수 있습니다.

**Example: Prevent salary decrease:**

```sql
CREATE TRIGGER prevent_salary_decrease
BEFORE UPDATE ON employees
FOR EACH ROW
BEGIN
  IF NEW.salary < OLD.salary THEN
    SIGNAL SQLSTATE '45000'
    SET MESSAGE_TEXT = 'Salary cannot be decreased';
  END IF;
END;
```

---

### 12.7 DELETE Trigger

**DELETE triggers** reference OLD values and can enforce referential integrity. | **DELETE 트리거**는 OLD 값을 참조하며 참조 무결성을 강제할 수 있습니다.

**Example: Backup deleted data:**

```sql
CREATE TRIGGER backup_deleted_employee
BEFORE DELETE ON employees
FOR EACH ROW
BEGIN
  INSERT INTO employees_backup
  SELECT * FROM employees WHERE employee_id = OLD.employee_id;
END;
```

---

### 12.8 Trigger Advantages and Disadvantages

**Advantages:**

- Automatic enforcement of business rules | 비즈니스 규칙의 자동 적용
- Centralized data integrity logic | 데이터 무결성 로직의 중앙화
- Automatic audit logging | 자동 감사 로깅

**Disadvantages:**

- Performance impact | 성능 영향
- Difficult to debug and maintain | 디버깅 및 유지보수 어려움
- Hidden logic in database | 데이터베이스에 숨겨진 로직
- May cause cascading errors | 연쇄 오류 발생 가능

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
(3, 'Park Minjun', 2, 4500000);
```

### salary_audit_log Table

```sql
CREATE TABLE salary_audit_log (
    log_id INT PRIMARY KEY AUTO_INCREMENT,
    employee_id INT,
    old_salary DECIMAL(10, 2),
    new_salary DECIMAL(10, 2),
    changed_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

---

## 💻 Part 3: Practice

### What You'll Learn in This Section

- 다양한 형태의 트리거 작성
- 트리거 디버깅
- 감시 및 감사 기능 구현
- 성능 고려사항

---

### 12-1. 기본 INSERT 트리거

INSERT 시 자동으로 실행되는 트리거를 생성하세요.

```sql
CREATE TRIGGER log_new_employee
AFTER INSERT ON employees
FOR EACH ROW
BEGIN
  INSERT INTO employee_log (action, emp_id, emp_name, timestamp)
  VALUES ('INSERT', NEW.employee_id, NEW.name, NOW());
END;
```

---

### 12-2. INSERT BEFORE 트리거

삽입 전 데이터를 검증하는 트리거를 생성하세요.

```sql
CREATE TRIGGER validate_salary_before_insert
BEFORE INSERT ON employees
FOR EACH ROW
BEGIN
  IF NEW.salary < 0 THEN
    SIGNAL SQLSTATE '45000'
    SET MESSAGE_TEXT = 'Salary cannot be negative';
  END IF;
END;
```

---

### 12-3. INSERT AFTER 트리거

삽입 후 감사 로그를 기록하는 트리거를 생성하세요.

```sql
CREATE TRIGGER audit_new_employee
AFTER INSERT ON employees
FOR EACH ROW
BEGIN
  INSERT INTO audit_log (table_name, operation, old_value, new_value, changed_at)
  VALUES ('employees', 'INSERT', NULL, NEW.name, NOW());
END;
```

---

### 12-4. UPDATE 트리거

UPDATE 시 변경 사항을 추적하는 트리거를 생성하세요.

```sql
CREATE TRIGGER track_salary_update
AFTER UPDATE ON employees
FOR EACH ROW
BEGIN
  IF NEW.salary != OLD.salary THEN
    INSERT INTO salary_changes (emp_id, old_salary, new_salary, changed_date)
    VALUES (NEW.employee_id, OLD.salary, NEW.salary, NOW());
  END IF;
END;
```

---

### 12-5. UPDATE BEFORE 트리거

수정 전 데이터를 검증하는 트리거를 생성하세요.

```sql
CREATE TRIGGER validate_salary_update
BEFORE UPDATE ON employees
FOR EACH ROW
BEGIN
  IF NEW.salary > OLD.salary * 2 THEN
    SIGNAL SQLSTATE '45000'
    SET MESSAGE_TEXT = 'Salary increase too large';
  END IF;
  
  SET NEW.last_modified = NOW();
END;
```

---

### 12-6. UPDATE AFTER 트리거

수정 후 변경 기록을 저장하는 트리거를 생성하세요.

```sql
CREATE TRIGGER log_employee_update
AFTER UPDATE ON employees
FOR EACH ROW
BEGIN
  INSERT INTO update_history (emp_id, old_data, new_data, updated_at)
  VALUES (NEW.employee_id, 
          CONCAT('Dept:', OLD.dept_id, ' Sal:', OLD.salary),
          CONCAT('Dept:', NEW.dept_id, ' Sal:', NEW.salary),
          NOW());
END;
```

---

### 12-7. DELETE 트리거

삭제 시 데이터를 아카이브하는 트리거를 생성하세요.

```sql
CREATE TRIGGER archive_deleted_employee
BEFORE DELETE ON employees
FOR EACH ROW
BEGIN
  INSERT INTO employee_archive (employee_id, name, salary, deleted_date)
  VALUES (OLD.employee_id, OLD.name, OLD.salary, NOW());
END;
```

---

### 12-8. DELETE BEFORE 트리거

삭제 전 데이터를 검증하는 트리거를 생성하세요.

```sql
CREATE TRIGGER validate_delete
BEFORE DELETE ON employees
FOR EACH ROW
BEGIN
  IF (SELECT COUNT(*) FROM orders WHERE emp_id = OLD.employee_id) > 0 THEN
    SIGNAL SQLSTATE '45000'
    SET MESSAGE_TEXT = 'Cannot delete employee with active orders';
  END IF;
END;
```

---

### 12-9. DELETE AFTER 트리거

삭제 후 아카이브 테이블에 저장하는 트리거를 생성하세요.

```sql
CREATE TRIGGER log_deletion
AFTER DELETE ON employees
FOR EACH ROW
BEGIN
  INSERT INTO deletion_log (emp_id, emp_name, deleted_at)
  VALUES (OLD.employee_id, OLD.name, NOW());
END;
```

---

### 12-10. 조건부 트리거

특정 조건일 때만 실행되는 트리거를 생성하세요.

```sql
CREATE TRIGGER bonus_trigger
AFTER UPDATE ON employees
FOR EACH ROW
BEGIN
  IF NEW.salary > OLD.salary * 1.2 THEN
    INSERT INTO bonus_eligible VALUES (NEW.employee_id, NEW.salary * 0.1, NOW());
  END IF;
END;
```

---

### 12-11. 여러 작업의 트리거

INSERT, UPDATE, DELETE를 각각 처리하는 트리거들을 생성하세요.

```sql
CREATE TRIGGER emp_insert_log
AFTER INSERT ON employees FOR EACH ROW
INSERT INTO emp_changes VALUES ('INSERT', NEW.employee_id, NOW());

CREATE TRIGGER emp_update_log
AFTER UPDATE ON employees FOR EACH ROW
INSERT INTO emp_changes VALUES ('UPDATE', NEW.employee_id, NOW());

CREATE TRIGGER emp_delete_log
AFTER DELETE ON employees FOR EACH ROW
INSERT INTO emp_changes VALUES ('DELETE', OLD.employee_id, NOW());
```

---

### 12-12. NEW 값 사용

트리거에서 NEW를 사용하여 새로운 값에 접근하세요.

```sql
CREATE TRIGGER process_new_data
AFTER INSERT ON employees FOR EACH ROW
BEGIN
  IF NEW.salary > 5000000 THEN
    INSERT INTO premium_employees VALUES (NEW.employee_id, NEW.name, NEW.salary);
  END IF;
END;
```

---

### 12-13. OLD 값 사용

트리거에서 OLD를 사용하여 이전 값에 접근하세요.

```sql
CREATE TRIGGER track_old_values
AFTER UPDATE ON employees FOR EACH ROW
BEGIN
  IF OLD.dept_id != NEW.dept_id THEN
    INSERT INTO dept_changes VALUES (OLD.employee_id, OLD.dept_id, NEW.dept_id, NOW());
  END IF;
END;
```

---

### 12-14. NEW와 OLD 비교

수정 시 OLD와 NEW를 비교하여 변경을 감지하세요.

```sql
CREATE TRIGGER detect_changes
AFTER UPDATE ON employees FOR EACH ROW
BEGIN
  IF OLD.salary != NEW.salary OR OLD.dept_id != NEW.dept_id THEN
    INSERT INTO change_log (emp_id, change_details, changed_at)
    VALUES (NEW.employee_id,
            CONCAT('Old: ', OLD.salary, '-', OLD.dept_id,
                   ' New: ', NEW.salary, '-', NEW.dept_id),
            NOW());
  END IF;
END;
```

---

### 12-15. 데이터 검증 트리거

SIGNAL을 사용하여 오류를 발생시키세요.

```sql
CREATE TRIGGER validate_employee_data
BEFORE INSERT ON employees FOR EACH ROW
BEGIN
  IF CHAR_LENGTH(NEW.name) < 2 THEN
    SIGNAL SQLSTATE '45000'
    SET MESSAGE_TEXT = 'Employee name must be at least 2 characters';
  END IF;
  
  IF NEW.hire_date > CURDATE() THEN
    SIGNAL SQLSTATE '45000'
    SET MESSAGE_TEXT = 'Hire date cannot be in the future';
  END IF;
END;
```

---

### 12-16. 자동 계산 트리거

트리거로 자동으로 계산된 값을 저장하세요.

```sql
CREATE TRIGGER auto_calculate
BEFORE INSERT ON salary_records FOR EACH ROW
BEGIN
  SET NEW.gross_pay = NEW.base_salary + NEW.allowance;
  SET NEW.tax = NEW.gross_pay * 0.1;
  SET NEW.net_pay = NEW.gross_pay - NEW.tax;
END;
```

---

### 12-17. 감사 로그 트리거

모든 INSERT/UPDATE/DELETE를 로깅하는 트리거를 생성하세요.

```sql
CREATE TRIGGER full_audit_log
AFTER INSERT ON employees FOR EACH ROW
BEGIN
  INSERT INTO audit_trail (table_name, operation, record_id, data, audit_time)
  VALUES ('employees', 'INSERT', NEW.employee_id, 
          CONCAT(NEW.employee_id, '|', NEW.name, '|', NEW.salary), NOW());
END;
```

---

### 12-18. 동기화 트리거

다른 테이블을 자동으로 동기화하는 트리거를 생성하세요.

```sql
CREATE TRIGGER sync_employee_summary
AFTER INSERT ON employees FOR EACH ROW
BEGIN
  UPDATE dept_summary 
  SET emp_count = emp_count + 1
  WHERE dept_id = NEW.dept_id;
END;
```

---

### 12-19. 카운터 업데이트 트리거

행 추가/삭제 시 카운터를 자동 업데이트하세요.

```sql
CREATE TRIGGER update_emp_counter_insert
AFTER INSERT ON employees FOR EACH ROW
BEGIN
  UPDATE counter SET total_employees = total_employees + 1;
END;

CREATE TRIGGER update_emp_counter_delete
AFTER DELETE ON employees FOR EACH ROW
BEGIN
  UPDATE counter SET total_employees = total_employees - 1;
END;
```

---

### 12-20. 타임스탐프 트리거

수정 시간을 자동으로 기록하세요.

```sql
CREATE TRIGGER set_timestamp
BEFORE UPDATE on employees FOR EACH ROW
BEGIN
  SET NEW.last_modified = NOW();
END;
```

---

### 12-21. 중복 방지 트리거

중복 데이터 삽입을 방지하세요.

```sql
CREATE TRIGGER prevent_duplicate
BEFORE INSERT ON employees FOR EACH ROW
BEGIN
  IF EXISTS(SELECT 1 FROM employees WHERE name = NEW.name AND dept_id = NEW.dept_id) THEN
    SIGNAL SQLSTATE '45000'
    SET MESSAGE_TEXT = 'This employee already exists in this department';
  END IF;
END;
```

---

### 12-22. 외래키 무결성 트리거

외래키 무결성을 확인하는 트리거를 생성하세요.

```sql
CREATE TRIGGER check_dept_exists
BEFORE INSERT ON employees FOR EACH ROW
BEGIN
  IF NOT EXISTS(SELECT 1 FROM departments WHERE dept_id = NEW.dept_id) THEN
    SIGNAL SQLSTATE '45000'
    SET MESSAGE_TEXT = 'Invalid department ID';
  END IF;
END;
```

---

### 12-23. 범위 검증 트리거

값이 유효한 범위 내인지 확인하세요.

```sql
CREATE TRIGGER validate_range
BEFORE INSERT ON employees FOR EACH ROW
BEGIN
  IF NEW.salary < 2000000 OR NEW.salary > 10000000 THEN
    SIGNAL SQLSTATE '45000'
    SET MESSAGE_TEXT = 'Salary must be between 2000000 and 10000000';
  END IF;
END;
```

---

### 12-24. 포맷 변환 트리거

데이터를 자동으로 포맷 변환하세요.

```sql
CREATE TRIGGER format_conversion
BEFORE INSERT ON employees FOR EACH ROW
BEGIN
  SET NEW.name = UPPER(TRIM(NEW.name));
  SET NEW.hire_date = DATE(NEW.hire_date);
END;
```

---

### 12-25. 트리거 조회

생성된 트리거의 정보를 조회하세요.

```sql
SHOW TRIGGERS;
SHOW TRIGGERS FROM database_name;
SELECT * FROM INFORMATION_SCHEMA.TRIGGERS WHERE TRIGGER_SCHEMA = 'database_name';
```

---

### 12-26. 트리거 삭제

불필요한 트리거를 삭제하세요.

```sql
DROP TRIGGER IF EXISTS log_new_employee;
DROP TRIGGER IF EXISTS database_name.log_new_employee;
```

---

## 📝 Part 4: Assignment Instructions

### Theoretical Assignments

**Assignment 1**: Explain trigger concept and when triggers should be used. Discuss advantages and disadvantages. | 트리거의 개념과 언제 사용해야 하는지 설명하세요. 장점과 단점을 논의하세요.

**Assignment 2**: Explain BEFORE and AFTER triggers. When should each be used? Provide examples. | BEFORE와 AFTER 트리거를 설명하세요. 각각이 언제 사용되어야 하는지 설명하고 예시를 제시하세요.

**Assignment 3**: Explain NEW and OLD references in triggers with examples. How are they used for different operations? | NEW와 OLD 참조를 설명하고 예시를 제시하세요. 다양한 작업에 어떻게 사용되는지 설명하세요.

**Assignment 4**: Design a complete audit logging system using triggers. Explain trigger strategy and implementation. | 트리거를 사용한 완전한 감사 로깅 시스템을 설계하세요. 트리거 전략과 구현을 설명하세요.

**Assignment 5**: Discuss performance impact of triggers and optimization strategies. When should triggers be avoided? | 트리거의 성능 영향과 최적화 전략을 논의하세요. 트리거를 피해야 할 경우는 언제인지 설명하세요.

Submission Format: Word or PDF document (2-3 pages)

---

### Practical Assignments

**Assignment 1**: Create validation triggers: validate salary range, check employee status, ensure data quality. | 검증 트리거를 생성하세요: 급여 범위 검증, 직원 상태 확인, 데이터 품질 보장.

**Assignment 2**: Create audit log triggers: record all changes to employees table, track modifications with timestamp. | 감사 로그 트리거를 생성하세요: 직원 테이블의 모든 변경 기록, 타임스탐프와 함께 수정 추적.

**Assignment 3**: Create cascading triggers: update related tables when changes occur, maintain data consistency. | 연쇄 트리거를 생성하세요: 변경 시 관련 테이블 수정, 데이터 일관성 유지.

**Assignment 4**: Create error handling triggers with SIGNAL. Handle business rule violations. | SIGNAL을 포함한 오류 처리 트리거를 생성하세요. 비즈니스 규칙 위반을 처리하세요.

**Assignment 5**: Execute all Practice 12-1 to 12-40 queries with full functionality. Attach screenshots. Design complete trigger system for real business scenario. | 실습 12-1부터 12-40까지의 모든 쿼리를 실행하세요. 스크린샷을 첨부하세요. 실제 비즈니스 시나리오를 위한 완전한 트리거 시스템을 설계하세요.

Submission Format: SQL file (Ch12_Trigger_[StudentID].sql) and result screenshots

---

Thank you for your hard work.

Prof. Cho Jeong-Hyun (peterchokr@gmail.com). Yeungnam University College
