# MEAN Stack CRUD Application with CI/CD & Nginx Reverse Proxy
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

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/15d665c7-9e78-4bf4-8ead-f1ea74d70d9d" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/076ba8f4-9138-430c-93e0-90aa18f4a18f" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/df63a731-551a-483e-9bf0-ec8b9333f115" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/3cf568c0-bcb1-4111-9993-a06d035c75ed" />



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

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/341f939e-4deb-4b6b-8721-dbc883bfaa6e" />


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

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d4a0040d-643b-4c00-9425-8a29f6bd67cb" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/23f3ee12-9e3f-4b9d-8efe-74c4cd6130e8" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a1dcbcb5-70f5-4948-8ba8-a30b9981782d" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/726544d5-28a4-492e-8b85-430a0ef9ab0e" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/3871ef4d-c8ce-455c-87fc-2afd0545da2e" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2cccdc80-b69a-48da-8207-9d8c363fea26" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f8200f9c-9871-4152-9469-61a68c9e5fc1" />


## 🔍 Verification

✔ Application accessible via port 80
✔ No direct use of port 8080 from browser
✔ Containers running via Docker Compose
✔ CI/CD automatically updates deployment
✔ Angular calls API using /api/tutorials

## 🧪 How to Run Locally
docker compose up --build

Access locally at:

http://localhost
## 🔐 Security Notes

Secrets stored in GitHub Secrets

No credentials committed

Backend not exposed directly to internet

MongoDB runs inside internal Docker network

## 📄 Conclusion

This project demonstrates:

Full containerization

Automated CI/CD pipeline

Cloud deployment on EC2

Reverse proxy configuration

Production-style Docker architecture

It reflects practical DevOps implementation and deployment best practices.
