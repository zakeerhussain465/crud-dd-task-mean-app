### MEAN Stack CRUD Application with CI/CD & Nginx Reverse Proxy
## 📌 Project Overview

This project demonstrates a full-stack CRUD application built using the MEAN stack (MongoDB, Express, Angular, Node.js).

The application is:

Fully containerized using Docker

Deployed on AWS EC2 (Ubuntu)

Configured with Nginx as a reverse proxy (Port 80)

Automated using GitHub Actions CI/CD pipeline

Integrated with Docker Hub for image management

This setup reflects a production-style DevOps workflow.

## 🏗 Architecture

GitHub → GitHub Actions → Docker Hub → AWS EC2 → Docker Compose → Nginx → Backend → MongoDB

Infrastructure Flow

Code pushed to GitHub

GitHub Actions builds Docker images

Images pushed to Docker Hub

EC2 pulls latest images

Containers restarted automatically

Nginx serves frontend and proxies API requests

## 🧰 Tech Stack

Frontend

Angular 15

Backend

Node.js

Express.js

Database

MongoDB (Docker container)

DevOps

Docker

Docker Compose

GitHub Actions

AWS EC2 (Ubuntu)

Nginx Reverse Proxy

## 📁 Project Structure
.
├── backend/
│   ├── Dockerfile
│   └── (Node.js source files)
├── frontend/
│   ├── Dockerfile
│   └── (Angular source files)
├── docker-compose.yml
├── nginx.conf
└── .github/workflows/deploy.yml
## 🐳 Docker Configuration
Backend Dockerfile

Uses Node base image

Installs dependencies

Exposes port 8080

Runs Express server

Frontend Dockerfile

Multi-stage build

Builds Angular production bundle

Serves static files using Nginx

docker-compose.yml

Defines services:

mean-frontend

mean-backend

mean-mongo

mean-nginx

All services run inside the same Docker network.

## 🔁 CI/CD Pipeline (GitHub Actions)

Location:

.github/workflows/deploy.yml
Pipeline Workflow

On every push to main:

Checkout repository

Login to Docker Hub

Build backend image

Build frontend image

Push images to Docker Hub

SSH into EC2

Pull latest images

Restart containers using Docker Compose

Docker builds use --no-cache to prevent stale Angular builds.

## ☁️ AWS EC2 Deployment
1️⃣ Launch EC2

Ubuntu Server

Open inbound port: 80 (HTTP)

Install Docker & Docker Compose

2️⃣ Clone Repository
git clone <your-repo-url>
cd crud-dd-task-mean-app
3️⃣ Start Containers
docker compose up -d

Application will be available at:

http://<EC2-PUBLIC-IP>
## 🌐 Nginx Reverse Proxy Configuration

Nginx serves the Angular frontend and proxies API requests.

Example configuration:

server {
    listen 80;

    location / {
        proxy_pass http://mean-frontend:80;
    }

    location /api {
        proxy_pass http://mean-backend:8080;
    }
}

This ensures:

Frontend accessible on port 80

Backend not exposed publicly

API accessed via /api/*

Example:

http://<EC2-IP>/api/tutorials
## 🔍 Verification

✔ Application accessible via port 80
✔ No direct use of port 8080 from browser
✔ Containers running via Docker Compose
✔ CI/CD automatically updates deployment
✔ Angular calls API using /api/tutorials
