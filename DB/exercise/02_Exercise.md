# Ch2 DDL Commands - Exercise Problems

Dear students! After completing Chapter 2, please solve the following exercises to verify your understanding.
Check the difficulty level of each section and study step by step.

---

## 📌 Learning Objectives Verification

After completing Chapter 2, you should understand:

- Understanding DDL commands (CREATE, ALTER, DROP)
- Criteria for selecting data types (VARCHAR vs CHAR, INT vs DECIMAL)
- Types and purposes of constraints (PRIMARY KEY, UNIQUE, NOT NULL, DEFAULT)
- Table structure design and modification

---

# Multiple Choice Questions (10 Questions)

## Beginner Level (5 Questions) - Basic Concept Verification

**Question 1** Which is appropriate for storing variable-length characters between VARCHAR(50) and CHAR(50)?

- ① VARCHAR(50) - variable-length, storage-efficient
- ② CHAR(50) - fixed-length, always 50 bytes
- ③ Both are variable-length
- ④ Depends on the situation

---

**Question 2** What is the correct choice between DECIMAL(10,2) and FLOAT when storing amounts?

- ① FLOAT is accurate (floating-point)
- ② DECIMAL(10,2) is accurate (fixed-point)
- ③ Both have same precision
- ④ Floating-point is safer

---

**Question 3** What is the biggest difference between PRIMARY KEY and UNIQUE?

- ① PRIMARY KEY does not allow NULL and only 1 per table; UNIQUE allows multiple NULLs and multiple per table
- ② UNIQUE allows multiple NULLs and multiple per table; PRIMARY KEY does not allow NULL
- ③ Functionally identical
- ④ PRIMARY KEY is slower

---

**Question 4** What is the purpose of NOT NULL + UNIQUE combination?

- ① Value must exist and not be duplicated
- ② Value may not exist but cannot be duplicated
- ③ Allows multiple NULLs
- ④ Identical to PRIMARY KEY

---

**Question 5** What is the role of DEFAULT constraint?

- ① Specifies default value to be saved automatically when not entered
- ② Prevents data modification
- ③ Prevents deletion
- ④ Restricts query permissions

---

## Intermediate Level (3 Questions) - Concept Application

**Question 6** Which of the following is correct CREATE TABLE syntax?

```sql
① CREATE TABLE student (
    student_id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(30) NOT NULL,
    gpa FLOAT,
    enrollment_date DATE DEFAULT CURRENT_DATE
);

② CREATE TABLE student (
    student_id INT,
    name VARCHAR(30) NOT NULL,
    gpa FLOAT PRIMARY KEY
);

③ CREATE TABLE student (
    student_id INT PRIMARY KEY NULL,
    name VARCHAR(30),
    gpa FLOAT
);
```

- ① Correct (all constraints appropriate)
- ② Correct (gpa as PRIMARY KEY)
- ③ Correct (NULL allowed in PRIMARY KEY)
- ④ Only ① is correct

---

**Question 7** To change salary column in employee table from INT to DECIMAL(12,2)?

- ① Only DROP TABLE and CREATE TABLE can be repeated
- ② ALTER TABLE employee MODIFY salary DECIMAL(12,2);
- ③ ALTER TABLE employee CHANGE salary salary DECIMAL(12,2);
- ④ Both ② and ③ are possible

---

**Question 8** Why use AUTO_INCREMENT?

- ① Automatically generates ID increasing by 1
- ② Prevents duplicate IDs
- ③ No need to manually enter ID when adding new row
- ④ All are correct

---

## Advanced Level (2 Questions) - Critical Thinking

**Question 9** Which of the following two table designs is better?

```
Design A:
student (student_id INT, name VARCHAR(100), 
         phone VARCHAR(15) NOT NULL,
         email VARCHAR(50) NOT NULL UNIQUE,
         major VARCHAR(50))

Design B:
student (student_id INT PRIMARY KEY AUTO_INCREMENT, 
         name VARCHAR(30) NOT NULL,
         phone VARCHAR(15),
         email VARCHAR(50) UNIQUE,
         major VARCHAR(30))
```

- ① Design A is better (all fields required)
- ② Design B is better (explicit PRIMARY KEY definition, flexibility)
- ③ Design A is better (all fields NOT NULL)
- ④ Both are same

---

**Question 10** Most appropriate criteria for determining VARCHAR length when designing table structure?

- ① Always as long as possible (ex: VARCHAR(1000))
- ② Consider maximum length of data to be stored + margin
  (ex: maximum 30 characters for name → VARCHAR(50))
- ③ Anything (no performance impact)
- ④ Always use CHAR

---

# Short Answer Questions (5 Questions)

## Beginner Level (3 Questions)

**Question 11** Explain the difference in storage space between VARCHAR(30) and CHAR(30) when storing 'Kim Chulsu' (5 characters).

---

**Question 12** Explain when each of the following 4 data types is used.

- INT, DECIMAL(10,2), VARCHAR(50), DATE

---

**Question 13** Explain 3 or more differences between PRIMARY KEY and UNIQUE.

---

## Intermediate Level (1 Question)

**Question 14** If you design a membership registration system, create a user table to store the following information:
User ID (auto-increment), Username (required), Email (no duplicates), Phone, Registration Date (auto current date), Monthly Salary (2 decimal places)

Write the CREATE TABLE SQL and explain why each constraint was chosen.

---

## Advanced Level (1 Question)

**Question 15** The following changes are needed in the existing employee table:

1. Add new department column (VARCHAR(30))
2. Change salary from INT to DECIMAL(12,2)
3. Add new hire_date column (DATE, default: current date)

Write the SQL using ALTER TABLE and analyze the impact of these changes on existing data.

---

# Practical Problems (5 Questions)

## Beginner Level (2 Questions)

**Question 16** Execute the following SQL and provide result screenshot.

```sql
CREATE DATABASE ch2_practice CHARACTER SET utf8mb4;
USE ch2_practice;

CREATE TABLE professor (
    prof_id INT PRIMARY KEY AUTO_INCREMENT,
    prof_name VARCHAR(30) NOT NULL,
    department VARCHAR(30),
    position VARCHAR(20),
    salary DECIMAL(10,2)
);

DESC professor;
```

Submission: One screenshot showing DESC professor result

---

**Question 17** Insert 5 professor records into professor table as follows and verify results.

```sql
INSERT INTO professor VALUES
(1, 'Kim Chulsu', 'Computer Science', 'Professor', 7500000),
(2, 'Lee Younghee', 'Software Engineering', 'Associate Professor', 6800000),
(3, 'Park Minjun', 'Information Security', 'Assistant Professor', 5900000),
(4, 'Choi Sunsin', 'Data Science', 'Professor', 7800000),
(5, 'Kang Gamchan', 'Computer Science', 'Associate Professor', 6500000);

SELECT * FROM professor;
```

Submission: One screenshot showing all 5 professor records

---

## Intermediate Level (2 Questions)

**Question 18** Perform the following changes to professor table and provide screenshot.

1. Add phone column (VARCHAR(15))
2. Modify salary to DECIMAL(12,2)
3. Verify changed table structure

```sql
ALTER TABLE professor ADD phone VARCHAR(15);
ALTER TABLE professor MODIFY salary DECIMAL(12,2);
DESC professor;
```

Submission: One screenshot showing final table structure (DESC result)

---

**Question 19** Design bank_account table for the following situation.

```
Information:
- Account number (auto-increment, primary key)
- Customer name (required)
- Account type (required: savings, fixed deposit, loan)
- Balance (2 decimal places, default 0)
- Opening date (default current date)
- Last transaction date (can be NULL)

Requirements:
1. Explain appropriate data type selection reasons
2. Explain constraint selection reasons
3. Write CREATE TABLE SQL
4. Insert 3 sample account records and provide screenshot

Submission: SQL and execution result screenshot
```

---

## Advanced Level (1 Question)

**Question 20** Improve the existing simple product table as follows.

```
Current table:
product (product_id, product_name, price, stock)

Improvement requirements:
1. product_id is PRIMARY KEY + AUTO_INCREMENT
2. product_name is NOT NULL + UNIQUE
3. price is DECIMAL(10,2) (no negative CHECK)
4. stock has default 0, no negative CHECK
5. Add category column (VARCHAR(30))
6. Add created_date column (default current date)
7. Add description column (text, can be NULL)

Questions:
1. Write improved CREATE TABLE SQL
2. Explain why each constraint was chosen
3. Insert 5 sample product records
4. Verify final table structure and data

Submission: Complete SQL code and execution result screenshot
```

---

---

# 📋 Answer Key and Model Answers

## Multiple Choice Answers (10 Questions)

| Question | Answer | Explanation                                                                               |
| :------: | :----: | :---------------------------------------------------------------------------------------- |
|    1    |   ①   | VARCHAR is variable-length and efficient (VARCHAR(50) with 5 chars uses 6 bytes)          |
|    2    |   ②   | DECIMAL is accurate fixed-point, essential for finance/accounting                         |
|    3    |   ①   | PRIMARY KEY doesn't allow NULL + 1 per table, UNIQUE allows multiple NULLs                |
|    4    |   ①   | NOT NULL + UNIQUE = value must exist and no duplicates (completely unique)                |
|    5    |   ①   | DEFAULT saves specified value automatically when not entered                              |
|    6    |   ①   | Correct syntax (② has error making gpa PK, ③ has error with NULL in PK)                 |
|    7    |   ④   | Both MODIFY and CHANGE possible (MODIFY changes type only, CHANGE can change name)        |
|    8    |   ④   | AUTO_INCREMENT satisfies all reasons                                                      |
|    9    |   ②   | Design B is better (explicit PRIMARY KEY, phone/email optional, appropriate VARCHAR size) |
|    10    |   ②   | Considering maximum length + margin is correct design principle                           |

---

## Short Answer Model Answers (5 Questions)

### Question 11 Storage Space Difference between VARCHAR(30) and CHAR(30)

**Model Answer**:

- **CHAR(30)**: Always 30 bytes (fixed-length)
- **VARCHAR(30)**: Storing 'Kim Chulsu' (5 chars) uses approximately 6~7 bytes (includes 1~2 bytes for length info)
- **Difference**: CHAR wastes 24 bytes, VARCHAR wastes nothing
- **Conclusion**: VARCHAR is storage-efficient for variable-length data

---

### Question 12 Data Type Usage Situations

**Model Answer**:

```
1. INT
   - Store integer data
   - Examples: student ID (202401), age (25), price (50000)

2. DECIMAL(10,2)
   - Requires decimal precision for amounts
   - Examples: salary (7500000.00), interest rate (4.25)

3. VARCHAR(50)
   - Variable-length strings
   - Examples: name (Kim Chulsu), email (kim@example.com), address

4. DATE
   - Store dates
   - Examples: enrollment date (2024-01-15), graduation date (2028-02-20)
```

---

### Question 13 PRIMARY KEY vs UNIQUE

**Model Answer** (3 or more differences):

1. **NULL Handling**: PRIMARY KEY doesn't allow NULL, UNIQUE allows multiple NULLs
2. **Quantity Limit**: PRIMARY KEY only 1 per table, UNIQUE multiple possible
3. **Performance**: PRIMARY KEY auto-creates index, UNIQUE also usually creates index
4. **Purpose**: PRIMARY KEY for row identification, UNIQUE for duplicate prevention
5. **Foreign Key**: Only PRIMARY KEY can be referenced by foreign keys in other tables

---

### Question 14 user Table Design

**Model Answer**:

```sql
CREATE TABLE user (
    user_id INT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(30) NOT NULL,
    email VARCHAR(50) NOT NULL UNIQUE,
    phone VARCHAR(15),
    signup_date DATE DEFAULT CURRENT_DATE,
    monthly_salary DECIMAL(12,2)
);
```

**Constraint Selection Reasons**:

- **user_id**: PRIMARY KEY + AUTO_INCREMENT → auto-generate, no duplicates
- **username**: NOT NULL → username is required
- **email**: UNIQUE → prevent duplicate registration, essential for login
- **phone**: can be NULL → optional information
- **signup_date**: DEFAULT CURRENT_DATE → auto-save current date
- **monthly_salary**: DECIMAL(12,2) → decimal precision needed

---

### Question 15 ALTER TABLE Modifications

**Model Answer**:

```sql
-- 1. Add department column
ALTER TABLE employee ADD department VARCHAR(30);

-- 2. Change salary from INT to DECIMAL(12,2)
ALTER TABLE employee MODIFY salary DECIMAL(12,2);

-- 3. Add hire_date column
ALTER TABLE employee ADD hire_date DATE DEFAULT CURRENT_DATE;
```

**Impact on Existing Data**:

1. **Adding department**: Existing rows' department set to NULL (existing data preserved)
2. **Changing salary**: Converted from INT to DECIMAL(12,2) (decimal auto-generated, data safe)
3. **Adding hire_date**: Existing rows' hire_date set to NULL (needs UPDATE for actual values later)

---

## Practical Model Answers (5 Questions)

### Question 16 Create professor Table

**Completion Criteria**:
✅ ch2_practice database created
✅ professor table created (5 columns)
✅ DESC professor result shows: prof_id, prof_name, department, position, salary
✅ Constraints verified: prof_id PRIMARY KEY AUTO_INCREMENT, prof_name NOT NULL

**Screenshot Content**:

- DESC professor result
- All column data types and constraints displayed

---

### Question 17 Insert professor Table Data

**Completion Criteria**:
✅ 5 professor records inserted
✅ SELECT * FROM professor executed
✅ All data saved correctly

**Screenshot Content**:

- All 5 professor records displayed

**Expected Result**:

```
prof_id | prof_name | department           | position              | salary
1       | Kim Chulsu| Computer Science     | Professor             | 7500000.00
2       | Lee Younghee | Software Engineering | Associate Professor | 6800000.00
3       | Park Minjun | Information Security | Assistant Professor | 5900000.00
4       | Choi Sunsin | Data Science        | Professor             | 7800000.00
5       | Kang Gamchan | Computer Science   | Associate Professor   | 6500000.00
```

---

### Question 18 ALTER TABLE Modifications

**Completion Criteria**:
✅ ALTER TABLE professor ADD phone VARCHAR(15); executed
✅ ALTER TABLE professor MODIFY salary DECIMAL(12,2); executed
✅ DESC professor result: 6 columns shown (phone added, salary type changed)

**Screenshot Content**:

- Modified table structure
- Newly added phone column displayed
- salary type changed to DECIMAL(12,2)

---

### Question 19 bank_account Table Design

**Model Answer**:

```sql
CREATE TABLE bank_account (
    account_id INT PRIMARY KEY AUTO_INCREMENT,
    customer_name VARCHAR(30) NOT NULL,
    account_type VARCHAR(20) NOT NULL,
    balance DECIMAL(15,2) DEFAULT 0,
    opening_date DATE DEFAULT CURRENT_DATE,
    last_transaction_date DATETIME
);

INSERT INTO bank_account VALUES
(1, 'Kim Chulsu', 'Savings', 5000000.00, CURRENT_DATE, NULL),
(2, 'Lee Younghee', 'Fixed Deposit', 2000000.00, CURRENT_DATE, '2024-12-25 14:30:00'),
(3, 'Park Minjun', 'Loan', 10000000.00, CURRENT_DATE, '2024-12-24 09:15:00');

SELECT * FROM bank_account;
```

**Data Type Selection Reasons**:

- **account_id**: INT PRIMARY KEY AUTO_INCREMENT → auto-generate account number, no duplicates
- **customer_name**: VARCHAR(30) NOT NULL → customer name required
- **account_type**: VARCHAR(20) NOT NULL → account type required
- **balance**: DECIMAL(15,2) DEFAULT 0 → amount needs precision, default 0
- **opening_date**: DATE DEFAULT CURRENT_DATE → auto-save current date
- **last_transaction_date**: DATETIME → time information needed (can be NULL)

---

### Question 20 Improve product Table

**Model Answer**:

```sql
CREATE TABLE product (
    product_id INT PRIMARY KEY AUTO_INCREMENT,
    product_name VARCHAR(50) NOT NULL UNIQUE,
    price DECIMAL(10,2) CHECK (price >= 0),
    stock INT DEFAULT 0 CHECK (stock >= 0),
    category VARCHAR(30),
    created_date DATE DEFAULT CURRENT_DATE,
    description TEXT
);

INSERT INTO product VALUES
(1, 'Wireless Mouse', 45000.00, 100, 'Electronics', CURRENT_DATE, 'Comfortable grip wireless mouse'),
(2, 'Mechanical Keyboard', 120000.00, 50, 'Electronics', CURRENT_DATE, 'RGB LED mechanical keyboard'),
(3, 'Monitor Arm', 65000.00, 75, 'Electronics', CURRENT_DATE, 'Stand-type monitor arm'),
(4, 'Desk', 250000.00, 20, 'Furniture', CURRENT_DATE, 'Solid wood study desk'),
(5, 'Chair Cushion', 28000.00, 150, 'Furniture', CURRENT_DATE, 'Breathable mesh chair cushion');

SELECT * FROM product;
```

**Constraint Selection Reasons**:

- **product_id**: PRIMARY KEY + AUTO_INCREMENT → auto-generate product ID, no duplicates
- **product_name**: NOT NULL + UNIQUE → product name required, no duplicates
- **price**: DECIMAL(10,2) + CHECK (price >= 0) → amount precision needed, prevent negatives
- **stock**: DEFAULT 0 + CHECK (stock >= 0) → default 0, prevent negatives
- **category**: VARCHAR(30) can be NULL → classification information
- **created_date**: DEFAULT CURRENT_DATE → auto-save registration date
- **description**: TEXT → store detailed description

---

Prof. Jeong-Hyun Cho (peterchokr@gmail.com)
Yeungnam University College
