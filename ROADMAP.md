# 🗺️ Project Roadmap

> A Medium-style REST API built with FastAPI, MongoDB, Redis, and deployed on AWS EC2.
> Track progress across all 4 milestones and 15 issues below.

---

## Milestone 1 — Core API
> Issues #1–#6

Build the project foundation: environment setup, FastAPI app, routing, data models, MongoDB integration, and query features.

---

### Phase 1 · Project Scaffold & Dev Environment [`#1`](https://github.com/Madhura-mb/blog-platform-api/issues/1) `setup`

Set up the local development environment and initialize the project structure.

**Skills:** Python venv · Type hints · pip & requirements.txt · VS Code (Pylance, Python)

**Checklist:**
- [ ] Verify Python 3.10+ is installed
- [ ] Create and activate virtual environment
- [ ] Install FastAPI and uvicorn
- [ ] Generate `requirements.txt`
- [ ] Create folder structure: `app/`, `app/routes/`, `app/models/`
- [ ] Push initial commit to GitHub

> ⚠️ Never commit the `venv/` folder. Use conventional commits from day 1 (`feat:`, `docs:`, `fix:`).

---

### Phase 2 · Base FastAPI App & Health Check Endpoint [`#2`](https://github.com/Madhura-mb/blog-platform-api/issues/2) `setup` `feature`

Create the base FastAPI application with a health check endpoint and verify Swagger UI works.

**Skills:** FastAPI initialization · Path operations · uvicorn --reload · Swagger UI at `/docs`

**Checklist:**
- [ ] Create `app/main.py` with FastAPI instance
- [ ] Add title, description, and version to `FastAPI()` constructor
- [ ] Add `GET /health` endpoint returning status JSON
- [ ] Run server and verify response in browser
- [ ] Verify Swagger UI loads at `http://127.0.0.1:8000/docs`

**Tests:**
- `GET /health` → `{"status": "ok"}`
- `/docs` → endpoint listed in Swagger UI

---

### Phase 3 · Git Workflow & Project Standards [`#3`](https://github.com/Madhura-mb/blog-platform-api/issues/3) `setup` `documentation`

Establish a proper Git workflow and project standards for the rest of the project.

**Skills:** Git branching strategy · Conventional commits · Branch protection · PR workflow

**Checklist:**
- [ ] Enable branch protection on `main` (require PR before merging)
- [ ] Practice full workflow — feature branch → commit → PR → merge
- [ ] Add `CONTRIBUTING.md` with branching and commit conventions
- [ ] Verify `requirements.txt` is complete and up to date
- [ ] Add `ROADMAP.md` with phase tracking linked to issues
- [ ] Update `README.md` with link to `ROADMAP.md`

> ⚠️ From Day 4 onwards, never commit directly to `main`. Feature branches: `feature/short-description`. Bug fixes: `fix/short-description`.

---

### Phase 4 · Posts & Comments Routes with In-Memory Data [`#4`](https://github.com/Madhura-mb/blog-platform-api/issues/4) `feature` `routes`

Build posts and comments endpoints using an in-memory Python list before adding MongoDB.

**Skills:** Pydantic BaseModel · Path & query parameters · HTTP status codes · HTTPException · APIRouter · Nested resource URLs

**Checklist:**
- [ ] Create `app/models/post.py` — fields: `id`, `title`, `content`, `author`, `tags`, `created_at`
- [ ] Create `app/models/comment.py` — fields: `id`, `post_id`, `author`, `content`, `created_at`
- [ ] Create `app/routes/posts.py` with `APIRouter`
- [ ] `POST /posts` — create a post
- [ ] `GET /posts` — list all posts
- [ ] `GET /posts/{id}` — get single post (404 if not found)
- [ ] `PUT /posts/{id}` — update a post
- [ ] `DELETE /posts/{id}` — delete a post
- [ ] Create `app/routes/comments.py` with `APIRouter`
- [ ] `POST /posts/{post_id}/comments`
- [ ] `GET /posts/{post_id}/comments`
- [ ] `DELETE /posts/{post_id}/comments/{comment_id}`
- [ ] Register both routers in `app/main.py`

**Tests:**
- `GET /posts/{id}` with non-existent ID → 404
- `POST /posts` with missing fields → 422
- `POST` comment on non-existent post → 404

> 💡 Use `uuid4()` for IDs. In-memory data is replaced with MongoDB in Phase 5.

---

### Phase 5 · MongoDB Integration & Full CRUD [`#5`](https://github.com/Madhura-mb/blog-platform-api/issues/5) `feature` `database`

Replace in-memory storage with MongoDB Atlas using the Motor async driver.

**Skills:** MongoDB vs SQL · Atlas setup · Motor async driver · async/await · python-dotenv · ObjectId handling · `$set` operator

**Checklist:**
- [ ] Create a free MongoDB Atlas cluster
- [ ] Install `motor` and `python-dotenv`
- [ ] Create `app/database.py` with async connection logic
- [ ] Store `MONGO_URI` in `.env` (add `.env` to `.gitignore` immediately)
- [ ] Update `POST /posts` to insert into MongoDB
- [ ] Update `GET /posts` and `GET /posts/{id}` to query MongoDB
- [ ] Update `PUT /posts/{id}` using `$set` operator
- [ ] Update `DELETE /posts/{id}` — also delete all its comments
- [ ] Update all comments endpoints to use MongoDB
- [ ] Return 400 if ObjectId format is invalid
- [ ] Return 404 if post or comment does not exist

**Tests:**
- Create post via `/docs` → check it appears in Atlas UI
- Restart server → data persists
- Delete a post → its comments are also gone
- Pass random string as post ID → 400, not 500

> ⚠️ NEVER commit your `.env` file or MongoDB URI.

---

### Phase 6 · Pagination, Filtering & Indexing [`#6`](https://github.com/Madhura-mb/blog-platform-api/issues/6) `feature` `database`

Add pagination and tag filtering to the posts listing endpoint and create MongoDB indexes.

**Skills:** Offset pagination · MongoDB `$in`, `$eq` operators · Indexes and query performance · Clean query parameter design

**Checklist:**
- [ ] Add `?page=` and `?limit=` query params to `GET /posts`
- [ ] Add `?tag=` filter to `GET /posts`
- [ ] Return total count alongside paginated results
- [ ] Create MongoDB index on `created_at` field
- [ ] Create MongoDB index on `tags` field

**Tests:**
- Insert 10+ posts → `GET /posts?page=1&limit=5` → returns max 5
- `GET /posts?page=2&limit=5` → returns next 5
- `GET /posts?tag=python` → only tagged posts
- Response includes `total` count field

---

## Milestone 2 — Auth & Security
> Issues #7–#9

Add user accounts, secure password storage, JWT authentication, and ownership-based access control.

---

### Phase 7 · User Registration & Password Hashing [`#7`](https://github.com/Madhura-mb/blog-platform-api/issues/7) `feature` `auth`

Create the User model and registration endpoint with secure password hashing using bcrypt.

**Skills:** bcrypt hashing & salts · passlib · Email validation with Pydantic · Unique MongoDB indexes

**Checklist:**
- [ ] Create `app/models/user.py` — fields: `id`, `name`, `email`, `hashed_password`, `is_admin`, `created_at`
- [ ] Create `app/routes/auth.py` with `APIRouter`
- [ ] Install `passlib[bcrypt]`
- [ ] Implement `POST /auth/register`
- [ ] Hash password before saving to DB
- [ ] Return 400 if email is already registered
- [ ] Never return the password field in any response
- [ ] Add unique index on `email` field in MongoDB

**Tests:**
- Register a user → check Atlas, password should be a bcrypt hash
- Register same email twice → 400
- Response body must not contain the password field

---

### Phase 8 · JWT Login & Protected Routes [`#8`](https://github.com/Madhura-mb/blog-platform-api/issues/8) `feature` `auth`

Implement JWT-based login and protect post creation, update, and delete behind authentication.

**Skills:** JWT structure · python-jose · FastAPI `Depends()` · OAuth2PasswordBearer · Access token expiry

**Checklist:**
- [ ] Install `python-jose[cryptography]`
- [ ] Create `app/auth/jwt.py` with `create_access_token()` and `verify_token()`
- [ ] Store `JWT_SECRET` in `.env`
- [ ] Implement `POST /auth/login` → returns access token on valid credentials
- [ ] Return 401 on wrong email or password
- [ ] Create `get_current_user` dependency using `Depends()`
- [ ] Protect `POST /posts` — requires valid JWT
- [ ] Protect `PUT /posts/{id}` — requires valid JWT
- [ ] Protect `DELETE /posts/{id}` — requires valid JWT
- [ ] Protect `POST /posts/{post_id}/comments` — requires valid JWT

**Tests:**
- Login with valid credentials → token returned
- Login with wrong password → 401
- `POST /posts` without token → 401
- `POST /posts` with valid token → 201

> ⚠️ Set token expiry to 30 minutes. Store `JWT_SECRET` as a long random string — never hardcode it.

---

### Phase 9 · Ownership Checks & User Profile [`#9`](https://github.com/Madhura-mb/blog-platform-api/issues/9) `feature` `auth`

Ensure users can only edit or delete their own posts, add admin override, and expose a profile endpoint.

**Skills:** RBAC basics · Attaching user identity to resources · 403 Forbidden vs 401 Unauthorized

**Checklist:**
- [ ] Store `author_id` (user's MongoDB `_id`) on every post at creation
- [ ] `PUT /posts/{id}` → 403 if current user is not the author
- [ ] `DELETE /posts/{id}` → 403 if current user is not the author
- [ ] Admin users (`is_admin: true`) can edit and delete any post
- [ ] Implement `GET /users/me` → returns current user profile (no password)

**Tests:**
- Create post as User A, update as User B → 403
- Create post as User A, update as User A → 200
- Create post as User A, delete as admin → 200
- `GET /users/me` with valid token → profile without password field

---

## Milestone 3 — Performance & Caching
> Issues #10–#11

Introduce Redis to cache responses and track trending posts by view count.

---

### Phase 10 · Redis Connection & Post Listing Cache [`#10`](https://github.com/Madhura-mb/blog-platform-api/issues/10) `feature` `cache`

Connect Redis to FastAPI and cache the post listing endpoint with a TTL.

**Skills:** Redis data types · TTL and cache freshness · Cache-aside pattern · aioredis async client

**Checklist:**
- [ ] Run Redis locally: `docker run -d -p 6379:6379 redis`
- [ ] Install `aioredis`
- [ ] Create `app/cache.py` with Redis connection logic
- [ ] Store `REDIS_URL` in `.env`
- [ ] `GET /posts` → check Redis first, return if cache hit
- [ ] On cache miss → query MongoDB, store result in Redis with 60s TTL
- [ ] Invalidate cache when `POST /posts` creates a new post

**Tests:**
- `GET /posts` first call hits MongoDB, second call returns from cache
- Create a new post → `GET /posts` → new post appears (cache refreshed)

> 💡 Serialize response to JSON string before storing in Redis. Include pagination params in cache key when filtering is active.

---

### Phase 11 · Trending Posts with Redis Sorted Set [`#11`](https://github.com/Madhura-mb/blog-platform-api/issues/11) `feature` `cache`

Track post view counts using a Redis sorted set and expose a trending posts endpoint.

**Skills:** Redis sorted sets (ZADD, ZINCRBY, ZRANGE, ZREVRANGE) · Leaderboard pattern · Combining Redis with MongoDB lookups

**Checklist:**
- [ ] On every `GET /posts/{id}` → increment view count in Redis sorted set
- [ ] Implement `GET /posts/trending` → returns top 10 posts by view count
- [ ] Fetch full post details from MongoDB for each trending post ID
- [ ] Add `?limit=` query param to trending endpoint (default 10, max 50)

**Tests:**
- Hit `GET /posts/{id}` multiple times across different posts
- `GET /posts/trending` → most viewed post appears first
- `GET /posts/trending?limit=3` → returns only 3 posts

---

## Milestone 4 — DevOps & Deployment
> Issues #12–#15

Containerise the app, automate testing with CI, deploy to AWS EC2, and finalize documentation.

---

### Phase 12 · Dockerise the Application [`#12`](https://github.com/Madhura-mb/blog-platform-api/issues/12) `devops`

Write a Dockerfile and docker-compose to run the full stack locally with one command.

**Skills:** Dockerfile instructions · .dockerignore · docker-compose services & volumes · env_file in docker-compose

**Checklist:**
- [ ] Write `Dockerfile` for the FastAPI app
- [ ] Write `.dockerignore` (exclude `venv/`, `.env`, `__pycache__`, `.git`)
- [ ] Write `docker-compose.yml` with 3 services: `app`, `mongo`, `redis`
- [ ] Use named Docker volume for MongoDB data persistence
- [ ] `app` service `depends_on` mongo and redis
- [ ] Pass environment variables via `env_file: .env`
- [ ] Verify `docker compose up --build` runs the full stack
- [ ] Test all major endpoints through Docker

**Tests:**
- `docker compose up` → `GET /health` → `{"status": "ok"}`
- Create a post, stop containers, restart → data persists

> 💡 Use a named volume (not bind mount) for MongoDB data.

---

### Phase 13 · GitHub Actions CI Pipeline & Tests [`#13`](https://github.com/Madhura-mb/blog-platform-api/issues/13) `devops` `ci`

Write pytest tests for core endpoints and set up a GitHub Actions workflow.

**Skills:** pytest · FastAPI TestClient · GitHub Actions YAML · GitHub Secrets · README badges (shields.io)

**Checklist:**
- [ ] Install `pytest` and `httpx`
- [ ] Write `tests/test_health.py` — test `GET /health`
- [ ] Write `tests/test_posts.py` — create, get, 404, delete
- [ ] Write `tests/test_auth.py` — register, login, protected route access
- [ ] Create `.github/workflows/ci.yml`
- [ ] CI: checkout → install deps → run pytest on every push to `main`
- [ ] Store test MongoDB URI in GitHub Secrets
- [ ] Add passing CI badge to `README.md`

**Tests:**
- Push a commit → Actions tab → pipeline passes green
- Break a test intentionally → push → pipeline fails red

> ⚠️ Use a separate test DB URI in CI — never point tests at the production DB.

---

### Phase 14 · AWS EC2 Deployment [`#14`](https://github.com/Madhura-mb/blog-platform-api/issues/14) `devops` `deployment`

Deploy the Dockerised application to an AWS EC2 instance with Nginx as a reverse proxy.

**Skills:** AWS EC2 · SSH into remote Linux · Docker on Ubuntu · Nginx reverse proxy · Secure `.env` management on production

**Checklist:**
- [ ] Launch a `t2.micro` EC2 instance (Ubuntu 22.04, free tier)
- [ ] Configure security group — open ports 22, 80, 443
- [ ] SSH into instance using `.pem` key pair
- [ ] Install Docker and Docker Compose on the instance
- [ ] Clone repo onto EC2
- [ ] Create `.env` on server manually (never via git)
- [ ] Run `docker compose up -d`
- [ ] Install Nginx, configure to proxy port 80 → 8000
- [ ] Verify live public URL works

**Tests:**
- `http://YOUR_EC2_IP/health` from local browser → `{"status": "ok"}`
- `http://YOUR_EC2_IP/docs` → Swagger UI loads
- Create a post via live URL → verify it saves to Atlas

> ⚠️ Save your `.pem` key file safely — AWS will not give it to you again. Add the live URL to your README.

---

### Phase 15 · Final Polish & Documentation [`#15`](https://github.com/Madhura-mb/blog-platform-api/issues/15) `documentation`

Write the final README, add API versioning, customise Swagger UI, and prepare the resume summary.

**Skills:** OpenAPI customisation in FastAPI · API versioning (`/api/v1/` prefix) · Writing a project README

**Checklist:**
- [ ] Add `/api/v1/` prefix to all routes
- [ ] Add tag descriptions and summaries to all endpoints in Swagger UI
- [ ] Update `README.md` — architecture section, local setup guide, API endpoint table, tech stack badges, live URL
- [ ] Update `ROADMAP.md` — mark all phases and days complete
- [ ] Close all 15 GitHub Issues
- [ ] Write 3-bullet resume summary for this project

> 💡 Resume bullet formula: "Built X using Y that achieves Z"
> Example: *"Built a Medium-style REST API in FastAPI with JWT auth, MongoDB, and Redis caching, deployed on AWS EC2 with a GitHub Actions CI/CD pipeline."*

---

## Progress Summary

| Milestone | Issues | Status |
|---|---|---|
| Core API | #1 – #6 | 🔲 Not started |
| Auth & Security | #7 – #9 | 🔲 Not started |
| Performance & Caching | #10 – #11 | 🔲 Not started |
| DevOps & Deployment | #12 – #15 | 🔲 Not started |

> Update this table as you complete each milestone. Change 🔲 to 🟡 (in progress) or ✅ (complete).
