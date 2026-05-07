# Let's Eat — Dockerize + CI/CD Refactoring

![CI](https://github.com/GWANG-MIN1/Lets-Eat-refactor/actions/workflows/ci.yml/badge.svg)

> React Native food-mate matching app — Dockerized with automated CI/CD pipeline

기존 팀 프로젝트([원본 레포](https://github.com/GWANG-MIN1/Let-s-Eat))에 **컨테이너화 및 자동화 배포 파이프라인**을 추가하는 리팩토링입니다.

---

## Infrastructure

| Component | Technology |
|-----------|------------|
| Containerization | Docker (multi-stage build) |
| Orchestration | Docker Compose |
| CI | GitHub Actions |
| Registry | Amazon ECR |
| Deployment | EC2 + SSH deploy |

---

## CI/CD Pipeline

```
main 브랜치 푸시
  └─ GitHub Actions
       ├─ 테스트 실행
       ├─ Docker 이미지 빌드
       └─ Amazon ECR 푸시
            └─ EC2 SSH 접속 → docker pull & run
```

`main` 브랜치에 푸시하면 테스트 → Docker 빌드 → ECR 푸시 → EC2 자동 배포까지 전 과정이 자동으로 실행됩니다.

---

## 🔄 작업 내용

| 항목 | 내용 |
|------|------|
| `Dockerfile` | 멀티스테이지 빌드로 백엔드 이미지 경량화 |
| `docker-compose.yml` | 백엔드 + DB 로컬 환경 통일 |
| `.github/workflows/ci.yml` | push/PR 시 자동 테스트 · 이미지 빌드 |
| `.github/workflows/cd.yml` | ECR 푸시 후 EC2 SSH 자동 배포 |

---

## 🛠 Tech Stack

**App**
- React Native (Expo)
- Node.js (Express) · WebSocket · SQLite · JWT

**DevOps**
- Docker · Docker Compose · GitHub Actions · Amazon ECR · Amazon EC2

---

