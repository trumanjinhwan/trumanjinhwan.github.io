---

layout: post
title: "23. 최소 공통 조상 (LCA)"
date: 2025-06-14
---

# 최소 공통 조상(LCA)이란?

LCA(Lowest Common Ancestor)는 트리 구조에서 **임의의 두 노드로부터 거슬러 올라갔을 때 처음으로 만나게 되는 공통 조상 노드**를 의미합니다. 두 노드 사이의 경로를 파악하거나 관계를 정의하는 데 매우 중요한 개념입니다.

> **LCA의 특징**
>
> * 두 노드를 모두 자손으로 갖는 조상 중 가장 깊이가 깊은(가장 가까운) 노드
> * 노드 간의 유일 경로(Unique Path)는 두 노드에서 LCA까지의 경로를 합친 것
> * 깊이(depth)와 부모(parent) 정보가 LCA를 찾는 데 핵심적인 역할을 함

<br>

# LCA를 구하는 핵심 이론

사용자님께서 정리해주신 내용처럼, 가장 기본적인 LCA 알고리즘은 다음과 같은 단계로 동작합니다. 이 방식은 직관적이고 구현이 쉽지만, 쿼리마다 트리의 높이에 비례하는 시간이 소요될 수 있습니다.

1. **전처리 (깊이와 부모 정보 계산)**
   먼저 DFS나 BFS를 이용해 트리의 루트(보통 1번 노드)부터 탐색을 시작합니다. 이 과정에서 모든 노드의 \*\*깊이(depth)\*\*와 각 노드의 **직속 부모 노드** 정보를 배열에 미리 계산하여 저장해 둡니다.

2. **깊이(Depth) 맞추기**
   LCA를 찾을 두 노드 `a`, `b`의 깊이를 비교합니다. 만약 두 노드의 깊이가 다르다면, 더 깊은 쪽에 있는 노드를 자신의 부모 노드로 한 칸씩 이동시켜 두 노드의 깊이가 같아질 때까지 이 과정을 반복합니다.

3. **동시에 위로 이동하며 공통 조상 찾기**
   두 노드의 깊이가 같아졌다면, 이제 두 노드를 **동시에** 한 칸씩 부모 노드로 이동시킵니다. 이동하다가 두 노드가 처음으로 같아지는 지점이 발생하는데, 이 노드가 바로 두 노드의 `최소 공통 조상(LCA)`입니다.

<br>

# 예제: 백준 11437번 "LCA"

## 문제 설명

* N개의 노드로 이루어진 트리가 주어진다. (루트는 1번 노드)
* M개의 노드 쌍(a, b)이 주어졌을 때, 각 쌍의 최소 공통 조상을 찾아 출력하라.
* N의 크기가 최대 50,000이므로, 각 쿼리를 효율적으로 처리해야 한다.

<br>

## 코드 구현

```python
import sys
sys.setrecursionlimit(int(1e5)) # 런타임 오류를 피하기
input = sys.stdin.readline

n = int(input())    # 노드의 개수 입력

parent = [0] * (n + 1) # 각 노드의 부모 노드
d = [0] * (n + 1) # 각 노드까지의 깊이
visited = [0] * (n + 1) # 방문 여부 (깊이 계산 여부)

graph = [[] for _ in range(n + 1)]

for _ in range(n - 1):
    a, b = map(int, input().split())
    graph[a].append(b)
    graph[b].append(a)

# 루트 노드부터 시작하여 깊이와 부모를 구하는 함수 (전처리)
def dfs(x, depth):
    visited[x] = True
    d[x] = depth
    for i in graph[x]:
        if visited[i]:
            continue
        parent[i] = x   # i의 부모는 x
        dfs(i, depth + 1)

# a, b의 최소 공통 조상을 찾는 함수
def lca(a, b):
    # 1. 깊이(depth)가 같아지도록 맞추기
    while d[a] != d[b]:
        if d[a] > d[b]:
            a = parent[a]
        else:
            b = parent[b]
    
    # 2. 노드가 같아질 때까지 부모로 이동
    while a != b:
        a = parent[a]
        b = parent[b]

    return a

dfs(1, 0)   # 루트는 1번 노드, 깊이는 0부터 시작

m = int(input())

for i in range(m):
    a, b = map(int, input().split())
    print(lca(a, b))
```

<br>

## 코드 분석

### 1. 전처리: 깊이와 부모 정보 계산 (`dfs`)

```python
parent = [0] * (n + 1)
d = [0] * (n + 1)
visited = [0] * (n + 1)

def dfs(x, depth):
    visited[x] = True
    d[x] = depth # 현재 노드의 깊이 기록
    for i in graph[x]:
        if visited[i]:
            continue
        parent[i] = x   # 방문하지 않은 자식 노드의 부모를 현재 노드(x)로 설정
        dfs(i, depth + 1)

dfs(1, 0) # 루트 1번부터 탐색 시작
```

→ (이론 1 적용) `lca` 함수를 호출하기 전에, `dfs(1, 0)`을 단 한 번 실행하여 모든 노드의 깊이(`d` 배열)와 직속 부모(`parent` 배열\`)를 미리 계산합니다. 이는 LCA를 찾기 위한 필수적인 전처리 과정입니다.

### 2. 최소 공통 조상 찾기 (`lca`)

```python
def lca(a, b):
    # 깊이가 같아지도록 맞추기
    while d[a] != d[b]:
        if d[a] > d[b]:
            a = parent[a]
        else:
            b = parent[b]

    # 노드가 같아지도록 부모로 이동
    while a != b:
        a = parent[a]
        b = parent[b]
    return a
```

→ (이론 2 적용) `lca` 함수의 첫 번째 while문에서는 두 노드 a와 b의 깊이가 다를 경우, `d` 배열을 참조하여 더 깊은 노드를 `parent` 배열을 이용해 한 칸 위로 올립니다. 이 과정을 반복하여 두 노드의 깊이를 동일하게 맞춥니다.

→ (이론 3 적용) 깊이가 같아진 두 노드를 동시에 `parent` 배열을 이용해 한 칸씩 부모 노드로 이동시킵니다. 이 과정을 반복하다가 `a`와 `b`가 같아지는 순간, 해당 노드를 반환하며 그것이 최소 공통 조상입니다.

<br>

# 정리

| 항목      | 설명                                  |
| ------- | ----------------------------------- |
| 알고리즘 유형 | 최소 공통 조상 (LCA) - 기본 탐색 방식           |
| 주요 조건   | 깊이 맞추기 후, 공통 조상 찾을 때까지 동시 이동        |
| 핵심 자료구조 | 인접 리스트, 부모 배열(parent), 깊이 배열(d)     |
| 시간 복잡도  | 전처리: O(N), 쿼리: O(H) (H는 트리의 높이)     |
| 응용 예시   | 노드 간 거리 계산, 계보(family tree) 관계 파악 등 |

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
