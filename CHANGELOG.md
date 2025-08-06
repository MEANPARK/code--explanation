# 프론트엔드 변경 사항 요약 (카카오 로그아웃 기능 구현)

이 문서는 카카오 로그인 후 로그아웃 시, 카카오 계정 세션까지 만료시켜 다음 로그인에 ID/PW를 다시 입력하도록 하는 기능 구현을 위한 프론트엔드 코드 변경 사항을 요약합니다.

## 주요 변경 파일 및 내용

### 1. `src/AuthContext.tsx`

인증 상태 관리의 핵심 로직을 수정했습니다.

- **상태 관리 방식 변경**: 기존의 단순 `isLoggedIn` boolean 상태 대신, 사용자 정보를 담는 `user` 객체로 상태를 관리하도록 변경했습니다.
  ```typescript
  interface User {
    provider?: 'kakao' | 'google' | 'github' | 'local';
  }
  const [user, setUser] = useState<User | null>(null);
  ```
- **로그인 방식(`provider`) 저장**: `user` 객체 안에 어떤 방식(카카오, 구글, 일반 로그인 등)으로 로그인했는지 식별하기 위한 `provider` 필드를 추가했습니다.
- **로그아웃 로직 수정**:
  - 로그아웃 함수(`logout`)가 호출되면, 현재 `user` 상태에 저장된 `provider` 값을 확인합니다.
  - 만약 `provider`가 `'kakao'`이면, 백엔드에 로그아웃 요청을 보낸 후 **카카오 로그아웃 URL로 페이지를 리디렉션**합니다.
  - 이를 통해 우리 서비스의 세션뿐만 아니라 카카오 자체의 세션도 만료시킵니다.
  ```javascript
  if (provider === 'kakao') {
    const REST_API_KEY = import.meta.env.VITE_KAKAO_REST_API_KEY;
    const LOGOUT_REDIRECT_URI = import.meta.env.VITE_KAKAO_LOGOUT_REDIRECT_URI;
    const kakaoLogoutUrl = `https://kauth.kakao.com/oauth/logout?client_id=${REST_API_KEY}&logout_redirect_uri=${LOGOUT_REDIRECT_URI}`;
    window.location.href = kakaoLogoutUrl;
  }
  ```
- **초기 로그인 상태 확인**: 앱이 시작될 때 백엔드 API(`api/users/me`)를 호출하여 현재 로그인된 사용자의 정보(및 `provider`)를 가져와 상태를 설정합니다.
- **코드 품질 개선**: TypeScript 및 ESLint 경고를 해결하기 위해 `import` 구문을 수정하고, 사용하지 않는 `isLoggedIn` 변수를 제거했습니다.

### 2. `src/components/Header.tsx`

`AuthContext`의 변경 사항을 반영하여 헤더 컴포넌트를 수정했습니다.

- **로그인 상태 UI 변경**: `isLoggedIn` boolean 대신 `user` 객체의 존재 여부 (`!!user`)로 로그인/로그아웃 버튼 표시를 결정합니다.
- **로그아웃 핸들러 수정**: `handleLogout` 함수가 `AuthContext`의 새로운 `logout` 함수를 호출하도록 유지했습니다. 카카오 로그인이 아닌 경우에만 수동으로 메인 페이지로 이동시키는 로직을 추가했습니다.
- **JSX 구조 복원**: 이전 수정 과정에서 실수로 누락되었던 `마이페이지`, `장바구니` 링크와 `</div>` 태그를 복원하여 깨졌던 레이아웃을 바로잡았습니다.

### 3. `src/pages/Login.tsx`

일반(local) 로그인 시 `provider` 정보를 올바르게 설정하도록 수정했습니다.

- **일반 로그인 처리**: 사용자가 아이디와 비밀번호로 로그인에 성공하면, `AuthContext`의 `login` 함수를 호출할 때 `{ provider: 'local' }` 정보를 전달하여 로그인 방식을 명시적으로 기록합니다.

---

## 추가 논의: 로그인 시 ID/PW 재입력 (`prompt=login`)

사용자가 이미 카카오에 로그인된 상태라도, 우리 서비스에서 로그인 버튼을 누를 때마다 ID/PW를 다시 입력하게 하려면 `prompt=login` 파라미터를 사용해야 합니다.

- **적용 시점**: 이 파라미터는 **로그아웃이 아닌, 로그인을 시작할 때** 사용됩니다.
- **수정 위치**: 프론트엔드가 아닌 **백엔드 서버**에서 카카오 로그인 페이지로 리디렉션하는 URL을 생성할 때 이 파라미터를 추가해야 합니다.

#### 백엔드 수정 예시 (Node.js / Passport.js)

백엔드의 카카오 로그인 라우터에서 `passport.authenticate`의 옵션으로 `{ prompt: 'login' }`을 추가합니다.

```javascript
// 예시: /routes/auth.js

// 수정 전
router.get('/kakao', passport.authenticate('kakao'));

// 수정 후
router.get('/kakao', passport.authenticate('kakao', {
  prompt: 'login'
}));
```

## 결론 및 필수 확인 사항

- **프론트엔드**: 위와 같이 `AuthContext`를 중심으로 코드를 수정하여 조건부 카카오 로그아웃 로직을 구현했습니다.
- **백엔드**: 이 기능이 완벽하게 동작하려면, 백엔드에서 다음 두 가지가 반드시 구현되어야 합니다.
  1. `/api/users/me` 엔드포인트가 사용자 정보 응답에 `{ "provider": "kakao" }` 와 같이 로그인 방식을 포함해야 합니다.
  2. 카카오 로그인 인증을 시작하는 로직에 `prompt=login` 옵션을 추가해야 합니다.
