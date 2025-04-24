---
layout: post
title: "26. 리액트 Redux toolkit"
date: 2025-04-24
---

# 리액트 Redux Toolkit

Redux Toolkit은 Redux의 공식 툴킷으로, 복잡한 설정 없이 효율적으로 Redux를 사용할 수 있도록 도와주는 라이브러리입니다.
기존 Redux보다 훨씬 간단하고 직관적인 코드 작성이 가능합니다

---

## 1. Redux Toolkit이 필요한 이유

기존 Redux는 `action`, `reducer`, `store`, `dispatch` 등 많은 코드를 작성해야 했죠.  
Redux Toolkit은 이런 반복적인 작업을 줄이고, 유지보수를 쉽게 해줍니다.

특징은 다음과 같아요:

- `configureStore()`로 간단하게 스토어 생성
- `createSlice()`로 액션과 리듀서를 한 번에 작성
- 불변성(immutability)을 자동 처리

---

## 2. store 설정하기

<div style="text-align: center;">
  <img src="/사진들/리액트/redux toolkit1.png" alt="Redux Toolkit store 설정 예시" />
</div>

이 코드에서는 `configureStore()`를 사용해 store를 설정하고 있습니다.:

- `counterSlice.reducer`를 `counter`라는 키로 등록해서 store에 포함시켜줍니다.
- 이후 이 store를 전체 앱에 공급할 수 있습니다.

---

## 3. 컴포넌트에서 Redux Toolkit 사용

<div style="text-align: center;">
  <img src="/사진들/리액트/redux toolkit2.png" alt="컴포넌트에서 Redux Toolkit 상태 사용 예시" />
</div>

이 컴포넌트는 Redux Toolkit을 사용하는 전형적인 예입니다:

- `useSelector()`로 현재 상태를 구독하고,
- `useDispatch()`로 액션을 발생시킵니다.
- `dispatch(up(2))`를 통해 `counterSlice`에서 만든 액션을 보냅니다.

---

## 4. 전체 구조 흐름

<div style="text-align: center;">
  <img src="/사진들/리액트/redux toolkit3.png" alt="Redux Toolkit 전체 구성 흐름" />
</div>

전체 구조를 요약하면 다음과 같습니다:

- `createSlice()`로 액션과 reducer를 생성
- `configureStore()`로 store 설정
- `<Provider>`로 store를 App컴포넌트에 공급
- `useSelector()`와 `useDispatch()`로 컴포넌트와 연결

---

## 5. 마무리

Redux Toolkit은 Redux를 더 쉽게, 더 빠르게 사용할 수 있도록 도와주는 도구입니다.  
쓰는 방법을 익혀두면 기존의 Redux보다 편하고 직관적으로 개발할 수 있을 것 입니다.

---

<style>
  table {
    width: 100%;
    border-collapse: collapse;
    margin: 20px 0;
  }

  th, td {
    border: 2px solid #333;
    padding: 12px;
    text-align: center;
  }

  th {
    background-color: #f4f4f4;
    font-weight: bold;
  }

  td {
    background-color: #fafafa;
  }

  table th, table td {
    border: 1px solid #ddd;
  }
</style>
