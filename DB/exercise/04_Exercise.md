# Ch4 WHERE Clause - Exercise Problems

Dear students! After completing Chapter 4, please solve the following exercises to verify your understanding.
Check the difficulty level of each section and study step by step.

---

## 📌 Learning Objectives Verification

After completing Chapter 4, you should understand:

- Data filtering using WHERE clause
- Comparison operators (=, !=, <, >, <=, >=)
- Logical operators (AND, OR, NOT)
- IN, BETWEEN, LIKE operators
- NULL value handling (IS NULL, IS NOT NULL)
- Complex conditional expression writing

---

# Multiple Choice Questions (10 Questions)

## Beginner Level (5 Questions) - Basic Concept Verification

**Question 1** What is the basic purpose of WHERE clause?

- ① Sort data
- ② Filter rows matching conditions
- ③ Change data type
- ④ Limit number of rows

---

**Question 2** What is the difference between != and <> comparison operators?

- ① != doesn't work
- ② <> doesn't work
- ③ Both mean the same (not equal)
- ④ Completely different functions

---

**Question 3** Which SQL correctly checks NULL values?

- ① WHERE phone = NULL;
- ② WHERE phone != NULL;
- ③ WHERE phone IS NULL;
- ④ WHERE phone <> NULL;

---

**Question 4** What is the main advantage of IN operator?

- ① Faster execution speed
- ② Better readability and concisely express multiple values
- ③ More accurate search
- ④ Reduced memory usage

---

**Question 5** What does LIKE 'K%' mean?

- ① Search only 'K'
- ② All strings starting with 'K'
- ③ All strings containing 'K'
- ④ All strings ending with 'K'

---

## Intermediate Level (3 Questions) - Concept Application

**Question 6** Which is correct about AND and OR priority?

- ① AND and OR have same priority
- ② AND executes before OR
- ③ OR executes before AND
- ④ Executes left to right only

---

**Question 7** What does BETWEEN 20 AND 30 equal to?

- ① age > 20 AND age > 30
- ② age >= 20 AND age <= 30
- ③ age > 20 AND age < 30
- ④ age >= 20 OR age <= 30

---

**Question 8** What is execution order in the following query?

```sql
WHERE city = 'Seoul' AND salary > 5000000 AND position = 'Manager'
```

- ① Sequential execution left to right
- ② Evaluate each condition and must satisfy all for TRUE
- ③ Condition order doesn't matter
- ④ Start with highest selectivity condition

---

## Advanced Level (2 Questions) - Critical Thinking

**Question 9** Which query has better performance?

```
Query A:
WHERE city IN ('Seoul', 'Busan', 'Daegu', 'Daejeon', 'Gwangju')

Query B:
WHERE city = 'Seoul' OR city = 'Busan' OR city = 'Daegu' 
      OR city = 'Daejeon' OR city = 'Gwangju'
```

- ① Query A is faster (more concise)
- ② Query B is faster (more explicit)
- ③ Both queries have same performance
- ④ Depends on situation

---

**Question 10** Analyze situation and select most efficient WHERE clause.

"Query employees with salary >= 5000000 AND position = 'Director' or 'Executive',
but exclude resigned employees (resign_date IS NOT NULL)"

```
① WHERE salary >= 5000000 AND (position = 'Director' OR position = 'Executive')
      AND resign_date IS NULL;

② WHERE salary >= 5000000 AND position IN ('Director', 'Executive')
      AND resign_date IS NULL;

③ WHERE salary >= 5000000 AND position IN ('Director', 'Executive')
      AND resign_date IS NOT NULL;
```

- ① Correct (all conditions appropriate)
- ② Correct (more concise with IN)
- ③ Error (IS NOT NULL shows resigned)
- ④ Both ① and ② are correct

---

# Short Answer Questions (5 Questions)

## Beginner Level (3 Questions)

**Question 11** Explain execution order of WHERE clause and SELECT.

---

**Question 12** Explain why = NULL doesn't work when checking NULL values.

---

**Question 13** Explain difference between IN and OR operators and provide examples when to use each.

---

## Intermediate Level (1 Question)

**Question 14** Explain why AND has higher priority than OR and provide practical example where parentheses must be used correctly.

---

## Advanced Level (1 Question)

**Question 15** Explain performance optimization methods when writing complex WHERE clauses.

---

# Practical Problems (5 Questions)

## Beginner Level (2 Questions)

**Question 16** Execute following SQL and provide result screenshot.

```sql
CREATE DATABASE ch4_filtering CHARACTER SET utf8mb4;
USE ch4_filtering;

CREATE TABLE customer (
    customer_id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(30) NOT NULL,
    city VARCHAR(20),
    age INT,
    grade VARCHAR(10),
    salary INT,
    phone VARCHAR(15)
) CHARACTER SET utf8mb4;

INSERT INTO customer VALUES
(1, 'Kim Chulsu', 'Seoul', 35, 'Gold', 5000000, '010-1111-1111'),
(2, 'Lee Younghee', 'Busan', 28, 'Silver', 3500000, '010-2222-2222'),
(3, 'Park Boyoung', 'Seoul', 42, 'Gold', 6000000, '010-3333-3333'),
(4, 'Choi Minji', 'Daegu', 25, 'Silver', 3000000, '010-4444-4444'),
(5, 'Jung Junho', 'Seoul', 38, 'Platinum', 7500000, NULL);

SELECT * FROM customer;
```

Submission: One screenshot showing all 5 customer records inserted

---

**Question 17** Perform following on customer table and verify results.

```sql
-- 1. Query customers in Seoul
SELECT * FROM customer WHERE city = 'Seoul';

-- 2. Query customers age 30 or older
SELECT * FROM customer WHERE age >= 30;

-- 3. Query customers with salary >= 4000000
SELECT * FROM customer WHERE salary >= 4000000;
```

Submission: Screenshot showing all 3 query results

---

## Intermediate Level (2 Questions)

**Question 18** Perform following queries on customer table.

```sql
-- 1. Customers in Seoul AND age >= 30
SELECT * FROM customer 
WHERE city = 'Seoul' AND age >= 30;

-- 2. Gold grade AND salary >= 4000000
SELECT * FROM customer 
WHERE grade = 'Gold' AND salary >= 4000000;

-- 3. Customers in Busan OR Daegu (using IN)
SELECT * FROM customer 
WHERE city IN ('Busan', 'Daegu');
```

Submission: Screenshot showing all 3 query results

---

**Question 19** Perform following analysis on customer table.

```
Questions:
1. Write SQL to query customers without phone number
2. Write SQL to filter by amount range (e.g., >= 3000000 AND <= 5000000)
3. Write SQL to query customers whose name starts with 'K'
4. Provide screenshot of each query result

Submission: 3 SQL codes + execution result screenshots
```

---

## Advanced Level (1 Question)

**Question 20** Write and execute following complex WHERE clauses using customer table.

```
Requirements:
1. Customers in Seoul AND (Gold or Platinum grade)
   SQL: WHERE city = 'Seoul' AND (grade = 'Gold' OR grade = 'Platinum')
        OR WHERE city = 'Seoul' AND grade IN ('Gold', 'Platinum')

2. (Age >= 30) AND (Busan OR Seoul resident) AND (salary >= 4000000)
   Write SQL

3. No phone number AND salary >= 5000000
   Write SQL

4. Write 2 or more free-form WHERE clauses using:
   - NOT operator
   - LIKE pattern
   - 3+ AND/OR combination

Submission: 
   - SQL code for each query
   - Screenshot of each query result
```

---

---

# 📋 Answer Key and Model Answers

## Multiple Choice Answers (10 Questions)

| Question | Answer | Explanation                                         |
| :------: | :----: | :-------------------------------------------------- |
|    1    |   ②   | WHERE filters rows matching conditions              |
|    2    |   ③   | != and <> both mean "not equal" (identical)         |
|    3    |   ③   | NULL must be checked with IS NULL/IS NOT NULL only  |
|    4    |   ②   | IN expresses multiple values concisely              |
|    5    |   ②   | % means 0+ characters (any beginning/end)           |
|    6    |   ②   | AND has higher priority than OR                     |
|    7    |   ②   | BETWEEN 20 AND 30 = >= 20 AND <= 30                 |
|    8    |   ③   | AND conditions all must be true regardless of order |
|    9    |  ①②  | Both work but IN is more concise (①② correct)     |
|    10    |   ④   | ① and ② both correct (③ wrong condition)         |

---

## Short Answer Model Answers (5 Questions)

### Question 11 Execution Order of WHERE and SELECT

**Model Answer**:

```
1. FROM: Select table
2. WHERE: Filter rows by condition
3. SELECT: Select desired columns from filtered rows
4. ORDER BY: Sort results (if present)
5. LIMIT: Limit number of rows (if present)

Example: SELECT name, salary FROM customer
         WHERE city = 'Seoul'
       
Execution: customer table → filter city='Seoul' → select name, salary columns
```

---

### Question 12 Why = NULL Doesn't Work

**Model Answer**:

```
Why = NULL doesn't work:
- NULL is not a value but a state
- Result of NULL = NULL is UNKNOWN (not TRUE/FALSE)
- SQL excludes UNKNOWN rows from SELECT results

Correct checks:
- IS NULL: Check NULL state
- IS NOT NULL: Check non-NULL values

Example:
Wrong query: WHERE phone = NULL; → Returns 0 rows
Correct query: WHERE phone IS NULL; → Returns customers without phone
```

---

### Question 13 IN vs OR

**Model Answer**:

```
IN operator:
- Syntax: WHERE city IN ('Seoul', 'Busan', 'Daegu')
- Advantage: Concise and readable
- Use: When 3+ values

OR operator:
- Syntax: WHERE city = 'Seoul' OR city = 'Busan' OR city = 'Daegu'
- Advantage: Explicit and flexible
- Use: When 1-2 values

Comparison:
IN: Superior readability, easier optimization
OR: Each condition clearly expressed

Real-world examples:
- 3+ cities: WHERE city IN ('Seoul', 'Busan', 'Daegu', 'Daejeon')
- Simple 2 conditions: WHERE grade = 'Gold' OR grade = 'Silver'
```

---

### Question 14 AND Priority and Parentheses

**Model Answer**:

```
Why AND has higher priority:
- Evaluates more specific conditions first
- Performs data filtering more efficiently
- Gets intended results in complex conditions

When parentheses are necessary:

Wrong example:
WHERE city = 'Seoul' OR city = 'Busan' AND salary > 5000000;
Executes as: (city = 'Seoul') OR (city = 'Busan' AND salary > 5000000)
→ All Seoul residents + Busan high earners

Correct example:
WHERE (city = 'Seoul' OR city = 'Busan') AND salary > 5000000;
→ Only Seoul or Busan high earners

Conclusion: Always use parentheses when mixing OR and AND
```

---

### Question 15 Complex WHERE Performance Optimization

**Model Answer**:

```
Optimization Method 1: Higher selectivity conditions first
WHERE salary > 5000000 AND city = 'Seoul'
(Filter by salary first to reduce rows)

Optimization Method 2: Utilize indexes
WHERE indexed_column = value AND other_column = value
(Include indexed columns in conditions)

Optimization Method 3: Minimize function usage
Good: WHERE hire_date >= '2024-01-01'
Bad: WHERE YEAR(hire_date) = 2024 (function usage)

Optimization Method 4: Remove unnecessary OR
WHERE city IN ('Seoul', 'Busan') ← Use IN
Instead of
WHERE city = 'Seoul' OR city = 'Busan' ← Avoid OR repetition

Optimization Method 5: Data type consistency
WHERE salary = 5000000 (type match)
Instead of
WHERE salary = '5000000' (implicit conversion occurs)
```

---

Prof. Jeong-Hyun Cho (peterchokr@gmail.com)
Yeungnam University College
