---

layout: post
title: "20. 트리의 기초"
date: 2025-05-24
---

# 트리(Tree)란?

트리는 **노드와 간선으로 이루어진 비순환 연결 그래프**입니다. 그래프의 일종이지만, **사이클이 존재하지 않고 하나의 루트 노드에서 출발해 모든 노드를 연결**한다는 점에서 특별한 구조를 가집니다.

> **트리의 특징**
>
> * 순환 구조가 없다 (사이클 없음)
> * 정확히 하나의 루트 노드가 존재
> * 루트를 제외한 모든 노드는 단 하나의 부모를 가짐
> * 트리의 부분 구조(서브트리)도 트리이다

<br>

# 트리 문제를 푸는 방식

트리는 일반 그래프와는 달리 특수한 구조를 이용해 **DFS, BFS**, 또는 \*\*트리 특화 알고리즘(LCA, 세그먼트 트리 등)\*\*으로 문제를 풉니다.

## 트리 문제 유형

1. **그래프로 푸는 트리**

   * 인접 리스트/행렬로 그래프처럼 저장하고 DFS/BFS로 탐색
2. **트리 전용 문제**

   * 이진 트리, LCA(최소 공통 조상), 세그먼트 트리 등

<br>

# 예제: 백준 11725번 "트리의 부모 찾기"

## 문제 설명

* 루트 노드가 1인 트리가 주어진다.
* 각 노드의 **부모 노드를 출력**하라.
* 입력은 노드 수 N과 N-1개의 간선이 주어지며, 트리를 구성한다.

<br>

## 코드 구현

```python
import sys
sys.setrecursionlimit(10**6)

N = int(sys.stdin.readline().strip())

visited = [False] * (N + 1)
tree = [[] for _ in range(N+1)]
answer = [0] * (N + 1)

# 간선 정보 저장
for _ in range(1, N):
    n1, n2 = map(int, sys.stdin.readline().split())
    tree[n1].append(n2)
    tree[n2].append(n1)

# DFS 탐색으로 부모 찾기
def dfs(number):
    visited[number] = True
    for i in tree[number]:
        if not visited[i]:
            answer[i] = number  # 부모 노드 기록
            dfs(i)

dfs(1)  # 루트 노드에서 탐색 시작

# 결과 출력
for i in range(2, N+1):
    print(answer[i])
```

<br>

## 코드 분석

### 1. 트리 구조 저장

```python
tree = [[] for _ in range(N+1)]
for _ in range(1, N):
    n1, n2 = map(int, sys.stdin.readline().split())
    tree[n1].append(n2)
    tree[n2].append(n1)
```

트리를 **양방향 그래프**처럼 인접 리스트로 저장합니다. 트리는 사이클이 없기에 이 구조로 DFS 시 부모를 명확히 구분할 수 있습니다.

---

### 2. DFS로 부모 노드 찾기

```python
def dfs(number):
    visited[number] = True
    for i in tree[number]:
        if not visited[i]:
            answer[i] = number
            dfs(i)
```

방문한 노드를 체크하며, **방문하지 않은 자식 노드의 부모를 현재 노드로 설정**합니다.
DFS의 깊이 우선 탐색으로 **트리 전체를 탐색**합니다.

---

### 3. 출력 처리

```python
for i in range(2, N+1):
    print(answer[i])
```

루트 노드를 제외한 노드들의 부모를 출력합니다.

<br>

## 정리

| 항목      | 설명                         |
| ------- | -------------------------- |
| 알고리즘 유형 | DFS 기반 트리 탐색               |
| 주요 조건   | 루트는 1번, 부모는 DFS를 통해 설정     |
| 핵심 자료구조 | 인접 리스트, 방문 배열, 정답 배열       |
| 시간 복잡도  | O(N) - 노드 수만큼 탐색           |
| 응용 예시   | 부모 탐색, 서브트리 계산, 트리 구조 파악 등 |

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
