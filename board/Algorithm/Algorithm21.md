---

layout: post
title: "21. 이진트리"
date: 2025-05-31
---

# 이진트리란?

이진트리는 **모든 노드가 최대 두 개의 자식 노드를 가지는 트리**입니다. 각 노드는 왼쪽 자식과 오른쪽 자식을 가질 수 있으며, 이 구조는 많은 알고리즘의 기반이 됩니다.

> **이진트리의 특징**
>
> * 자식 노드가 최대 2개 (왼쪽, 오른쪽)
> * 구조화된 탐색에 용이하여 **재귀적 접근이 자연스럽다**
> * 다양한 트리 구조로 확장 가능 (이진 탐색 트리, 힙, 세그먼트 트리 등)

<br>

# 이진트리의 종류

1. **편향 이진트리**: 자식이 한쪽으로만 치우쳐 있는 트리
2. **포화 이진트리**: 모든 레벨이 꽉 찬 이진트리
3. **완전 이진트리**: 마지막 레벨을 제외하고 노드가 꽉 차 있으며, 왼쪽부터 채워진 트리 (코딩 테스트에서 선호)

<br>

# 이진트리의 순차 표현 (배열 기반)

이진트리는 배열로도 표현할 수 있습니다. 노드 위치에 따른 인덱스 관계는 다음과 같습니다:

* 루트 노드: index = 1
* 부모 노드: index // 2
* 왼쪽 자식: index \* 2
* 오른쪽 자식: (index \* 2) + 1

> 이 구조는 힙이나 완전 이진트리 구현에서 자주 사용됩니다.

<br>

# 예제: 백준 1991번 "트리 순회"

## 문제 설명

* 트리의 노드 수 `N`과 루트 노드를 포함한 각 노드의 왼쪽, 오른쪽 자식 노드가 주어진다.
* 이를 기반으로 **전위 순회, 중위 순회, 후위 순회 결과**를 출력하라.

<br>

## 코드 구현

```python
import sys

N = int(sys.stdin.readline().strip())
tree = {}

# 트리 입력 저장
for n in range(N):
    root, left, right = sys.stdin.readline().strip().split()
    tree[root] = [left, right]

# 전위 순회 (Root - Left - Right)
def preorder(root):
    if root != '.':
        print(root, end='')
        preorder(tree[root][0])
        preorder(tree[root][1])

# 중위 순회 (Left - Root - Right)
def inorder(root):
    if root != '.':
        inorder(tree[root][0])
        print(root, end='')
        inorder(tree[root][1])

# 후위 순회 (Left - Right - Root)
def postorder(root):
    if root != '.':
        postorder(tree[root][0])
        postorder(tree[root][1])
        print(root, end='')

# 루트 노드 'A'부터 순회 시작
preorder('A')
print()
inorder('A')
print()
postorder('A')
```

<br>

## 코드 분석

### 1. 트리 저장 방식

```python
tree = {}
for n in range(N):
    root, left, right = sys.stdin.readline().strip().split()
    tree[root] = [left, right]
```

→ 노드를 키로 하고, 자식 노드들을 값으로 저장하여 **딕셔너리 기반 트리 구조**를 생성합니다.

---

### 2. 전위 순회 (Preorder)

```python
def preorder(root):
    if root != '.':
        print(root, end='')
        preorder(tree[root][0])
        preorder(tree[root][1])
```

→ 루트 → 왼쪽 → 오른쪽 순서로 순회. **연산을 먼저 수행하고 하위로 이동**합니다.

---

### 3. 중위 순회 (Inorder)

```python
def inorder(root):
    if root != '.':
        inorder(tree[root][0])
        print(root, end='')
        inorder(tree[root][1])
```

→ 왼쪽 → 루트 → 오른쪽 순서로 순회. **이진 탐색 트리에서는 오름차순 정렬된 결과**를 얻을 수 있습니다.

---

### 4. 후위 순회 (Postorder)

```python
def postorder(root):
    if root != '.':
        postorder(tree[root][0])
        postorder(tree[root][1])
        print(root, end='')
```

→ 왼쪽 → 오른쪽 → 루트 순서. **연산을 나중에 수행**하며 **삭제 연산 등에서 유용**합니다.

---

### 5. 출력 결과

```python
preorder('A')
print()
inorder('A')
print()
postorder('A')
```

→ 전위, 중위, 후위 순회 결과를 각각 출력합니다.

<br>

## 정리

| 항목      | 설명                             |
| ------- | ------------------------------ |
| 알고리즘 유형 | 이진트리 기반 순회                     |
| 주요 조건   | 루트 노드에서 재귀적 순회 진행              |
| 핵심 자료구조 | 딕셔너리 기반 트리 저장, 순회 함수           |
| 시간 복잡도  | O(N) - 노드 수만큼 탐색               |
| 응용 예시   | 연산 순서 제어, 삭제 연산, 이진 탐색 트리 정렬 등 |

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
