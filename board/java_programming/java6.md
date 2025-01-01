---
layout: post
title: 6. Control Statment
date: 2025-01-01
---

<img src="/사진들/제어문/제어문1.png" alt="alt text" />
>위의 그림은 Java의 제어문들을 요약 정리해놓은 이미지입니다.<br>
제가 이 항목에서 다루고 싶은 2가지는 1. if vs switch / 2. for vs 향상된 for 입니다.

### 1. if vs switch

어느 상황에서 if문이 적합하고 어느 상황에서 switch문이 적합할까요?

#### if문이 적합한 상황

1. 조건이 복잡하거나 범위 비교가 필요할 때
   - 예: `x > 10 && x < 20` 또는 `y % 2 == 0` 같은 복잡한 논리 연산이 필요한 경우.
   
2. 조건의 갯수가 적거나 명확하지 않을 때
   - 조건이 유동적이거나 몇 가지 안 되는 경우.

```java
public class Main {

	public static void main(String[] args) {
	
		int age = 25;
		
		if (age >= 8 && age < 13) {
			System.out.println("초등학생");
		} 
		else if (age >= 13 && age < 20) {
			System.out.println("중,고등학생");
		} 
		else {
		System.out.println("성인");
		}
	}
}
```

#### switch문이 적합한 상황

1. 하나의 변수를 특정 값과 비교할 때
   - 메뉴 선택, 특정 명령어 처리 등 변수의 값이 명확히 구분되는 경우.

2. 조건이 명확하고 데이터 타입이 특정 타입일 때
   - `int`, `char`, `String`, `enum` 값을 비교하는 간단한 경우.

3. 조건이 많고, 가독성이 중요할 때
   - 여러 조건을 나열해야 할 때 `switch`문은 가독성이 뛰어나고 유지보수도 용이합니다.

```java
public class Main {

	public static void main(String[] args) {
	
		int day = 3;
		
		switch (day) {
			case 1:
				System.out.println("월요일");
				break;
			case 2:
				System.out.println("화요일");
				break;
			case 3:
				System.out.println("수요일");
				break;
			case 4:
				System.out.println("목요일");
				break;
			case 5:
				System.out.println("금요일");
				break;
			case 6:
			case 7:
				System.out.println("주말");
				break;
			default:
				System.out.println("잘못된 요일 입력");
		}
	}
}
```

>정리하자면 if문은 조건식이 복잡하고 범위비교가 들어가면서 조건의 갯수가 비교적 적을 때, 적합하고<br>
>switch문은 조건이 특정 데이터 타입으로서 간단하고 조건의 갯수가 많을 때, 적합합니다.

-근데 만약 조건식이 복잡한데(if문 적합) 조건의 갯수가 많으면(switch문 적합) 둘 중 어느 것을 선택해야 하는 것 일까요?<br>
 이에 대한 고찰은 답이 없을 것 같습니다...

 ---
### 2. 일반적인 for문 vs 향상된 for문

일반적인 for문과 향상된 for문은 어떤 차이가 있으며, 각각 어떤 상황에서 적합할까요?

#### 일반적인 for문이 적합한 상황

1. 인덱스가 필요한 경우
   - 배열이나 리스트의 특정 인덱스 값을 조작하거나 사용할 때.

2. 부분적으로만 요소를 순회해야 하는 경우
   - 특정 조건에 맞는 요소만 처리할 때 적합.

3. 반복 흐름을 제어해야 하는 경우
   - `break`, `continue` 등을 사용해 반복을 세밀히 제어할 필요가 있을 때.

```java
public class Main {

    public static void main(String[] args) {
        int[] num = {10, 20, 30, 40, 50};

        // 짝수 인덱스만 출력
        for (int i = 0; i < num.length; i++) {
            if (i % 2 == 0) {
                System.out.println("인덱스 " + i + ": " + num[i]);
            }
        }
    }
}
```

#### 향상된 for문이 적합한 상황

1. 배열이나 컬렉션의 모든 요소를 순차적으로 순회해야 하는 경우
   - 단순히 데이터를 출력하거나 처리하는 경우에 적합합니다.

2. 인덱스가 필요 없는 경우
   - 값만 사용하고 인덱스는 필요하지 않을 때.

```java
public class Main {

    public static void main(String[] args) {
        int[] num = {10, 20, 30, 40, 50};

        // 배열의 모든 요소 출력
        for (int value : num) {
            System.out.println("값: " + value);
        }
    }
}
```

>정리하자면, 일반적인 for문은 인덱스를 가지고 세밀한 반복횟수 조작이 필요할 때 적합하고<br>
>향상된 for문은 인덱스 필요없이 배열의 전체 요소를 순회할 때 사용하면 가독성도 높이고 좋다.