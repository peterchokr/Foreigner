# Database Fundamentals

### Instructor

**Prof. Jung-Hyun Cho** (peterchokr@gmail.com)

### Course Objectives

```
✅ Understand fundamental concepts of databases
✅ Develop SQL query writing skills
✅ Learn database design and normalization
✅ Develop DB applications using Python
```

---

## 📋 Weekly Schedule

| Week | Chapter | Topic                    | Coverage                    |                 Assignment                 |
| :--: | :-----: | :----------------------- | :-------------------------- | :----------------------------------------: |
|  1  |   Ch1   | Database Overview        | DB fundamentals             | Multiple Choice + Short Answer + Practical |
|  2  |   Ch2   | DDL (CREATE/ALTER/DROP)  | Table creation/management   |             Exercise Problems             |
|  3  |   Ch3   | SELECT Basics            | SELECT syntax + practice    |             Exercise Problems             |
|  4  |   Ch4   | WHERE Clause             | Conditional queries         |             Exercise Problems             |
|  5  |   Ch5   | JOIN Basics              | Inner joins                 |             Exercise Problems             |
|  6  |   Ch6   | Advanced JOIN            | Outer joins + self-join     |             Exercise Problems             |
|  7  |   Ch7   | Aggregate Functions      | Data analysis queries       |             Exercise Problems             |
|  8  |   Ch8   | Subquery                 | Nested queries              |             Exercise Problems             |
|  9  |   Ch9   | DML & Transaction        | Data manipulation + control |             Exercise Problems             |
|  10  |  Ch10  | VIEW & Stored Procedures | Database objects            |        Problems + Procedure coding        |
|  11  |  Ch11  | Control Structures       | Control statements/loops    |     Problems + Control implementation     |
|  12  |  Ch12  | Triggers                 | Auto-execution objects      |         Problems + Trigger coding         |
|  13  |  Ch13  | Normalization & Design   | DB design + normalization   |         Problems + Design project         |

### Learning Methods

```
✅ Lectures: Theory + Practical examples combined
✅ Self-study: Practice problems for individual learning
✅ Lab work: Coding assignments for practical skills
✅ Support: Email/LMS for Q&A
```

### Career Paths

```
✅ Web Backend Developer
✅ Database Administrator (DBA)
✅ Data Analyst
✅ Professional Certifications
```

---

### Grade Evaluation

```
Midterm Exam (Written): 30%
Final Exam (Written): 30%
Practical Evaluation (Quizzes, Assignments): 20%
Attendance: 20%
```

---

## 💡 Learning Tips

```
✅ Practice all examples in each chapter
✅ Solve practice problems before checking answers
✅ Analyze why incorrect answers are wrong
✅ Review on the same day as lecture (spaced repetition)
✅ Ask immediately if you don't understand
```

---

## 📊 Course Structure Overview

### 🔷 Chapter 1: Database Overview

|             Item             | Content                                                        |
| :---------------------------: | :------------------------------------------------------------- |
|        **Topic**        | Fundamental concepts and characteristics of databases          |
|  **Learning Content**  | Database definition, DBMS, data models, 5 core characteristics |
|    **Key Concepts**    | Data, information, database, relational model                  |
| **Real-world Examples** | Employee database, student management system                   |
|     **Core Syntax**     | Understanding fundamental concepts                             |
|    **Problem Count**    | 10 Multiple Choice + 5 Short Answer + 5 Practical              |

---

### 🔷 Chapter 2: DDL (CREATE, ALTER, DROP)

|             Item             | Content                                            |
| :---------------------------: | :------------------------------------------------- |
|        **Topic**        | Database structure definition                      |
|  **Learning Content**  | CREATE TABLE, ALTER TABLE, DROP TABLE, constraints |
|    **Key Concepts**    | Data types, PRIMARY KEY, FOREIGN KEY, constraints  |
| **Real-world Examples** | Create employee table, add columns, modify tables  |
|     **Core Syntax**     | `CREATE TABLE`, `PRIMARY KEY`, `NOT NULL`    |
|    **Problem Count**    | 10 Multiple Choice + 5 Short Answer + 5 Practical  |

---

### 🔷 Chapter 3: SELECT Basics

|             Item             | Content                                                                 |
| :---------------------------: | :---------------------------------------------------------------------- |
|        **Topic**        | SELECT statement structure and usage                                    |
|  **Learning Content**  | SELECT clause, FROM clause, data retrieval, column selection, wildcards |
|    **Key Concepts**    | Query, SELECT statement, columns, rows                                  |
| **Real-world Examples** | Retrieve employee info, select specific columns                         |
|     **Core Syntax**     | `SELECT column FROM table;`                                           |
|    **Problem Count**    | 10 Multiple Choice + 5 Short Answer + 5 Practical                       |

---

### 🔷 Chapter 4: WHERE Clause

|             Item             | Content                                                           |
| :---------------------------: | :---------------------------------------------------------------- |
|        **Topic**        | Conditional data retrieval                                        |
|  **Learning Content**  | WHERE clause, comparison operators, logical operators, conditions |
|    **Key Concepts**    | Conditional expressions, AND/OR/NOT, LIKE, IN, BETWEEN            |
| **Real-world Examples** | Salary range search, department-based queries                     |
|     **Core Syntax**     | `WHERE salary > 4000000 AND dept_id = 1`                        |
|    **Problem Count**    | 10 Multiple Choice + 5 Short Answer + 5 Practical                 |

---

### 🔷 Chapter 5: JOIN Basics

|             Item             | Content                                                              |
| :---------------------------: | :------------------------------------------------------------------- |
|        **Topic**        | Combining data from multiple tables (Basics)                         |
|  **Learning Content**  | INNER JOIN, cartesian product, JOIN principles                       |
|    **Key Concepts**    | Relationships, primary keys, foreign keys, one-to-many relationships |
| **Real-world Examples** | Retrieve employee and department info together                       |
|     **Core Syntax**     | `JOIN ON`, `INNER JOIN`                                          |
|    **Problem Count**    | 10 Multiple Choice + 5 Short Answer + 5 Practical                    |

---

### 🔷 Chapter 6: Advanced JOIN

|             Item             | Content                                                     |
| :---------------------------: | :---------------------------------------------------------- |
|        **Topic**        | Combining data from multiple tables (Advanced)              |
|  **Learning Content**  | LEFT JOIN, RIGHT JOIN, FULL JOIN, self-join                 |
|    **Key Concepts**    | Outer joins, NULL handling, multiple table joins            |
| **Real-world Examples** | Retrieve all employees (including those without department) |
|     **Core Syntax**     | `LEFT JOIN`, `RIGHT JOIN`, `FULL OUTER JOIN`          |
|    **Problem Count**    | 10 Multiple Choice + 5 Short Answer + 5 Practical           |

---

### 🔷 Chapter 7: Aggregate Functions

|             Item             | Content                                             |
| :---------------------------: | :-------------------------------------------------- |
|        **Topic**        | Data aggregation and grouping                       |
|  **Learning Content**  | COUNT, SUM, AVG, MAX, MIN, GROUP BY, HAVING         |
|    **Key Concepts**    | Aggregate functions, grouping, HAVING clause        |
| **Real-world Examples** | Average salary by department, highest paid employee |
|     **Core Syntax**     | `GROUP BY dept_id HAVING AVG(salary) > 4000000`   |
|    **Problem Count**    | 10 Multiple Choice + 5 Short Answer + 5 Practical   |

---

### 🔷 Chapter 8: Subquery

|             Item             | Content                                                |
| :---------------------------: | :----------------------------------------------------- |
|        **Topic**        | Query within query                                     |
|  **Learning Content**  | SELECT subquery, WHERE subquery, FROM subquery         |
|    **Key Concepts**    | Single-row/multiple-row results, IN, EXISTS            |
| **Real-world Examples** | Retrieve employees with salary above average           |
|     **Core Syntax**     | `WHERE salary > (SELECT AVG(salary) FROM employees)` |
|    **Problem Count**    | 10 Multiple Choice + 5 Short Answer + 5 Practical      |

---

### 🔷 Chapter 9: DML and Transaction

|             Item             | Content                                                 |
| :---------------------------: | :------------------------------------------------------ |
|        **Topic**        | Data manipulation and transaction control               |
|  **Learning Content**  | INSERT, UPDATE, DELETE, COMMIT, ROLLBACK, ACID          |
|    **Key Concepts**    | DML, transaction, atomicity, consistency, integrity     |
| **Real-world Examples** | Data insertion, salary update, partial failure handling |
|     **Core Syntax**     | `INSERT INTO`, `UPDATE SET`, `BEGIN`, `COMMIT`  |
|    **Problem Count**    | 10 Multiple Choice + 5 Short Answer + 5 Practical       |

---

### 🔷 Chapter 10: VIEW and Stored Procedure

|             Item             | Content                                                      |
| :---------------------------: | :----------------------------------------------------------- |
|        **Topic**        | Utilizing database objects                                   |
|  **Learning Content**  | CREATE and manage VIEWs, stored procedure concept/creation   |
|    **Key Concepts**    | Virtual table, function, parameter, reusability              |
| **Real-world Examples** | Department-based employee VIEW, salary calculation procedure |
|     **Core Syntax**     | `CREATE VIEW`, `CREATE PROCEDURE`                        |
|    **Problem Count**    | 10 Multiple Choice + 5 Short Answer + 5 Practical            |

---

### 🔷 Chapter 11: Control Structures

|             Item             | Content                                                 |
| :---------------------------: | :------------------------------------------------------ |
|        **Topic**        | Programming logic in procedures                         |
|  **Learning Content**  | IF-THEN-ELSE, CASE, WHILE, REPEAT, Cursor               |
|    **Key Concepts**    | Conditional branching, loops, row processing via cursor |
| **Real-world Examples** | Salary grade determination, batch data insertion        |
|     **Core Syntax**     | `IF`, `CASE`, `WHILE`, `LOOP`                   |
|    **Problem Count**    | 10 Multiple Choice + 5 Short Answer + 5 Practical       |

---

### 🔷 Chapter 12: Triggers

|             Item             | Content                                                       |
| :---------------------------: | :------------------------------------------------------------ |
|        **Topic**        | Auto-executing database objects                               |
|  **Learning Content**  | BEFORE/AFTER triggers, INSERT/UPDATE/DELETE triggers, NEW/OLD |
|    **Key Concepts**    | Event monitoring, auto-execution, audit logging               |
| **Real-world Examples** | Salary change audit log, data validation                      |
|     **Core Syntax**     | `CREATE TRIGGER`, `BEFORE/AFTER`, `NEW/OLD`             |
|    **Problem Count**    | 10 Multiple Choice + 5 Short Answer + 5 Practical             |

---

### 🔷 Chapter 13: Normalization and Design

|             Item             | Content                                                     |
| :---------------------------: | :---------------------------------------------------------- |
|        **Topic**        | Efficient database design                                   |
|  **Learning Content**  | Anomalies, functional dependency, 1NF~3NF, BCNF, ER diagram |
|    **Key Concepts**    | Eliminate data redundancy, ensure integrity, design process |
| **Real-world Examples** | Student management system design, online store design       |
|     **Core Syntax**     | `1NF→2NF→3NF`, ER Diagram                               |
|    **Problem Count**    | 10 Multiple Choice + 5 Short Answer + 5 Practical           |

---


## 🎯 Learning Outcomes

By the end of this course, students will be able to:

1. **Understand** fundamental database concepts and DBMS architecture
2. **Design** efficient databases using normalization principles
3. **Write** complex SQL queries including JOINs, subqueries, and aggregations
4. **Create** database objects (Views, Procedures, Triggers)
5. **Implement** database applications using Python
6. **Apply** best practices for data integrity and security
7. **Solve** real-world data management problems

---

## 📞 Contact & Support

**Instructor:** Prof. Jeong-Hyun Cho
**Email:** peterchokr@gmail.com

---

**Let's master Database Fundamentals together! 🚀**
