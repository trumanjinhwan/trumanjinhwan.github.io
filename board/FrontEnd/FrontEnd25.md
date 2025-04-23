---
layout: post
title: "25. 리액트 Redux"
date: 2025-04-23
---

# 리액트 Redux

React 앱이 커지다 보면 상태 관리가 점점 어려워집니다.  
이럴 때 **Redux**는 복잡한 상태를 중앙에서 관리할 수 있도록 도와줘요.

---

## 1. Redux가 필요한 이유

React의 기본 상태 관리 도구인 `useState`나 `useReducer`도 훌륭하지만, 다음과 같은 상황에서는 Redux가 훨씬 적합합니다:

- 컴포넌트 트리의 여러 레벨에서 상태가 필요할 때
- 다양한 컴포넌트가 같은 상태를 공유해야 할 때
- 상태 변화 로직이 복잡하거나 액션이 다양할 때

Redux는 상태를 **store**라는 하나의 중앙 저장소에서 관리하고,  
**dispatch → reducer → store 업데이트 → 컴포넌트 리렌더링**의 흐름으로 동작합니다.

---

## 2. Redux 구조 이해

<div style="text-align: center;">
  <img src="/사진들/리액트/redux1.png" alt="Redux 구조와 액션 흐름 예시" />
</div>

이미지의 코드는 다음과 같은 흐름을 보여줍니다:

- `createStore()`를 통해 상태 저장소(store)를 생성합니다.
- `reducer`는 액션을 받아 상태를 어떻게 변경할지 결정합니다.
- `Provider`를 통해 전체 컴포넌트에 store를 공급합니다.
- 하위 컴포넌트에서는 `useSelector()`로 상태를 읽고, `useDispatch()`로 액션을 보냅니다.

---

## 3. 실제 사용 예시
<div style="text-align: center;">
  <img src="/사진들/리액트/redux2.png" alt="React 컴포넌트 트리와 Redux 연동 예시" />
</div>

이 코드의 중요한 흐름은 다음과 같아요:

- 가장 위의 `App` 컴포넌트에서 store를 만든 뒤, `<Provider store={store}>`로 전체를 감쌉니다.
- `Left3` 컴포넌트는 `useSelector()`를 통해 현재 `number` 상태를 출력합니다.
- `Right3` 컴포넌트는 `useDispatch()`를 통해 `type: 'PLUS'` 액션을 전달합니다.
- 이 액션은 reducer에서 처리되어 상태가 변경되고, 해당 상태를 구독 중인 `Left3`가 자동으로 업데이트됩니다.

---

## 4. Redux vs 다른 상태 관리

| 항목                    | useState/useReducer                | Redux (react-redux)                     |
|-------------------------|-------------------------------------|-----------------------------------------|
| 상태 위치               | 각 컴포넌트 내부                    | 중앙 store에서 전역 관리                |
| 상태 공유               | props로 전달하거나 Context 필요     | 어느 컴포넌트든 직접 접근 가능          |
| 액션 처리 방식          | 컴포넌트 내부 로직                  | dispatch → reducer → 상태 변경          |
| 적합한 경우             | 간단한 컴포넌트 상태 관리           | 규모가 크고 상태가 여러 곳에서 필요할 때 |

---

## 5. 마무리

Redux는 단순한 상태 관리 도구를 넘어서, **앱 전체의 상태를 예측 가능하고 일관되게 관리할 수 있게 해줍니다.**  
특히 `react-redux` 라이브러리를 함께 쓰면, React와 Redux를 부드럽게 연결해주기 때문에 복잡한 구조에서도 효율적인 코드 작성이 가능해요.

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
