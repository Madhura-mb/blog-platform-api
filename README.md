# Blog Platform API

A Medium-like blog backend built with FastAPI, MongoDB, Redis, Docker, and deployed on AWS EC2.

## Tech Stack
- **FastAPI** — REST API framework
- **MongoDB** — database for posts and comments
- **Redis** — caching for trending posts
- **Docker** — containerisation
- **GitHub Actions** — CI/CD pipeline
- **AWS EC2** — deployment

## Setup

```bash
git clone https://github.com/YOUR_USERNAME/blog-platform-api.git
cd blog-platform-api
python -m venv venv
venv\Scripts\activate
pip install -r requirements.txt
uvicorn app.main:app --reload
```

