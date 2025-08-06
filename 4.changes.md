# 소셜 로그인 기능 추가 변경 사항

## 1. `src/pages/Login.tsx`

- **변경 내용:** 카카오, Google, GitHub 로그인 버튼에 `onClick` 이벤트를 추가하여, 클릭 시 백엔드의 소셜 로그인 API 엔드포인트로 이동하도록 수정했습니다.
- **구현 방식:**
  - `handleSocialLogin` 함수를 추가하여, `provider` 값(kakao, google, github)에 따라 동적으로 API 경로를 생성합니다.
  - `window.location.href`를 사용하여 해당 경로로 페이지를 이동시킵니다.

```tsx
// ... (기존 코드)

const Login = () => {
  // ... (기존 코드)
  const { login } = useAuth();

  const handleSocialLogin = (provider: 'google' | 'kakao' | 'github') => {
    window.location.href = `http://localhost:3000/auth/${provider}`;
  };

  const handleSubmit = async (event: React.FormEvent<HTMLFormElement>) => {
    // ... (기존 코드)
  };

  return (
    // ... (기존 코드)
        <div className="space-y-4">
          <button
            type="button"
            onClick={() => handleSocialLogin('kakao')}
            className="w-full flex items-center justify-center px-4 py-2 border border-transparent rounded-md shadow-sm text-sm font-medium text-black bg-[#FEE500] hover:bg-yellow-400"
          >
            카카오로 로그인
          </button>
          <button
            type="button"
            onClick={() => handleSocialLogin('google')}
            className="w-full flex items-center justify-center px-4 py-2 border border-gray-300 rounded-md shadow-sm text-sm font-medium text-gray-700 bg-white hover:bg-gray-50"
          >
            Google로 로그인
          </button>
          <button
            type="button"
            onClick={() => handleSocialLogin('github')}
            className="w-full flex items-center justify-center px-4 py-2 border border-transparent rounded-md shadow-sm text-sm font-medium text-white bg-[#333] hover:bg-gray-800"
          >
            GitHub으로 로그인
          </button>
        </div>
    // ... (기존 코드)
  );
};

export default Login;
```

## 2. `src/pages/SocialLoginCallback.tsx` (신규 파일)

- **파일 추가:** 소셜 로그인 성공 후, 백엔드에서 리디렉션될 콜백 페이지를 처리하기 위한 컴포넌트를 새로 생성했습니다.
- **주요 기능:**
  - 페이지가 로드되면 `useEffect`를 통해 `AuthContext`의 `login()` 함수를 호출하여 전역 로그인 상태를 `true`로 변경합니다.
  - 로그인 상태 변경 후, 사용자를 홈페이지(`/`)로 즉시 리디렉션합니다.

```tsx
import { useEffect } from 'react';
import { useNavigate } from 'react-router-dom';
import { useAuth } from '../AuthContext';

const SocialLoginCallback = () => {
  const { login } = useAuth();
  const navigate = useNavigate();

  useEffect(() => {
    // 소셜 로그인 성공 후, 백엔드에서 이 페이지로 리디렉션되면
    // 전역 상태를 "로그인됨"으로 변경하고 홈페이지로 이동합니다.
    login();
    navigate('/');
  }, [login, navigate]);

  return (
    <div className="flex items-center justify-center min-h-screen">
      <p>로그인 처리 중...</p>
    </div>
  );
};

export default SocialLoginCallback;
```

## 3. `src/main.tsx`

- **변경 내용:** `react-router-dom`의 라우터 설정에 소셜 로그인 콜백 경로를 추가했습니다.
- **구현 방식:**
  - `/auth/kakao/callback`, `/auth/google/callback`, `/auth/github/callback` 경로로 접근 시, 새로 만든 `SocialLoginCallback` 컴포넌트가 렌더링되도록 설정했습니다.

```tsx
import { createRoot } from 'react-dom/client'
import './index.css'
import { createBrowserRouter, RouterProvider } from 'react-router-dom'
import Home from './pages/Home.tsx'
import Login from './pages/Login.tsx'
import SocialLoginCallback from './pages/SocialLoginCallback.tsx' // 추가
import { AuthProvider } from './AuthContext.tsx'

const router = createBrowserRouter([
  {
    path: '/',
    element: <Home />,
  },
  {
    path: '/login',
    element: <Login />,
  },
  {
    path: '/auth/kakao/callback',
    element: <SocialLoginCallback />,
  },
  {
    path: '/auth/google/callback',
    element: <SocialLoginCallback />,
  },
  {
    path: '/auth/github/callback',
    element: <SocialLoginCallback />,
  },
])

createRoot(document.getElementById('root')!).render(
  <>
    <AuthProvider>
      <RouterProvider router={router} />
    </AuthProvider>
  </>,
)
```
