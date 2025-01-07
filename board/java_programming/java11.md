---
layout: post
title: 11. Abstract Class vs Interface
date: 2025-01-07
---

> 객체지향 프로그래밍에서 추상클래스와 인터페이스는 상속을 활용할 수 있는 양대 산맥입니다. 이 포스트에서는 추상클래스와 인터페이스의 차이점과 각각의 사용 사례에 대해 정리해 보겠습니다.



## 1. 추상클래스(Abstract Class)

- **추상클래스**는 **일부 구현이 없는 메서드**를 가진 클래스입니다. 자식 클래스에서 이를 상속받아 구현하도록 강제할 수 있습니다.
- **추상 메서드**를 가질 수 있지만, 구현된 메서드도 포함할 수 있습니다.
- **상속**을 통해 재사용성을 높이고, 공통 기능을 정의하는 데 유용합니다.
- **단일 상속**만 허용되며, 하나의 클래스만 상속할 수 있습니다.

```java
abstract class Animal {
    // 추상 메서드
    public abstract void sound();

    // 구현된 메서드
    public void sleep() {
        System.out.println("동물이 자고 있습니다.");
    }
}

class Dog extends Animal {
    @Override
    public void sound() {
        System.out.println("개가 멍멍 소리를 낸다.");
    }
}
```

---

## 2. 인터페이스(Interface)

- **인터페이스**는 **모든 메서드가 추상 메서드**인 클래스입니다. 인터페이스를 구현한 클래스는 모든 메서드를 구현해야 합니다.
- **다중 상속**을 지원하여 여러 인터페이스를 구현할 수 있습니다.
- 자바 8 이후로 **default 메서드**를 통해 일부 구현을 가질 수 있지만, 기본적으로는 추상 메서드만 정의됩니다.

```java
interface Animal {
    // 추상 메서드
    void sound();

    // default 메서드
    default void sleep() {
        System.out.println("동물이 자고 있습니다.");
    }
}

class Dog implements Animal {
    @Override
    public void sound() {
        System.out.println("개가 멍멍 소리를 낸다.");
    }
}
```

---

## 3. 추상클래스와 인터페이스의 차이점

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
      <th>추상클래스</th>
      <th>인터페이스</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>다중 상속</td>
      <td>X (단일 상속)</td>
      <td>O (다중 구현 가능)</td>
    </tr>
    <tr>
      <td>메서드 구현</td>
      <td>일부 메서드 구현 가능</td>
      <td>기본적으로 추상 메서드만, default 메서드로 구현 가능</td>
    </tr>
    <tr>
      <td>상속/구현</td>
      <td>클래스가 상속</td>
      <td>클래스가 구현</td>
    </tr>
    <tr>
      <td>필드</td>
      <td>인스턴스 변수 가질 수 있음</td>
      <td>상수만 가능</td>
    </tr>
    <tr>
      <td>생성자</td>
      <td>가질 수 있음</td>
      <td>X</td>
    </tr>
  </tbody>
</table>


---

## 4. 각각의 적합한 사용 상황

### 4.1 추상클래스가 적합한 경우
1. **공통된 기능을 상속을 통해 공유할 때**:
   - 여러 클래스에서 공통적인 기능을 사용하면서 일부는 자식 클래스에서 구현하도록 할 때.
2. **클래스 내에서 상태를 관리하고 싶을 때**:
   - 인스턴스 변수나 생성자 등이 필요할 때.

```java
abstract class Shape {
    String color;

    public Shape(String color) {
        this.color = color;
    }

    public abstract void draw();
}

class Circle extends Shape {
    public Circle(String color) {
        super(color);
    }

    @Override
    public void draw() {
        System.out.println("원을 그린다.");
    }
}
```

### 4.2 인터페이스가 적합한 경우
1. **다중 상속이 필요할 때**:
   - 하나의 클래스가 여러 개의 다른 기능을 구현해야 할 때.
2. **클래스 간의 계약을 정의할 때**:
   - 여러 클래스가 공통의 메서드를 구현하도록 강제하고 싶을 때.

```java
interface Drawable {
    void draw();
}

interface Paintable {
    void paint();
}

class Circle implements Drawable, Paintable {
    @Override
    public void draw() {
        System.out.println("원을 그린다.");
    }

    @Override
    public void paint() {
        System.out.println("원을 칠한다.");
    }
}
```

---

## 5. 결론

- **추상클래스**는 공통된 기능을 구현하고, 상태를 관리하는 데 유용합니다. **상속**을 통해 코드의 재사용성을 높일 수 있습니다.
- **인터페이스**는 **다중 상속**이 필요하고, 여러 클래스가 동일한 메서드를 구현해야 하는 경우에 유용합니다.
- 두 개념 모두 적절한 사용 상황에서 활용하면 유지보수성과 확장성을 높일 수 있습니다.

> **정리**: 추상클래스와 인터페이스는 상속 구조를 설계할 때 중요한 역할을 하며, 각기 다른 목적을 가지고 있습니다. 상황에 맞게 선택하여 설계하는 것이 중요합니다.
