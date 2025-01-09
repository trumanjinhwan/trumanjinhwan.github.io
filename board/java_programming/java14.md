---
layout: post
title: Java API Class 정리 2
date: 2025-01-09
---

> 이 포스트에서는 자주 사용되는 java.lang과 java.util 패키지의 클래스를 정리하고, 각각 메서드들의 활용 예제를 적어봤습니다.

## 1. java.lang 패키지

### 1.1 StringBuilder 클래스

`StringBuilder`는 문자열을 효율적으로 다룰 수 있도록 다양한 메서드를 제공합니다.

#### 주요 메서드
- `append()`: 문자열을 추가
- `insert()`: 특정 위치에 문자열 삽입
- `delete()`: 문자열의 특정 부분 삭제
- `deleteCharAt()`: 특정 위치의 문자 삭제
- `replace()`: 문자열의 특정 부분 교체
- `reverse()`: 문자열을 뒤집음
- `setCharAt()`: 특정 위치의 문자 설정

#### 예제

```java
public class StringBuilderExample {
    public static void main(String[] args) {
        StringBuilder sb = new StringBuilder("Hello");

        // append() 예제
        sb.append(" World");
        System.out.println(sb); // Hello World

        // insert() 예제
        sb.insert(6, "Java ");
        System.out.println(sb); // Hello Java World

        // delete() 예제
        sb.delete(6, 11);
        System.out.println(sb); // Hello World

        // deleteCharAt() 예제
        sb.deleteCharAt(5);
        System.out.println(sb); // HelloWorld

        // replace() 예제
        sb.replace(5, 10, "Java");
        System.out.println(sb); // HelloJava

        // reverse() 예제
        sb.reverse();
        System.out.println(sb); // avaJolleH

        // setCharAt() 예제
        sb.setCharAt(0, 'J');
        System.out.println(sb); // JavaJolleH
    }
}
```

#### 실행 결과

```java
Hello World
Hello Java World
Hello World
HelloWorld
HelloJava
avaJolleH
JavaJolleH
```

---

### 1.2 Math 클래스

`Math` 클래스는 수학 계산과 관련된 다양한 메서드를 제공합니다.

#### 주요 메서드
- `abs()`: 절댓값 반환
- `max()`, `min()`: 최대값, 최소값 반환
- `pow()`: 제곱 계산
- `sqrt()`: 제곱근 계산
- `random()`: 0.0 이상 1.0 미만의 랜덤값 반환

#### 예제

```java
public class MathExample {
    public static void main(String[] args) {
        // abs() 예제
        System.out.println("절댓값: " + Math.abs(-10)); // 10

        // max(), min() 예제
        System.out.println("최대값: " + Math.max(10, 20)); // 20
        System.out.println("최소값: " + Math.min(10, 20)); // 10

        // pow() 예제
        System.out.println("2의 3제곱: " + Math.pow(2, 3)); // 8.0

        // sqrt() 예제
        System.out.println("16의 제곱근: " + Math.sqrt(16)); // 4.0

        // random() 예제
        System.out.println("랜덤값: " + Math.random());
    }
}
```

#### 실행 결과

```java
절댓값: 10
최대값: 20
최소값: 10
2의 3제곱: 8.0
16의 제곱근: 4.0
랜덤값: (0.0 ~ 1.0 사이의 난수)
```

---

### 1.3 Wrapper 클래스

`Wrapper` 클래스는 기본형 데이터를 객체로 변환하여 다룰 수 있도록 제공합니다.

#### 주요 클래스와 메서드
- `Integer.parseInt()`, `Double.parseDouble()`: 문자열을 기본형으로 변환
- `valueOf()`: 기본형을 객체로 변환
- `toString()`: 기본형 또는 객체를 문자열로 변환

#### 예제

```java
public class WrapperExample {
    public static void main(String[] args) {
        // parseInt() 예제
        int num = Integer.parseInt("123");
        System.out.println("문자열을 정수로 변환: " + num);

        // valueOf() 예제
        Integer obj = Integer.valueOf(456);
        System.out.println("정수를 객체로 변환: " + obj);

        // toString() 예제
        String str = obj.toString();
        System.out.println("객체를 문자열로 변환: " + str);
    }
}
```

#### 실행 결과

```java
문자열을 정수로 변환: 123
정수를 객체로 변환: 456
객체를 문자열로 변환: 456
```

---

## 2. java.util 패키지

### 2.1 StringTokenizer 클래스

`StringTokenizer`는 문자열을 구분자로 나눌 때 사용됩니다.

#### 주요 메서드
- `countTokens()`: 남아 있는 토큰 수 반환
- `hasMoreTokens()`: 다음 토큰이 존재하는지 확인
- `nextToken()`: 다음 토큰 반환

#### 예제

```java
import java.util.StringTokenizer;

public class StringTokenizerExample {
    public static void main(String[] args) {
        String str = "Java,Python,C++,JavaScript";
        StringTokenizer tokenizer = new StringTokenizer(str, ",");

        // countTokens() 예제
        System.out.println("토큰 수: " + tokenizer.countTokens()); // 4

        // hasMoreTokens(), nextToken() 예제
        while (tokenizer.hasMoreTokens()) {
            System.out.println("토큰: " + tokenizer.nextToken());
        }
    }
}
```

#### 실행 결과

```java
토큰 수: 4
토큰: Java
토큰: Python
토큰: C++
토큰: JavaScript
```

---

### 2.2 Arrays 클래스

`Arrays`는 배열과 관련된 유용한 메서드를 제공합니다.

#### 주요 메서드
- `sort()`: 배열 정렬
- `binarySearch()`: 정렬된 배열에서 요소의 인덱스 검색
- `copyOf()`, `copyOfRange()`: 배열 복사
- `toString()`: 배열을 문자열로 변환

#### 예제

```java
import java.util.Arrays;

public class ArraysExample {
    public static void main(String[] args) {
        int[] numbers = {5, 3, 8, 1, 2};

        // sort() 예제
        Arrays.sort(numbers);
        System.out.println("정렬된 배열: " + Arrays.toString(numbers)); // [1, 2, 3, 5, 8]

        // binarySearch() 예제
        int index = Arrays.binarySearch(numbers, 3);
        System.out.println("숫자 3의 인덱스: " + index); // 2

        // copyOf() 예제
        int[] copied = Arrays.copyOf(numbers, 3);
        System.out.println("복사된 배열: " + Arrays.toString(copied)); // [1, 2, 3]

        // copyOfRange() 예제
        int[] rangeCopy = Arrays.copyOfRange(numbers, 1, 4);
        System.out.println("범위 복사된 배열: " + Arrays.toString(rangeCopy)); // [2, 3, 5]
    }
}
```

#### 실행 결과

```java
정렬된 배열: [1, 2, 3, 5, 8]
숫자 3의 인덱스: 2
복사된 배열: [1, 2, 3]
범위 복사된 배열: [2, 3, 5]
```

---

### 2.3 Random 클래스

`Random` 클래스는 난수를 생성하는 데 사용됩니다.

#### 주요 메서드
- `nextInt()`: 정수 난수 반환
- `nextDouble()`: 0.0 이상 1.0 미만의 난수 반환
- `nextBoolean()`: 랜덤한 `true` 또는 `false` 반환

#### 예제

```java
import java.util.Random;

public class RandomExample {
    public static void main(String[] args) {
        Random random = new Random();

        // nextInt() 예제
        System.out.println("정수 난수: " + random.nextInt(100)); // 0 ~ 99 사이의 난수

        // nextDouble() 예제
        System.out.println("실수 난수: " + random.nextDouble()); // 0.0 ~ 1.0

        // nextBoolean() 예제
        System.out.println("불리언 난수: " + random.nextBoolean());
    }
}
```

#### 실행 결과

```java
정수 난수: 42 (예시값, 실행 시마다 달라짐)
실수 난수: 0.756 (예시값, 실행 시마다 달라짐)
불리언 난수: true (예시값, 실행 시마다 달라짐)
```

---

### 2.4 Date 클래스

`Date`는 날짜와 시간을 다룰 때 사용됩니다.

#### 주요 메서드
- `getTime()`: 1970년 1월 1일 기준 밀리초 반환
- `toString()`: 날짜와 시간을 문자열로 반환

#### 예제

```java
import java.util.Date;

public class DateExample {
    public static void main(String[] args) {
        Date date = new Date();

        // toString() 예제
        System.out.println("현재 날짜와 시간: " + date.toString());

        // getTime() 예제
        System.out.println("현재 시간의 밀리초: " + date.getTime());
    }
}
```

#### 실행 결과

```java
현재 날짜와 시간: Thu Jan 09 14:23:45 KST 2025
현재 시간의 밀리초: 1704797025000 (예시값)
```

---


### 2.5 Calendar 클래스

`Calendar`는 날짜와 시간을 다룰 때 좀 더 세부적인 조작이 필요한 경우에 사용됩니다.

#### 주요 메서드
- `get()`: 특정 필드 값을 반환 (예: 연도, 월, 일 등)
- `set()`: 특정 필드 값 설정
- `add()`: 날짜에 연산 추가 (예: 일자 증가/감소)
- `getInstance()`: 현재 날짜와 시간 정보를 가진 Calendar 인스턴스 생성

#### 예제

```java
import java.util.Calendar;

public class CalendarExample {
    public static void main(String[] args) {
        // 현재 날짜와 시간
        Calendar calendar = Calendar.getInstance();

        // get() 예제
        int year = calendar.get(Calendar.YEAR);
        int month = calendar.get(Calendar.MONTH) + 1; // (0부터 시작하므로 원하는 결과를 얻으려면 +1해야함)
        int day = calendar.get(Calendar.DAY_OF_MONTH);
        System.out.println("현재 날짜: " + year + "-" + month + "-" + day);

        // set() 예제
        calendar.set(Calendar.YEAR, 2025);
        calendar.set(Calendar.MONTH, 0+1); // (0부터 시작하므로 원하는 결과를 얻으려면 +1해야함)
        calendar.set(Calendar.DAY_OF_MONTH, 25);
        System.out.println("설정한 날짜: " + calendar.get(Calendar.YEAR) + "-"
                + (calendar.get(Calendar.MONTH) + 1) + "-" + calendar.get(Calendar.DAY_OF_MONTH));

        // add() 예제
        calendar.add(Calendar.DAY_OF_MONTH, 10); // 10일 더하기
        System.out.println("10일 후: " + calendar.get(Calendar.YEAR) + "-"
                + (calendar.get(Calendar.MONTH) + 1) + "-" + calendar.get(Calendar.DAY_OF_MONTH));
    }
}
```

#### 실행 결과

```java
현재 날짜: 2025-1-9
설정한 날짜: 2022-12-25
10일 후: 2023-1-4
```

