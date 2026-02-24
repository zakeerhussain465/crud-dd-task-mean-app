# 🚀 MEAN Stack CRUD Application with CI/CD & Nginx Reverse Proxy
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
Deployment Flow
Developer Push

        ↓
        
GitHub Actions

        ↓
        
Docker Build

        ↓
        
Docker Hub

        ↓
        
AWS EC2

        ↓
        
Docker Compose

        ↓
        
Nginx Reverse Proxy

        ↓
        
Backend API

        ↓
        
MongoDB
## 🧰 Tech Stack
Frontend

Angular 15

Backend

Node.js

Express.js

Database

MongoDB (Docker container)

DevOps & Cloud

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

mean-nginx

mean-frontend

mean-backend

mean-mongo

All services run inside the same Docker network.

## 🔁 CI/CD Configuration (GitHub Actions)
Workflow Location
.github/workflows/deploy.yml
Trigger

The pipeline runs automatically on every push to the main branch.

on:
  push:
    branches:
      - main
## 🔄 Pipeline Steps

Checkout repository

Login to Docker Hub

Build backend Docker image

Build frontend Docker image

Push images to Docker Hub

SSH into EC2

Pull latest images

Restart containers

Docker builds use --no-cache to prevent stale Angular builds.

## 📄 Complete CI/CD Workflow
name: CI/CD Deployment

on:
  push:
    branches:
      - main

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Login to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}

      - name: Build Backend Docker Image
        run: |
          docker build --no-cache -t ${{ secrets.DOCKERHUB_USERNAME }}/mean-backend:latest ./backend
          docker push ${{ secrets.DOCKERHUB_USERNAME }}/mean-backend:latest

      - name: Build Frontend Docker Image
        run: |
          docker build --no-cache -t ${{ secrets.DOCKERHUB_USERNAME }}/mean-frontend:latest ./frontend
          docker push ${{ secrets.DOCKERHUB_USERNAME }}/mean-frontend:latest

      - name: Deploy to EC2
        uses: appleboy/ssh-action@v1.0.3
        with:
          host: ${{ secrets.EC2_HOST }}
          username: ${{ secrets.EC2_USER }}
          key: ${{ secrets.EC2_SSH_KEY }}
          script: |
            cd crud-dd-task-mean-app
            docker compose pull
            docker compose down
            docker compose up -d
## 🔐 Required GitHub Secrets

Configured in:

Repository → Settings → Secrets and variables → Actions

DOCKERHUB_USERNAME

DOCKERHUB_TOKEN

EC2_HOST

EC2_USER

EC2_SSH_KEY

## ☁️ AWS EC2 Deployment
1️⃣ Launch EC2

Ubuntu 22.04

Open inbound ports: 22 (SSH), 80 (HTTP)

2️⃣ Connect to EC2
ssh -i your-key.pem ubuntu@<EC2-PUBLIC-IP>
3️⃣ Install Docker
sudo apt update
sudo apt install docker.io -y
sudo systemctl enable docker
sudo systemctl start docker
4️⃣ Install Docker Compose Plugin
sudo apt install docker-compose-plugin -y
docker compose version
5️⃣ Clone Repository
git clone <repo-url>
cd crud-dd-task-mean-app
6️⃣ Start Application
docker compose up -d

Application available at:

http://<EC2-PUBLIC-IP>
🌐 Nginx Reverse Proxy Configuration
Purpose

Serve Angular frontend

Forward /api requests to backend

Expose only port 80

Hide backend and MongoDB from public access

nginx.conf
events {}

http {
    upstream frontend {
        server mean-frontend:80;
    }

    upstream backend {
        server mean-backend:8080;
    }

    server {
        listen 80;

        location / {
            proxy_pass http://frontend;
        }

        location /api/ {
            proxy_pass http://backend;
        }
    }
}
Infrastructure Flow
Client → Port 80 → Nginx
                      ├── /        → Angular
                      └── /api/*   → Backend → MongoDB
## 🔍 Verification

✔ Application accessible via http://<EC2-IP>
✔ API accessible via /api/tutorials
✔ No direct access to port 8080
✔ All containers running via Docker Compose
✔ CI/CD automatically updates deployment

## 🧪 Run Locally
docker compose up --build

Access:

http://localhost
## 🔐 Security Notes

Secrets stored securely in GitHub Secrets

No credentials committed

Backend not publicly exposed

MongoDB runs inside Docker internal network

## 📄 Conclusion

This project demonstrates:

Full containerization

Automated CI/CD pipeline

Cloud deployment on AWS EC2

Reverse proxy configuration

Production-style Docker architecture

It reflects practical DevOps implementation and deployment best practices.

## images

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/15d665c7-9e78-4bf4-8ead-f1ea74d70d9d" />


<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/076ba8f4-9138-430c-93e0-90aa18f4a18f" />


<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/df63a731-551a-483e-9bf0-ec8b9333f115" />


<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/3cf568c0-bcb1-4111-9993-a06d035c75ed" />


<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4106a3aa-8aba-4cbd-8839-6652cfd4451b" />


<img width="1920" height="1080" alt="Screenshot 2026-02-24 171920" src="https://github.com/user-attachments/assets/4efd815f-5921-4181-8eab-206964623160" />


<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/341f939e-4deb-4b6b-8721-dbc883bfaa6e" />


<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b96138af-bd57-4ee7-b4a1-62428f81623b" />


<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d4a0040d-643b-4c00-9425-8a29f6bd67cb" />


<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/23f3ee12-9e3f-4b9d-8efe-74c4cd6130e8" />


<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a1dcbcb5-70f5-4948-8ba8-a30b9981782d" />


<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/726544d5-28a4-492e-8b85-430a0ef9ab0e" />


<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/3871ef4d-c8ce-455c-87fc-2afd0545da2e" />


<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2cccdc80-b69a-48da-8207-9d8c363fea26" />


<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f8200f9c-9871-4152-9469-61a68c9e5fc1" />



