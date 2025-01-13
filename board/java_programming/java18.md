---
layout: post
title: 18. Map
date: 2025-01-13
---
<div style="text-align: center;">
    <img src="/사진들/컬렉션 자료형/컬렉션 자료형2.png" alt="alt text" />
</div>

> 맵(Map)은 키-값 쌍으로 데이터를 저장하는 자바의 자료구조입니다. 파이썬의 '딕셔너리'와 유사하다고 볼 수 있습니다. 이 글에서는 `HashMap`, `Hashtable`, 그리고 Iterator를 활용한 맵 탐색 방법을 정리했습니다.



## 1. HashMap

**HashMap**은 키와 값을 한 쌍으로 저장하며, 빠른 검색 속도를 제공합니다. 

- **특징**:
  - 키는 중복될 수 없으며, 값은 중복될 수 있습니다.
  - 정렬 순서를 보장하지 않습니다.

```java
import java.util.HashMap;

class HashMapExample {
    public static void main(String[] args) {
        HashMap<String, String> map = new HashMap<>();
        map.put("사과", "Apple");
        map.put("바나나", "Banana");
        map.put("체리", "Cherry");

        System.out.println("전체 데이터:");
        for (String key : map.keySet()) {
            System.out.println(key + " -> " + map.get(key));
        }
    }
}
```


**출력:**
```
전체 데이터:
사과 -> Apple
바나나 -> Banana
체리 -> Cherry
```

---

## 2. Hashtable

**Hashtable**은 `HashMap`과 유사하지만, 동기화(synchronized)를 지원하여 멀티스레드 환경에서 안전하게 사용할 수 있습니다.

- **특징**:
  - 동기화된 구조로, 멀티스레드 환경에서 안전합니다.
  - 키와 값에 `null`을 허용하지 않습니다.

```java
import java.util.Hashtable;

class HashtableExample {
    public static void main(String[] args) {
        Hashtable<String, String> table = new Hashtable<>();
        table.put("봄", "Spring");
        table.put("여름", "Summer");
        table.put("가을", "Autumn");

        System.out.println("Hashtable 데이터:");
        for (String key : table.keySet()) {
            System.out.println(key + " -> " + table.get(key));
        }
    }
}
```

**출력:**
```
Hashtable 데이터:
봄 -> Spring
여름 -> Summer
가을 -> Autumn
```

---

## 3. Iterator와의 응용

**Iterator**를 사용하면 맵의 데이터를 반복적으로 탐색할 수 있습니다.

- **특징**:
  - `Iterator`는 컬렉션 데이터 구조를 순회하는 데 유용합니다.
  - 안전하게 요소를 추가하거나 삭제할 수 있습니다.

```java
import java.util.HashMap;
import java.util.Iterator;
import java.util.Map;

class IteratorExample {
    public static void main(String[] args) {
        Map<String, Integer> scores = new HashMap<>();
        scores.put("영희", 95);
        scores.put("철수", 85);
        scores.put("미나", 90);

        System.out.println("학생 점수:");
        Iterator<Map.Entry<String, Integer>> iterator = scores.entrySet().iterator();
        while (iterator.hasNext()) {
            Map.Entry<String, Integer> entry = iterator.next();
            System.out.println(entry.getKey() + " -> " + entry.getValue() + "점");
        }
    }
}
```

**출력:**
```
학생 점수:
영희 -> 95점
철수 -> 85점
미나 -> 90점
```

---

## 4. 결론

맵 자료형은 키-값 쌍으로 데이터를 관리하기 위한 강력한 도구입니다. `HashMap`과 `Hashtable`은 각각 비동기 및 동기 환경에서 효과적으로 사용할 수 있으며, `Iterator`를 활용하면 데이터를 효율적으로 탐색할 수 있습니다. 맵 자료형에서 가장 유의해야 할 점은 "키" 값은 중복이 안된다는 점입니다.
