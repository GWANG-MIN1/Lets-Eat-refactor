# Let's Eat

![CI](https://github.com/GWANG-MIN1/Lets-Eat-refactor/actions/workflows/ci.yml/badge.svg)
![Deploy](https://github.com/GWANG-MIN1/Lets-Eat-refactor/actions/workflows/deploy.yml/badge.svg)

> React Native 밥친구 매칭 앱 — Dockerized + GitHub Actions CI/CD

기존 팀 프로젝트([원본 레포](https://github.com/GWANG-MIN1/Let-s-Eat))에 **컨테이너화 및 자동화 배포 파이프라인**을 추가하는 리팩토링입니다.

---

## 디렉토리 구조

```
Lets-Eat-refactor/
├── server/                     # Express + SQLite + WebSocket 백엔드
│   ├── middleware/
│   │   └── auth.js
│   ├── routes/
│   │   ├── auth.js
│   │   ├── rooms.js
│   │   ├── users.js
│   │   └── ratings.js
│   ├── db.js
│   ├── index.js
│   ├── package.json
│   └── Dockerfile
├── pages/                      # React Native 화면 컴포넌트
├── services/
│   └── api.js                  # Axios 클라이언트
├── assets/
├── App.js
├── docker-compose.yml          # 로컬 개발
├── docker-compose.prod.yml     # 프로덕션 배포
├── .env.example
├── .github/
│   └── workflows/
│       ├── ci.yml              # 테스트 + 이미지 빌드
│       └── deploy.yml          # ECR 푸시 + EC2 배포
└── README.md
```

---

## Tech Stack

| 영역 | 기술 |
|------|------|
| Client | React Native (Expo) · React Navigation |
| Server | Node.js · Express · WebSocket (ws) |
| Database | SQLite (better-sqlite3) |
| Auth | JWT · bcryptjs |
| Container | Docker (multi-stage build) · Docker Compose |
| CI/CD | GitHub Actions |
| Registry | Amazon ECR |
| Deployment | Amazon EC2 + SSH |

---

## CI/CD Pipeline

```
main 브랜치 push
  └─ ci.yml     : npm install → Docker 이미지 빌드
  └─ deploy.yml : ECR 푸시 → EC2 SSH → docker compose up
```

---

## 로컬 실행

```bash
cp .env.example .env
# .env 의 JWT_SECRET 값 수정

docker compose up --build
# → http://localhost:3000
```

---

## GitHub Secrets 설정

| Secret | 설명 |
|--------|------|
| `AWS_ACCESS_KEY_ID` | IAM 액세스 키 |
| `AWS_SECRET_ACCESS_KEY` | IAM 시크릿 키 |
| `AWS_REGION` | ECR 리전 (예: ap-northeast-2) |
| `ECR_REPOSITORY` | ECR 리포지토리 이름 |
| `EC2_HOST` | EC2 퍼블릭 IP |
| `EC2_USER` | EC2 SSH 유저 (예: ec2-user) |
| `EC2_SSH_KEY` | EC2 SSH 프라이빗 키 |
| `JWT_SECRET` | JWT 서명 키 |

---

## API 엔드포인트

| Method | Path | 설명 |
|--------|------|------|
| POST | `/api/auth/signup` | 회원가입 |
| POST | `/api/auth/login` | 로그인 |
| GET | `/api/rooms` | 방 목록 |
| POST | `/api/rooms` | 방 생성 |
| GET | `/api/rooms/:id` | 방 상세 |
| POST | `/api/rooms/:id/join` | 방 참여 |
| POST | `/api/rooms/:id/leave` | 방 나가기 |
| DELETE | `/api/rooms/:id` | 방 삭제 |
| GET | `/api/rooms/:id/messages` | 채팅 기록 |
| GET | `/api/users/:id` | 유저 프로필 |
| PUT | `/api/users/:id` | 유저 정보 수정 |
| POST | `/api/ratings` | 평점 등록 |
| GET | `/api/ratings/tags` | 평점 태그 목록 |
| GET | `/health` | 헬스체크 |
