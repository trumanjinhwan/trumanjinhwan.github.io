---
layout: post
title: Java API Class 정리 1
date: 2025-01-08
---

> 이 포스트에서는 자주 사용되는 `java.lang`과 `java.util` 패키지의 클래스를 정리하고, 각각 메서드들의 활용 예제를 적어봤습니다.

## 1. java.lang 패키지

### 1.1 Object 클래스

`Object` 클래스는 자바의 모든 클래스가 기본적으로 상속하는 최상위 클래스입니다. 여기에는 객체 비교나 복제와 관련된 메서드들이 포함되어 있습니다.

#### 주요 메서드
- `equals()`: 객체의 동등성 비교
- `hashCode()`: 객체의 해시 코드 반환
- `toString()`: 객체를 문자열로 변환
- `clone()`: 객체의 복제

#### 예제

```java
public class ObjectExample {
    public static void main(String[] args) {
        Object obj1 = new Object();
        Object obj2 = new Object();

        // equals() 예제
        System.out.println("obj1.equals(obj2): " + obj1.equals(obj2)); // false

        // hashCode() 예제
        System.out.println("obj1.hashCode(): " + obj1.hashCode());

        // toString() 예제
        System.out.println("obj1.toString(): " + obj1.toString());
    }
}
```

#### 실행 결과

```java
obj1.equals(obj2): false
obj1.hashCode(): 1738891776
obj1.toString(): java.lang.Object@6730f10e
```

### 1.2 System 클래스
System 클래스는 시스템과 관련된 여러 가지 유용한 메서드를 제공합니다.

#### 주요 메서드
- `getProperty()`: 시스템 속성 값을 가져옴
- `getenv()`: 환경 변수 값을 가져옴

#### 예제

```java
public class SystemExample {
    public static void main(String[] args) {
        // getProperty() 예제
        String javaVersion = System.getProperty("java.version");
        System.out.println("Java 버전: " + javaVersion);

        // getenv() 예제
        String userHome = System.getenv("USERPROFILE");
        System.out.println("사용자 홈 디렉토리: " + userHome);
    }
}
```

#### 실행 결과

```java
Java 버전: 1.8.0_231
사용자 홈 디렉토리: C:\Users\Username
```

### 1.3 Class 클래스
Class 클래스는 객체의 클래스 정보를 제공하며, 클래스의 메서드, 필드 등을 다룰 수 있는 방법을 제공합니다.

#### 주요 메서드
- `getClass()`: 객체의 클래스 정보를 반환
- `forName()`: 클래스 이름을 문자열로 받아 클래스를 동적으로 로드
- `getDeclaredConstructors()`: 클래스의 생성자 목록 반환

#### 예제

```java
public class ClassExample {
    public static void main(String[] args) throws ClassNotFoundException {
        // getClass() 예제
        String str = "Hello";
        Class clazz = str.getClass();
        System.out.println("클래스 이름: " + clazz.getName());

        // forName() 예제
        Class loadedClass = Class.forName("java.lang.String");
        System.out.println("동적으로 로드된 클래스 이름: " + loadedClass.getName());
    }
}
```

#### 실행 결과

```java
클래스 이름: java.lang.String
동적으로 로드된 클래스 이름: java.lang.String
```

### 1.4 String 클래스
String 클래스는 문자열을 다루기 위한 다양한 메서드를 제공합니다.

#### 주요 메서드
- `charAt()`: 문자열에서 특정 인덱스의 문자 반환
- `indexOf()`: 특정 문자가 처음 등장하는 인덱스 반환
- `length()`: 문자열의 길이 반환
- `replace()`: 문자열에서 특정 문자를 다른 문자로 교체
- `substring()`: 문자열의 일부분을 잘라냄
- `toLowerCase()`: 문자열을 소문자로 변환
- `toUpperCase()`: 문자열을 대문자로 변환
- `trim()`: 문자열 양옆 공백 제거
- `valueOf()`: 다른 데이터 타입을 문자열로 변환

#### 예제

```java
public class StringExample {
    public static void main(String[] args) {
        String str = " Hello, World! ";

        // charAt() 예제
        System.out.println("첫 번째 문자: " + str.charAt(0));

        // indexOf() 예제
        System.out.println("문자 'o'의 위치: " + str.indexOf('o'));

        // length() 예제
        System.out.println("문자열 길이: " + str.length());

        // replace() 예제
        System.out.println("문자 'o'를 'O'로 교체: " + str.replace('o', 'O'));

        // substring() 예제
        System.out.println("부분 문자열: " + str.substring(7, 12));

        // toLowerCase() 예제
        System.out.println("소문자로 변환: " + str.toLowerCase());

        // toUpperCase() 예제
        System.out.println("대문자로 변환: " + str.toUpperCase());

        // trim() 예제
        System.out.println("양옆 공백 제거: " + str.trim());

        // valueOf() 예제
        int number = 123;
        System.out.println("정수 123을 문자열로 변환: " + String.valueOf(number));
    }
}
```

#### 실행 결과

```java
첫 번째 문자:  
문자 'o'의 위치: 4
문자열 길이: 15
문자 'o'를 'O'로 교체: HellO, WOrld! 
부분 문자열: World
소문자로 변환:  hello, world! 
대문자로 변환:  HELLO, WORLD! 
양옆 공백 제거: Hello, World!
정수 123을 문자열로 변환: 123
```

## 2. java.util 패키지

### 2.1 Objects 클래스
Objects 클래스는 객체와 관련된 여러 유용한 메서드를 제공합니다.

#### 주요 메서드
- `compare()`: 두 객체를 비교
- `equals()`: 두 객체가 같은지 비교
- `deepEquals()`: 두 객체의 깊은 비교

#### 예제

```java
import java.util.Objects;

public class ObjectsExample {
    public static void main(String[] args) {
        String str1 = "Hello";
        String str2 = "Hello";

        // compare() 예제
        System.out.println("두 객체 비교: " + Objects.compare(str1, str2, String::compareTo));

        // equals() 예제
        System.out.println("두 객체 같음: " + Objects.equals(str1, str2));

        // deepEquals() 예제
        String[] array1 = {"a", "b", "c"};
        String[] array2 = {"a", "b", "c"};
        System.out.println("두 배열 깊은 비교: " + Objects.deepEquals(array1, array2));
    }
}
```

#### 실행 결과

```java
두 객체 비교: 0
두 객체 같음: true
두 배열 깊은 비교: true
```
