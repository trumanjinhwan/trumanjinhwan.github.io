---
layout: post
title: 7. O.O.P(Object-Oriented Programming)
date: 2025-01-02
---
<div style="text-align: center;">
    <img src="/사진들/객체지향/객체지향1.png" alt="alt text" />
</div>

> 객체지향 프로그래밍(Object-Oriented Programming, OOP)은 소프트웨어 개발 패러다임 중 하나로, 데이터를 객체로 묶어 설계하는 방법론입니다.

## 1. 등장배경

객체지향 프로그래밍은 1970년대 초에 절차적 프로그래밍의 한계를 보완하기 위해 등장했습니다. 복잡한 소프트웨어 시스템에서 모듈화를 강조하며, 데이터와 이를 처리하는 메서드를 하나로 묶어 재사용성과 유지보수성을 높이는 데 중점을 둡니다.

### 절차적 프로그래밍의 한계
- 코드 중복 문제
- 함수와 데이터의 분리로 인해 유지보수 어려움
- 프로그램이 커질수록 복잡도가 증가

OOP는 이러한 문제를 해결하기 위해 **캡슐화**, **상속성**, **다형성**이라는 세 가지 주요 특징을 기반으로 개발되었습니다.

---

## 2. 객체지향의 특징

### 2.1 캡슐화 (Encapsulation)

캡슐화는 데이터와 메서드를 하나의 클래스 내부에 숨기고, 외부에서 접근할 수 있는 인터페이스만 제공하는 것을 의미합니다. 이를 통해 데이터 무결성을 유지하고, 객체 간 상호작용을 명확히 할 수 있습니다.

```java
public class Person {
    private String name; // 필드
    private int age;

    // 접근자 (Getter)
    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    // 설정자 (Setter)
    public void setName(String name) {
        this.name = name;
    }

    public void setAge(int age) {
        if (age > 0) {
            this.age = age;
        }
    }
}
```

> **캡슐화의 장점**
> - 데이터를 보호하고 무결성을 유지
> - 코드의 변경이 외부에 영향을 덜 미침
> - 클라이언트 측에선 내부 로직을 알 필요가 없음

### 2.2 상속성 (Inheritance)

상속은 기존 클래스의 속성과 메서드를 새로운 클래스에서 재사용하거나 확장할 수 있도록 지원하는 기능입니다. 이를 통해 코드의 재사용성을 극대화할 수 있습니다.

```java
class Animal {
    void sound() {
        System.out.println("소리를 냅니다.");
    }
}

class Dog extends Animal {  // class 자식클래스 extends 부모클래스
    @Override
    void sound() {
        System.out.println("멍멍!");
    }
}
```

> **상속성의 장점**
> - 코드 재사용성 증대
> - 계층적 설계를 통해 구조적인 코드를 작성 가능
> - 상속 받은 클래스를 바탕으로 유연하고 확장성 있는 로직 구현

### 2.3 다형성 (Polymorphism)

다형성은 동일한 메서드나 연산이 다양한 데이터 타입에서 동작할 수 있는 능력을 의미합니다. 이를 통해 유연하고 확장 가능한 코드를 작성할 수 있습니다.

#### 오버로딩 vs 오버라이딩

- **오버로딩**: 같은 이름의 메서드를 매개변수의 타입, 개수, 선언된 순서에 따라 여러 개 정의하는 것

```java
class MathUtils {
    int add(int a, int b) {
        return a + b;
    }

    double add(double a, double b) {
        return a + b;
    }
}
```

- **오버라이딩**: 상위 클래스에서 정의된 메서드를 하위 클래스에서 재정의하는 것

```java
class Parent {
    void greet() {
        System.out.println("안녕하세요!");
    }
}

class Child extends Parent {
    @Override
    void greet() {
        System.out.println("안녕하세요!, 반갑습니다~");
    }
}
```

---

## 3. 클래스의 구성 요소

### 3.1 필드 (Field)와 메서드 (Method)

- **필드**: 클래스가 가지는 데이터 (속성)
- **메서드**: 클래스가 수행하는 동작 (행위)

```java
public class Car {
    String model; // 필드

    void drive() { // 메서드
        System.out.println(model + "가 달립니다.");
    }
}
```

### 3.2 생성자 (Constructor)

- 생성자는 객체 생성 시 호출되는 특별한 메서드로, 클래스 이름과 동일해야 하며 반환 타입이 없습니다.

```java
public class Car {
    String model;

    // 생성자
    public Car(String model) {
        this.model = model;
    }
}
```

### 3.3 한 클래스 파일 당 하나의 클래스

Java는 한 클래스 파일 당 하나의 public 클래스를 가지는 것을 권장합니다. 이를 통해 코드의 명확성을 높이고 파일 관리를 용이하게 합니다.

---

## 정리

객체지향 프로그래밍은 현대 소프트웨어 설계의 핵심 철학으로, **캡슐화**, **상속성**, **다형성**이라는 특징을 통해 모듈화와 코드 재사용성을 극대화합니다. 이를 제대로 이해하고 활용하면 더 간결하고 유지보수하기 쉬운 코드를 작성할 수 있습니다. 개발자는 객체지향의 원칙을 이해하고, 이를 실제 프로젝트에 어떻게 응용할지 고민해야 합니다.

