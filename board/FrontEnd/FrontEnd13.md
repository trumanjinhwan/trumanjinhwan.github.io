---
layout: post
title: "13. 리액트 props"
date: 2025-04-05
---

# props

<div style="text-align: center;">
	<img src="/사진들/리액트/props 만들기.png" alt="alt text" />
</div>

- 리액트의 컴포넌트들에게도 html태그의 속성값과 같은 기능을 추가하고싶다면 ```props```를 이용할 수 있습니다.
- ```props```를 구현하는 방법은 컴포넌트에 ```name=value```를 넣고 컴포넌트를 정의한 함수의 파라미터로 ```props```으로 ```value```를 인자로 받아서 함수 안에서 ```{props.}```이런 식으로 사용할 수 있습니다. 

<div style="text-align: center;">
	<img src="/사진들/리액트/동적 props 할당1.png" alt="alt text" />
</div>

<br>

<div style="text-align: center;">
	<img src="/사진들/리액트/동적 props 할당2.png" alt="alt text" />
</div>

<br>

- for 반복문을 사용해서 동적으로 props를 할당 할 수도 있습니다.
