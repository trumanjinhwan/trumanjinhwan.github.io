---
layout: post
title: "21. 리액트 라우터 Nested Routing"
date: 2025-04-17
---

# 리액트 라우터에서 Nested Routing 사용하기

이번 글에서는 `react-router-dom`을 사용할 때 자주 등장하는 **중첩 라우팅(Nested Routing)** 개념을 살펴보겠습니다. 여러 개의 컴포넌트를 라우팅 구조에 따라 자연스럽게 중첩 렌더링하고 싶을 때 매우 유용하죠.

---

## 1. 중첩된 라우팅 구조 예시

다음은 중첩 라우팅을 사용해서 `Topics` 내의 각각의 상세 페이지를 구성한 예시입니다. `topic_id`를 URL 파라미터로 받아 해당 항목의 상세 내용을 보여줍니다.

<div style="text-align: center;">
  <img src="/사진들/리액트/topic.png" alt="useParams를 이용한 topic 상세 페이지" />
</div>

위 예시에서 보듯이, URL의 파라미터(`topic_id`)를 `useParams()`로 가져와서, 해당 데이터를 찾아 출력합니다.

---

## 2. 토픽 데이터 배열과 링크 목록 구성

중첩 라우팅을 구성하기 위해선 각 항목으로 이동할 수 있는 링크가 필요합니다. 여기선 `NavLink`를 사용해 현재 선택된 항목을 강조할 수 있도록 했습니다.

<div style="text-align: center;">
  <img src="/사진들/리액트/topcis.png" alt="토픽 리스트와 NavLink로 연결" />
</div>

예시:
  
`<NavLink to={"/topics/" + contents[i].id}> {contents[i].title} </NavLink>`

각 항목을 누르면, URL이 `/topics/1`, `/topics/2`처럼 동적으로 바뀌고, 해당 내용을 불러오게 됩니다.

---

## 3. Topics 컴포넌트 내에서 중첩 Route 설정

`Topics` 컴포넌트 안에서 `<Routes>`와 `<Route>`를 선언해, 내부에 중첩된 라우트를 설정해줘야 합니다. 아래 예시처럼 `/topics/:topic_id` 형식의 경로를 `Route`로 추가하면 됩니다.

<div style="text-align: center;">
  <img src="/사진들/리액트/nested routing.png" alt="중첩된 Route 설정 예시" />
</div>

예시:

`<Routes> <Route path=":topic_id" element={<Topic />} /> </Routes>`

> ⚠️ 주의: 중첩된 Route에서는 `path="/topics/:id"`가 아닌 `path=":id"`처럼 상대 경로로 작성해야 합니다. 이미 Topics에서 `/topics`를 사용하고 있기 때문이에요.

---

## 정리

| 항목                      | 설명                                                                 |
|---------------------------|----------------------------------------------------------------------|
| 중첩 라우팅 정의 위치      | 부모 컴포넌트 (`Topics`) 내부에서 `<Routes>`로 설정                      |
| URL 파라미터 추출 방식     | `useParams()`를 사용해 URL에서 동적 값 추출                              |
| 라우팅 경로 작성 방법      | 상대 경로 (`:topic_id`)로 중첩 라우트 작성                               |
| 링크 컴포넌트 추천         | `NavLink`를 사용해 현재 위치 강조 및 스타일 지정 가능                    |
