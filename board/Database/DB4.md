---
layout: post
title: "4. Primary Key & Foreign Key"
date: 2025-01-21
---

<div style="text-align: center;">
    <img src="/사진들/Primary Key & Foreign Key/Primary Key & Foreign Key.png" alt="alt text" />
</div>

> 관계형 데이터베이스에서 **Primary Key**와 **Foreign Key**는 데이터를 구조화하고 테이블 간의 관계를 정의하는 데 핵심적인 역할을 합니다. 이 글에서는 두 개념의 정의와 특징, 그리고 활용 예제를 실행 결과와 함께 정리해보겠습니다.

## 1. Primary Key

### 정의
- Primary Key는 테이블의 각 레코드를 고유하게 식별하는 데 사용됩니다.
- 하나의 필드 또는 여러 필드(복합키)로 구성될 수 있습니다.
- **```NULL``` 값을 가질 수 없습니다**. (고유 식별 능력 상실 방지)

### 주요 특징
1. **유일성**: 테이블의 각 레코드가 유일하게 식별됩니다.(중복 X)
2. **무결성**: 항상 값이 있어야 합니다.(```NULL```X)
3. **불변성**: 변하는 값이여서는 안됩니다.
4. **존재성**: 반드시 존재해야 합니다. 누구한텐 있고 누구한텐 없어서는 안됩니다.

### 예제: Primary Key 설정
```sql
CREATE TABLE DEPARTMENT (
    DEPARTMENT_ID INT PRIMARY KEY,
    DEPARTMENT_NAME VARCHAR(50)
);

INSERT INTO DEPARTMENT (DEPARTMENT_ID, DEPARTMENT_NAME) VALUES
(1, '개발팀'),
(2, '기획팀'),
(3, '디자인팀');
```

#### 실행 결과
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
      <th>DEPARTMENT_ID</th>
      <th>DEPARTMENT_NAME</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1</td>
      <td>개발팀</td>
    </tr>
    <tr>
      <td>2</td>
      <td>기획팀</td>
    </tr>
    <tr>
      <td>3</td>
      <td>디자인팀</td>
    </tr>
  </tbody>
</table>

---

## 2. Foreign Key

### 정의
- Foreign Key는 한 테이블의 열(Column)이 다른 테이블의 Primary Key를 참조하도록 설정된 키입니다.
- 두 테이블 간의 관계를 정의하며, 데이터 무결성을 보장합니다.

### 주요 특징
1. **참조 무결성**: Foreign Key는 참조하는 Primary Key의 값을 가져야 하며, 잘못된 값을 저장할 수 없습니다.
2. **연결성**: 테이블 간의 관계를 명확히 정의하여 조인(Join) 쿼리를 효율적으로 수행할 수 있습니다.
3. **삭제 및 갱신 규칙**: ON DELETE 및 ON UPDATE 옵션으로 참조 무결성 규칙을 설정할 수 있습니다.

### 예제: Foreign Key 설정
```sql
CREATE TABLE EMPLOYEE (
    EMPLOYEE_ID INT PRIMARY KEY,
    NAME VARCHAR(50),
    DEPARTMENT_ID INT,
    FOREIGN KEY (DEPARTMENT_ID) REFERENCES DEPARTMENT(DEPARTMENT_ID)
);

INSERT INTO EMPLOYEE (EMPLOYEE_ID, NAME, DEPARTMENT_ID) VALUES
(1, '김철수', 1),
(2, '박영희', 2),
(3, '이민호', 1);
```

#### 실행 결과
<table>
  <thead>
    <tr>
      <th>EMPLOYEE_ID</th>
      <th>NAME</th>
      <th>DEPARTMENT_ID</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1</td>
      <td>김철수</td>
      <td>1</td>
    </tr>
    <tr>
      <td>2</td>
      <td>박영희</td>
      <td>2</td>
    </tr>
    <tr>
      <td>3</td>
      <td>이민호</td>
      <td>1</td>
    </tr>
  </tbody>
</table>

---

## 3. 추가 개념 및 활용

### 3.1 복합 Primary Key

복합 Primary Key는 두 개 이상의 열을 조합하여 레코드를 고유하게 식별합니다.

#### 예제: 복합 Primary Key
```sql
CREATE TABLE PROJECT (
    PROJECT_ID INT,
    EMPLOYEE_ID INT,
    ROLE VARCHAR(50),
    PRIMARY KEY (PROJECT_ID, EMPLOYEE_ID),
    FOREIGN KEY (EMPLOYEE_ID) REFERENCES EMPLOYEE(EMPLOYEE_ID)
);

INSERT INTO PROJECT (PROJECT_ID, EMPLOYEE_ID, ROLE) VALUES
(101, 1, '팀장'),
(101, 2, '팀원'),
(102, 3, '팀장');
```

#### 실행 결과
<table>
  <thead>
    <tr>
      <th>PROJECT_ID</th>
      <th>EMPLOYEE_ID</th>
      <th>ROLE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>101</td>
      <td>1</td>
      <td>팀장</td>
    </tr>
    <tr>
      <td>101</td>
      <td>2</td>
      <td>팀원</td>
    </tr>
    <tr>
      <td>102</td>
      <td>3</td>
      <td>팀장</td>
    </tr>
  </tbody>
</table>

### 3.2 자기 참조 Foreign Key

테이블이 자기 자신을 참조할 수 있습니다. 예를 들어, 조직 구조에서 상사와 직원 관계를 정의할 때 사용됩니다. 상사와 직원은 계층(트리) 구조가 되어서 계층형 쿼리를 사용 할 수 있습니다.

#### 예제: 자기 참조 Foreign Key
```sql
CREATE TABLE EMPLOYEE_HIERARCHY (
    EMPLOYEE_ID INT PRIMARY KEY,
    NAME VARCHAR(50),
    MANAGER_ID INT,
    FOREIGN KEY (MANAGER_ID) REFERENCES EMPLOYEE_HIERARCHY(EMPLOYEE_ID)
);

INSERT INTO EMPLOYEE_HIERARCHY (EMPLOYEE_ID, NAME, MANAGER_ID) VALUES
(1, '김철수', NULL),
(2, '박영희', 1),
(3, '이민호', 1);
```

#### 실행 결과
<table>
  <thead>
    <tr>
      <th>EMPLOYEE_ID</th>
      <th>NAME</th>
      <th>MANAGER_ID</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1</td>
      <td>김철수</td>
      <td>NULL</td>
    </tr>
    <tr>
      <td>2</td>
      <td>박영희</td>
      <td>1</td>
    </tr>
    <tr>
      <td>3</td>
      <td>이민호</td>
      <td>1</td>
    </tr>
  </tbody>
</table>

---

## 4. 결론

Primary Key와 Foreign Key는 관계형 데이터베이스의 핵심 구성 요소로, 데이터의 고유성과 무결성을 보장하며 테이블 간의 관계를 명확히 정의합니다. 이를 통해 효율적인 데이터 관리와 분석이 가능해집니다. 복합 Primary Key와 자기 참조 Foreign Key 같은 개념도 필요 시 엔티티 설계시 적극적으로 이용하면 수준 높은 설계가 될 것입니다.

