---
layout: post
title: 5. Operator
date: 2025-01-01
---

<img src="/사진들/연산자/연산자1.png" alt="alt text" />
>출처 : https://velog.io/@falling_star3/Java-Ch032.-%EC%97%B0%EC%82%B0%EC%9E%90-%EC%A2%85%EB%A5%98%EC%A6%9D%EA%B0%90-%EC%82%B0%EC%88%A0-%EB%85%BC%EB%A6%AC-
<br><br>
>위의 그림은 Java의 연산자들을 요약 정리해놓은 이미지입니다.<br>
제가 이 항목에서 다루고 싶은 2가지는 1.다른 자료형간의 연산과 산출 값 / 2. "삼항 연산자" 입니다.

### 1. 다른 자료형간의 연산 & 산출 값

```java
public class Main {

	public static void main(String[] args) {
		int a = 1;
		double b = 2.2;

		// 1번
		int c = a + b; // int + double -> double

		// 2번
		double d = a + b; // int + double -> double
		
		}
}
```

1번 구문은 `Type mismatch`오류가 뜹니다. 왜냐하면 "int"랑 "double"이랑 연산해서 나오는 산출 값은  큰 그릇인 "double"값이 되는데, "c"를 "int"로 변수 선언했기 때문입니다.<br>

2번 구문처럼 산출 값을 고려한 알맞은 변수 선언을 해줘야합니다.<br>

암묵적으로 데이터 값에 맞춰 변수 선언을 해주는 파이썬과 달리 자바는 일일히 변수 선언을 요구하기 때문에 엄격하게 연산의 산출 값을 파악해주는 것이 중요합니다.

---

### 2. 삼항 연산자

```java
public class Main {

	public static void main(String[] args) {
	int n = 2;
	
	//1번
	( n % 2 == 0) ? System.out.println("짝수"); : System.out.println("홀수");
	
	//2번
	System.out.printf("%s", (n % 2 == 0) ? "짝수" : "홀수" );
	}
}
```

삼항 연산자는 "if~else"구문으로 4행 나올 것을 1행으로 줄여 줄 수 있는 강력한 조건 연산식입니다.<br>
간결한 구문 작성을 위해서 "모 아니면 도"느낌의 코드를 구현 할 일이 있으면 적극적으로 삼항 연산자를 적용하려 합니다.<br>
삼항 연산자를 연습하는 과정에서 짝수/홀수를 구분하는 코드를 작성할 적에 오류를 만난 적이 있는데 처음에는 1번 구문처럼 코딩을 했었습니다. 근데 Sysntax오류를 만난 것입니다. 즉, 잘못된 문법의 토큰이 들어갔다는 말이죠.<br>
그게 뭔가 하고 찾아봤는데 삼항 연산자는 `(조건식) ? 참일때의 값 : 거짓일 때의 값;`이 문법을 따르는 데, 1번 구문은 값이 들어갈 자리에 `System.out.println()`를 썼기에 반환 값이 없는 `void`였던 것 입니다. 그래서 값이 있어야 할 자리에 값이 없으니까 오류가 발생한 것 입니다.<br>
그래서 String값인 2번 구문이 의도에 적합한 코드였습니다.<br><br>

>정리하자면, 삼항 연산자는 "?" 뒤에 무조건 "값"이 들어가야만 합니다.
