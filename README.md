# PAYZLE

> **복잡한 정산을 간단하게, 페이즐.**

PAYZLE은 여러 사람이 함께 사용한 비용을 기록하고, 각자의 부담 금액과 정산 내역을 계산하는 공동 지출 관리 웹 서비스입니다.

단순한 정산 기능 구현에서 시작하여 **백엔드 아키텍처 설계, AWS 인프라 구축, CI/CD, 모니터링 및 분산 시스템**까지 단계적으로 학습하고 실험하는 팀 프로젝트입니다.

현재 상태: `Planning`  
프로젝트 유형: `Team Side Project`  
주요 목표: `Portfolio` · `Infrastructure` · `Backend` · `DevOps`

---

## 01. Overview

### 프로젝트 배경

여행, 모임, 공동생활 등 여러 사람이 비용을 나누는 상황에서는 누가 얼마를 지출했고, 누구에게 얼마를 보내야 하는지 계산하는 과정이 번거로울 수 있습니다.

PAYZLE은 이러한 문제를 해결하기 위해 공동 지출 관리와 정산 계산을 하나의 서비스로 제공합니다.

### 개발 목표

PAYZLE은 기능 구현뿐 아니라 **서비스가 실제 운영되는 과정에서 필요한 기술과 설계 원칙을 경험하는 것**을 목표로 합니다.

- 사용자가 이용할 수 있는 웹 서비스 개발
- 정산 알고리즘 및 데이터 모델링
- REST API 설계 및 백엔드 구현
- AWS 기반 서비스 인프라 구축
- Docker를 활용한 컨테이너 환경 구성
- CI/CD 자동화 및 서비스 모니터링
- 성능 측정과 장애 대응 경험
- 모놀리식 아키텍처와 MSA의 설계 및 운영 방식 비교

---

## 02. Key Features

### MVP

- [ ] 공동 지출 그룹 생성 및 관리
- [ ] 그룹 참여자 관리
- [ ] 지출 내역 등록, 수정 및 삭제
- [ ] 참여자별 분담 금액 계산
- [ ] 정산 결과 생성
- [ ] 지출 및 정산 내역 조회

### 향후 확장 기능

- [ ] 사용자 인증 및 권한 관리
- [ ] 실시간 데이터 동기화
- [ ] 영수증 이미지 업로드
- [ ] 영수증 정보 자동 추출
- [ ] 그룹별 지출 통계
- [ ] 정산 알림
- [ ] 고급 검색 및 필터링

기능의 우선순위와 구현 범위는 개발 과정에서 조정할 수 있습니다.

---

## 03. Tech Stack

아래 기술은 현재 검토 중인 기술 스택으로, 프로젝트 진행에 따라 변경될 수 있습니다.

| Category | Technology |
|---|---|
| Frontend | React, TypeScript, Vite |
| Backend | Java, Spring Boot |
| Database | PostgreSQL |
| Infrastructure | AWS EC2, S3, CloudFront |
| Container | Docker, Docker Compose |
| Reverse Proxy | Traefik / Nginx |
| CI/CD | GitHub Actions |
| Monitoring | Prometheus, Grafana |
| Message Broker | RabbitMQ (Planned) |
| Cache | Redis (Planned) |
| Search Engine | OpenSearch (Optional) |

### 기술 선정 방향

기술 스택은 단순히 많은 기술을 사용하는 것이 아니라, **각 기술을 도입하는 이유와 해결하려는 문제를 설명할 수 있도록** 선정합니다.

새로운 기술을 도입할 때는 다음 사항을 검토합니다.

1. 기존 구조에서 해결하기 어려운 문제가 있는가?
2. 도입으로 얻는 이점은 무엇인가?
3. 운영 복잡성과 비용은 얼마나 증가하는가?
4. 실제 성능이나 안정성 개선을 측정할 수 있는가?

---

## 04. System Architecture

PAYZLE은 초기 모놀리식 아키텍처를 기반으로 개발하고, 이후 서비스의 요구사항과 학습 목표에 따라 확장합니다.

### Phase 1 — Monolithic Architecture

초기에는 하나의 Spring Boot 애플리케이션에서 API와 비즈니스 로직을 처리합니다.

```text
                  Client
                     |
               React + Vite
                     |
              REST API (HTTPS)
                     |
                 Traefik
                     |
                Spring Boot
                     |
                PostgreSQL
```

**Infrastructure**
- React 정적 파일: S3 + CloudFront
- Backend: AWS EC2 + Docker
- Database: PostgreSQL
- Reverse Proxy: Traefik 또는 Nginx

### Phase 2 — DevOps & Observability

- GitHub Actions 기반 CI/CD
- Docker 이미지 빌드 및 배포 자동화
- Prometheus 메트릭 수집
- Grafana 모니터링 대시보드
- 애플리케이션 로그 및 장애 분석
- 데이터베이스 백업 및 복구 실습

### Phase 3 — Asynchronous Processing

영수증 처리와 같은 작업을 비동기적으로 실행하기 위한 구조를 실험합니다.

```text
             Spring Boot API
                   |
                RabbitMQ
                   |
                Worker
                   |
          Receipt Processing
                   |
               PostgreSQL
```

주요 학습 항목:
- 메시지 큐
- Producer / Consumer
- 비동기 작업 처리
- 재시도 및 실패 처리
- 멱등성 보장
- 데이터 일관성

### Phase 4 — Architecture Evolution

초기 모놀리식 구조를 분석하고, 필요에 따라 일부 기능을 독립 서비스로 분리하는 실험을 진행합니다.

**학습 목표**
- 모놀리식 아키텍처의 장단점 분석
- 서비스 경계 및 책임 분리
- 서비스 간 통신
- 분산 트랜잭션과 데이터 일관성
- 장애 전파 및 복구
- MSA 전환 전후 비교

MSA 전환은 확정된 필수 요구사항이 아니며, 아키텍처 비교와 학습을 위한 실험 단계입니다.

---

## 05. Database Design

**Database: PostgreSQL**

초기 데이터 모델은 다음 엔터티를 중심으로 설계할 예정입니다.

| Entity | Description |
|---|---|
| Users | 사용자 정보 |
| Groups | 공동 지출 그룹 |
| Group Members | 그룹 참여자 및 권한 |
| Expenses | 지출 내역 |
| Expense Shares | 개인별 분담 금액 |
| Settlements | 정산 내역 |

데이터베이스 설계에서는 관계형 데이터 모델링, 정규화, 외래 키, 트랜잭션과 데이터 무결성을 중점적으로 다룹니다.

ERD 및 상세 스키마는 설계 확정 후 추가합니다.

---

## 06. Settlement Algorithm

PAYZLE의 핵심 비즈니스 로직은 공동 지출에 대한 정산 계산입니다.

### 주요 목표

- 사용자별 총 지출 금액 계산
- 사용자별 실제 부담 금액 계산
- 사용자 간 채권·채무 관계 계산
- 유효한 정산 경로 생성
- 다양한 분담 조건 지원

### 고려 사항

- 정수 기반 금액 계산
- 반올림 및 잔여 금액 처리
- 중복 계산 방지
- 입력 데이터 검증
- 알고리즘의 시간 복잡도 분석
- 단위 테스트 및 예외 케이스 검증

정산 알고리즘의 설계 과정과 성능 분석은 별도 문서로 관리할 예정입니다.

---

## 07. Project Structure

예상 디렉터리 구조입니다.

```text
payzle/
├── frontend/
│   ├── src/
│   ├── public/
│   └── package.json
│
├── backend/
│   ├── src/
│   ├── build.gradle
│   └── Dockerfile
│
├── infrastructure/
│   ├── docker/
│   ├── proxy/
│   └── monitoring/
│
├── docs/
│   ├── architecture/
│   ├── database/
│   ├── api/
│   ├── decisions/
│   └── troubleshooting/
│
├── .github/
│   ├── workflows/
│   ├── ISSUE_TEMPLATE/
│   └── PULL_REQUEST_TEMPLATE.md
│
├── .env.example
├── .gitignore
├── CONTRIBUTING.md
└── README.md
```

---

## 08. Getting Started

개발 환경 구축 방법은 프로젝트 초기 설정 완료 후 작성할 예정입니다.

### Prerequisites

- Node.js
- Java JDK
- Docker / Docker Compose
- Git

### Environment Variables

환경 변수 예시는 `.env.example`에서 관리하며, 실제 인증 정보와 비밀 키는 저장소에 포함하지 않습니다.

---

## 09. Development Workflow

### Branch Strategy

```text
main
 └── develop
      ├── feat/feature-name
      ├── fix/issue-name
      └── refactor/module-name
```

- `main`: 안정적인 배포 버전
- `develop`: 기능 통합 브랜치
- `feat/*`: 신규 기능 개발
- `fix/*`: 오류 수정
- `refactor/*`: 구조 개선

### Commit Convention

| Type | Description |
|---|---|
| feat | 기능 추가 |
| fix | 버그 수정 |
| refactor | 코드 리팩터링 |
| docs | 문서 수정 |
| test | 테스트 코드 |
| chore | 설정 및 기타 작업 |
| ci | CI/CD 설정 |

### Collaboration Rules

- GitHub Issues 기반 작업 관리
- 기능별 브랜치에서 작업
- Pull Request 기반 병합
- 팀원 간 코드 리뷰
- 아키텍처 변경 사항 문서화
- 주요 기술적 의사결정 기록

---

## 10. Testing & CI/CD

### Testing Strategy

- 정산 알고리즘 단위 테스트
- REST API 통합 테스트
- 데이터베이스 트랜잭션 테스트
- 인증 및 권한 테스트
- 부하 테스트
- 장애 상황 및 복구 테스트

### CI/CD

GitHub Actions를 이용해 다음 과정을 자동화하는 것을 목표로 합니다.

1. 코드 변경 및 Pull Request
2. 정적 검사 및 테스트
3. 애플리케이션 빌드
4. Docker 이미지 생성
5. 배포
6. Health Check

---

## 11. Infrastructure & Monitoring

AWS 인프라 구축과 운영 과정을 기록합니다.

### Infrastructure

- EC2 인스턴스 구성
- VPC 및 Security Group 설정
- Docker 네트워크 관리
- HTTPS 및 Reverse Proxy
- IAM 권한 관리
- 데이터베이스 백업
- 비용 모니터링

### Observability

- CPU 및 메모리 사용량
- API 응답 시간
- 요청량 및 오류율
- 데이터베이스 상태
- 컨테이너 상태
- 장애 발생 기록

---

## 12. Engineering Log

프로젝트 개발 과정에서 발생한 문제와 해결 과정을 기록합니다.

| Date | Topic | Status |
|---|---|---|
| TBD | 초기 시스템 아키텍처 설계 | Planned |
| TBD | PostgreSQL 데이터 모델링 | Planned |
| TBD | Docker 기반 개발 환경 | Planned |
| TBD | AWS 배포 자동화 | Planned |
| TBD | 비동기 처리 아키텍처 | Planned |

주요 설계 결정은 ADR(Architecture Decision Record) 형식으로 문서화합니다.

---

## 13. Roadmap

### Phase 1 — MVP

- [ ] 요구사항 분석
- [ ] ERD 설계
- [ ] 프론트엔드 초기 구성
- [ ] Spring Boot 프로젝트 설정
- [ ] PostgreSQL 연동
- [ ] 핵심 REST API 개발
- [ ] 정산 알고리즘 구현
- [ ] MVP 배포

### Phase 2 — Infrastructure

- [ ] AWS 인프라 구성
- [ ] Docker 컨테이너화
- [ ] Reverse Proxy 및 HTTPS
- [ ] GitHub Actions CI/CD
- [ ] Prometheus / Grafana
- [ ] 백업 및 복구 실습

### Phase 3 — Advanced Backend

- [ ] RabbitMQ 기반 비동기 처리
- [ ] Worker 구현
- [ ] Redis 캐싱 실험
- [ ] 부하 테스트
- [ ] 성능 개선 전후 비교

### Phase 4 — Distributed Systems

- [ ] 모놀리식 구조 분석
- [ ] 서비스 분리 대상 선정
- [ ] 서비스 간 통신 구현
- [ ] 장애 및 데이터 일관성 실험
- [ ] 아키텍처 비교 보고서 작성

---

## 14. Contributors

현재 프로젝트 팀원을 모집하고 있습니다.

| Role | Member | Responsibilities |
|---|---|---|
| Project Lead | TBD | 프로젝트 관리 및 개발 |
| Developer | Recruiting | 서비스 개발 및 협업 |
| Developer | Recruiting | 서비스 개발 및 협업 |

팀 구성 이후 역할과 담당 범위를 협의할 예정입니다.

---

## 15. License

라이선스는 추후 결정할 예정입니다.

---

**PAYZLE — Pay + Puzzle**

*복잡한 정산을 간단하게, 페이즐.*
