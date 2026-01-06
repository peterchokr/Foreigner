# Ch3 SELECT Basics - Exercise Problems

Dear students! After completing Chapter 3, please solve the following exercises to verify your understanding.
Check the difficulty level of each section and study step by step.

---

## 📌 Learning Objectives Verification

After completing Chapter 3, you should understand:

- Understanding basic structure of SELECT command
- SELECT execution order (FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT)
- Remove duplicates with DISTINCT
- Sort with ORDER BY
- Pagination with LIMIT/OFFSET
- Use of column aliases

---

# Multiple Choice Questions (10 Questions)

## Beginner Level (5 Questions) - Basic Concept Verification

**Question 1** Where is the WHERE clause executed in SELECT execution order?

- ① Before FROM
- ② After FROM, before GROUP BY
- ③ After SELECT
- ④ After ORDER BY

---

**Question 2** What is the role of DISTINCT?

- ① Sort rows
- ② Remove duplicate rows and display only unique rows
- ③ Select only rows matching conditions
- ④ Count the number of rows

---

**Question 3** What is the purpose of column alias in the following SQL?

```sql
SELECT student_id AS 'StudentID', student_name AS 'StudentName'
FROM student;
```

- ① Change table name
- ② Display column names in query results more understandably
- ③ Change data type
- ④ Improve WHERE clause performance

---

**Question 4** What is the default sort order when using ORDER BY?

- ① Random
- ② Descending (DESC)
- ③ Ascending (ASC)
- ④ Input order

---

**Question 5** What does LIMIT 10 OFFSET 20 mean?

- ① Retrieve first 10 rows
- ② Retrieve first 20 rows
- ③ Retrieve 10 rows from 21st to 30th
- ④ Infinitely retrieve 10 rows from 20th

---

## Intermediate Level (3 Questions) - Concept Application

**Question 6** Why is the WHERE clause executed before the GROUP BY clause in the following query?

```sql
SELECT department, COUNT(*)
FROM employee
WHERE salary > 5000000
GROUP BY department;
```

- ① WHERE is always executed first
- ② Filter only necessary data with WHERE first, then apply GROUP BY (efficiency)
- ③ Group by department with GROUP BY first, then filter with WHERE
- ④ Order doesn't matter

---

**Question 7** To retrieve only the top 5 movies released after 2023 sorted by high ratings from movie table?

```sql
① SELECT * FROM movie
   WHERE release_year >= 2023
   ORDER BY rating DESC
   LIMIT 5;

② SELECT * FROM movie
   LIMIT 5
   WHERE release_year >= 2023
   ORDER BY rating DESC;

③ SELECT * FROM movie
   ORDER BY rating DESC
   WHERE release_year >= 2023
   LIMIT 5;
```

- ① Correct order (WHERE → ORDER BY → LIMIT)
- ② Correct order (only LIMIT position differs)
- ③ Correct order (only WHERE position differs)
- ④ Only ① is correct

---

**Question 8** What does ORDER BY category ASC, price DESC mean when sorting by multiple columns?

- ① Both category and price in ascending order
- ② Category ascending, price descending (sort by price within same category)
- ③ Sort by category first, not sorted by price
- ④ Sort by price first, ignore category

---

## Advanced Level (2 Questions) - Critical Thinking

**Question 9** Why do the following two queries produce different results?

```
Query A:
SELECT * FROM movie
WHERE rating >= 8.0
LIMIT 10;

Query B:
SELECT * FROM movie
LIMIT 10
WHERE rating >= 8.0;
```

- ① Both queries produce same result
- ② Query A is valid, Query B has syntax error (WHERE must come before LIMIT)
- ③ Query B is faster
- ④ Both depend on the system

---

**Question 10** What is the most efficient query for this situation?

"From 1000 products, retrieve top 20 products in 'Electronics' category sorted by high price,
showing 10 per page and currently viewing page 3"

```sql
① SELECT * FROM product 
   WHERE category = 'Electronics'
   ORDER BY price DESC
   LIMIT 20;

② SELECT * FROM product 
   WHERE category = 'Electronics'
   ORDER BY price DESC
   LIMIT 10 OFFSET 20;

③ SELECT * FROM product 
   ORDER BY price DESC
   LIMIT 20 OFFSET 10;
```

- ① Correct (retrieves all 20, not paginated)
- ② Correct (page 3 = offset 20, display 10 per page)
- ③ Correct (ignores category filter)
- ④ ① and ② are both appropriate depending on requirements

---

# Short Answer Questions (5 Questions)

## Beginner Level (3 Questions)

**Question 11** Explain what column aliases are and give 3 examples of using them.

---

**Question 12** Explain the purpose of DISTINCT and provide an example SQL.

---

**Question 13** When sorting with ORDER BY, describe what happens when using ASC and DESC separately.

---

## Intermediate Level (1 Question)

**Question 14** Write a SELECT query that:

1. Retrieves student name and grade from student table
2. Filters students with grade >= 80
3. Sorts by grade descending
4. Shows only top 10 students
5. Uses aliases for columns

---

## Advanced Level (1 Question)

**Question 15** Explain the execution order of SELECT command with a practical example:

Consider a sales table with columns: product_id, quantity, price, region.
Write a query that:

1. Filters only sales where price > 100000
2. Groups by region
3. Counts the number of sales
4. Orders by count descending

Explain which clause is executed first and why this order matters for performance.

---

# Practical Problems (5 Questions)

## Beginner Level (2 Questions)

**Question 16** Create a student table and execute the following:

```sql
CREATE DATABASE ch3_practice CHARACTER SET utf8mb4;
USE ch3_practice;

CREATE TABLE student (
    student_id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(30) NOT NULL,
    major VARCHAR(30),
    grade INT,
    gpa FLOAT
);

INSERT INTO student VALUES
(1, 'Kim Chulsu', 'Computer Science', 2, 3.8),
(2, 'Lee Younghee', 'Software Engineering', 3, 3.9),
(3, 'Park Minjun', 'Computer Science', 1, 3.5),
(4, 'Choi Sunsin', 'Data Science', 2, 4.0),
(5, 'Kang Gamchan', 'Software Engineering', 4, 3.6);

SELECT * FROM student;
```

Submission: Screenshot showing all student records

---

**Question 17** Using the student table from Question 16, write and execute the following queries:

```sql
1. SELECT all columns and rows
2. SELECT only name and gpa columns with alias
3. SELECT students with gpa >= 3.7
4. SELECT students sorted by gpa descending
5. SELECT top 3 students by gpa

Provide screenshots for each query result
```

---

## Intermediate Level (2 Questions)

**Question 18** Using the student table, write queries for each situation and provide results:

```
1. Retrieve distinct majors
2. Count students by major
3. Retrieve students with Computer Science major sorted by name
4. Retrieve students with gpa >= 3.8 sorted by gpa descending, top 5
5. Implement pagination: page 1 (rows 1-3), page 2 (rows 4-6)
```

Submission: SQL code and result screenshots for each

---

**Question 19** Create a course table and write queries:

```sql
CREATE TABLE course (
    course_id INT PRIMARY KEY AUTO_INCREMENT,
    course_name VARCHAR(50) NOT NULL,
    professor VARCHAR(30),
    credits INT,
    semester INT
);

INSERT INTO course VALUES
(1, 'Database', 'Kim Chulsu', 3, 2),
(2, 'Web Programming', 'Lee Younghee', 4, 1),
(3, 'Data Science', 'Park Minjun', 3, 2),
(4, 'AI', 'Choi Sunsin', 3, 2),
(5, 'Security', 'Kang Gamchan', 3, 1);

Write queries:
1. Retrieve all courses
2. Retrieve 3-credit courses
3. Retrieve courses by semester sorted ascending
4. Retrieve courses with credit >= 3 by professor name
5. Retrieve top 3 courses by credits

Submission: SQL and result screenshots
```

---

## Advanced Level (1 Question)

**Question 20** Design and implement a comprehensive SELECT query practice:

```
Create two tables: student and enrollment
Student table: student_id, name, major
Enrollment table: enrollment_id, student_id, course_id, score

Requirements:
1. Retrieve students with average score >= 80
2. Retrieve top 10 students by score, showing name and score with alias
3. Retrieve students from 'Computer Science' major sorted by name
4. Implement pagination for large result sets (10 per page, page 2)
5. Retrieve all students and courses with DISTINCT

Submission: Complete SQL code, table structure, sample data, 
and result screenshots for each query
```

---

---

# 📋 Answer Key and Model Answers

## Multiple Choice Answers (10 Questions)

| Question | Answer | Explanation                                                       |
| :------: | :----: | :---------------------------------------------------------------- |
|    1    |   ②   | WHERE executes after FROM, before GROUP BY for filtering          |
|    2    |   ②   | DISTINCT removes duplicate rows                                   |
|    3    |   ②   | Alias makes column names more readable                            |
|    4    |   ③   | Default is ascending (ASC)                                        |
|    5    |   ③   | OFFSET 20 skips first 20, LIMIT 10 shows next 10 (21-30)          |
|    6    |   ②   | WHERE filters first for efficiency                                |
|    7    |   ①   | Correct clause order is WHERE → ORDER BY → LIMIT                |
|    8    |   ②   | Multiple columns: primary sort category ASC, secondary price DESC |
|    9    |   ②   | Query B has syntax error - WHERE must come before LIMIT           |
|    10    |   ②   | OFFSET 20 (skip 20) with LIMIT 10 = page 3 (rows 21-30)           |

---

## Short Answer Model Answers (5 Questions)

### Question 11 Column Aliases

**Model Answer**:

- **Definition**: Alternative name given to column in SELECT result
- **Purpose**: Make column names more readable and understandable
- **Examples**:
  1. SELECT student_id AS 'StudentID'
  2. SELECT student_name AS '학생명'
  3. SELECT salary * 12 AS 'Annual Salary'

---

### Question 12 DISTINCT Purpose and Example

**Model Answer**:

- **Purpose**: Remove duplicate rows and display only unique values
- **Example**:

```sql
SELECT DISTINCT major FROM student;
-- Displays each major only once, even if multiple students have same major
```

---

### Question 13 ASC vs DESC

**Model Answer**:

- **ASC (Ascending)**: Sorts from smallest to largest (A-Z, 0-9, oldest-newest)
- **DESC (Descending)**: Sorts from largest to smallest (Z-A, 9-0, newest-oldest)
- **Example**:
  - ORDER BY salary ASC: 3000000, 4000000, 5000000
  - ORDER BY salary DESC: 5000000, 4000000, 3000000

---

### Question 14 SELECT Query with Conditions

**Model Answer**:

```sql
SELECT name AS '이름', grade AS '학년'
FROM student
WHERE grade >= 80
ORDER BY grade DESC
LIMIT 10;
```

---

### Question 15 SELECT Execution Order Explanation

**Model Answer**:

```sql
SELECT region, COUNT(*) AS '판매수'
FROM sales
WHERE price > 100000
GROUP BY region
ORDER BY COUNT(*) DESC;
```

**Execution Order**:

1. FROM sales (access table)
2. WHERE price > 100000 (filter rows - IMPORTANT: reduces data first)
3. GROUP BY region (group filtered data)
4. SELECT region, COUNT(*) (calculate count)
5. ORDER BY COUNT(*) DESC (sort results)

**Why This Order Matters**:

- WHERE filters first → reduces data before grouping → better performance
- If filter comes after GROUP BY → must process all data first → slower

---

Prof. Jeong-Hyun Cho (peterchokr@gmail.com)
Yeungnam University College
