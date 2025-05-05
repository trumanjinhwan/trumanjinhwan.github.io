---
layout: post
title: "14. 유니온 파인드"
date: 2025-03-18
---

# 유니온 파인드란?

유니온 파인드(Disjoint Set, Union-Find) 알고리즘은 서로소(서로 겹치지 않는) 집합들을 효율적으로 관리할 수 있는 자료구조입니다. 대표적으로 다음 두 연산으로 구성됩니다:

* **Find 연산**: 특정 원소가 어떤 집합에 속해 있는지를 찾아 그 집합의 대표 노드를 반환합니다.
* **Union 연산**: 두 집합을 하나로 합칩니다.

---

## 핵심 원리 요약

1. **대표 노드끼리 연결해야 한다** (Union 시)
2. **Find 시 경로 압축(Path Compression)**: 재귀적으로 루트 노드를 찾은 뒤, 거쳐온 모든 노드의 부모를 루트로 설정해줍니다. 이는 추후 탐색 시 시간을 절약합니다.

---

## 백준 1717번 - 집합의 표현

### 문제 설명

* n개의 원소(0 \~ n)로 이루어진 집합이 있습니다.
* 합집합 연산(0 a b): a가 포함된 집합과 b가 포함된 집합을 합칩니다.
* 같은 집합에 속해있는지를 확인하는 연산(1 a b): YES 또는 NO를 출력합니다.

---

## 코드 구현

```python
import sys

n, m = map(int, sys.stdin.readline().split())
parent = []

def checkSame(a: int, b: int) -> bool:
    a = find(a)
    b = find(b)
    return a == b

def find(a: int) -> int:
    if a == parent[a]:
        return a
    else:
        parent[a] = find(parent[a])  # 경로 압축 수행
        return parent[a]

def union(a: int, b: int) -> None:
    a = find(a)
    b = find(b)
    if a != b:
        parent[b] = a  # 항상 대표 노드끼리 연결

# 초기화: 각 원소의 대표는 자기 자신
for i in range(0, n+1):
    parent.append(i)

# 질의 처리
for _ in range(m):
    question, a, b = map(int, sys.stdin.readline().split())
    if question == 0:
        union(a, b)
    else:
        print("YES" if checkSame(a, b) else "NO")
```

---

## 코드 분석: 핵심 원리 적용 위치

### 대표 노드끼리 연결

```python
parent[b] = a
```

* `union()` 함수 내에서 단순히 `a`, `b`가 아니라 `find(a)`, `find(b)`를 통해 각각의 **대표 노드**를 구한 뒤 연결합니다.

### 경로 압축

```python
parent[a] = find(parent[a])
```

* `find()` 함수에서 재귀적으로 루트 노드를 찾은 후, 거쳐온 노드들의 부모를 **루트로 설정**합니다.
* 이를 통해 트리의 높이가 줄어들며 이후 `find` 연산이 훨씬 빨라집니다.

---

## 시간 복잡도 분석

* **Find/Union 평균 시간 복잡도**: 거의 O(1)에 가까움

  * 실제로는 O(α(N)) 수준이며, 이는 매우 느리게 증가하는 역 아커만 함수입니다.
* **최악의 경우에도** 경로 압축이 존재하기 때문에 효율적으로 작동합니다.

---

## 정리

| 항목      | 설명                                      |
| ------- | --------------------------------------- |
| 알고리즘 유형 | 유니온 파인드 (Disjoint Set)                  |
| 주요 연산   | Union (합집합), Find (대표 노드 탐색)            |
| 최적화 기법  | 경로 압축 (Path Compression)                |
| 시간 복잡도  | 거의 O(1) 수준 (O(α(N)))                    |
| 적용 예시   | 집합 간 연결, 네트워크 연결 여부 판별, MST 크루스칼 알고리즘 등 |

유니온 파인드는 그래프 기반 자료구조로써 매우 효율적인 연결 여부 판단 및 집합 병합을 지원합니다. 특히 `경로 압축`과 `대표 노드 간 연결` 원칙을 잘 지키면 빠르고 안정적인 집합 관리가 가능합니다.

