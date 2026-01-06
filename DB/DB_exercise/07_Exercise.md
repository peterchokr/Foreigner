# Ch7 Aggregate Functions and Grouping - Exercise Problems

Dear students! After completing Chapter 7, please solve the following exercises to verify your understanding.
Check the difficulty level of each section and study step by step.

---

## 📌 Learning Objectives Verification

After completing Chapter 7, you should understand:

- COUNT, SUM, AVG, MAX, MIN aggregate functions
- Data grouping using GROUP BY
- Filter groups with HAVING clause
- Impact of NULL values on aggregate functions
- Multiple column grouping (GROUP BY col1, col2)

---

# Multiple Choice Questions (10 Questions)

## Beginner Level (5 Questions) - Basic Concept Verification

**Question 1** Biggest difference between COUNT(*) and COUNT(column)?

- ① COUNT(*) is faster
- ② COUNT(*) includes NULL, COUNT(column) excludes NULL
- ③ COUNT(column) removes duplicates, COUNT(*) doesn't
- ④ Functions completely identical

---

**Question 2** Basic purpose of GROUP BY?

- ① Sort data
- ② Divide rows into groups and apply aggregate functions
- ③ Filter data
- ④ Redefine table

---

**Question 3** How are NULL values handled in aggregate functions?

- ① NULL calculated as 0
- ② NULL ignored and excluded
- ③ NULL causes error
- ④ Depends on function

---

**Question 4** Role of HAVING clause?

- ① Filter individual rows (same as WHERE)
- ② Filter groups created by GROUP BY
- ③ Sort data
- ④ Select columns

---

**Question 5** Meaning of COUNT(DISTINCT column)?

- ① Count all rows including duplicates
- ② Count unique values excluding duplicates
- ③ Count rows including NULL
- ④ Count specific values only

---

## Intermediate Level (3 Questions) - Concept Application

**Question 6** Correct usage of GROUP BY and aggregate functions?

```sql
① SELECT dept_id, salary, COUNT(*)
   FROM employees
   GROUP BY dept_id;

② SELECT dept_id, COUNT(*), AVG(salary)
   FROM employees
   GROUP BY dept_id;

③ SELECT dept_id, COUNT(*), salary
   FROM employees
   GROUP BY dept_id;
```

- ① Correct
- ② Correct
- ③ Error (salary not grouped)
- ④ ①② are correct

---

**Question 7** Difference between HAVING and WHERE clauses?

```sql
SELECT dept_id, AVG(salary)
FROM employees
WHERE salary > 5000000        -- ①
GROUP BY dept_id
HAVING AVG(salary) > 5500000; -- ②
```

- ① WHERE filters individual rows before grouping, HAVING filters groups after grouping
- ② WHERE and HAVING have same function
- ③ Only HAVING can use aggregate functions
- ④ ① and ③ both correct

---

**Question 8** To show departments with average salary >= 4,000,000?

- ① WHERE AVG(salary) >= 4000000
- ② HAVING AVG(salary) >= 4000000
- ③ GROUP BY HAVING AVG(salary) >= 4000000
- ④ ORDER BY AVG(salary) >= 4000000

---

## Advanced Level (2 Questions) - Critical Thinking

**Question 9** Why might these queries return different results?

```
Query A:
SELECT COUNT(*) FROM employees;

Query B:
SELECT COUNT(manager_id) FROM employees;
```

- ① Query A is slower
- ② Query B excludes rows where manager_id is NULL (may return smaller count)
- ③ Both return same result
- ④ Query B has error

---

**Question 10** Result when using aggregate functions without GROUP BY?

```sql
SELECT COUNT(*), SUM(salary), AVG(salary), MAX(salary)
FROM employees;
```

- ① Error occurs
- ② Returns overall statistics (1 row)
- ③ Repeats for each employee
- ④ Empty result

---

# Short Answer Questions (5 Questions)

## Beginner Level (3 Questions)

**Question 11** Explain difference between COUNT(*), SUM(), AVG(), MAX(), MIN().

---

**Question 12** Explain when to use GROUP BY and write query to get employee count per department.

---

**Question 13** Explain impact of NULL values on aggregate functions and provide example where COUNT(*) and COUNT(column) return different results.

---

## Intermediate Level (1 Question)

**Question 14** Explain necessity of HAVING clause and clearly distinguish between WHERE and HAVING.

---

## Advanced Level (1 Question)

**Question 15** Explain precautions when grouping by multiple columns (GROUP BY col1, col2) and performance optimization methods.

---

# Practical Problems (5 Questions)

## Beginner Level (2 Questions)

**Question 16** Execute following SQL and provide result screenshot.

```sql
CREATE DATABASE ch7_aggregation CHARACTER SET utf8mb4;
USE ch7_aggregation;

CREATE TABLE sales (
    sale_id INT PRIMARY KEY AUTO_INCREMENT,
    product_id INT,
    quantity INT,
    unit_price DECIMAL(10, 2),
    sale_date DATE,
    employee_id INT
);

CREATE TABLE products (
    product_id INT PRIMARY KEY AUTO_INCREMENT,
    product_name VARCHAR(50),
    category VARCHAR(30),
    price DECIMAL(10, 2)
);

INSERT INTO sales VALUES
(1, 1, 10, 50000, '2024-01-15', 1),
(2, 2, 5, 100000, '2024-01-15', 1),
(3, 1, 8, 50000, '2024-01-16', 2),
(4, 3, 3, 200000, '2024-01-16', 2),
(5, 2, 15, 100000, '2024-01-17', 1),
(6, 1, 20, 50000, '2024-01-17', 3),
(7, 4, 2, 500000, '2024-01-18', 3),
(8, 2, 10, 100000, '2024-01-18', 2);

INSERT INTO products VALUES
(1, 'Laptop A', 'Electronics', 50000),
(2, 'Mouse B', 'Electronics', 100000),
(3, 'Monitor C', 'Electronics', 200000),
(4, 'Keyboard D', 'Electronics', 500000);

SELECT * FROM sales;
SELECT * FROM products;
```

Submission: Screenshot showing sales (8 records) and products (4 records) tables

---

**Question 17** Perform following on sales table and verify results.

```sql
-- 1. Total quantity sold
SELECT SUM(quantity) AS total_quantity FROM sales;

-- 2. Average price
SELECT AVG(unit_price) AS avg_price FROM sales;

-- 3. Maximum price
SELECT MAX(unit_price) AS max_price FROM sales;
```

Submission: Screenshot showing all 3 query results

---

## Intermediate Level (2 Questions)

**Question 18** Perform following on sales table.

```sql
-- 1. Quantity by product
SELECT product_id, SUM(quantity) AS total_quantity
FROM sales
GROUP BY product_id;

-- 2. Average price by product
SELECT product_id, AVG(unit_price) AS avg_price
FROM sales
GROUP BY product_id;

-- 3. Number of sales by product
SELECT product_id, COUNT(*) AS sales_count
FROM sales
GROUP BY product_id;
```

Submission: Screenshot showing all 3 query results

---

**Question 19** Perform following analysis on sales table.

```sql
-- 1. Products with quantity >= 5
SELECT product_id, SUM(quantity) AS total_quantity
FROM sales
GROUP BY product_id
HAVING SUM(quantity) >= 5;

-- 2. Products with average price >= 100000
SELECT product_id, AVG(unit_price) AS avg_price
FROM sales
GROUP BY product_id
HAVING AVG(unit_price) >= 100000;

-- 3. Products with 2+ sales
SELECT product_id, COUNT(*) AS sales_count
FROM sales
GROUP BY product_id
HAVING COUNT(*) >= 2;
```

Submission: Screenshot showing all 3 query results

---

## Advanced Level (1 Question)

**Question 20** Write and execute complex aggregate queries.

```
Requirements:
1. Group by category and product to aggregate quantity and price

2. Category sales overview (quantity, sales count, average price)
   with descending order by quantity

3. Top 3 categories by quantity (using LIMIT)

4. Write 2+ free-form queries using:
   - GROUP BY with HAVING combination
   - COUNT(DISTINCT) usage
   - Sorting and limiting

Submission:
   - SQL code for each query
   - Screenshot of each query result
```

---

---

# 📋 Answer Key and Model Answers

## Multiple Choice Answers (10 Questions)

| Question | Answer | Explanation                                                                      |
| :------: | :----: | :------------------------------------------------------------------------------- |
|    1    |   ②   | COUNT(*) includes NULL, COUNT(column) excludes NULL                              |
|    2    |   ②   | GROUP BY divides rows into groups and applies aggregate functions                |
|    3    |   ④   | Handling differs by function (some exclude NULL, some include)                   |
|    4    |   ②   | HAVING filters groups created by GROUP BY                                        |
|    5    |   ②   | COUNT(DISTINCT) counts unique values only                                        |
|    6    |   ④   | ①② are correct, ③ has error                                                   |
|    7    |   ④   | WHERE filters rows before GROUP BY, HAVING filters groups after, ③ also correct |
|    8    |   ②   | HAVING for filtering groups with aggregate conditions                            |
|    9    |   ②   | manager_id NULL values excluded from COUNT(column)                               |
|    10    |   ②   | Without GROUP BY, returns single row with overall statistics                     |

---

## Short Answer Model Answers (5 Questions)

### Question 11: Difference Between Aggregate Functions

**Model Answer**:

```
COUNT(*): Counts all rows including NULL

COUNT(column): Counts rows where column is NOT NULL

SUM(column): Sums numeric values (NULL excluded)

AVG(column): Calculates average (NULL excluded)

MAX(column): Returns maximum value (NULL excluded)

MIN(column): Returns minimum value (NULL excluded)

NULL Handling:
- SUM, AVG, MAX, MIN: Automatically exclude NULL
- COUNT(*): Includes NULL in count
- COUNT(column): Excludes NULL from count
```

---

### Question 12: When to Use GROUP BY

**Model Answer**:

```
GROUP BY needed when:
- Grouping data by specific criteria (department, category, etc.)
- Need statistics for each group
- Example: Average salary per department, sales quantity per product

Query to count employees per department:
SELECT dept_id, COUNT(*) AS employee_count
FROM employees
GROUP BY dept_id;

Expected result:
dept_id | employee_count
1       | 3
2       | 2
3       | 2
```

---

### Question 13: Impact of NULL on Aggregate Functions

**Model Answer**:

```
NULL impact on aggregates:
- COUNT(*): Includes NULL in count
- COUNT(column): Excludes NULL values
- SUM, AVG, MAX, MIN: Ignore NULL values

Example scenario:
10 employees, 2 have manager_id = NULL

COUNT(*) FROM employees → 10
COUNT(manager_id) FROM employees → 8 (excludes 2 NULL)

Result:
- Difference shows number of NULL values
- Large NULL presence causes significant result differences
```

---

### Question 14: HAVING Clause Necessity

**Model Answer**:

```
HAVING purpose:
- Filter groups created by GROUP BY based on aggregate conditions
- Only works with grouped data

WHERE vs HAVING:

WHERE:
- Timing: Before GROUP BY
- Target: Individual rows
- Example: WHERE salary > 4000000

HAVING:
- Timing: After GROUP BY
- Target: Groups
- Example: HAVING AVG(salary) > 4500000
- Can use aggregate functions

Example query:
SELECT dept_id, AVG(salary)
FROM employees
WHERE salary > 4000000        -- Filter individual rows
GROUP BY dept_id
HAVING AVG(salary) > 4500000; -- Filter groups
```

---

### Question 15: Multiple Column Grouping and Optimization

**Model Answer**:

```
Cautions with GROUP BY multiple columns:
1. Group count increases exponentially
   GROUP BY col1: 5 groups
   GROUP BY col1, col2: 5 × 10 = 50 groups

2. Must include all grouped columns in GROUP BY
   MySQL 5.7+ requires all non-aggregated columns in GROUP BY

3. Specify sort order within groups
   col2 sorting within col1 groups

Performance optimization:
- Include only necessary columns in GROUP BY
- Use WHERE to filter rows before grouping
- Create indexes on GROUP BY columns
- Monitor column cardinality

Optimized query:
SELECT dept_id, position, COUNT(*) AS count
FROM employees
WHERE dept_id IS NOT NULL
GROUP BY dept_id, position
ORDER BY dept_id, COUNT(*) DESC;
```

---

## Practical Problems Model Answers (5 Questions)

### Question 16: Create sales Table

**Completion Criteria**:
✅ ch7_aggregation database created
✅ sales table with 6 columns created
✅ 8 sales records inserted
✅ products table with 4 records

---

### Question 17: Basic Aggregate Functions

**Expected Results**:

```
1. total_quantity: 73 (10+5+8+3+15+20+2+10)
2. avg_price: 143,750 (average unit price)
3. max_price: 500,000 (product 4)
```

---

### Question 18: GROUP BY with Aggregates

**Model Results**:

```
Query 1 - Quantity by product:
product_id | total_quantity
1          | 38
2          | 30
3          | 3
4          | 2

Query 2 - Average price by product:
product_id | avg_price
1          | 50,000
2          | 100,000
3          | 200,000
4          | 500,000

Query 3 - Sales count by product:
product_id | sales_count
1          | 3
2          | 3
3          | 1
4          | 1
```

---

### Question 19: HAVING Clause Filtering

**Model Answer**:

```sql
-- Query 1: Products with quantity >= 5
SELECT product_id, SUM(quantity) AS total_quantity
FROM sales
GROUP BY product_id
HAVING SUM(quantity) >= 5;

Expected:
product_id | total_quantity
1          | 38
2          | 30

-- Query 2: Average price >= 100000
SELECT product_id, AVG(unit_price) AS avg_price
FROM sales
GROUP BY product_id
HAVING AVG(unit_price) >= 100000;

Expected:
product_id | avg_price
2          | 100,000
3          | 200,000
4          | 500,000

-- Query 3: 2+ sales
SELECT product_id, COUNT(*) AS sales_count
FROM sales
GROUP BY product_id
HAVING COUNT(*) >= 2;

Expected:
product_id | sales_count
1          | 3
2          | 3
```

---

### Question 20: Complex Aggregate Queries

**Model Answer**:

```sql
-- 1. Category and product grouping
SELECT category, product_name, SUM(quantity), AVG(price)
FROM sales
GROUP BY category, product_name;

-- 2. Category sales overview
SELECT category, 
       SUM(quantity) AS total_qty,
       COUNT(*) AS sales_count,
       AVG(price) AS avg_price
FROM sales
GROUP BY category
ORDER BY total_qty DESC;

Result example:
category    | total_qty | sales_count | avg_price
Electronics | 73        | 8           | 212,500

-- 3. Top 3 categories by quantity
SELECT category, SUM(quantity) AS total_qty
FROM sales
GROUP BY category
ORDER BY total_qty DESC
LIMIT 3;

-- 4. Creative Query 1: COUNT(DISTINCT)
SELECT COUNT(DISTINCT product_id) AS product_count,
       COUNT(DISTINCT employee_id) AS employee_count
FROM sales;

-- 5. Creative Query 2: Total sales amount
SELECT product_id, SUM(quantity * unit_price) AS total_sales
FROM sales
GROUP BY product_id
ORDER BY total_sales DESC;
```

---

Prof. Jeong-Hyun Cho (peterchokr@gmail.com)
Yeungnam University College
