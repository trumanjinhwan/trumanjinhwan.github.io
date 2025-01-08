---
layout: post
title: 8. Encapsulation
date: 2025-01-07
---

> 객체 지향 프로그래밍(OOP)에서 캡슐화의 원리이자 핵심 개념들인 접근자(Getter), 설정자(Setter), 그리고 생성자(Constructor)의 차이점과 적합한 사용 시점을 정리해보았습니다.



## 1. 개념 정리

### 1.1 접근자(Getter)와 설정자(Setter)
- **접근자**는 객체의 필드 값을 반환하는 메서드입니다.
- **설정자**는 객체의 필드 값을 변경하는 메서드입니다.
- 두 메서드는 **캡슐화(Encapsulation)** 원칙을 따르며, 객체의 내부 데이터를 보호하고 제어하기 위해 사용됩니다.

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
        if (age > 0) { // 유효성 검사
            this.age = age;
        }
    }
}
```

---

### 1.2 생성자(Constructor)
- **생성자**는 객체 생성 시 호출되며, 초기값을 설정하는 특별한 메서드입니다.
- 메서드 이름은 클래스와 동일해야 하며, 반환 타입이 없습니다.
- 생성자는 **초기화 로직**을 캡슐화하여 객체를 일관된 상태로 유지합니다.

```java
public class Person {
    private String name;
    private int age;

    // 생성자
    public Person(String name, int age) {
        this.name = name;
        if (age > 0) { // 유효성 검사
            this.age = age;
        }
    }
}
```
---

## 2. 접근자/설정자와 생성자의 차이

<style>
  table {
    border-collapse: collapse;
    width: 100%;
  }
  table, th, td {
    border: 1px solid black;
  }
  th, td {
    padding: 10px;
    text-align: left;
  }
</style>

<style>
  table {
    border-collapse: collapse;
    width: 100%;
    margin: 0 auto; /* 테이블을 페이지 가운데로 정렬 */
  }
  table, th, td {
    border: 1px solid black;
  }
  th, td {
    padding: 10px;
    text-align: center; /* 셀의 내용을 가운데 정렬 */
  }
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
      <th>구분</th>
      <th>접근자/설정자</th>
      <th>생성자</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>역할</td>
      <td>필드 값을 조회하거나 변경</td>
      <td>객체 초기화</td>
    </tr>
    <tr>
      <td>호출 시점</td>
      <td>객체 생성 후에 호출</td>
      <td>객체 생성 시 자동 호출</td>
    </tr>
    <tr>
      <td>반환 타입</td>
      <td>반환 타입 존재</td>
      <td>반환 타입 없음</td>
    </tr>
    <tr>
      <td>유효성 검사</td>
      <td>설정자에서 수행 가능</td>
      <td>생성자 내부에서 수행 가능</td>
    </tr>
    <tr>
      <td>재사용성</td>
      <td>객체 생성 후에도 지속적으로 사용 가능</td>
      <td>객체 생성 시 1회 호출</td>
    </tr>
  </tbody>
</table>





---

## 3. 각각의 적합한 사용 상황

### 3.1 접근자/설정자가 적합한 경우
1. **객체 상태를 반복적으로 수정해야 하는 경우**
   - 예: 사용자의 이름이나 나이를 프로그램 실행 중 업데이트해야 할 때.
2. **특정 필드의 유효성을 개별적으로 제어해야 하는 경우**
   - 설정자에서 각 필드의 유효성 검사를 구현할 수 있습니다.

```java
public class Account {
    private double balance;

    public double getBalance() {
        return balance;
    }

    public void setBalance(double balance) {
        if (balance >= 0) { // 유효성 검사
            this.balance = balance;
        }
    }
}
```

---

### 3.2 생성자가 적합한 경우
1. **객체가 반드시 특정 초기 상태를 가져야 하는 경우**
   - 생성자는 객체를 생성과 동시에 초기화하여 객체를 불완전한 상태로 두지 않습니다.
2. **필드 초기화 로직이 복잡하거나 일관성을 유지해야 하는 경우**
   - 초기화 과정에서 필요한 모든 검증과 연산을 생성자 내부에서 처리합니다.

```java
public class Account {
    private double balance;

    public Account(double initialDeposit) {
        if (initialDeposit < 0) {
            throw new IllegalArgumentException("초기 잔액은 음수일 수 없습니다.");
        }
        this.balance = initialDeposit;
    }
}
```
---

## 4. 접근자/설정자 vs 생성자: 함께 사용할 때

1. **필드 초기화는 생성자, 이후 상태 변경은 설정자 사용**
   - 예: 사용자 객체 생성 시 이름과 나이를 초기화하되, 이후 상태 변경은 설정자를 사용.
2. **생성자를 통한 초기화 후 특정 필드만 노출**
   - 예: 생성자를 통해 `id`를 초기화하고, 설정자를 통해 `name`이나 `email`을 업데이트.

```java
public class User {
    private final String id; // 불변 필드
    private String name;
    private String email;

    // 생성자
    public User(String id, String name, String email) {
        this.id = id;
        this.name = name;
        this.email = email;
    }

    // 접근자
    public String getId() {
        return id;
    }

    public String getName() {
        return name;
    }

    public String getEmail() {
        return email;
    }

    // 설정자
    public void setName(String name) {
        this.name = name;
    }

    public void setEmail(String email) {
        this.email = email;
    }
}
```

---

## 5. 정리

- **접근자/설정자**는 객체의 상태를 외부에서 제어하거나 접근할 때 유용하며, 개별 필드에 대한 유효성 검사를 수행할 수 있습니다.
- **생성자**는 객체 생성 시 일관된 초기 상태를 설정하고, 초기화 과정을 캡슐화하는 데 적합합니다.
- **활용 팁**:
  - 생성자로 필수 필드를 초기화하고, 설정자를 통해 선택적인 필드나 동적 변경이 필요한 데이터를 관리하세요.
  - 불변 객체(Immutable Object)를 생성하려면 필드를 `final`로 선언하고, 생성자로 초기화한 뒤 설정자를 제공하지 않는 방식이 효과적입니다.

```java
public final class ImmutableUser {
    private final String id;
    private final String name;

    public ImmutableUser(String id, String name) {
        this.id = id;
        this.name = name;
    }

    public String getId() {
        return id;
    }

    public String getName() {
        return name;
    }
}
```

> **결론**: "생성자"와 "접근자&설정자" 둘 다 설정해놓고 필요에 따라 인스턴스 생성 시점에 "생성자"를 이용해서 객체 변수를 설정할 수도 있고 인스턴스 생성 후에 객체 변수의 값을 변경할 일이 있다면 "접근자&설정자"를 이용할 수도 있는 활용성 있는 설계를 하면 좋을 것 같습니다.
