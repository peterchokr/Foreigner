# Ch12 Triggers - Exercise Problems

Dear students! After completing Chapter 12, please solve the following exercises to verify your understanding.
Check the difficulty level of each section and study step by step.

---

## 📌 Learning Objectives Verification

After completing Chapter 12, you should understand:

- Trigger concept and operation
- Difference between BEFORE and AFTER triggers
- INSERT, UPDATE, DELETE triggers
- NEW and OLD references
- Trigger use cases (audit logs, data validation)
- Trigger creation, viewing, deletion

---

# Multiple Choice Questions (10 Questions)

## Beginner Level (5 Questions) - Basic Concept Verification

**Question 1** Correct definition of Trigger?

- ① Procedure called manually
- ② Auto-executes when DML work occurs on specific table
- ③ Query for data retrieval
- ④ DDL command for table modification

---

**Question 2** Main purpose of BEFORE trigger?

- ① Log recording
- ② Data validation and auto transformation
- ③ Perform cascading work
- ④ Data retrieval

---

**Question 3** Main purpose of AFTER trigger?

- ① Data validation
- ② Modify original data
- ③ Log recording and cascading work
- ④ Query optimization

---

**Question 4** Meaning of NEW and OLD in trigger?

- ① Old and new data
- ② Trigger execution order
- ③ Two names for table
- ④ Database versions

---

**Question 5** What's usable in UPDATE trigger?

- ① Only NEW
- ② Only OLD
- ③ Both NEW and OLD
- ④ Variables only

---

## Intermediate Level (3 Questions) - Concept Application

**Question 6** Purpose of this INSERT trigger with NEW reference:

```sql
CREATE TRIGGER validate_salary
BEFORE INSERT ON employees
FOR EACH ROW
BEGIN
  IF NEW.salary < 0 THEN
    SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Negative not allowed';
  END IF;
END;
```

- ① Insert only negative salary employees
- ② Prevent negative salary insertion
- ③ Auto calculate salary
- ④ Validate all data

---

**Question 7** Difference between Trigger and Procedure?

- ① Trigger auto-executes, Procedure manual
- ② Procedure auto-executes, Trigger manual
- ③ Same functionality
- ④ Performance only

---

**Question 8** Command to delete trigger?

- ① DELETE TRIGGER trigger_name;
- ② DROP TRIGGER trigger_name;
- ③ REMOVE TRIGGER trigger_name;
- ④ TRUNCATE TRIGGER trigger_name;

---

## Advanced Level (2 Questions) - Critical Thinking

**Question 9** Difference between BEFORE INSERT and AFTER INSERT?

- ① BEFORE for validation, AFTER for logging
- ② AFTER can modify data
- ③ BEFORE for logging only
- ④ No difference

---

**Question 10** Precautions when using Trigger?

- ① Performance degradation possible
- ② Difficult debugging
- ③ Unexpected cascading effects
- ④ All of ①②③

---

# Short Answer Questions (5 Questions)

## Beginner Level (3 Questions)

**Question 11** Define Trigger and explain why it auto-executes.

---

**Question 12** Explain difference between BEFORE and AFTER triggers with use case examples.

---

**Question 13** Explain meaning of NEW and OLD references and their availability in INSERT, UPDATE, DELETE triggers.

---

## Intermediate Level (1 Question)

**Question 14** Explain Audit Log trigger concept and design trigger for salary change monitoring.

---

## Advanced Level (1 Question)

**Question 15** Analyze advantages and disadvantages of Triggers and explain when to implement logic in application instead.

---

# Practical Problems (5 Questions)

## Beginner Level (2 Questions)

**Question 16** Execute following SQL and provide result screenshot.

```sql
CREATE DATABASE ch12_trigger CHARACTER SET utf8mb4;
USE ch12_trigger;

CREATE TABLE employees (
    employee_id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(30),
    salary INT,
    hire_date DATE
);

CREATE TABLE salary_history (
    history_id INT PRIMARY KEY AUTO_INCREMENT,
    emp_id INT,
    old_salary INT,
    new_salary INT,
    change_date DATETIME
);

INSERT INTO employees VALUES
(1, 'Kim Chulsu', 5000000, '2020-01-15'),
(2, 'Lee Younghee', 4000000, '2020-06-20');

SELECT * FROM employees;
```

Submission: Screenshot showing employees table with 2 records

---

**Question 17** Create and execute AFTER UPDATE trigger for salary change monitoring.

```sql
-- Create trigger
CREATE TRIGGER log_salary_update
AFTER UPDATE ON employees
FOR EACH ROW
BEGIN
  IF NEW.salary != OLD.salary THEN
    INSERT INTO salary_history (emp_id, old_salary, new_salary, change_date)
    VALUES (NEW.employee_id, OLD.salary, NEW.salary, NOW());
  END IF;
END;

-- Modify salary
UPDATE employees SET salary = 5500000 WHERE employee_id = 1;

-- Verify results
SELECT * FROM employees WHERE employee_id = 1;
SELECT * FROM salary_history;
```

Submission: Screenshot showing employees and salary_history results

---

## Intermediate Level (2 Questions)

**Question 18** Create and execute BEFORE INSERT validation trigger.

```sql
-- Prevent negative salary and auto-set hire_date
CREATE TRIGGER validate_salary_insert
BEFORE INSERT ON employees
FOR EACH ROW
BEGIN
  IF NEW.salary < 0 THEN
    SIGNAL SQLSTATE '45000'
    SET MESSAGE_TEXT = 'Salary cannot be negative';
  END IF;
  
  -- Auto-set hire_date
  IF NEW.hire_date IS NULL THEN
    SET NEW.hire_date = CURDATE();
  END IF;
END;

-- Normal INSERT (hire_date auto-set)
INSERT INTO employees (name, salary) VALUES ('Park Minjun', 4500000);

-- Negative salary attempt (error)
-- INSERT INTO employees VALUES (NULL, 'Jung Junho', -3000000, '2024-01-01');

-- Verify results
SELECT * FROM employees WHERE name = 'Park Minjun';
```

Submission: Screenshot of INSERT result and trigger behavior

---

**Question 19** Create BEFORE DELETE trigger for data archiving.

```sql
-- Create archive table
CREATE TABLE employee_archive (
    archive_id INT PRIMARY KEY AUTO_INCREMENT,
    emp_id INT,
    emp_name VARCHAR(30),
    emp_salary INT,
    deleted_date DATETIME
);

-- Create DELETE trigger
CREATE TRIGGER archive_employee_delete
BEFORE DELETE ON employees
FOR EACH ROW
BEGIN
  INSERT INTO employee_archive (emp_id, emp_name, emp_salary, deleted_date)
  VALUES (OLD.employee_id, OLD.name, OLD.salary, NOW());
END;

-- Delete employee
DELETE FROM employees WHERE employee_id = 2;

-- Verify results
SELECT * FROM employees;
SELECT * FROM employee_archive;
```

Submission: Screenshot of DELETE and archive results

---

## Advanced Level (1 Question)

**Question 20** Write and execute complex Triggers.

```
Requirements:
1. Salary raise monitoring trigger
   - Log when salary increases 20%+
   - Enforce salary ceiling (6,000,000 max)

2. Data integrity trigger
   - Auto-set default hire_date
   - Prevent negative salary
   - Log salary changes

3. Cascading trigger
   - Delete salary history with employee
   - Record in archive table

4. Trigger verification
   - View all created triggers
   - Verify trigger operations

Submission:
   - SQL code for each trigger
   - Execution result screenshot
   - Trigger list (SHOW TRIGGERS)
```

---

---

# 📋 Answer Key and Model Answers

## Multiple Choice Answers (10 Questions)

| Question | Answer | Explanation                              |
| :------: | :----: | :--------------------------------------- |
|    1    |   ②   | Trigger auto-executes on DML             |
|    2    |   ②   | BEFORE for validation and transformation |
|    3    |   ③   | AFTER for logging and cascading          |
|    4    |   ①   | NEW and OLD are new/old data             |
|    5    |   ③   | UPDATE can use both NEW and OLD          |
|    6    |   ②   | Prevent negative salary                  |
|    7    |   ①   | Trigger auto, Procedure manual           |
|    8    |   ②   | DROP TRIGGER deletes                     |
|    9    |   ①   | BEFORE validation, AFTER logging         |
|    10    |   ④   | All ①②③ precautions                   |

---

## Short Answer Model Answers (5 Questions)

### Question 11: Trigger Definition and Auto-Execution

**Model Answer**:

```
Definition:
- Stored procedure that auto-executes when DML (INSERT, UPDATE, DELETE)
  occurs on specific table

Auto-Execution Reason:
- Trigger stored as database object
- DBMS checks automatically on DML
- Executes without explicit call
- Enforces business rules automatically

Advantages:
- Consistent data integrity
- Eliminate code duplication
- Automate audit functions
```

---

### Question 12: BEFORE and AFTER Triggers

**Model Answer**:

```
BEFORE Trigger:
- Timing: Before DML execution
- NEW/OLD: Can reference NEW (INSERT: only NEW, UPDATE: both)
- Purpose: Data validation, auto transformation
- Can refuse: SIGNAL can prevent operation

Example:
CREATE TRIGGER validate_email
BEFORE INSERT ON users
FOR EACH ROW
BEGIN
  IF NEW.email NOT LIKE '%@%' THEN
    SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Valid email required';
  END IF;
END;

AFTER Trigger:
- Timing: After DML execution
- NEW/OLD: Both available (read-only)
- Purpose: Log recording, cascading work, notification
- Cannot refuse: Operation already complete

Example:
CREATE TRIGGER log_user_changes
AFTER UPDATE ON users
FOR EACH ROW
BEGIN
  IF NEW.status != OLD.status THEN
    INSERT INTO audit_log VALUES (...);
  END IF;
END;

Use Cases:
BEFORE: Input validation, default values, value transformation
AFTER: Audit logs, statistics update, auto-update other tables
```

---

### Question 13: NEW and OLD References

**Model Answer**:

```
NEW:
- Meaning: New value to be inserted or modified
- INSERT trigger: NEW available (new data)
- UPDATE trigger: NEW available (modified value)
- DELETE trigger: NEW not available

OLD:
- Meaning: Value before modification or deletion
- INSERT trigger: OLD not available
- UPDATE trigger: OLD available (before modification)
- DELETE trigger: OLD available (deleted value)

Availability:

             | NEW | OLD |
INSERT BEFORE|  ✓ |  ✗ |
UPDATE BEFORE|  ✓ |  ✓ |
DELETE BEFORE|  ✗ |  ✓ |

Example:
-- Detect change in UPDATE
IF NEW.salary != OLD.salary THEN
  -- Record salary change
END IF;

-- Log deletion
INSERT INTO archive SELECT OLD.*;
```

---

### Question 14: Audit Log Trigger

**Model Answer**:

```
Concept:
- Trigger that records all data changes
- Tracks when, who, what changed
- Database security and monitoring

Design (Salary Change Monitoring):

CREATE TABLE salary_audit (
  audit_id INT PRIMARY KEY AUTO_INCREMENT,
  emp_id INT,
  old_salary INT,
  new_salary INT,
  change_date DATETIME,
  change_percent DECIMAL(5,2)
);

CREATE TRIGGER track_salary_changes
AFTER UPDATE ON employees
FOR EACH ROW
BEGIN
  IF NEW.salary != OLD.salary THEN
    INSERT INTO salary_audit VALUES (
      NULL,
      NEW.employee_id,
      OLD.salary,
      NEW.salary,
      NOW(),
      ROUND((NEW.salary - OLD.salary) / OLD.salary * 100, 2)
    );
  END IF;
END;

Usage:
UPDATE employees SET salary = 5500000 WHERE employee_id = 1;

Verify:
SELECT * FROM salary_audit;
-- All change details, timestamps, raise percentages recorded
```

---

### Question 15: Trigger Advantages and Disadvantages

**Model Answer**:

```
Advantages:
1. Automate data integrity
   - Auto-enforce business rules
   - Apply to all data paths

2. Audit and security
   - Record all changes
   - Control access

3. Automate complex logic
   - Auto-perform complex calculations
   - Data synchronization

Disadvantages:
1. Performance degradation
   - Overhead on all DML
   - Critical with cascading

2. Difficult debugging
   - Hidden logic
   - Unexpected cascading
   - Hard to trace performance issues

3. Complex maintenance
   - Trigger dependencies
   - Migration difficulties

4. Low reusability
   - DB-specific
   - Hard to integrate

Trigger vs Application Logic:

Use Trigger:
✅ Need to apply to all paths (direct SQL, application)
✅ Database itself needs protection
✅ Simple validation

Use Application Logic:
✅ Complex business logic
✅ External system integration needed
✅ Testing and debugging easier
✅ Performance optimization needed
```

---

## Practical Problems Model Answers (5 Questions)

### Question 16: Create employees Table

**Completion Criteria**:
✅ ch12_trigger database created
✅ employees and salary_history tables
✅ 2 employees inserted

---

### Question 17: UPDATE Trigger

**Completion Criteria**:
✅ log_salary_update trigger created
✅ UPDATE executed (5000000 → 5500000)
✅ salary_history records change

---

### Question 18: BEFORE INSERT Trigger

**Completion Criteria**:
✅ validate_salary_insert trigger created
✅ Auto hire_date setting verified
✅ Negative salary prevention verified

---

### Question 19: BEFORE DELETE Trigger

**Completion Criteria**:
✅ archive_employee_delete trigger created
✅ DELETE executed
✅ Backup confirmed in employee_archive

---

### Question 20: Complex Triggers

**Model Answer**:

```sql
-- 1. Salary raise monitoring + ceiling
CREATE TRIGGER salary_raise_check
BEFORE UPDATE ON employees
FOR EACH ROW
BEGIN
  DECLARE raise_pct INT;
  
  -- Calculate raise percentage
  SET raise_pct = ROUND((NEW.salary - OLD.salary) / OLD.salary * 100);
  
  -- Enforce ceiling
  IF NEW.salary > 6000000 THEN
    SET NEW.salary = 6000000;
  END IF;
  
  -- Log big raises
  IF raise_pct > 20 THEN
    INSERT INTO salary_audit (note) VALUES (CONCAT('High raise:', raise_pct, '%'));
  END IF;
END;

-- 2. Data integrity
CREATE TRIGGER employee_insert_validation
BEFORE INSERT ON employees
FOR EACH ROW
BEGIN
  IF NEW.salary < 0 THEN
    SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Negative salary not allowed';
  END IF;
  
  IF NEW.hire_date IS NULL THEN
    SET NEW.hire_date = CURDATE();
  END IF;
END;

-- 3. UPDATE history log
CREATE TRIGGER salary_history_log
AFTER UPDATE ON employees
FOR EACH ROW
BEGIN
  IF NEW.salary != OLD.salary THEN
    INSERT INTO salary_history (emp_id, old_salary, new_salary, change_date)
    VALUES (NEW.employee_id, OLD.salary, NEW.salary, NOW());
  END IF;
END;

-- 4. DELETE handling
CREATE TRIGGER delete_employee_cascade
BEFORE DELETE ON employees
FOR EACH ROW
BEGIN
  -- Archive
  INSERT INTO employee_archive SELECT NULL, OLD.*;
  
  -- Log salary history
  INSERT INTO salary_history VALUES (NULL, OLD.employee_id, OLD.salary, 0, NOW());
END;

-- Verify
SHOW TRIGGERS;
```

---

Prof. Jeong-Hyun Cho (peterchokr@gmail.com)
Yeungnam University College
