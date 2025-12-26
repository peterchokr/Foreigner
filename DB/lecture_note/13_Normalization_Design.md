# Chapter 13: Normalization and Database Design

---

## 📖 Course Overview

In this chapter, you will learn Normalization, the foundation of efficient and integral database design. Normalization is a process that minimizes data redundancy and prevents anomalies to maintain data consistency. This chapter covers normalization stages from 1NF to 3NF and BCNF, database design methods through ER diagrams, and normalization application in practice. The goal is to develop the ability to design actual databases based on theoretical foundations. 

| 이 장에서는 효율적이고 무결성 있는 데이터베이스 설계의 기초인 정규화(Normalization)를 학습합니다. 정규화는 데이터의 중복을 최소화하고 이상 현상(Anomaly)을 방지하여 데이터의 일관성을 유지하는 과정입니다. 1차 정규형부터 3차 정규형, BCNF까지의 정규화 단계와 ER 다이어그램을 통한 데이터베이스 설계 방법, 그리고 실무에서의 정규화 적용을 다룹니다. 이론적 기초를 바탕으로 실제 데이터베이스를 설계할 수 있는 능력을 개발하는 것이 목표입니다.

---

## 📚 Part 1: Theoretical Learning

### What You'll Learn in This Section

- Normalization concept and objectives | 정규화의 개념과 목표
- Functional Dependency | 함수 종속성 (Functional Dependency)
- First Normal Form (1NF) | 1차 정규형 (1NF)
- Second Normal Form (2NF) | 2차 정규형 (2NF)
- Third Normal Form (3NF) | 3차 정규형 (3NF)
- Boyce-Codd Normal Form (BCNF) | BCNF (Boyce-Codd Normal Form)
- ER diagrams and design principles | ER 다이어그램 및 설계 원칙
- Denormalization considerations | 비정규화 (De-normalization) 고려사항

---

### 13.1 Normalization Concept

**Normalization** is a process that systematically decomposes tables to eliminate anomalies and ensure data integrity in databases. | **정규화**는 데이터베이스의 이상 현상을 제거하고 데이터 무결성을 보장하기 위해 테이블을 체계적으로 분해하는 과정입니다.

**Objectives:**

- Minimize data redundancy | 데이터 중복 최소화
- Eliminate anomalies (insertion, update, deletion) | 이상 현상 제거 (삽입, 수정, 삭제 이상)
- Maintain data integrity | 데이터 무결성 유지
- Improve storage space efficiency | 저장 공간 효율성

**Anomalies:**

1. **Insertion Anomaly:** Must insert unnecessary information when adding new data | 새로운 데이터 삽입 시 불필요한 정보도 함께 삽입해야 함
2. **Update Anomaly:** Must modify multiple rows with same information | 데이터 수정 시 같은 정보의 여러 행을 수정해야 함
3. **Deletion Anomaly:** Unwanted information deleted when deleting needed data | 필요한 데이터를 삭제할 때 원하지 않는 정보까지 삭제됨

---

### 13.2 Functional Dependency

**Functional Dependency** represents dependency relationships between attributes. | **함수 종속성**은 속성 간의 종속 관계를 나타냅니다.

**Notation:** X → Y (When X is determined, Y is uniquely determined) | X → Y (X가 결정되면 Y도 유일하게 결정됨)

**Example:**

- StudentID → StudentName, Major, Year
- EmployeeID → EmployeeName, Department, Salary

**Full Functional Dependency:** Y depends on entire X (not on part of X) | Y가 X 전체에 종속 (X의 일부에는 종속되지 않음)

**Partial Functional Dependency:** Y depends on only part of X (undesirable) | Y가 X의 일부에만 종속 (불바람직)

**Transitive Dependency:** If X → Y and Y → Z, then X → Z (goal of 3NF to eliminate) | X → Y, Y → Z이면 X → Z (이를 제거하는 것이 3NF의 목표)

---

### 13.3 First Normal Form (1NF)

**1NF Requirement:**

- All attribute values are atomic values (cannot be further decomposed) | 모든 속성 값이 원자값(Atomic Value, 더 이상 분해할 수 없는 값)

**Incorrect Example:**

```
| StudentID | StudentName | PhoneNumber (home, mobile) |
|-----------|-------------|---------------------------|
| 001       | Kim Chulsu  | 02-1234-5678, 010-1111-2222 |
```

**Normalized Example:**

```
| StudentID | StudentName | PhoneNumber      | PhoneType |
|-----------|-------------|------------------|-----------|
| 001       | Kim Chulsu  | 02-1234-5678     | Home      |
| 001       | Kim Chulsu  | 010-1111-2222    | Mobile    |
```

---

### 13.4 Second Normal Form (2NF)

**2NF Requirements:**

- Satisfies 1NF | 1NF를 만족
- All non-key attributes are fully functionally dependent on the entire primary key | 모든 비주요 속성이 기본키 전체에 완전 함수 종속

**Composite Key Table Example:**

Incorrect example with partial dependency:

```
| StudentID | CourseID | CourseName | Credits | Grade |
|-----------|----------|-----------|---------|-------|
| 001       | C001     | Database  | 3       | A     |
```

Here, CourseName depends only on CourseID (partial dependency - undesirable).

Correct separation:

**Courses Table:**

```
| CourseID | CourseName | Credits |
|----------|-----------|---------|
| C001     | Database  | 3       |
```

**Enrollments Table:**

```
| StudentID | CourseID | Grade |
|-----------|----------|-------|
| 001       | C001     | A     |
```

---

### 13.5 Third Normal Form (3NF)

**3NF Requirements:**

- Satisfies 2NF | 2NF를 만족
- No non-key attribute is transitively dependent on the primary key | 어떤 비주요 속성도 기본키에 이행 함수 종속되지 않음

Remove transitive dependencies:

**Incorrect Example:**

```
| EmployeeID | EmployeeName | DepartmentID | DepartmentName |
|-----------|--------------|-------------|----------------|
| 001       | Kim Chulsu   | D01         | Sales          |
```

Here, DepartmentName depends on DepartmentID which depends on EmployeeID (transitive - undesirable).

**Correct Separation:**

**Employees Table:**

```
| EmployeeID | EmployeeName | DepartmentID |
|-----------|--------------|-------------|
| 001       | Kim Chulsu   | D01         |
```

**Departments Table:**

```
| DepartmentID | DepartmentName |
|-------------|----------------|
| D01         | Sales          |
```

---

### 13.6 Boyce-Codd Normal Form (BCNF)

**BCNF Requirement:**

- For every functional dependency X → Y, X must be a candidate key | 모든 함수 종속성 X → Y에서 X는 후보키여야 함

BCNF is more strict than 3NF and eliminates all anomalies related to functional dependencies. | BCNF는 3NF보다 더 엄격하며 함수 종속성과 관련된 모든 이상 현상을 제거합니다.

---

### 13.7 ER Diagram and Design Principles

**ER Diagram Components:**

- **Entity:** Object represented in database | 데이터베이스에서 표현되는 객체
- **Attribute:** Properties of entity | 엔티티의 속성
- **Relationship:** Association between entities | 엔티티 간의 관계
- **Cardinality:** 1:1, 1:N, N:M relationships | 1:1, 1:N, N:M 관계

**Design Principles:**

1. Identify all entities | 모든 엔티티 파악
2. Define attributes for each entity | 각 엔티티의 속성 정의
3. Identify primary keys | 기본키 식별
4. Define relationships between entities | 엔티티 간의 관계 정의
5. Normalize tables according to NF requirements | NF 요구사항에 따라 테이블 정규화
6. Create referential integrity constraints | 참조 무결성 제약조건 생성

---

### 13.8 Denormalization Considerations

**When to Consider Denormalization:**

- Performance optimization for read-heavy queries | 읽기 위주 쿼리의 성능 최적화
- Reduced number of joins needed | 필요한 조인 수 감소
- Trading normalization for query speed | 정규화를 포기하고 쿼리 속도 향상

**Trade-offs:**

- Increased data redundancy | 데이터 중복 증가
- More complex update logic | 복잡한 수정 로직
- Risk of data inconsistency | 데이터 불일치의 위험

---

## 📚 Part 2: Database Design Process

### Step-by-Step Design Process

1. **Requirement Analysis** | 요구사항 분석

   - Identify business requirements | 비즈니스 요구사항 파악
   - Understand data flows | 데이터 흐름 이해
2. **Conceptual Design** | 개념적 설계

   - Create ER diagram | ER 다이어그램 작성
   - Define entities and relationships | 엔티티와 관계 정의
3. **Logical Design** | 논리적 설계

   - Create normalized tables | 정규화된 테이블 생성

## 💻 Part 3: Practice

### What You'll Learn in This Section

- 정규화 단계의 실제 적용
- 이상 현상의 식별과 해결
- ER 다이어그램 작성
- 데이터베이스 설계 실습

---

### 13-1. 1NF 식별

비정규형 데이터를 1NF로 변환하세요.

**문제 테이블:**

```sql
-- 잘못된 형태 (전화번호가 원자값이 아님)
students (학번, 이름, 전화번호)
001, 김철수, 02-123-4567, 010-1111-2222
```

**해결 (1NF):**

```sql
CREATE TABLE students (
    student_id VARCHAR(5),
    name VARCHAR(50)
);

CREATE TABLE phone_numbers (
    student_id VARCHAR(5),
    phone_number VARCHAR(20)
);
```

---

### 13-2. 2NF 변환

1NF 데이터를 2NF로 변환하세요.

**문제 (부분 함수 종속):**

```sql
-- 수강 (학번, 강의번호, 강의명, 학점)
-- 강의명과 학점은 강의번호에만 종속
```

**해결:**

```sql
CREATE TABLE enrollment (
    student_id VARCHAR(5),
    course_id VARCHAR(5),
    grade CHAR(1)
);

CREATE TABLE courses (
    course_id VARCHAR(5),
    course_name VARCHAR(50),
    credits INT
);
```

---

### 13-3. 3NF 변환

2NF 데이터를 3NF로 변환하세요.

**문제 (이행 함수 종속):**

```sql
-- 학생 (학번, 이름, 학과, 학과장)
-- 학과장은 학과에 종속
```

**해결:**

```sql
CREATE TABLE students (
    student_id VARCHAR(5),
    name VARCHAR(50),
    major_id INT
);

CREATE TABLE majors (
    major_id INT,
    major_name VARCHAR(50),
    chairman VARCHAR(50)
);
```

---

### 13-4. BCNF 확인

데이터가 BCNF를 만족하는지 확인하세요.

```sql
-- 교수 (교수id, 과목, 시간)
-- 문제: 과목 -> 교수는 아니지만, 과목 -> 시간

-- BCNF 형태로 변환:
CREATE TABLE professor_assignment (
    professor_id INT,
    course_id INT,
    PRIMARY KEY (professor_id, course_id)
);

CREATE TABLE course_schedule (
    course_id INT,
    time_slot VARCHAR(20),
    PRIMARY KEY (course_id, time_slot)
);
```

---

### 13-5. 함수 종속성 식별

테이블에서 함수 종속성을 찾아내세요.

```sql
-- 직원 테이블의 함수 종속성:
-- 직원ID -> 이름, 부서, 직급
-- 부서 -> 부서명, 위치
-- 직급 -> 급여범위

CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    name VARCHAR(50),
    dept_id INT,
    job_id INT
);

CREATE TABLE departments (
    dept_id INT PRIMARY KEY,
    dept_name VARCHAR(50),
    location VARCHAR(50)
);

CREATE TABLE jobs (
    job_id INT PRIMARY KEY,
    job_title VARCHAR(50),
    min_salary DECIMAL(10, 2),
    max_salary DECIMAL(10, 2)
);
```

---

### 13-6. 부분 함수 종속 제거

부분 함수 종속을 제거하여 2NF로 변환하세요.

**문제:**

```sql
-- 수강 (학번, 강의번호, 강의명, 점수)
-- 강의명은 강의번호에만 종속 (부분 함수 종속)
```

**해결:**

```sql
CREATE TABLE enrollments (
    student_id INT,
    course_id INT,
    grade DECIMAL(3, 2),
    PRIMARY KEY (student_id, course_id)
);

CREATE TABLE courses (
    course_id INT PRIMARY KEY,
    course_name VARCHAR(50)
);
```

---

### 13-7. 이행 함수 종속 제거

이행 함수 종속을 제거하여 3NF로 변환하세요.

**문제:**

```sql
-- 학생 (학번, 이름, 학과, 학과장)
-- 학번 -> 학과, 학과 -> 학과장 (이행 함수 종속)
```

**해결:**

```sql
CREATE TABLE students (
    student_id INT PRIMARY KEY,
    name VARCHAR(50),
    major_id INT,
    FOREIGN KEY (major_id) REFERENCES majors(major_id)
);

CREATE TABLE majors (
    major_id INT PRIMARY KEY,
    major_name VARCHAR(50),
    chairman_id INT,
    FOREIGN KEY (chairman_id) REFERENCES professors(professor_id)
);
```

---

### 13-8. 복합키 테이블 설계

복합키를 가지는 정규화된 테이블을 설계하세요.

```sql
CREATE TABLE course_enrollment (
    student_id INT,
    course_id INT,
    semester VARCHAR(10),
    grade CHAR(1),
    PRIMARY KEY (student_id, course_id, semester),
    FOREIGN KEY (student_id) REFERENCES students(student_id),
    FOREIGN KEY (course_id) REFERENCES courses(course_id)
);
```

---

### 13-9. 외래키 관계 설정

테이블 간 외래키 관계를 정의하세요.

```sql
CREATE TABLE customers (
    customer_id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(50),
    email VARCHAR(50)
);

CREATE TABLE orders (
    order_id INT PRIMARY KEY AUTO_INCREMENT,
    customer_id INT NOT NULL,
    order_date DATE,
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);

CREATE TABLE order_items (
    order_item_id INT PRIMARY KEY AUTO_INCREMENT,
    order_id INT NOT NULL,
    product_id INT NOT NULL,
    quantity INT,
    FOREIGN KEY (order_id) REFERENCES orders(order_id),
    FOREIGN KEY (product_id) REFERENCES products(product_id)
);
```

---

### 13-10. 참조 무결성 유지

외래키 제약조건이 참조 무결성을 보장하는지 확인하세요.

```sql
-- 올바른 INSERT
INSERT INTO orders VALUES (1, 1, '2024-01-15');

-- 오류: customer_id 99가 존재하지 않음
INSERT INTO orders VALUES (2, 99, '2024-01-16'); -- 실패

-- 참조 무결성 확인
SELECT * FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id;
```

---

### 13-11. 다대다 관계 처리

M:N 관계를 정규화하여 처리하세요.

**문제:**

```sql
-- 학생이 여러 과목을 수강하고, 과목마다 여러 학생이 수강
```

**해결:**

```sql
CREATE TABLE students (
    student_id INT PRIMARY KEY,
    name VARCHAR(50)
);

CREATE TABLE courses (
    course_id INT PRIMARY KEY,
    course_name VARCHAR(50)
);

CREATE TABLE enrollments (
    student_id INT,
    course_id INT,
    grade CHAR(1),
    PRIMARY KEY (student_id, course_id),
    FOREIGN KEY (student_id) REFERENCES students(student_id),
    FOREIGN KEY (course_id) REFERENCES courses(course_id)
);
```

---

### 13-12. ER 다이어그램 해석

주어진 ER 다이어그램으로부터 테이블 구조를 도출하세요.

```
[Customer] 1:N [Order]
[Order] 1:N [OrderItem]
[Product] 1:N [OrderItem]
```

**DDL:**

```sql
CREATE TABLE customers (...);
CREATE TABLE orders (customer_id INT REFERENCES customers);
CREATE TABLE products (...);
CREATE TABLE order_items (order_id INT REFERENCES orders, product_id INT REFERENCES products);
```

---

### 13-13. ER 다이어그램 작성

온라인 쇼핑몰의 ER 다이어그램 및 테이블 구조를 설계하세요.

```
엔티티: Customer, Product, Category, Order, OrderItem, Inventory
관계: Customer 1:N Order
      Product N:1 Category
      Product 1:N Inventory
      Order 1:N OrderItem
      Product 1:N OrderItem
```

---

### 13-14. 삽입 이상 식별

정규화되지 않은 테이블의 삽입 이상을 식별하세요.

**문제:**

```sql
-- 학생 (학번, 이름, 학과, 학과장, 학과위치)
-- 새로운 학과 정보만 추가하려면 학번을 입력해야 함 (삽입 이상)
```

---

### 13-15. 삽입 이상 해결

정규화를 통해 삽입 이상을 제거하세요.

**해결:**

```sql
CREATE TABLE students (학번, 이름, 학과ID);
CREATE TABLE majors (학과ID, 학과장, 위치);
-- 이제 학과 정보만 독립적으로 추가 가능
```

---

### 13-16. 수정 이상 식별

수정 이상의 사례를 찾으세요.

**문제:**

```sql
-- 과목 (학번, 강의번호, 강의명, 강사, 학점)
-- 강의명을 변경하려면 모든 수강 학생 행을 수정해야 함 (수정 이상)
```

---

### 13-17. 수정 이상 해결

정규화로 수정 이상을 제거하세요.

```sql
CREATE TABLE courses (강의번호, 강의명, 강사, 학점);
CREATE TABLE enrollments (학번, 강의번호);
-- 이제 강의명 변경 시 courses 테이블만 수정
```

---

### 13-18. 삭제 이상 식별

삭제 이상의 사례를 찾으세요.

**문제:**

```sql
-- 직원 (직원ID, 이름, 부서, 부서장)
-- 마지막 직원을 삭제하면 부서 정보도 사라짐 (삭제 이상)
```

---

### 13-19. 삭제 이상 해결

정규화로 삭제 이상을 제거하세요.

```sql
CREATE TABLE employees (emp_id, name, dept_id);
CREATE TABLE departments (dept_id, dept_name, manager_id);
-- 이제 부서 정보는 별도로 유지 가능
```

---

### 13-20. 성능 vs 정규화

정규화된 스키마와 비정규화된 스키마의 성능을 분석하세요.

**정규화 (쓰기 빠름, 읽기 느림):**

```sql
SELECT e.name, d.dept_name
FROM employees e
JOIN departments d ON e.dept_id = d.dept_id;
```

**비정규화 (쓰기 느림, 읽기 빠름):**

```sql
SELECT name, dept_name FROM employees;
-- dept_name이 employees에 중복 저장
```

---

### 13-21. 의도적 비정규화

성능을 위해 의도적으로 비정규화하세요.

```sql
-- 읽기가 매우 빈번한 경우
ALTER TABLE orders
ADD COLUMN customer_name VARCHAR(50);

-- INSERT/UPDATE 시 customer_name도 함께 수정해야 함
```

---

### 13-22. 온라인 쇼핑몰 설계

상품, 고객, 주문 정보를 정규화하여 설계하세요.

```sql
CREATE TABLE categories (category_id INT, category_name VARCHAR(50));
CREATE TABLE products (product_id INT, name VARCHAR(50), category_id INT, price DECIMAL);
CREATE TABLE customers (customer_id INT, name VARCHAR(50), email VARCHAR(50), city VARCHAR(50));
CREATE TABLE orders (order_id INT, customer_id INT, order_date DATE);
CREATE TABLE order_items (order_item_id INT, order_id INT, product_id INT, quantity INT, unit_price DECIMAL);
```

---

### 13-23. 대학 정보 시스템 설계

학생, 강의, 수강 정보를 정규화하여 설계하세요.

```sql
CREATE TABLE departments (dept_id INT PRIMARY KEY, dept_name VARCHAR(50));
CREATE TABLE students (student_id INT PRIMARY KEY, name VARCHAR(50), dept_id INT);
CREATE TABLE courses (course_id INT PRIMARY KEY, course_name VARCHAR(50), credits INT);
CREATE TABLE enrollments (student_id INT, course_id INT, grade CHAR(1));
CREATE TABLE professors (prof_id INT PRIMARY KEY, name VARCHAR(50));
CREATE TABLE course_instructors (course_id INT, prof_id INT);
```

---

### 13-24. 병원 관리 시스템 설계

환자, 의료진, 진료 정보를 정규화하세요.

```sql
CREATE TABLE patients (patient_id INT, name VARCHAR(50), medical_record_number VARCHAR(20));
CREATE TABLE doctors (doctor_id INT, name VARCHAR(50), specialty VARCHAR(50));
CREATE TABLE visits (visit_id INT, patient_id INT, doctor_id INT, visit_date DATE);
CREATE TABLE diagnoses (diagnosis_id INT, visit_id INT, diagnosis_description VARCHAR(255));
CREATE TABLE prescriptions (prescription_id INT, visit_id INT, medication VARCHAR(50), dosage VARCHAR(50));
```

---

### 13-25. 도서관 관리 시스템 설계

도서, 회원, 대출 정보를 정규화하세요.

```sql
CREATE TABLE members (member_id INT, name VARCHAR(50), join_date DATE);
CREATE TABLE categories (category_id INT, category_name VARCHAR(50));
CREATE TABLE books (book_id INT, title VARCHAR(100), author VARCHAR(50), category_id INT);
CREATE TABLE loans (loan_id INT, member_id INT, book_id INT, loan_date DATE, return_date DATE);
```

---

## 📝 Part 4: Assignment Instructions

### Theoretical Assignments

**Assignment 1**: Explain normalization concept and objectives. What anomalies does normalization prevent? | 정규화의 개념과 목표를 설명하세요. 정규화가 어떤 이상 현상을 방지하는가요?

**Assignment 2**: Explain 1NF, 2NF, 3NF, and BCNF. Provide examples of each normalization form. | 1NF, 2NF, 3NF, BCNF를 설명하세요. 각 정규형의 예시를 제시하세요.

**Assignment 3**: Explain functional dependencies and how they relate to different normal forms. | 함수 종속성을 설명하고 각 정규형과의 관계를 설명하세요.

**Assignment 4**: Explain ER diagram components and how to create ER diagrams. | ER 다이어그램의 구성요소와 작성 방법을 설명하세요.

**Assignment 5**: Discuss when denormalization might be appropriate and associated tradeoffs. | 비정규화가 적절할 때가 언제인지와 트레이드오프를 논의하세요.

Submission Format: Word or PDF document (3-4 pages)

---

### Practical Assignments

**Assignment 1**: Analyze unnormalized tables and identify insertion, update, and deletion anomalies. | 비정규화 테이블을 분석하고 삽입, 수정, 삭제 이상 현상을 식별하세요.

**Assignment 2**: Normalize tables to 1NF, 2NF, and 3NF. Show step-by-step transformation. | 테이블을 1NF, 2NF, 3NF로 정규화하세요. 단계별 변환을 보여주세요.

**Assignment 3**: Create ER diagrams for 5 different business scenarios (e-commerce, hospital, university, etc.). | 5가지 다른 비즈니스 시나리오 (전자상거래, 병원, 대학교 등)에 대한 ER 다이어그램을 만드세요.

**Assignment 4**: Convert ER diagrams to relational schema with proper primary and foreign keys. | ER 다이어그램을 적절한 기본키와 외래키를 포함한 관계 스키마로 변환하세요.

**Assignment 5**: Execute all Practice 13-1 to 13-40 queries and design complete normalized database. Attach ER diagrams and SQL schema. | 실습 13-1부터 13-40까지 모두 수행하고 완전한 정규화 데이터베이스를 설계하세요. ER 다이어그램과 SQL 스키마를 첨부하세요.

Submission Format: Design document (Ch13_Database_Design_[StudentID].pdf) with ER diagrams and SQL scripts

---

Thank you for your hard work.

Prof. Cho Jeong-Hyun (peterchokr@gmail.com). Yeungnam University College
