# 🗂️ user-auth-service 1인 개발 프로젝트 계획서

---

## ✅ 프로젝트 목표

- 네이버/쿠팡 수준의 대규모 인증 서비스 설계 및 구현
- 100만 DAU(일일 활성 사용자) 수준 트래픽 대응
- JWT, OAuth2, Refresh Token, 이메일 인증
- Spring Boot, Redis, Kafka, MySQL, Docker, AWS 실무 기술 스택

---

## ✅ 단계별 개발 플랜

---

### [0단계] 준비 완료

- 요구사항 정의서
- ERD 설계서
- 아키텍처 설계서

✅ 현재까지 완료됨

---

### [1단계] API 스펙 설계서 작성

- 엔드포인트 정의
- HTTP Method
- 요청/응답 예제
- 인증 여부 명시

---

### [2단계] 시퀀스 다이어그램 작성

- 회원가입 + 이메일 인증
- 로그인 + 토큰 재발급
- 마이페이지 조회
- OAuth2 로그인

---

### [3단계] DB 마이그레이션 스크립트 작성

- 테이블 생성 DDL
- 인덱스 정의
- 샘플 데이터(Optional)

---

### [4단계] Spring Boot 프로젝트 세팅

- Gradle/Maven
- application.yml
- DB/Redis/Kafka 접속 정보

---

### [5단계] Entity / Repository / Service / Controller 설계

- Clean Architecture 계층 분리
- DTO/Entity 설계
- Validation

---

### [6단계] 인증/보안 기능 구현

- Spring Security
- JWT 발급/검증
- Refresh Token 관리
- BCrypt 비밀번호 암호화

---

### [7단계] Kafka 연동

- 이메일 인증 이벤트 발행
- 로그인 기록 비동기 저장
- DLQ 처리

---

### [8단계] Redis 연동

- 이메일 인증 코드 캐싱
- Refresh Token 캐싱
- 블랙리스트 관리

---

### [9단계] 인덱스/쿼리 성능 최적화

- DDL 인덱스 추가
- EXPLAIN 계획 분석
- Slow Query 로그 분석

---

### [10단계] Swagger 문서화

- springdoc-openapi
- @Operation 주석
- Swagger-UI 제공

---

### [11단계] 테스트 코드 작성

- JUnit, Mockito
- 단위 테스트
- 통합 테스트

---

### [12단계] Docker/AWS 배포 준비

- Dockerfile, docker-compose
- EC2 배포
- Redis, Kafka 설정
- Nginx HTTPS

---

### [13단계] 운영/모니터링

- ELK Stack
- Prometheus + Grafana
- Slack 알림 연동

---

### [14단계] README 포트폴리오 문서화

- 설계서 링크
- 설치/배포 가이드
- 기술 스택 정리

---

## ✅ 📌 개발 전략

- 기능 설계 → 코드 구현 → 문서화 → 성능 튜닝 → 배포
- 단계별 Git 커밋, PR 리뷰 시뮬레이션
- 테스트 및 모니터링 전략 포함

---

## ✅ 📈 포트폴리오 강조 포인트

- 클린 아키텍처 적용
- 대규모 트래픽 대비 설계
- 보안(암호화, JWT, OAuth2)
- 비동기 처리(Kafka)
- 캐시(Redis)
- 운영 자동화(Docker, AWS)

---

# ✅ [끝]
