---
layout: post
title: "19. AWT1 - LayoutManager"
date: 2025-01-14
---
<div style="text-align: center;">
    <img src="/사진들/AWT/AWT_Tree/AWT2.png" alt="alt text" />
</div>
> 이 글에서는 AWT의 LayoutManager를 사용하여 다양한 레이아웃을 구성하는 방법을 정리하고, 자주 사용되는 레이아웃들의 특징과 예제를 살펴보겠습니다.


## 1. LayoutManager란?

**LayoutManager**는 AWT 컴포넌트(Container)에 포함된 구성 요소(Component)의 위치와 크기를 관리하는 인터페이스입니다. 기본적으로 AWT에서 제공하는 주요 LayoutManager로는 다음과 같은 것들이 있습니다:

- **BorderLayout**
- **FlowLayout**
- **GridLayout**

이 레이아웃 매니저를 사용하면 창의 크기가 변경될 때 컴포넌트의 배치가 자동으로 조정됩니다.

---

## 2. 주요 LayoutManager 종류 및 활용

### 2.1 BorderLayout

`BorderLayout`은 컴포넌트를 ```North```, ```South```, ```East```, ```West```, ```Center```의 다섯 영역으로 나누어 배치합니다. 기본 레이아웃으로 많이 사용됩니다.

```java
import java.awt.*;

class BorderLayoutExample {
    public static void main(String[] args) {
        Frame frame = new Frame("BorderLayout 예제");
        frame.setLayout(new BorderLayout());

        Button northButton = new Button("북");
        Button southButton = new Button("남");
        Button eastButton = new Button("동");
        Button westButton = new Button("서");
        Button centerButton = new Button("중앙");

        frame.add(northButton, BorderLayout.NORTH);
        frame.add(southButton, BorderLayout.SOUTH);
        frame.add(eastButton, BorderLayout.EAST);
        frame.add(westButton, BorderLayout.WEST);
        frame.add(centerButton, BorderLayout.CENTER);

        frame.setSize(400, 300);
        frame.setVisible(true);
    }
}
```

**출력:**
<div style="text-align: center;">
    <img src="/사진들/AWT/LayoutManager/BorderLayout예제.png" alt="alt text" />
</div>

### 2.2 FlowLayout

`FlowLayout`은 컴포넌트를 왼쪽에서 오른쪽으로, 한 줄씩 추가하며 배치합니다. 줄이 넘어가면 다음 줄로 자동으로 이동합니다.

```java
import java.awt.*;

class FlowLayoutExample {
    public static void main(String[] args) {
        Frame frame = new Frame("FlowLayout 예제");
        frame.setLayout(new FlowLayout());

        for (int i = 1; i <= 5; i++) {
            frame.add(new Button("버튼 " + i));
        }

        frame.setSize(300, 200);
        frame.setVisible(true);
    }
}
```

**출력:**
<div style="text-align: center;">
    <img src="/사진들/AWT/LayoutManager/FlowLayout.png" alt="alt text" />
</div>

---

### 2.3 GridLayout

`GridLayout`은 행(Row)과 열(Column)을 지정하여 격자 형태로 컴포넌트를 배치합니다. 모든 셀이 동일한 크기를 갖습니다.

```java
import java.awt.*;

class GridLayoutExample {
    public static void main(String[] args) {
        Frame frame = new Frame("GridLayout 예제");
        frame.setLayout(new GridLayout(2, 3)); // 2행 3열

        for (int i = 1; i <= 6; i++) {
            frame.add(new Button("버튼 " + i));
        }

        frame.setSize(400, 200);
        frame.setVisible(true);
    }
}
```

**출력:**
<div style="text-align: center;">
    <img src="/사진들/AWT/LayoutManager/GridLayout예제.png" alt="alt text" />
</div>

---

## 3. Panel과 LayoutManager

`Panel`은 컨테이너로서, 자체적으로 LayoutManager를 설정하여 다른 컴포넌트를 배치할 수 있습니다. 중첩된 레이아웃을 구성할 때 유용합니다.

```java
Frame frame = new Frame("Panel과 LayoutManager 예제");
        frame.setLayout(new BorderLayout());
        TextField txt = new TextField("텍스트를 입력하세요.");
        

        Panel panel = new Panel();
        panel.setLayout(new BorderLayout());
        panel.setBackground(Color.blue);
        
        frame.add(txt, BorderLayout.NORTH);
        frame.add(panel, BorderLayout.CENTER);

        frame.setSize(400, 300);
        frame.setVisible(true);
```

**출력:**
<div style="text-align: center;">
    <img src="/사진들/AWT/LayoutManager/Panel예제.png" alt="alt text" />
</div>

---

## 4. 결론

`LayoutManager`를 활용하면 프레임에다 다양한 방식으로 컴포넌트들을을 배치할 수 있습니다. `BorderLayout`, `FlowLayout`, `GridLayout`은 기본적이지만 강력한 기능을 제공하며, `Panel`과 결합하면 더욱 유연한 레이아웃 구성이 가능합니다.