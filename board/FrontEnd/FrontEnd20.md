---
layout: post
title: "20. 리액트 Link, NavLink, HashRouter"
date: 2025-04-16
---

# React Router의 다양한 라우팅 방식 비교

이번 글에서는 `react-router-dom`에서 자주 사용되는 컴포넌트인 `Link`, `NavLink`, 그리고 라우터 설정에 쓰이는 `BrowserRouter`, `HashRouter`의 차이를 알아보겠습니다. 각각의 특징과 동작 방식을 이미지와 함께 비교해볼게요.

---

## 1. NavLink: 현재 페이지 강조가 필요한 경우

`NavLink`는 `Link`와 거의 동일한 기능을 하지만, 현재 활성화된 링크에 자동으로 클래스를 추가해줍니다.

아래 예시처럼 `Topics`를 클릭하면 해당 링크에 class="active"가 적용된 것을 확인할 수 있어요.

<div style="text-align: center;">
  <img src="/사진들/리액트/NavLink 컴포넌트.png" alt="NavLink 사용 예시" />
</div>

이처럼 현재 페이지와 일치하는 링크에 스타일을 쉽게 적용할 수 있어 메뉴 UI에 적합합니다.

---

## 2. NavLink 임포트 방식

사용하기 위해선 `react-router-dom`에서 NavLink를 import 해야 합니다.

<div style="text-align: center;">
  <img src="/사진들/리액트/NavLink 임포트.png" alt="NavLink 임포트 예시" />
</div>

불필요한 컴포넌트는 import하지 않도록 주의하고, 사용하지 않으면 경고가 발생할 수 있어요.

---

## 3. Link: 단순한 페이지 이동에 적합

`Link`는 가장 기본적인 페이지 이동을 처리할 때 사용합니다. 하지만 active 클래스를 자동으로 부여하진 않아요.

아래는 Link만을 사용해 구현한 예시입니다. 메뉴는 잘 작동하지만, 어떤 페이지에 있는지 스타일로는 구분되지 않죠.

<div style="text-align: center;">
  <img src="/사진들/리액트/Link 컴포넌트.png" alt="Link 컴포넌트 사용 예시" />
</div>

---

## 4. Link 임포트 방식

Link도 마찬가지로 react-router-dom에서 가져와야 합니다.

<div style="text-align: center;">
  <img src="/사진들/리액트/Link 임포트.png" alt="Link 임포트 예시" />
</div>

단순한 라우팅이 필요한 경우, 스타일링 없이 깔끔하게 사용할 수 있습니다.

---

## 5. HashRouter vs BrowserRouter

라우터를 정의할 때는 보통 BrowserRouter를 사용하지만, 환경에 따라 HashRouter를 쓰기도 합니다.

<div style="text-align: center;">
  <img src="/사진들/리액트/HashRouter 임포트.png" alt="HashRouter 사용 예시" />
</div>

### HashRouter

- URL에 '#'이 포함됨 (예: `/#/topics`)
- 정적 파일 기반 환경에서 적합 (서버 설정 없이도 작동)

### BrowserRouter

- 깔끔한 URL (예: `/topics`)
- 서버에서 라우팅 처리가 필요 (404 방지용 리디렉션 설정 필요)

---

## 정리

| 비교 항목        | Link             | NavLink           | BrowserRouter       | HashRouter              |
|------------------|------------------|-------------------|----------------------|--------------------------|
| 스타일 적용       | ❌ 수동 설정 필요 | ✅ 자동 class 적용 | ✅ URL 깔끔           | ✅ 서버 설정 필요 없음     |
| 현재 위치 인식    | ❌ 불가           | ✅ 가능            | ✅ 경로 사용           | ✅ 해시 기반 경로 사용      |
| URL 예시         | `/topics`        | `/topics`         | `/topics`           | `/#/topics`              |
| 사용 용도         | 단순 이동         | 메뉴 강조           | 일반적 웹사이트 라우팅 | 정적 서버, GitHub Pages 등 |

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
