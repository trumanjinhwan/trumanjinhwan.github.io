---
layout: post
title: "26. 조합 (Combination)과 이항 계수"
date: 2025-07-05
---

# 조합(Combination)이란?

조합은 **서로 다른 n개의 원소에서 순서를 생각하지 않고 k개를 뽑는 경우의 수**를 의미합니다. 예를 들어, 3명의 학생 {A, B, C} 중에서 2명을 뽑는 경우, {A, B}, {A, C}, {B, C}의 3가지가 있으며, {A, B}와 {B, A}는 같은 경우로 취급합니다.

조합은 이항 계수(Binomial Coefficient)와도 밀접한 관련이 있으며, `nCk` 또는 `C(n, k)`로 표기합니다.

> **조합의 특징**
>
> * **순서 무시**: 원소를 뽑는 순서는 결과에 영향을 주지 않음
> * **핵심 공식**: `nCk = n! / (k! * (n-k)!)`
> * **응용 분야**: 확률, 통계, 경우의 수를 다루는 다양한 문제의 기반이 됨

<br>

# 조합을 구하는 핵심 이론

조합을 계산하는 가장 기본적인 방법은 수학 공식을 직접 이용하는 것입니다.

### 1. 팩토리얼(Factorial) 기반 공식

가장 널리 알려진 공식입니다.
`nCk = n! / (k! * (n-k)!)`
하지만 n이 조금만 커져도 n!의 값은 매우 커지기 때문에, 컴퓨터의 자료형 범위를 초과할 수 있어 직접적인 구현이 어려울 수 있습니다.

### 2. 순열(Permutation) 기반 공식

팩토리얼의 약분 성질을 이용한 공식으로, 제공된 코드에서 사용한 방식입니다.
`nCk = nPk / k!`
`nCk = (n * (n-1) * ... * (n-k+1)) / (k * (k-1) * ... * 1)`

이 방식은 분자와 분모를 각각 계산한 후 나누기 때문에, 중간 계산 과정에서 발생하는 오버플로우를 어느 정도 방지할 수 있습니다.

**참고: 파스칼의 삼각형**
점화식을 이용하는 방법도 있습니다. `nCk = (n-1)C(k-1) + (n-1)Ck`. 이 방법은 동적 프로그래밍(DP)으로 구현하며, 모듈러 연산이 포함된 조합 문제에서 매우 유용하게 사용됩니다.

<br>

# 예제: 백준 11051번 "이항 계수 2"

## 문제 설명

* 자연수 `N`과 정수 `K`가 주어졌을 때, 이항 계수 `nCk`를 10,007로 나눈 나머지를 출력하라.
* `1 ≤ N ≤ 1,000`, `0 ≤ K ≤ N`

<br>

## 코드 구현

```python
import sys
input = sys.stdin.readline

N, K = map(int, input().split())

# 분자(Numerator) 계산: n * (n-1) * ... * (n-k+1)
result = 1
for i in range(K):
    result *= N
    N -= 1

# 분모(Denominator) 계산: k!
divisor = 1
for i in range(2, K + 1):
    divisor *= i

# (분자 / 분모) % 10007
print((result // divisor) % 10007)
```

<br>
코드 분석

1. 분자 계산 (순열 nPk 부분)

```python
result = 1
for i in range(K):
    result *= N
    N -= 1
```

→ 조합 공식 nPk / k!에서 분자에 해당하는 nPk 즉, n \* (n-1) \* ... \* (n-k+1)을 계산하는 부분입니다. for 루프가 K번 반복되면서 result에 N을 곱하고, N을 1씩 감소시킵니다.
첫 번째 반복: result = N
두 번째 반복: result = N \* (N-1)
...
K 번째 반복: result = N \* (N-1) \* ... \* (N-K+1)

2. 분모 계산 (k! 부분)

```python
divisor = 1
for i in range(2, K + 1):
    divisor *= i
```

→ 조합 공식에서 분모에 해당하는 k! (k 팩토리얼)을 계산합니다. divisor에 2부터 K까지의 모든 수를 차례로 곱하여 k!를 만듭니다.

3. 최종 계산 및 나머지 연산

```python
print((result // divisor) % 10007)
```

→ result (분자)를 divisor (분모)로 나눈 후, 그 결과를 10,007로 나눈 나머지를 출력합니다.

<br>
정리

| 항목      | 설명                             |
| ------- | ------------------------------ |
| 알고리즘 유형 | 조합론 (Combinatorics), 이항 계수     |
| 주요 조건   | nCk = (nPk) / k! 공식을 이용한 직접 계산 |
| 핵심 자료구조 | 기본 변수를 이용한 누적 곱셈               |
| 시간 복잡도  | O(K) - 분자, 분모 계산에 각각 K번의 반복 필요 |
| 응용 예시   | 경우의 수 계산, 확률 문제, 파스칼의 삼각형 등    |

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
