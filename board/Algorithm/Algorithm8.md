---
layout: post
title: "8. 이진 탐색"
date: 2025-03-05
---

> 이진 탐색(Binary Search)은 정렬된 배열에서 특정 값을 빠르게 찾는 알고리즘입니다. 시간 복잡도가 O(log N)으로, 선형 탐색(O(N))보다 훨씬 효율적입니다. 이번 글에서는 **이진 탐색의 개념과 구현 방법**을 설명하고, 이를 활용한 **백준 1920번 "수 찾기" 문제 풀이**를 다뤄보겠습니다.

## 1. 이진 탐색 개념

이진 탐색은 **정렬된 배열에서만** 사용할 수 있는 탐색 알고리즘으로, 다음과 같은 방식으로 진행됩니다:

1. 배열의 **중앙 값(mid)**을 선택합니다.
2. 찾고자 하는 값과 **중앙 값**을 비교합니다.
   - 중앙 값이 목표 값과 같다면 탐색 성공.
   - 중앙 값이 목표 값보다 크다면 **왼쪽 부분 배열**을 탐색.
   - 중앙 값이 목표 값보다 작다면 **오른쪽 부분 배열**을 탐색.
3. 이를 **재귀 또는 반복문**을 통해 수행하며, 탐색 범위를 절반씩 줄여나갑니다.

### **이진 탐색의 시간 복잡도**
- **O(log N)** → 데이터 크기가 커질수록 완전 탐색(Brute Forth)보다 훨씬 빠름.

---

## 2. 이진 탐색 구현 (반복 & 재귀 방식)

### **파이썬 (반복문 방식)**
```python
# 이진 탐색 함수 (반복문 방식)
def binary_search(arr, target):
    left, right = 0, len(arr) - 1
    
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] == target:
            return 1  # 찾았을 때
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    
    return 0  # 못 찾았을 때
```

### **자바 (반복문 방식)**
```java
public class BinarySearch {
    public static int binarySearch(int[] arr, int target) {
        int left = 0, right = arr.length - 1;
        
        while (left <= right) {
            int mid = (left + right) / 2;
            if (arr[mid] == target) {
                return 1;
            } else if (arr[mid] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return 0;
    }
}
```

---

## 3. 백준 1920번 "수 찾기" 문제 풀이

### **문제 설명**
> N개의 정수 중에서 특정한 수가 존재하는지 찾아야 한다.

**입력**
```
5
4 1 5 2 3
5
1 3 7 9 5
```

**출력**
```
1
1
0
0
1
```

### **파이썬 코드 구현**
```python
import sys

# 입력 받기
n = int(sys.stdin.readline().strip())
arr = list(map(int, sys.stdin.readline().split()))
m = int(sys.stdin.readline().strip())
check = list(map(int, sys.stdin.readline().split()))

arr.sort()  # 이진 탐색을 위해 정렬 필수

# 각 숫자에 대해 이진 탐색 수행
for target in check:
    left, right = 0, n - 1
    found = 0  # 찾으면 1, 못 찾으면 0
    
    while left <= right:
        mid = (left + right) // 2  # 중앙값 인덱스
        
        if arr[mid] == target:  # 값을 찾았으면 1 출력
            found = 1
            break
        elif arr[mid] < target:  # 중앙값보다 크면 오른쪽 탐색
            left = mid + 1
        else:  # 중앙값보다 작으면 왼쪽 탐색
            right = mid - 1
    
    print(found)  # 찾았으면 1, 못 찾았으면 0 출력
```

### **코드 설명**
1. **입력 받기**: `sys.stdin.readline()`을 사용하여 입력을 빠르게 처리.
2. **정렬 필수**: 이진 탐색을 수행하려면 배열이 정렬되어 있어야 함.
3. **이진 탐색 수행**:
   - `mid` 값을 기준으로 목표 값을 비교하여 탐색 범위를 좁혀감.
   - 목표 값을 찾으면 `1`을 출력, 못 찾으면 `0`을 출력.

---

## 4. 이진 탐색과 선형 탐색 비교

| 탐색 방법  | 시간 복잡도 | 특징 |
|------------|------------|--------------------------|
| 완전 탐색  | O(N)       | 순차적으로 탐색 |
| 이진 탐색  | O(log N)   | 정렬된 배열에서만 사용 가능 |
| BST 탐색   | O(log N) ~ O(N) | 트리 구조에서 탐색 |

---

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
