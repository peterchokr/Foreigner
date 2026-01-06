# Ch9 DML and Transaction - Exercise Problems

Dear students! After completing Chapter 9, please solve the following exercises to verify your understanding.
Check the difficulty level of each section and study step by step.

---

## 📌 Learning Objectives Verification

After completing Chapter 9, you should understand:

- DML (Data Manipulation Language) concept
- INSERT, UPDATE, DELETE commands
- Transaction concept
- COMMIT, ROLLBACK, SAVEPOINT
- Data consistency and integrity
- Transaction isolation levels

---

# Multiple Choice Questions (10 Questions)

## Beginner Level (5 Questions) - Basic Concept Verification

**Question 1** Correct definition of DML?

- ① Language defining database structure (CREATE, ALTER)
- ② Language inserting, updating, deleting data (INSERT, UPDATE, DELETE)
- ③ Language querying data (SELECT)
- ④ Language managing user permissions

---

**Question 2** Without specifying columns in INSERT statement?

- ① Error occurs
- ② Must enter all column values in order
- ③ NULL is inserted
- ④ Can be entered optionally

---

**Question 3** Without WHERE clause in UPDATE?

- ① Error occurs
- ② All rows in column are modified
- ③ Nothing modified
- ④ Only last row modified

---

**Question 4** Basic characteristic of Transaction?

- ① Only database queries allowed
- ② Multiple SQLs treated as single work unit
- ③ Always must succeed
- ④ Cannot be undone

---

**Question 5** Role of COMMIT?

- ① Cancel work
- ② Save work (end transaction)
- ③ Revert to specific point
- ④ Start transaction

---

## Intermediate Level (3 Questions) - Concept Application

**Question 6** Correct syntax among INSERT, UPDATE, DELETE?

```sql
① INSERT INTO employees (name, salary) VALUES ('Kim Chulsu', 5000000);

② UPDATE employees SET salary = 6000000 WHERE name = 'Lee Younghee';

③ DELETE FROM employees WHERE employee_id > 10;
```

- ① Correct
- ② Correct
- ③ Correct
- ④ All ①②③ correct

---

**Question 7** When is ROLLBACK necessary?

- ① Save all changes
- ② Cancel all changes on transaction error
- ③ Revert only specific command
- ④ Initialize database

---

**Question 8** Purpose of SAVEPOINT?

- ① Set transaction start point
- ② Set savepoint mid-transaction to revert if needed
- ③ Create database backup
- ④ Control multiple user access

---

## Advanced Level (2 Questions) - Critical Thinking

**Question 9** Why transaction essential in this situation?

"Bank transfer: Withdraw 1,000,000 from Account A → Deposit 1,000,000 to Account B"

- ① Prevent money loss if withdrawal succeeds but deposit fails
- ② Both actions succeed or both fail (atomicity)
- ③ Save transfer record
- ④ Both ①②

---

**Question 10** Constraint required for data consistency?

- ① Transaction alone sufficient
- ② Constraints (PRIMARY KEY, FOREIGN KEY, CHECK) + Transaction needed
- ③ Constraints alone sufficient
- ④ Log file usage

---

# Short Answer Questions (5 Questions)

## Beginner Level (3 Questions)

**Question 11** Explain difference between INSERT, UPDATE, DELETE commands.

---

**Question 12** Define transaction and explain necessity, providing 3+ practical cases.

---

**Question 13** Explain difference between COMMIT and ROLLBACK with usage examples.

---

## Intermediate Level (1 Question)

**Question 14** Explain SAVEPOINT concept and how to use it in complex transactions.

---

## Advanced Level (1 Question)

**Question 15** Explain ACID properties and role of each in transaction management.

---

# Practical Problems (5 Questions)

## Beginner Level (2 Questions)

**Question 16** Execute following SQL and provide result screenshot.

```sql
CREATE DATABASE ch9_dml CHARACTER SET utf8mb4;
USE ch9_dml;

CREATE TABLE employees (
    employee_id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(30),
    position VARCHAR(20),
    salary INT,
    hire_date DATE
);

-- Insert initial data
INSERT INTO employees VALUES
(1, 'Kim Chulsu', 'Manager', 5000000, '2020-01-15'),
(2, 'Lee Younghee', 'Assistant Manager', 4000000, '2021-03-20'),
(3, 'Park Minjun', 'Staff', 3500000, '2022-06-10');

SELECT * FROM employees;
```

Submission: Screenshot with all initial 3 employee records

---

**Question 17** Perform following and verify results.

```sql
-- 1. INSERT: Add new employee
INSERT INTO employees VALUES
(4, 'Choi Sunsin', 'Staff', 3200000, '2023-09-01');

SELECT * FROM employees;

-- 2. UPDATE: Raise salary
UPDATE employees SET salary = 5500000 WHERE name = 'Kim Chulsu';

SELECT * FROM employees WHERE name = 'Kim Chulsu';

-- 3. DELETE: Remove employee
DELETE FROM employees WHERE employee_id = 3;

SELECT * FROM employees;
```

Submission: Screenshot showing all steps

---

## Intermediate Level (2 Questions)

**Question 18** Use transaction to perform following.

```sql
-- Start transaction
START TRANSACTION;

-- 1. Add multiple employees
INSERT INTO employees VALUES
(5, 'Kang Gamchan', 'Assistant Manager', 4300000, '2023-01-15');

INSERT INTO employees VALUES
(6, 'Lee Sunsin', 'Staff', 3400000, '2023-02-20');

-- 2. Bulk salary increase (5%)
UPDATE employees SET salary = ROUND(salary * 1.05) WHERE position = 'Staff';

-- 3. Verify data
SELECT * FROM employees;

-- Commit transaction
COMMIT;

-- Verify final result
SELECT * FROM employees;
```

Submission: Screenshot of transaction execution

---

**Question 19** Perform transaction with ROLLBACK.

```sql
-- Transaction 1: Normal COMMIT
START TRANSACTION;
UPDATE employees SET salary = 6000000 WHERE employee_id = 1;
COMMIT;

-- Transaction 2: ROLLBACK on error
START TRANSACTION;
UPDATE employees SET salary = 7000000 WHERE employee_id = 1;
INSERT INTO employees VALUES (7, 'Chang Bogo', 'Manager', 5500000, '2023-03-10');
-- (Error detected) ROLLBACK
ROLLBACK;

-- Result check: employee_id 1 is 6000000, no employee 7
SELECT * FROM employees;
```

Submission: Screenshot of ROLLBACK transaction

---

## Advanced Level (1 Question)

**Question 20** Write and execute complex transaction.

```
Requirements:
1. Organization restructuring transaction
   - Change employee positions
   - Adjust salaries
   - Cancel everything on failure

2. Partial rollback with SAVEPOINT
   - Set multiple SAVEPOINTs
   - Revert only specific work
   - Keep others

3. Test success/failure scenarios
   - Normal: COMMIT
   - Error: ROLLBACK
   - Partial: SAVEPOINT + ROLLBACK

4. Data integrity verification
   - Confirm consistency before/after

Submission:
   - SQL code for each transaction
   - Result screenshots for each step
   - Before/after data comparison
```

---

---

# 📋 Answer Key and Model Answers

## Multiple Choice Answers (10 Questions)

| Question | Answer | Explanation                                               |
| :------: | :----: | :-------------------------------------------------------- |
|    1    |   ②   | DML is INSERT, UPDATE, DELETE (modifying data)            |
|    2    |   ②   | Without columns specified, must enter all values in order |
|    3    |   ②   | Without WHERE, all rows are modified (dangerous!)         |
|    4    |   ②   | Transaction treats multiple SQLs as single unit           |
|    5    |   ②   | COMMIT saves changes and ends transaction                 |
|    6    |   ④   | All ①②③ have correct syntax                            |
|    7    |   ②   | ROLLBACK cancels all changes when error occurs            |
|    8    |   ②   | SAVEPOINT marks point to partially revert                 |
|    9    |   ④   | Both ①② reasons make transaction necessary              |
|    10    |   ②   | Constraints + Transaction both required                   |

---

## Short Answer Model Answers (5 Questions)

### Question 11: INSERT, UPDATE, DELETE Differences

**Model Answer**:

```
INSERT (Add):
- Add new rows
- Syntax: INSERT INTO table (cols) VALUES (vals);
- Example: Add new employee

UPDATE (Modify):
- Change existing row data
- Syntax: UPDATE table SET col = val WHERE condition;
- Example: Raise employee salary

DELETE (Remove):
- Delete rows
- Syntax: DELETE FROM table WHERE condition;
- Example: Remove retired employee

Important:
- UPDATE, DELETE require WHERE (avoid deleting all!)
- Recommend backup before execution
```

---

### Question 12: Transaction Definition and Necessity

**Model Answer**:

```
Definition:
- Logical work unit
- Multiple SQLs treated as one
- All succeed or all fail (atomicity)

Necessity:
- Ensure data consistency
- Prevent partial success (data corruption)

Real-world cases:

1. Bank transfer
   Withdraw(A) → Deposit(B)
   Both succeed or both cancel

2. Order processing
   Decrease product qty → Create order
   Both must apply or both cancel

3. Personnel record
   Resign → Delete salary
   Must process together

4. Inventory management
   Sell → Decrease inventory → Record sales
   All three or none

5. Account transfer
   Decrease account A → Increase account B
   Both must succeed
```

---

### Question 13: COMMIT vs ROLLBACK

**Model Answer**:

```
COMMIT:
- Role: Save transaction changes
- Timing: After work completion
- Effect: Changes persist in database

ROLLBACK:
- Role: Cancel transaction changes
- Timing: On error or intentional cancel
- Effect: Restore state before transaction

Usage situations:

COMMIT use:
START TRANSACTION;
INSERT INTO employees ...;
UPDATE employees ...;
-- All work verified
COMMIT;

ROLLBACK use:
START TRANSACTION;
DELETE FROM employees WHERE ...;
-- Realized mistake
ROLLBACK;  -- Undo delete

Data perspective:
COMMIT before: Others don't see changes
COMMIT after: All users see changes
ROLLBACK after: Changes never happened
```

---

### Question 14: SAVEPOINT Concept and Usage

**Model Answer**:

```
Concept:
- Mark point mid-transaction
- Partial rollback to specific point
- Rest of transaction continues

Syntax:
SAVEPOINT savepoint_name;
ROLLBACK TO savepoint_name;

Usage example:
START TRANSACTION;
  INSERT INTO employees VALUES (...);
  SAVEPOINT sp1;  -- Save point 1
  
  UPDATE employees SET ...;
  SAVEPOINT sp2;  -- Save point 2
  
  DELETE FROM employees WHERE ...;
  -- DELETE causes error
  ROLLBACK TO sp2;  -- Rollback to sp2 only (UPDATE kept)
  
COMMIT;

Result:
- INSERT preserved
- UPDATE preserved
- DELETE cancelled

Real-world use:
Complex multi-step job:
- Mark checkpoint after each step
- Error in one step → rollback to previous
- Other steps remain
```

---

### Question 15: ACID Properties

**Model Answer**:

```
ACID Characteristics:

1. Atomicity (All or Nothing)
   - Transaction all succeeds or all fails
   - No partial success
   - Role: Ensures data integrity

2. Consistency (Data Validity)
   - Database state valid before/after
   - Rules and constraints maintained
   - Example: Foreign key integrity

3. Isolation (Independent Execution)
   - Concurrent transactions don't interfere
   - Each transaction independent
   - Levels: READ UNCOMMITTED, READ COMMITTED

4. Durability (Permanent Storage)
   - COMMIT changes persist permanently
   - Survives system failures
   - Guaranteed by logs

Transaction management role:

In withdrawal example:
A. Atomicity: Withdraw all or not at all
C. Consistency: Balance never negative
I. Isolation: Other withdrawals don't interfere
D. Durability: Withdrawal permanent after COMMIT
```

---

## Practical Problems Model Answers (5 Questions)

### Question 16: Create employees Table

**Completion Criteria**:
✅ ch9_dml database created
✅ employees table created (5 columns)
✅ 3 initial employee records

---

### Question 17: INSERT, UPDATE, DELETE Operations

**Expected Results**:

```
After INSERT: 4 employees (new employee added)
After UPDATE: Kim Chulsu salary 5,500,000
After DELETE: 3 employees (Park Minjun removed)
```

---

### Question 18: COMMIT Transaction

**Completion Criteria**:
✅ 2 new employees added
✅ Staff salary increased 5%
✅ COMMIT successful
✅ Final data confirmed

---

### Question 19: ROLLBACK Transaction

**Model Answer**:

```sql
-- Transaction 1: COMMIT
START TRANSACTION;
UPDATE employees SET salary = 6000000 WHERE employee_id = 1;
COMMIT;

-- Transaction 2: ROLLBACK
START TRANSACTION;
UPDATE employees SET salary = 7000000 WHERE employee_id = 1;
INSERT INTO employees VALUES (7, 'Chang Bogo', 'Manager', 5500000, '2023-03-10');
-- ROLLBACK reverses both
ROLLBACK;

Result:
- Employee 1: 6000000 (Transaction 2 UPDATE cancelled)
- Employee 7: Doesn't exist (INSERT cancelled)
```

---

### Question 20: Complex Transaction

**Model Answer**:

```sql
-- 1. Position change transaction
START TRANSACTION;
  UPDATE employees SET position = 'Assistant Manager' WHERE employee_id = 2;
  SAVEPOINT sp1;  -- Save after first change
  
  UPDATE employees SET salary = ROUND(salary * 1.1) WHERE position = 'Manager';
  SAVEPOINT sp2;  -- Save after salary increase
  
  INSERT INTO employees VALUES (8, 'New Employee', 'Staff', 3000000, '2024-01-01');
  -- Hypothetical error
  ROLLBACK TO sp2;  -- Rollback INSERT only
  
COMMIT;

-- 2. Partial rollback example
START TRANSACTION;
  DELETE FROM employees WHERE employee_id = 1;
  SAVEPOINT sp1;
  
  DELETE FROM employees WHERE employee_id = 2;
  -- Error detected
  ROLLBACK TO sp1;  -- Undo 2nd DELETE
  
COMMIT;

Result:
- First DELETE preserved
- Second DELETE cancelled
- Other data unchanged
```

---

Prof. Jeong-Hyun Cho (peterchokr@gmail.com)
Yeungnam University College
