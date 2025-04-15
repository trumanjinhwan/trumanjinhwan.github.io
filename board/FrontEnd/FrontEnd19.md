---
layout: post
title: "19. 리액트 라우터 DOM"
date: 2025-04-15
---

# React Router DOM으로 페이지 이동 구현하기

SPA(Single Page Application)인 리액트에서 **페이지 전환 효과**를 자연스럽게 구현하려면 `react-router-dom`을 사용하는 것이 일반적입니다. 이번 글에서는 간단한 예제를 통해 라우터를 어떻게 구성하고, 각 컴포넌트를 어떻게 연결하는지를 이미지와 함께 알아보겠습니다.

## 1. 설치하기

라우터 기능을 사용하려면 먼저 `react-router-dom` 패키지를 설치해야 합니다.

<div style="text-align: center;">
	<img src="/사진들/리액트/reacr-router-dom 설치.png" alt="alt text" />
</div>

```bash
npm install react-router-dom
```

설치 후에는 라우팅 기능을 사용할 수 있게 됩니다.

## 2. 라우터 관련 컴포넌트 임포트

<div style="text-align: center;">
	<img src="/사진들/리액트/Router 임포트.png" alt="alt text" />
</div>


다음과 같이 필요한 컴포넌트들을 import 합니다.

- `BrowserRouter`: 가장 바깥을 감싸는 컴포넌트로, 라우팅 기능을 전역에서 사용할 수 있도록 설정합니다.
    
- `Routes`: 여러 개의 `Route`를 감싸주는 역할을 합니다.
    
- `Route`: 특정 경로와 해당 경로에 보여줄 컴포넌트를 연결합니다.
    

## 3. BrowserRouter로 전체 감싸기

<div style="text-align: center;">
	<img src="/사진들/리액트/BrowserRouter.png" alt="alt text" />
</div>

`index.js`의 가장 최상단에서 `App` 컴포넌트를 `<BrowserRouter>`로 감싸줘야 **라우터 기능이 활성화**됩니다.

이처럼 감싸주지 않으면 내부에서 `<Route>` 등을 사용할 수 없고 에러가 발생할 수 있습니다.

## 4. Routes와 Route 구성하기

<div style="text-align: center;">
	<img src="/사진들/리액트/Route태그.png" alt="alt text" />
</div>

각 경로별 컴포넌트를 정의할 땐 `<Routes>`로 감싸고 그 안에 `<Route>`들을 배치합니다.

- `path` 속성은 **URL 경로**를 의미합니다. 예를 들어 `/topics`는 `localhost:3000/topics`일 때 해당됩니다.
    
- `element` 속성은 해당 경로에 진입했을 때 **렌더링할 컴포넌트**를 지정합니다. 주의할 점은 이 속성에 컴포넌트 자체가 아닌 JSX 형태로 넘겨야 하므로 `<Topics />`처럼 태그로 작성해야 합니다.
    

```jsx
<Routes>
  <Route path="/" element={<Home />} />
  <Route path="/topics" element={<Topics />} />
  <Route path="/contact" element={<Contact />} />
</Routes>
```

이렇게 구성하면 메뉴 클릭 시 해당 컴포넌트로 이동하면서도 새로고침 없이 콘텐츠만 바뀌는 경험을 제공합니다.

## 정리

- `react-router-dom`은 리액트에서 화면 전환을 구현하기 위한 필수 라이브러리입니다.
    
- `BrowserRouter`는 라우팅 기능을 전체에 적용하기 위해 가장 바깥을 감싸는 컴포넌트입니다.
    
- 각 경로는 `Route`로 정의하며, 반드시 `Routes`로 감싸야 합니다.
    
- `Route`의 `element` 속성에는 렌더링할 컴포넌트를 JSX 형태로 전달해야 합니다.
    
