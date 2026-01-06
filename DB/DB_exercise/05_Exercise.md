# Ch5 JOIN Basics - Exercise Problems

Dear students! After completing Chapter 5, please solve the following exercises to verify your understanding.
Check the difficulty level of each section and study step by step.

---

## 📌 Learning Objectives Verification

After completing Chapter 5, you should understand:

- Basic concept and syntax of INNER JOIN
- LEFT JOIN to include all rows from one table
- Role of ON clause (table connection condition)
- Multiple table JOIN (3 or more)
- Necessity and application of JOIN

---

# Multiple Choice Questions (10 Questions)

## Beginner Level (5 Questions) - Basic Concept Verification

**Question 1** What is basic characteristic of INNER JOIN?

- ① Only rows matching both tables
- ② All rows from left table included
- ③ All rows from right table included
- ④ All rows displayed with duplication

---

**Question 2** What is role of ON clause?

- ① Specify condition to connect tables
- ② Filter results (same as WHERE)
- ③ Sort data
- ④ Limit number of rows

---

**Question 3** What is characteristic of LEFT JOIN?

- ① Include all rows from left table
- ② Include all rows from right table
- ③ Both tables have same number of rows
- ④ Unmatched rows are excluded

---

**Question 4** What is most important reason for JOIN?

- ① Database operates faster
- ② Combine normalized data to create meaningful information
- ③ Convert data type
- ④ Strengthen security

---

**Question 5** Basic difference between INNER JOIN and LEFT JOIN?

- ① Speed difference
- ② Difference in handling unmatched rows
- ③ Difference in available operators
- ④ Function usage availability

---

## Intermediate Level (3 Questions) - Concept Application

**Question 6** What is correct order when JOINing 3 tables?

```sql
① SELECT s.name, c.course_name, e.grade
   FROM student s
   INNER JOIN enrollment e ON s.student_id = e.student_id
   INNER JOIN course c ON e.course_id = c.course_id;

② SELECT s.name, c.course_name, e.grade
   FROM student s
   INNER JOIN course c ON s.student_id = c.course_id
   INNER JOIN enrollment e ON e.course_id = c.course_id;
```

- ① Correct (student → enrollment → course order)
- ② Correct (other order also possible)
- ③ Only ① is correct
- ④ Only ② is correct

---

**Question 7** When to use LEFT JOIN?

- ① Data only in both tables
- ② All data from left table + matching information from right
- ③ All data from right table
- ④ When fast performance needed

---

**Question 8** Difference between ON and WHERE clause?

- ① Functions completely identical
- ② ON at JOIN time, WHERE after JOIN for filtering
- ③ ON has better performance
- ④ Only WHERE uses index

---

## Advanced Level (2 Questions) - Critical Thinking

**Question 9** What is difference between these two queries?

```
Query A:
SELECT p.prof_name, c.course_name
FROM professor p
LEFT JOIN course c ON p.prof_id = c.prof_id;

Query B:
SELECT p.prof_name, c.course_name
FROM professor p
LEFT JOIN course c ON p.prof_id = c.prof_id
WHERE c.course_name IS NOT NULL;
```

- ① Results are same
- ② Query A includes professors without courses, Query B excludes
- ③ Query B properly utilizes LEFT JOIN meaning
- ④ Query A same result as INNER JOIN

---

**Question 10** Why is JOIN necessary when database normalized into multiple tables?

- ① Faster speed
- ② Data duplication removed, so combine again to create meaningful information
- ③ Strengthened security
- ④ Memory reduction

---

# Short Answer Questions (5 Questions)

## Beginner Level (3 Questions)

**Question 11** Explain most fundamental difference between INNER JOIN and LEFT JOIN.

---

**Question 12** Explain role of ON clause and difference from WHERE clause.

---

**Question 13** Explain why JOIN should be used in this situation:
"Need to display courses taken by students together with their professors"

---

## Intermediate Level (1 Question)

**Question 14** Explain whether table order in multiple JOINs affects results,
and explain why position of left table is important in LEFT JOIN.

---

## Advanced Level (1 Question)

**Question 15** Explain considerations when JOINing 3+ tables,
and analyze impact of NULL values on JOIN results.

---

# Practical Problems (5 Questions)

## Beginner Level (2 Questions)

**Question 16** Execute following SQL and provide result screenshot.

```sql
CREATE DATABASE ch5_join CHARACTER SET utf8mb4;
USE ch5_join;

CREATE TABLE professor (
    professor_id INT PRIMARY KEY AUTO_INCREMENT,
    professor_name VARCHAR(30) NOT NULL,
    department VARCHAR(30)
) CHARACTER SET utf8mb4;

CREATE TABLE course (
    course_id INT PRIMARY KEY AUTO_INCREMENT,
    course_name VARCHAR(30) NOT NULL,
    credits INT,
    professor_id INT,
    FOREIGN KEY (professor_id) REFERENCES professor(professor_id)
) CHARACTER SET utf8mb4;

INSERT INTO professor VALUES
(1, 'Park Chulsu', 'AI Software Department'),
(2, 'Lee Younghee', 'AI Software Department'),
(3, 'Choi Junho', 'AI Software Department');

INSERT INTO course VALUES
(1, 'Database', 3, 1),
(2, 'Web Programming', 3, 2),
(3, 'Artificial Intelligence', 3, 1),
(4, 'Cloud Computing', 3, 3);

SELECT * FROM professor;
SELECT * FROM course;
```

Submission: Screenshot showing both professor and course table data

---

**Question 17** INNER JOIN professor and course tables to query following.

```sql
-- 1. Query courses and responsible professors
SELECT c.course_name, p.professor_name
FROM course c
INNER JOIN professor p ON c.professor_id = p.professor_id;

-- 2. Query course name, professor name, credits
SELECT c.course_name, p.professor_name, c.credits
FROM course c
INNER JOIN professor p ON c.professor_id = p.professor_id;
```

Submission: Screenshot showing results of both queries

---

## Intermediate Level (2 Questions)

**Question 18** Create student and enrollment tables and perform following.

```sql
CREATE TABLE student (
    student_id INT PRIMARY KEY AUTO_INCREMENT,
    student_name VARCHAR(30) NOT NULL,
    major VARCHAR(30)
) CHARACTER SET utf8mb4;

CREATE TABLE enrollment (
    enrollment_id INT PRIMARY KEY AUTO_INCREMENT,
    student_id INT,
    course_id INT,
    grade VARCHAR(2),
    FOREIGN KEY (student_id) REFERENCES student(student_id),
    FOREIGN KEY (course_id) REFERENCES course(course_id)
) CHARACTER SET utf8mb4;

INSERT INTO student VALUES
(1, 'Student A', 'AI Software Department'),
(2, 'Student B', 'AI Software Department'),
(3, 'Student C', 'Computer Science'),
(4, 'Student D', 'AI Software Department');

INSERT INTO enrollment VALUES
(1, 1, 1, 'A'),
(2, 1, 2, 'B'),
(3, 2, 1, 'A'),
(4, 2, 3, 'B'),
(5, 3, 2, 'C'),
(6, 4, 1, 'A');

-- Query students, courses, grades
SELECT s.student_name, c.course_name, e.grade
FROM student s
INNER JOIN enrollment e ON s.student_id = e.student_id
INNER JOIN course c ON e.course_id = c.course_id;
```

Submission: Screenshot of 3-table JOIN result

---

**Question 19** LEFT JOIN professor table to query following.

```sql
-- LEFT JOIN: Include professors without courses
SELECT p.professor_name, c.course_name
FROM professor p
LEFT JOIN course c ON p.professor_id = c.professor_id;

-- Number of courses per professor
SELECT p.professor_name, COUNT(c.course_id) AS course_count
FROM professor p
LEFT JOIN course c ON p.professor_id = c.professor_id
GROUP BY p.professor_id, p.professor_name;
```

Submission: Screenshot showing results of both queries

---

## Advanced Level (1 Question)

**Question 20** Write and execute following complex JOIN queries.

```
Requirements:
1. JOIN 4 tables: student, enrollment, course, professor
   Result: student name, course name, grade, professor name

2. student LEFT JOIN enrollment to include students without courses
   Result: All students + their courses (NULL if no courses)

3. professor LEFT JOIN course to include professors without courses
   Result: All professors + their courses

4. Write 2+ free-form JOIN queries using:
   - Combination of INNER JOIN and LEFT JOIN
   - Usage with GROUP BY

Submission:
   - SQL code for each query
   - Screenshot of each query result
```

---

---

# 📋 Answer Key and Model Answers

## Multiple Choice Answers (10 Questions)

| Question | Answer | Explanation                                                           |
| :------: | :----: | :-------------------------------------------------------------------- |
|    1    |   ①   | INNER JOIN only matches rows from both tables                         |
|    2    |   ①   | ON specifies condition to connect two tables                          |
|    3    |   ①   | LEFT JOIN includes all rows from left table                           |
|    4    |   ②   | Create meaningful information by combining normalized data            |
|    5    |   ②   | Difference in handling unmatched rows (INNER excludes, LEFT includes) |
|    6    |   ③   | Only ① correct (student_id and course_id can connect properly)       |
|    7    |   ②   | All left data + matching information from right                       |
|    8    |   ②   | ON at JOIN time, WHERE after JOIN for filtering                       |
|    9    |   ②   | LEFT JOIN with WHERE becomes like INNER JOIN when filtering           |
|    10    |   ②   | Need JOIN to recombine normalized data                                |

---

## Short Answer Model Answers (5 Questions)

### Question 11 INNER JOIN vs LEFT JOIN

**Model Answer**:

```
INNER JOIN:
- Result: Only rows matching both tables
- Characteristic: Intersection
- Example: Only students enrolled in courses

LEFT JOIN:
- Result: All rows from left table + matching info from right
- Characteristic: Left table is basis
- Example: All students + courses taken (NULL if no courses)
```

---

### Question 12 Role of ON and WHERE Clauses

**Model Answer**:

```
ON clause:
- Role: Specify how to connect two tables
- Time: At JOIN execution
- Characteristic: Different from WHERE in LEFT JOIN

WHERE clause:
- Role: Additional filtering after JOIN
- Time: After JOIN
- Characteristic: Can exclude rows

Example difference:
LEFT JOIN ... WHERE: May lose one table's data
LEFT JOIN ... ON: Preserves all left table data
```

---

### Question 13 Why JOIN is Necessary

**Model Answer**:

```
Situation: Student, enrollment, course, professor in separate tables

Problems before normalization:
- Same course/professor info repeated for each student
- Require updating all rows when professor info changes
- Data consistency issues

Solved by JOIN:
SELECT s.student_name, c.course_name, p.professor_name
FROM student s
JOIN enrollment e ON s.student_id = e.student_id
JOIN course c ON e.course_id = c.course_id
JOIN professor p ON c.professor_id = p.professor_id;

Advantages:
- Maintains normalization benefits (eliminate duplication)
- Combine info only when needed
- Create meaningful information in single query
```

---

### Question 14 JOIN Table Order Importance

**Model Answer**:

```
INNER JOIN: Order doesn't affect result
- Both tables symmetric
- These two queries produce same result:
  SELECT * FROM A JOIN B ON ...
  SELECT * FROM B JOIN A ON ...

LEFT JOIN: Order is critical
- Left table is reference point
- SELECT * FROM professor LEFT JOIN course
  → All professors (NULL if no courses)
  
- SELECT * FROM course LEFT JOIN professor
  → All courses (NULL if professor absent)
  
Therefore: In LEFT JOIN, place reference table on left side
```

---

### Question 15 Multiple Table JOIN and NULL Handling

**Model Answer**:

```
Considerations:

1. Select primary table
   - Place most important data in FROM

2. JOIN order
   - Follow relationship diagram sequentially

3. NULL handling
   - INNER JOIN: NULL in join condition → match fails → row excluded
   - LEFT JOIN: NULL possible → use COALESCE to handle

4. Performance
   - Use index in conditions
   - Minimize number of join conditions

5. NULL's impact:
SELECT * FROM student s
LEFT JOIN enrollment e ON s.student_id = e.student_id
- If student_id = NULL in enrollment, JOIN fails
- Students without enrollment have all e.* columns as NULL
```

---

Prof. Jeong-Hyun Cho (peterchokr@gmail.com)
Yeungnam University College
