# Chapter 3: SELECT Basics and Single Table Queries

---

## 📋 Course Overview

**Course Topic**: Basic Data Query Using SELECT Statement | SELECT 문을 이용한 기본 데이터 조회

**Course Objectives**

- Understand the basic structure and execution order of SELECT statements | SELECT 문의 기본 구조와 실행 순서 이해
- Master basic features such as column selection, aliases, and sorting | 열 선택, 별칭, 정렬 등 기본 기능 숙달
- Develop practical data query skills | 실무 중심의 데이터 조회 능력 배양

---

## 📚 Part 1: Theoretical Learning

### What You'll Learn in This Section

In this section, you will learn the SELECT statement, which is the foundation of SQL. You will understand the execution order of SELECT statements and the role of each clause, and learn various features such as column selection, duplicate removal, alias designation, sorting, and row limiting. Through this, you will develop the ability to effectively query desired data from a database.

| 이 섹션에서는 SQL의 가장 기본이 되는 SELECT 문을 배웁니다. SELECT 문의 실행 순서와 각 절의 역할을 이해하고, 열 선택, 중복 제거, 별칭 지정, 정렬, 행 제한 등 다양한 기능을 학습합니다. 이를 통해 데이터베이스에서 원하는 데이터를 효과적으로 조회하는 능력을 기르게 됩니다.

### 1-1. Basic Structure of SELECT Statement

#### **Basic Form**

```sql
SELECT column_name1, column_name2, ...
FROM table_name;
```

#### **Execution Order**

```
1. FROM: Which table?
2. SELECT: Which columns?
3. WHERE: Which conditions? (if any)
4. GROUP BY: How to group? (if any)
5. HAVING: What conditions for groups? (if any)
6. ORDER BY: In what order? (if any)
7. LIMIT: How many? (if any)
```

### 1-2. Column Selection

```
* Usage: Query all columns
SELECT * FROM student;

Specific columns only: Query only needed columns
SELECT student_id, name FROM student;

Multiple columns: Query in desired order
SELECT name, student_id, gpa FROM student;
```

### 1-3. Column Alias

```sql
-- Using AS
SELECT student_id AS student_number, name AS student_name FROM student;

-- AS omitted
SELECT student_id student_number, name student_name FROM student;

-- With spaces, use quotes
SELECT student_id AS 'Student ID' FROM student;
```

### 1-4. DISTINCT (Remove Duplicates)

```sql
-- Remove duplicate department information
SELECT DISTINCT department FROM student;

-- Remove duplicates from multiple columns
SELECT DISTINCT department, gpa FROM student;
```

### 1-5. LIMIT (Limit Row Count)

```sql
-- Query only top 5
SELECT * FROM student LIMIT 5;

-- Query 10 from 6th row
SELECT * FROM student LIMIT 10 OFFSET 5;

-- Pagination: 10 per page
-- Page 1
SELECT * FROM student LIMIT 10;
-- Page 2
SELECT * FROM student LIMIT 10 OFFSET 10;
```

### 1-6. ORDER BY (Sorting)

```sql
-- Ascending order (ASC)
SELECT * FROM student ORDER BY gpa ASC;

-- Descending order (DESC)
SELECT * FROM student ORDER BY gpa DESC;

-- Sort by multiple columns: department ascending, GPA descending
SELECT * FROM student 
ORDER BY department ASC, gpa DESC;

-- NULL handling
SELECT * FROM student 
ORDER BY phone IS NULL, phone;
```

---

## 📚 Part 2: Sample Data Setup

### What You'll Learn in This Section

In this section, you will create movie and product tables for SELECT practice. By practicing with two tables with various attributes, you will learn how to use SELECT statements in various situations.

| 이 섹션에서는 SELECT 실습에 사용할 movie와 product 테이블을 생성합니다. 다양한 속성을 가진 두 개의 테이블에서 실습함으로써 여러 상황에 대응하는 SELECT 문 활용법을 배웁니다.

```sql
CREATE DATABASE ch3_practice CHARACTER SET utf8mb4;
USE ch3_practice;

-- Movie table
CREATE TABLE movie (
    movie_id INT PRIMARY KEY AUTO_INCREMENT,
    title VARCHAR(50) NOT NULL,
    genre VARCHAR(20),
    release_year INT,
    release_date DATE,
    director VARCHAR(30),
    rating DECIMAL(3, 1),
    runtime INT,
    country VARCHAR(20)
) CHARACTER SET utf8mb4;

INSERT INTO movie VALUES
(1, 'Shopping Mall King', 'Drama', 2023, '2023-01-15', 'Director Kim', 8.5, 120, 'Korea'),
(2, 'Art of Coding', 'Action', 2023, '2023-02-20', 'Director Lee', 7.8, 135, 'Korea'),
(3, 'Data World', 'Fantasy', 2022, '2022-12-10', 'Director Park', 8.2, 145, 'USA'),
(4, 'Romantic SQL', 'Romance', 2023, '2023-03-05', 'Director Choi', 7.5, 110, 'Korea'),
(5, 'Animation Server', 'Animation', 2023, '2023-04-01', 'Director Jung', 8.0, 90, 'USA');

-- Product table
CREATE TABLE product (
    product_id INT PRIMARY KEY AUTO_INCREMENT,
    product_name VARCHAR(50) NOT NULL,
    category VARCHAR(20),
    price INT,
    stock INT,
    upload_date DATE,
    brand VARCHAR(20),
    rating DECIMAL(3, 2)
) CHARACTER SET utf8mb4;

INSERT INTO product VALUES
(1, 'Wireless Mouse', 'Electronics', 45000, 150, '2024-01-10', 'Logitech', 4.50),
(2, 'Mechanical Keyboard', 'Electronics', 120000, 80, '2024-01-12', 'Ducky', 4.60),
(3, 'Monitor Arm', 'Electronics', 65000, 200, '2024-01-08', 'Elgo', 4.30),
(4, 'Desk Lamp', 'Living', 35000, 300, '2024-01-05', 'Philips', 4.40),
(5, 'Chair Cushion', 'Furniture', 28000, 250, '2024-01-15', 'Ikea', 4.20);
```

---

## 💻 Part 3: Practice

### What You'll Learn in This Section

In this section, you will actually execute all the features of the SELECT statement you have learned to query data. Starting from basic SELECT, you will progressively practice various features such as alias assignment, duplicate removal, sorting, and row limiting. Through each practice, you will be able to use SELECT statements freely.

| 이 섹션에서는 배운 SELECT 문의 모든 기능을 실제로 실행하여 데이터를 조회합니다. 기본적인 SELECT부터 시작하여 별칭 지정, 중복 제거, 정렬, 행 제한 등 다양한 기능을 단계적으로 실습합니다. 각 실습을 통해 SELECT 문을 자유롭게 사용할 수 있게 됩니다.

### 3-1. SELECT Basics

**Practice 3-1~3-10: Basic SELECT Statements**

```sql
1. Query all movies
SELECT * FROM movie;

2. Query only movie titles
SELECT title FROM movie;

3. Query movie titles and directors
SELECT title, director FROM movie;

4. Query all products
SELECT * FROM product;

5. Query only product names and prices
SELECT product_name, price FROM product;

6. Remove duplicate categories in products
SELECT DISTINCT category FROM product;

7. Remove duplicate movie genres
SELECT DISTINCT genre FROM movie;

8. Query top 3 products
SELECT * FROM product LIMIT 3;

9. Query 2 products starting from the 3rd product
SELECT * FROM product LIMIT 2 OFFSET 2;

10. Query movie titles and ratings
SELECT title, rating FROM movie;
```

### 3-2. Column Alias and Sorting

**Practice 3-11~3-20: Alias and Sorting**

```sql
11. Alias movie title as 'Movie Title' and director as 'Director Name'
SELECT title AS 'Movie Title', director AS 'Director Name' FROM movie;

12. Alias product name as 'Product' and price as 'Sale Price'
SELECT product_name AS Product, price AS 'Sale Price' FROM product;

13. Alias with spaces
SELECT product_name AS 'Product Name', price AS 'Sale Price' FROM product;

14. Calculate alias including price
SELECT product_name, price, price * 1.1 AS '10% Increased Price' FROM product;

15. Sort movies by rating descending
SELECT title, rating FROM movie ORDER BY rating DESC;

16. Sort products by price ascending
SELECT product_name, price FROM product ORDER BY price ASC;

17. Sort products by highest stock
SELECT product_name, stock FROM product ORDER BY stock DESC;

18. Sort movies by release year ascending, rating descending
SELECT title, release_year, rating FROM movie 
ORDER BY release_year ASC, rating DESC;

19. Sort products by category, then by price
SELECT product_name, category, price FROM product 
ORDER BY category ASC, price DESC;

20. Query top 3 highest rated movies
SELECT title, rating FROM movie 
ORDER BY rating DESC LIMIT 3;
```

### 3-3. Advanced Practice

**Practice 3-21~3-33: Complex Queries**

```sql
21. Query product name and '20% discount price'
SELECT product_name, price * 0.8 AS 'Discount Price' FROM product;

22. Query all movie information sorted by rating descending
SELECT * FROM movie ORDER BY rating DESC;

23. Query all categories without duplicate categories
SELECT DISTINCT category FROM product;

24. Query only top 5 products
SELECT * FROM product LIMIT 5;

25. Query movie title and rating sorted by highest rating
SELECT title AS 'Movie Title', rating AS Rating 
FROM movie ORDER BY rating DESC;

26. Query 5 products starting from the 3rd product
SELECT product_name, price FROM product LIMIT 5 OFFSET 2;

27. Query movie title, director, release year (sorted by year)
SELECT title, director, release_year 
FROM movie ORDER BY release_year;

28. Sort products by highest stock, then by highest price if equal
SELECT product_name, stock, price 
FROM product ORDER BY stock DESC, price DESC;

29. Query movie title, rating, director sorted by rating
SELECT title, rating, director FROM movie ORDER BY rating DESC;

30. Query product name, price, category sorted by category
SELECT product_name, price, category FROM product ORDER BY category;

31. Query only 2023 movies (title and director only)
SELECT title, director FROM movie WHERE release_year = 2023;

32. Query only products with price 50000 or higher
SELECT * FROM product WHERE price >= 50000;

33. Query top 5 highest rated movies
SELECT title, rating FROM movie ORDER BY rating DESC LIMIT 5;
```

---

## 📝 Part 4: Assignment Instructions

### Theoretical Assignments

**Assignment 1**: Explain in detail the execution order of SELECT statements and the processing at each step. With specific SQL examples, describe the entire process from retrieving data in the FROM clause to sorting with ORDER BY. | SELECT 문의 실행 순서와 각 단계에서의 처리 과정을 상세히 설명하세요. 구체적인 SQL 예시를 들어 FROM 절에서 데이터를 가져오는 과정부터 ORDER BY로 정렬되는 전체 과정을 서술하세요.

**Assignment 2**: Explain the role and usage of DISTINCT keyword and specifically describe situations in practice where duplicate values need to be removed. | DISTINCT 키워드의 역할과 사용 방법을 설명하고, 실무에서 중복된 값을 제거해야 하는 상황을 구체적으로 예시하여 설명하세요.

**Assignment 3**: Explain how priority is determined when sorting by multiple columns using ORDER BY clause and clearly distinguish the concepts of ASC and DESC. | ORDER BY 절을 사용하여 복수의 열로 정렬할 때 우선순위가 어떻게 결정되는지 설명하고, ASC와 DESC의 개념을 명확히 구분하여 서술하세요.

**Assignment 4**: Explain how to implement pagination using LIMIT and OFFSET keywords and provide an example of querying 3 pages with 10 items per page. | LIMIT과 OFFSET 키워드를 사용하여 페이지네이션을 구현하는 방식을 설명하고, 한 페이지에 10개씩 3페이지를 조회하는 예시를 제시하세요.

**Assignment 5**: Explain the concept and purpose of column alias and present principles and cautions for writing readable aliases in practical work. | 열 별칭의 개념과 사용 목적을 설명하고, 실무에서 가독성 좋은 별칭을 작성하기 위한 원칙과 주의사항을 제시하세요.

Submission Format: Word or PDF document (1-2 pages)

---

### Practical Assignments

**Assignment 1**: Query all movie titles, directors, and ratings from the movie table, sorting by highest rating first and present the results. | movie 테이블에서 모든 영화의 제목, 감독, 평점을 조회하되, 평점이 높은 순서대로 정렬하여 결과를 제시하세요.

**Assignment 2**: Query product name, price, and 10% increased price from the product table with alias. Results should be sorted by original price ascending with only top 5 queried. | product 테이블에서 상품명, 가격, 그리고 10% 인상가를 별칭으로 표시하여 조회하세요. 결과는 원래 가격의 오름차순으로 정렬되어야 하며, 상위 5개만 조회하세요.

**Assignment 3**: Query all types of movie genres from the movie table without duplication. Use DISTINCT keyword to identify how many different genres exist and list what each genre is. | movie 테이블에서 영화 장르의 모든 종류를 중복 없이 조회하세요. DISTINCT 키워드를 사용하여 서로 다른 장르가 몇 개인지 확인하고, 각 장르가 무엇인지 제시하세요.

**Assignment 4**: Query product name, price, and rating of electronics category from the product table, sorted by highest rating. Write SQL statement filtering electronics only using WHERE clause and sorting with ORDER BY. | product 테이블에서 전자제품 카테고리의 상품명, 가격, 평점을 조회하되, 평점이 높은 순서대로 정렬하세요. WHERE 절을 사용하여 전자제품만 필터링하고, ORDER BY로 정렬하는 과정을 SQL 문으로 작성하세요.

**Assignment 5**: Execute all queries provided from Practice 3-1 to 3-33 in Part 3 and attach screenshots of each query result. Additionally, write 5 or more creative queries and present their results as well. | Part 3의 실습 3-1부터 3-33까지 제공된 모든 쿼리를 직접 실행하고, 각 쿼리의 결과를 스크린샷으로 첨부하세요. 추가로 5개 이상의 창의적인 쿼리를 작성하여 그 결과도 함께 제시하세요.

Submission Format: SQL file (Ch3_SELECT_Practice_[StudentID].sql) and result screenshots

---

Thank you for your hard work.

Prof. Cho Jeong-Hyun (peterchokr@gmail.com). Yeungnam University College
