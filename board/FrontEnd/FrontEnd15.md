---
layout: post
title: "15. 리액트 state"
date: 2025-04-09
---

# state

<div style="text-align: center;">
	<img src="/사진들/리액트/setId.png" alt="alt text" />
</div>

- 리액트에서 컴포넌트의 **내부 데이터(상태)**를 저장하고 싶을 땐 `useState()` 훅을 사용합니다.
- 예를 들어 버튼을 클릭했을 때, 화면에 표시되는 내용을 바꾸고 싶다면 `useState`를 통해 상태를 저장하고 `set함수`로 업데이트할 수 있습니다.
- 위 예시에서는 `setMode`를 호출해 `'READ'`로 모드를 바꾸고, 동시에 `setId(_id)`를 통해 현재 선택된 항목의 id도 저장하고 있습니다.

<br>

<div style="text-align: center;">
	<img src="/사진들/리액트/state 임포트.png" alt="alt text" />
</div>

- `useState`를 사용하려면 먼저 `react`에서 import 해줘야 합니다.
- `import { useState } from 'react';` ← 꼭 필요한 부분!

<br>

<div style="text-align: center;">
	<img src="/사진들/리액트/useState.png" alt="alt text" />
</div>

- `const [state, setState] = useState(초기값);` 형식으로 선언합니다.
- 예를 들어, 위 코드처럼
  - `const [mode, setMode] = useState('WELCOME');`
  - `const [id, setId] = useState(null);`
  이런 방식으로 여러 개의 상태를 다룰 수 있습니다.

<br>

<div style="text-align: center;">
	<img src="/사진들/리액트/형변환.png" alt="alt text" />
</div>

- 이벤트 함수 안에서 동적으로 값을 넣고 싶다면 `event.target.id`를 통해 전달받은 값을 사용하면 됩니다.
- 단, `event.target.id`는 문자열(string)이라 숫자형 상태(id)와 비교하려면 `Number()`를 써서 **형변환**을 해줘야 합니다.


