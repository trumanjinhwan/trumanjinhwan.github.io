---
layout: post
title: "11. 리액트 소스코드 수정"
date: 2025-04-05
---

# index.js

<div style="text-align: center;">
	<img src="/사진들/리액트/index.js 변경 전.png" alt="alt text" />
</div>

- 리액트를 시작할 때 가장 먼저 시작되는 "입구"를 말하자면 ```index.js```파일입니다.
- ```</App>```부분을 사용자 정의 태그라고 하는데 저걸 지우면 화면에 랜더링 되는 게 아무것도 없으므로 백지 화면만 보이게 됩니다.

<div style="text-align: center;">
	<img src="/사진들/리액트/index.js 변경 후.png" alt="alt text" />
</div>

<br>

# App.js

<div style="text-align: center;">
	<img src="/사진들/리액트/App.js 변경 전.png" alt="alt text" />
</div>

- ```App.js```에서 ```</App>```태그를 정의 하고 있다. 형식은 보시다시피 함수(function)이고 리턴 값 안에 내용이 태그에 정의되는 내용들입니다.
- 리턴 문 안의 내용을 Hello World!!!로 바꿔보았습니다.

<div style="text-align: center;">
	<img src="/사진들/리액트/App.js 변경 후.png" alt="alt text" />
</div>

<br>

# App.css

<div style="text-align: center;">
	<img src="/사진들/리액트/App.css 변경 전.png" alt="alt text" />
</div>

- ```App.css```에서 화면에 보이는 ```App``` 클래스 선택자에 효과를 주는 css를 정의합니다.

<div style="text-align: center;">
	<img src="/사진들/리액트/App.css 변경 후.png" alt="alt text" />
</div>

<br>

# index.css

<div style="text-align: center;">
	<img src="/사진들/리액트/index.css 변경 전.png" alt="alt text" />
</div>

- ```index.css```도 마찬가지로 태그들의 css 설정을 정의합니다.

<div style="text-align: center;">
	<img src="/사진들/리액트/index.css 변경 후.png" alt="alt text" />
</div>

<br>

# index.html

<div style="text-align: center;">
	<img src="/사진들/리액트/index.html 변경 전.png" alt="alt text" />
</div>

- ```index.html```에서 ```root```태그가 중요한데, 사실상 이놈이 리액트의 핵심이라 할 수 있습니다.
- 리액트의 입구라고 했던 ```index.js```에서 ```const root = ReactDOM.createRoot(document.getElementById('root'));``` 이 구문을 볼 수 있는데, 이것의 뜻은 "root 태그를 랜더링(불러와서 웹 페이지에 띄우기)해라"라는 뜻입니다.
- 쉽게 말해서 리액트의 "심장"이라고 할 수 있는 것이죠. 저게 없으면 아무리 리액트 코드를 잘 짜도 웹페이지에 보여 줄 수 없으니까 말이죠.

<div style="text-align: center;">
	<img src="/사진들/리액트/index.html 변경 후.png" alt="alt text" />
</div>

<br>

# 정리

- "입구"인 ```index.js```를 거쳐서 ```App.js```를 보여주고 ```App.css```와 ```index.css```를 이용해서 꾸며주고 ```index.html```에서의 ```root```태그로 그것들을 웹페이지에 뿌려준다라고 정리할 수 있습니다.
