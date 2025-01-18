---
layout: post
title: 1. Relation(Table)
date: 2025-01-18
---

> 데이터베이스의 기본 개념인 릴레이션(Relation)은 2차원 테이블 구조로 표현됩니다. 이 글에서는 릴레이션의 주요 요소와 각 요소를 활용한 예제를 정리해보겠습니다.

## 1. 릴레이션의 개념

릴레이션은 2차원 테이블 구조로, 데이터를 행(Row)과 열(Column)의 형태로 저장합니다. 즉, 행은 개별 데이터 항목을 나타내고, 열은 데이터의 속성을 정의합니다. 예를 들어, 학생 정보를 저장하는 테이블에서는 각 행이 한 명의 학생을, 각 열이 학생의 속성(이름, 나이 등)을 나타냅니다. 주요 구성 요소는 다음과 같습니다:

- **속성(Attribute), 필드(Field), 컬럼(Column)**: 테이블의 열을 나타내며, 데이터의 특성을 정의합니다. 속성은 데이터의 유효성과 무결성을 보장하는 데 중요한 역할을 합니다. 예를 들어, 특정 속성에 데이터 타입을 지정하거나 제약 조건을 설정함으로써 잘못된 데이터 입력을 방지할 수 있습니다.
- **튜플(Tuple), 레코드(Record), 행(Row)**: 테이블의 한 행을 나타내며, 하나의 데이터 항목을 의미합니다.
- **도메인(Domain)**: 각 속성에 허용되는 값의 범위를 정의합니다. 예를 들어, 정수형(int), 문자열(varchar2), 날짜형(date) 등이 있습니다.
- **널(Null)**: 값이 존재하지 않음을 의미합니다.

---

## 2. 릴레이션의 주요 요소와 예제

### 2.1 속성(Attribute), 필드(Field), 컬럼(Column)

속성은 테이블의 각 열(Column)을 의미하며, 데이터의 유형과 특성을 정의합니다.

#### 예제
```sql
CREATE TABLE STUDENT (
    STUDENTID INT PRIMARY KEY,
    NAME VARCHAR(50),
    MAJOR VARCHAR(50),
    ENROLLMENTYEAR INT
);
```

이 테이블의 속성은 다음과 같습니다:
- STUDENTID: 정수형(INT)
- NAME: 문자열(VARCHAR)
- MAJOR: 문자열(VARCHAR)
- ENROLLMENTYEAR: 정수형(INT)

---

### 2.2 튜플(Tuple), 레코드(Record), 행(Row)

튜플은 테이블의 각 행(Row)을 의미하며, 하나의 데이터 항목을 표현합니다.

#### 예제
```sql
INSERT INTO STUDENT (STUDENTID, NAME, MAJOR, ENROLLMENTYEAR) VALUES
(1, '김철수', '컴퓨터공학', 2020),
(2, '박영희', '전자공학', 2019),
(3, '이민호', '기계공학', 2021);
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
      <th>STUDENTID</th>
      <th>NAME</th>
      <th>MAJOR</th>
      <th>ENROLLMENTYEAR</th>
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

### 2.3 도메인(Domain)

도메인은 각 속성에 허용되는 값의 범위를 정의합니다. 도메인은 데이터 무결성을 보장하는 데 중요한 역할을 합니다. 예를 들어, 도메인을 통해 속성에 특정 데이터 타입이나 값의 범위를 강제하면 잘못된 데이터 입력을 방지할 수 있습니다. 예를 들어, "EnrollmentYear" 속성의 도메인은 2000~2025 사이의 정수로 제한할 수 있습니다.

#### 예제
```sql
CREATE TABLE COURSE (
    COURSECODE CHAR(6) PRIMARY KEY,
    COURSENAME VARCHAR(100),
    CREDITS INT CHECK (CREDITS BETWEEN 1 AND 4)
);

INSERT INTO COURSE (COURSECODE, COURSENAME, CREDITS) VALUES
('CS101', '데이터베이스', 3),
('CS102', '운영체제', 4);
```

#### 실행 결과
<table>
  <thead>
    <tr>
      <th>COURSECODE</th>
      <th>COURSENAME</th>
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

### 2.4 널(Null)

널은 값이 존재하지 않음을 나타냅니다. 널 값은 데이터베이스 쿼리나 분석에서 예상치 못한 결과를 초래할 수 있습니다. 예를 들어, 널 값은 수학적 연산에서 무시되거나, 집계 함수에서 결과를 왜곡할 수 있으므로 이를 적절히 처리하는 것이 중요합니다. 예를 들어, 학생이 아직 전공을 선택하지 않았다면 해당 열에 `NULL` 값을 저장할 수 있습니다.

#### 예제
```sql
INSERT INTO STUDENT (STUDENTID, NAME, MAJOR, ENROLLMENTYEAR) VALUES
(4, '최지훈', NULL, 2022);
```

#### 실행 결과
<table>
  <thead>
    <tr>
      <th>STUDENTID</th>
      <th>NAME</th>
      <th>MAJOR</th>
      <th>ENROLLMENTYEAR</th>
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
    <tr>
      <td>4</td>
      <td>최지훈</td>
      <td>NULL</td>
      <td>2022</td>
    </tr>
  </tbody>
</table>

---

## 3. 릴레이션의 활용 사례

### 학생들의 전공별 인원 수 구하기
```sql
SELECT MAJOR, COUNT(*) AS MAJORCOUNT
FROM STUDENT
GROUP BY MAJOR;
```

#### 실행 결과
<table>
  <thead>
    <tr>
      <th>MAJOR</th>
      <th>MAJORCOUNT</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>컴퓨터공학</td>
      <td>1</td>
    </tr>
    <tr>
      <td>전자공학</td>
      <td>1</td>
    </tr>
    <tr>
      <td>기계공학</td>
      <td>1</td>
    </tr>
    <tr>
      <td>NULL</td>
      <td>1</td>
    </tr>
  </tbody>
</table>

### 특정 입학년도 이후 학생 검색
```sql
SELECT *
FROM STUDENT
WHERE ENROLLMENTYEAR > 2020;
```

#### 실행 결과
<table>
  <thead>
    <tr>
      <th>STUDENTID</th>
      <th>NAME</th>
      <th>MAJOR</th>
      <th>ENROLLMENTYEAR</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>3</td>
      <td>이민호</td>
      <td>기계공학</td>
      <td>2021</td>
    </tr>
    <tr>
      <td>4</td>
      <td>최지훈</td>
      <td>NULL</td>
      <td>2022</td>
    </tr>
  </tbody>
</table>

---

## 4. 결론

릴레이션은 데이터베이스의 핵심 개념으로, 데이터를 체계적으로 저장하고 관리할 수 있게 합니다. 특히, 릴레이션은 관계형 데이터 모델에서 2차원 테이블 구조를 사용함으로써 데이터를 직관적으로 표현하고, 중복을 최소화하며, 효율적인 쿼리와 데이터 분석을 가능하게 합니다. 속성, 튜플, 도메인, 널의 개념을 이해하고 이를 활용하면 복잡한 데이터 구조도 효율적으로 설계할 수 있습니다.

