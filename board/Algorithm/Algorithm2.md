---
layout: post
title: "2. 배열(리스트)"
date: 2025-02-19
---

> 알고리즘을 공부하다 보면 **배열(Array)**과 **리스트(List)**는 필수적으로 이해해야 하는 개념입니다. 언어마다 구현 방식과 사용법이 다르기 때문에, **자바와 파이썬에서의 차이점을 비교**하고, **입력 받는 방식**까지 정리해보겠습니다.

# 1. 자바와 파이썬의 배열과 리스트 비교

배열과 리스트는 기본적으로 **데이터를 연속된 메모리에 저장**하는 자료구조입니다. 하지만 **자바와 파이썬의 차이점**을 살펴보면 몇 가지 중요한 차이점이 있습니다.

## 1.1 인덱스 접근 방식

두 언어 모두 **index**를 사용하여 요소에 접근합니다. 즉, 첫 번째 요소의 인덱스는 `0`입니다.

### 자바 (배열)
```java
int[] arr = {10, 20, 30};
System.out.println(arr[0]); // 출력: 10
```

### 파이썬 (리스트)
```python
arr = [10, 20, 30]
print(arr[0])  # 출력: 10
```

## 1.2 슬라이싱(Slicing) 지원 여부

- **자바**: 배열은 기본적으로 **슬라이싱 기능이 없습니다**. 대신 `Arrays.copyOfRange()`를 사용해야 합니다.
- **파이썬**: 리스트는 기본적으로 **슬라이싱이 가능**합니다.

### 자바 (배열에서 부분 복사)
```java
import java.util.Arrays;

int[] arr = {10, 20, 30, 40, 50};
int[] subArr = Arrays.copyOfRange(arr, 1, 4); // 인덱스 1~3까지 복사
System.out.println(Arrays.toString(subArr)); // 출력: [20, 30, 40]
```

### 파이썬 (리스트 슬라이싱)
```python
arr = [10, 20, 30, 40, 50]
sub_arr = arr[1:4]  # 인덱스 1~3까지 슬라이싱
print(sub_arr)  # 출력: [20, 30, 40]
```

## 1.3 요소 추가 방식

### 자바 (배열)
자바의 배열은 **크기가 고정**되므로, **한 번 생성된 배열의 크기를 변경할 수 없습니다**. 크기를 변경하려면 **새로운 배열을 생성**하고 기존 데이터를 복사해야 합니다. 일반적으로 `ArrayList`를 사용하여 가변 배열을 다룹니다.

```java
import java.util.ArrayList;

ArrayList<Integer> list = new ArrayList<>();
list.add(10);
list.add(20);
list.add(30);
System.out.println(list); // 출력: [10, 20, 30]
```

### 파이썬 (리스트)
파이썬의 리스트는 크기를 동적으로 조절할 수 있어 `append()` 메서드를 사용하면 자유롭게 요소를 추가할 수 있습니다.

```python
arr = []
arr.append(10)
arr.append(20)
arr.append(30)
print(arr)  # 출력: [10, 20, 30]
```

## 1.4 요소의 자료형 제한 여부

- **자바**: 배열 및 `ArrayList`는 **한 가지 자료형만 저장 가능** (제네릭 사용)
- **파이썬**: 리스트는 **자료형이 달라도 저장 가능** (동적 타입 언어)

### 자바 (동일한 자료형만 가능)
```java
ArrayList<Integer> list = new ArrayList<>();
list.add(10);
list.add("str"); // 오류 발생 (타입 불일치)
```

### 파이썬 (다양한 자료형 저장 가능)
```python
arr = [10, "str", 3.14, True]
print(arr)  # 출력: [10, 'str', 3.14, True]
```

---

# 2. 자바와 파이썬에서 입력 받는 방법

## 2.1 한 줄 입력 받기

### 자바
```java
import java.util.Scanner;

Scanner sc = new Scanner(System.in);
String input = sc.nextLine();
System.out.println("입력 값: " + input);
```

### 파이썬
```python
input_value = input()
print("입력 값:", input_value)
```

---

## 2.2 공백을 기준으로 여러 개의 값 입력 받기

### 자바
```java
import java.util.Scanner;
import java.util.Arrays;

Scanner sc = new Scanner(System.in);
String[] input = sc.nextLine().split(" "); // 공백 기준 분할
System.out.println(Arrays.toString(input));
```

### 파이썬
```python
arr = input().split()  # 기본적으로 공백 기준으로 분할
print(arr)
```

---

## 2.3 2차원 배열 입력 받기

### 자바
```java
import java.util.Scanner;

Scanner sc = new Scanner(System.in);
int n = sc.nextInt();  // 행 개수
int m = sc.nextInt();  // 열 개수
int[][] matrix = new int[n][m];

for (int i = 0; i < n; i++) {
    for (int j = 0; j < m; j++) {
        matrix[i][j] = sc.nextInt();
    }
}
```

### 파이썬
```python
n, m = map(int, input().split())  # 행과 열 개수 입력
matrix = [list(map(int, input().split())) for _ in range(n)]
```
```map(int, input().split())```의 결과는 ```map```객체이기 때문에 ```list```로 한번 형변환 해줘야 컴파일 에러가 안남니다.

---

# 정리

| 비교 항목      | 자바(Java)                                          | 파이썬(Python)                                  |
|--------------|--------------------------------------------------|---------------------------------------------|
| **인덱스 접근** | `arr[i]`                                       | `arr[i]`                                  |
| **슬라이싱**   | `Arrays.copyOfRange(arr, start, end)` 사용      | `arr[start:end]` 사용 가능                  |
| **요소 추가**  | `ArrayList` 사용 (`add()`)                      | `append()` 사용                             |
| **자료형 제한** | 한 가지 자료형만 저장 가능 (`ArrayList<Integer>`) | 자료형 상관없이 저장 가능 (`[10, "str", 3.14]`) |
| **단일 입력**  | `sc.nextLine()`                                  | `input()` 사용                             |
| **공백 입력**  | `sc.nextLine().split(" ")`                       | `input().split()` 사용                      |
| **2차원 입력** | 중첩 루프 사용 (`sc.nextInt()`)                  | 리스트 컴프리헨션 사용 <br>(`[list(map(int, input().split())) for _ in range(n)]`) |

자바와 파이썬은 배열과 리스트를 처리하는 방식에서 여러 차이점이 있습니다. 언어의 특징을 잘 파악하고, 문제 해결에 맞는 최적의 방법을 선택하는 것이 중요합니다.

---

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

