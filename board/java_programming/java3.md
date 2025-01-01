---
layout: post
title: 3. Input/Output
date: 2025-01-01
---
>이곳에서는 콘솔 입,출력방식을 정리해봤습니다.

# 출력

### 1.System.out.print()
줄 바꿈("\n")을 포함하지 않고 단순 출력

```java
public class Main {

  public static void main(String[] args) {
	  System.out.print("Hello");
	  System.out.print("World");
	}
}

//HelloWorld <-- 이렇게 붙어서 출력
```

### 2.System.out.println()
줄 바꿈("\n")을 포함하며 출력

```java
public class Main {

  public static void main(String[] args) {
	  System.out.println("Hello");
	  System.out.println("World");
	}
}

/*
Hello   <-- 이렇게 줄 바꿈되서 출력
World
*/
```

### 3.System.out.printf()
형식 지정자로 변수를 대입하여 출력

```java
public class Main {

  public static void main(String[] args) {
	  Stirng str = "World";
	  System.out.println("Hello %s", str);
	}
}


//Hello World <-- str에 담긴 "World"가 %s(형식 지정자)에 담겨서 추력
```

- 형식 지정자 정리
	1. %d : 정수
	2. %f  : 실수
	3. %c :  문자
	4. %s :  문자열

 ---
# 입력

### 1.Scanner
Scanner 클래스를 임포트하고 인스턴스 생성한 후 사용해야합니다.

```java
import java.util.Scanner; // Scanner클래스 임포트

public class Main {

  public static void main(String[] args) {
	  Scanner sc = new Scanner(System.in); // 인스턴스 생성
	  
	  System.out.println(sc.next());
	  System.out.println(sc.nextLine());
	  System.out.println(sc.nextInt());
	  System.out.println(sc.nextFloat());
	}
}
```

-입력 받을 시점에, 입력 값의 자료형을 애초에 지정해 놓아야한다. 만약, 실제 입력 값과 지정해놓은  자료형이 틀릴 경우 `InputMismatchException`에러가 발생합니다.
	1. 문자 입력: next()
	2. 문자열 입력: nextLine()
	3. 정수형 입력: nextInt()
	4. 실수형 입력: nextFloat()
<br><br>
>파이썬은 `input()`으로 문자열로만 입력 받고 필요에 따라 형 변환을 해야하지만 
자바는 애초에 입력 타입을 정해놓으니까 컴파일러 입장에서는 추가 연산이 필요없으니까 효율적이라고 볼 수 있습니다. 그러나, 유연성 입장에서는 파이썬이 더 좋다고 말할 수 있기 때문에 각자 방식의 장단점이 있습니다.