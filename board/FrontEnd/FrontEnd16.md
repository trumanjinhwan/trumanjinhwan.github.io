---
layout: post
title: "16. 리액트 Create"
date: 2025-04-10
---

# Create 컴포넌트란?

<div style="text-align: center;">
	<img src="/사진들/리액트/create 페이지 이동.png" alt="alt text" />
</div>

- 리액트 앱에서 사용자가 새로운 데이터를 입력할 수 있는 UI를 만들고 싶다면?  
바로 `Create 컴포넌트`를 만들어야 합니다.
- `Create`는 보통 폼(form) 태그와 input 요소를 활용해 사용자의 입력을 받고, 제출 이벤트를 통해 상위 컴포넌트로 데이터를 전달합니다.

<br>

<div style="text-align: center;">
	<img src="/사진들/리액트/create 컴포넌트 정의.png" alt="alt text" />
</div>

- 폼의 기본적인 제출 방식은 `onSubmit` 이벤트를 사용합니다.
- `<form onSubmit={function(e){ ... }}>` 형태로,  
  이벤트 객체 `e`를 통해 `e.preventDefault()`를 호출하면 브라우저의 기본 동작(페이지 리로딩)을 막을 수 있습니다.
- 입력창은 `<input>` 요소를 쓰고, 해당 입력값은 `useState()`로 상태로 관리합니다.

<br>

# 원시 자료형과 객체형 setState 차이점

<div style="text-align: center;">
	<img src="/사진들/리액트/create 컴포넌트.png" alt="alt text" />
</div>

- 상태를 `원시 자료형`으로 관리할 땐 단순히 값을 바꾸기만 하면 되지만,  
  `객체(object)`로 상태를 관리할 때는 **불변성**에 주의해야 합니다.
- 예를 들어, `const [title, setTitle] = useState('')` 같은 원시형 상태는 바로 값을 바꿔도 문제가 없습니다.
- 하지만 `const [form, setForm] = useState({ title: '', body: '' })` 처럼 객체형일 경우,  
  전체 객체를 복사한 다음 일부 속성만 바꿔서 새로운 객체를 만들어야 합니다.

- 즉, **기존 객체를 직접 수정하면 리액트가 변경을 감지하지 못할 수 있기 때문에**,  
  스프레드 연산자(...)를 이용해서 복사 후 수정하는 방식이 중요합니다.

- 이는 상태의 불변성을 유지하고, 컴포넌트가 제대로 리렌더링되도록 보장합니다.
