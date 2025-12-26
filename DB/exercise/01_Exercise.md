# Ch1 Database Overview - Exercise Problems

Dear students! After completing Chapter 1, please solve the following exercises to verify your understanding.
Check the difficulty level of each section and study step by step.

---

## 📌 Learning Objectives Verification

After completing Chapter 1, you should understand:

- The difference between data and information
- The difference between file systems and databases
- Characteristics of RDBMS and MySQL
- MySQL installation and basic commands

---

# Multiple Choice Questions (10 Questions)

## Beginner Level (5 Questions) - Basic Concept Verification

**Question 1** Which of the following corresponds to "information"?

- ① Student ID: 20240001
- ② Name: Kim Chulsu, Student ID: 20240001, Enrollment Year: 2024
- ③ Grade Score: 85
- ④ Course Code: CS101

---

**Question 2** Which category does MySQL belong to?

- ① File system
- ② Relational Database (RDBMS)
- ③ Flat file database
- ④ NoSQL database

---

**Question 3** Which of the following is NOT a role of DBMS?

- ① Data definition (schema design)
- ② Data manipulation (insert, update, delete)
- ③ Hardware manufacturing
- ④ Data security (access control)

---

**Question 4** What is the biggest problem with file systems?

- ① Slow execution speed
- ② Possibility of data inconsistency due to duplication
- ③ Complex installation
- ④ English language only support

---

**Question 5** What does NULL value mean?

- ① 0 (the number zero)
- ② Empty string ('')
- ③ Value does not exist or is unknown
- ④ False

---

## Intermediate Level (3 Questions) - Concept Application

**Question 6** In the following scenario, why is a database better than a file system?

"A university student information system exists. There are 1000 students,
and students take multiple courses. When a student's name changes,
it must be reflected everywhere in the system."

- ① Only faster
- ② Centralized management allows modification in one place to be reflected everywhere
- ③ Provides a prettier interface
- ④ Takes up more capacity so it's safer

---

**Question 7** Which is the most inappropriate characteristic of RDBMS?

- ① Table-based 2D structure
- ② Table relationships defined by foreign keys
- ③ Uses SQL as a standard language
- ④ Can only store hierarchical data

---

**Question 8** Which is NOT a reason why MySQL is a standard database for web applications?

- ① Open source and free
- ② Part of LAMP stack
- ③ Provides the fastest performance
- ④ Simple installation and operation

---

## Advanced Level (2 Questions) - Critical Thinking

**Question 9** Select all advantages of database normalization.
(Multiple choice)

- ① Eliminates data redundancy
- ② Reduces storage space
- ③ Makes all query speeds faster
- ④ Prevents anomalies (insertion, update, deletion)

---

**Question 10** In the following situation, what is the most appropriate data storage method?

"A hospital information system. It contains patient information, doctor information,
prescription information, and medical records. Data accuracy and consistency are very important.
Also, only specific doctors can view specific patients' medical records."

- ① Use Excel files
- ② Store in text files
- ③ Relational Database (RDBMS)
- ④ Cloud note pad

---

# Short Answer Questions (5 Questions)

## Beginner Level (3 Questions)

**Question 11** Explain the difference between "data" and "information" in your own words. (3-5 lines)

---

**Question 12** List 3 commands to check first after installing MySQL and explain the purpose of each.

---

**Question 13** Describe 2 or more problems that could occur when managing student information with a file system.

---

## Intermediate Level (1 Question)

**Question 14** Explain the importance of "data security" in DBMS using real-life examples.

---

## Advanced Level (1 Question)

**Question 15** Compare the advantages and disadvantages of normalized and non-normalized databases.
Explain when normalization should be prioritized and when slight denormalization can be considered.

---

# Practical Problems (5 Questions)

## Beginner Level (2 Questions)

**Question 16** Run MySQL Workbench and perform the following, then provide a screenshot.

```
Step 1: Execute the following commands and verify the results
SELECT VERSION();
SELECT USER();

Verification checklist:
- Does the MySQL version appear?
- Is the current user information displayed?

Submission: One screenshot showing the above results
```

---

**Question 17** Create the ch1_mydata database and product table again, and perform the following.

```sql
1. Create database
CREATE DATABASE ch1_practice CHARACTER SET utf8mb4;
USE ch1_practice;

2. Create product table
CREATE TABLE product (
    product_id INT PRIMARY KEY AUTO_INCREMENT,
    product_name VARCHAR(50) NOT NULL,
    price INT NOT NULL,
    stock INT NOT NULL
);

3. Verify table structure with DESC command
DESC product;

4. Insert 3 product records
INSERT INTO product VALUES
(1, 'Wireless Mouse', 45000, 100),
(2, 'Mechanical Keyboard', 120000, 50),
(3, 'Monitor Arm', 65000, 75);

5. Query data with SELECT and take a screenshot
SELECT * FROM product;

Submission: One screenshot showing both table structure and data
```

---

## Intermediate Level (2 Questions)

**Question 18** Analyze the following situation and present a database design.

```
Situation: An online bookstore sells books.
Required information:
- Books: book ID, title, author, price, inventory
- Customers: customer ID, name, email, address
- Orders: order ID, customer ID, order date, total price
- Order Details: order ID, book ID, quantity, unit price

Questions:
1. How many tables are needed?
2. Explain the relationships between each table.
3. Explain why normalization is necessary.

Submission: Answers to the above questions (3-5 lines)
```

---

**Question 19** Modify the product table as follows and provide a screenshot.

```sql
1. Current product table
DESC product;

2. Add category column with ALTER TABLE
ALTER TABLE product ADD category VARCHAR(30);

3. Verify modified product table structure
DESC product;

4. Update existing data
UPDATE product SET category = 'Electronics' WHERE product_id IN (1, 2, 3);

5. Verify final data
SELECT * FROM product;

Submission: One screenshot showing modified table structure and data
```

---

## Advanced Level (1 Question)

**Question 20** Identify problems in your designed database and improve it.

```
Situation: You designed a student information system with a single table.

student_info table:
- student_id, name, email, phone
- course_id, course_name, professor_name, grade, semester

Problems:
1. If a student takes 3 courses, student information is repeated 3 times
2. When professor name changes, must update all course records
3. Students without courses cannot be entered into the database

Questions:
1. What are the exact names of these problems? (types of anomalies)
2. How would you redesign the database? 
   (table separation and relationship definition)
3. What are 2 or more advantages of the improved design?

Submission: Detailed answers to the above questions
```

---

---

# 📋 Answer Key and Model Answers

## Multiple Choice Answers (10 Questions)

| Question | Answer | Explanation                                                                    |
| :------: | :----: | :----------------------------------------------------------------------------- |
|    1    |   ②   | Processed meaningful data = information                                        |
|    2    |   ②   | MySQL is a representative RDBMS                                                |
|    3    |   ③   | DBMS is software, not hardware manufacturing                                   |
|    4    |   ②   | Data duplication → consistency problems during updates                        |
|    5    |   ③   | NULL represents a state where value doesn't exist                              |
|    6    |   ②   | Normalization eliminates duplication, one update reflects everywhere           |
|    7    |   ④   | RDBMS can express various types of relationships                               |
|    8    |   ③   | Advantage of MySQL but not 'fastest' performance                               |
|    9    | ①②④ | ③ is incorrect: normalization may slow queries due to JOINs                   |
|    10    |   ③   | Data integrity + security + relationship representation needed = RDBMS optimal |

---

## Short Answer Model Answers (5 Questions)

### Question 11 Difference between "Data" and "Information"

**Model Answer**:

- Data: Raw, unprocessed material (numbers, characters, etc.)
- Information: Meaningful processing result of data
- Example: 85 (data) → "Kim Chulsu scored 85 on the database exam" (information)

---

### Question 12 Three MySQL Verification Commands

**Model Answer**:

```
1. SELECT VERSION(); 
   Purpose: Check MySQL version

2. SELECT USER();   
   Purpose: Check current user

3. SELECT DATABASE(); 
   Purpose: Check currently selected database
```

---

### Question 13 Problems with File Systems

**Model Answer** (3 examples):

1. **Modification Anomaly**: Student name stored in multiple files requires updating all → consistency issues
2. **Deletion Anomaly**: Deleting last course also deletes student information → data loss
3. **Insertion Anomaly**: Students without courses cannot be entered → difficult new student registration

---

### Question 14 Importance of Data Security

**Model Answer** (Real-life examples):

- **Hospitals**: Only doctors can view patient medical records (protect patient privacy)
- **Banks**: Employees can only see account holder's accounts (protect asset information)
- **Schools**: Students can only view their own grades (protect personal information)
- **Conclusion**: DBMS controls this automatically, making it very important (legal compliance, reliability)

---

### Question 15 Normalized vs Non-normalized Database Comparison

**Model Answer**:

**Normalized Database**

- Advantages: Eliminates data duplication, ensures consistency, reduces storage space
- Disadvantages: Possible performance degradation due to JOINs

**Non-normalized Database**

- Advantages: Fast query performance (no JOINs)
- Disadvantages: Data duplication, consistency problems during updates

**When to prioritize normalization**

- When data accuracy is critical
- Example: Finance (banking), Healthcare (hospitals), Administration (government)

**When to consider denormalization**

- When reads are frequent and speed is critical
- Example: Analysis, reports, bulk queries

---

## Practical Model Answers (5 Questions)

### Question 16 MySQL Installation Verification

**Completion Criteria**:
✅ SELECT VERSION() executed → MySQL version displayed
✅ SELECT USER() executed → root@localhost displayed
✅ Both commands successful → Installation complete

**Screenshot Content**:

- MySQL Workbench query window
- Executed commands and results

---

### Question 17 Create product Table and Insert Data

**Completion Criteria**:
✅ ch1_practice database created
✅ product table created (4 columns)
✅ DESC product results shown: product_id, product_name, price, stock
✅ 3 product records inserted
✅ SELECT * FROM product results verified

**Screenshot Content**:

- Table structure (DESC result)
- 3 rows of inserted data

**Expected Result**:

```
product_id | product_name      | price  | stock
1          | Wireless Mouse    | 45000  | 100
2          | Mechanical Keyboard | 120000 | 50
3          | Monitor Arm       | 65000  | 75
```

---

### Question 18 Online Bookstore Database Design

**Model Answer**:

**1. Number of Tables Needed**

- 4 tables (Books, Customers, Orders, Order Details)

**2. Relationships Between Tables**

```
Customer (1) ──────→ (N) Order
                       │
                       ↓
Order Details (N) ←────┘
    │
    └────→ (1) Book
```

- One customer has multiple orders (1:N)
- One order contains multiple products (1:N)
- One book can be in multiple orders (1:N)

**3. Why Normalization is Necessary**

- Eliminates book information duplication (same book not repeated in multiple orders)
- Book price changes only require single update (ensures consistency)
- Reduces storage space

---

### Question 19 Modify product Table with ALTER TABLE

**Completion Criteria**:
✅ ALTER TABLE product ADD category VARCHAR(30); executed
✅ DESC product result: 5 columns shown (product_id, product_name, price, stock, category)
✅ Category values added via UPDATE
✅ SELECT * FROM product final result verified

**Screenshot Content**:

- Modified table structure
- Data with category column added

**Expected Result**:

```
product_id | product_name      | price  | stock | category
1          | Wireless Mouse    | 45000  | 100   | Electronics
2          | Mechanical Keyboard | 120000 | 50    | Electronics
3          | Monitor Arm       | 65000  | 75    | Electronics
```

---

### Question 20 Improve Table Design

**Model Answer**:

**1. Names of Problems (Types of Anomalies)**

- **Repeating Group**: Student information repeated as many times as courses
- **Modification Anomaly**: Professor name change requires updates in all records
- **Insertion Anomaly**: Cannot register students without courses

**2. Improved Redesign (Normalization)**

Original Table (Problematic):

```
student_info (student_id, name, email, phone, course_id, course_name, professor_name, grade, semester)
```

Improved Design (4 Tables):

```
student (student_id, name, email, phone)
course (course_id, course_name, professor_id)
professor (professor_id, professor_name)
enrollment (student_id, course_id, grade, semester)
```

Relationship Diagram:

```
student (1) ────────→ (N) enrollment ←────── (1) course
                          │                      │
                          └─ grade, semester     │
                                              professor
```

**3. Advantages of Improved Design**

- **Eliminates data duplication**: Student information stored only once
- **Ensures consistency**: Professor name update only requires modification in professor table
- **Enables easy insertion**: Students without courses can be registered in student table immediately
- **Reduces storage space**: Repeating information eliminated for efficiency
- **Simplifies maintenance**: Each table has clear purpose

---

Prof.  Jeong-Hyun Cho (peterchokr@gmail.com)
Yeungnam University College
