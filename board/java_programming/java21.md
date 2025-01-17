---
layout: post
title: "21. AWT3 - Component"
date: 2025-01-16
---
<div style="text-align: center;">
    <img src="/사진들/AWT/AWT_Tree/AWT4.png" alt="alt text" />
</div>
> 이 글에서는 AWT의 핵심 컴포넌트(Component)에 대해 정리하고, 자주 사용하는 주요 컴포넌트의 예제와 주요 메서드들을 정리했습니다.

## 1. Component란?

**Component**는 AWT에서 제공하는 UI 요소의 기본 클래스입니다. 버튼, 텍스트 필드, 체크박스 등 다양한 GUI 요소가 모두 `Component`를 상속받아 구현됩니다.

AWT에서 제공하는 주요 컴포넌트는 다음과 같습니다:

- **Button**: 버튼을 생성하는 컴포넌트
- **Label**: 텍스트를 표시하는 컴포넌트
- **TextField**: 단일 행 텍스트 입력 필드
- **TextArea**: 다중 행 텍스트 입력 영역
- **Checkbox**: 체크박스 컴포넌트
- **Choice**: 드롭다운 리스트
- **List**: 선택 가능한 항목 리스트
- **Frame**: 기본 창 컴포넌트
- **Canvas**: 사용자 정의 그래픽을 그리기 위한 컴포넌트
- **Dialog**: 팝업 대화 상자
- **FileDialog**: 파일 열기/저장 대화 상자
- **Menu** 및 **PopupMenu**: 메뉴 구성 요소

---

## 2. 주요 Component 사용법

### 2.1 Frame
**Frame**은 AWT의 최상위 창입니다. 애플리케이션의 기본 창으로 사용되며, 다른 컴포넌트를 추가할 수 있습니다.

```java
import java.awt.*;

class FrameExample {
    public static void main(String[] args) {
        Frame frame = new Frame("Frame 예제");
        frame.setSize(400, 300);
        frame.setLayout(new FlowLayout());

        Button button = new Button("클릭");
        frame.add(button);

        frame.setVisible(true);
    }
}
```
**실행 결과:**
<div style="text-align: center;">
    <img src="/사진들/AWT/Component/FrameExample.png" alt="alt text" />
</div>

**주요 메서드:**
- `setTitle(String title)`: 창 제목 설정
- `setSize(int width, int height)`: 창 크기 설정
- `setVisible(boolean visible)`: 창 표시/숨김
- `add(Component comp)`: 프레임에 컴포넌트 추가
- `setLayout(LayoutManager mgr)`: 레이아웃 매니저 설정

---

### 2.2 Canvas
**Canvas**는 사용자 정의 그래픽을 그리기 위한 컴포넌트입니다. `paint(Graphics g)` 메서드를 오버라이드하여 원하는 그래픽을 그릴 수 있습니다.

```java
import java.awt.*;

class CanvasExample extends Canvas {
    public void paint(Graphics g) {
        g.setColor(Color.BLUE);
        g.fillRect(50, 50, 200, 100);
        g.setColor(Color.WHITE);
        g.drawString("안녕하세요요!", 90, 100);
    }

    public static void main(String[] args) {
        Frame frame = new Frame("Canvas 예제");
        CanvasExample canvas = new CanvasExample();

        frame.add(canvas);
        frame.setSize(300, 200);
        frame.setVisible(true);
    }
}
```
**실행 결과:**
<div style="text-align: center;">
    <img src="/사진들/AWT/Component/CanvasExample.png" alt="alt text" />
</div>
---

### 2.3 Dialog
**Dialog**는 부모 창에 종속된 팝업 창입니다. 모달 대화 상자로 설정하여 사용자 입력을 차단할 수 있습니다.

```java
import java.awt.*;

class DialogExample {
    public static void main(String[] args) {
        Frame frame = new Frame("Dialog 예제");
        Dialog dialog = new Dialog(frame, "모달 대화 상자", true);

        dialog.setSize(200, 150);
        dialog.setLayout(new FlowLayout());

        Button closeButton = new Button("닫기");
        closeButton.addActionListener(e -> dialog.setVisible(false));
        dialog.add(closeButton);

        frame.setSize(300, 200);
        frame.setVisible(true);
        dialog.setVisible(true);
    }
}
```
**실행 결과:**
<div style="text-align: center;">
    <img src="/사진들/AWT/Component/DialogExample.png" alt="alt text" />
</div>
**주요 메서드:**
- `setModal(boolean modal)`: 모달 설정
- `setVisible(boolean visible)`: 대화 상자 표시/숨김

---

### 2.4 FileDialog
**FileDialog**는 파일 열기 또는 저장을 위한 대화 상자입니다.

```java
import java.awt.*;

class FileDialogExample {
    public static void main(String[] args) {
        Frame frame = new Frame("FileDialog 예제");
        FileDialog fileDialog = new FileDialog(frame, "파일 열기", FileDialog.LOAD);

        Button button = new Button("파일 열기");
        button.addActionListener(e -> {
            fileDialog.setVisible(true);
            System.out.println("선택된 파일: " + fileDialog.getFile());
        });

        frame.add(button);
        frame.setSize(300, 200);
        frame.setVisible(true);
    }
}
```

**실행 결과:**
<div style="text-align: center;">
    <img src="/사진들/AWT/Component/FileDialogExample.png" alt="alt text" />
</div>

**주요 메서드:**
- `getFile()`: 선택된 파일 이름 반환
- `getDirectory()`: 선택된 디렉터리 반환

---

### 2.5 Menu 및 PopupMenu
**Menu**는 메뉴 바를 구성하는 컴포넌트이며, **PopupMenu**는 마우스 우클릭 메뉴를 제공합니다.

```java
import java.awt.*;
import java.awt.event.MouseAdapter;
import java.awt.event.MouseEvent;

class MenuExample {
    public static void main(String[] args) {
        Frame frame = new Frame("Menu 예제");
        MenuBar menuBar = new MenuBar();

        Menu menu = new Menu("파일");
        MenuItem openItem = new MenuItem("열기");
        MenuItem saveItem = new MenuItem("저장");

        menu.add(openItem);
        menu.add(saveItem);
        menuBar.add(menu);

        frame.setMenuBar(menuBar);

        PopupMenu popupMenu = new PopupMenu("편집");
        MenuItem cutItem = new MenuItem("잘라내기");
        MenuItem copyItem = new MenuItem("복사");
        popupMenu.add(cutItem);
        popupMenu.add(copyItem);

        frame.add(popupMenu);
        frame.addMouseListener(new MouseAdapter() {
            public void mouseReleased(MouseEvent e) {
                if (e.isPopupTrigger()) {
                    popupMenu.show(frame, e.getX(), e.getY());
                }
            }
        });

        frame.setSize(400, 300);
        frame.setVisible(true);
    }
}
```
**실행 결과:**
<div style="text-align: center;">
    <img src="/사진들/AWT/Component/MenuExample.png" alt="alt text" />
</div>

**주요 메서드:**
- `add(MenuItem item)`: 메뉴에 항목 추가
- `show(Component invoker, int x, int y)`: 팝업 메뉴 표시

---

### 2.6 Button
**Button**은 클릭 가능한 버튼 컴포넌트입니다.

```java
import java.awt.*;

class ButtonExample {
    public static void main(String[] args) {
        Frame frame = new Frame("Button 예제");
        Button button = new Button("클릭");

        button.addActionListener(e -> System.out.println("버튼 클릭됨"));

        frame.add(button);
        frame.setSize(200, 100);
        frame.setVisible(true);
    }
}
```

**실행 결과:**
<div style="text-align: center;">
    <img src="/사진들/AWT/Component/ButtonExample.png" alt="alt text" />
</div>

**주요 메서드:**
- `getLabel()`: 버튼의 레이블 반환
- `setLabel(String label)`: 버튼의 레이블 설정

---

### 2.7 Choice
**Choice**는 드롭다운 선택 컴포넌트입니다.

```java
import java.awt.*;

class ChoiceExample {
    public static void main(String[] args) {
        Frame frame = new Frame("Choice 예제");
        Choice choice = new Choice();

        choice.add("Option 1");
        choice.add("Option 2");
        choice.add("Option 3");

        frame.add(choice);
        frame.setSize(200, 150);
        frame.setVisible(true);
    }
}
```

**실행 결과:**
<div style="text-align: center;">
    <img src="/사진들/AWT/Component/ChoiceExample.png" alt="alt text" />
</div>
**주요 메서드:**
- `add(String item)`: 선택 항목 추가
- `getSelectedItem()`: 선택된 항목 반환

---

### 2.8 List
**List**는 여러 선택 가능한 항목을 제공하는 컴포넌트입니다.

```java
import java.awt.*;

class ListExample {
    public static void main(String[] args) {
        Frame frame = new Frame("List 예제");
        List list = new List();

        list.add("Item 1");
        list.add("Item 2");
        list.add("Item 3");

        list.addItemListener(e -> System.out.println("선택됨: " + list.getSelectedItem()));

        frame.add(list);
        frame.setSize(200, 150);
        frame.setVisible(true);
    }
}
```
**실행 결과:**
<div style="text-align: center;">
    <img src="/사진들/AWT/Component/ListExample.png" alt="alt text" />
</div>

**주요 메서드:**
- `add(String item)`: 항목 추가
- `getSelectedItem()`: 선택된 항목 반환

---

### 2.9 Label
**Label**은 텍스트를 표시하는 컴포넌트입니다.

```java
import java.awt.*;

class LabelExample {
    public static void main(String[] args) {
        Frame frame = new Frame("Label 예제");
        Label label = new Label("안녕하세요!");

        frame.add(label);
        frame.setSize(200, 100);
        frame.setVisible(true);
    }
}
```
**실행 결과:**
<div style="text-align: center;">
    <img src="/사진들/AWT/Component/LabelExample.png" alt="alt text" />
</div>

**주요 메서드:**
- `getText()`: 텍스트 반환
- `setText(String text)`: 텍스트 설정

---

### 2.10 Checkbox
**Checkbox**는 체크 가능한 컴포넌트입니다.

```java
import java.awt.*;

class CheckboxExample {
    public static void main(String[] args) {
        Frame frame = new Frame("Checkbox 예제");
        Checkbox checkbox = new Checkbox("동의");

        checkbox.addItemListener(e -> System.out.println("상태: " + checkbox.getState()));

        frame.add(checkbox);
        frame.setSize(200, 100);
        frame.setVisible(true);
    }
}
```

**실행 결과:**
<div style="text-align: center;">
    <img src="/사진들/AWT/Component/CheckboxExample.png" alt="alt text" />
</div>

**주요 메서드:**
- `getState()`: 체크 상태 반환
- `setState(boolean state)`: 체크 상태 설정

---

### 2.11 TextField
**TextField**는 단일 행 텍스트 입력 필드입니다.

```java
import java.awt.*;

class TextFieldExample {
    public static void main(String[] args) {
        Frame frame = new Frame("TextField 예제");
        TextField textField = new TextField();

        textField.addActionListener(e -> System.out.println("입력됨: " + textField.getText()));

        frame.add(textField);
        frame.setSize(300, 100);
        frame.setVisible(true);
    }
}
```

**실행 결과:**
<div style="text-align: center;">
    <img src="/사진들/AWT/Component/TextFieldExample.png" alt="alt text" />
</div>

**주요 메서드:**
- `getText()`: 텍스트 반환
- `setText(String text)`: 텍스트 설정

---

### 2.12 TextArea
**TextArea**는 다중 행 텍스트 입력 필드입니다.

```java
import java.awt.*;

class TextAreaExample {
    public static void main(String[] args) {
        Frame frame = new Frame("TextArea 예제");
        TextArea textArea = new TextArea();

        frame.add(textArea);
        frame.setSize(300, 200);
        frame.setVisible(true);
    }
}
```

**실행 결과:**
<div style="text-align: center;">
    <img src="/사진들/AWT/Component/TextAreaExample.png" alt="alt text" />
</div>

**주요 메서드:**
- `getText()`: 텍스트 반환
- `setText(String text)`: 텍스트 설정

---

## 3. 결론

AWT의 다양한 Component를 활용하면 사용자와 상호작용할 수 있는 GUI를 쉽게 구현할 수 있습니다. 본 글에서 다룬 Button, Label, TextField 등은 기본적인 컴포넌트이지만, Frame, Canvas, Dialog와 같은 컴포넌트를 추가로 활용하여 더욱 풍부한 사용자 친화적인 인터페이스를 만들 수 있습니다.

