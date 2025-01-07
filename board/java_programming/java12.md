---
layout: post
title: 12. Exception Handling
date: 2025-01-07
---

> 예외 처리는 프로그램의 안정성과 가독성을 높이는 중요한 요소입니다. 이 글에서는 예외 처리의 기본 개념, 예외 종류, 그리고 예외를 처리하는 방법을 정리해보겠습니다.



## 1. 예외 처리란?

예외 처리(Exception Handling)는 프로그램 실행 중 발생할 수 있는 예기치 않은 오류를 처리하는 기술입니다. 예외를 적절히 처리하면 프로그램이 중단되지 않고, 오류에 대한 적절한 대응을 할 수 있습니다.

---

## 2. 예외의 종류

자바에서 예외는 크게 두 가지로 나눌 수 있습니다:

1. **일반 예외**: 
   - 컴파일 타임에 확인되는 예외로, `try-catch` 블록으로 처리하거나 메서드 서명에 `throws`를 명시해야 합니다.
   - 예: `IOException`, `SQLException`

2. **실행 예외**:
   - 런타임 예외로, `RuntimeException`을 상속받은 예외들입니다. 주로 프로그래머의 실수로 발생하며, 명시적인 처리가 없어도 컴파일이 통과됩니다.
   - 예: `NullPointerException`, `ArrayIndexOutOfBoundsException`

---

## 3. 예외 처리 방법

### 3.1 `try-catch` 블록 사용

`try` 블록에서 예외가 발생할 수 있는 코드를 작성하고, `catch` 블록에서 발생한 예외를 처리합니다.

```java
try {
    int result = 10 / 0;  // 예외 발생
} catch (ArithmeticException e) {
    System.out.println("0으로 나눌 수 없습니다!");
}
```
---

### 3.2 `throws`를 이용한 예외 던지기

메서드가 예외를 처리할 책임을 호출한 곳에 위임하려면 `throws` 키워드를 사용합니다.

```java
public void readFile(String filename) throws IOException {
    FileReader file = new FileReader(filename);
    BufferedReader reader = new BufferedReader(file);
    String line = reader.readLine();
}
```

---

### 3.3 `throw`를 이용한 예외 발생

`throw`는 예외를 **강제로 발생**시키는 키워드입니다. 메서드 내부에서 예외를 발생시킬 때 사용합니다. 예외 객체를 생성하고 `throw` 키워드를 사용해 던질 수 있습니다.

```java
// 예제 코드: throw 사용
public void validateAge(int age) {
    if (age < 0) {
        throw new IllegalArgumentException("나이는 음수일 수 없습니다.");
    }
}
```

---

### 3.4 `finally` 블록

`finally` 블록은 예외가 발생하든 안 하든 반드시 실행되는 코드입니다. 주로 자원 해제 작업에 사용됩니다.

```java
// 예제 코드: finally 블록
try {
    int[] arr = new int[5];
    arr[10] = 3;  // 예외 발생
} catch (ArrayIndexOutOfBoundsException e) {
    System.out.println("배열 인덱스 초과");
} finally {
    System.out.println("finally 블록 실행");
}
```

---

## 4. `catch` 절의 예외 처리 순서

`catch` 절에서 예외를 처리할 때, 상위 예외 클래스는 항상 하위 예외 클래스보다 **아래**에 위치해야 합니다. 자바에서 예외는 계층 구조를 가지므로, 상위 예외 클래스가 먼저 오면 하위 예외 클래스가 처리되지 않고, 컴파일 오류가 발생합니다.

### 예외 처리 순서

- **상위 예외 클래스**는 **하위 예외 클래스**보다 **나중에** 나와야 합니다.
- 예를 들어, `Exception`과 같은 상위 클래스는 구체적인 예외인 `FileNotFoundException`보다 뒤에 위치해야 합니다.

```java
// 잘못된 순서 (컴파일 오류 발생)
try {
    // 예외 발생 코드
} catch (Exception e) {
    System.out.println("예외 처리");
} catch (FileNotFoundException e) {
    System.out.println("파일을 찾을 수 없습니다.");
}
```

위와 같이 작성하면 컴파일 오류가 발생합니다. 이유는 `FileNotFoundException`이 `Exception`의 하위 클래스이므로, `Exception`이 먼저 오면 `FileNotFoundException`은 처리되지 않기 때문입니다.

### 올바른 순서

```java
// 올바른 순서
try {
    // 예외 발생 코드
} catch (FileNotFoundException e) {
    System.out.println("파일을 찾을 수 없습니다.");
} catch (Exception e) {
    System.out.println("일반적인 예외 처리");
}
```

이와 같이 구체적인 예외를 먼저 처리하고, 상위 예외를 나중에 처리해야 합니다.

---

## 5. 예외 처리에서의 고려사항

### 5.1 구체적인 예외 처리

구체적인 예외를 처리하는 것이 중요합니다. 예외가 발생할 수 있는 위치에 대한 예외를 명확하게 처리해주어야 합니다.

### 5.2 예외를 너무 많이 던지지 않기

불필요하게 예외를 던지는 것은 코드 가독성을 떨어뜨리고, 성능에도 악영향을 줄 수 있습니다. 예외가 필요한 경우에만 던지는 것이 중요합니다.

### 5.3 예외 메시지 추가

예외 발생 시 충분히 설명적인 메시지를 제공하여, 나중에 문제를 해결하는 데 도움을 줄 수 있도록 합니다.

```java
// 예제 코드: 예외 메시지 추가
try {
    int[] arr = new int[5];
    arr[10] = 3;
} catch (ArrayIndexOutOfBoundsException e) {
    System.out.println("배열의 크기를 초과했습니다. 배열 크기: 5, 접근한 인덱스: 10");
}
```

---

## 6. 결론

예외 처리는 프로그램의 안정성을 높이고, 예상치 못한 오류가 발생했을 때 적절한 대응을 할 수 있게 합니다. 예외 처리의 다양한 방법을 적절히 사용하면 더 안전하고 유지보수가 용이한 코드를 작성할 수 있습니다.
