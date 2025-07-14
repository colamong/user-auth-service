
---

## 인증/회원가입 카테고리 - 전체 Markdown 설계서


# 📌 API 스펙 설계서 - [카테고리: 인증/회원가입]

---

## ✅ 엔드포인트 목록

| 기능                     | 엔드포인트                             | Method | 인증 | 설명                                    |
|--------------------------|----------------------------------------|--------|------|-----------------------------------------|
| 회원가입                 | /auth/signup                           | POST   | ❌   | 이메일/비밀번호 회원가입                |
| 이메일 인증 코드 전송     | /auth/email/send-code                   | POST   | ❌   | 이메일 인증 코드 발송                   |
| 이메일 인증 코드 검증     | /auth/email/verify-code                 | POST   | ❌   | 이메일 인증 코드 검증                   |
| 로그인                    | /auth/login                            | POST   | ❌   | 이메일/비밀번호 로그인                   |
| OAuth2 로그인             | /auth/oauth/{provider}                 | GET    | ❌   | 소셜 로그인 리디렉트 URL                |
| OAuth2 콜백 처리          | /auth/oauth/{provider}/callback        | GET    | ❌   | 소셜 인증 코드 처리, 토큰 발급          |
| AccessToken 재발급        | /auth/refresh                          | POST   | ✅   | RefreshToken으로 AccessToken 재발급     |
| 로그아웃                  | /auth/logout                           | POST   | ✅   | RefreshToken 무효화 및 로그아웃         |
| 비밀번호 재설정 요청       | /auth/password/reset-request           | POST   | ❌   | 비밀번호 재설정 이메일 요청             |
| 비밀번호 재설정 완료       | /auth/password/reset                   | POST   | ❌   | 이메일 링크로 받은 토큰으로 새 비밀번호 설정 |
| 2FA 설정                  | /auth/2fa/setup                        | POST   | ✅   | 2FA QR코드/시크릿 생성                   |
| 2FA 검증                  | /auth/2fa/verify                       | POST   | ✅   | 2FA 코드 검증                            |

---

## 각 엔드포인트 상세 스펙


###  1. /auth/signup [POST]
- 인증: ❌
- 설명: 이메일/비밀번호 기반 회원가입

#### 요청 Body
```json
{
  "email": "user@example.com",
  "password": "StrongPassword!123",
  "nickname": "myNickname"
}

#### 응답 Body

```json
{
  "message": "Signup successful. Please verify your email."
}
```

#### 상태 코드

| 코드  | 설명      |
| --- | ------- |
| 201 | 회원가입 성공 |
| 400 | 파라미터 오류 |
| 409 | 이메일 중복  |

#### 오류 예시

```json
{
  "message": "Email already exists.",
  "code": "EMAIL_DUPLICATE"
}
```

#### 비즈니스 규칙

* 이메일 중복 불가
* 비밀번호 최소 8자, 대문자/소문자/숫자/특수문자 포함

#### 비고 / 노트

* 이메일 인증 완료 후 계정 활성화

---

### 2. /auth/email/send-code \[POST]

* 인증: ❌
* 설명: 이메일 인증 코드 전송

#### 요청 Body

```json
{
  "email": "user@example.com"
}
```

#### 응답 Body

```json
{
  "message": "Verification code sent to your email."
}
```

#### 상태 코드

| 코드  | 설명            |
| --- | ------------- |
| 200 | 코드 전송 성공      |
| 400 | 파라미터 오류       |
| 429 | Rate Limit 초과 |

#### 오류 예시

```json
{
  "message": "Too many requests.",
  "code": "RATE_LIMIT_EXCEEDED"
}
```

#### 비즈니스 규칙

* 인증코드 6자리
* TTL 5분

#### 비고 / 노트

* 동일 이메일 기준 5회/시간 제한

---

### 3. /auth/email/verify-code \[POST]

* 인증: ❌
* 설명: 이메일 인증 코드 검증

#### 요청 Body

```json
{
  "email": "user@example.com",
  "code": "123456"
}
```

#### 응답 Body

```json
{
  "message": "Email verified successfully."
}
```

#### 상태 코드

| 코드  | 설명     |
| --- | ------ |
| 200 | 인증 성공  |
| 400 | 잘못된 코드 |
| 410 | 만료된 코드 |

#### 오류 예시

```json
{
  "message": "Invalid verification code.",
  "code": "INVALID_CODE"
}
```

#### 비즈니스 규칙

* 잘못된 코드 5회 이상 시 계정 잠금

#### 비고 / 노트

* 만료된 코드 거부

---

### 4️. /auth/login \[POST]

* 인증: ❌
* 설명: 이메일/비밀번호 로그인

#### 요청 Body

```json
{
  "email": "user@example.com",
  "password": "StrongPassword!123"
}
```

#### 응답 Body

```json
{
  "accessToken": "jwt-access-token",
  "refreshToken": "jwt-refresh-token"
}
```

#### 상태 코드

| 코드  | 설명     |
| --- | ------ |
| 200 | 로그인 성공 |
| 401 | 인증 실패  |

#### 오류 예시

```json
{
  "message": "Invalid credentials.",
  "code": "INVALID_CREDENTIALS"
}
```

#### 비즈니스 규칙

* BCrypt 해싱 비교
* 로그인 실패 5회 이상 계정 잠금

#### 비고 / 노트

* 로그인 시 로그인 기록 저장

---

### 5. /auth/oauth/{provider} \[GET]

* 인증: ❌
* 설명: 소셜 로그인 리디렉트 URL 제공

#### 응답 Body

```json
{
  "redirectUrl": "https://accounts.google.com/o/oauth2/auth?...client_id..."
}
```

#### 비고 / 노트

* 지원: google, kakao, naver
* 상태 파라미터로 CSRF 방지

---

### 6. /auth/oauth/{provider}/callback \[GET]

* 인증: ❌
* 설명: 소셜 로그인 인증 코드 처리

#### 응답 Body

```json
{
  "accessToken": "jwt-access-token",
  "refreshToken": "jwt-refresh-token"
}
```

#### 비고 / 노트

* Provider access token 검증
* 신규 사용자는 자동 회원가입

---

### 7. /auth/refresh \[POST]

* 인증: ✅ (RefreshToken)
* 설명: AccessToken 재발급

#### 요청 Body

```json
{
  "refreshToken": "jwt-refresh-token"
}
```

#### 응답 Body

```json
{
  "accessToken": "new-jwt-access-token"
}
```

#### 비고 / 노트

* RefreshToken Redis 저장
* 만료기간 14일

---

### 8. /auth/logout \[POST]

* 인증: ✅
* 설명: 로그아웃 처리

#### 요청 Body

```json
{
  "refreshToken": "jwt-refresh-token"
}
```

#### 응답 Body

```json
{
  "message": "Logged out successfully."
}
```

#### 비고 / 노트

* RefreshToken Redis에서 삭제
* AccessToken 블랙리스트 등록

---

### 9. /auth/password/reset-request \[POST]

* 인증: ❌
* 설명: 비밀번호 재설정 이메일 요청

#### 요청 Body

```json
{
  "email": "user@example.com"
}
```

#### 응답 Body

```json
{
  "message": "Password reset email sent."
}
```

#### 비고 / 노트

* 이메일 링크 만료시간 30분
* Rate Limit 5회/시간

---

### 10. /auth/password/reset \[POST]

* 인증: ❌
* 설명: 비밀번호 재설정 완료

#### 요청 Body

```json
{
  "resetToken": "reset-link-token",
  "newPassword": "NewStrongPassword!456"
}
```

#### 응답 Body

```json
{
  "message": "Password reset successful."
}
```

#### 비고 / 노트

* 비밀번호 정책 동일 적용

---

### 11. /auth/2fa/setup \[POST]

* 인증: ✅
* 설명: 2FA QR코드/시크릿 생성

#### 응답 Body

```json
{
  "qrCodeUrl": "otpauth://totp/.../user@example.com",
  "secret": "JBSWY3DPEHPK3PXP"
}
```

#### 비고 / 노트

* TOTP 기반 (Google Authenticator 호환)

---

### 12. /auth/2fa/verify \[POST]

* 인증: ✅
* 설명: 2FA 코드 검증

#### 요청 Body

```json
{
  "code": "123456"
}
```

#### 응답 Body

```json
{
  "message": "2FA verified successfully."
}
```

#### 비고 / 노트

* OTP 코드 6자리
* 30초 주기

---

