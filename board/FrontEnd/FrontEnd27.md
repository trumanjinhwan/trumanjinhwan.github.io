---
layout: post
title: "27. Next.js"
date: 2025-04-26
---

# Next.js

Next.js는 React 기반의 프레임워크로, **서버 사이드 렌더링(SSR)**, **정적 사이트 생성(SSG)**, **API 라우팅**, 그리고 **폴더 기반 라우팅** 등 다양한 기능을 제공합니다.  
이번 글에서는 Next.js의 기본적인 라우팅, API, 환경변수 사용법 등을 이미지 예제와 함께 알아보겠습니다.

---

## 1. API 라우팅과 동적 라우팅

<div style="text-align: center;">
  <img src="/사진들/리액트/nextjs1.png" alt="Next.js API 라우팅 및 동적 파라미터" />
</div>

Next.js에서는 `pages/api` 폴더에 파일을 생성하면 바로 API 경로로 동작합니다.  
위 예제는 `/api/[id]`와 같이 **동적 라우팅을 지원하는 API**입니다.

- `req.query.id`를 통해 URL의 `id` 값을 받아오고, 이를 `Number()`로 감싸서 숫자로 변환한 뒤 JSON으로 응답합니다.
- 예를 들어 `/api/1`에 요청하면 `{ "id": 1 }`이라는 응답이 옵니다.

---

## 2. 환경변수를 통한 API 호출

<div style="text-align: center;">
  <img src="/사진들/리액트/nextjs2.png" alt="환경변수 사용 예시" />
</div>

Next.js에서는 `.env` 파일을 통해 환경변수를 설정할 수 있습니다.  
단, 클라이언트에서 사용하는 변수는 **`NEXT_PUBLIC_`** 접두사가 필요합니다.

- `.env`에 `NEXT_PUBLIC_API_URL=http://localhost:3000/`을 설정해 두고
- `process.env.NEXT_PUBLIC_API_URL + 'api/hello'`로 fetch를 실행합니다.
- API에서 반환된 `user.name` 값을 받아서 화면에 표시합니다.

이 방식은 서버 주소를 여러 환경(개발, 배포)에 따라 쉽게 바꿀 수 있도록 해줍니다.

---

## 3. 동적 페이지 라우팅과 useRouter

<div style="text-align: center;">
  <img src="/사진들/리액트/nextjs3.png" alt="Next.js useRouter를 통한 파라미터 활용" />
</div>

Next.js에서는 파일 이름을 `[id].js`처럼 작성하면, 해당 위치에 오는 값이 **URL 파라미터**로 처리됩니다.

- `useRouter()`를 사용해 라우터 객체를 가져오고, `router.query.id`를 통해 동적 값을 읽어옵니다.
- 예를 들어 `/sub/1`에 접근하면 `1`이라는 값이 `id`로 받아지고, 이를 출력할 수 있습니다.

이 구조는 React Router와 다르게, **파일 구조만으로 라우팅을 자동 처리**해주는 게 큰 장점입니다.

---

## 4. 정리

Next.js는 React 개발자에게 매우 친숙한 구조를 제공하면서도,  
**SSR, API, 파일 기반 라우팅** 같은 강력한 기능들을 함께 제공합니다.  
위에서 본 것처럼 환경변수 설정, API 연동, 동적 라우팅까지 모두 간단하게 구현할 수 있습니다.



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
