# 요구사항 정의서 - user-auth-service (v0.1)

---

## 1. 기본 기능 요구사항

### 1.1 회원가입 기능

- 회원가입 방식: 이메일 기반
- 회원가입 항목: 이메일, 비밀번호, 닉네임
- 중복체크: 이메일, 닉네임 중복 체크 필요
- 비밀번호 암호화: BCrypt 사용
- 이메일 인증: 필요 6자리 인증코드 (유효시간: 5분)

### 1.2 로그인 기능

- 로그인 방식: JWT 기반 토큰 로그인
- OAuth2 소셜 로그인: Google, Kakao, Naver 지원
- 토큰 유형: Access Token, Refresh Token 분리 관리
- Refresh Token 저장소: Redis
- 토큰 재발급 기능: 필요 (Refresh Token 이용)
- 토큰 만료 정책: Access Token(1시간), Refresh Token(14일)
- 토큰 갱신: Access Token 만료 5분 전 자동 무음 갱신
- 로그아웃 시 Refresh Token Redis에서 즉시 무효화
- 비활성 사용자: 14일 후 재로그인 필요

### 1.3 마이페이지 기능

- 접근권한: 인증된 사용자만 접근 가능
- 제공 기능: 회원정보 조회, 닉네임 수정, 비밀번호 변경
- 이메일 변경 가능 여부: 불가 (추후 관리자 기능으로 고려)

### 1.4 보안 기능

- CSRF 방어: JWT 기반이라 기본 비활성화 (관리 콘솔은 활성화 고려)
- XSS 방어: 입력 Validation 및 Output Encoding 적용
- HTTPS: 필수 적용 (AWS ACM 활용)
- 비밀번호 보안: BCrypt
- 비밀번호 정책: 최소 8자, 영문+숫자+특수문자 조합
- 세션 관리: Stateless (JWT + Redis RefreshToken)
- OAuth2 클라이언트 보안: state 파라미터 및 PKCE 적용 고려

### 1.5 확장성 기능

- Redis 사용처: RefreshToken 저장, 캐싱
- Kafka 사용처: 이메일 발송, 알림, 감사로그 비동기 처리
- 로드밸런싱: AWS ALB 적용
- 마이크로서비스 확장성: Auth Service → User Service 분리 가능성 고려
- 서버 확장: 무중단 Blue/Green 배포 지원

### 1.6 인프라 및 운영 기능

- 클라우드: AWS (EC2, RDS, ElastiCache, S3, MSK)
- 컨테이너: Docker
- Kubernetes: 선택 적용 (AWS EKS 확장 가능성)
- CI/CD: GitHub Actions
- 모니터링: Prometheus, Grafana, CloudWatch
- 로깅: ELK Stack

### 1.7 문서화

- API 명세: Swagger (springdoc-openapi)
- DB 설계: ERD 작성
- 아키텍처 다이어그램: 작성
- 운영 문서: README 작성

---

## 2. 기술 스택 (최종안)

| 영역 | 기술 스택 |
|--|--|
| Backend | Spring Boot 3.x, Spring Security 6.x, JWT, OAuth2 Client |
| Database | MySQL (AWS RDS) |
| Cache | Redis (ElastiCache) |
| Messaging | Kafka (MSK) |
| CI/CD | GitHub Actions |
| Deployment | Docker + AWS EC2 or EKS |
| Monitoring | Prometheus + Grafana + ELK |
| API Docs | springdoc-openapi |
| Language | Java 17 |

