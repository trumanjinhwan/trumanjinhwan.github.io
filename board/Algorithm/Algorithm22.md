---
layout: post
title: "22. 세그먼트 트리"
date: 2025-06-07
---

# 세그먼트 트리란?

세그먼트 트리는 **특정 구간에 대한 연산을 매우 빠르게 처리하기 위한 트리 자료구조**입니다. 데이터의 개수가 매우 많고, 특정 구간의 합이나 최댓값/최솟값을 구하는 질의와 데이터 변경이 빈번하게 발생할 때 압도적인 성능을 발휘합니다.

미리 구간별 연산 결과를 계산하여 트리 형태로 저장해두고, 질의가 들어오면 이 값들을 조합하여 \*\*O(log N)\*\*의 시간 복잡도로 결과를 도출합니다.

> **세그먼트 트리의 특징**
>
> * **빠른 구간 연산**: 구간 합, 최대/최소 등을 로그 시간 안에 처리 가능
> * **효율적인 데이터 업데이트**: 특정 원소의 값이 변경되어도 로그 시간 안에 트리 전체에 반영 가능
> * **배열 기반 구현**: 완전 이진트리 구조를 활용해 배열로 쉽게 구현할 수 있음

<br>

# 세그먼트 트리의 구현 단계

세그먼트 트리는 크게 3단계로 구현됩니다.

1. **트리 초기화하기**
   리프 노드가 원본 데이터의 개수(N)를 모두 포함할 수 있도록 트리의 크기를 결정합니다. 일반적으로 `N*4`의 크기로 배열을 할당하면 충분하며, 재귀적으로 각 노드에 구간의 연산 결과를 저장하며 트리를 구축합니다.

2. **질의값 구하기**
   주어진 범위(구간)에 대한 연산 결과를 찾는 과정입니다. 찾고자 하는 구간을 완전히 포함하는 노드들의 값을 효율적으로 조합하여 결과를 반환합니다. 이 과정이 세그먼트 트리의 핵심 효율성을 보여줍니다.

3. **데이터 업데이트하기**
   특정 위치의 데이터가 변경되었을 때, 그 변경 사항을 관련된 모든 노드(리프 노드부터 루트 노드까지의 경로)에 전파하여 트리의 정합성을 유지합니다.

<br>

# 예제: 백준 2042번 "구간 합 구하기"

## 문제 설명

* N개의 숫자가 주어지고, M번의 데이터 변경과 K번의 구간 합 질의가 발생한다.
* **데이터 변경**: 특정 인덱스의 숫자를 다른 숫자로 변경한다.
* **구간 합 질의**: 특정 구간에 속한 숫자들의 합을 구하여 출력한다.
* N이 최대 100만, M+K가 최대 2만이므로 O(N) 방식의 순차 합산으로는 시간 초과가 발생한다.

<br>

## 코드 구현

```python
import sys
input=sys.stdin.readline


# start, end: 숫자 배열의 인덱스
# idx: 세그먼트 트리의 인덱스 (1-based index)
def make_tree(start, end, idx):
    if start == end:
        tree[idx] = arr[start]
        return tree[idx]

    mid = (start + end) // 2
    tree[idx] = make_tree(start, mid, idx * 2) + make_tree(mid + 1, end, idx * 2 + 1)

    return tree[idx]


# target: 수정할 값의 숫자 배열 인덱스
# value: 수정할 값 (기존 값에 얼만큼 더해야 하는지의 값)
def update_tree(start, end, idx, target, value):
    if target < start or target > end:
        return
    
    tree[idx] += value
    if start == end:
        return
    
    mid = (start + end) // 2
    update_tree(start, mid, idx * 2, target, value)
    update_tree(mid + 1, end, idx * 2 + 1, target, value)


# left, right: 구하고자 하는 범위
def sum_tree(start, end, idx, left, right):
    if right < start or left > end:
        return 0

    if left <= start and right >= end:
        return tree[idx]

    mid = (start + end) // 2
    return sum_tree(start, mid, idx * 2, left, right) + sum_tree(mid + 1, end, idx * 2 + 1, left, right)


N, M, K = map(int, input().split())
arr = []
tree = [0] * (N * 4)  # 충분한 트리 공간 확보
for _ in range(N):
    arr.append(int(input()))

make_tree(0, N-1, 1)
for _ in range(M + K):
    a, b, c = map(int, input().split())
    if a == 1:
        diff = c - arr[b-1]
        arr[b-1] = c  # 기존 숫자 배열 값 변경
        update_tree(0, N-1, 1, b-1, diff)
    else:
        print(sum_tree(0, N-1, 1, b-1, c-1))
```

<br>

## 코드 분석

### 1. 트리 초기화하기 (`make_tree`)

```python
tree = [0] * (N * 4)
arr = []
for _ in range(N):
    arr.append(int(input()))

make_tree(0, N-1, 1)
```

→ (개념 1 적용) 원본 데이터를 `arr`에 저장하고, `N*4` 크기의 `tree` 배열을 생성하여 충분한 공간을 확보합니다. `make_tree` 함수는 루트 노드(인덱스 1)부터 시작하여 재귀적으로 각 노드에 자식 노드들의 합을 채워 넣어 구간 합 트리를 완성합니다.

### 2. 질의값 구하기 (`sum_tree`)

```python
def sum_tree(start, end, idx, left, right):
    if right < start or left > end:
        return 0

    if left <= start and right >= end:
        return tree[idx]

    mid = (start + end) // 2
    return sum_tree(start, mid, idx * 2, left, right) + sum_tree(mid + 1, end, idx * 2 + 1, left, right)
```

→ (개념 2 적용) "선택된 노드를 모두 더한다"는 개념이 여기에 적용됩니다. Case 2에서처럼, 찾으려는 구간을 완전히 대표하는 노드를 만나면 그 노드의 값(미리 계산된 합)을 즉시 반환하여 탐색을 최적화합니다. Case 3에서는 구간을 나누어 필요한 노드들을 찾아 합칩니다. 이 과정을 통해 O(log N)의 빠른 속도를 달성합니다.

### 3. 데이터 업데이트하기 (`update_tree`)

```python
def update_tree(start, end, idx, target, value):
    if target < start or target > end:
        return

    tree[idx] += value
    if start == end:
        return

    mid = (start + end) // 2
    update_tree(start, mid, idx * 2, target, value)
    update_tree(mid + 1, end, idx * 2 + 1, target, value)
```

→ (개념 3 적용) "자신의 부모 노드로 이동하면서 업데이트"하는 원리를 Top-down 재귀 방식으로 구현했습니다. `update_tree`는 변경이 필요한 인덱스(target)를 포함하는 모든 노드를 루트부터 리프까지 찾아 내려가면서 값을 갱신합니다. 특히, `value` 파라미터로 새로운 값이 아닌 \*\*기존 값과의 차이(diff)\*\*를 전달하여, 관련된 모든 구간 합에 해당 차이만큼만 더해주면 되므로 매우 효율적입니다.

<br>

# 정리

| 항목      | 설명                            |
| ------- | ----------------------------- |
| 알고리즘 유형 | 세그먼트 트리 (Segment Tree)        |
| 주요 조건   | 빈번한 데이터 변경과 구간 합 쿼리 처리        |
| 핵심 자료구조 | 배열 기반 트리, 재귀 함수               |
| 시간 복잡도  | 초기화: O(N), 쿼리/업데이트: O(log N)  |
| 응용 예시   | 구간 최소/최대값, 구간 곱, 펜윅 트리(BIT) 등 |

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
