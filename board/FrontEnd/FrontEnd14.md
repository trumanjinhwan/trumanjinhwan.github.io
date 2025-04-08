---
layout: post
title: "14. 리액트 이벤트"
date: 2025-04-08
---

# 이벤트

<div style="text-align: center;">
	<img src="/사진들/리액트/이벤트1.png" alt="alt text" />
</div>

- 일반적인 Javascript로 이벤트 처리를 하려면 ```onclick``` 속성을 이용합니다. ```<button><a onclick="특정함수"></a></button>``` 이런식으로 말이죠.
- 그러나 리액트로 만든 컴포넌트에는 좀 다르게 ```onChangeMode={function(){}}```이런 형식으로 가고
- 컴포넌트를 정의한 함수영역 안에서의 태그에서 ```onClick={function(event){props.onChangeMode();}}``` 이런식으로 ```event```객체를 무조건 주입한 콜백함수를 이용합니다.

<br>

<div style="text-align: center;">
	<img src="/사진들/리액트/이벤트2.png" alt="alt text" />
</div>

- 화살표 함수로 해도 상관은 없습니다.
- 그리고 동적으로 매개변수를 받아서 처리하고 싶다면?
- ```<a>```태그 안에서 ```id={매개변수}``` 속성을 선언하고 이벤트 함수 내에서```event.target.id```를 매개변수로 인자로 넘겨서 동적으로 할당할 수 있습니다.
