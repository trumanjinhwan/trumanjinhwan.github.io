---
layout: post
title: "22. 리액트 Styled components"
date: 2025-04-19
---

# 리액트에서 Styled Components 사용하기

이번 글에서는 React에서 스타일을 적용할 수 있는 다양한 방식 중 하나인 **styled-components**에 대해 알아봅니다. 기존 인라인 스타일과 어떤 차이가 있고, 실제로 어떻게 적용되는지 비교해볼게요.

---

## 1. 인라인 스타일 방식

가장 기본적인 방식은 JSX 내부에 직접 `style={{ }}` 형태로 CSS 속성을 지정하는 것입니다. 아래는 간단한 버튼을 인라인 스타일로 정의한 예시입니다.

<div style="text-align: center;">
  <img src="/사진들/리액트/리액트 styled components.png" alt="인라인 스타일과 styled-components 비교" />
</div>

위 예시에서 상단에 있는 초록색 "Simple" 버튼은 다음과 같이 정의되어 있습니다:

- JSX 내부에서 `style={{ color: 'white', backgroundColor: 'green' }}` 로 직접 작성
- 유지보수나 재사용성 측면에서는 불편할 수 있음

---

## 2. styled-components 방식

`styled-components`는 CSS-in-JS 방식으로 컴포넌트에 직접 스타일을 부여할 수 있는 라이브러리입니다. 아래처럼 새로운 스타일 컴포넌트를 만들고 JSX에서 사용합니다.

예시:

- `const SimpleButton = styled.button\`color: white; background-color: green;\`;`
- 이 컴포넌트는 마치 일반 React 컴포넌트처럼 `<SimpleButton>Simple</SimpleButton>`으로 사용 가능

이렇게 하면 **스타일과 컴포넌트가 결합되어 가독성이 좋아지고, 재사용도 쉬워집니다.**

---

## 3. 스타일 확장도 간편

또한 `styled-components`는 이미 정의된 스타일을 기반으로 새로운 스타일을 확장할 수 있습니다. 예를 들어, 기존의 `SimpleButton`을 기반으로 폰트 크기만 더 키운 `LargeButton`을 만들 수도 있죠.

- `const LargeButton = styled(SimpleButton)\`font-size: 50px;\`;`

이 방식은 CSS의 상속 개념처럼 작동해, **코드의 중복을 줄이고 유지보수성을 향상**시켜줍니다.

---

## 4. props를 활용한 조건부 스타일링

styled-components는 컴포넌트에 전달된 `props`에 따라 스타일을 동적으로 변경할 수도 있습니다. 예를 들어, 아래의 `PrimaryButton`은 `props.primary`가 true일 때 흰색 글씨, 아니면 검정색 글씨를 사용합니다.

이처럼 **조건부 스타일링이 매우 직관적**이고 깔끔하게 처리됩니다.

---

## 정리

| 비교 항목              | 인라인 스타일                          | styled-components                                    |
|------------------------|------------------------------------------|-----------------------------------------------------|
| 사용 방식              | JSX 내부 style 속성 사용                | 별도 컴포넌트 생성 (`styled.div` 등)                |
| 재사용성               | 낮음                                     | 높음                                                 |
| 가독성 및 유지보수     | 스타일이 컴포넌트와 분리됨              | 스타일과 구조가 함께 있어 더 직관적                   |
| 조건부 스타일링        | JS 코드에서 if문 등 별도 로직 필요       | props를 통해 직접 스타일 내에 조건 처리 가능         |
| 스타일 확장성          | 중복 많음                                 | `styled()` 함수로 기존 스타일 확장 가능              |

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
