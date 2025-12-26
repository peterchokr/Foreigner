# Ch13 Normalization and Database Design - Exercise Problems

Dear students! After completing Chapter 13, please solve the following exercises to verify your understanding.
Check the difficulty level of each section and study step by step.

---

## 📌 Learning Objectives Verification

After completing Chapter 13, you should understand:

- Normalization concept and goals
- Identify and resolve Anomalies
- Functional Dependency
- 1NF to 3NF and BCNF
- ER Diagram creation
- Database design process
- Foreign Keys and referential integrity

---

# Multiple Choice Questions (10 Questions)

## Beginner Level (5 Questions) - Basic Concept Verification

**Question 1** Main purpose of Normalization?

- ① Increase database size
- ② Minimize data duplication and eliminate anomalies
- ③ Speed up query performance
- ④ Standardize programming languages

---

**Question 2** Anomaly type that is NOT correct?

- ① Insertion Anomaly
- ② Update Anomaly
- ③ Deletion Anomaly
- ④ Processing Anomaly

---

**Question 3** Condition for 1NF (First Normal Form)?

- ① All attributes are atomic values
- ② Remove partial functional dependency
- ③ Remove transitive functional dependency
- ④ Define superkey

---

**Question 4** NOT a basic element of ER Diagram?

- ① Entity
- ② Attribute
- ③ Relationship
- ④ Function

---

**Question 5** Example of 1:N (one-to-many) relationship?

- ① Student and student ID
- ② Professor and Course
- ③ Department and department name
- ④ Employee and SSN

---

## Intermediate Level (3 Questions) - Concept Application

**Question 6** Which violates 2NF (Second Normal Form)?

```
Enrollment (student_id, course_id, course_name, grade)
```

- ① course_name depends only on course_id
- ② grade depends on both student_id and course_id
- ③ student_id is key attribute
- ④ course_id is key attribute

---

**Question 7** Correct condition for 3NF (Third Normal Form)?

- ① Satisfies 2NF
- ② All non-key attributes not transitively depend on primary key
- ③ Both ① and ②
- ④ Depend only on superkey

---

**Question 8** Purpose of Foreign Key?

- ① Data encryption
- ② Define relationship between tables and ensure referential integrity
- ③ Improve query speed
- ④ Data replication

---

## Advanced Level (2 Questions) - Critical Thinking

**Question 9** Normalization vs Denormalization situation?

- ① Always use normalization
- ② Normalization is theory, denormalization is practice
- ③ Design normalized, consider denormalization for performance if needed
- ④ Always use denormalization

---

**Question 10** Meaning of Functional Dependency X → Y?

- ① X depends on Y
- ② Y depends on X
- ③ When X is determined, Y is uniquely determined
- ④ Relationship between X and Y unclear

---

# Short Answer Questions (5 Questions)

## Beginner Level (3 Questions)

**Question 11** Define Normalization and explain Anomalies (Insertion, Update, Deletion).

---

**Question 12** Explain conditions for 1NF, 2NF, 3NF respectively.

---

**Question 13** Explain basic ER Diagram elements (Entity, Attribute, Relationship) and Cardinality (1:1, 1:N, M:N).

---

## Intermediate Level (1 Question)

**Question 14** Explain Functional Dependency and complete, partial, transitive functional dependencies with examples.

---

## Advanced Level (1 Question)

**Question 15** Explain database design process (requirements → conceptual → logical → physical → implementation) and when normalization applies.

---

# Practical Problems (5 Questions)

## Beginner Level (2 Questions)

**Question 16** Convert following non-normalized data to 1NF.

```
Problem Data (composite phone numbers):
Students (student_id, name, phone)
001, Kim Chulsu, 02-123-4567 / 010-1111-2222
002, Lee Younghee, 031-456-7890 / 010-2222-3333

Requirements:
1. Explain non-normalization problems
2. Convert to 1NF table design
3. Implement with CREATE TABLE
4. Insert data
```

Submission: Conversion process explanation and 1NF table SQL

---

**Question 17** Convert following 1NF data to 2NF.

```
Problem Data (partial functional dependency):
Enrollment (student_id, course_id, course_name, credit, grade)
001, CS101, Data Structure, 3, A
001, CS102, Algorithm, 4, B

Problem: course_name, credit depend only on course_id

Requirements:
1. Explain partial functional dependency
2. Convert to 2NF (separate tables)
3. Set foreign keys
4. Insert data
```

Submission: 2NF table structure and SQL

---

## Intermediate Level (2 Questions)

**Question 18** Convert following 2NF data to 3NF.

```
Problem Data (transitive functional dependency):
Students (student_id, name, department, office)
001, Kim Chulsu, Computer Science, Room 301
002, Lee Younghee, Computer Science, Room 301
003, Park Minjun, Electronics, Room 302

Problem: office depends on department (not directly on student_id)
         student_id → department → office (transitive)

Requirements:
1. Explain transitive functional dependency
2. Convert to 3NF (separate tables)
3. Define foreign keys
4. Insert all data
5. Verify normalization benefits
```

Submission: 3NF structure, SQL, normalization analysis

---

**Question 19** Design online shopping database with ER Diagram and implement.

```
Requirements:
1. ER Diagram Design
   - Entities: Customers, Products, Categories, Orders, OrderDetails
   - Relationships:
     * Customer 1:N Orders
     * Order 1:N OrderDetails
     * Product M:N Orders (via OrderDetails)
     * Category 1:N Products

2. Create Tables (3NF)
   - Customers
   - Categories
   - Products (FK: category_id)
   - Orders (FK: customer_id)
   - OrderDetails (FK: order_id, product_id)

3. Insert Sample Data
   - 2 customers
   - 2 categories
   - 3 products
   - 2 orders
   - 3 order details

4. Verification Queries
   - JOIN customers with orders
   - List products by category
```

Submission: ER Diagram explanation, SQL code, execution results

---

## Advanced Level (1 Question)

**Question 20** Perform complex database design.

```
Requirements:
1. University Academic Management System Design
   - Entities: Students, Professors, Departments, Courses, Enrollments
   - Requirements:
     * Student belongs to one department (1:N)
     * Professor teaches multiple courses (1:N)
     * Course taught by one professor (1:1)
     * Course belongs to department (1:N)
     * Student enrolls in multiple courses (M:N)

2. Apply Normalization
   - Each entity 3NF+
   - Analyze functional dependencies
   - Eliminate anomalies

3. Set Data Integrity
   - Define primary keys
   - Define foreign key constraints
   - Validation rules

4. Sample Data and Queries
   - Count students per department
   - List courses per professor
   - List enrollments per student

Submission:
   - ER Diagram explanation
   - Normalization analysis (1NF → 3NF process)
   - Complete SQL code
   - Sample data and verification queries
```

---

---

# 📋 Answer Key and Model Answers

## Multiple Choice Answers (10 Questions)

| Question | Answer | Explanation                                            |
| :------: | :----: | :----------------------------------------------------- |
|    1    |   ②   | Normalization removes duplication and anomalies        |
|    2    |   ④   | Processing Anomaly is not a normalization anomaly      |
|    3    |   ①   | 1NF requires atomic values only                        |
|    4    |   ④   | Function is not ER element                             |
|    5    |   ②   | One professor teaches multiple courses                 |
|    6    |   ①   | course_name depends only on course_id = 2NF violation  |
|    7    |   ③   | Both conditions required                               |
|    8    |   ②   | Foreign Key defines relationship and ensures integrity |
|    9    |   ③   | Normalize first, denormalize only for performance      |
|    10    |   ③   | X determined → Y uniquely determined                  |

---

## Short Answer Model Answers (5 Questions)

### Question 11: Normalization and Anomalies

**Model Answer**:

```
Normalization:
- Systematic decomposition of tables to eliminate anomalies
  and ensure data integrity

Goals:
- Minimize data duplication
- Eliminate anomalies
- Maintain data consistency

Anomalies:

1. Insertion Anomaly
   - Cannot insert new data without unnecessary information
   - Example: Cannot add professor without course assignment

2. Update Anomaly
   - Must update same information in multiple places
   - Example: Professor name change requires updating all course records
   - Risk: Inconsistency if partial update

3. Deletion Anomaly
   - Deleting needed data removes unwanted information
   - Example: Deleting last course removes professor
```

---

### Question 12: Normalization Forms

**Model Answer**:

```
1NF (First Normal Form):
Condition:
- All attribute values are atomic (indivisible)
- No repeated attribute values

Example:
❌ Not normalized:
Students(student_id, phone: [02-123, 010-111])

✅ 1NF:
Students(student_id, phone, type)
001, 02-123, Home
001, 010-111, Mobile

2NF (Second Normal Form):
Condition:
- Satisfies 1NF
- All non-key attributes fully depend on entire primary key
  (not just part of composite key)

Example:
❌ Violates 2NF:
Enrollment(student_id, course_id, course_name, grade)
Problem: course_name depends only on course_id

✅ 2NF:
Enrollment(student_id, course_id, grade)
Course(course_id, course_name)

3NF (Third Normal Form):
Condition:
- Satisfies 2NF
- All non-key attributes directly depend on primary key
  (not transitively through other attributes)

Example:
❌ Violates 3NF:
Student(student_id, department, office)
Problem: office depends on department (student_id → department → office)

✅ 3NF:
Student(student_id, department)
Department(department, office)
```

---

### Question 13: ER Diagram and Cardinality

**Model Answer**:

```
Basic Elements:

1. Entity
   - Definition: Object to store information
   - ER notation: Rectangle
   - Example: Student, Course, Professor

2. Attribute
   - Definition: Characteristic of entity
   - ER notation: Oval
   - Example: student_id, name, department

3. Relationship
   - Definition: Association between entities
   - ER notation: Diamond
   - Example: "Student enrolls in Course"

Cardinality:

1:1 (One-to-One)
Example: Student and Student ID
- One student has exactly one ID
- One ID belongs to one student

1:N (One-to-Many)
Example: Professor and Course
- One professor teaches multiple courses
- Each course taught by one professor

M:N (Many-to-Many)
Example: Student and Course
- One student enrolls in multiple courses
- One course has multiple students
- Requires junction table (Enrollment)
```

---

### Question 14: Functional Dependency

**Model Answer**:

```
Functional Dependency:
- Notation: X → Y (X determines Y)
- Meaning: When X value is determined, Y value is uniquely determined

Types:

1. Complete FD
   - Y depends on entire X
   - Not on part of X
   Example: (student_id, course_id) → grade
            grade requires both attributes

2. Partial FD ⚠️ Violates 2NF
   - Y depends on part of X only
   Example: (student_id, course_id) → course_name
            course_name depends only on course_id (student_id unnecessary)

3. Transitive FD ⚠️ Violates 3NF
   - X → Y and Y → Z, therefore X → Z (indirect)
   Example: student_id → department → office
            student_id indirectly determines office

Normalization by Form:
- 1NF: Atomic values only
- 2NF: Remove partial FD
- 3NF: Remove transitive FD
```

---

### Question 15: Database Design Process and Normalization

**Model Answer**:

```
5-Step Database Design Process:

Step 1: Requirements Analysis
- Gather business requirements
- Collect data
- Interview users

Step 2: Conceptual Design
- Create ER Diagram
- Define entities and relationships
- Design logical structure

Step 3: Logical Design ← APPLY NORMALIZATION HERE!
- Apply normalization levels (1NF → 3NF+)
- Analyze functional dependencies
- Finalize table structures

Step 4: Physical Design
- Determine storage structure
- Design indexes
- Performance optimization

Step 5: Implementation and Verification
- Create tables with DDL
- Validate data integrity
- Set constraints

Normalization Role:
✅ Applied in logical design step
✅ Eliminate anomalies
✅ Ensure data consistency
✅ Improve storage efficiency

Process:
Non-normalized → 1NF → 2NF → 3NF → BCNF
```

---

## Practical Problems Model Answers (5 Questions)

### Question 16: Convert to 1NF

**Completion Criteria**:
✅ Identify non-normalization problem (composite values)
✅ Convert to 1NF (atomic values only)
✅ Create separate phone table

---

### Question 17: Convert to 2NF

**Completion Criteria**:
✅ Identify partial functional dependency
✅ Separate tables (enrollments, courses)
✅ Define foreign keys

---

### Question 18: Convert to 3NF

**Completion Criteria**:
✅ Identify transitive functional dependency
✅ Separate tables (students, departments)
✅ Analyze normalization benefits

---

### Question 19: ER Diagram Implementation

**Completion Criteria**:
✅ 5 table structures defined
✅ Foreign key relationships set
✅ Sample data inserted
✅ JOIN queries executed

---

### Question 20: Complex Design

**Completion Criteria**:
✅ 5-entity ER Diagram
✅ Normalization analysis per step
✅ All constraints applied
✅ Verification queries created

**Model Answer**:

```sql
-- 1. Create Departments
CREATE TABLE departments (
  dept_id INT PRIMARY KEY AUTO_INCREMENT,
  dept_name VARCHAR(50) NOT NULL,
  office VARCHAR(30)
);

-- 2. Create Students
CREATE TABLE students (
  student_id INT PRIMARY KEY AUTO_INCREMENT,
  name VARCHAR(50) NOT NULL,
  dept_id INT NOT NULL,
  FOREIGN KEY (dept_id) REFERENCES departments(dept_id)
);

-- 3. Create Professors
CREATE TABLE professors (
  prof_id INT PRIMARY KEY AUTO_INCREMENT,
  name VARCHAR(50) NOT NULL,
  dept_id INT NOT NULL,
  FOREIGN KEY (dept_id) REFERENCES departments(dept_id)
);

-- 4. Create Courses
CREATE TABLE courses (
  course_id INT PRIMARY KEY AUTO_INCREMENT,
  course_name VARCHAR(50) NOT NULL,
  credit INT,
  prof_id INT NOT NULL,
  dept_id INT NOT NULL,
  FOREIGN KEY (prof_id) REFERENCES professors(prof_id),
  FOREIGN KEY (dept_id) REFERENCES departments(dept_id)
);

-- 5. Create Enrollments (M:N relationship)
CREATE TABLE enrollments (
  enrollment_id INT PRIMARY KEY AUTO_INCREMENT,
  student_id INT NOT NULL,
  course_id INT NOT NULL,
  grade CHAR(1),
  FOREIGN KEY (student_id) REFERENCES students(student_id),
  FOREIGN KEY (course_id) REFERENCES courses(course_id)
);

-- Insert Sample Data
INSERT INTO departments VALUES
(1, 'Computer Science', 'Building 301'),
(2, 'Electronics', 'Building 302'),
(3, 'Business', 'Building 303');

INSERT INTO students VALUES
(1, 'Kim Chulsu', 1),
(2, 'Lee Younghee', 1),
(3, 'Park Minjun', 2);

INSERT INTO professors VALUES
(1, 'Prof. Cho', 1),
(2, 'Prof. Park', 2),
(3, 'Prof. Lee', 1);

INSERT INTO courses VALUES
(101, 'Data Structures', 3, 1, 1),
(102, 'Database', 3, 3, 1),
(103, 'Digital Circuit', 3, 2, 2);

INSERT INTO enrollments VALUES
(1, 1, 101, 'A'),
(2, 1, 102, 'B'),
(3, 2, 101, 'A'),
(4, 2, 102, 'A'),
(5, 3, 103, 'B');

-- Verification Queries
-- 1. Students per department
SELECT d.dept_name, COUNT(s.student_id) AS student_count
FROM departments d
LEFT JOIN students s ON d.dept_id = s.dept_id
GROUP BY d.dept_id;

-- 2. Courses per professor
SELECT p.name, GROUP_CONCAT(c.course_name) AS courses
FROM professors p
LEFT JOIN courses c ON p.prof_id = c.prof_id
GROUP BY p.prof_id;

-- 3. Student enrollments
SELECT s.name, GROUP_CONCAT(c.course_name) AS enrolled_courses
FROM students s
LEFT JOIN enrollments e ON s.student_id = e.student_id
LEFT JOIN courses c ON e.course_id = c.course_id
GROUP BY s.student_id;
```

---

Prof. Jeong-Hyun Cho (peterchokr@gmail.com)
Yeungnam University College
