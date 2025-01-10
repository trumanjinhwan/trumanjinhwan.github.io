---
layout: post
title: 15. Multi Thread
date: 2025-01-09
---

> 자바에서 멀티스레드 프로그래밍은 프로그램의 성능을 향상시키고, 동시에 여러 작업을 수행할 수 있도록 도와줍니다. 이 글에서는 `Thread`와 `Runnable`을 이용한 스레드 구현 방법과 각 방식의 특징을 살펴보겠습니다.


## 1. 스레드란?

스레드는 프로세스 내에서 실행되는 단위입니다. 여러 스레드를 사용하면 동시에 여러 작업을 수행할 수 있습니다. 

- **멀티스레드**: 여러 스레드가 동시에 작업을 수행하는 것.
- **장점**: 프로그램의 응답성을 높이고, CPU 자원을 효율적으로 사용.
- **단점**: 동기화 문제로 인해 복잡성이 증가.

---

## 2. 스레드 구현 방법

자바에서는 다음 두 가지 방법으로 스레드를 구현할 수 있습니다:

1. `Runnable` 인터페이스 구현
2. `Thread` 클래스 상속

---

### 2.1 `Runnable` 인터페이스 구현

`Runnable` 인터페이스를 구현하면 스레드를 생성할 수 있습니다. 이 방법은 클래스가 다른 클래스를 상속받아야 하는 경우에도 유연하게 사용할 수 있습니다.

#### 코드 예제

```java
class MyRunnable implements Runnable {
    @Override
    public void run() {
        for (int i = 1; i <= 5; i++) {
            System.out.println("Runnable 실행 중: " + i);
            try {
                Thread.sleep(1000); // 1초 대기
            } catch (InterruptedException e) {
                System.out.println("스레드 인터럽트 발생");
            }
        }
    }
}

public class RunnableExample {
    public static void main(String[] args) {
        Runnable myRunnable = new MyRunnable();
        Thread thread = new Thread(myRunnable);
        thread.start();
    }
}
```

#### 실행 결과
```java
Runnable 실행 중: 1
Runnable 실행 중: 2
Runnable 실행 중: 3
Runnable 실행 중: 4
Runnable 실행 중: 5
```

---

### 2.2 `Thread` 클래스 상속

`Thread` 클래스를 상속받아 스레드를 생성할 수도 있습니다. 이 방법은 클래스가 다른 클래스를 상속받을 필요가 없을 때 주로 사용합니다.

#### 코드 예제

```java
class MyThread extends Thread {
    @Override
    public void run() {
        for (int i = 1; i <= 5; i++) {
            System.out.println("Thread 실행 중: " + i);
            try {
                Thread.sleep(1000); // 1초 대기
            } catch (InterruptedException e) {
                System.out.println("스레드 인터럽트 예외 발생");
            }
        }
    }
}

public class ThreadExample {
    public static void main(String[] args) {
        Thread myThread = new MyThread();
        myThread.start();
    }
}
```

#### 실행 결과
```java
Thread 실행 중: 1
Thread 실행 중: 2
Thread 실행 중: 3
Thread 실행 중: 4
Thread 실행 중: 5
```

---

## 3. `Runnable` vs `Thread`

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

<table>
  <thead>
    <tr>
      <th>특징</th>
      <th>`Runnable` 인터페이스</th>
      <th>`Thread` 클래스</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>상속 여부</td>
      <td>다른 클래스 상속 가능</td>
      <td>추가 상속 불가능</td>
    </tr>
    <tr>
      <td>구조</td>
      <td>작업 단위만 분리</td>
      <td>스레드와 작업 단위가 동일</td>
    </tr>
    <tr>
      <td>유연성</td>
      <td>높음</td>
      <td>낮음</td>
    </tr>
    <tr>
      <td>재사용성</td>
      <td>높음</td>
      <td>낮음</td>
    </tr>
  </tbody>
</table>


---

## 4. 적합한 사용 상황

### 4.1 `Runnable`을 사용하는 경우

- 클래스가 다른 클래스를 상속받아야 할 때.
- 작업 단위와 스레드 구현을 분리하고 싶을 때.

### 4.2 `Thread`를 사용하는 경우

- 간단한 작업을 빠르게 구현할 때.
- 클래스가 다른 클래스를 상속받을 필요가 없을 때.

---

## 5. 결론

- `Runnable`은 재사용성과 유연성이 높아 일반적으로 더 권장됩니다.
- `Thread`는 간단한 작업에 적합하며, 상황에 맞게 두 방식을 선택하면 됩니다.

> 스레드 프로그래밍은 강력한 도구이지만, 잘못된 사용은 동기화 문제를 야기할 수 있습니다. 따라서 적절한 설계를 통해 안전하고 효율적인 코드를 작성해야 합니다.
