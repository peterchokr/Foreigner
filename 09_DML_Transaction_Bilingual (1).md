### DELETE vs TRUNCATE Comparison

# Chapter 9: DML (Data Manipulation Language) and Transactions

---

## 📖 Course Overview

In this chapter, you will learn about DML (Data Manipulation Language) commands—INSERT, UPDATE, and DELETE—which are used to manipulate data in a database. You will also explore transactions, which are critical for ensuring data integrity and consistency. This chapter covers how to safely insert, modify, and delete data, and how to group multiple SQL operations into a single logical unit to maintain data consistency. The goal is to develop the ability to prevent data integrity errors and implement recovery mechanisms in real-world scenarios.

| 이 장에서는 데이터베이스의 데이터를 조작하는 DML 명령어(INSERT, UPDATE, DELETE)와 데이터 무결성을 보장하는 트랜잭션(Transaction)을 학습합니다. 데이터를 정확하고 안전하게 입력, 수정, 삭제하는 방법과 여러 작업을 하나의 논리적 단위로 처리하여 데이터 일관성을 유지하는 방법을 다룹니다. 실무에서 데이터 무결성 오류를 방지하고 복구할 수 있는 능력을 개발하는 것이 목표입니다.

---

## 📚 Part 1: Theoretical Learning

### What You'll Learn in This Section

- INSERT statement variations: basic, bulk, and subquery-based
- UPDATE statement syntax and safe practices
- DELETE statement safety and foreign key constraints
- Transaction concepts and the ACID model
- COMMIT and ROLLBACK mechanisms
- SAVEPOINT for partial rollback
- Isolation levels and concurrency control
- Common DML and transaction pitfalls

| - INSERT 문의 다양한 형태
| - UPDATE 문과 조건 처리
| - DELETE 문과 데이터 보호
| - 트랜잭션의 개념과 ACID 특성
| - COMMIT과 ROLLBACK의 역할
| - 동시성 제어 및 잠금
| - 격리 수준과 트랜잭션 문제

---

## 9.1 INSERT Statement (Data Insertion)

An **INSERT** statement adds new data to a table.

| **INSERT 문**은 테이블에 새로운 데이터를 추가합니다.

### Basic Syntax

```sql
INSERT INTO table_name (column1, column2, ...)
VALUES (value1, value2, ...);
```

### Example

```sql
-- Insert new employee information into specified columns
INSERT INTO employees (name, dept_id, salary)
VALUES ('Kim Chul-soo', 1, 5000000);
```

### Why Specify Column Names?

Using explicit column names is a best practice:

- **Clarity**: Each value's destination column is immediately clear
- **Robustness**: Adding new columns to the table won't break existing queries
- **Maintainability**: Code readers understand the intention
- **Default values**: You can skip columns that have DEFAULT values

| **열 이름을 지정해야 할까?**
|
| 명시적으로 열 이름을 지정하는 것이 좋은 이유:
|
| - **명확성**: 각 값이 어느 열로 가는지 한눈에 알 수 있음
| - **견고성**: 테이블에 새로운 열이 추가되어도 기존 쿼리는 깨지지 않음
| - **유지보수성**: 누군가 코드를 읽을 때 의도가 명확함
| - **기본값**: DEFAULT 값이 있는 열을 건너뛸 수 있음

### ❌ The Dangerous Way (Avoid This!)

```sql
-- Insert values without specifying column names (NOT RECOMMENDED!)
INSERT INTO employees
VALUES (NULL, 'Lee Young-hee', 1, 4000000);  -- ❌ RISKY!
```

| **왜 위험한가?**
|
| - 테이블 구조가 변경되면 이 쿼리가 깨짐
| - 열의 순서를 정확히 알아야 함
| - 코드를 읽는 사람이 무엇을 삽입하는지 불명확함

### Bulk INSERT (Performance Optimization)

```sql
-- Insert multiple rows in one statement (much faster!)
INSERT INTO employees (name, dept_id, salary) VALUES
('Park Min-jun', 2, 4500000),
('Choi Soon-shin', 2, 3500000),
('Hwang Su-jung', 1, 4100000);
```

| **대량 INSERT의 장점:**
|
| - ⚡ 개별 INSERT 문보다 훨씬 빠름
| - 🌐 네트워크 트래픽 감소
| - 🔒 단일 트랜잭션으로 처리되어 안전함

### INSERT with Subquery (Data Copy)

```sql
-- Copy data from one table to another
INSERT INTO employee_backup
SELECT * FROM employees 
WHERE dept_id = 1;
```

| **사용 사례:**
|
| - 💾 주요 변경 전 데이터 백업
| - 📦 오래된 데이터를 아카이브 테이블로 복사
| - 🔄 데이터 마이그레이션
| - 🧪 실제 데이터로부터 테스트 데이터 생성

---

## 9.2 UPDATE Statement (Data Modification)

An **UPDATE** statement modifies existing data in a table.

| **UPDATE 문**은 기존 데이터를 수정합니다.

### Basic Syntax

```sql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
```

### ⚠️ CRITICAL WARNING

**Always include a WHERE clause! Without it, ALL rows will be updated!**

| **⚠️ 치명적인 경고!**
|
| **WHERE 조건을 반드시 포함하세요. 없으면 모든 행이 수정됩니다!**

```sql
-- 🚨 EXTREMELY DANGEROUS - NEVER DO THIS!
UPDATE employees SET salary = 5000000;  -- NO WHERE CLAUSE!
-- Result: EVERY employee's salary becomes 5,000,000! 💥

-- ✅ CORRECT APPROACH
UPDATE employees SET salary = 5000000 
WHERE employee_id = 1;  -- Only specific employee
```

### Safe UPDATE Procedure

```sql
-- Step 1: Verify what will be updated using SELECT first
-- 단계 1: 먼저 SELECT로 무엇이 수정될지 확인
SELECT * FROM employees WHERE dept_id = 1;

-- Step 2: If results look correct, execute UPDATE
-- 단계 2: 결과가 맞으면 UPDATE 실행
UPDATE employees 
SET salary = salary * 1.1  -- 10% raise
WHERE dept_id = 1;

-- Step 3: Even safer - use transactions
-- 단계 3: 트랜잭션을 사용하면 더 안전함
START TRANSACTION;
UPDATE employees SET salary = salary * 1.1 WHERE dept_id = 1;
COMMIT;  -- Or ROLLBACK if something is wrong
```

### Formula-Based UPDATE

```sql
-- Update based on current values
UPDATE employees
SET salary = salary * 1.1  -- Multiply current salary by 1.1 (10% raise)
WHERE dept_id = 1;
```

### Conditional UPDATE with CASE

```sql
-- Apply different raises based on department
-- 부서별로 다른 인상률 적용
UPDATE employees
SET salary = CASE
    WHEN dept_id = 1 THEN salary * 1.15  -- Sales: 15% raise
    WHEN dept_id = 2 THEN salary * 1.12  -- Tech: 12% raise
    WHEN dept_id = 3 THEN salary * 1.10  -- HR: 10% raise
    ELSE salary  -- Others: no raise
END;
```

### UPDATE with JOIN

```sql
-- Update only Seoul-based department employees
-- 서울 지역 부서 직원만 급여 인상
UPDATE employees e
JOIN departments d ON e.dept_id = d.dept_id
SET e.salary = e.salary * 1.1
WHERE d.location = 'Seoul';
```

---

## 9.3 DELETE Statement (Data Deletion)

A **DELETE** statement removes data from a table.

| **DELETE 문**은 테이블의 데이터를 삭제합니다.

### Basic Syntax

```sql
DELETE FROM table_name
WHERE condition;
```

### ⚠️ VERY IMPORTANT WARNING

**Without a WHERE clause, ALL rows will be permanently deleted!**

| **⚠️ 매우 중요한 경고!**
|
| **WHERE 조건이 없으면 모든 행이 영구적으로 삭제됩니다!**

```sql
-- 🚨 EXTREMELY DANGEROUS - NEVER DO THIS!
DELETE FROM employees;  -- NO WHERE CLAUSE!
-- Result: ALL employee data is permanently deleted! 💥

-- ✅ CORRECT APPROACH
DELETE FROM employees WHERE employee_id = 7;  -- Only specific employee
```

### Safe DELETE Procedure

```sql
-- Step 1: First verify what will be deleted
-- 단계 1: 삭제될 데이터를 먼저 확인
SELECT * FROM employees WHERE salary < 3500000;

-- Step 2: If results look correct, execute DELETE
-- 단계 2: 결과가 맞으면 DELETE 실행
DELETE FROM employees WHERE salary < 3500000;

-- Step 3: Safest approach - use transactions
-- 단계 3: 가장 안전한 방법 - 트랜잭션 사용
START TRANSACTION;
DELETE FROM employees WHERE salary < 3500000;
-- After verifying results:
-- 결과 확인 후
COMMIT;  -- Confirm deletion or
ROLLBACK;  -- Cancel and restore original state
```

| Feature           | DELETE                  | TRUNCATE         |
| :---------------- | :---------------------- | :--------------- |
| WHERE Condition   | ✅ Supported            | ❌ Not supported |
| Rollback Possible | ✅ Yes (in transaction) | ❌ No            |
| Speed             | Slow                    | Very fast        |
| When to Use       | Selective deletion      | Delete all rows  |
| Table Structure   | Maintained              | Maintained       |

|  |  |  |
| :-: | :-: | :-: |

### Foreign Key Constraints Warning

```sql
-- This DELETE might fail!
-- 이 DELETE가 실패할 수 있음!
DELETE FROM departments WHERE dept_id = 1;

-- Error: employees table references this department!
-- 오류: 직원 테이블이 이 부서를 참조하고 있음!

-- Solution: First delete child records
-- 해결책: 먼저 자식 레코드 삭제
DELETE FROM employees WHERE dept_id = 1;  -- First delete employees
DELETE FROM departments WHERE dept_id = 1;  -- Then delete department
```

---

## 9.4 Transaction Concept

A **transaction** is a group of SQL statements that are logically related and executed as a single unit.

| **트랜잭션**은 여러 SQL 작업을 하나의 논리적 단위로 묶습니다.

### Why Transactions Are Needed: Bank Transfer Example

Consider a real banking transfer scenario:

| **왜 트랜잭션이 필요한가? (은행 송금 사례)**
| 실제 은행 시스템에서 계좌 송금을 생각해봅시다.

#### ❌ Problem Without Transactions

```sql
-- Kim Chul-soo's account: withdraw 100,000
-- 김철수 계좌에서 100,000원 출금
UPDATE accounts SET balance = balance - 100000 
WHERE account_id = 1001;  
-- ✅ Success!

-- System suddenly crashes! 😱
-- Server down, database connection lost, power outage, etc...
-- 여기서 갑자기 시스템이 다운됨! 😱
-- 서버 먹통, 데이터베이스 연결 끊김, 전기 끊김 등...

-- Lee Young-hee's account: deposit 100,000 (NOT executed)
-- 이영희 계좌에 100,000원 입금 (실행 안 됨)
UPDATE accounts SET balance = balance + 100000 
WHERE account_id = 1002;  
-- ❌ Not executed
-- ❌ 실행되지 않음
```

**Result:**

- 🔴 Kim's account: 100,000 withdrawn ✅ (completed)
- 🔴 Lee's account: Unchanged (deposit not made) ❌
- 💥 Result: 100,000 is lost! (MAJOR problem!)

**Bank goes bankrupt** 😞

| **결과가 어떻게 되는가?**
| - 🔴 김철수 계좌: 100,000원 차감됨 ✅ (완료됨)
| - 🔴 이영희 계좌: 그대로 (입금 안 됨) ❌
| - 💥 결과: 100,000원이 사라짐! (매우 큰 문제!)
| **은행은 망함** 😞

#### ✅ Solution (Using Transactions)

```sql
-- Start transaction
-- 트랜잭션 시작
START TRANSACTION;

  -- Withdraw from Kim's account
  -- 김철수 계좌에서 출금
  UPDATE accounts SET balance = balance - 100000 
  WHERE account_id = 1001;
  
  -- Deposit to Lee's account
  -- 이영희 계좌에 입금
  UPDATE accounts SET balance = balance + 100000 
  WHERE account_id = 1002;

-- If both succeed, confirm
-- 둘 다 성공했으면 확정
COMMIT;

-- Or if problems occur, cancel both
-- 또는 문제가 있으면 모두 취소
ROLLBACK;
```

**Result:**

- ✅ Both updates succeed: COMMIT → Both changes are saved
- ❌ Either fails: ROLLBACK → Both changes are cancelled
- 🚫 Intermediate state NEVER occurs!

**Bank is safe** 😊

| **결과:**
| - ✅ 두 업데이트가 모두 성공: COMMIT → 두 변경사항이 모두 저장됨
| - ❌ 어느 하나라도 실패: ROLLBACK → 두 변경사항이 모두 취소됨
| - 🚫 중간 상태는 절대 발생하지 않음!
| **은행이 안전함** 😊

### Transaction Characteristics

- **All or Nothing**: All succeed or all fail
- **Intermediate state prevention**: Incomplete states are never saved
- **Data consistency guarantee**: Data is always in a valid state
- **Safety**: Problems can be recovered from by reverting to previous state

| **트랜잭션의 특징**
| - **All or Nothing**: 모두 성공하거나 모두 실패함
| - **중간 상태 방지**: 불완전한 상태가 절대 저장되지 않음
| - **데이터 일관성 보장**: 언제나 데이터는 유효한 상태를 유지
| - **안전성**: 문제가 발생하면 이전 상태로 돌려놓을 수 있음

---

## 9.5 ACID Properties

Four essential characteristics that guarantee transaction safety.

| 트랜잭션의 안전성을 보장하는 네 가지 핵심 특성입니다.

### A - Atomicity (원자성)

**Definition:** "All or Nothing" - Either all operations in a transaction are executed or all are canceled.

| **정의:** "All or Nothing" - 트랜잭션의 모든 작업이 완전히 수행되거나 완전히 취소됨

#### Problem Situation (문제 상황)

```sql
START TRANSACTION;
  UPDATE accounts SET balance = balance - 100000 WHERE id = 1001;  -- ✅ Success
  UPDATE accounts SET balance = balance + 100000 WHERE id = 1002;  -- ❌ Fails!

COMMIT;  
-- First is saved, second is not → Data mismatch!
-- 첫 번째만 저장되고 두 번째는 안 됨 → 데이터 불일치!
```

#### Solution (해결책)

Database handles this automatically:

- ❌ If any operation fails → ROLLBACK (cancel all)
- ✅ If all succeed → COMMIT (save all)

| 데이터베이스가 자동으로 처리:

| - ❌ 어느 하나라도 실패 → ROLLBACK (모두 취소)
| - ✅ 모두 성공 → COMMIT (모두 저장)

### Why It Matters (왜 중요한가?)

Without atomicity:

- Money withdrawn from one account but not deposited to another
- Inventory decreases but sales record is not created
- Confusing, incomplete data states emerge

| 원자성이 없으면:

| - 돈이 한 계좌에서는 빠지지만 다른 계좌에 들어오지 않음
| - 재고가 1개 감소하지만 판매 기록이 남지 않음
| - 혼란스러운 불완전한 데이터 상태 발생

---

### C - Consistency (일관성)

**Definition:** The database remains in a valid state before and after the transaction.

| **정의:** 트랜잭션 전후로 데이터베이스가 유효한 상태를 유지함

#### Example

```
Bank Rule: Sum of all account balances = Bank reserves
```

|`` 은행 규칙: 모든 계좌 잔액의 합 = 은행 준비금``

```sql
START TRANSACTION;
  UPDATE accounts SET balance = balance - 100000 WHERE id = 1001;  -- -100,000
  UPDATE accounts SET balance = balance + 100000 WHERE id = 1002;  -- +100,000
COMMIT;

-- Result: Total balance unchanged! ✅ Consistency maintained
-- 결과: 총 잔액은 변하지 않음! ✅ 일관성 유지됨
```

#### Constraint Verification (제약조건 확인)

Database verifies all constraints to maintain consistency:

| 데이터베이스는 일관성을 위해 모든 제약조건을 확인:

```sql
-- This query fails (consistency violation)
-- 이 쿼리는 실패함 (일관성 위반)
UPDATE employees SET dept_id = 999 WHERE employee_id = 1;

-- Error: dept_id 999 does not exist in departments table!
-- (Foreign key constraint violation)
-- Auto ROLLBACK ❌
-- 오류: dept_id 999는 departments 테이블에 없음! 
-- (외래키 제약조건 위반) 
-- 자동으로 ROLLBACK됨 ❌ 
```

### Why It Matters (왜 중요한가?)

- A department cannot be assigned if it doesn't exist
- Negative amounts or impossible values cannot be saved
- Data always remains in a valid state

| - 부서가 없는데 직원이 그 부서로 배정될 수 없음
| - 음수 금액이나 불가능한 값이 저장될 수 없음
| - 데이터가 항상 타당한 상태를 유지

---

### I - Isolation (격리성)

**Definition:** Concurrent transactions execute independently; one transaction's operations don't interfere with another's.

| **정의:** 동시에 실행되는 여러 트랜잭션이 서로 영향을 주지 않음

#### Problem Situation - without Isolation (문제 상황 - 격리성 없이)

```
Session A (Employee A)          Session B (Employee B)
─────────────────────────────────────────────
START TRANSACTION;
  SELECT balance;  -- 1,000,000
  balance = balance - 100,000;
                                START TRANSACTION;
                                  SELECT balance;  -- 900,000 (saw A's change!)
                                  -- But this is uncommitted data!
                                  -- 이건 아직 커밋되지 않은 데이터인데
                                  -- Dangerous! 🔴
  UPDATE accounts 
  SET balance = 900000;
  COMMIT;
                                  -- But B already saw 900,000!
                                  -- 하지만 B는 이미 900,000을 봤다
                                  UPDATE accounts 
                                  SET balance = 800000;
                                  COMMIT;

Result: Data mismatch! Both calculated with different information
결과: 데이터 불일치! 두 명이 다른 정보로 계산함
```

#### Solution - with Isolation (해결책 - 격리성으로 보호)

```
Session A                       Session B
─────────────────────────────────────────────
START TRANSACTION;
  SELECT balance;  -- 1,000,000 (lock begins)
  [During A's transaction, other sessions cannot see this row]
  [A의 트랜잭션 동안 이 행을 다른 세션이 볼 수 없음]
                                START TRANSACTION;
                                  SELECT balance;  -- Waiting... 🔄
                                  -- Waiting for A's transaction to end
                                  -- A의 트랜잭션이 끝날 때까지 기다림
  UPDATE accounts 
  SET balance = 900,000;
  COMMIT;  -- Lock released
                                  -- Now can finally read data
                                  -- 이제 비로소 데이터를 읽을 수 있음
                                  SELECT balance;  -- 900,000
                                  UPDATE accounts 
                                  SET balance = 800,000;
                                  COMMIT;

Result: Safe! ✅
결과: 안전함! ✅
```

### Why It Matters (왜 중요한가?)

- Prevent same data from being modified differently by two people
- Cannot see uncommitted changes from other people
- Safe even with concurrent work

| - 같은 데이터를 두 명이 다르게 수정하는 문제 방지
| - 다른 사람의 미커밋 변경을 볼 수 없음
| - 동시에 작업해도 안전함

---

### D - Durability (지속성)

**Definition:** Once a transaction is committed, data changes are permanent, even if system crashes.

| **정의:** COMMIT 후 데이터는 영구적으로 저장되고 손실되지 않음

#### Example

```sql
START TRANSACTION;
  INSERT INTO employees VALUES (10, 'New Employee', 1, 3500000);
COMMIT;  -- Data is now permanently saved ✅ 데이터가 이제 영구적으로 저장됨

-- At this moment: Server reboots. Power outage occurs. Disk fails. Data is still safe!
-- 이 순간: 서버가 재부팅됨. 정전이 발생함. 디스크가 깨짐. 데이터는 여전히 안전함!
```

### Storage Guarantee (저장 보장 방식)

```
Memory (Volatile)
    ↓
    ↓ At COMMIT
    ↓
Disk (Non-Volatile) ← Permanently saved!
```

### Why It Matters (왜 중요한가?)

- Committed data is never lost
- Protected from system failures
- Ensures business continuity

| - COMMIT한 데이터는 절대 손실되지 않음
| - 시스템 장애로부터 보호됨
| - 업무 연속성 보장

---

## 9.6 COMMIT and ROLLBACK

### COMMIT

**Role:** Permanently save all changes in a transaction

| **역할:** 트랜잭션의 모든 변경사항을 영구적으로 저장

```sql
START TRANSACTION;
  UPDATE employees SET salary = 5000000 WHERE employee_id = 1;
  -- At this point, only my session can see the changes
 -- 이 시점에서는 나 자신의 세션에서만 변경사항을 볼 수 있음
  
COMMIT;  -- Now everyone can see the changes
COMMIT;  -- 이제 모든 사람이 변경사항을 볼 수 있음
```

### ROLLBACK

**Role:** Cancel all changes in a transaction and restore to previous state

| **역할:** 트랜잭션의 모든 변경사항을 취소하고 이전 상태로 복원

```sql
START TRANSACTION;
  INSERT INTO employees VALUES (10, 'New Employee', 1, 3500000);
  -- New employee added (temporary)
  -- 새 직원이 추가됨 (임시)
  
  -- Problem discovered! Wrong information!
 -- 문제 발견! 잘못된 정보다!
  
ROLLBACK;  -- Insertion is canceled, employee not created
ROLLBACK;  -- 삽입이 취소됨, 직원이 생성되지 않음
```

### Real-World Use Cases (실제 사용 사례)

```sql
-- Case 1: Successful transaction  성공적인 트랜잭션
START TRANSACTION;
  UPDATE employees SET salary = 5500000 WHERE employee_id = 1;
  UPDATE employees SET dept_id = 2 WHERE employee_id = 1;
COMMIT;  -- Both changes are saved ✅  두 변경사항이 모두 저장됨

-- Case 2: Transaction with error  오류가 발생한 트랜잭션
START TRANSACTION;
  INSERT INTO employees (name, dept_id, salary) 
  VALUES ('New Employee', 99, 3500000);
  -- Error! dept_id 99 does not exist  dept_id 99는 존재하지 않음`
  
  -- Try another way  다른 방법으로 시도
  INSERT INTO employees (name, dept_id, salary) 
  VALUES ('New Employee', 1, 3500000);
  -- This one succeeds!  이번엔 성공!
  
COMMIT;  -- Only second insertion is saved
```

---

## 9.7 SAVEPOINT - Partial Rollback

**Purpose:** You can rollback only to a specific point within a transaction

| **용도:** 트랜잭션 내에서 특정 지점까지만 롤백할 수 있음

### Problem Situation - without  SAVEPOINT (문제 상황 - SAVEPOINT 없이)

```sql
START TRANSACTION;
  INSERT INTO employees VALUES (10, 'Employee1', 1, 3000000);  -- ✅ Success
  INSERT INTO employees VALUES (11, 'Employee2', 1, 3500000);  -- ✅ Success
  INSERT INTO employees VALUES (12, 'Employee3', 99, 3700000); -- ❌ Error!

ROLLBACK;  
-- All canceled (first two as well!)  모두 취소됨 (처음 두 개도!)
-- But I wanted to keep the first two...  하지만 처음 두 개는 지키고 싶었는데...
```


### Solution - with SAVEPOINT (해결책 - SAVEPOINT 사용)

```sql
START TRANSACTION;
  INSERT INTO employees VALUES (10, 'Employee1', 1, 3000000);  -- ✅ Success
  INSERT INTO employees VALUES (11, 'Employee2', 1, 3500000);  -- ✅ Success
  
  SAVEPOINT sp1;  -- Mark this point  이 지점을 표시해둠
  
  INSERT INTO employees VALUES (12, 'Employee3', 99, 3700000); -- ❌ Error!
  
  -- Rollback only to sp1 (first two are kept)
  -- sp1까지만 롤백 (처음 두 개는 유지)
  ROLLBACK TO sp1;
  
  -- Now try again with correct data
 -- 이제 올바른 데이터로 다시 시도
  INSERT INTO employees VALUES (12, 'Employee3', 1, 3700000);  -- ✅ Success!
  
COMMIT;  -- All three employees inserted! ✅  세 명 모두 삽입됨!.
```

### Multiple SAVEPOINTs - with multi-SAVEPOINT (여러 SAVEPOINT 사용)

```sql
START TRANSACTION;
  DELETE FROM logs WHERE created_date < '2024-01-01';  -- Delete logs
  SAVEPOINT sp1;
  
  UPDATE employees SET salary = salary * 1.1;  -- Salary increase
  SAVEPOINT sp2;
  
  DELETE FROM old_data WHERE archived = true;  -- Delete old data
  -- Problem occurs in this task!
  
  -- Rollback to sp2: salary increase is kept, data deletion is canceled
  -- sp2로 롤백: 급여 인상은 유지, 데이터 삭제는 취소
  ROLLBACK TO sp2;
  
COMMIT;  -- Only log deletion and salary increase are saved
         -- 로그 삭제와 급여 인상만 저장됨
```

---

## 9.8 Isolation Levels

**Isolation Levels** determine how much concurrent transactions affect each other and what concurrency problems may occur.

| **격리 수준**은 동시에 실행되는 트랜잭션들이 얼마나 서로 영향을 받을지를 결정합니다.

### Types of Concurrency Problems (동시성 문제의 종류)

#### 1. Dirty Read

**Problem:** Uncommitted changes are read by another transaction

| **문제:** 아직 커밋되지 않은 변경사항을 다른 트랜잭션이 읽음

```
Session A (Transaction 1)        Session B (Transaction 2)
───────────────────────────────────────────────────────
START TRANSACTION;
  UPDATE balance = 900;
  (NOT YET COMMIT)
                                 START TRANSACTION;
                                   SELECT balance;  -- 900 (Uncommitted value!)
                                   -- This is called Dirty Read
                                   Using 900 for calculation
                     
  ROLLBACK;  -- Cancel change!
  (balance returns to 1000)
                                   -- But B already calculated with 900!
                                   COMMIT;  -- Committed with wrong data ❌
```

#### 2. Non-Repeatable Read (반복 불가능한 읽기)

**Problem:** Same data returns different values when read twice

| **문제:** 같은 데이터를 두 번 읽을 때 다른 값이 나옴

```
Session A                        Session B
───────────────────────────────────────────────────────
START TRANSACTION;
  SELECT balance;  -- 1000
                                 START TRANSACTION;
                                   UPDATE balance = 900;
                                   COMMIT;  -- Committed
  
  SELECT balance;  -- 900 (Changed!)
  -- Same query but different result!
  
COMMIT;
```

#### 3. Phantom Read (팬텀 읽기)

**Problem:** New rows appear or disappear

| **문제:** 새로운 행이 나타나거나 사라짐

```
Session A                        Session B
───────────────────────────────────────────────────────
START TRANSACTION;
  SELECT COUNT(*) FROM accounts;  -- 100 rows
                                 START TRANSACTION;
                                   INSERT INTO accounts...;  -- Add new account
                                   COMMIT;  -- Committed
  
  SELECT COUNT(*) FROM accounts;  -- 101 rows!
  -- Phantom row appeared!
  
COMMIT;
```

### 4 Isolation Levels (4가지 격리 수준)

| Level                      | Dirty Read | Non-Repeatable Read | Phantom Read | Performance    |
| -------------------------- | ---------- | ------------------- | ------------ | -------------- |
| **READ UNCOMMITTED** | ✅ Occurs  | ✅ Occurs           | ✅ Occurs    | ⚡⚡⚡ Fastest |
| **READ COMMITTED**   | ❌ None    | ✅ Occurs           | ✅ Occurs    | ⚡⚡ Fast      |
| **REPEATABLE READ**  | ❌ None    | ❌ None             | ✅ Occurs    | ⚡ Slow        |
| **SERIALIZABLE**     | ❌ None    | ❌ None             | ❌ None      | 🐌 Slowest     |

|  |  |  |  |  |
| - | - | - | - | - |

### Characteristics of Each Level (각 수준의 특징)

#### 1. READ UNCOMMITTED (Lowest Protection)

```sql
SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
```

- ⚠️ Lowest protection level
- ✅ Fastest performance
- ❌ All problems possible: Dirty Read, Non-repeatable Read, Phantom Read
- 📊 Use: Non-critical statistics queries

| - ⚠️ 가장 낮은 보호 수준
| - ✅ 가장 빠른 성능
| - ❌ Dirty Read, Non-repeatable Read, Phantom Read 모두 발생 가능
| - 📊 사용: 정확성이 크리티컬하지 않은 통계 조회 등

#### 2. READ COMMITTED (Intermediate Protection)

```sql
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;
```

- 🛡️ Intermediate protection level
- ✅ Most commonly used (MySQL default)
- ❌ Prevents Dirty Read but Non-repeatable Read, Phantom Read possible
- 📊 Use: Most general business operations

| - 🛡️ 중간 수준 보호
| - ✅ 가장 일반적으로 사용됨 (MySQL 기본값)
| - ❌ Dirty Read는 방지하지만 Non-repeatable Read, Phantom Read 발생 가능
| - 📊 사용: 대부분의 일반적인 업무

#### 3. REPEATABLE READ (High Protection)

```sql
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;
```

- 🛡️🛡️ High protection level
- ⚠️ Performance degradation
- ✅ Prevents Dirty Read, Non-repeatable Read
- ❌ Phantom Read still possible
- 📊 Use: Operations requiring multiple reads of same data

| - 🛡️🛡️ 높은 수준 보호
| - ⚠️ 성능 감소
| - ✅ Dirty Read, Non-repeatable Read 방지
| - ❌ Phantom Read는 여전히 발생 가능
| - 📊 사용: 같은 데이터를 여러 번 읽어야 하는 업무

#### 4. SERIALIZABLE (Highest Protection)

```sql
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
```

- 🛡️🛡️🛡️ Highest protection level
- 🐌 Very slow performance
- ✅ Prevents all concurrency problems
- ⚠️ Transactions behave as if executed sequentially
- 📊 Use: Critical financial transactions

| - 🛡️🛡️🛡️ 최고 수준 보호
| - 🐌 매우 느린 성능
| - ✅ 모든 동시성 문제 방지
| - ⚠️ 트랜잭션이 마치 순차적으로 실행되는 것처럼 동작
| - 📊 사용: 매우 중요한 금융 거래 등

### Setting Isolation Levels (격리 수준 설정)

```sql
-- Set for current session  현재 세션에 대해 설정
SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED;

-- Apply to specific transaction only   특정 트랜잭션에만 적용
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;
START TRANSACTION;
  SELECT * FROM accounts;
COMMIT;

-- Check current setting  설정 확인
SELECT @@transaction_isolation;
```

---

## 9.9 Deadlock

**Deadlock** occurs when two or more transactions hold resources that each other needs, resulting in an infinite wait state.

| **데드락**은 두 개 이상의 트랜잭션이 서로 필요한 리소스를 가지고 있어서 무한 대기 상태가 되는 것입니다.

### Deadlock Occurrence Example (데드락 발생 사례)

```


Session A (Transaction 1)        Session B (Transaction 2)
───────────────────────────────────────────────────────
START TRANSACTION;
  SELECT * FROM accounts 
  WHERE id = 1 FOR UPDATE;  -- Lock account 1  계좌 1 잠금 ✅
  
  -- Want to update account 2
  -- 계좌 2를 업데이트하려고 함
  UPDATE accounts 
  SET balance = balance - 100
  WHERE id = 2;  -- Waiting for account 2 lock... 🔄
                 -- 계좌 2 잠금을 기다리는 중... 🔄
                                 START TRANSACTION;
                                   SELECT * FROM accounts 
                                   WHERE id = 2 FOR UPDATE;  -- Lock account 2  계좌 2 잠금 ✅
   
                                   -- Want to update account 1
                                   -- 계좌 1을 업데이트하려고 함
                                   UPDATE accounts 
                                   SET balance = balance + 100
                                   WHERE id = 1;  -- Waiting for account 1 lock... 🔄
                                                  -- 계좌 1 잠금을 기다리는 중... 🔄

💥 DEADLOCK OCCURS!
- A is waiting for account 2 lock held by B
- B is waiting for account 1 lock held by A
- Both will wait forever!
💥 데드락 발생! 
- A는 B가 가진 계좌 2의 잠금을 기다림 
- B는 A가 가진 계좌 1의 잠금을 기다림 
- 둘 다 영원히 기다릴 수밖에 없음! |``
```

### MySQL Deadlock Detection (MySQL의 데드락 감지)

When MySQL detects a deadlock, it automatically handles it:

| MySQL이 데드락을 감지하면 자동으로 처리합니다:

```sql
-- MySQL's automatic handling:
-- 1. Select one transaction (usually the one that modified fewer rows)
-- 2. Automatically ROLLBACK that transaction
-- 3. Return error 1213: "Deadlock found when trying to get lock"
-- 4. Other transaction continues normally

-- Result:
-- ❌ One transaction: Fails with error
-- ✅ Other transaction: Continues normally
```

| ``-- MySQL이 데드락을 감지되면 자동으로 처리: | -- 1. 한 트랜잭션 선택 (보통 더 적은 행을 변경한 것) | -- 2. 그 트랜잭션 자동 ROLLBACK | -- 3. 오류 1213 반환: "Deadlock found when trying to get lock" | -- 4. 다른 트랜잭션 계속 실행 | | -- 결과: | -- ❌ 한 트랜잭션: 오류와 함께 실패 | -- ✅ 다른 트랜잭션: 정상 계속 | ``

### Deadlock Prevention Strategies (데드락 예방 전략)

#### 1. Maintain Consistent Resource Order (리소스 순서 일관성 유지)

```sql
-- ❌ Bad Example
-- Session A
UPDATE accounts WHERE id = 1;
UPDATE accounts WHERE id = 2;

-- Session B
UPDATE accounts WHERE id = 2;  -- Different order!
UPDATE accounts WHERE id = 1;  -- Deadlock risk!

-- ✅ Good Example
-- All sessions access in same order
-- 모든 세션이 같은 순서로 접근
-- Session A
UPDATE accounts WHERE id = 1;
UPDATE accounts WHERE id = 2;

-- Session B
UPDATE accounts WHERE id = 1;  -- Same order!
UPDATE accounts WHERE id = 2;
```

#### 2. Keep Transactions Short (트랜잭션을 짧게 유지)

```sql
-- ❌ Bad Example (Long transaction)
START TRANSACTION;
  -- Many queries executed  많은 쿼리 실행
  UPDATE accounts SET...;
  INSERT INTO logs...;
  DELETE FROM old_data...;
  -- Holding locks for 30 seconds  30초 동안 잠금 유지
COMMIT;

-- ✅ Good Example (Short transaction)
START TRANSACTION;
  UPDATE accounts SET...;  -- Execute quickly  빠르게 실행
COMMIT;  -- Release lock quickly  빨리 잠금 해제
```

#### 3. Adjust Transaction Isolation Level (트랜잭션 고립 수준 조정)

```sql
-- Don't use unnecessarily high isolation levels
-- 필요 이상으로 높은 격리 수준 사용하지 않기
SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED;
-- READ COMMITTED is often sufficient
-- READ COMMITTED로 충분한 경우가 많음
```

#### 4. Use Row Locking Instead of Table Locking (테이블 잠금 대신 행 잠금 사용)

```sql
-- ❌ Bad Example (Table-level lock)
LOCK TABLES accounts WRITE;
UPDATE accounts SET...;
UNLOCK TABLES;

-- ✅ Good Example (Row-level lock)
START TRANSACTION;
  SELECT * FROM accounts WHERE id = 1 FOR UPDATE;  -- Lock specific row  행 잠금
  UPDATE accounts SET... WHERE id = 1;
COMMIT;
```

---

## 📚 Part 2: Sample Data Setup

### Create Sample Tables

```sql
CREATE DATABASE ch9_dml CHARACTER SET utf8mb4;
USE ch9_dml;

CREATE TABLE employees (
    employee_id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(50) NOT NULL,
    dept_id INT,
    salary DECIMAL(10, 2)
);

INSERT INTO employees VALUES
(1, 'Chulsu Kim', 1, 5000000),
(2, 'Younghee Lee', 1, 4000000),
(3, 'Minjun Park', 2, 4500000),
(4, 'Sunsin Choi', 2, 3500000);

CREATE TABLE departments (
    dept_id INT PRIMARY KEY AUTO_INCREMENT,
    department_name VARCHAR(50) NOT NULL
);

INSERT INTO departments VALUES
(1, 'Sales'),
(2, 'Engineering');
```

---

## 💻 Part 3: Practical Exercises


```sql
-- =====================================================
-- 9-1. Basic INSERT
-- =====================================================
-- Insert new employee info into specified columns only
-- Why specifying column names is safe: Table structure changes won't affect this query
-- 새로운 직원 정보를 지정된 열에만 삽입
-- 열 이름을 명시하는 것이 안전한 이유: 테이블 구조가 변경되어도 영향 없음

INSERT INTO employees (name, dept_id, salary)
VALUES ('Sujeong Hwang', 1, 4100000);

-- =====================================================
-- 9-2. INSERT - Specify Column Names (Recommended)
-- =====================================================
-- Always use this method!
-- 항상 이 방법을 사용하세요!

INSERT INTO employees (name, dept_id, salary)
VALUES ('Sunmin Geum', 2, 4300000);

-- =====================================================
-- 9-3. Bulk INSERT (Efficient)
-- =====================================================
-- Insert multiple rows at once - much faster than individual INSERTs!
-- Reduces network traffic and processed as single transaction
-- 여러 행을 한 번에 삽입하면 개별 INSERT보다 훨씬 빠름!
-- 네트워크 트래픽도 줄어들고, 트랜잭션으로도 처리됨

INSERT INTO employees (name, dept_id, salary) VALUES
('Jungi Song', 1, 3900000),
('Sejun Lim', 2, 4100000),
('Junho Park', 1, 3700000);

-- =====================================================
-- 9-4. INSERT with Subquery (Data Copying)
-- =====================================================
-- Copy data from one table to another
-- Use: Backup, Archive, Data Migration
-- 한 테이블의 데이터를 다른 테이블로 복사
-- 용도: 백업, 아카이브, 데이터 마이그레이션

CREATE TABLE IF NOT EXISTS employee_archive AS 
SELECT * FROM employees LIMIT 0;

INSERT INTO employee_archive
SELECT * FROM employees
WHERE salary >= (SELECT AVG(salary) FROM employees WHERE dept_id = 1);

-- =====================================================
-- 9-5. INSERT with DEFAULT
-- =====================================================
-- Columns with DEFAULT values can use DEFAULT keyword
-- 기본값이 설정된 열은 DEFAULT 키워드 사용 가능

INSERT INTO employees (name, dept_id, salary)
VALUES ('Soyoung Lee', 1, 4200000);

-- =====================================================
-- 9-6. Basic UPDATE
-- =====================================================
-- ⚠️ CRITICAL: Always include WHERE condition!
-- ⚠️ 중요: WHERE 조건을 반드시 포함하세요!

UPDATE employees
SET salary = 5200000
WHERE employee_id = 1;

-- =====================================================
-- 9-7. UPDATE Multiple Columns
-- =====================================================
-- Modify multiple columns at once
-- 여러 열을 동시에 수정

UPDATE employees
SET salary = 5500000, dept_id = 2
WHERE employee_id = 2;

-- =====================================================
-- 9-8. UPDATE with Expression
-- =====================================================
-- Update based on current values
-- 현재 값을 기반으로 수정

UPDATE employees
SET salary = salary * 1.1  -- 10% raise / 10% 인상
WHERE dept_id = 1;

-- =====================================================
-- 9-9. UPDATE with CASE
-- =====================================================
-- Apply different conditions based on department
-- 부서별로 다른 조건 적용

UPDATE employees
SET salary = CASE 
    WHEN dept_id = 1 THEN salary * 1.15
    WHEN dept_id = 2 THEN salary * 1.12
    ELSE salary * 1.10
END;

-- =====================================================
-- 9-10. Safe UPDATE Procedure
-- =====================================================
-- Step 1: Always verify first with SELECT
-- 단계 1: SELECT로 먼저 확인
SELECT * FROM employees WHERE dept_id = 2;

-- Step 2: Execute UPDATE if results are correct
-- 단계 2: 결과가 올바르면 UPDATE 실행
UPDATE employees
SET salary = salary * 1.05
WHERE dept_id = 2;

-- =====================================================
-- 9-11. Basic DELETE
-- =====================================================
-- ⚠️ CRITICAL: Always include WHERE condition!
-- ⚠️ 중요: WHERE 조건을 반드시 포함하세요!

DELETE FROM employees
WHERE employee_id = 7;

-- =====================================================
-- 9-12. DELETE with Condition
-- =====================================================
-- Delete multiple records matching condition
-- 조건에 맞는 여러 레코드 삭제

DELETE FROM employees
WHERE salary < 3500000;

-- =====================================================
-- 9-13. Safe DELETE Procedure
-- =====================================================
-- Step 1: Verify what will be deleted
-- 단계 1: 삭제될 데이터 확인
SELECT * FROM employees WHERE salary < 3600000;

-- Step 2: Execute DELETE if correct
-- 단계 2: 올바르면 DELETE 실행
DELETE FROM employees WHERE salary < 3600000;

-- =====================================================
-- 9-14. Simple Transaction - COMMIT
-- =====================================================

START TRANSACTION;
  INSERT INTO employees (name, dept_id, salary) 
  VALUES ('Yujung Choi', 1, 4300000);
  UPDATE employees 
  SET salary = salary * 1.05 
  WHERE dept_id = 1;
COMMIT;

-- =====================================================
-- 9-15. Transaction - ROLLBACK
-- =====================================================

START TRANSACTION;
  INSERT INTO employees (name, dept_id, salary) 
  VALUES ('Hojin Lee', 2, 4400000);
ROLLBACK;  -- Insertion is canceled / 삽입이 취소됨

-- =====================================================
-- 9-16. Bank Transfer Simulation
-- =====================================================
-- This exercise demonstrates the importance of transactions in maintaining data integrity
-- when transferring money between accounts.
-- 이 실습은 계좌 간 송금 시 트랜잭션의 중요성을 보여줍니다.

CREATE TABLE IF NOT EXISTS accounts (
    account_id INT PRIMARY KEY,
    account_name VARCHAR(50),
    balance DECIMAL(10, 2)
);

INSERT INTO accounts VALUES
(1001, 'Chulsu Kim', 1000000),
(1002, 'Younghee Lee', 500000);

START TRANSACTION;
  -- Withdraw 100,000 / 100,000 출금
  UPDATE accounts 
  SET balance = balance - 100000 
  WHERE account_id = 1001;
  
  -- Deposit 100,000 / 100,000 입금
  UPDATE accounts 
  SET balance = balance + 100000 
  WHERE account_id = 1002;
  
COMMIT;  -- Both succeed or both fail / 둘 다 성공하거나 둘 다 실패

-- =====================================================
-- 9-17. SAVEPOINT - Partial Rollback
-- =====================================================

START TRANSACTION;
  INSERT INTO employees (name, dept_id, salary) 
  VALUES ('Nahyeon Kim', 1, 4100000);
  
  SAVEPOINT sp1;
  
  INSERT INTO employees (name, dept_id, salary) 
  VALUES ('Suho Lee', 2, 4300000);
  
  ROLLBACK TO sp1;  -- Undo only the second insert / 두 번째 삽입만 취소
  
  INSERT INTO employees (name, dept_id, salary) 
  VALUES ('Suho Lee', 1, 4300000);  -- Correct data / 올바른 데이터
  
COMMIT;

-- =====================================================
-- 9-18. Multiple SAVEPOINTs
-- =====================================================

START TRANSACTION;
  DELETE FROM employees WHERE salary < 3000000;
  SAVEPOINT sp1;
  
  UPDATE employees SET salary = salary * 1.1;
  SAVEPOINT sp2;
  
  DELETE FROM employees WHERE dept_id = 3;
  
  ROLLBACK TO sp2;  -- Undo only the last delete / 마지막 삭제만 취소
  
COMMIT;

-- =====================================================
-- 9-19. Complex Transaction
-- =====================================================

START TRANSACTION;
  INSERT INTO employees (name, dept_id, salary) 
  VALUES ('New Employee', 1, 3500000);
  
  UPDATE employees 
  SET dept_id = 2 
  WHERE employee_id IN (2, 3);
  
  DELETE FROM employees WHERE salary < 3000000;
  
COMMIT;

-- =====================================================
-- 9-20. INSERT with Data Validation
-- =====================================================

INSERT INTO employees (name, dept_id, salary)
SELECT 'New Employee', 1, 4100000
WHERE NOT EXISTS (SELECT 1 FROM employees WHERE name = 'New Employee');

-- =====================================================
-- 9-21. INSERT IGNORE
-- =====================================================

INSERT IGNORE INTO employees (employee_id, name, dept_id, salary)
VALUES (1, '김철수', 1, 5000000);
INSERT IGNORE INTO employees (employee_id, name, dept_id, salary)
VALUES (100, 'John Doe', 1, 5000000);

-- =====================================================
-- 9-22. Data Migration
-- =====================================================

START TRANSACTION;
  INSERT INTO employee_archive 
  SELECT * FROM employees WHERE employee_id >= 10;
  
  DELETE FROM employees WHERE employee_id >= 10;
  
COMMIT;

-- =====================================================
-- 9-23. Batch UPDATE
-- =====================================================

START TRANSACTION;
  UPDATE employees
  SET salary = CASE 
      WHEN employee_id IN (1, 2, 3) THEN salary * 1.1
      WHEN employee_id IN (4, 5, 6) THEN salary * 1.05
      ELSE salary
  END;
COMMIT;

-- =====================================================
-- 9-24. Row Locking
-- =====================================================

START TRANSACTION;
  -- Lock this row / 이 행을 잠금
  SELECT * FROM employees WHERE employee_id = 1 FOR UPDATE;
  
  -- Other sessions cannot modify this row / 다른 세션은 이 행을 수정할 수 없음
  UPDATE employees SET salary = 5500000 WHERE employee_id = 1;
  
COMMIT;

-- =====================================================
-- 9-25. Verification Before Changes
-- =====================================================

-- Step 1: Verify employees / 직원 확인
SELECT * FROM employees WHERE dept_id = 1 AND salary < 4000000;

-- Step 2: Execute UPDATE / UPDATE 실행
START TRANSACTION;
  UPDATE employees 
  SET salary = salary * 1.15
  WHERE dept_id = 1 AND salary < 4000000;
COMMIT;

-- =====================================================
-- 9-26. Audit Trail
-- =====================================================
-- This table records all changes to employee salaries for compliance and auditing purposes.
-- 모든 직원 급여 변경을 기록하여 규정 준수 및 감시 목적으로 사용합니다.

CREATE TABLE IF NOT EXISTS employee_audit (
    audit_id INT AUTO_INCREMENT PRIMARY KEY,
    action VARCHAR(10),
    employee_id INT,
    old_salary DECIMAL(10, 2),
    new_salary DECIMAL(10, 2),
    change_date TIMESTAMP
);

START TRANSACTION;
  UPDATE employees SET salary = 5200000 WHERE employee_id = 1;
  INSERT INTO employee_audit 
  VALUES (NULL, 'UPDATE', 1, 5000000, 5200000, NOW());
COMMIT;

-- =====================================================
-- 9-27. TRUNCATE vs DELETE Comparison
-- =====================================================
-- TRUNCATE: Delete all rows very quickly (cannot rollback)
-- DELETE: Delete rows matching condition (can rollback)
-- TRUNCATE: 모든 행을 매우 빠르게 삭제 (롤백 불가)
-- DELETE: 조건에 맞는 행 삭제 (롤백 가능)

-- DELETE (can be protected by transaction) / DELETE (트랜잭션으로 보호 가능)
START TRANSACTION;
DELETE FROM employees WHERE dept_id = 4;
COMMIT;  -- or ROLLBACK / 또는 ROLLBACK

-- TRUNCATE (very fast but cannot rollback) / TRUNCATE (매우 빠르지만 롤백 불가)
-- TRUNCATE TABLE employees;  -- Delete all rows (cannot undo!) / 모든 행 삭제 (되돌릴 수 없음!)
```

---


```

```

---

## 📝 Part 4: Assignment Instructions

### Theoretical Assignments

**Assignment 1**: Explain INSERT, UPDATE, and DELETE statements in detail. When should each be used? Provide real examples.

| **과제 1:** INSERT, UPDATE, DELETE 문을 상세히 설명하세요. 각각이 언제 사용되어야 하는지 설명하고, 실제 사례를 들어주세요.

**Assignment 2**: Explain transaction concepts and ACID properties in detail. Why are transactions important? Use bank transfer example.

| **과제 2:** 트랜잭션의 개념과 ACID 특성을 상세히 설명하세요. 데이터베이스에서 트랜잭션이 중요한 이유를 은행 송금 예시로 설명하세요.

**Assignment 3**: Explain COMMIT, ROLLBACK, and SAVEPOINT in detail. When should each be used? Provide examples.

| **과제 3:** COMMIT, ROLLBACK, SAVEPOINT를 상세히 설명하세요. 각각이 언제 사용되며, 어떻게 동작하는지 예시와 함께 설명하세요.

**Assignment 4**: Discuss data consistency problems without transactions. Provide real scenarios and solutions.

| **과제 4:** 트랜잭션 없이 발생할 수 있는 데이터 일관성 문제를 논의하세요. 실제 발생 가능한 상황을 들어 설명하고, 어떻게 해결할 수 있는지 제시하세요.

**Assignment 5**: Compare safe DML practices with dangerous ones. Explain best practices for data integrity.

| **과제 5:** 안전한 DML 실습과 위험한 DML 실습을 비교하세요. 데이터 무결성을 보호하기 위한 베스트 프랙티스를 설명하세요.

Submit as: Word or PDF document (2-3 pages)

---

### Practical Assignments

**Assignment 1**: Practice various INSERT forms: basic, bulk, and subquery-based insertion.

| **과제 1:** INSERT 문의 여러 형태를 실습하세요: 기본 INSERT, 대량 INSERT, 서브쿼리를 이용한 INSERT.

**Assignment 2**: Practice safe UPDATE usage: verify with SELECT first, conditional UPDATE, CASE-based UPDATE.

| **과제 2:** UPDATE 문의 안전한 사용법을 실습하세요: WHERE 조건 확인, 조건부 UPDATE, CASE를 사용한 UPDATE.

**Assignment 3**: Practice safe DELETE usage: verify first with SELECT, include WHERE condition, use transactions.

| **과제 3:** DELETE 문의 안전한 사용법을 실습하세요: SELECT로 먼저 확인, WHERE 조건 포함, 트랜잭션 사용.

**Assignment 4**: Create transaction examples: successful COMMIT, failed ROLLBACK, SAVEPOINT partial rollback.

| **과제 4:** 트랜잭션 예시를 작성하세요: 성공하는 COMMIT, 실패하는 ROLLBACK, SAVEPOINT를 사용한 부분 롤백.

**Assignment 5**: Execute all exercises 9-1 to 9-27 and attach screenshots. Additionally, create 5+ business scenarios and implement them with transactions, explaining purpose and practical usage.

| **과제 5:** Part 3의 모든 실습(9-1 ~ 9-27)을 직접 실행하고 결과 스크린샷을 첨부하세요. 추가로 5개 이상의 비즈니스 시나리오를 만들어 트랜잭션으로 구현하고, 각 시나리오의 목적과 실무 활용 방법을 설명하세요.

Submit as: SQL file (Ch9_DML_Transaction_[StudentID].sql) and screenshots

---

Thank you for your hard work.

Prof. Cho Jeong-Hyun (peterchokr@gmail.com). Yeungnam University College
