---
layout: post
title: "23. 리액트 Context API"
date: 2025-04-21
---

# 리액트 Context API

React 앱을 만들다 보면 컴포넌트 간에 데이터를 주고받을 일이 많아집니다. 가장 흔한 방식은 `props`를 이용하는 것이죠. 하지만 컴포넌트가 깊어질수록 일일이 props를 전달하는 건 꽤 번거롭고 유지보수도 어려워집니다.

**Context API**는 이런 문제를 해결하기 위해 만들어졌습니다. 상위 컴포넌트에서 하위 컴포넌트로 **직접 데이터를 전달**할 수 있어, 여러 단계의 `props` 전달을 피할 수 있죠.

---

## 1. Context API의 실행방식

아래 이미지를 보면서 구조를 이해해볼게요.

<div style="text-align: center;">
  <img src="/사진들/리액트/Context API.png" alt="Context API를 사용한 컴포넌트 트리와 스타일 흐름" />
</div>

<br>

이미지를 보면 **3중 컴포넌트 구조**가 보입니다: `App → Sub1 → Sub2 → Sub3`. 각각의 컴포넌트는 `useContext()`로 스타일 데이터를 받아서 `style` 속성에 바로 적용하고 있어요.

- 최상단(App 컴포넌트)에서는 `createContext()`를 통해 `themeContext`를 만들고, 빨간색 테두리를 기본값으로 설정해요.
- 그 다음, App 컴포넌트 내부에서 `<themeContext.Provider>`를 사용해 **파란색 테두리**를 전달합니다.
- Sub1 컴포넌트는 새로운 Provider로 **초록색 테두리**를 설정하고, 그 하위 컴포넌트(Sub2, Sub3)에서 그대로 적용되죠.
- Sub2와 Sub3는 context를 통해 전달받은 값을 `style={theme}` 형태로 적용합니다.

---

## 3. Context API의 장점

| 항목                    | Context API 사용 전             | Context API 사용 후                    |
|-------------------------|----------------------------------|----------------------------------------|
| 데이터 전달 방식        | 여러 단계 props 전달 필요       | 중간 단계 생략 가능                    |
| 코드 복잡도            | 컴포넌트가 많아질수록 증가       | 하위 컴포넌트에서 직접 접근 가능       |
| 사용 예시               | 테마, 다국어 설정, 로그인 정보 등 | 전역 데이터 관리에 적합                 |

---

## 4. 기억할 점


Context API는 **"부모 → 자식 → 손자..."로 데이터를 일일이 넘기지 않고**, 필요한 컴포넌트에서 바로 값을 받아오는 방식입니다.  
이번 이미지 예시처럼, **Provider를 중첩해서 다른 스타일을 적용할 수도 있다는 점**도 중요합니다.

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
