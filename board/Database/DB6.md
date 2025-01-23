---
layout: post
title: "6. Normalization & Denormalization"
date: 2025-01-23
---

> 데이터베이스 설계에서 정규화(Normalization)와 반정규화(Denormalization)는 효율적인 데이터 저장과 성능 최적화를 위해 필수적인 개념입니다. 이 글에서는 정규화와 반정규화의 정의와 각각의 단계를 예제와 함께 설명하겠습니다.

## 1. 정규화 (Normalization)

### 정의
- 데이터 중복을 최소화하여 데이터의 무결성을 유지하고 **갱신 이상(Update Anomaly)**을 제거하기 위한 데이터베이스 설계 과정입니다.

### 주요 단계

#### 1.1 1정규화 (1NF)
- **도메인이 원자값(Atomic Value)**을 가져야 합니다.
- 테이블의 각 열에는 하나의 값만 저장됩니다.

#### 예제: 1정규화 적용 전
<style>
  table {
    width: 100%;
    border-collapse: collapse;
    margin: 20px 0;
  }

  th, td {
    border: 2px solid #333;
    padding: 12px;
    text-align: center;
  }

  th {
    background-color: #f4f4f4;
    font-weight: bold;
  }

  td {
    background-color: #fafafa;
  }

  table th, table td {
    border: 1px solid #ddd;
  }
</style>

<table>
  <thead>
    <tr>
      <th>EMPLOYEE_ID</th>
      <th>NAME</th>
      <th>PHONE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1</td>
      <td>김철수</td>
      <td>010-1234-5678, 010-5678-1234</td>
    </tr>
  </tbody>
</table>

#### 1정규화 적용 후
```sql
CREATE TABLE EMPLOYEE (
    EMPLOYEE_ID INT,
    NAME VARCHAR(50),
    PHONE VARCHAR(20)
);

INSERT INTO EMPLOYEE (EMPLOYEE_ID, NAME, PHONE) VALUES
(1, '김철수', '010-1234-5678'),
(1, '김철수', '010-5678-1234');
```

<table>
  <thead>
    <tr>
      <th>EMPLOYEE_ID</th>
      <th>NAME</th>
      <th>PHONE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1</td>
      <td>김철수</td>
      <td>010-1234-5678</td>
    </tr>
    <tr>
      <td>2</td>
      <td>김철수</td>
      <td>010-5678-1234</td>
    </tr>
  </tbody>
</table>

---

#### 1.2 2정규화 (2NF)
- **부분 함수 종속성**을 제거합니다.
- 기본키의 일부가 아닌 전체 기본키에 종속된 속성만 테이블에 남깁니다.

#### 예제: 2정규화 적용 전
<table>
  <thead>
    <tr>
      <th>COURSE_ID</th>
      <th>STUDENT_ID</th>
      <th>COURSE_NAME</th>
      <th>GRADE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>101</td>
      <td>1</td>
      <td>데이터베이스</td>
      <td>A+</td>
    </tr>
  </tbody>
</table>

#### 2정규화 적용 후
```sql
CREATE TABLE COURSE (
    COURSE_ID INT PRIMARY KEY,
    COURSE_NAME VARCHAR(50)
);

CREATE TABLE ENROLLMENT (
    COURSE_ID INT,
    STUDENT_ID INT,
    GRADE VARCHAR(10),
    PRIMARY KEY (COURSE_ID, STUDENT_ID)
);

INSERT INTO COURSE (COURSE_ID, COURSE_NAME) VALUES
(101, '데이터베이스');

INSERT INTO ENROLLMENT (COURSE_ID, STUDENT_ID, GRADE) VALUES
(101, 1, 'A+');
```

---

#### 1.3 3정규화 (3NF)
- **이행적 함수 종속성**을 제거합니다.
- 비기본 속성이 다른 비기본 속성에 종속되지 않도록 설계합니다.

#### 예제: 3정규화 적용 전
<table>
  <thead>
    <tr>
      <th>EMPLOYEE_ID</th>
      <th>DEPARTMENT_ID</th>
      <th>DEPARTMENT_NAME</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1</td>
      <td>10</td>
      <td>개발팀</td>
    </tr>
  </tbody>
</table>

#### 3정규화 적용 후
```sql
CREATE TABLE DEPARTMENT (
    DEPARTMENT_ID INT PRIMARY KEY,
    DEPARTMENT_NAME VARCHAR(50)
);

CREATE TABLE EMPLOYEE (
    EMPLOYEE_ID INT PRIMARY KEY,
    DEPARTMENT_ID INT,
    FOREIGN KEY (DEPARTMENT_ID) REFERENCES DEPARTMENT(DEPARTMENT_ID)
);

INSERT INTO DEPARTMENT (DEPARTMENT_ID, DEPARTMENT_NAME) VALUES
(10, '개발팀');

INSERT INTO EMPLOYEE (EMPLOYEE_ID, DEPARTMENT_ID) VALUES
(1, 10);
```

---

## 2. 반정규화 (Denormalization)

### 정의
- 데이터 중복을 감수하고 성능을 향상시키기 위해 정규화된 데이터 구조를 의도적으로 해체하는 과정입니다.

### 주요 사례

#### 2.1 중복 속성
- 특정 속성을 다른 테이블에도 중복 저장하여 조회 속도를 향상시킵니다.

#### 예제
```sql
ALTER TABLE EMPLOYEE
ADD DEPARTMENT_NAME VARCHAR(50);

UPDATE EMPLOYEE E
SET E.DEPARTMENT_NAME = (
    SELECT D.DEPARTMENT_NAME
    FROM DEPARTMENT D
    WHERE E.DEPARTMENT_ID = D.DEPARTMENT_ID
);
```

---

#### 2.2 테이블 통합
- 빈번히 조인되는 테이블을 하나로 합칩니다.

#### 예제
```sql
CREATE TABLE EMPLOYEE_WITH_DEPARTMENT AS
SELECT E.EMPLOYEE_ID, E.NAME, D.DEPARTMENT_NAME
FROM EMPLOYEE E
JOIN DEPARTMENT D
ON E.DEPARTMENT_ID = D.DEPARTMENT_ID;
```

---

#### 2.3 테이블 분할
- 특정 조건에 따라 테이블을 수평(행 기반) 또는 수직(열 기반)으로 분리합니다.

##### 수평 분할 예제
```sql
CREATE TABLE EMPLOYEE_DEV AS
SELECT *
FROM EMPLOYEE
WHERE DEPARTMENT_ID = 10;

CREATE TABLE EMPLOYEE_MGMT AS
SELECT *
FROM EMPLOYEE
WHERE DEPARTMENT_ID != 10;
```

##### 수직 분할 예제
```sql
CREATE TABLE EMPLOYEE_BASIC AS
SELECT EMPLOYEE_ID, NAME
FROM EMPLOYEE;

CREATE TABLE EMPLOYEE_SALARY AS
SELECT EMPLOYEE_ID, SALARY
FROM EMPLOYEE;
```

---

## 3. 결론

정규화는 데이터의 무결성을 유지하고 중복을 최소화하는 데 중점을 둡니다. 반대로, 반정규화는 성능 향상을 위해 데이터 중복을 허용하는 설계 방식입니다. 데이터베이스 설계 시 요구사항에 따라 정규화와 반정규화를 적절히 활용하면 안정적이고 효율적인 시스템을 구축할 수 있습니다.

