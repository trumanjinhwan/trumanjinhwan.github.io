---
layout: post
title: "24. 리액트 useReducer"
date: 2025-04-21
---

# 리액트 useReducer

React에서 상태를 관리할 때 가장 많이 사용하는 건 `useState`죠. 하지만 상태의 **변화 로직이 복잡하거나 여러 종류의 액션이 필요한 경우**, `useReducer`가 훨씬 적합할 수 있습니다.

`useReducer`는 Redux의 개념을 React 훅으로 가져온 것으로, **상태와 액션을 분리해 더 명확한 로직**을 만들 수 있도록 도와줍니다.

---

## 1. useState vs useReducer

| 항목                    | useState                             | useReducer                                      |
|-------------------------|----------------------------------------|--------------------------------------------------|
| 상태 업데이트 방식        | `setState(새 값)`                     | `dispatch(액션 객체)`                           |
| 복잡한 로직 처리         | 직접 if문 또는 분기 처리 필요          | reducer 함수에서 로직 분리 가능                 |
| 관련 상태가 많은 경우    | 여러 개의 useState 사용 필요           | 하나의 reducer에서 관련 상태 통합 관리 가능     |
| 적합한 상황              | 단순 상태 값 관리                     | 상태 변경 조건이 여러 가지인 경우               |

---

## 2. useReducer 구조 이해

아래 이미지를 보면서 구조를 같이 볼게요.

<div style="text-align: center;">
  <img src="/사진들/리액트/useReducer.png" alt="useReducer 구조 예시" />
</div>

이 예시는 숫자 값을 증가, 감소, 초기화하는 간단한 카운터 앱입니다.

- 가장 먼저, `countReducer`라는 함수를 정의해 상태를 어떻게 변경할지 작성해요. 이 함수는 `(현재 상태, 액션)`을 받아서 새로운 상태를 반환합니다.
- 그 다음, `useReducer(countReducer, 0)`을 통해 상태를 초기화합니다. 여기서 `count`는 현재 상태, `countDispatch`는 액션을 보낼 수 있는 함수예요.
- 버튼 클릭 시 `countDispatch`를 통해 액션을 보냅니다. 예: `countDispatch({ type: 'up', number: number })`
- `countReducer`는 액션의 타입에 따라 계산을 수행하고 새로운 값을 반환합니다.

---

## 3. 이미지 코드 흐름 요약

- `useReducer`로 선언한 `count` 상태는 버튼을 누를 때마다 액션을 받아 reducer 함수를 통해 계산됩니다.
- `down`, `reset`, `up` 함수는 각각 다른 액션을 `dispatch`로 전달하며, 이들은 모두 `countReducer` 내 조건문에서 처리됩니다.
- `number`라는 별도의 상태는 입력 필드의 값으로 유지되고, 이 숫자가 `dispatch`의 payload로 함께 전달됩니다.

---

# 마무리

`useReducer`는 단순히 상태를 바꾸는 걸 넘어서, **상태 변화의 흐름을 명확하게 설계**할 수 있게 해줍니다. 특히 액션의 타입과 데이터가 분리되어 있어서, 로직을 분리하고 유지보수하기가 훨씬 좋아집니다.

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
