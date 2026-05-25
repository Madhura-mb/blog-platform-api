# 📰 FastAPI Blog API

A production-grade Medium-style REST API built with **FastAPI**, **MongoDB**, and **Redis**, deployed on **AWS EC2** with a **GitHub Actions** CI/CD pipeline.

![CI](https://img.shields.io/github/actions/workflow/status/Madhura-mb/blog-platform-api/ci.yml?label=CI&style=flat-square)
![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=flat-square&logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-0.100+-009688?style=flat-square&logo=fastapi&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-Atlas-47A248?style=flat-square&logo=mongodb&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-7.0+-DC382D?style=flat-square&logo=redis&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?style=flat-square&logo=docker&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-EC2-FF9900?style=flat-square&logo=amazonaws&logoColor=white)

---

## 📌 Project Status

See the full **[ROADMAP.md](./ROADMAP.md)** for a breakdown of all 4 milestones, 15 phases, and their completion status.

| Milestone | Issues | Status |
|---|---|---|
| Core API | #1 – #6 | 🔲 Not started |
| Auth & Security | #7 – #9 | 🔲 Not started |
| Performance & Caching | #10 – #11 | 🔲 Not started |
| DevOps & Deployment | #12 – #15 | 🔲 Not started |

---

## ✨ Features

- **Posts & Comments** — Full CRUD with nested resource URLs
- **JWT Authentication** — Secure login, protected routes, token expiry
- **Role-Based Access Control** — Ownership checks + admin override
- **MongoDB Atlas** — Persistent storage with async Motor driver
- **Redis Caching** — Cache-aside pattern on post listings (60s TTL)
- **Trending Posts** — Redis sorted set tracks view counts in real time
- **Pagination & Filtering** — `?page`, `?limit`, `?tag` query parameters
- **Dockerised** — Full stack (app + MongoDB + Redis) via docker-compose
- **CI/CD** — GitHub Actions runs pytest on every push to `main`
- **Deployed** — Live on AWS EC2 behind Nginx reverse proxy

---

## 🏗️ Architecture

```
Client
  │
  ▼
Nginx (port 80)
  │
  ▼
FastAPI App (port 8000)
  ├── MongoDB Atlas  ← primary data store
  └── Redis          ← caching & trending leaderboard
```

**Folder structure:**

```
app/
├── main.py           # FastAPI instance, router registration
├── database.py       # MongoDB async connection
├── cache.py          # Redis connection & helpers
├── auth/
│   └── jwt.py        # Token creation & verification
├── models/
│   ├── post.py
│   ├── comment.py
│   └── user.py
└── routes/
    ├── posts.py
    ├── comments.py
    └── auth.py
tests/
├── test_health.py
├── test_posts.py
└── test_auth.py
```

---

## 🚀 Local Setup

### Prerequisites

- Python 3.10+
- Docker & Docker Compose
- A free [MongoDB Atlas](https://www.mongodb.com/atlas) account

### 1 · Clone the repo

```bash
git clone https://github.com/YOUR_USERNAME/YOUR_REPO.git
cd YOUR_REPO
```

### 2 · Create a virtual environment

```bash
python -m venv venv
source venv/bin/activate       # macOS / Linux
venv\Scripts\activate          # Windows
```

### 3 · Install dependencies

```bash
pip install -r requirements.txt
```

### 4 · Configure environment variables

```bash
cp .env.example .env
```

Edit `.env` and fill in your values:

```env
MONGO_URI=mongodb+srv://<user>:<password>@cluster.mongodb.net/blogdb
REDIS_URL=redis://localhost:6379
JWT_SECRET=your-long-random-secret-here
```

> ⚠️ Never commit `.env` to version control.

### 5 · Run with Docker Compose (recommended)

```bash
docker compose up --build
```

This starts the FastAPI app, MongoDB, and Redis together.

### 6 · Or run locally with uvicorn

```bash
uvicorn app.main:app --reload
```

### 7 · Open Swagger UI

```
http://127.0.0.1:8000/docs
```

---

## 🧪 Running Tests

```bash
pytest tests/
```

Tests use FastAPI's `TestClient` — no live server needed.

---

## 🔌 API Endpoints

### Health

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| GET | `/health` | — | Health check |

### Auth

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| POST | `/api/v1/auth/register` | — | Register a new user |
| POST | `/api/v1/auth/login` | — | Login and receive JWT |
| GET | `/api/v1/users/me` | ✅ JWT | Get current user profile |

### Posts

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| GET | `/api/v1/posts` | — | List posts (`?page`, `?limit`, `?tag`) |
| POST | `/api/v1/posts` | ✅ JWT | Create a post |
| GET | `/api/v1/posts/{id}` | — | Get a single post |
| PUT | `/api/v1/posts/{id}` | ✅ JWT | Update a post (owner/admin only) |
| DELETE | `/api/v1/posts/{id}` | ✅ JWT | Delete a post (owner/admin only) |
| GET | `/api/v1/posts/trending` | — | Top posts by view count (`?limit`) |

### Comments

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| GET | `/api/v1/posts/{id}/comments` | — | List comments on a post |
| POST | `/api/v1/posts/{id}/comments` | ✅ JWT | Add a comment |
| DELETE | `/api/v1/posts/{id}/comments/{cid}` | ✅ JWT | Delete a comment |

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| API Framework | FastAPI |
| Language | Python 3.10+ |
| Database | MongoDB Atlas (Motor async driver) |
| Cache | Redis 7 (aioredis) |
| Auth | JWT via python-jose, bcrypt via passlib |
| Containerisation | Docker + Docker Compose |
| CI/CD | GitHub Actions |
| Hosting | AWS EC2 (Ubuntu 22.04) |
| Reverse Proxy | Nginx |

---

## 🌐 Live Demo

> 🔗 **`http://YOUR_EC2_IP/docs`** ← Update this after Phase 14 deployment

---

## 🤝 Contributing

Please read [CONTRIBUTING.md](./CONTRIBUTING.md) for branching conventions and commit message guidelines before opening a PR.

---

## 📄 License

MIT
