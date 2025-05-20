---
layout: post
title: "18. 벨만-포드 알고리즘"
date: 2025-05-20
---

# 벨만-포드 알고리즘이란?

벨만-포드(Bellman-Ford) 알고리즘은 **하나의 정점에서 다른 모든 정점까지의 최단 거리**를 구하는 알고리즘입니다. 이 알고리즘의 가장 큰 특징은 **간선 가중치가 음수여도 동작**하며, \*\*음수 사이클(Negative Cycle)\*\*의 존재 여부를 판단할 수 있다는 점입니다.

> **특징**
>
> * 간선 가중치가 **음수여도** 작동한다.
> * **음수 사이클 탐지**가 가능하다.
> * 시간 복잡도는 \*\*O(V × E)\*\*로 다익스트라보다 느리지만, 그만큼 **일반적인 상황에 유연**하다.

<br>

# 벨만-포드 알고리즘의 원리

1. \*\*엣지 리스트(Edge List)\*\*로 그래프를 구현한다.
2. **최단 거리 배열**을 초기화한다. (시작 노드는 0, 나머지는 무한대)
3. **노드 수 - 1번 반복**하여 모든 엣지를 순회하며 최단 거리를 갱신한다.
4. 마지막에 **모든 엣지를 한 번 더 확인**하여 값이 갱신되는 엣지가 있다면, **음수 사이클이 존재**한다고 판단한다.

> 벨만-포드는 "간선을 N-1번 순회하며 Relaxation(완화)"을 수행하는 것이 핵심입니다.

<br>

# 예제: 백준 11657번 "타임머신"

## 문제 설명

* 도시의 개수 `N`, 버스 노선의 개수 `M`이 주어진다.
* 각 버스 노선은 출발 도시, 도착 도시, 걸리는 시간을 의미한다.
* 1번 도시에서 나머지 도시로 가는 **가장 빠른 시간**을 출력하라.
* 만약 **음수 사이클이 존재**하면 `-1`을 출력한다.

<br>

## 코드 구현

```python
import sys

N, M = map(int, sys.stdin.readline().split())
edges = []
distance = [sys.maxsize] * (N + 1)

# 엣지 데이터 저장
for i in range(M):
    start, end, time = map(int, sys.stdin.readline().split())
    edges.append((start, end, time))

# 시작 노드는 0으로 초기화
distance[1] = 0

# (1) N-1번 반복하면서 최단 거리 갱신
for _ in range(N-1):
    for start, end, time in edges:
        if distance[start] != sys.maxsize and distance[end] > distance[start] + time:
            distance[end] = distance[start] + time

# (2) 음수 사이클 존재 여부 확인
mCycle = False
for start, end, time in edges:
    if distance[start] != sys.maxsize and distance[end] > distance[start] + time:
        mCycle = True
        break

# (3) 출력
if not mCycle:
    for i in range(2, N+1):
        if distance[i] != sys.maxsize:
            print(distance[i])
        else:
            print("-1")
else:
    print("-1")
```

<br>

## 코드 분석

### 엣지 리스트 기반 그래프 구성

```python
edges = []
for i in range(M):
    start, end, time = map(int, sys.stdin.readline().split())
    edges.append((start, end, time))
```

→ 벨만-포드는 노드 간 연결보다는 **엣지를 중심으로 반복 연산**하기 때문에 인접 리스트 대신 엣지 리스트를 사용합니다.

---

### 최단 거리 배열 초기화

```python
distance = [sys.maxsize] * (N + 1)
distance[1] = 0  # 시작 노드
```

→ 모든 노드를 무한대로 초기화하고, 시작 노드는 거리 0으로 설정합니다.

---

### N-1번 반복하며 Relaxation 수행

```python
for _ in range(N-1):
    for start, end, time in edges:
        if distance[start] != sys.maxsize and distance[end] > distance[start] + time:
            distance[end] = distance[start] + time
```

→ 각 엣지를 순회하며 **최단 거리 정보를 갱신**합니다. 이 작업은 **노드 수 - 1번 반복**해야 모든 최단 거리 경로가 보장됩니다.

---

### 음수 사이클 탐지

```python
for start, end, time in edges:
    if distance[start] != sys.maxsize and distance[end] > distance[start] + time:
        mCycle = True
        break
```

→ 이 과정을 한 번 더 수행했을 때 갱신이 발생하면 **음수 사이클이 존재**한다는 뜻입니다.
→ 왜냐하면 계속해서 더 짧아질 수 있다는 건, **무한히 돌 수 있는 사이클**이 있다는 뜻이기 때문입니다.

---

### 결과 출력

```python
if not mCycle:
    for i in range(2, N+1):
        if distance[i] != sys.maxsize:
            print(distance[i])
        else:
            print("-1")
else:
    print("-1")
```

**음수 사이클이 없다면** 각 도시까지의 최단 거리 출력
**도달할 수 없는 도시**는 `-1` 출력
**음수 사이클이 있다면**, 최단 거리 정보 자체가 **무의미하므로 `-1` 출력**

<br>

## 정리

| 항목      | 설명                                           |
| ------- | -------------------------------------------- |
| 알고리즘 유형 | 벨만-포드 알고리즘                                   |
| 주요 조건   | 간선에 **음수 가중치가 있어도 동작**, 음수 사이클 탐지 가능         |
| 핵심 자료구조 | **엣지 리스트**, 최단 거리 배열                         |
| 시간 복잡도  | **O(V × E)** (V: 노드 수, E: 간선 수)              |
| 응용 예시   | 환율 그래프, 최저 비용 경로 탐색, **음수 사이클 검증이 필요한 문제** 등 |

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
