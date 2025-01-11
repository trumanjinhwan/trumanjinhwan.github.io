---
layout: post
title: 10. Polymorphism
date: 2025-01-07
---

> 다형성은 객체 지향 프로그래밍의 핵심 개념 중 하나로, 같은 메서드나 연산자가 다양한 방식으로 동작할 수 있도록 합니다. 이 포스트에서는 다형성의 주요 특징인 오버로딩, 오버라이딩, IS-A 관계에 대해 다뤄보겠습니다.



## 1. 개념 정리

### 1.1 오버로딩(Overloading)
- **오버로딩**은 **같은 이름의 메서드**를 매개변수의 개수나 타입에 따라 다르게 정의하는 기법입니다.
- 반환 타입은 다를 수 없습니다. 메서드 이름이 동일하면 매개변수가 달라야 구분할 수 있습니다.

```java
class Calculator {
    // 두 정수의 합
    public int add(int a, int b) {
        return a + b;
    }

    // 세 정수의 합
    public int add(int a, int b, int c) {
        return a + b + c;
    }
}
```

### 1.2 오버라이딩(Overriding)
- **오버라이딩**은 **부모 클래스의 메서드**를 자식 클래스에서 **재정의**하여 사용하는 기법입니다.
- 부모 클래스에서 정의된 메서드를 자식 클래스에서 다시 구현하여 **동적 바인딩**을 통해 호출됩니다.

```java
class Animal {
    public void sound() {
        System.out.println("동물이 소리를 낸다.");
    }
}

class Dog extends Animal {
    @Override
    public void sound() {
        System.out.println("개가 멍멍 소리를 낸다.");
    }
}
```

### 1.3 IS-A 관계
- **IS-A 관계**는 **부모 클래스의 타입**을 **자식 클래스**가 그대로 물려받는 관계를 의미합니다.
- 부모 클래스의 타입으로 자식 클래스의 객체를 참조할 수 있는 **다형성**을 지원합니다.

```java
class Animal {
    public void sound() {
        System.out.println("동물이 소리를 낸다.");
    }
}

class Dog extends Animal {
    @Override
    public void sound() {
        System.out.println("개가 멍멍 소리를 낸다.");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal animal = new Dog(); // IS-A 관계
        animal.sound(); // 개가 멍멍 소리를 낸다.
    }
}
```

---

## 2. 다형성의 활용

- 다형성은 코드의 **유연성**을 증가시키고, **확장성**을 높이는 중요한 개념입니다.
- 부모 클래스의 타입으로 자식 클래스의 객체를 처리할 수 있기 때문에, 같은 타입으로 다양한 객체를 다룰 수 있습니다.

---

## 3. 결론

- **오버로딩**은 메서드 이름을 재사용하면서 다양한 파라미터를 처리할 수 있게 도와주며, **오버라이딩**은 부모 클래스의 메서드를 자식 클래스에서 동적으로 변경하여 유연한 코드 작성을 가능하게 합니다.
- **IS-A 관계**는 부모 클래스로 자식 클래스를 참조할 수 있게 하여 다형성을 지원합니다. 이를 통해 프로그램의 확장성과 유지보수성을 높일 수 있습니다.
