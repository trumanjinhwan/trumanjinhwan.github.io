---
layout: post
title: "1.RDB Overview"
date: 2025-01-20
---

> 관계형 데이터베이스(Relational Database, RDB)는 현대 데이터 관리의 핵심입니다. 이 글에서는 데이터베이스의 정의, DBMS의 기능, 스키마 구조를 활용 예제와 함께 정리해보겠습니다.

## 1. 데이터베이스의 정의

데이터베이스는 다음 네 가지 특징을 갖는 데이터를 체계적으로 저장하는 시스템입니다:

- **통합 데이터**: 중복을 최소화하고 통합된 구조로 관리됩니다.
- **저장 데이터**: 컴퓨터가 접근 가능하도록 물리적으로 저장됩니다.
- **운영 데이터**: 지속적으로 갱신되고 활용되는 데이터를 의미합니다.
- **공용 데이터**: 여러 사용자와 애플리케이션이 공동으로 이용할 수 있습니다.

### 예제: 간단한 학생 테이블

```sql
CREATE TABLE STUDENT (
    STUDENT_ID INT PRIMARY KEY,
    NAME VARCHAR(50),
    MAJOR VARCHAR(50),
    ENROLLMENT_YEAR INT
);

INSERT INTO STUDENT (STUDENT_ID, NAME, MAJOR, ENROLLMENT_YEAR) VALUES
(1, '김철수', '컴퓨터공학', 2020),
(2, '박영희', '전자공학', 2019),
(3, '이민호', '기계공학', 2021);
```

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

#### 실행 결과
<table>
  <thead>
    <tr>
      <th>STUDENT_ID</th>
      <th>NAME</th>
      <th>MAJOR</th>
      <th>ENROLLMENT_YEAR</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1</td>
      <td>김철수</td>
      <td>컴퓨터공학</td>
      <td>2020</td>
    </tr>
    <tr>
      <td>2</td>
      <td>박영희</td>
      <td>전자공학</td>
      <td>2019</td>
    </tr>
    <tr>
      <td>3</td>
      <td>이민호</td>
      <td>기계공학</td>
      <td>2021</td>
    </tr>
  </tbody>
</table>

---

## 2. DBMS(Database Management System)의 기능

DBMS는 데이터베이스를 관리하기 위한 소프트웨어로, 다음 세 가지 필수 기능을 제공합니다:

### 2.1 정의 기능
테이블의 구조, 데이터 타입을 정의하고 제약조건을 명시하는 기능입니다.

#### 예제: 정의 기능
```sql
CREATE TABLE COURSE (
    COURSE_ID CHAR(6) PRIMARY KEY,
    COURSE_NAME VARCHAR(100),
    CREDITS INT CHECK (CREDITS BETWEEN 1 AND 4)
);
```

#### 실행 결과
<table>
  <thead>
    <tr>
      <th>COURSE_ID</th>
      <th>COURSE_NAME</th>
      <th>CREDITS</th>
    </tr>
  </thead>
</table>

---

### 2.2 조작 기능
데이터를 검색, 삽입, 갱신, 삭제하는 기능입니다.

#### 예제: 조작 기능
```sql
INSERT INTO COURSE (COURSE_ID, COURSE_NAME, CREDITS) VALUES
('CS101', '데이터베이스', 3),
('CS102', '운영체제', 4);

SELECT * FROM COURSE;
```

#### 실행 결과
<table>
  <thead>
    <tr>
      <th>COURSE_ID</th>
      <th>COURSE_NAME</th>
      <th>CREDITS</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>CS101</td>
      <td>데이터베이스</td>
      <td>3</td>
    </tr>
    <tr>
      <td>CS102</td>
      <td>운영체제</td>
      <td>4</td>
    </tr>
  </tbody>
</table>

---

### 2.3 제어 기능
사용자들에 대한 권한과 보안, 데이터의 무결성을 유지하며 동시성을 제어합니다.

#### 예제: 제어 기능
```sql
CREATE USER 'student_user' IDENTIFIED BY 'password';
GRANT SELECT, INSERT ON STUDENT TO 'student_user';
```

---

## 3. 스키마(Schema)
<div style="text-align: center;">
    <img src="/사진들/RDB Overview/RDB Overview1.png" alt="alt text" />
</div>
> 출처: https://prinha.tistory.com/entry/DB-3%EB%8B%A8%EA%B3%84-%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4-%EC%8B%9C%EC%8A%A4%ED%85%9C-%EC%99%B8%EB%B6%80%EA%B0%9C%EB%85%90%EB%82%B4%EB%B6%80%EC%8A%A4%ED%82%A4%EB%A7%88

스키마는 데이터베이스의 구조를 정의하는 틀로, 다음 세 가지로 나뉩니다:

### 3.1 내부 스키마
- 데이터의 물리적 저장 구조를 정의합니다.
- DBMS가 데이터의 물리적 저장소와 상호작용하는 방식을 다룹니다.

### 3.2 개념 스키마
- 데이터베이스의 논리적 구조를 정의합니다.
- 테이블, 속성, 관계 등을 포함합니다.

#### 예제: 개념 스키마
```sql
CREATE TABLE PROFESSOR (
    PROFESSOR_ID INT PRIMARY KEY,
    NAME VARCHAR(50),
    DEPARTMENT VARCHAR(50)
);
```

#### 실행 결과
<table>
  <thead>
    <tr>
      <th>PROFESSOR_ID</th>
      <th>NAME</th>
      <th>DEPARTMENT</th>
    </tr>
  </thead>
</table>

---

### 3.3 외부 스키마
- 사용자 관점에서 데이터베이스를 정의합니다.
- 특정 사용자에게 맞는 데이터를 보여주기 위해 뷰(View)를 생성합니다.

#### 예제: 외부 스키마
```sql
CREATE VIEW STUDENT_VIEW AS
SELECT STUDENT_ID, NAME FROM STUDENT;

SELECT * FROM STUDENT_VIEW;
```

#### 실행 결과
<table>
  <thead>
    <tr>
      <th>STUDENT_ID</th>
      <th>NAME</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1</td>
      <td>김철수</td>
    </tr>
    <tr>
      <td>2</td>
      <td>박영희</td>
    </tr>
    <tr>
      <td>3</td>
      <td>이민호</td>
    </tr>
  </tbody>
</table>

---

## 4. 결론

RDB는 데이터를 체계적으로 저장하고 관리할 수 있습니다. 데이터베이스의 정의, DBMS의 기능, 스키마 구조를 이해하면 더욱 효율적으로 데이터 관리를 할 수 있습니다.
