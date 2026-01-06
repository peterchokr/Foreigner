# Chapter 4: WHERE Clause and Conditional Expressions

---

## 📋 Course Overview

**Course Topic**: Data Filtering Using WHERE Clause | WHERE 절을 이용한 데이터 필터링

**Course Objectives**

- Perfect understanding of comparison operators and logical operators | 비교 연산자 및 논리 연산자 완벽 이해
- Master use of IN, BETWEEN, LIKE operators | IN, BETWEEN, LIKE 연산자 활용
- Learn methods to handle NULL values | NULL 값 처리 방법 습득
- Develop ability to write complex conditional expressions | 복잡한 조건식 작성 능력

---

## 📚 Part 1: Theoretical Learning

### What You'll Learn in This Section

In this section, you will learn various methods of filtering data using the WHERE clause. You will study comparison operators, logical operators, IN, BETWEEN, LIKE, and other operators, and develop the ability to write complex conditional expressions. You will also understand how to handle NULL values to perform accurate data queries.

| 이 섹션에서는 WHERE 절을 사용하여 데이터를 필터링하는 다양한 방법을 배웁니다. 비교 연산자, 논리 연산자, IN, BETWEEN, LIKE 등의 연산자를 학습하고, 복잡한 조건식을 작성하는 능력을 기릅니다. 또한 NULL 값의 처리 방법을 이해하여 정확한 데이터 조회를 할 수 있게 됩니다.

### 1-1. Comparison Operators

```
= (equal): SELECT * FROM customer WHERE grade = 'Gold';
!= or <> (not equal): SELECT * FROM customer WHERE city != 'Seoul';
< (less than): SELECT * FROM customer WHERE age < 30;
> (greater than): SELECT * FROM customer WHERE age > 25;
<= (less than or equal): SELECT * FROM customer WHERE salary <= 5000000;
>= (greater than or equal): SELECT * FROM customer WHERE age >= 20;
```

### 1-2. Logical Operators

```
AND (all conditions satisfied): WHERE city = 'Seoul' AND age > 25;
OR (at least one satisfied): WHERE city = 'Seoul' OR city = 'Busan';
NOT (opposite condition): WHERE NOT city = 'Seoul';
```

### 1-3. IN, BETWEEN, LIKE, IS NULL

```
IN: WHERE grade IN ('Gold', 'Silver');
BETWEEN: WHERE age BETWEEN 20 AND 30;
LIKE: WHERE name LIKE 'Kim%';
IS NULL: WHERE address IS NULL;
```

---

## 📚 Part 2: Sample Data

### What You'll Learn in This Section

In this section, you will create a customer table for WHERE clause practice and insert various customer data. Based on real business customer information, this allows you to practice various cases of condition filtering.

| 이 섹션에서는 WHERE 절 실습에 사용할 customer 테이블을 생성하고 다양한 고객 데이터를 삽입합니다. 실제 비즈니스 상황의 고객 정보를 기반으로 설계되어, 조건 필터링의 다양한 사례를 실습할 수 있습니다.

```sql
CREATE DATABASE ch4_filtering CHARACTER SET utf8mb4;
USE ch4_filtering;

CREATE TABLE customer (
    customer_id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(30) NOT NULL,
    city VARCHAR(20),
    age INT,
    grade VARCHAR(10),
    salary INT,
    phone VARCHAR(15)
) CHARACTER SET utf8mb4;

INSERT INTO customer VALUES
(1, 'Kim Chulsu', 'Seoul', 35, 'Gold', 5000000, '010-1111-1111'),
(2, 'Lee Younghee', 'Busan', 28, 'Silver', 3500000, '010-2222-2222'),
(3, 'Park Boyoung', 'Seoul', 42, 'Gold', 6000000, '010-3333-3333'),
(4, 'Choi Minji', 'Daegu', 25, 'Silver', 3000000, '010-4444-4444'),
(5, 'Jung Junho', 'Seoul', 38, 'Platinum', 7500000, NULL);
```

---

## 💻 Part 3: Practice

### What You'll Learn in This Section

In this section, you will actually apply the various conditions of the WHERE clause learned to filter data. From single conditions to complex conditions, you will be able to use WHERE clauses freely through various examples. You will also understand the priority of conditional expressions and develop the ability to write efficient queries.

| 이 섹션에서는 배운 WHERE 절의 다양한 조건을 실제로 적용하여 데이터를 필터링합니다. 단일 조건부터 복합 조건까지 다양한 예제를 통해 WHERE 절을 자유롭게 사용할 수 있게 됩니다. 또한 조건식의 우선순위를 이해하고, 효율적인 쿼리를 작성하는 능력을 기르게 됩니다.

### 3-1. Comparison Operator Practice

**Practice 4-1~4-10: Basic Comparison Operators**

```sql
1. Query customers residing in Seoul
SELECT * FROM customer WHERE city = 'Seoul';

2. Query customers 30 years or older
SELECT * FROM customer WHERE age >= 30;

3. Query Gold grade customers
SELECT * FROM customer WHERE grade = 'Gold';

4. Query customers under 35 years old
SELECT * FROM customer WHERE age < 35;

5. Query customers with salary 4000000 or higher
SELECT * FROM customer WHERE salary >= 4000000;

6. Query customers not from Busan
SELECT * FROM customer WHERE city != 'Busan';

7. Query customers other than Kim Chulsu
SELECT * FROM customer WHERE name <> 'Kim Chulsu';

8. Query customers exactly 28 years old
SELECT * FROM customer WHERE age = 28;

9. Query customers with salary 5000000 or lower
SELECT * FROM customer WHERE salary <= 5000000;

10. Query customers with grade other than Silver
SELECT * FROM customer WHERE grade != 'Silver';
```

### 3-2. Logical Operator Practice

**Practice 4-11~4-20: AND, OR, NOT Operators**

```sql
11. Query customers from Seoul who are 30 or older
SELECT * FROM customer WHERE city = 'Seoul' AND age >= 30;

12. Query Gold grade customers with salary 5000000 or higher
SELECT * FROM customer WHERE grade = 'Gold' AND salary >= 5000000;

13. Query customers from Seoul or Busan
SELECT * FROM customer WHERE city = 'Seoul' OR city = 'Busan';

14. Query Silver or Gold grade customers
SELECT * FROM customer WHERE grade = 'Silver' OR grade = 'Gold';

15. Query customers 30 or older and under 50
SELECT * FROM customer WHERE age >= 30 AND age < 50;

16. Query customers from Daegu or Platinum grade
SELECT * FROM customer WHERE city = 'Daegu' OR grade = 'Platinum';

17. Query customers not from Seoul
SELECT * FROM customer WHERE NOT city = 'Seoul';

18. Query customers with salary 4000000 or higher and Silver grade
SELECT * FROM customer WHERE salary >= 4000000 AND grade = 'Silver';

19. Query customers 35 or older who live in Seoul
SELECT * FROM customer WHERE age >= 35 AND city = 'Seoul';

20. Query Gold grade customers with salary 6000000 or higher
SELECT * FROM customer WHERE grade = 'Gold' AND salary >= 6000000;
```

### 3-3. IN, BETWEEN, LIKE Practice

**Practice 4-21~4-30: IN, BETWEEN, LIKE Operators**

```sql
21. Query customers from Seoul, Busan, or Daegu
SELECT * FROM customer WHERE city IN ('Seoul', 'Busan', 'Daegu');

22. Query Gold or Platinum grade customers
SELECT * FROM customer WHERE grade IN ('Gold', 'Platinum');

23. Query customers between 25 and 35 years old
SELECT * FROM customer WHERE age BETWEEN 25 AND 35;

24. Query customers with salary between 3000000 and 5000000
SELECT * FROM customer WHERE salary BETWEEN 3000000 AND 5000000;

25. Query customers whose names start with 'Kim'
SELECT * FROM customer WHERE name LIKE 'Kim%';

26. Query customers whose names contain 'Young'
SELECT * FROM customer WHERE name LIKE '%Young%';

27. Query customers whose names end with 'ho'
SELECT * FROM customer WHERE name LIKE '%ho';

28. Query customers with phone numbers starting with '010-4'
SELECT * FROM customer WHERE phone LIKE '010-4%';

29. Query customers from Seoul or Busan with Gold grade
SELECT * FROM customer WHERE city IN ('Seoul', 'Busan') AND grade = 'Gold';

30. Query customers 30-45 years old with salary 4000000 or higher
SELECT * FROM customer WHERE age BETWEEN 30 AND 45 AND salary >= 4000000;
```

### 3-4. NULL Handling and Complex Conditions

**Practice 4-31~4-40: NULL Handling and Complex Conditions**

```sql
31. Query customers with registered phone numbers
SELECT * FROM customer WHERE phone IS NOT NULL;

32. Query customers without phone numbers
SELECT * FROM customer WHERE phone IS NULL;

33. Query customers from Seoul or Platinum grade
SELECT * FROM customer WHERE city = 'Seoul' OR grade = 'Platinum';

34. Query customers with salary less than 3500000
SELECT * FROM customer WHERE salary < 3500000;

35. Query customers with salary greater than 5000000
SELECT * FROM customer WHERE salary > 5000000;

36. Query customers 25-40 years old with phone numbers
SELECT * FROM customer WHERE age BETWEEN 25 AND 40 AND phone IS NOT NULL;

37. Query Silver or Gold grade customers with salary 4000000 or higher
SELECT * FROM customer WHERE grade IN ('Silver', 'Gold') AND salary >= 4000000;

38. Query customers from Daegu who are 30 or older
SELECT * FROM customer WHERE city = 'Daegu' AND age >= 30;

39. Query customers whose names contain 'Ji' and are 30 or older
SELECT * FROM customer WHERE name LIKE '%Ji%' AND age >= 30;

40. Query customers with Platinum grade or salary 6000000 or higher
SELECT * FROM customer WHERE grade = 'Platinum' OR salary >= 6000000;
```

---

## 📝 Part 4: Assignment Instructions

### Theoretical Assignments

**Assignment 1**: Explain the concepts and priority of AND and OR operators and explain with specific examples why parentheses must be used correctly when writing complex conditional expressions. | AND와 OR 연산자의 개념과 우선순위를 설명하고, 복합 조건식을 작성할 때 괄호를 올바르게 사용해야 하는 이유를 구체적인 예시를 들어 설명하세요.

**Assignment 2**: Explain the differences between IN operator and OR operator and discuss the readability improvement effect when using IN. | IN 연산자와 OR 연산자의 차이점을 설명하고, IN을 사용했을 때의 가독성 개선 효과를 논의하세요.

**Assignment 3**: Explain the differences between wildcard characters % and _ in LIKE operator and present practical examples using each. | LIKE 연산자의 와일드카드 문자인 %와 _의 차이를 설명하고, 각각을 사용하는 실무 예시를 제시하세요.

**Assignment 4**: Explain what NULL values are and clearly describe the differences between IS NULL and = NULL. | NULL 값이 무엇인지 설명하고, IS NULL과 = NULL의 차이점을 명확히 서술하세요.

**Assignment 5**: Explain how to compose conditional expressions considering performance when writing complex WHERE clauses and present principles for efficient query writing. | 복합 WHERE 절을 작성할 때 성능을 고려한 조건식 구성 방법을 설명하고, 효율적인 쿼리 작성의 원칙을 제시하세요.

Submission Format: Word or PDF document (1-2 pages)

---

### Practical Assignments

**Assignment 1**: Write a SQL statement to query customers residing in Seoul who are 30 or older from the customer table and present the results. | customer 테이블에서 서울 거주하면서 나이가 30세 이상인 고객을 조회하는 SQL 문을 작성하고 결과를 제시하세요.

**Assignment 2**: Write a query to query Gold grade customers with salary 4000000 or higher. | 월급이 4000000원 이상인 Gold 등급 고객을 조회하는 쿼리를 작성하세요.

**Assignment 3**: Write a SQL statement to query customers without phone numbers. | 휴대폰 번호가 없는 고객을 조회하는 SQL 문을 작성하세요.

**Assignment 4**: Query customers from Busan or Daegu using the IN operator. | 부산 또는 대구 거주 고객을 IN 연산자를 사용하여 조회하세요.

**Assignment 5**: Execute all queries provided from Practice 4-1 to 4-40 in Part 3 and attach screenshots of each query result. | Part 3의 실습 4-1부터 4-40까지 제공된 모든 쿼리를 직접 실행하고, 각 쿼리의 결과를 스크린샷으로 첨부하세요.

Submission Format: SQL file (Ch4_WHERE_Practice_[StudentID].sql) and screenshots

---

Thank you for your hard work.

Prof. Cho Jeong-Hyun (peterchokr@gmail.com). Yeungnam University College
