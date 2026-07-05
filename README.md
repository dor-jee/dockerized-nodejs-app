# 🐳 Dockerize Node.js Application

Containerizing a Node.js application with a custom, production-minded `Dockerfile` — the first building block of a full CI/CD and container-orchestration pipeline.

![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat&logo=docker&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-339933?style=flat&logo=node.js&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=flat&logo=linux&logoColor=black)

---

## Project Overview

This project takes a standard Node.js application and packages it into a portable, reproducible Docker image. The goal is to eliminate "it works on my machine" problems by ensuring the app runs identically across local dev, CI pipelines, and production servers.

This is part of a broader DevOps learning path and lays the groundwork for later projects involving Docker Compose, private registries (Nexus, AWS ECR), CI/CD with Jenkins, and deployment to Kubernetes/EKS.

## What This Demonstrates

- Understanding of **Docker image layering** and how to keep images small and cache-efficient
- Ability to translate an application's runtime requirements into a working `Dockerfile`
- Comfort with core Docker CLI workflows (build, tag, run, debug, inspect logs)

## Tech Stack

| Tool | Purpose |
|------|---------|
| **Node.js** | Application runtime |
| **Docker** | Containerization |
| **Linux** | Base OS environment for the image |

## Project Structure

```
.
├── app/                # Node.js application source code
│   ├── package.json
│   └── index.js
├── Dockerfile
├── .dockerignore
└── README.md
```

## The Dockerfile

```dockerfile
# Use a lightweight, official Node.js base image
FROM node:20-alpine

# Set working directory inside the container
WORKDIR /app

# Copy dependency manifests first to leverage Docker layer caching
COPY app/package.json app/package-lock.json ./

# Install only production dependencies
RUN npm install 

# Copy the rest of the application code
COPY app .


# Document the port the app listens on
EXPOSE 3000

# Start the application
CMD ["npm", "start"]
```


- **`node:20-alpine`** — a minimal base image to reduce attack surface and image size.
- **Dependency install before code copy** — so Docker can cache the `npm install` layer and only rebuild it when `package.json` actually changes, speeding up rebuilds significantly.


## How to Run It

```bash
# 1. Build the image
docker build -t nodejs-app:latest .

# 2. Run the container
docker run -d -p 3000:3000 --name nodejs-app nodejs-app:latest

# 3. Verify it's running
curl http://localhost:3000

# 4. Check logs
docker logs nodejs-app

# 5. Stop and remove
docker stop nodejs-app && docker rm nodejs-app
```

## Verification / Testing

- Confirmed the container starts cleanly with `docker ps` and `docker logs`
- Verified the app responds correctly on the exposed port
- Checked final image size with `docker images` to confirm the Alpine base kept it lean



## 👤 Author

**Dorjee**
[GitHub](https://github.com/dor-jee) · [LinkedIn](https://linkedin.com/in/namgeldorjee)

