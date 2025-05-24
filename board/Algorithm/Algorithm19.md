---

layout: post
title: "19. 최소 신장 트리 (MST)"
date: 2025-05-24
---

# 최소 신장 트리란?

최소 신장 트리(Minimum Spanning Tree, MST)는 **그래프 내 모든 노드를 연결**할 때, **사용된 간선들의 가중치 합이 최소가 되는 트리**입니다. 흔히 통신망, 전력망 설계 등에서 비용을 최소화하기 위한 그래프 최적화 기법으로 쓰입니다.

> **특징**
>
> * 사이클이 포함되면 가중치의 합이 더 커질 수 있으므로, **사이클을 포함하지 않습니다**.
> * **노드 개수가 N이라면, 최소 신장 트리의 간선 수는 항상 N - 1개**입니다.
> * MST는 여러 개가 존재할 수 있지만, 그중 가중치 합이 동일한 최소 트리 중 하나만 구하면 됩니다.

<br>

# MST를 구하는 대표 알고리즘: 크루스칼

크루스칼(Kruskal) 알고리즘은 **가중치가 가장 작은 간선부터 하나씩 선택**하며, **사이클이 생기지 않도록 연결**하는 방식입니다. \*\*유니온-파인드(Disjoint Set)\*\*를 활용하여 사이클 여부를 판단합니다.

## 알고리즘 흐름

1. \*\*에지 리스트(Edge List)\*\*로 그래프를 구성한다.
2. **간선들을 가중치 기준 오름차순 정렬**한다.
3. **유니온-파인드 배열 초기화** (자기 자신을 대표로 설정)
4. **가중치가 가장 낮은 간선부터 연결**하되, **사이클이 생기지 않는 경우에만 선택**한다.
5. N-1개의 간선을 선택하면 종료한다.
6. 선택된 간선들의 **총 가중치 합을 출력**한다.

> 핵심 개념: "싸이클 없이 모든 노드를 연결하면서 비용이 최소가 되도록 한다."

<br>

# 예제: 백준 1197번 "최소 스패닝 트리"

## 문제 설명

* 정점의 개수 `V`, 간선의 개수 `E`가 주어진다.
* 이후 `E`개의 줄에 걸쳐 간선의 정보: 정점1, 정점2, 가중치가 주어진다.
* 이 그래프에서 **최소 신장 트리의 가중치 합**을 출력하라.

<br>

## 코드 구현

```python
import sys
sys.setrecursionlimit(10**6)  # 재귀 제한 해제
from queue import PriorityQueue

N, M = map(int, sys.stdin.readline().split())
pq = PriorityQueue()
parent = [i for i in range(N + 1)]

# 간선 입력 및 우선순위 큐 저장
for _ in range(M):
    s, e, w = map(int, sys.stdin.readline().split())
    pq.put((w, s, e))

# 유니온-파인드 구현

def find(a):
    if a == parent[a]:
        return a
    parent[a] = find(parent[a])  # 경로 압축
    return parent[a]

def union(a, b):
    a = find(a)
    b = find(b)
    if a != b:
        parent[b] = a

# MST 구성
useEdge = 0
result = 0

while useEdge < N - 1:
    w, s, e = pq.get()
    if find(s) != find(e):
        union(s, e)
        result += w
        useEdge += 1

print(result)
```

<br>

## 코드 분석

### 1. 우선순위 큐 기반 간선 정렬

```python
pq = PriorityQueue()
for _ in range(M):
    s, e, w = map(int, sys.stdin.readline().split())
    pq.put((w, s, e))
```

우선순위 큐를 통해 **가중치가 낮은 간선부터 자동으로 추출**되도록 합니다.

---

### 2. 유니온-파인드 자료구조

```python
def find(a):
    if a == parent[a]:
        return a
    parent[a] = find(parent[a])  # 경로 압축
    return parent[a]
```

각 노드의 대표 노드를 찾습니다. 경로 압축을 통해 **탐색 효율**을 높입니다.

```python
def union(a, b):
    a = find(a)
    b = find(b)
    if a != b:
        parent[b] = a
```

두 집합을 하나로 합칩니다. 이 과정에서 **사이클이 발생하지 않도록 조건을 검사**합니다.

---

### 3. MST 구성 반복

```python
while useEdge < N - 1:
    w, s, e = pq.get()
    if find(s) != find(e):
        union(s, e)
        result += w
        useEdge += 1
```

간선을 하나씩 꺼내어 **사이클이 생기지 않으면 선택**, 그 가중치를 누적합니다.
N-1개의 간선을 선택한 순간 MST가 완성되며 종료합니다.

---

### 4. 출력 처리

```python
print(result)
```

모든 노드를 연결하는 최소 비용을 출력합니다.

<br>

## 정리

| 항목      | 설명                                           |
| ------- | -------------------------------------------- |
| 알고리즘 유형 | 최소 신장 트리 (Minimum Spanning Tree) - 크루스칼 알고리즘 |
| 주요 조건   | 사이클이 없어야 함, 간선 수 = 노드 수 - 1                  |
| 핵심 자료구조 | 엣지 리스트, 유니온-파인드 (Disjoint Set)               |
| 시간 복잡도  | O(E log E) (간선 정렬 + 유니온 파인드 최적화)             |
| 응용 예시   | 전기 배선, 도로 건설, 네트워크 설계 등                      |

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
