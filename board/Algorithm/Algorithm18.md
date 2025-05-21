---
layout: post
title: "18. 플로이드-워셜"
date: 2025-05-21
---

# 플로이드-워셜 알고리즘이란?

플로이드-워셜(Floyd-Warshall) 알고리즘은 **그래프 내 모든 정점 쌍 간의 최단 거리**를 구하는 알고리즘입니다.
\*\*동적 계획법(DP)\*\*을 기반으로 하며, 모든 정점을 거쳐가는 경우를 고려하여 최단 경로를 점차 갱신해 나가는 방식입니다.

> **특징**
>
> * 모든 정점 간 최단 경로를 구할 수 있다.
> * **음수 가중치**가 있어도 동작한다. (단, **음수 사이클은 처리 불가**)
> * 인접 행렬(2차원 배열)을 이용하여 구현한다.
> * 시간 복잡도는 \*\*O(N³)\*\*로, 노드 수가 많을수록 성능에 주의가 필요하다.

<br>

# 플로이드-워셜 알고리즘의 원리

플로이드-워셜은 다음과 같은 점화식을 기반으로 최단 경로를 계산합니다:

D\[S]\[E] = min(D\[S]\[E], D\[S]\[K] + D\[K]\[E])

이는 S에서 E로 가는 최단 거리보다, **중간에 K를 거쳐 가는 것이 더 짧다면 갱신**하라는 뜻입니다.
이 점화식을 기반으로, 세 개의 중첩 반복문을 사용하여 거리를 갱신합니다.

<br>

## 알고리즘 흐름

1. \*\*그래프를 인접 행렬(2차원 배열)\*\*로 초기화한다.

   * 자기 자신으로 가는 거리는 0
   * 나머지는 무한대로 설정 (`sys.maxsize`)
2. **주어진 간선 정보로 초기 비용 저장**

   * `distance[s][e] = min(distance[s][e], cost)`
3. **중간 노드 K를 기준으로**

   * `distance[i][j] = min(distance[i][j], distance[i][k] + distance[k][j])`
   * 3중 for문 사용 (가장 바깥 루프가 K)

> 플로이드-워셜 알고리즘의 핵심은 “모든 정점을 경유지로 삼아 최단 거리 정보를 완성하는 것”입니다.

<br>

# 예제: 백준 11404번 "플로이드"

## 문제 설명

* 도시의 개수 `N`, 버스 노선의 개수 `M`이 주어진다.
* 각 노선은 시작 도시 `s`, 도착 도시 `e`, 비용 `v`로 주어진다.
* **각 도시 간의 최소 비용**을 모두 출력하되, **갈 수 없는 경우는 0**으로 출력한다.

<br>

## 코드 구현

```python
import sys

# 도시 수, 노선 수 입력
N = int(sys.stdin.readline().strip())
M = int(sys.stdin.readline().strip())

# 인접 행렬 초기화
distance = [[sys.maxsize] * (N+1) for _ in range(N+1)]

# 자기 자신으로 가는 거리는 0
for i in range(1, N+1):
    distance[i][i] = 0

# 간선 정보 입력
for _ in range(M):
    s, e, v = map(int, sys.stdin.readline().split())
    if distance[s][e] > v:
        distance[s][e] = v

# 플로이드-워셜 알고리즘 수행
for k in range(1, N+1):
    for i in range(1, N+1):
        for j in range(1, N+1):
            if distance[i][j] > distance[i][k] + distance[k][j]:
                distance[i][j] = distance[i][k] + distance[k][j]

# 결과 출력
for i in range(1, N+1):
    for j in range(1, N+1):
        if distance[i][j] == sys.maxsize:
            print("0", end=" ")
        else:
            print(distance[i][j], end=" ")
    print()
```

<br>

## 코드 분석

### 1. 인접 행렬 초기화

```python
distance = [[sys.maxsize] * (N+1) for _ in range(N+1)]
for i in range(1, N+1):
    distance[i][i] = 0
```

2차원 배열을 사용하여 거리 정보를 저장하고, 자기 자신은 0, 나머지는 무한대로 설정합니다.

### 2. 간선 정보 갱신

```python
if distance[s][e] > v:
    distance[s][e] = v
```

같은 노선이 여러 번 입력될 수 있으므로, 가장 작은 비용만 저장합니다.

### 3. 최단 거리 점화식 적용 (3중 for문)

```python
for k in range(1, N+1):
    for i in range(1, N+1):
        for j in range(1, N+1):
            if distance[i][j] > distance[i][k] + distance[k][j]:
                distance[i][j] = distance[i][k] + distance[k][j]
```

이 부분이 플로이드-워셜 알고리즘의 핵심입니다.
i → j로 가는 경로 중, k를 거치는 경로가 더 짧다면 갱신합니다.

### 4. 출력 처리

```python
if distance[i][j] == sys.maxsize:
    print("0", end=" ")
else:
    print(distance[i][j], end=" ")
```

경로가 없으면 0을 출력하고, 존재하면 최소 비용을 출력합니다.

<br>

## 정리

| 항목      | 설명                             |
| ------- | ------------------------------ |
| 알고리즘 유형 | 플로이드-워셜 알고리즘                   |
| 주요 조건   | 모든 정점 간 최단 거리 계산 (N x N)       |
| 핵심 자료구조 | 인접 행렬 (2차원 배열)                 |
| 시간 복잡도  | O(N³)                          |
| 적용 예시   | 지도 전체 거리 계산, 모든 쌍 간 경로 비용 확인 등 |

<style> table { width: 100%; border-collapse: collapse; margin: 20px 0; } th, td { border: 2px solid #333; padding: 12px; text-align: center; } th { background-color: #f4f4f4; font-weight: bold; } td { background-color: #fafafa; } table th, table td { border: 1px solid #ddd; } </style>
