---
layout: post
title: "15. 다익스트라"
date: 2025-05-07
---

# 다익스트라 알고리즘이란?

다익스트라(Dijkstra) 알고리즘은 **그래프에서 하나의 정점에서 다른 모든 정점까지의 최단 경로**를 구하는 알고리즘입니다. 이 알고리즘은 **간선의 가중치가 모두 양수**일 때만 사용할 수 있으며, 실제로 지도 앱의 길찾기 등 다양한 실생활 문제에 적용됩니다.

> **특징**
>
> * 음의 가중치가 없어야 한다.
> * 우선순위 큐(힙 구조)를 사용하면 시간 복잡도를 줄일 수 있다.
> * BFS와 유사한 방식으로 최단 거리를 점차 확정시켜 나간다.

<br>

# 다익스트라 알고리즘의 원리

1. **그래프를 인접 리스트**로 구현한다.
2. **최단 거리 배열**을 무한대로 초기화하고, 시작 노드는 0으로 설정한다.
3. **우선순위 큐를 활용**하여 현재까지의 최단 거리가 가장 짧은 노드를 선택한다.
4. 해당 노드에서 갈 수 있는 인접 노드들의 거리를 업데이트한다.
5. 위 과정을 큐가 빌 때까지 반복한다. (이때 **중복 방문 방지를 위해 visited 배열** 사용)

<br>

# 예제: 백준 1753번 "최단경로"

## 문제 설명

* 정점의 개수 `V`와 간선의 개수 `E`, 시작 정점 `K`가 주어진다.
* 간선 정보는 `u, v, w`로 주어지며, `u`에서 `v`로 가는 가중치 `w`를 의미한다.
* `K`에서 출발하여 각 정점으로 가는 최단 경로의 거리를 출력한다.

<br>

## 코드 구현

```python
from queue import PriorityQueue
import sys

V, E = map(int, sys.stdin.readline().split())
K = int(sys.stdin.readline().strip())
distance = [sys.maxsize] * (V + 1)
visited = [False] * (V + 1)
myList = [[] for _ in range(V+1)]
q = PriorityQueue()

for i in range(E):
    u, v, w = map(int, sys.stdin.readline().split())
    myList[u].append((v, w))

q.put((0, K))
distance[K] = 0

while q.qsize() > 0:
    current = q.get()
    c_v = current[1]
    if visited[c_v]:
        continue
    visited[c_v] = True
    for temp in myList[c_v]:
        next = temp[0]
        value = temp[1]
        if distance[next] > distance[c_v] + value:
            distance[next] = distance[c_v] + value
            q.put((distance[next], next))

for i in range(1, V+1):
    if visited[i]:
        sys.stdout.write(f"{distance[i]}" + "\n")
    else:
        sys.stdout.write("INF" + "\n")
```

<br>

## 코드 분석

### 인접 리스트로 그래프 구현

```python
myList = [[] for _ in range(V+1)]
for i in range(E):
    u, v, w = map(int, sys.stdin.readline().split())
    myList[u].append((v, w))
```

→ 공간 효율성과 접근 속도 면에서 인접 리스트가 유리하며, 다익스트라에 적합합니다.

### 최단 거리 배열 초기화

```python
distance = [sys.maxsize] * (V + 1)
distance[K] = 0
```

→ 시작 노드의 거리는 0, 나머지는 무한대로 초기화하여 갱신 기준을 마련합니다.

### 최단 거리가 가장 짧은 노드 선택

```python
current = q.get()
c_v = current[1]
if visited[c_v]:
    continue
visited[c_v] = True
```

→ 우선순위 큐를 통해 최소 거리 노드를 고르고, 방문한 적이 있다면 스킵합니다.

### 거리 배열 업데이트

```python
for temp in myList[c_v]:
    next = temp[0]
    value = temp[1]
    if distance[next] > distance[c_v] + value:
        distance[next] = distance[c_v] + value
        q.put((distance[next], next))
```

→ `distance[next]`가 더 짧은 경로로 갱신될 수 있다면 업데이트하고 큐에 삽입합니다.

<br>

## 정리

| 항목      | 설명                                           |
| ------- | -------------------------------------------- |
| 알고리즘 유형 | 다익스트라 알고리즘                                   |
| 주요 조건   | 간선 가중치가 양수                                   |
| 핵심 자료구조 | 우선순위 큐 (PriorityQueue), 인접 리스트, 거리 배열, 방문 배열 |
| 시간 복잡도  | O((V + E) log V) (우선순위 큐 사용 시)               |
| 응용 예시   | 지도 길찾기, 네트워크 비용 계산, 최소 비용 경로 탐색 등            |

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
