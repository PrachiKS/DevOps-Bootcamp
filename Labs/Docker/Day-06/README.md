# Docker Day 06 — Containerizing a Real MERN Application

## Objective

In Day 06, I applied the Docker concepts learned in Days 01–05 to a real MERN stack application.

The project used for this task is **HireAI**.

The main goal was to containerize the frontend and backend, configure communication between them, connect the backend to MongoDB, troubleshoot Docker-related issues, and verify that the complete application works using Docker.

---

# 1. Application Overview

HireAI is a MERN-based application consisting of:

- React frontend
- Node.js backend
- Express.js
- MongoDB
- REST APIs
- Authentication
- AI features

The application was originally running without Docker.

In this task, the frontend and backend were containerized separately.

---

# 2. Final Architecture

The final local architecture is:

```text
                         Browser
                            |
                            |
                    http://localhost:3000
                            |
                            v
                 +----------------------+
                 | Frontend Container   |
                 | React + Nginx        |
                 | Container Port: 80   |
                 +----------------------+
                            |
                            |
                    API Requests
                            |
                            v
                    localhost:5000
                            |
                            v
                 +----------------------+
                 | Backend Container    |
                 | Node.js + Express    |
                 | Container Port: 5000 |
                 +----------------------+
                            |
                            |
                            v
                       MongoDB

| Service  | Host Port | Container Port |
| -------- | --------: | -------------: |
| Frontend |      3000 |             80 |
| Backend  |      5000 |           5000 |

HireAI/
│
├── backend/
│   ├── Dockerfile
│   ├── .dockerignore
│   └── ...
│
└── frontend/
    ├── Dockerfile
    ├── .dockerignore
    ├── .env.docker
    └── ...


**During Day 06, I used the following Docker commands:**

docker build
docker run
docker ps
docker logs
docker logs -f
docker exec
docker images
docker stop
docker rm
docker run --rm

**The workflow is:**

Application
     |
     v
Dockerfile
     |
     v
Docker Image
     |
     v
Docker Container
     |
     v
Configuration
     |
     v
Testing



