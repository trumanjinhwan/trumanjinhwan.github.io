---
layout: post
title: "AWT 이벤트 처리 - Event와 Listener 활용"
date: 2025-01-15
---
<div style="text-align: center;">
    <img src="/사진들/AWT/AWT_Tree/AWT3.png" alt="alt text" />
</div>
> AWT에서 이벤트(Event) 처리는 GUI 애플리케이션의 핵심입니다. 이 글에서는 이벤트와 리스너의 개념, 그리고 자주 사용하는 코딩 패턴과 예제를 정리하였습니다다.

## 1. 이벤트와 리스너란?

**이벤트(Event)**는 사용자와 프로그램 간의 상호작용을 나타냅니다. 예를 들어, 버튼을 클릭하거나 창을 닫는 행위가 이벤트에 해당합니다. **리스너(Listener)**는 이러한 이벤트를 감지하고 처리하는 인터페이스입니다.

---

## 2. 이벤트 처리 방법

AWT에서 이벤트를 처리하는 방법은 크게 네 가지로 나뉩니다:

1. 화면 클래스와 Event Handler 클래스가 같은 경우
2. 화면 클래스와 Event Handler 클래스가 다른 경우
3. 내부 클래스를 Event Handler로 사용하는 경우
4. 익명 클래스를 Event Handler로 사용하는 경우

이제 각 방법을 코드와 함께 살펴보겠습니다.

---

### 2.1 화면 클래스와 Event Handler 클래스가 같은 경우

```java
import java.awt.*;
import java.awt.event.*;

class SameClassHandlerExample extends Frame implements ActionListener {
    Button button;

    public SameClassHandlerExample() {
        button = new Button("클릭하세요");
        button.addActionListener(this); // 이벤트 핸들러 등록

        add(button);
        setSize(300, 200);
        setVisible(true);
    }

    @Override
    public void actionPerformed(ActionEvent e) {
        System.out.println("버튼 클릭 이벤트 발생!");
    }

    public static void main(String[] args) {
        new SameClassHandlerExample();
    }
}
```

**실행 결과:** 
<div style="text-align: center;">
    <img src="/사진들/AWT/Event/SameClassHandlerExample.png" alt="alt text" />
</div>

---

### 2.2 화면 클래스와 Event Handler 클래스가 다른 경우

```java
import java.awt.*;
import java.awt.event.*;

class SeparateHandlerExample extends Frame {
    Button button;

    public SeparateHandlerExample() {
        button = new Button("클릭하세요");
        button.addActionListener(new ButtonClickHandler()); // 별도 클래스 등록

        add(button);
        setSize(300, 200);
        setVisible(true);
    }

    public static void main(String[] args) {
        new SeparateHandlerExample();
    }
}

class ButtonClickHandler implements ActionListener {
    @Override
    public void actionPerformed(ActionEvent e) {
        System.out.println("별도의 클래스에서 버튼 클릭 이벤트 처리!");
    }
}
```

**실행 결과:**
<div style="text-align: center;">
    <img src="/사진들/AWT/Event/SeparateHandlerExample.png" alt="alt text" />
</div>

---

### 2.3 내부 클래스를 Event Handler로 사용하는 경우

```java
import java.awt.*;
import java.awt.event.*;

class InnerClassHandlerExample extends Frame {
    Button button;

    public InnerClassHandlerExample() {
        button = new Button("클릭하세요");

        button.addActionListener(new ButtonClickHandler()); // 내부 클래스 등록

        add(button);
        setSize(300, 200);
        setVisible(true);
    }

    private class ButtonClickHandler implements ActionListener {
        @Override
        public void actionPerformed(ActionEvent e) {
            System.out.println("내부 클래스에서 버튼 클릭 이벤트 처리!");
        }
    }

    public static void main(String[] args) {
        new InnerClassHandlerExample();
    }
}
```

**실행 결과:**
<div style="text-align: center;">
    <img src="/사진들/AWT/Event/InnerClassHandlerExample.png" alt="alt text" />
</div>

---

### 2.4 익명 클래스를 Event Handler로 사용하는 경우

```java
import java.awt.*;
import java.awt.event.*;

class AnonymousClassHandlerExample extends Frame {
    Button button;

    public AnonymousClassHandlerExample() {
        button = new Button("클릭하세요");

        button.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                System.out.println("익명 클래스에서 버튼 클릭 이벤트 처리!");
            }
        });

        add(button);
        setSize(300, 200);
        setVisible(true);
    }

    public static void main(String[] args) {
        new AnonymousClassHandlerExample();
    }
}
```

**실행 결과:**
<div style="text-align: center;">
    <img src="/사진들/AWT/Event/AnonymousClassHandlerExample.png" alt="alt text" />
</div>

---

## 3. Window 이벤트 처리

창을 닫는 이벤트를 처리하려면 `WindowListener` 또는 `WindowAdapter`를 사용합니다.

### 3.1 `WindowAdapter`와 `WindowListener` 비교

- **`WindowListener`**: 모든 창 이벤트를 처리해야 하는 인터페이스로, `windowOpened`, `windowClosing`, `windowClosed` 등 7개의 메서드를 구현해야 합니다.
- **`WindowAdapter`**: `WindowListener`의 추상 클래스로, 필요한 메서드만 오버라이딩하여 사용할 수 있습니다. 불필요한 메서드 구현을 피할 수 있어 간결한 코드 작성을 돕습니다.

### 3.2 windowClosing() 메서드 구현하기

`windowClosing()` 메서드는 창을 닫는 이벤트를 처리하는 핵심 메서드입니다. 다음은 이를 구현하기 위한 3단계입니다:

1. **`WindowAdapter` 상속**
   - 클래스를 정의하고 `WindowAdapter`를 상속받습니다.
2. **addWindowListener() 메서드 사용**
   - `addWindowListener` 메서드를 호출하여 이벤트 핸들러를 등록합니다.
3. **`windowClosing()` 메서드 오버라이딩**
   - 창 닫기 이벤트를 처리하는 코드를 `windowClosing` 메서드에 작성합니다.

### 3.3 `WindowAdapter`를 사용한 예제

```java
import java.awt.*;
import java.awt.event.*;

class WindowAdapterExample extends Frame {
    public WindowAdapterExample() {
        addWindowListener(new CustomWindowAdapter());

        setSize(300, 200);
        setVisible(true);
    }

    public static void main(String[] args) {
        new WindowAdapterExample();
    }
}

class CustomWindowAdapter extends WindowAdapter {
    @Override
    public void windowClosing(WindowEvent e) {
        ((Frame) e.getSource()).dispose(); // 창 닫기
    }
}
```

**실행 결과:** 창을 닫으면 비로소 이제야 닫힙니다!

---

## 4. 결론

AWT의 이벤트 처리 방법은 다양한 요구에 맞게 설계되었습니다. 이 글에서 정리한 네 가지 방법과 창 닫기 이벤트 처리를 응용하면 요구에 맞는 이벤트를 발생시킬 수 있습니다.

