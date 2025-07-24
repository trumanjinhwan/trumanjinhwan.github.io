---
layout: post
title: "24. 최소 공통 조상 (LCA) - 심화"
date: 2025-06-21
---

# 최소 공통 조상(LCA) 빠르게 구하기

기본적인 LCA 알고리즘은 각 쿼리마다 트리의 높이(H)에 비례하는 시간, 즉 O(H)가 소요됩니다. 트리가 한쪽으로 치우친 경우, 최악의 시간 복잡도는 O(N)에 달할 수 있어 노드와 쿼리의 개수가 많은 문제에서는 시간 초과가 발생할 수 있습니다.

이를 해결하기 위해 **희소 배열(Sparse Table)** 또는 **이진 리프팅(Binary Lifting)** 기법을 사용합니다. 이 방법은 부모 노드로 한 칸씩 이동하는 대신, **2의 거듭제곱(2^k) 단위로 점프**하여 이동함으로써 쿼리당 시간 복잡도를 \*\*O(log N)\*\*으로 획기적으로 개선합니다.

> **빠른 LCA의 특징**
>
> * **O(N log N) 전처리**: 모든 노드에 대해 2^k번째 부모를 미리 계산해 둠
> * **O(log N) 쿼리**: 2의 거듭제곱 단위 점프를 통해 매우 빠르게 LCA 탐색
> * **메모리 사용**: 2차원 배열 `parent[N][logN]` 만큼의 추가 메모리가 필요

<br>

# LCA 응용(빠르게 구하기) 핵심 이론

사용자님께서 정리해주신 내용처럼, 빠른 LCA 알고리즘은 3단계의 핵심 원리를 가집니다.

1. **부모 노드 배열 확장 (`parent[N][K]`)**
   단순히 직속 부모만 저장하는 대신, 모든 노드 `N`에 대해 `2^k`번째 부모가 누구인지를 저장하는 2차원 배열 `parent[node][k]`를 만듭니다. 이 배열은 다음과 같은 점화식을 통해 채울 수 있습니다.

   * **정의**: `parent[node][k]`는 `node`의 `2^k`번째 부모를 의미합니다.
   * **점화식**: `parent[node][k] = parent[ parent[node][k-1] ][k-1]`
   * **의미**: "나의 2^k번째 조상은 (나의 2^(k-1)번째 조상)의 2^(k-1)번째 조상이다."

2. **깊이 맞추기 (이진 점프)**
   두 노드의 깊이를 맞출 때, 한 칸씩 이동하는 대신 `2^k` 단위로 점프합니다. 가장 큰 `k`값부터 시작하여 깊이 차이가 `2^k`보다 크거나 같으면, 더 깊은 노드를 `2^k`만큼 위로 점프시켜 깊이 차이를 효율적으로 줄입니다.

3. **최소 공통 조상 찾기 (이진 점프)**
   깊이가 같아진 두 노드를 동시에 위로 올릴 때도 `2^k` 점프를 사용합니다. 가장 큰 `k`부터 시작하여, `2^k`번째 부모가 서로 다른 지점까지 최대한 올라갑니다. 이 과정이 끝나면 두 노드는 LCA의 바로 아래 자식 노드에 위치하게 되므로, 최종적으로 둘 중 아무 노드의 직속 부모가 LCA가 됩니다.

<br>

# 예제: 백준 11438번 "LCA 2"

## 문제 설명

* N개의 노드로 이루어진 트리가 주어지고, M개의 노드 쌍이 주어진다.
* 각 노드 쌍의 최소 공통 조상을 출력해야 한다.
* N과 M이 최대 100,000이므로, O(log N) 쿼리 시간 복잡도를 갖는 알고리즘이 필수적이다.

<br>

## 코드 구현

```python
import sys
sys.setrecursionlimit(100000)
input = sys.stdin.readline
LENGTH = 21 # 2^20 > 100,000, 충분한 크기

n = int(input())
# parent[i][j]: i의 2^j 번째 부모
parent = [[0] * LENGTH for _ in range(n + 1)]
visited = [False] * (n + 1)
d = [0] * (n + 1) # 깊이
graph = [[] for _ in range(n + 1)]

for _ in range(n - 1):
    a, b = map(int, input().split())
    graph[a].append(b)
    graph[b].append(a)

# 1. DFS로 각 노드의 깊이와 직속 부모(2^0번째 부모) 계산
def dfs(x, depth):
    visited[x] = True
    d[x] = depth

    for node in graph[x]:
        if visited[node]:
            continue
        parent[node][0] = x
        dfs(node, depth + 1)

# 2. 모든 노드의 전체 부모 관계(2^k번째 부모) 갱신
def set_parent():
    dfs(1, 0)
    for i in range(1, LENGTH):
        for j in range(1, n + 1):
            parent[j][i] = parent[parent[j][i - 1]][i - 1]

# 3. LCA 찾기 함수
def lca(a, b):
    # b의 깊이가 더 깊도록 설정
    if d[a] > d[b]:
        a, b = b, a

    # 깊이 맞추기 (이진 점프)
    for i in range(LENGTH - 1, -1, -1):
        if d[b] - d[a] >= (1 << i): # 2**i
            b = parent[b][i]

    # 깊이를 맞췄더니 노드가 같아진 경우 (같은 경로에 있었음)
    if a == b:
        return a

    # 공통 조상 찾기 (이진 점프)
    for i in range(LENGTH - 1, -1, -1):
        # 2^i 점프해서 올라갔을 때 부모가 서로 다른 경우에만 이동
        if parent[a][i] != parent[b][i]:
            a = parent[a][i]
            b = parent[b][i]

    # 최종적으로 a와 b는 LCA 바로 아래에 위치하므로, a의 직속 부모가 LCA
    return parent[a][0]

set_parent()

m = int(input())

for _ in range(m):
    a, b = map(int, input().split())
    print(lca(a, b))
```

<br>
코드 분석

1. 부모 배열 생성 및 전처리 (set\_parent)

```python
def set_parent():
    dfs(1, 0) # 1단계: 직속 부모(parent[][0])와 깊이(d) 계산
    for i in range(1, LENGTH):
        for j in range(1, n + 1):
            # 2단계: 2^i 번째 부모 정보 갱신
            parent[j][i] = parent[parent[j][i - 1]][i - 1]
```

→ (이론 1 적용) set\_parent 함수는 두 부분으로 구성됩니다. 먼저 dfs를 호출해 모든 노드의 깊이와 2^0번째 부모(직속 부모)를 계산합니다. 그 후, 이중 for문을 통해 "나의 2^i번째 조상은 (나의 2^(i-1)번째 조상)의 2^(i-1)번째 조상이다" 라는 점화식을 그대로 코드로 구현하여 parent 배열 전체를 채웁니다. 이 과정이 O(N log N)의 전처리입니다.

2. 최소 공통 조상 찾기 (lca)

```python
# 깊이 맞추기
for i in range(LENGTH - 1, -1, -1):
    if d[b] - d[a] >= (1 << i):
        b = parent[b][i]
```

→ (이론 2 적용) lca 함수의 첫 번째 for문입니다. 두 노드의 깊이 차이를 2^i와 비교하며 가장 큰 점프(큰 i 값)부터 시도합니다. 깊이 차이가 2^i보다 크거나 같다면, 더 깊은 노드 b를 2^i만큼 위로 점프시킵니다. 이를 통해 깊이를 효율적으로 맞춰줍니다.

```python
# 공통 조상 찾기
for i in range(LENGTH - 1, -1, -1):
    if parent[a][i] != parent[b][i]:
        a = parent[a][i]
        b = parent[b][i]
return parent[a][0]
```

→ (이론 3 적용) 두 번째 for문입니다. 깊이가 같아진 a와 b를 동시에 위로 올립니다. 여기서 핵심은 parent\[a]\[i] != parent\[b]\[i] 조건입니다. 즉, 2^i만큼 점프해도 여전히 조상이 다르다면 아직 LCA에 도달하지 않은 것이므로 안전하게 점프합니다. 이 과정을 마치면, a와 b는 LCA 바로 한 칸 아래의 자식 노드가 됩니다. 따라서 parent\[a]\[0] (a의 직속 부모)를 반환하면 그것이 바로 LCA입니다.

정리

| 항목      | 설명                                   |
| ------- | ------------------------------------ |
| 알고리즘 유형 | 최소 공통 조상 (LCA) - 희소 배열/이진 리프팅        |
| 주요 조건   | 2^k 단위 점프를 이용한 깊이 맞추기 및 조상 탐색        |
| 핵심 자료구조 | 2차원 부모 배열 parent\[N]\[logN], 깊이 배열 d |
| 시간 복잡도  | 전처리: O(N log N), 쿼리: O(log N)        |
| 응용 예시   | 노드 간의 거리 쿼리, 동적 연결성 쿼리(LCT) 등        |

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
