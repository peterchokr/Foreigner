# Ch11 Control Structures and Loops - Exercise Problems

Dear students! After completing Chapter 11, please solve the following exercises to verify your understanding.
Check the difficulty level of each section and study step by step.

---

## 📌 Learning Objectives Verification

After completing Chapter 11, you should understand:

- IF-THEN-ELSE statement structure
- CASE statements (simple and searched forms)
- WHILE, REPEAT, LOOP loops
- Nested control structures
- Labels and loop control (LEAVE)
- Cursors for row processing

---

# Multiple Choice Questions (10 Questions)

## Beginner Level (5 Questions) - Basic Concept Verification

**Question 1** Correct structure of IF-THEN-ELSE statement?

- ① IF condition THEN statement; END;
- ② IF condition THEN statement; ELSE statement; END IF;
- ③ IF (condition) { statement; }
- ④ IF condition BEGIN statement; END;

---

**Question 2** Simple form of CASE statement (Simple CASE)?

- ① CASE WHEN condition THEN statement;
- ② CASE variable WHEN value THEN statement;
- ③ CASE condition WHEN value THEN statement;
- ④ CASE THEN statement;

---

**Question 3** Characteristic of WHILE loop?

- ① Executes minimum once unconditionally
- ② Check condition first then loop
- ③ Loop without checking condition
- ④ Exit with BREAK

---

**Question 4** Difference between REPEAT-UNTIL and WHILE?

- ① REPEAT checks condition first
- ② WHILE executes once unconditionally
- ③ REPEAT executes once unconditionally (then check condition)
- ④ No difference

---

**Question 5** How to exit LOOP?

- ① BREAK;
- ② EXIT;
- ③ LEAVE label_name;
- ④ STOP;

---

## Intermediate Level (3 Questions) - Concept Application

**Question 6** Correct use of ELSEIF?

```sql
① IF score >= 90 THEN SET grade = 'A';
   ELSEIF score >= 80 THEN SET grade = 'B';
   ELSE SET grade = 'C';
   END IF;

② IF score >= 90 THEN SET grade = 'A';
   ELSE IF score >= 80 THEN SET grade = 'B';
   END IF;
   END IF;
```

- ① Correct
- ② Correct
- ③ Only ① correct
- ④ Only ② correct

---

**Question 7** Searched form of CASE statement?

```sql
① CASE month_num WHEN 1 THEN '1월'

② CASE
   WHEN salary >= 5000000 THEN 'High'
   WHEN salary >= 4000000 THEN 'Medium'
   END CASE;
```

- ① Simple form
- ② Searched form
- ③ Both same form
- ④ Both searched form

---

**Question 8** Example of nested control structures?

- ① Use WHILE, IF, CASE together
- ② Use IF with CASE, CASE with IF
- ③ Use IF and CASE sequentially
- ④ ① and ②

---

## Advanced Level (2 Questions) - Critical Thinking

**Question 9** Main purpose of Cursor?

- ① Process multiple rows at once
- ② Repeatedly process each row of query result
- ③ Only check row count
- ④ Select table

---

**Question 10** Relationship between Label and LEAVE?

```sql
my_loop: LOOP
  IF condition THEN
    LEAVE my_loop;
  END IF;
END LOOP;
```

- ① Label identifies LOOP, LEAVE exits
- ② Can exit with LEAVE alone
- ③ Label is optional
- ④ Not required for LOOP exit

---

# Short Answer Questions (5 Questions)

## Beginner Level (3 Questions)

**Question 11** Explain IF-THEN-ELSE structure and how to use ELSEIF.

---

**Question 12** Explain two forms of CASE statement (simple and searched) and provide usage examples.

---

**Question 13** Explain differences between WHILE, REPEAT, LOOP loops and compare characteristics.

---

## Intermediate Level (1 Question)

**Question 14** Explain nested control structures and write example including IF-THEN-CASE-WHILE.

---

## Advanced Level (1 Question)

**Question 15** Explain Cursor concept, usage methods and situations requiring Cursor.

---

# Practical Problems (5 Questions)

## Beginner Level (2 Questions)

**Question 16** Execute following SQL and provide result screenshot.

```sql
CREATE DATABASE ch11_control_structure CHARACTER SET utf8mb4;
USE ch11_control_structure;

CREATE TABLE employees (
    employee_id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(30),
    salary INT
);

INSERT INTO employees VALUES
(1, 'Kim Chulsu', 5000000),
(2, 'Lee Younghee', 4000000),
(3, 'Park Minjun', 4500000);

SELECT * FROM employees;
```

Submission: Screenshot showing employees table with 3 records

---

**Question 17** Create and execute procedures using IF-THEN-ELSE and CASE.

```sql
-- 1. IF-THEN-ELSE procedure for salary level
CREATE PROCEDURE CheckSalaryLevel (IN emp_id INT)
BEGIN
  DECLARE emp_salary INT;
  SELECT salary INTO emp_salary FROM employees WHERE employee_id = emp_id;
  
  IF emp_salary > 4500000 THEN
    SELECT CONCAT(emp_salary, ' - High Salary');
  ELSEIF emp_salary > 4000000 THEN
    SELECT CONCAT(emp_salary, ' - Medium Salary');
  ELSE
    SELECT CONCAT(emp_salary, ' - Low Salary');
  END IF;
END;

CALL CheckSalaryLevel(1);
CALL CheckSalaryLevel(2);

-- 2. CASE statement for grade assignment
CREATE PROCEDURE AssignGrade (IN emp_id INT, OUT grade CHAR)
BEGIN
  DECLARE emp_salary INT;
  SELECT salary INTO emp_salary FROM employees WHERE employee_id = emp_id;
  
  SET grade = CASE
    WHEN emp_salary >= 5000000 THEN 'A'
    WHEN emp_salary >= 4500000 THEN 'B'
    WHEN emp_salary >= 4000000 THEN 'C'
    ELSE 'D'
  END;
END;

CALL AssignGrade(1, @grade1);
SELECT @grade1;
```

Submission: Screenshots of procedure execution results

---

## Intermediate Level (2 Questions)

**Question 18** Use WHILE loop to process data.

```sql
-- Create temporary table
CREATE TABLE temp_results (
    id INT,
    data VARCHAR(50)
);

-- WHILE loop to insert data
CREATE PROCEDURE InsertTestData (IN count INT)
BEGIN
  DECLARE i INT DEFAULT 1;
  WHILE i <= count DO
    INSERT INTO temp_results VALUES (i, CONCAT('Data_', i));
    SET i = i + 1;
  END WHILE;
END;

CALL InsertTestData(5);
SELECT * FROM temp_results;

-- REPEAT loop alternative
CREATE PROCEDURE InsertTestDataRepeat (IN count INT)
BEGIN
  DECLARE i INT DEFAULT 1;
  REPEAT
    INSERT INTO temp_results VALUES (i + 5, CONCAT('DataR_', i));
    SET i = i + 1;
  UNTIL i > 5
  END REPEAT;
END;

CALL InsertTestDataRepeat(5);
SELECT * FROM temp_results;
```

Submission: Screenshot showing final temp_results table

---

**Question 19** Execute procedure with nested control structures.

```sql
CREATE TABLE salary_analysis (
    emp_id INT,
    emp_name VARCHAR(30),
    salary INT,
    grade CHAR,
    status VARCHAR(30)
);

-- Complex procedure with nested IF-CASE
CREATE PROCEDURE AnalyzeSalary (IN emp_id INT)
BEGIN
  DECLARE emp_name VARCHAR(30);
  DECLARE emp_salary INT;
  DECLARE grade CHAR;
  DECLARE status VARCHAR(30);
  
  SELECT name, salary INTO emp_name, emp_salary FROM employees WHERE employee_id = emp_id;
  
  -- IF condition
  IF emp_salary >= 4000000 THEN
    -- CASE for grade assignment
    SET grade = CASE
      WHEN emp_salary >= 5000000 THEN 'A'
      WHEN emp_salary >= 4500000 THEN 'B'
      ELSE 'C'
    END;
    SET status = 'Normal';
  ELSE
    SET grade = 'D';
    SET status = 'Needs Raise';
  END IF;
  
  INSERT INTO salary_analysis VALUES (emp_id, emp_name, emp_salary, grade, status);
END;

CALL AnalyzeSalary(1);
CALL AnalyzeSalary(2);
CALL AnalyzeSalary(3);

SELECT * FROM salary_analysis;
```

Submission: Screenshot showing salary_analysis results

---

## Advanced Level (1 Question)

**Question 20** Write and execute complex control structure procedures.

```
Requirements:
1. Cursor-based procedure
   - Iterate through all employee salaries
   - Determine raise rate based on salary
   - Store results in separate table

2. LOOP with LEAVE procedure
   - Create labeled LOOP
   - LEAVE on specific condition
   - Limit iteration count

3. Nested loops
   - WHILE with nested IF-CASE
   - Calculate/classify salary by department
   - Process multiple conditions

4. Execute and verify results
   - Compare before/after data
   - Verify each step

Submission:
   - SQL code for each procedure
   - Execution result screenshots
   - Created table data
```

---

---

# 📋 Answer Key and Model Answers

## Multiple Choice Answers (10 Questions)

| Question | Answer | Explanation                          |
| :------: | :----: | :----------------------------------- |
|    1    |   ②   | Correct IF-THEN-ELSE syntax          |
|    2    |   ②   | Simple CASE: CASE variable WHEN      |
|    3    |   ②   | WHILE checks condition first         |
|    4    |   ③   | REPEAT executes once then checks     |
|    5    |   ③   | LEAVE with label exits LOOP          |
|    6    |   ③   | Only ① has correct ELSEIF syntax    |
|    7    |   ②   | Searched CASE: CASE WHEN condition   |
|    8    |   ④   | Both nesting approaches possible     |
|    9    |   ②   | Cursor processes each row repeatedly |
|    10    |   ①   | Label identifies, LEAVE exits        |

---

## Short Answer Model Answers (5 Questions)

### Question 11: IF-THEN-ELSE and ELSEIF

**Model Answer**:

```
IF-THEN-ELSE Structure:

IF condition THEN
  -- Condition true
  statement1;
ELSEIF condition2 THEN
  -- Condition2 true
  statement2;
ELSE
  -- All conditions false
  statement3;
END IF;

ELSEIF Usage:
- Needed for 3+ branches
- Conditions checked in order
- First true branch executes
- Remaining conditions skipped

Example:
IF score >= 90 THEN SET grade = 'A';
ELSEIF score >= 80 THEN SET grade = 'B';
ELSEIF score >= 70 THEN SET grade = 'C';
ELSE SET grade = 'D';
END IF;
```

---

### Question 12: Two CASE Forms

**Model Answer**:

```
1. Simple CASE
CASE variable
  WHEN value1 THEN statement1;
  WHEN value2 THEN statement2;
  ELSE statement_default;
END CASE;

Characteristics:
- Compare single variable with multiple values
- Only = comparison possible
- Clear value mapping

Use Case:
- Month number → Month name
- Code → Description
- Fixed value mapping

Example:
SET month_name = CASE month
  WHEN 1 THEN 'January'
  WHEN 2 THEN 'February'
  ELSE 'Unknown'
END;

2. Searched CASE
CASE
  WHEN condition1 THEN statement1;
  WHEN condition2 THEN statement2;
  ELSE statement_default;
END CASE;

Characteristics:
- Complex conditions possible
- Supports >, <, AND, OR
- Range checking possible

Use Case:
- Grade by range
- Multiple condition combinations
- Complex business logic

Example:
SET grade = CASE
  WHEN salary >= 5000000 THEN 'A'
  WHEN salary >= 4000000 AND dept = 1 THEN 'B'
  WHEN salary < 3000000 OR dept = 3 THEN 'D'
  ELSE 'C'
END;
```

---

### Question 13: Loop Comparison

**Model Answer**:

```
1. WHILE (Check first)
WHILE condition DO
  statement;
END WHILE;

Characteristics:
- Check condition → execute
- False condition: 0 executions possible
- Standard loop

2. REPEAT-UNTIL (Execute first)
REPEAT
  statement;
UNTIL condition
END REPEAT;

Characteristics:
- Execute minimum once
- Check condition after
- Like do-while

3. LOOP (Infinite, LEAVE exit)
[label:] LOOP
  statement;
  IF condition THEN
    LEAVE label;
  END IF;
END LOOP;

Characteristics:
- Infinite loop
- Exit with explicit LEAVE
- Label required

Comparison:
WHILE: Pre-check, 0+ iterations
REPEAT: Post-check, 1+ iterations
LOOP: Infinite, explicit exit
```

---

### Question 14: Nested Control Structures

**Model Answer**:

```
Concept:
- Control structure within another
- IF with CASE, CASE with IF
- Complex logic implementation

Example:
CREATE PROCEDURE complex_logic ()
BEGIN
  DECLARE i INT DEFAULT 1;
  
  -- WHILE loop
  WHILE i <= 5 DO
    -- IF condition
    IF i MOD 2 = 0 THEN
      -- CASE statement
      SET grade = CASE i
        WHEN 2 THEN '2: Even'
        WHEN 4 THEN '4: Even'
        ELSE 'Other'
      END;
    ELSE
      SET grade = CONCAT(i, ': Odd');
    END IF;
  
    INSERT INTO results VALUES (i, grade);
    SET i = i + 1;
  END WHILE;
END;
```

---

### Question 15: Cursor Concept and Usage

**Model Answer**:

```
Concept:
- Process query result set row by row
- Pointer-like operation on file
- Iterate through rows for processing

Usage:
1. Declare Cursor
   DECLARE cursor_name CURSOR FOR SELECT ...;

2. Open Cursor
   OPEN cursor_name;

3. Fetch rows (repeat)
   FETCH cursor_name INTO var1, var2, ...;

4. Close Cursor
   CLOSE cursor_name;

5. Handle completion
   DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = TRUE;

Example:
CREATE PROCEDURE process_employees ()
BEGIN
  DECLARE done INT DEFAULT FALSE;
  DECLARE emp_name VARCHAR(50);
  DECLARE emp_salary INT;
  DECLARE emp_cursor CURSOR FOR SELECT name, salary FROM employees;
  DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = TRUE;
  
  OPEN emp_cursor;
  read_loop: LOOP
    FETCH emp_cursor INTO emp_name, emp_salary;
    IF done THEN LEAVE read_loop; END IF;
  
    -- Process emp_name, emp_salary
    INSERT INTO processed VALUES (...);
  END LOOP;
  CLOSE emp_cursor;
END;

Required Situations:
- Different processing per row
- Additional queries based on row data
- Batch processing
- Row-by-row calculations
```

---

## Practical Problems Model Answers (5 Questions)

### Question 16: Create employees Table

**Completion Criteria**:
✅ ch11_control_structure database created
✅ employees table created
✅ 3 employees inserted

---

### Question 17: Conditional Procedures

**Completion Criteria**:
✅ IF-THEN-ELSE procedure executed
✅ CASE procedure executed
✅ Results verified per employee

---

### Question 18: Loop Procedures

**Completion Criteria**:
✅ WHILE loop: 5 rows inserted
✅ REPEAT loop: 5 additional rows
✅ temp_results: 10 total rows verified

---

### Question 19: Nested Control Structures

**Completion Criteria**:
✅ salary_analysis table created
✅ IF-CASE nested procedure executed
✅ 3 employees analyzed and stored

---

### Question 20: Complex Control Structures

**Model Answer**:

```sql
-- 1. Cursor-based procedure
CREATE TABLE salary_history (
  emp_id INT,
  old_salary INT,
  raise_rate INT,
  new_salary INT
);

CREATE PROCEDURE ApplySalaryRaise ()
BEGIN
  DECLARE done INT DEFAULT FALSE;
  DECLARE emp_id INT;
  DECLARE emp_salary INT;
  DECLARE raise_rate INT;
  DECLARE new_salary INT;
  
  DECLARE emp_cursor CURSOR FOR 
    SELECT employee_id, salary FROM employees;
  DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = TRUE;
  
  OPEN emp_cursor;
  emp_loop: LOOP
    FETCH emp_cursor INTO emp_id, emp_salary;
    IF done THEN LEAVE emp_loop; END IF;
  
    IF emp_salary >= 5000000 THEN
      SET raise_rate = 5;
    ELSEIF emp_salary >= 4000000 THEN
      SET raise_rate = 10;
    ELSE
      SET raise_rate = 15;
    END IF;
  
    SET new_salary = ROUND(emp_salary * (1 + raise_rate/100));
    INSERT INTO salary_history VALUES (emp_id, emp_salary, raise_rate, new_salary);
  END LOOP;
  CLOSE emp_cursor;
END;

CALL ApplySalaryRaise();
SELECT * FROM salary_history;

-- 2. LOOP with LEAVE
CREATE TABLE loop_test (
  iteration INT,
  value VARCHAR(50)
);

CREATE PROCEDURE TestLoopLeave ()
BEGIN
  DECLARE i INT DEFAULT 1;
  
  loop_label: LOOP
    IF i > 10 THEN
      LEAVE loop_label;
    END IF;
  
    INSERT INTO loop_test VALUES (i, CONCAT('Iteration_', i));
    SET i = i + 1;
  END LOOP;
END;

CALL TestLoopLeave();
SELECT * FROM loop_test;

-- 3. Nested loops with conditions
CREATE TABLE dept_salary_calc (
  dept_id INT,
  iteration INT,
  processed VARCHAR(50)
);

CREATE PROCEDURE ProcessDeptSalaries ()
BEGIN
  DECLARE dept INT DEFAULT 1;
  DECLARE i INT DEFAULT 1;
  DECLARE status VARCHAR(50);
  
  WHILE dept <= 2 DO
    SET i = 1;
    WHILE i <= 3 DO
      SET status = CASE
        WHEN i MOD 2 = 0 THEN 'Even'
        ELSE 'Odd'
      END;
    
      INSERT INTO dept_salary_calc VALUES (dept, i, status);
      SET i = i + 1;
    END WHILE;
    SET dept = dept + 1;
  END WHILE;
END;

CALL ProcessDeptSalaries();
SELECT * FROM dept_salary_calc;
```

---

Prof. Jeong-Hyun Cho (peterchokr@gmail.com)
Yeungnam University College
