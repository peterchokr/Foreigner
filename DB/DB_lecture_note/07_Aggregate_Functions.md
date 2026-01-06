# Chapter 7: Aggregate Functions

---

## 📖 Course Overview

In this chapter, you will learn aggregate functions that condense multiple rows of a database into a single result value. This chapter covers basic aggregate functions such as COUNT, SUM, AVG, MAX, MIN, and advanced grouping techniques using GROUP BY and HAVING. These are essential techniques in practice for calculating various statistics such as sales volume, revenue, and average scores. 

| 이 장에서는 데이터베이스의 여러 행을 하나의 결과값으로 축약하는 집계함수(Aggregate Functions)를 학습합니다. COUNT, SUM, AVG, MAX, MIN 등 기본 집계함수부터 GROUP BY와 HAVING을 사용한 고급 그룹화 기법까지 다룹니다. 실무에서 판매량, 매출액, 평균 점수 등 다양한 통계정보를 계산하는 데 필수적인 기술입니다.

---

## 📚 Part 1: Theoretical Learning

### What You'll Learn in This Section

- Various uses of COUNT function
- SUM, AVG, MAX, MIN functions
- Grouping using GROUP BY
- Group filtering with HAVING clause
- NULL value handling
- Performance optimization of grouping

---

### 7.1 COUNT Function

The COUNT function returns the number of rows of a specified column. | COUNT 함수는 지정한 열의 행 개수를 반환합니다.

**Syntax:**

```sql
SELECT COUNT(*) FROM table_name;
SELECT COUNT(column) FROM table_name;
SELECT COUNT(DISTINCT column) FROM table_name;
```

**Characteristics:**

- COUNT(*): Returns count of all rows (includes NULL) | COUNT(*): 모든 행의 개수 반환 (NULL 포함)
- COUNT(column): Count of non-NULL values in that column | COUNT(column): 해당 열의 NULL이 아닌 값의 개수
- COUNT(DISTINCT column): Count of unique values after removing duplicates | COUNT(DISTINCT column): 중복을 제거한 서로 다른 값의 개수

**Example:**

```sql
SELECT COUNT(*) FROM employees;              -- Total employees
SELECT COUNT(manager_id) FROM employees;     -- Employees with supervisor
SELECT COUNT(DISTINCT dept_id) FROM employees; -- Number of departments
```

---

### 7.2 SUM Function

The SUM function returns the sum of a numeric column. | SUM 함수는 숫자 열의 합계를 반환합니다.

**Syntax:**

```sql
SELECT SUM(column) FROM table_name;
```

**Characteristics:**

- Only numeric data possible | 숫자 데이터만 가능
- NULL values are automatically excluded | NULL 값은 자동으로 제외
- Returns NULL if all values are NULL | 모든 값이 NULL이면 NULL 반환

**Example:**

```sql
SELECT SUM(salary) FROM employees;  -- Total salary
SELECT SUM(quantity) FROM orders;   -- Total order quantity
```

---

### 7.3 AVG Function

The AVG function returns the average of a numeric column. | AVG 함수는 숫자 열의 평균을 반환합니다.

**Syntax:**

```sql
SELECT AVG(column) FROM table_name;
```

**Characteristics:**

- NULL values are automatically excluded | NULL 값은 자동으로 제외
- Returns NULL if all values are NULL | 모든 값이 NULL이면 NULL 반환
- Pay attention to decimal places | 소수점 자리수에 주의

**Example:**

```sql
SELECT AVG(salary) FROM employees;   -- Average salary
SELECT ROUND(AVG(score), 2) FROM results; -- Average score (2 decimals)
```

---

### 7.4 MAX and MIN Functions

The MAX function returns the maximum value, and the MIN function returns the minimum value. | MAX 함수는 최대값, MIN 함수는 최소값을 반환합니다.

**Syntax:**

```sql
SELECT MAX(column) FROM table_name;
SELECT MIN(column) FROM table_name;
```

**Characteristics:**

- Usable with all data types: numbers, characters, dates, etc. | 숫자, 문자, 날짜 등 모든 데이터타입에 사용 가능
- NULL values are automatically excluded | NULL 값은 자동으로 제외
- Useful for finding most recent dates or largest character values | 가장 최근 날짜나 가장 큰 문자값 찾기에 유용

**Example:**

```sql
SELECT MAX(salary) FROM employees;        -- Highest salary
SELECT MIN(birth_date) FROM employees;    -- Oldest birth date
SELECT MAX(order_date) FROM orders;       -- Most recent order date
```

---

### 7.5 GROUP BY Clause

GROUP BY divides rows into groups and applies aggregate functions to each group. | GROUP BY는 행들을 그룹으로 나누어 각 그룹에 대해 집계함수를 적용합니다.

**Syntax:**

```sql
SELECT column, aggregate_function(column)
FROM table_name
GROUP BY column;
```

**Rules:**

- All ungrouped columns in SELECT must be included in GROUP BY | SELECT에 있는 그룹화되지 않은 열은 모두 GROUP BY에 포함되어야 함
- Columns in GROUP BY don't need to be in SELECT (MySQL 5.7 and earlier) | GROUP BY의 열은 SELECT에 없어도 됨 (MySQL 5.7 이전)

**Example:**

```sql
SELECT dept_id, AVG(salary)
FROM employees
GROUP BY dept_id;           -- Average salary by department

SELECT category, COUNT(*) AS count
FROM products
GROUP BY category;          -- Product count by category
```

---

### 7.6 GROUP BY Multiple Columns

Grouping by multiple columns allows for more detailed analysis. | 여러 열로 그룹화하여 더 세밀한 분석이 가능합니다.

**Example:**

```sql
SELECT dept_id, year, COUNT(*) AS count
FROM employees
GROUP BY dept_id, year;     -- Employee count by dept and year

SELECT category, color, SUM(quantity) AS total_qty
FROM inventory
GROUP BY category, color;   -- Stock quantity by category and color
```

---

### 7.7 HAVING Clause

HAVING acts like WHERE for GROUP BY results, applying conditions to groups. | HAVING은 GROUP BY 결과에 조건을 적용하는 WHERE 역할을 합니다.

**Key Differences:**

- WHERE: Filters rows before GROUP BY | WHERE: GROUP BY 전에 행을 필터링
- HAVING: Filters groups after GROUP BY | HAVING: GROUP BY 후에 그룹을 필터링

**Syntax:**

```sql
SELECT column, aggregate_function(column)
FROM table_name
GROUP BY column
HAVING aggregate_function(column) > value;
```

**Example:**

```sql
SELECT dept_id, AVG(salary) AS avg_salary
FROM employees
GROUP BY dept_id
HAVING AVG(salary) > 4000000;  -- Departments with average salary > 4 million

SELECT category, COUNT(*) AS product_count
FROM products
GROUP BY category
HAVING COUNT(*) >= 5;  -- Categories with 5 or more products
```

---

### 7.8 Aggregate Functions and NULL Handling

NULL values are handled specially in aggregate functions. | NULL 값은 집계함수에서 특별하게 처리됩니다.

**Characteristics:**

- COUNT(*) Exception: NULL values included in count | COUNT(*) 제외: NULL 값도 개수에 포함
- COUNT(column): NULL values excluded | COUNT(column): NULL 값 제외
- SUM, AVG, MAX, MIN: All exclude NULL | SUM, AVG, MAX, MIN: 모두 NULL 제외

**NULL Conversion:**

```sql
SELECT SUM(commission) FROM employees;      -- NULLs excluded
SELECT SUM(IFNULL(commission, 0)) FROM employees;  -- NULLs converted to 0
```

---

### 7.9 WITH ROLLUP

WITH ROLLUP displays subtotals for each group and a grand total together. | WITH ROLLUP은 그룹별 소계와 총계를 함께 표시합니다.

**Syntax:**

```sql
SELECT column, aggregate_function(column)
FROM table_name
GROUP BY column WITH ROLLUP;
```

**Example:**

```sql
SELECT dept_id, SUM(salary)
FROM employees
GROUP BY dept_id WITH ROLLUP;  -- Displays by dept and grand total
```

---

## 📚 Part 2: Sample Data

### sales Table

```sql
CREATE TABLE sales (
    sale_id INT PRIMARY KEY AUTO_INCREMENT,
    product_id INT,
    quantity INT,
    unit_price DECIMAL(10, 2),
    sale_date DATE,
    employee_id INT
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
```

### products Table

```sql
CREATE TABLE products (
    product_id INT PRIMARY KEY AUTO_INCREMENT,
    product_name VARCHAR(50),
    category VARCHAR(30),
    price DECIMAL(10, 2),
    stock INT
);

INSERT INTO products VALUES
(1, 'Notebook A', 'Electronics', 50000, 100),
(2, 'Mouse B', 'Electronics', 100000, 200),
(3, 'Monitor C', 'Electronics', 200000, 50),
(4, 'Keyboard D', 'Electronics', 500000, 30);
```

---

## 💻 Part 3: Practice

### What You'll Learn in This Section

- Proper use of aggregate functions | 집계함수의 올바른 사용법
- Combination of GROUP BY and HAVING | GROUP BY와 HAVING의 조합
- Common aggregation patterns in practice | 실무에서 자주 사용되는 집계 패턴
- NULL handling and performance optimization | NULL 처리 및 성능 최적화

---

### 7-1. COUNT Function Basics

Query the total number of sales transactions. | 전체 판매 건수를 조회하세요.

```sql
SELECT COUNT(*) AS total_sales FROM sales;
```

---

### 7-2. COUNT with DISTINCT

Query the number of different products sold. | 판매된 서로 다른 상품의 개수를 조회하세요.

```sql
SELECT COUNT(DISTINCT product_id) AS distinct_products FROM sales;
```

---

### 7-3. COUNT with NULL Values

Query the count of non-NULL product_id. | NULL이 아닌 product_id의 개수를 조회하세요.

```sql
SELECT COUNT(product_id) AS non_null_count FROM sales;
```

---

### 7-4. SUM Function

Query the total sales quantity. | 전체 판매량의 합계를 조회하세요.

```sql
SELECT SUM(quantity) AS total_quantity FROM sales;
```

---

### 7-5. SUM with Conditions

Query the total sales amount after a specific date. | 특정 날짜 이후의 판매액 합계를 조회하세요.

```sql
SELECT SUM(quantity * unit_price) AS total_sales_amount 
FROM sales 
WHERE sale_date >= '2024-01-16';
```

---

### 7-6. AVG Function

Query the average sales quantity. | 평균 판매 수량을 조회하세요.

```sql
SELECT AVG(quantity) AS avg_quantity FROM sales;
```

---

### 7-7. AVG with ROUND

Query the average sales amount rounded to 2 decimal places. | 평균 판매액을 소수 2자리까지 반올림하여 조회하세요.

```sql
SELECT ROUND(AVG(unit_price), 2) AS avg_price FROM sales;
```

---

### 7-8. MAX Function

Query the highest unit price. | 가장 높은 단가를 조회하세요.

```sql
SELECT MAX(unit_price) AS max_price FROM sales;
```

---

### 7-9. MIN Function

Query the lowest unit price. | 가장 낮은 단가를 조회하세요.

```sql
SELECT MIN(unit_price) AS min_price FROM sales;
```

---

### 7-10. MAX and MIN Combined

Query the price range (max - min). | 단가의 범위(최대-최소)를 조회하세요.

```sql
SELECT MAX(unit_price) - MIN(unit_price) AS price_range FROM sales;
```

---

### 7-11. GROUP BY Basics

Query sales quantity by product. | 상품별 판매 수량을 조회하세요.

```sql
SELECT product_id, SUM(quantity) AS total_qty
FROM sales
GROUP BY product_id;
```

---

### 7-12. GROUP BY with COUNT

Query number of sales by product. | 상품별 판매 건수를 조회하세요.

```sql
SELECT product_id, COUNT(*) AS sales_count
FROM sales
GROUP BY product_id;
```

---

### 7-13. GROUP BY with SUM

Query total sales amount by product. | 상품별 총 판매액을 조회하세요.

```sql
SELECT product_id, SUM(quantity * unit_price) AS total_sales
FROM sales
GROUP BY product_id;
```

---

### 7-14. GROUP BY with AVG

Query average sales amount by product. | 상품별 평균 판매액을 조회하세요.

```sql
SELECT product_id, AVG(unit_price) AS avg_price
FROM sales
GROUP BY product_id;
```

---

### 7-15. GROUP BY Multiple Columns

Query sales amount by employee and date. | 직원별, 날짜별 판매액을 조회하세요.

```sql
SELECT employee_id, DATE(sale_date) AS sale_date, SUM(quantity * unit_price) AS daily_sales
FROM sales
GROUP BY employee_id, DATE(sale_date);
```

---

### 7-16. GROUP BY with ORDER BY

Query sales quantity by product in descending order. | 상품별 판매량을 많은 순서대로 조회하세요.

```sql
SELECT product_id, SUM(quantity) AS total_qty
FROM sales
GROUP BY product_id
ORDER BY total_qty DESC;
```

---

### 7-17. Group Filtering with HAVING

Query products with 3 or more sales transactions. | 판매 건수가 3건 이상인 상품을 조회하세요.

```sql
SELECT product_id, COUNT(*) AS sales_count
FROM sales
GROUP BY product_id
HAVING COUNT(*) >= 3;
```

---

### 7-18. HAVING with Aggregate Condition

Query products with total sales of 500000 or more. | 판매액 합계가 500000 이상인 상품을 조회하세요.

```sql
SELECT product_id, SUM(quantity * unit_price) AS total_sales
FROM sales
GROUP BY product_id
HAVING SUM(quantity * unit_price) >= 500000;
```

---

### 7-19. WHERE and HAVING Combined

Query products after 2024-01-01 with total sales of 600000 or more. | 2024년 이후 판매 데이터에서 판매액 합계가 600000 이상인 상품을 조회하세요.

```sql
SELECT product_id, SUM(quantity * unit_price) AS total_sales
FROM sales
WHERE sale_date >= '2024-01-01'
GROUP BY product_id
HAVING SUM(quantity * unit_price) >= 600000;
```

---

### 7-20. GROUP BY with LIMIT

Query top 3 products with highest sales amount. | 판매액이 가장 높은 상위 3개 상품을 조회하세요.

```sql
SELECT product_id, SUM(quantity * unit_price) AS total_sales
FROM sales
GROUP BY product_id
ORDER BY total_sales DESC
LIMIT 3;
```

---

### 7-21. Statistics by Category

Query product count, average price, and maximum price by category. | 카테고리별 상품 개수, 평균 가격, 최대 가격을 조회하세요.

```sql
SELECT category, COUNT(*) AS product_count, 
       AVG(price) AS avg_price, MAX(price) AS max_price
FROM products
GROUP BY category;
```

---

### 7-22. Sales Performance by Employee

Query total sales, sales count, and average sales by employee. | 직원별 총 판매액, 판매 건수, 평균 판매액을 조회하세요.

```sql
SELECT employee_id, 
       SUM(quantity * unit_price) AS total_sales,
       COUNT(*) AS sales_count,
       AVG(quantity * unit_price) AS avg_sales
FROM sales
GROUP BY employee_id;
```

---

### 7-23. Aggregation by Date

Query sales amount, transaction count, and average price by date. | 날짜별 판매액, 판매 건수, 평균 단가를 조회하세요.

```sql
SELECT DATE(sale_date) AS sale_date,
       SUM(quantity * unit_price) AS daily_sales,
       COUNT(*) AS transaction_count,
       AVG(unit_price) AS avg_unit_price
FROM sales
GROUP BY DATE(sale_date);
```

---

### 7-24. Complex Aggregation

Query sales quantity, total sales, and average price by category and product. | 카테고리, 상품별로 판매량, 판매액, 평균 가격을 조회하세요.

```sql
SELECT p.category, p.product_id, p.product_name,
       SUM(s.quantity) AS total_qty,
       SUM(s.quantity * s.unit_price) AS total_sales,
       AVG(s.unit_price) AS avg_unit_price
FROM products p
JOIN sales s ON p.product_id = s.product_id
GROUP BY p.category, p.product_id, p.product_name;
```

---

### 7-25. NULL Handling

Calculate the sum after handling NULL values. | NULL 값을 처리한 후 합계를 계산하세요.

```sql
SELECT SUM(IFNULL(stock, 0)) AS total_stock FROM products;
```

---

### 7-26. CASE with Aggregate Functions

Classify sales amount into categories and query count by category. | 판매액을 범주로 분류하여 범주별 개수를 조회하세요.

```sql
SELECT CASE 
           WHEN quantity * unit_price >= 500000 THEN 'Large'
           WHEN quantity * unit_price >= 300000 THEN 'Medium'
           ELSE 'Small'
       END AS sales_category,
       COUNT(*) AS count
FROM sales
GROUP BY sales_category;
```

---

### 7-27. GROUP_CONCAT for Data Consolidation

Display the selling employees for each product separated by commas. | 상품별 판매 직원들을 쉼표로 구분하여 표시하세요.

```sql
SELECT p.product_id, p.product_name,
       GROUP_CONCAT(DISTINCT s.employee_id SEPARATOR ', ') AS employee_ids
FROM products p
JOIN sales s ON p.product_id = s.product_id
GROUP BY p.product_id, p.product_name;
```

---

### 7-28. Nested Aggregate Functions

Query the count of products with average sales higher than overall average. | 전체 평균 판매액보다 높은 상품의 개수를 조회하세요.

```sql
SELECT COUNT(*) AS products_above_avg
FROM (
    SELECT product_id, AVG(quantity * unit_price) AS avg_sales
    FROM sales
    GROUP BY product_id
    HAVING AVG(quantity * unit_price) > (SELECT AVG(quantity * unit_price) FROM sales)
) AS subquery;
```

---

### 7-29. Subtotal with Rollup

Display category subtotals and grand total together. | 카테고리별 소계와 전체 합계를 함께 표시하세요.

```sql
SELECT category, COUNT(*) AS product_count
FROM products
GROUP BY category
WITH ROLLUP;
```

---

### 7-30. Hierarchical Aggregation

Perform hierarchical aggregation by employee and date. | 직원별, 날짜별로 계층적으로 집계하세요.

```sql
SELECT employee_id, DATE(sale_date) AS sale_date,
       SUM(quantity * unit_price) AS total_sales
FROM sales
GROUP BY employee_id, DATE(sale_date)
WITH ROLLUP;
```

---

### 7-31. Correlated Subquery with Aggregation

Compare each sales amount with the average sales of the same product. | 각 판매액이 같은 상품의 평균 판매액과 비교하세요.

```sql
SELECT sale_id, product_id, quantity * unit_price AS sales_amount,
       (SELECT AVG(quantity * unit_price) FROM sales s2 
        WHERE s2.product_id = s1.product_id) AS avg_product_sales
FROM sales s1;
```

---

### 7-32. Cumulative Aggregation

Calculate cumulative sum sorted by sales amount in ascending order. | 판매액을 오름차순으로 정렬하여 누적 합계를 계산하세요.

```sql
SELECT sale_date, quantity * unit_price AS sales_amount,
       SUM(quantity * unit_price) OVER (ORDER BY sale_date) AS cumulative_sales
FROM sales
ORDER BY sale_date;
```

---

### 7-33. Percentage Calculation

Calculate what percentage each product's sales are of total sales. | 각 상품의 판매액이 전체 판매액의 몇 %인지 계산하세요.

```sql
SELECT product_id,
       SUM(quantity * unit_price) AS product_sales,
       ROUND(SUM(quantity * unit_price) / (SELECT SUM(quantity * unit_price) FROM sales) * 100, 2) AS percentage
FROM sales
GROUP BY product_id;
```

---

### 7-34. Quartile Aggregation

Divide sales amount into quartiles and query count by quartile. | 판매액을 4분위수로 나누어 등급별 개수를 조회하세요.

```sql
SELECT NTILE(4) OVER (ORDER BY quantity * unit_price) AS quartile,
       COUNT(*) AS count
FROM sales
GROUP BY NTILE(4) OVER (ORDER BY quantity * unit_price);
```

---

### 7-35. Change Rate Calculation

Calculate day-over-day change rate in sales amount. | 날짜별 판매액의 전일 대비 변화율을 계산하세요.

```sql
SELECT sale_date, SUM(quantity * unit_price) AS daily_sales,
       LAG(SUM(quantity * unit_price)) OVER (ORDER BY sale_date) AS prev_day_sales
FROM sales
GROUP BY sale_date;
```

---

### 7-36. Ranking

Rank products by sales amount in descending order. | 판매액이 높은 순서대로 순위를 매기세요.

```sql
SELECT product_id, SUM(quantity * unit_price) AS total_sales,
       ROW_NUMBER() OVER (ORDER BY SUM(quantity * unit_price) DESC) AS rank
FROM sales
GROUP BY product_id;
```

---

### 7-37. Period Comparison

Compare sales amount for specific periods. | 특정 기간의 판매액을 비교하세요.

```sql
SELECT 
    CASE WHEN sale_date < '2024-01-16' THEN 'Period1' ELSE 'Period2' END AS period,
    SUM(quantity * unit_price) AS period_sales,
    COUNT(*) AS transaction_count
FROM sales
GROUP BY period;
```

---

### 7-38. Outlier Detection

Query sales amount with large deviation from average. | 평균에서 큰 편차가 있는 판매액을 조회하세요.

```sql
SELECT sale_id, product_id, quantity * unit_price AS sales_amount,
       AVG(quantity * unit_price) OVER (PARTITION BY product_id) AS avg_sales,
       STDDEV(quantity * unit_price) OVER (PARTITION BY product_id) AS stddev_sales
FROM sales;
```

---

### 7-39. Ranking within Groups

Query sales ranking within each product. | 각 상품 내에서 판매 순위를 조회하세요.

```sql
SELECT product_id, sale_date, quantity * unit_price AS sales_amount,
       ROW_NUMBER() OVER (PARTITION BY product_id ORDER BY quantity * unit_price DESC) AS product_rank
FROM sales;
```

---

### 7-40. Statistical Functions

Query average, variance, and standard deviation of sales amount. | 판매액의 평균, 분산, 표준편차를 조회하세요.

```sql
SELECT 
    AVG(quantity * unit_price) AS avg_sales,
    VARIANCE(quantity * unit_price) AS variance,
    STDDEV(quantity * unit_price) AS stddev
FROM sales;
```

---

---

## 📝 Part 4: Assignment Instructions

### Theoretical Assignments

**Assignment 1**: Explain the characteristics of COUNT, SUM, AVG, MAX, and MIN functions respectively. Compare and analyze their behavior when NULL values are included. Present real-world situations where each function is needed with examples. | COUNT, SUM, AVG, MAX, MIN 함수의 특징을 각각 설명하고, NULL 값이 포함되었을 때의 동작 방식을 비교 분석하세요. 실무에서 각 함수가 필요한 상황을 사례와 함께 제시하세요.

**Assignment 2**: Explain the differences between GROUP BY, WHERE, and HAVING. Clearly describe the execution order and role of each when using WHERE and HAVING together. | GROUP BY와 WHERE, HAVING의 차이점을 설명하세요. WHERE와 HAVING을 함께 사용할 때의 실행 순서와 각각의 역할을 명확히 서술하세요.

**Assignment 3**: Explain cautions when using GROUP BY with multiple columns. Discuss problems and solutions when ungrouped columns are included in SELECT clause. | 여러 열로 GROUP BY할 때의 주의사항을 설명하세요. SELECT 절에 그룹화되지 않은 열이 포함될 경우의 문제점과 해결 방법을 논의하세요.

**Assignment 4**: Present methods for performance optimization of queries containing aggregate functions. Explain techniques such as index utilization, timing of aggregation, and partial aggregation. | 집계함수가 포함된 쿼리의 성능 최적화 방법을 제시하세요. 인덱스 활용, 집계 시점 조절, 부분 집계 등의 기법을 설명하세요.

**Assignment 5**: Explain advanced aggregation techniques using WITH ROLLUP, window functions, and subqueries. Compare advantages and disadvantages of each technique and present use cases. | WITH ROLLUP, 윈도우 함수, 서브쿼리를 사용한 고급 집계 기법을 설명하세요. 각 기법의 장단점을 비교하고 활용 사례를 제시하세요.

Submission Format: Word or PDF document (2-3 pages)

### Practical Assignments

**Assignment 1**: Write the following aggregate queries on sales data: Total sales transactions, total sales amount, average sales amount; Sales count by product, total sales amount by product, average sales amount by product; Sales performance by employee (sales amount, count, average); Top 5 products sorted by highest sales amount. | 판매 데이터에서 다음의 집계 쿼리를 작성하세요: 전체 판매 건수, 판매액 합계, 평균 판매액; 상품별 판매 건수, 판매액 합계, 평균 판매액; 직원별 판매 실적 (판매액, 건수, 평균); 판매액이 높은 순서대로 상위 5개 상품.

**Assignment 2**: Query the following using GROUP BY and HAVING: Products with 3 or more sales transactions; Employees with total sales amount of 500000 or higher; Products with average sales amount higher than overall average. | GROUP BY와 HAVING을 사용하여 다음을 조회하세요: 판매 건수가 3건 이상인 상품; 판매액 합계가 500000 이상인 직원; 평균 판매액이 전체 평균보다 높은 상품.

**Assignment 3**: Perform complex aggregation including NULL handling, CASE statements, and format conversion: Calculate total compensation by employee including commission; Classify sales amount into categories (large/medium/small) and show statistics by category; Quarterly sales performance analysis. | NULL 처리, CASE 문, 형식 변환을 포함한 복합 집계를 수행하세요: commission 포함 직원별 총 보상액 계산; 판매액을 범주(대/중/소)로 분류하여 범주별 통계; 분기별 판매 성과 분석.

**Assignment 4**: Perform hierarchical aggregation using WITH ROLLUP: Category and product sales subtotals with grand total; Region and branch sales performance shown hierarchically; Year and month cumulative sales. | WITH ROLLUP을 사용하여 계층적 집계를 수행하세요: 카테고리별, 상품별 판매액 소계 및 전체 합계; 지역별, 지점별 판매 실적 계층적 표시; 연도별, 월별 누적 판매액.

**Assignment 5**: Execute all provided practice queries and attach screenshots of each result. Additionally, write 5 or more creative aggregate queries and present their results. Explain how each query can help in business decision-making. | 제공된 모든 쿼리를 직접 실행하고, 각 쿼리의 결과를 스크린샷으로 첨부하세요. 추가로 5개 이상의 창의적인 집계 쿼리를 작성하여 그 결과를 제시하고, 각 쿼리가 어떤 비즈니스 의사결정에 도움이 될 수 있는지 설명하세요.

Submission Format: SQL file (Ch7_Aggregate_Functions_[StudentID].sql) and result screenshots

---

Thank you for your hard work.

Prof. Cho Jeong-Hyun (peterchokr@gmail.com). Yeungnam University College
