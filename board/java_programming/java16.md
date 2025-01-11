---
layout: post
title: 16. Generic Type
date: 2025-01-10
---

> 제네릭(Generic)은 자바에서 타입의 안정성을 높이고, 코드 재사용성을 극대화할 수 있는 강력한 도구입니다. 이 글에서는 제네릭 타입이 무엇인지, 왜 사용하는지, 그리고 실용적인 예제를 통해 이를 정리해보겠습니다.


## 1. 제네릭 타입이란?

**제네릭(Generic)**은 클래스, 인터페이스, 메서드에서 사용할 데이터 타입을 미리 지정하지 않고, 사용할 때 구체적인 타입을 지정할 수 있도록 하는 자바의 기능입니다.

제네릭은 다음과 같은 문제를 해결하기 위해 등장했습니다:

1. **타입 안정성 보장**: 컴파일 시점에 타입을 체크하여 런타임 오류를 줄임.
2. **형 변환 필요 없음**: 불필요한 형 변환을 제거하여 코드의 가독성 및 유지보수성을 높임.

---

## 2. 왜 제네릭을 사용하는가?

제네릭을 사용하면 다음과 같은 장점을 얻을 수 있습니다:

1. **코드 재사용성**:
   - 다양한 데이터 타입을 처리할 수 있는 클래스나 메서드를 작성할 수 있습니다.
2. **컴파일 시 타입 체크**:
   - 런타임 예외를 줄이고, 안정성을 높입니다.
3. **명시적 캐스팅 제거**:
   - 형 변환 코드를 제거하여 가독성이 향상됩니다.

---

## 3. 제네릭 타입의 사용 예제

### 3.1 제네릭 클래스

제네릭 클래스는 클래스 선언에 타입 매개변수를 추가하여 다양한 데이터 타입을 처리할 수 있습니다.

```java
class Box<T> {
    private T item;

    public void setItem(T item) {
        this.item = item;
    }

    public T getItem() {
        return item;
    }
}

// 사용 예시
Box<String> stringBox = new Box<>();
stringBox.setItem("안녕하세요");
System.out.println(stringBox.getItem()); // 출력: 안녕하세요
```

### 3.2 제네릭 메서드

제네릭 메서드는 메서드 선언에 타입 매개변수를 추가하여 다양한 데이터 타입을 처리할 수 있습니다.

```java
class Util {
    public static <T> void printArray(T[] array) {
        for (T element : array) {
            System.out.println(element);
        }
    }
}

// 사용 예시
Integer[] intArray = {1, 2, 3};
String[] strArray = {"안", "녕", "하세요"};

Util.printArray(intArray); // 출력: 1, 2, 3
Util.printArray(strArray); // 출력: 안, 녕, 하세요
```

### 3.3 제네릭 제한 (Bounded Type Parameters)

제네릭 타입에 제한을 두어 특정 타입만 허용할 수 있습니다.

```java
class Calculator<T extends Number> {
    public double add(T a, T b) {
        return a.doubleValue() + b.doubleValue();
    }
}

// 사용 예시
Calculator<Integer> intCalc = new Calculator<>();
System.out.println(intCalc.add(10, 20)); // 출력: 30.0

Calculator<Double> doubleCalc = new Calculator<>();
System.out.println(doubleCalc.add(5.5, 3.3)); // 출력: 8.8
```

---

## 4. 실행 결과

### 4.1 제네릭 클래스 실행 결과

입력:

```java
Box<String> stringBox = new Box<>();
stringBox.setItem("안녕하세요");
System.out.println(stringBox.getItem());
```

출력:

```java
안녕하세요
```

### 4.2 제네릭 메서드 실행 결과

입력:

```java
Integer[] intArray = {1, 2, 3};
String[] strArray = {"안", "녕", "하세요"};
Util.printArray(intArray);
Util.printArray(strArray);
```
출력:

```java
1
2
3
안
녕
하세요
```

### 4.3 제네릭 제한 실행 결과

입력:

```java
Calculator<Integer> intCal = new Calculator<>();
System.out.println(intCal.add(10, 20));
```

출력:

```
30.0
```

---

## 5. 결론

제네릭은 자바에서 타입 안정성을 보장하고 코드의 재사용성을 극대화할 수 있는 중요한 도구입니다. 제네릭을 적절히 활용하면 가독성이 높고 유지보수가 용이한 코드를 작성할 수 있습니다. 제네릭 클래스, 메서드, 그리고 타입 제한 기능을 이용하여 한층 더 수준 높은 코드를 작성할 수 있습니다.
