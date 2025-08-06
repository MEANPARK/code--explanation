
# 로그인 기능 추가 및 라우팅 설정 요약

이 문서는 "로그인" 링크 클릭 시 로그인 페이지가 나타나도록 설정하는 과정에서 변경된 내용을 요약합니다. 이 기능은 **React Router** 라이브러리를 사용하여 구현되었습니다.

---

### 1. React Router 라이브러리 설치

페이지 간 이동(라우팅) 기능을 사용하기 위해 `react-router-dom` 라이브러리를 프로젝트에 추가했습니다.

- **실행된 명령어:**
```bash
npm install react-router-dom
```

---

### 2. 로그인 페이지(`Login.tsx`) 생성

로그인 UI를 보여주는 새로운 페이지 컴포넌트를 생성했습니다.

- **새로 생성된 파일:** `src/pages/Login.tsx`
- **파일 내용:**
```tsx
const Login = () => {
  return (
    <div className="flex items-center justify-center min-h-screen bg-gray-100">
      <div className="w-full max-w-md p-8 space-y-6 bg-white rounded-lg shadow-md">
        <h2 className="text-2xl font-bold text-center">로그인</h2>
        <form className="space-y-6">
          <div>
            <label className="block text-sm font-medium text-gray-700">이메일</label>
            <input
              type="email"
              className="w-full px-3 py-2 mt-1 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500"
              placeholder="you@example.com"
            />
          </div>
          <div>
            <label className="block text-sm font-medium text-gray-700">비밀번호</label>
            <input
              type="password"
              className="w-full px-3 py-2 mt-1 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500"
              placeholder="********"
            />
          </div>
          <div>
            <button
              type="submit"
              className="w-full px-4 py-2 text-white bg-indigo-600 rounded-md hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
            >
              로그인
            </button>
          </div>
        </form>
      </div>
    </div>
  );
};

export default Login;
```

---

### 3. 라우터 설정 (`main.tsx` 수정)

애플리케이션의 진입점인 `main.tsx` 파일을 수정하여, 브라우저 주소(URL)에 따라 어떤 페이지를 보여줄지 규칙을 설정했습니다.

- **수정된 파일:** `src/main.tsx`
- **변경 내용:** 기존 `App.tsx`를 직접 렌더링하는 대신, `RouterProvider`를 사용하여 URL 경로에 따라 `Home` 또는 `Login` 컴포넌트를 렌더링하도록 변경했습니다.

**수정 전:**
```tsx
import { createRoot } from 'react-dom/client'
import './index.css'
import App from './App.tsx'

createRoot(document.getElementById('root')!).render(
  <>
    <App />
  </>,
)
```

**수정 후:**
```tsx
import { createRoot } from 'react-dom/client'
import './index.css'
import { createBrowserRouter, RouterProvider } from 'react-router-dom'
import Home from './pages/Home.tsx'
import Login from './pages/Login.tsx'

// URL 경로와 컴포넌트를 매핑하는 라우터 설정
const router = createBrowserRouter([
  {
    path: '/',
    element: <Home />,
  },
  {
    path: '/login',
    element: <Login />,
  },
])

// 라우터 설정을 앱에 적용
createRoot(document.getElementById('root')!).render(
  <>
    <RouterProvider router={router} />
  </>,
)
```

---

### 4. 헤더 링크 수정 (`Header.tsx` 수정)

기존의 일반 `<a>` 태그를 React Router의 `<Link>` 컴포넌트로 교체하여, 페이지 새로고침 없이 `/login` 경로로 부드럽게 이동하도록 수정했습니다.

- **수정된 파일:** `src/components/Header.tsx`
- **변경 내용:** `import { Link } ...` 구문을 추가하고, "로그인" 부분의 `<a>` 태그를 `<Link to="/login">`으로 변경했습니다.

**수정 전:**
```tsx
// ...
<a href="#" className="ml-4 hover:text-gray-800">로그인</a>
// ...
```

**수정 후:**
```tsx
import { Link } from 'react-router-dom';
// ...
<Link to="/login" className="ml-4 hover:text-gray-800">로그인</Link>
// ...
```
