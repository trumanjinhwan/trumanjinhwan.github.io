---
layout: post
title: 17. Collection Data Type
date: 2025-01-11
---

> 컬렉션(Collection)은 데이터를 효율적으로 저장하고 관리하기 위해 자바에서 제공하는 참조형 자료형입니다. 이 글에서는 List, Set, 그리고 Iterator의 활용법을 정리합니다.

---

## 1. List

**List**는 순서가 있는 데이터의 집합으로, 중복을 허용합니다. 대표적인 구현체로는 `ArrayList`, `LinkedList`, 그리고 `Vector`가 있습니다.

### 1.1 ArrayList

- **특징**:
  - 배열 기반으로 구현되어, 데이터 접근 속도가 빠름.
  - 크기가 동적으로 조정됨.

```java
import java.util.ArrayList;

class ArrayListExample {
    public static void main(String[] args) {
        ArrayList<String> list = new ArrayList<>();
        list.add("사과");
        list.add("바나나");
        list.add("체리");

        for (String item : list) {
            System.out.println(item);
        }
    }
}
```

**출력:**
```
사과
바나나
체리
```

### 1.2 LinkedList

- **특징**:
  - 노드(앞 요소의 값과 뒤 요소의 주소값이 연결) 기반으로 구현되어, 삽입과 삭제가 빠름.
  - 순차 접근에 적합.

```java
import java.util.LinkedList;

class LinkedListExample {
    public static void main(String[] args) {
        LinkedList<String> list = new LinkedList<>();
        list.add("봄");
        list.add("여름");
        list.add("가을");

        list.addFirst("겨울");
        for (String season : list) {
            System.out.println(season);
        }
    }
}
```

**출력:**
```
겨울
봄
여름
가을
```

### 1.3 Vector

- **특징**:
  - 동기화된(synchronized) 리스트 구조로, 멀티스레드 환경에서 안전하게 사용할 수 있음.
  - 크기가 동적으로 조정됨.

```java
import java.util.Vector;

class VectorExample {
    public static void main(String[] args) {
        Vector<String> vector = new Vector<>();
        vector.add("첫 번째 값");
        vector.add("두 번째 값");

        for (String value : vector) {
            System.out.println(value);
        }
    }
}
```

**출력:**
```
첫 번째 값
두 번째 값
```

---

## 2. Set

**Set**은 중복을 허용하지 않는 데이터의 집합입니다. 대표적인 구현체로는 `HashSet`과 `TreeSet`이 있습니다.

### 2.1 HashSet

- **특징**:
  - 해시 테이블("Key: Value" 값의 쌍) 기반으로 구현되어, 데이터 저장과 검색 속도가 빠름.
  - 순서를 보장하지 않음.

```java
import java.util.HashSet;

class HashSetExample {
    public static void main(String[] args) {
        HashSet<String> set = new HashSet<>();
        set.add("고양이");
        set.add("강아지");
        set.add("고양이"); // 중복된 항목

        for (String animal : set) {
            System.out.println(animal);
        }
    }
}
```

**출력:**
```
고양이
강아지
```
(순서는 실행 환경에 따라 달라질 수 있습니다.)

### 2.2 TreeSet

- **특징**:
  - 이진 트리 기반으로 구현되어, 데이터가 정렬된 상태로 저장됨.
  - 중복을 허용하지 않음.

```java
import java.util.TreeSet;

class TreeSetExample {
    public static void main(String[] args) {
        TreeSet<Integer> set = new TreeSet<>();
        set.add(10);
        set.add(5);
        set.add(20);

        for (Integer number : set) {
            System.out.println(number);
        }
    }
}
```

**출력:**
```
5
10
20
```

---

## 3. Iterator와의 응용

**Iterator**는 컬렉션에 저장된 데이터를 순차적으로 접근할 수 있는 객체입니다.

```java
import java.util.ArrayList;
import java.util.Iterator;

class IteratorExample {
    public static void main(String[] args) {
        ArrayList<String> list = new ArrayList<>();
        list.add("첫 번째");
        list.add("두 번째");
        list.add("세 번째");

        Iterator<String> iterator = list.iterator();
        while (iterator.hasNext()) {
            System.out.println(iterator.next());
        }
    }
}
```

**출력:**
```
첫 번째
두 번째
세 번째
```

---

## 4. 결론

컬렉션은 배열과 달리 가변적인 데이터를 효율적으로 관리하고 처리하는 데 필수적인 도구입니다. 이 글에서 살펴본 List, Set, 그리고 Iterator의 특징과 예제를 바탕으로 적절한 자료구조를 선택하여 프로젝트에 활용해본다면 더 많은 문제와 요구사항들을 해결할 수 있을 것입니다.

