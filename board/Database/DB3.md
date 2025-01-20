---
layout: post
title: "3. Schema & Instance"
date: 2025-01-20
---

<div style="text-align: center;">
    <img src="/사진들/Schema&Instance/Schema&Instance1.png" alt="alt text" />
</div>

> 스키마(Schema)와 인스턴스(Instance)는 실세계의 데이터들을 RDB로 옮길 때 사용되는 단어들입니다. 이 글에서는 각각의 정의, 특징, 그리고 활용 예제를 실행 결과와 함께 정리해보겠습니다.

## 1. 스키마(Schema)

스키마는 테이블의 정의에 따라 만들어진 데이터 구조를 의미합니다. 스키마는 데이터베이스의 틀로, 테이블의 구조, 속성, 제약 조건 등을 정의합니다. 일반적으로 한 번 정의된 스키마는 거의 변하지 않습니다.

### 주요 개념
- **차수(Degree)**: 테이블에 포함된 속성(Attribute)의 개수를 의미합니다.

### 예제: 스키마 정의

#### 테이블 생성
```sql
CREATE TABLE EMPLOYEE (
    EMPLOYEE_ID INT PRIMARY KEY,
    NAME VARCHAR(50),
    DEPARTMENT VARCHAR(50),
    SALARY INT CHECK (SALARY > 0)
);
```

#### 차수 확인
테이블 EMPLOYEE의 속성 개수:
- ```EMPLOYEE_ID```
- ```NAME```
- ```DEPARTMENT```
- ```SALARY```

차수 = 4

---

## 2. 인스턴스(Instance)

인스턴스는 테이블 스키마에 현실 세계의 데이터를 레코드로 저장한 형태를 의미합니다. 인스턴스는 동적으로 변경될 수 있으며, 레코드의 삽입, 삭제, 수정 등이 발생합니다.

### 주요 개념
- **기수(Cardinality)**: 테이블에 저장된 레코드의 개수를 의미합니다.

### 예제: 인스턴스 생성 및 수정

#### 레코드 삽입
```sql
INSERT INTO EMPLOYEE (EMPLOYEE_ID, NAME, DEPARTMENT, SALARY) VALUES
(1, '김철수', '개발', 5000),
(2, '박영희', '기획', 4500),
(3, '이민호', '디자인', 4000);
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
      <th>EMPLOYEE_ID</th>
      <th>NAME</th>
      <th>DEPARTMENT</th>
      <th>SALARY</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1</td>
      <td>김철수</td>
      <td>개발</td>
      <td>5000</td>
    </tr>
    <tr>
      <td>2</td>
      <td>박영희</td>
      <td>기획</td>
      <td>4500</td>
    </tr>
    <tr>
      <td>3</td>
      <td>이민호</td>
      <td>디자인</td>
      <td>4000</td>
    </tr>
  </tbody>
</table>

기수(Cardinality) = 3

#### 레코드 추가
```sql
INSERT INTO EMPLOYEE (EMPLOYEE_ID, NAME, DEPARTMENT, SALARY) VALUES
(4, '최지훈', '통계', 4700);
```

#### 수정된 실행 결과
<table>
  <thead>
    <tr>
      <th>EMPLOYEE_ID</th>
      <th>NAME</th>
      <th>DEPARTMENT</th>
      <th>SALARY</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1</td>
      <td>김철수</td>
      <td>개발</td>
      <td>5000</td>
    </tr>
    <tr>
      <td>2</td>
      <td>박영희</td>
      <td>기획</td>
      <td>4500</td>
    </tr>
    <tr>
      <td>3</td>
      <td>이민호</td>
      <td>디자인</td>
      <td>4000</td>
    </tr>
    <tr>
      <td>4</td>
      <td>최지훈</td>
      <td>통계</td>
      <td>4700</td>
    </tr>
  </tbody>
</table>

기수(Cardinality) = 4

---

## 3. 스키마와 인스턴스의 차이

| 구분           | 스키마 (Schema)                             | 인스턴스 (Instance)                     |
|----------------|---------------------------------------------|------------------------------------------|
| 정의           | 데이터 구조 및 틀                          | 데이터베이스에 저장된 실제 데이터        |
| 변화 빈도      | 거의 변하지 않음                           | 삽입, 삭제, 수정 등으로 수시로 변경      |
| 예시           | 테이블 정의 (CREATE TABLE)                 | 테이블에 저장된 레코드 (INSERT, UPDATE) |

---

## 4. 결론

스키마와 인스턴스는 데이터베이스 관리에서 중요한 개념으로, 데이터를 체계적으로 관리하는 데 핵심적인 역할을 합니다. 스키마는 데이터의 구조(필드)를 정의하며, 인스턴스는 스키마에 기반한 데이터(레코드)를 저장하고 관리합니다. 스키마의 개수를 차수(Degree)라고 하고 인스턴스의 개수를 기수(Cardinality)라고 합니다.

