
# 프론트엔드 개발 요약: 로그인 기능 구현 및 상태 관리

이 문서는 Gemini와 함께 프론트엔드 로그인 기능을 개발하고 관련 핵심 개념을 학습한 전체 과정을 요약합니다.

---

## 1. 초기 목표: 로그인 페이지 이동 기능 구현

가장 먼저, 헤더의 "로그인" 링크를 클릭했을 때, 별도의 로그인 페이지(`Login.tsx`)가 화면에 나타나도록 하는 것이 목표였습니다.

### 질문: SPA인가요, MPA인가요?

이 기능을 구현하기에 앞서, 현재 애플리케이션의 동작 방식에 대한 질문이 있었습니다.

- **답변:** 현재 방식은 **SPA (Single-Page Application)** 입니다.
- **이유:** React Router를 사용하여 페이지 전체를 새로고침하지 않고, 내용이 필요한 부분만 JavaScript가 동적으로 교체하기 때문입니다. 이는 더 빠르고 부드러운 사용자 경험을 제공합니다.

### 구현: React Router를 이용한 페이지 라우팅

SPA에서 페이지 이동을 구현하기 위해 `react-router-dom` 라이브러리를 사용했습니다.

**1. 라이브러리 설치**
```bash
npm install react-router-dom
```

**2. `Login.tsx` 페이지 생성 (`src/pages/Login.tsx`)**

간단한 UI를 가진 로그인 페이지 컴포넌트를 새로 만들었습니다.

**3. 라우터 설정 (`src/main.tsx`)**

URL 경로에 따라 어떤 컴포넌트를 보여줄지 규칙을 정의했습니다. `/` 경로는 `Home`을, `/login` 경로는 `Login`을 보여줍니다.

```tsx
// src/main.tsx
import { createBrowserRouter, RouterProvider } from 'react-router-dom';
import Home from './pages/Home.tsx';
import Login from './pages/Login.tsx';

const router = createBrowserRouter([
  { path: '/', element: <Home /> },
  { path: '/login', element: <Login /> },
]);

createRoot(document.getElementById('root')!).render(
  <RouterProvider router={router} />
);
```

**4. 헤더 링크 수정 (`src/components/Header.tsx`)**

단순 `<a>` 태그를 React Router의 `<Link>` 컴포넌트로 교체하여, 페이지 새로고침 없이 `/login`으로 이동하도록 수정했습니다.

```tsx
// 수정 전
<a href="#" ...>로그인</a>

// 수정 후
import { Link } from 'react-router-dom';
<Link to="/login" ...>로그인</Link>
```

---

## 2. 로그인 기능 구현: 백엔드 연동

단순 페이지 이동을 넘어, 실제 로그인 정보를 입력하고 백엔드 서버와 통신하는 기능을 구현했습니다.

### 구현: 로그인 폼 제출 (Fetch API)

사용자가 입력한 아이디와 비밀번호를 백엔드 API(`http://localhost:3000/users/login`)로 전송하도록 `Login.tsx`를 수정했습니다.

- **주요 로직:**
  - `useState`로 사용자의 아이디(`username`), 비밀번호(`password`) 입력을 관리합니다.
  - `<form>`의 `onSubmit` 이벤트를 처리하는 `handleSubmit` 함수를 만듭니다.
  - `fetch` 함수를 사용하여 `POST` 요청을 보내고, `body`에 사용자 정보를 JSON 형태로 담아 전송합니다.

```tsx
// src/pages/Login.tsx의 handleSubmit 함수 부분
const handleSubmit = async (event: React.FormEvent<HTMLFormElement>) => {
  event.preventDefault(); // 새로고침 방지
  try {
    const response = await fetch('http://localhost:3000/users/login', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ username, password }),
    });
    // ...응답 처리
  } catch (error) {
    // ...에러 처리
  }
};
```

---

## 3. 최종 목표: 로그인 상태에 따른 동적 UI 구현

로그인 성공 후, 단순히 알림창을 띄우는 것을 넘어 실제 웹사이트처럼 동작하도록 하는 것이 최종 목표였습니다.

- **요구사항:**
  1.  로그인 성공 시, 자동으로 홈페이지로 이동한다.
  2.  홈페이지 헤더의 "로그인" 링크가 "로그아웃" 버튼으로 변경된다.

### 질문: "전역" 상태는 모든 사용자가 공유하나요?

이 기능을 위해 "전역 상태 관리" 개념을 도입하기 전, 중요한 질문이 있었습니다.

- **답변:** **아닙니다.** 여기서 "전역"은 **한 명의 사용자가 보고 있는 브라우저 창 내부에서만** 유효한 독립적인 상태를 의미합니다. 다른 사용자의 브라우저 상태와는 완전히 분리되어 서로 영향을 주지 않습니다.

### 구현: Context API를 이용한 전역 상태 관리

React의 내장 기능인 **Context API**를 사용하여 로그인 상태를 관리하는 시스템을 구축했습니다.

**1. `AuthContext.tsx` 생성 (`src/AuthContext.tsx`)**

로그인 상태(`isLoggedIn`)와 상태를 변경할 함수(`login`, `logout`)를 담는 "설계도"이자 "저장소"를 만들었습니다.

```tsx
// src/AuthContext.tsx
import { createContext, useState, useContext, ReactNode } from 'react';

interface AuthContextType { /* ...타입 정의... */ }
const AuthContext = createContext<AuthContextType | undefined>(undefined);

export const AuthProvider = ({ children }: { children: ReactNode }) => {
  const [isLoggedIn, setIsLoggedIn] = useState(false);
  const login = () => setIsLoggedIn(true);
  const logout = () => setIsLoggedIn(false);
  // ...
};

export const useAuth = () => { /* ...커스텀 훅... */ };
```

**2. `AuthProvider` 적용 (`src/main.tsx`)**

애플리케이션 전체를 `AuthProvider`로 감싸, 모든 컴포넌트가 로그인 상태에 접근할 수 있도록 만들었습니다.

```tsx
// src/main.tsx
createRoot(document.getElementById('root')!).render(
  <AuthProvider>
    <RouterProvider router={router} />
  </AuthProvider>
);
```

**3. `Login.tsx` 수정**

로그인 성공 시, 전역 `login()` 함수를 호출하여 상태를 변경하고, `useNavigate`를 사용해 홈페이지로 이동시켰습니다.

```tsx
// src/pages/Login.tsx
import { useNavigate } from 'react-router-dom';
import { useAuth } from '../AuthContext';

const { login } = useAuth();
const navigate = useNavigate();

// ... handleSubmit 함수 내부 ...
if (response.ok) {
  login();      // 1. 전역 상태 변경
  navigate('/'); // 2. 페이지 이동
}
```

**4. `Header.tsx` 수정**

전역 상태(`isLoggedIn`)를 구독하여, 값에 따라 조건부로 "로그인" 링크 또는 "로그아웃" 버튼을 보여주도록 수정했습니다.

```tsx
// src/components/Header.tsx
import { useAuth } from '../AuthContext';

const { isLoggedIn, logout } = useAuth();

// ... JSX 내부 ...
{
  isLoggedIn ? (
    <button onClick={logout}>로그아웃</button>
  ) : (
    <Link to="/login">로그인</Link>
  )
}
```

## 결론

이 과정을 통해 단순 UI 개발을 넘어, React Router를 이용한 SPA 라우팅, Fetch API를 통한 백엔드 통신, 그리고 Context API를 활용한 전역 상태 관리까지 성공적으로 구현했습니다. 이를 통해 실제 웹 애플리케이션과 유사한 로그인/로그아웃 흐름을 완성할 수 있었습니다.
