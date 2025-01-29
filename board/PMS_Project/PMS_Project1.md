---
layout: post
title: "1.AJAX(jQUery)와 JSON에 대한 고찰"
date: 2025-01-27
---

> 현대 웹 프로그래밍에서 AJAX를 이용한 비동기식 데이터 처리와 JSON을 이용한 데이터 송,수신은 빼놓을 수 없는 구현 필요조건입니다. 이 곳에서는 AJAX와 JSON에 대해서 정리 겸 고찰을 해보겠습니다.

# AJAX

## 정의

AJAX는 "Asynchronous JavaScript and XML"의 약자로 Asynchronous단어에서 알 수 있듯이 데이터들의 비동기 처리를 지원하는 API입니다. 원래 동기식 처리라면 새로고침을 해야지만 데이터 갱신이 가능해야 하는 건데, 비동기식 처리를 하면 쉽게 말해 "새로고침"없이도 데이터들을 갱신하는 것이 가능하다는 것입니다.

## 이유

AJAX로 비동기식 처리를 함으로써 데이터들의 비지니스 로직을 클라이언트에 분산할 수 있기에 서버의 부하를 감소시킬 수 있습니다.

form과 submit 속성을 이용한 동기식 처리는 name 속성 값을 form 태그의 action 속성 값의 jsp파일로 페이지 전환(이동)이 무조건 이루어져야만 데이터 송,수신이 일어납니다.

```html
<form name=login action="jsp/login.jsp" method="post">
	<div class="form-group">
		<input type="email" class="form-control form-control-user" name="Email" aria-describedby="emailHelp" placeholder="Enter Email Address...">
	</div>
		<div class="form-group">
			<input type="password" class="form-control form-control-user" name="Password" placeholder="Password">
		</div>
		<div class="form-group">
			<div class="custom-control custom-checkbox small">
				<input type="checkbox" class="custom-control-input" name="customCheck">
				<label class="custom-control-label" for="customCheck">Remember Me</label>
		</div>
	</div>
	<input type="submit" class="btn btn-primary btn-user btn-block" value="Login">
</form>
```

그러나 AJAX(jQuery)를 이용하면 페이지 전환 없이 비동기적으로 jsp로 갔다가 데이터를 받을 수 있고 그 받은 데이터에 대한 비지니스 로직을 클라이언트(html 수준의 페이지)에서 가능 하다는 것입니다. 

```html
<div class="form-group">
	<input type="email" class="form-control form-control-user" id="Email" aria-describedby="emailHelp" placeholder="Enter Email Address...">
</div>
<div class="form-group">
	<input type="password" class="form-control form-control-user" id="Password" placeholder="Password">
</div>
<div class="form-group">
	<div class="custom-control custom-checkbox small">
		<input type="checkbox" class="custom-control-input" id="customCheck">
		<label class="custom-control-label" for="customCheck">Remember Me</label>
	</div>
</div>
<input type="submit" class="btn btn-primary btn-user btn-block" value="Login" onclick="login()">
```

```onclick="login()"``` 이 부분이 실행되면 ```<script>```태그의 login( )함수가 실행되서 $(#" ")으로 id태그 값을 가져와서 JSON 형식으로 데이터를 형성한 후 AJAX.call( )의 params 파라미터로 넘겨서 jsp파일에서 받은 값은 data 변수에 담기고 그 값을 이용해서 자바스크립트로 비지니스 로직을 구현할 수 있다는 점이 굉장한 장점입니다. 원래 동기식으로 form/submit으로 데이터를 송수신하면 서버 측이라고 볼 수가 있는 jsp파일에서 비지니스 로직을 구현하고 사용자 인터페이스도 그쪽에서 구현해야하는 건데, 그럼 너무 서버가 하는 일이 많아지니까 사용자가 많아지면 서버의 부하가 심해지는 건데, 비지니스 로직 + 응답 사용자 인터페이스 생성 이 두개를 클라이언트 측에 위임을 가능하게 하기위해 AJAX를 이용한 비동기식 처리를 해야하는 큰 이유입니다.

```html
<script>
function login() {
	var id = $("#Email").val().trim();
	console.log(id);
	if (id == "") {
		alert("아이디를 입력해 주세요.");
		$("#Email").focus();
		return;
	}
	
	var ps = $("#Password").val().trim();
	console.log(ps);
	
	if (ps == "") {
		alert("패스워드를 입력해 주세요.");
		$("#Password").focus();
		return;
	}
	var params = {id:id, ps:ps}; // {id:choi@naver.com, ps:1111}
	var url = "jsp/login.jsp";
	
	AJAX.call(url, params, function(data) {
		var code = data.trim();
		if (code == "NE") {
			alert("아이디가 존재하지 않습니다.");
		} else if (code == "PE") {
			alert("패스워드가 일치하지 않습니다.");
		} else {
			window.location.href = "index.html"; // 로그인 성공
		}
	});
}
</script>
```

## 사용방법

form/sumbit태그에서 AJAX를 이용하기 위해서는 3가지의 변경점이 있습니다.
1. form태그 제거
2. ```input```태그에서 ```name``` 속성을 ```id```속성으로 변경
3. ```onclick```으로 자바스크립트 함수 호출 이벤트 


```html
<!-- 1.form태그 제거 -->
<div class="form-group">
	<input type="email" class="form-control form-control-user" id="Email" aria-describedby="emailHelp" placeholder="Enter Email Address...">
</div>
<div class="form-group">                        <!-- 2. name 속성 → id 속성 -->
	<input type="password" class="form-control form-control-user" id="Password" placeholder="Password">
</div>
<div class="form-group">
	<div class="custom-control custom-checkbox small">
		<input type="checkbox" class="custom-control-input" id="customCheck">
		<label class="custom-control-label" for="customCheck">Remember Me</label>
	</div>
</div>
<input type="submit" class="btn btn-primary btn-user btn-block" value="Login"
onclick="login()">
<!-- 3. onclick속성 이벤트 생성 -->

<!-- ---------------------------------------------------------------------- -->

<script>
function login() {
	var id = $("#Email").val().trim();
	console.log(id);
	if (id == "") {
		alert("아이디를 입력해 주세요.");
		$("#Email").focus();
		return;
	}
	
	var ps = $("#Password").val().trim();
	console.log(ps);
	
	if (ps == "") {
		alert("패스워드를 입력해 주세요.");
		$("#Password").focus();
		return;
	}
	var params = {id:id, ps:ps}; // {id:choi@naver.com, ps:1111}
	var url = "jsp/login.jsp";
	
	AJAX.call(url, params, function(data) {
		var code = data.trim();
		if (code == "NE") {
			alert("아이디가 존재하지 않습니다.");
		} else if (code == "PE") {
			alert("패스워드가 일치하지 않습니다.");
		} else {
			window.location.href = "index.html"; // 로그인 성공
		}
	});
}
</script>
```

---