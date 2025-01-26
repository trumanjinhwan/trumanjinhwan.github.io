---
layout: post
title: "5. Relational Algebra"
date: 2025-01-22
---
<div style="text-align: center;">
    <img src="/사진들/Relational Algebra/Relational Algebra.png" alt="alt text" />
</div>

> 출처: https://velog.io/@khs0415p/8-%EA%B4%80%EA%B3%84-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EC%97%B0%EC%82%B0


> 관계대수(Relational Algebra)는 관계형 데이터베이스에서 데이터를 검색하거나 조작하기 위한 수학적 이론의 기초가 되는 개념입니다다. 이 글에서는 관계대수의 기본 연산과 추가 연산이 어떤 문법으로 SQL질의어에 활용되었는지를 실행 결과와 함께 정리하겠습니다.

## 1. 관계대수의 기본 연산

### 1.1 선택 연산 (Selection)
- 조건에 맞는 레코드(행)를 선택합니다.
- SQL의 `WHERE` 절에 해당합니다.

#### 예제
```sql
SELECT *
FROM EMPLOYEE
WHERE DEPARTMENT = '개발';
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
      <td>3</td>
      <td>이민호</td>
      <td>개발</td>
      <td>4000</td>
    </tr>
  </tbody>
</table>

---

### 1.2 추출 연산 (Projection)
- 특정 속성(열)만 추출합니다.
- SQL의 `SELECT` 절에 해당합니다.

#### 예제
```sql
SELECT NAME, SALARY
FROM EMPLOYEE;
```

#### 실행 결과
<table>
  <thead>
    <tr>
      <th>NAME</th>
      <th>SALARY</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>김철수</td>
      <td>5000</td>
    </tr>
    <tr>
      <td>박영희</td>
      <td>4500</td>
    </tr>
    <tr>
      <td>이민호</td>
      <td>4000</td>
    </tr>
  </tbody>
</table>

---

### 1.3 재명명 연산 (Renaming)
- 테이블이나 속성에 별칭을 부여합니다.

#### 예제
```sql
SELECT NAME AS EMPLOYEE_NAME, SALARY AS MONTHLY_INCOME
FROM EMPLOYEE;
```

#### 실행 결과
<table>
  <thead>
    <tr>
      <th>EMPLOYEE_NAME</th>
      <th>MONTHLY_INCOME</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>김철수</td>
      <td>5000</td>
    </tr>
    <tr>
      <td>박영희</td>
      <td>4500</td>
    </tr>
    <tr>
      <td>이민호</td>
      <td>4000</td>
    </tr>
  </tbody>
</table>

---

### 1.4 집합 연산 (Set Operations)
- 테이블 간의 연산을 수행합니다.

#### 1.4.1 합집합 (UNION)
- 두 테이블의 모든 행을 합집합으로 반환합니다.

```sql
SELECT NAME FROM EMPLOYEE_A
UNION
SELECT NAME FROM EMPLOYEE_B;
```

#### 1.4.2 교집합 (INTERSECT)
- 두 테이블에 공통으로 존재하는 행을 반환합니다.

```sql
SELECT NAME FROM EMPLOYEE_A
INTERSECT
SELECT NAME FROM EMPLOYEE_B;
```

#### 1.4.3 차집합 (MINUS)
- 첫 번째 테이블에만 있는 행을 반환합니다.

```sql
SELECT NAME FROM EMPLOYEE_A
MINUS
SELECT NAME FROM EMPLOYEE_B;
```

---

### 1.5 카티션 프로덕트 (Cartesian Product)
- 두 테이블 간의 모든 가능한 행 조합을 반환합니다.

```sql
SELECT *
FROM EMPLOYEE
CROSS JOIN DEPARTMENT;
```

---

## 2. 관계대수의 추가 연산

### 2.1 조인 연산 (Join)
#### 세타 조인 (Theta Join)
- 특정 조건에 따라 두 테이블을 결합합니다.

```sql
SELECT *
FROM EMPLOYEE E
JOIN DEPARTMENT D
ON E.DEPARTMENT_ID = D.DEPARTMENT_ID;
```

#### 자연 조인 (Natural Join)
- 공통된 속성을 기준으로 테이블을 결합합니다.

```sql
SELECT *
FROM EMPLOYEE
NATURAL JOIN DEPARTMENT;
```

#### 외부 조인 (Outer Join)
- 조인 조건을 만족하지 않는 행도 포함합니다.

```sql
SELECT *
FROM EMPLOYEE E
LEFT OUTER JOIN DEPARTMENT D
ON E.DEPARTMENT_ID = D.DEPARTMENT_ID;
```

---

### 2.2 지정 연산
- 중간 결과에 이름을 부여하거나 쿼리를 분리합니다.

#### 예제
```sql
WITH TEMP AS (
    SELECT NAME, SALARY
    FROM EMPLOYEE
    WHERE SALARY > 4500
)
SELECT *
FROM TEMP;
```

---

### 2.3 나누기 연산 (Division)
- 테이블 A에서 테이블 B에 있는 모든 조건을 만족하는 행만 반환합니다.

#### 예제
```sql
SELECT STUDENT_ID
FROM ENROLLMENT
WHERE COURSE_ID IN (
    SELECT COURSE_ID
    FROM REQUIRED_COURSES
);
```

---

## 3. 결론

관계대수는 관계형 데이터베이스에서 데이터를 조작하거나 검색하는 데 필수적인 개념입니다. 기본 연산(선택, 추출, 재명명 등)과 추가 연산(조인, 나누기 등)을 이해하면 SQL을 정의, 조작하는 데에 더 많은 이해를 도울 수 있습니다.

