---
layout: post
title: "1. AJAX와 JSON을 활용한 비동기 데이터 처리"
date: 2025-01-27
---

> 현대 웹 프로그래밍에서 AJAX를 이용한 비동기식 데이터 처리와 JSON을 이용한 데이터 송,수신은 빼놓을 수 없는 구현 필요조건입니다. 이 곳에서는 AJAX와 JSON에 대해서 한번 정리도 할 겸 고찰을 해보겠습니다.

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


onclick="login()" 이 부분이 실행되면 
"<script>"
태그의 login( )함수가 실행되서 $(#" ")으로 id태그 값을 가져와서 JSON 형식으로 데이터를 형성한 후 AJAX.call( )의 params 파라미터로 넘겨서 jsp파일에서 받은 값은 data 변수에 담기고 그 값을 이용해서 자바스크립트로 비지니스 로직을 구현할 수 있다는 점이 굉장한 장점입니다. 원래 동기식으로 form/submit으로 데이터를 송수신하면 서버 측이라고 볼 수가 있는 jsp파일에서 비지니스 로직을 구현하고 사용자 인터페이스도 그쪽에서 구현해야하는 건데, 그럼 너무 서버가 하는 일이 많아지니까 사용자가 많아지면 서버의 부하가 심해지는 건데, 비지니스 로직 + 응답 사용자 인터페이스 생성 이 두개를 클라이언트 측에 위임을 가능하게 하기위해 AJAX를 이용한 비동기식 처리를 해야하는 큰 이유입니다.

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


## 활용하기 위한 조건 3가지

form/sumbit태그에서 AJAX를 이용하기 위해서는 3가지의 변경점이 있습니다.
1. form태그 제거
2. 
input
태그에서 
name
 속성을 
id
속성으로 변경
3. 
onclick
으로 자바스크립트 함수 호출 이벤트 


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

# JSON

## 정의

JSON(JavaScript Object Notation)은 경량 데이터 형식으로, 키-값 쌍의 구조를 가집니다.

```json
{
    "id": "choi@naver.com",
    "password": "1234"
}
```

JSON은 직관적이며 다양한 프로그래밍 언어에서 쉽게 파싱할 수 있어, 데이터 교환의 표준으로 자리 잡았습니다.

## 이유 

JSON
이 표준 포맷이 된 이유는 한마디로 간단해서 입니다. 그래서 많은 프로그래밍 언어들이 JSON을 파싱하는 기능을 지원하고있습니다.

## 활용 방법

JSON
을 사용함으로써 얻을 수 있는 최고의 장점은 DB구조의 간편화입니다.

<div style="text-align: center;">
    <img src="/사진들/PMS/PMS1/원래 DB 구조.png" alt="alt text" />
</div>

```java
public JSONArray getFriendTasks(String customerId) { // 동료 및 자신의 작업 조회
    System.out.println(customerId);
    JSONArray tasks = new JSONArray();

    // 로그인한 사용자의 동료들 + 자신과 관련된 작업들 조회
    String query = "SELECT C.CUSTOMER_ID, C.NAME AS CUSTOMER_NAME, " +
                   "T.TASK_ID, T.TASK_NAME, T.STATUS, T.START_DATE, T.END_DATE, T.EPIC, T.DASHBOARD_ID " +
                   "FROM FRIENDLIST F " +
                   "JOIN CLIENT C ON C.CUSTOMER_ID = F.FRIEND_ID " +
                   "JOIN TASK T ON T.CUSTOMER_ID = F.FRIEND_ID " +
                   "WHERE F.CUSTOMER_ID = ? " +

                   "UNION " +

                   "SELECT C.CUSTOMER_ID, C.NAME AS CUSTOMER_NAME, " +
                   "T.TASK_ID, T.TASK_NAME, T.STATUS, T.START_DATE, T.END_DATE, T.EPIC, T.DASHBOARD_ID " +
                   "FROM CLIENT C " +
                   "JOIN TASK T ON C.CUSTOMER_ID = T.CUSTOMER_ID " +
                   "WHERE C.CUSTOMER_ID = ?";

    try {
        connDB(); // DB 연결
        stmt = con.prepareStatement(query);
        stmt.setString(1, customerId); // 사용자의 CUSTOMER_ID를 인자로 이용
        stmt.setString(2, customerId);
        rs = stmt.executeQuery();

        while (rs.next()) {
            try {
                // JSON 객체 생성 및 값 매핑
                JSONObject taskJson = new JSONObject();
                taskJson.put("taskId", rs.getString("TASK_ID")); 
                taskJson.put("task", rs.getString("TASK_NAME"));
                taskJson.put("status", rs.getString("STATUS"));
                taskJson.put("startdate", rs.getString("START_DATE"));
                taskJson.put("enddate", rs.getString("END_DATE"));
                taskJson.put("epic", rs.getString("EPIC"));
                taskJson.put("dashboardId", rs.getString("DASHBOARD_ID"));
                taskJson.put("customerName", rs.getString("CUSTOMER_NAME"));

                tasks.add(taskJson); // 결과 배열에 추가
                System.out.println(tasks.toJSONString());
            } catch (Exception e) {
                System.err.println("데이터 매핑 오류: " + e.getMessage());
            }
        }
    } catch (Exception e) {
        e.printStackTrace(); // 에러 출력
    } finally {
        closeResources();
    }

    return tasks;
}
```


원래라면 DB설계 할 때 필드들을 하나 씩 다 생성하고 신경써서 DAO와 매핑을 해줘야하지만

<div style="text-align: center;">
    <img src="/사진들/PMS/PMS1/JSON DB 구조.png" alt="alt text" />
</div>

```java
public JSONArray getFriendTasks(String customerId) { // 동료과의 작업물만 보는 메서드
		System.out.println(customerId);
	    JSONArray tasks = new JSONArray();
	    //로그인한 사용자의 동료들 + 나자신과의 관련된 작업들만 조회
	    String query = "SELECT C.CUSTOMER_ID, C.JSONSTR AS CLIENT_JSON, T.TASK_ID, T.DASHBOARD_ID, T.JSONSTR AS TASK_JSON "
	             + "FROM FRIENDLIST F "
	             + "JOIN CLIENT C ON C.CUSTOMER_ID = F.FRIEND_ID "
	             + "JOIN TASK T ON T.CUSTOMER_ID = F.FRIEND_ID "
	             + "WHERE F.CUSTOMER_ID = ? "

	             + "UNION "

	             + "SELECT C.CUSTOMER_ID, C.JSONSTR AS CLIENT_JSON, T.TASK_ID, T.DASHBOARD_ID, T.JSONSTR AS TASK_JSON "
	             + "FROM CLIENT C "
	             + "JOIN TASK T ON C.CUSTOMER_ID = T.CUSTOMER_ID "
	             + "WHERE C.CUSTOMER_ID = ?";


	    try {
	        connDB(); // DB 연결
	        stmt = con.prepareStatement(query);
	        stmt.setString(1, customerId); // 사용자의 CUSTOMER_ID를 인자로 이용
	        stmt.setString(2, customerId);
	        rs = stmt.executeQuery();

	        while (rs.next()) {
	            try {
	                // CLIENT JSON 파싱
	                String clientJsonStr = rs.getString("CLIENT_JSON");
	                JSONObject clientJson = (JSONObject) new         JSONParser().parse(clientJsonStr);
	                String customerName = clientJson.get("name").toString();

	                // TASK JSON 생성
	                String taskJsonStr = rs.getString("TASK_JSON");
	                JSONObject taskJson = (JSONObject) new JSONParser().parse(taskJsonStr);
	                // taskId 추가
	                taskJson.put("taskId", rs.getString("TASK_ID")); 
	                taskJson.put("dashboardId", rs.getString("DASHBOARD_ID"));
	                taskJson.put("customerName", customerName);

	                tasks.add(taskJson); // 결과 배열에 추가
	                System.out.println(tasks.toJSONString());
	            } catch (Exception e) {
	                System.err.println("JSON 파싱 오류: " + e.getMessage());
	            }
	        }
	    } catch (Exception e) {
	        e.printStackTrace(); // 에러 출력
	    } finally {
	    	closeResources();
	    }

	    return tasks;
	}
```

이렇게 "jsonstr"이라는 필드에 기본키, 외래키들을 제외한 필드들을 넣어주면 파싱하고 매핑해야 할 부분들이 현저히 줄어드는 것을 볼 수 있습니다.
<br>
하지만, 단점도 존재합니다. 
아무래도 "jsonstr"필드에 원래 필드여야 할 것들이 JSON형식의 키값으로 들어가다 보니까 
키 값 하나하나의 제약조건을 설정 할 수 가 없어서 데이터 무결성을 저해할 수 있습니다. ex ```check``` , ```default``` , ```not null```
<br>
그래서 보완점으로 하이브리드 방식이 있습니다. 제약 조건이 필요한 부분은 DB의 필드로 빼고 나머지만 jsonstr로 JSON형식으로 유지 하는 것입니다. 예를 들어, status의 value들이 "진행", "예정", "완료", "멈춤" 이 4가지 내에서만 나와야 한다면 
check
제약조건을 이용할 수 있습니다.

```sql
CREATE TABLE TASK (
    TASK_ID VARCHAR(1000) PRIMARY KEY,  
    DASHBOARD_ID VARCHAR(1000) NOT NULL, 
    CUSTOMER_ID VARCHAR(1000) NOT NULL,
    -- 제약 조건 추가된 상태 값
    STATUS VARCHAR(1000) CHECK (STATUS IN ('진행', '예정', '완료', '멈춤')), 
    JSONSTR  VARCHAR(1000) NOT NULL, 
    
    CONSTRAINT FK_TASK_CUSTOMER FOREIGN KEY (CUSTOMER_ID) REFERENCES CLIENT(CUSTOMER_ID),
    CONSTRAINT FK_TASK_DASHBOARD FOREIGN KEY (DASHBOARD_ID) REFERENCES DASHBOARD(DASHBOARD_ID)
);
```
# 정리

AJAX와 JSON은 현대 웹 개발에서 필수적인 기술이며, 이를 효과적으로 활용하면 사용자 경험을 향상시키고 서버 부하를 줄일 수 있습니다.

- **AJAX를 활용하면** 페이지 새로고침이나 전환없이도 데이터를 실시간으로 갱신할 수 있으며, 클라이언트 측에서 더 많은 비즈니스 로직을 처리하여 서버의 부담을 줄일 수 있습니다.
- **JSON을 사용하면** 데이터 구조를 유연하게 만들 수 있으며, JSON 기반 데이터베이스 설계를 통해 테이블 간 불필요한 JOIN을 줄일 수도 있습니다.
- **최적의 설계 방식**으로는 JSON을 적절히 활용하되, 제약 조건이 필요한 필드는 개별 컬럼으로 유지하는 하이브리드 방식을 적용하는 것이 바람직합니다.

이러한 기법을 적용하면 보다 효율적이고 확장성이 높은 웹 애플리케이션을 구축할 수 있습니다.

