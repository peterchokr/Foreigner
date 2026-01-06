# Ch6 Advanced JOIN - Exercise Problems

Dear students! After completing Chapter 6, please solve the following exercises to verify your understanding.
Check the difficulty level of each section and study step by step.

---

## 📌 Learning Objectives Verification

After completing Chapter 6, you should understand:

- Difference and usage of LEFT JOIN and RIGHT JOIN
- Self Join (JOIN table with itself)
- MySQL implementation of FULL OUTER JOIN (LEFT + RIGHT + UNION)
- Multiple table JOIN (3 or more)
- JOIN performance optimization

---

# Multiple Choice Questions (10 Questions)

## Beginner Level (5 Questions) - Basic Concept Verification

**Question 1** What is basic characteristic of RIGHT JOIN?

- ① Include all rows from left table
- ② Include all rows from right table
- ③ Only rows matching both tables
- ④ Only right table allows NULL

---

**Question 2** What is concept of Self Join?

- ① Connect two different tables
- ② Use same table twice and connect with itself
- ③ Copy one table multiple times
- ④ Connect two databases

---

**Question 3** What is FULL OUTER JOIN?

- ① Provided by MySQL by default
- ② Not supported in MySQL, implemented with LEFT + RIGHT + UNION
- ③ Execute LEFT and RIGHT JOIN sequentially
- ④ Merge all data from both tables

---

**Question 4** When is Self Join necessary?

- ① Connect two different tables
- ② Express employee and their supervisor (manager) relationship
- ③ Query multiple tables simultaneously
- ④ When sorting data

---

**Question 5** When right table has no data in LEFT JOIN, result is?

- ① Row excluded from results
- ② Columns from right table shown as NULL
- ③ Left data repeated
- ④ Error occurs

---

## Intermediate Level (3 Questions) - Concept Application

**Question 6** To represent employees and managers using Self Join?

```sql
① SELECT e1.name, e2.name
   FROM employee e1
   LEFT JOIN employee e2 ON e1.manager_id = e2.employee_id;

② SELECT e1.name, e2.name
   FROM employee e1
   JOIN employee e2 ON e1.manager_id = e2.manager_id;

③ SELECT e1.name FROM employee e1
   WHERE e1.manager_id IS NOT NULL;
```

- ① Correct (employee and their manager)
- ② Correct (colleagues with same manager)
- ③ Correct (employees with manager)
- ④ ① and ② are correct

---

**Question 7** In this situation, use LEFT JOIN or RIGHT JOIN?

"Query all departments with employees in each,
include departments without employees"

```sql
① SELECT d.dept_name, e.name
   FROM department d
   LEFT JOIN employee e ON d.dept_id = e.dept_id;

② SELECT d.dept_name, e.name
   FROM employee e
   RIGHT JOIN department d ON d.dept_id = e.dept_id;
```

- ① Correct (department is left reference)
- ② Correct (achieves same result)
- ③ Only ① correct
- ④ Only ② correct

---

**Question 8** To implement FULL OUTER JOIN in MySQL?

- ① Use FULL OUTER JOIN directly
- ② LEFT JOIN UNION RIGHT JOIN
- ③ LEFT JOIN UNION ALL RIGHT JOIN EXCEPT duplicates
- ④ Multiple LEFT JOINs

---

## Advanced Level (2 Questions) - Critical Thinking

**Question 9** Difference between LEFT and RIGHT JOIN?

- ① Only direction/naming differs, result same
- ② LEFT/RIGHT determines which table is reference for NULL rows
- ③ LEFT has better performance
- ④ RIGHT requires additional conditions

---

**Question 10** Why learn Self Join if can use two separate tables?

- ① Simpler code
- ② Represent hierarchical relationships within single table
- ③ Perform comparisons within same table
- ④ All above

---

# Short Answer Questions (5 Questions)

## Beginner Level (3 Questions)

**Question 11** Explain basic concept and purpose of Self Join.

---

**Question 12** Explain difference and appropriate usage scenarios for LEFT JOIN and RIGHT JOIN.

---

**Question 13** What is FULL OUTER JOIN and how is it implemented in MySQL?

---

## Intermediate Level (1 Question)

**Question 14** Explain why Self Join with LEFT JOIN is needed to include employees without supervisors.

---

## Advanced Level (1 Question)

**Question 15** Explain optimization strategies when using multiple JOIN types and handling NULL values in FULL OUTER JOIN.

---

# Practical Problems (5 Questions)

## Beginner Level (2 Questions)

**Question 16** Execute following SQL and provide result screenshot.

```sql
CREATE DATABASE ch6_advanced_join CHARACTER SET utf8mb4;
USE ch6_advanced_join;

CREATE TABLE employee (
    employee_id INT PRIMARY KEY AUTO_INCREMENT,
    employee_name VARCHAR(30) NOT NULL,
    position VARCHAR(20),
    manager_id INT
) CHARACTER SET utf8mb4;

INSERT INTO employee VALUES
(1, 'Director Kim', 'Director', NULL),
(2, 'Manager Lee', 'Manager', 1),
(3, 'Manager Park', 'Manager', 1),
(4, 'Staff Choi', 'Staff', 2),
(5, 'Staff Jung', 'Staff', 2),
(6, 'Intern Kang', 'Intern', 3);

SELECT * FROM employee;
```

Submission: Screenshot showing all 6 employee records

---

**Question 17** Perform Self Join on employee table to query employee-manager relationships.

```sql
SELECT e1.employee_name, e2.employee_name AS manager_name
FROM employee e1
LEFT JOIN employee e2 ON e1.manager_id = e2.employee_id;
```

Submission: Screenshot showing employee and manager pairs

---

## Intermediate Level (2 Questions)

**Question 18** Create department and department_head tables and perform following.

```sql
CREATE TABLE department (
    dept_id INT PRIMARY KEY AUTO_INCREMENT,
    dept_name VARCHAR(30) NOT NULL
) CHARACTER SET utf8mb4;

CREATE TABLE department_head (
    dept_id INT PRIMARY KEY,
    emp_id INT,
    FOREIGN KEY (dept_id) REFERENCES department(dept_id),
    FOREIGN KEY (emp_id) REFERENCES employee(employee_id)
) CHARACTER SET utf8mb4;

INSERT INTO department VALUES
(1, 'Development'),
(2, 'Sales'),
(3, 'HR');

INSERT INTO department_head VALUES
(1, 1),
(2, 2),
(3, NULL);

-- LEFT JOIN to include department without head
SELECT d.dept_name, e.employee_name
FROM department d
LEFT JOIN department_head h ON d.dept_id = h.dept_id
LEFT JOIN employee e ON h.emp_id = e.employee_id;
```

Submission: Screenshot of LEFT JOIN result

---

**Question 19** Execute and analyze LEFT JOIN vs RIGHT JOIN difference.

```sql
-- Query 1: LEFT JOIN (all employees)
SELECT e.employee_name, d.dept_name
FROM employee e
LEFT JOIN department d ON e.employee_id = d.dept_id;

-- Query 2: RIGHT JOIN (all departments)
SELECT e.employee_name, d.dept_name
FROM employee e
RIGHT JOIN department d ON d.dept_id = e.employee_id;

Analyze and compare results
```

Submission: SQL code and comparison screenshot

---

## Advanced Level (1 Question)

**Question 20** Implement and execute FULL OUTER JOIN using UNION.

```
Requirements:
1. Create FULL OUTER JOIN using LEFT + RIGHT + UNION
2. Query all data from both tables (include unmatched)
3. Include Self Join example
4. Analyze performance with multiple JOINs

Submission:
   - SQL code for FULL OUTER JOIN
   - Screenshot of UNION result
   - Performance analysis explanation
```

---

---

# 📋 Answer Key and Model Answers

## Multiple Choice Answers (10 Questions)

| Question | Answer | Explanation                                    |
| :------: | :----: | :--------------------------------------------- |
|    1    |   ②   | RIGHT JOIN includes all rows from right table  |
|    2    |   ②   | Self Join uses same table twice with itself    |
|    3    |   ②   | MySQL uses LEFT + RIGHT + UNION for FULL OUTER |
|    4    |   ②   | Self Join expresses hierarchical relationships |
|    5    |   ②   | LEFT columns become NULL when no match         |
|    6    |   ④   | Both ① and ② are correct depending on need   |
|    7    |   ④   | Both achieve same result (different syntax)    |
|    8    |   ②   | LEFT JOIN UNION RIGHT JOIN                     |
|    9    |   ②   | Determines which table is reference            |
|    10    |   ④   | All reasons valid                              |

---

## Short Answer Model Answers (5 Questions)

### Question 11 Self Join Concept

**Model Answer**:

```
Self Join:
- Definition: Join table with itself
- Purpose: Express relationships within single table
- Requirement: Use table aliases (e1, e2)
- Example: Employee-Manager relationship

Syntax:
SELECT e1.name, e2.name AS manager_name
FROM employee e1
LEFT JOIN employee e2 ON e1.manager_id = e2.employee_id;

Use cases:
- Hierarchical data (employee-manager)
- Team comparison
- Self-referential relationships
```

---

### Question 12 LEFT vs RIGHT JOIN

**Model Answer**:

```
LEFT JOIN:
- Include all from left table + matching right data
- Null in right columns when no match
- Usage: When left table is reference

RIGHT JOIN:
- Include all from right table + matching left data
- Null in left columns when no match
- Usage: When right table is reference

Example:
LEFT: SELECT * FROM employees LEFT JOIN departments
      → All employees, NULL if no dept

RIGHT: SELECT * FROM employees RIGHT JOIN departments
       → All departments, NULL if no employee

Choice: Depends on which table is reference point
```

---

### Question 13 FULL OUTER JOIN Implementation

**Model Answer**:

```
FULL OUTER JOIN:
- Definition: All data from both tables
- MySQL support: Not directly, use UNION

Implementation:
SELECT * FROM table_a
LEFT JOIN table_b ON condition
UNION
SELECT * FROM table_a
RIGHT JOIN table_b ON condition;

Result: All rows from both tables, NULL where no match

Usage: When need all data from both sources
```

---

### Question 14 Self Join with LEFT JOIN

**Model Answer**:

```
Why LEFT JOIN in Self Join:
- Include employees without managers
- NULL in manager_id field

Example:
SELECT e1.name, e2.name AS manager_name
FROM employee e1
LEFT JOIN employee e2 ON e1.manager_id = e2.employee_id;

Result:
- Director Kim → NULL (no manager)
- Manager Lee → Director Kim
- Staff → their respective managers
- All employees shown

Without LEFT JOIN (INNER):
- Director would be excluded
```

---

### Question 15 Optimization Strategies

**Model Answer**:

```
Optimization for Multiple JOINs:

1. Join order
   - Most selective tables first
   - Follow foreign key relationships

2. Index strategy
   - Index on join conditions
   - Avoid function calls in ON

3. NULL handling
   - Use COALESCE for display
   - Filter early with WHERE

4. FULL OUTER implementation
   - Avoid redundant queries
   - Use efficient UNION
   - Consider materialized view

5. Performance monitoring
   - Use EXPLAIN to analyze
   - Profile slow queries
```

---

Prof. Jeong-Hyun Cho (peterchokr@gmail.com)
Yeungnam University College
