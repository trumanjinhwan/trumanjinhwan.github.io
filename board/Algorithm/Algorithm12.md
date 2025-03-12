---
layout: post
title: "12. 유클리드 호제법"
date: 2025-03-12
---

## 유클리드 호제법

**유클리드 호제법(Euclidean Algorithm)**은 두 수의 **최대공약수(GCD, Greatest Common Divisor)**를 빠르게 구하는 알고리즘입니다.

### 원리
유클리드 호제법의 기본 원리는 다음과 같습니다:

1. **큰 수를 작은 수로 나눈 나머지**를 구합니다.
2. 앞 단계에서 사용한 **작은 수**와 **나머지 값**으로 다시 나눗셈을 수행합니다.
3. 나머지가 `0`이 될 때까지 반복하며, 마지막에 나누는 수가 **최대공약수(GCD)**가 됩니다.


## 백준 1934번 - 최소공배수 문제

### 문제 설명

두 개의 자연수 A와 B가 주어질 때, A와 B의 **최소공배수(LCM, Least Common Multiple)**를 구하는 문제입니다.

- 입력: 첫 번째 줄에 테스트 케이스의 개수 `T`가 주어집니다. 다음 `T`개의 줄에는 두 정수 `A`와 `B`가 주어집니다.
- 출력: 각 테스트 케이스에 대해 A와 B의 최소공배수를 한 줄씩 출력합니다.


### 해결 방법

최소공배수(LCM)는 두 수 A, B의 곱을 **최대공약수(GCD)**로 나눈 값으로 구할 수 있습니다.

\[
\text{LCM}(A, B) = \frac{A \times B}{\text{GCD}(A, B)}
\]

유클리드 호제법을 활용해 최대공약수를 구한 뒤, 최소공배수를 계산하면 빠르게 문제를 해결할 수 있습니다.


## 코드 구현

```python
import sys

# 최대공약수(GCD) - 유클리드 호제법
def GCD(x: int, y: int):
    big = max(x, y)
    small = min(x, y)
    mod = big % small

    if mod == 0:
        return small
    else:
        return GCD(small, mod)

# 입력 받기
t = int(sys.stdin.readline().strip())

for _ in range(t):
    a, b = map(int, sys.stdin.readline().split())
    gcd = GCD(a, b)
    lcm = (a * b) // gcd
    print(lcm)
```


## 코드 설명 (유클리드 호제법 적용 과정)

### 1. 최대공약수(GCD) 계산 (`GCD` 함수)
```python
def GCD(x: int, y: int):
    big = max(x, y)  # 두 수 중 큰 값 선택
    small = min(x, y)  # 작은 값 선택
    mod = big % small  # 나머지 계산

    if mod == 0:
        return small  # 나머지가 0이면 작은 수가 최대공약수
    else:
        return GCD(small, mod)  # 나머지를 작은 수로 삼아 재귀 호출
```
- **1단계**: `big % small`을 수행하여 나머지를 구합니다.
- **2단계**: 나머지가 `0`이 아니면 재귀 호출을 수행합니다.
- **3단계**: 나머지가 `0`이면, `small`이 최대공약수가 됩니다.


### 2. 최소공배수(LCM) 계산
```python
lcm = (a * b) // gcd
```
- 최소공배수는 두 수의 곱을 `GCD`로 나눈 값입니다.
- 유클리드 호제법을 활용해 `GCD`를 구한 후 계산하면 효율적으로 `LCM`을 구할 수 있습니다.


## 예제 입출력

### 입력 예시
```
3
15 20
21 6
24 36
```

### 출력 예시
```
60
42
72
```

## 시간 복잡도 분석

유클리드 호제법의 시간 복잡도는 **O(log N)**으로 매우 빠른 속도로 최대공약수를 구할 수 있습니다. 이를 활용해 최소공배수를 구하는 것도 **O(log N)**의 시간 복잡도를 가지므로, 대량의 입력을 처리할 때도 효율적입니다.

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

