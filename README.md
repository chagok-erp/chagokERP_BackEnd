# 차곡ERP 백엔드

Spring Boot 기반 모듈형 모놀리스의 초기 골격이야. 도메인 구현과 데이터베이스 마이그레이션은 아직 없어.

## 필요 환경

- JDK 17+
- Docker Compose (로컬 PostgreSQL 사용 시)

Gradle은 Wrapper가 필요한 버전을 자동으로 내려받으므로 별도 설치가 필요 없어. 처음 실행할 때 인터넷 연결이 필요해.

## 로컬 실행

```powershell
cd C:\Users\<사용자명>\chagok_PROJECT\chagokERP_BackEnd
docker compose up -d postgres
.\gradlew.bat bootRun
```

PostgreSQL이 준비됐는지 확인하려면 `docker compose ps`를 실행해. `postgres`의 상태가 `healthy`로 나오면 준비된 거야. Docker Desktop이 먼저 실행 중이어야 해.

```powershell
docker compose ps
```

macOS/Linux에서는 아래처럼 실행해.

```bash
cd ~/chagok_PROJECT/chagokERP_BackEnd
docker compose up -d postgres
./gradlew bootRun
```

앱은 `http://localhost:8080`에서 실행돼. 인증되지 않은 요청은 Spring Security 로그인 화면으로 이동하고, `/actuator/health`만 인증 없이 확인할 수 있어. 세션 기반 인증과 CSRF 보호는 활성화되어 있어. 실제 계정·역할·로그인 API는 아직 구현하지 않았어.

환경 변수 `DB_URL`, `DB_USERNAME`, `DB_PASSWORD`, `SERVER_PORT`, `SESSION_COOKIE_SECURE`로 실행 설정을 바꿀 수 있어. 로컬 DB 기본 암호는 개발 전용이야.

## 설계 기준

- API 계약: `../chagok_docs/docs/api/openapi.yaml`
- DB·트랜잭션: `../chagok_docs/docs/database/DB-SPEC.md`
- A/B 도메인 책임: 구매·PR·PO는 A, 인증·창고·GR·재고는 B, GR 확정 트랜잭션 안에서 PO 수량 서비스를 호출하는 구조

도메인별 패키지는 계약과 담당 범위에 따라 기능 작업 브랜치에서 추가해.
