---
layout: post
title: 2. How to Debug
date: 2025-01-01
---

>코드를 짜면서 오류를 찾아내기 위해서 디버깅하는 방법을 알 필요가 있습니다. 변수 값이나 변수 값의 연산들을 코드의 흐름따라 확인하며 잘못된 부분을 제거합니다.

```java
public class Main {

	public static void main(String[] args) {
		for (int i = 1; i<=15; i++) {
			System.out.print(i + " ");		
			}

	}

}
```
`main`메서드에 `breakpoint`를 걸어두고 Debug 버튼을 눌러서 변수 i의 값을 관찰해봅시다.

<div style="margin-top: 30px; margin-bottom: 30px;">
  <hr />
</div>

<div style="display: flex; flex-wrap: wrap; align-items: center; margin-top: 20px;">
  <img src="/사진들/디버그/디버그1.png" alt="alt text" style="width: 100%; max-width: 300px; height: auto; margin-right: 20px;" />
  <p style="flex: 1; margin: 0;">
    1. F6을 누르면 Step Over되어서 다음 코드 흐름이 실행됩니다. 여기서는 for문이 실행돼서 i의 값이 시작 값인 1이 됩니다.
  </p>
</div>

<div style="margin-top: 30px; margin-bottom: 30px;">
  <hr />
</div>

<div style="display: flex; flex-wrap: wrap; align-items: center; margin-top: 20px;">
  <img src="/사진들/디버그/디버그2.png" alt="alt text" style="width: 100%; max-width: 300px; height: auto; margin-right: 20px;" />
  <p style="flex: 1; margin: 0;">
    2. F6를 또 누르면 다음 코드 흐름인 출력 메서드가 실행됩니다.
  </p>
</div>

<div style="margin-top: 30px; margin-bottom: 30px;">
  <hr />
</div>

<div style="display: flex; flex-wrap: wrap; align-items: center; margin-top: 20px;">
  <img src="/사진들/디버그/디버그3.png" alt="alt text" style="width: 100%; max-width: 300px; height: auto; margin-right: 20px;" />
  <p style="flex: 1; margin: 0;">
    3. 또 F6을 누르면 다시 for문으로 돌아가서 'i++'이 실행되니까 'i'값은 2가 됩니다.
  </p>
</div>


<br><br>
>이런 식으로 디버깅을 하면 코드의 흐름을 읽기가 수월하고 변수 값 체크나 연산식 오류를 찾아내며 에러를 고쳐나가는 데 도움을 줍니다.