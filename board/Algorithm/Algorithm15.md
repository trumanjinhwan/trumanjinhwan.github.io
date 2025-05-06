---
layout: post
title: "17. 위상 정렬"
date: 2025-05-05
---

# 위상 정렬이란?

위상 정렬(Topological Sort)은 \*\*사이클이 없는 방향 그래프(DAG)\*\*에서 노드들을 **선후 관계를 지키면서 나열**하는 정렬 방법입니다. 보통 **작업 순서**, **강의 계획**, **줄 세우기**와 같이 선행 조건이 주어진 문제에서 자주 등장합니다.

> **특징**
>
> * 사이클이 없어야 한다.
> * 위상 정렬 결과는 **여러 개 존재**할 수 있다.

<br>

# 위상 정렬의 원리

1. 모든 노드의 **진입 차수**를 계산한다.
2. 진입 차수가 0인 노드를 큐에 삽입한다.
3. 큐에서 노드를 꺼내 정렬 결과에 추가하고, 해당 노드에서 나가는 간선을 제거하며 **연결된 노드들의 진입 차수**를 1씩 감소시킨다.
4. 다시 진입 차수가 0이 된 노드를 큐에 넣는다.
5. 큐가 빌 때까지 이 과정을 반복한다.

<br>

# 예제: 백준 2252번 "줄 세우기"

## 문제 설명

* 학생 수 `n`명과 `m`개의 비교 조건이 주어진다.
* "A는 B보다 앞에 서야 한다"는 관계가 m개 존재한다.
* 이를 만족하도록 학생들을 나열하는 것이 목적이다.

<br>

## 코드 구현

```python
import sys

# n: 학생 수, m: 비교 데이터
n, m = map(int, sys.stdin.readline().split())

# 인접 리스트
A = []
for i in range(n):
    A.append(list())

# 진입 차수 배열 초기화
indegree = [0 for _ in range(n)]

# 간선 정보 입력
for i in range(m):
    s, e = map(int, sys.stdin.readline().split())
    A[s-1].append(e)          # 간선 추가
    indegree[e-1] += 1        # 진입 차수 증가

# 큐 초기화 (진입 차수가 0인 노드부터 시작)
queue = []
for i in range(1, n+1):
    if indegree[i-1] == 0:
        queue.append(i)

# 위상 정렬 수행
while (queue):
    now = queue.pop(0)
    print(str((now)), end=" ")
    for next in A[now - 1]:
        indegree[next - 1] -= 1
        if (indegree[next - 1] == 0):
            queue.append(next)
```

<br>

## 코드 분석

### 위상 정렬 정의 및 원리 적용

* **진입 차수 배열 구성:**

  ```python
  indegree = [0 for _ in range(n)]
  for s, e in edges:
      indegree[e-1] += 1
  ```

  → 모든 노드에 대해 몇 개의 노드가 자신에게 연결돼 있는지 카운트합니다.

* **진입 차수 0인 노드 큐에 삽입:**

  ```python
  for i in range(1, n+1):
      if indegree[i-1] == 0:
          queue.append(i)
  ```

* **정렬 배열에 추가하고 연결된 노드 진입 차수 감소:**

  ```python
  while queue:
      now = queue.pop(0)
      print(now, end=" ")
      for next in A[now - 1]:
          indegree[next - 1] -= 1
          if indegree[next - 1] == 0:
              queue.append(next)
  ```

  → 선행 관계를 만족시키며 노드를 나열하는 핵심 로직입니다.

<br>

## 정리

| 항목      | 설명                          |
| ------- | --------------------------- |
| 문제 유형   | 위상 정렬 (Topological Sort)    |
| 핵심 아이디어 | 진입 차수 배열을 통해 선행 관계를 제거하며 나열 |
| 주요 자료구조 | 인접 리스트, 큐, 진입 차수 배열         |
| 시간 복잡도  | O(N + M) (노드 수 + 간선 수)      |

> 위상 정렬은 사이클이 없어야 하며, 정답이 유일하지 않을 수 있습니다.
> 작업 순서, 강의 수강 순서, 줄 세우기 등 다양한 현실 문제에 응용됩니다.

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
