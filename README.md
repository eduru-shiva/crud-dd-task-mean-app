🚀 MEAN Stack DevOps Assignment – Discover Dollar

A complete CI/CD + Docker + Nginx + AWS deployment of a full-stack MEAN (MongoDB, Express, Angular, Node.js) application.

📌 Project Overview

This project implements and deploys a CRUD application built using the MEAN stack:

MongoDB – Database

Express.js – Backend API

Angular 15 – Frontend UI

Node.js – Application runtime

The project was containerized using Docker, deployed on an AWS EC2 Ubuntu Server, automated using GitHub Actions, and exposed using an Nginx reverse proxy on port 80.


Project stracture

crud-dd-task-mean-app/
├── backend/            # Node.js + Express backend API
│   ├── Dockerfile
│   ├── app/
│   ├── package.json
│   └── server.js
│
├── frontend/           # Angular 15 frontend code
│   ├── Dockerfile
│   ├── angular.json
│   └── src/
│
├── nginx.conf          # Nginx reverse proxy configuration
├── docker-compose.yml  # Multi-container orchestration
└── .github/workflows/
       └── cicd.yml    # GitHub Actions CI/CD pipeline



1. Dockerization

🔹 Backend Dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package.json ./
RUN npm install
COPY . .
EXPOSE 8080
CMD ["node", "server.js"]


🔹 Frontend Dockerfile
FROM node:18-alpine as build
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm install
COPY . .
RUN npm run build --prod

FROM nginx:alpine
COPY --from=build /app/dist/angular-15-crud /usr/share/nginx/html


🗄️ 2. Docker Compose Deployment

docker-compose.yml
services:
  mongo:
    image: mongo:6
    container_name: mongo
    restart: always
    ports:
      - "27017:27017"
    volumes:
      - mongo-data:/data/db

  backend:
    build: ./backend
    container_name: backend
    restart: always
    environment:
      MONGO_URL: "mongodb://mongo:27017/dd_db"
    ports:
      - "8080:8080"
    depends_on:
      - mongo

  frontend:
    build: ./frontend
    container_name: frontend
    restart: always
    depends_on:
      - backend

  nginx:
    image: nginx:alpine
    container_name: nginx
    restart: always
    volumes:
      - ./nginx.conf:/etc/nginx/conf.d/default.conf
    ports:
      - "80:80"
    depends_on:
      - frontend
      - backend

volumes:
  mongo-data:


3. Nginx Reverse Proxy

nginx.conf
server {
    listen 80;

    location / {
        proxy_pass http://frontend:80;
    }

    location /api/ {
        proxy_pass http://backend:8080/;
    }
}


✔ Frontend available at → http://<EC2_PUBLIC_IP>/
✔ Backend API available at → http://<EC2_PUBLIC_IP>/api

☁️ 4. EC2 Deployment Steps
🔹 Launch Ubuntu EC2 Instance

Ubuntu 22.04

t2.medium (recommended)

Open ports: 22, 80, 8080, 27017

Install Docker + Docker Compose

sudo apt update
sudo apt install docker.io -y
sudo usermod -aG docker ubuntu
newgrp docker

Deploy containers

docker compose up -d --build


5. CI/CD Pipeline (GitHub Actions)

File: .github/workflows/ci-cd.yml

CI/CD Flow:

✔ Build Docker images
✔ Push to Docker Hub
✔ SSH into EC2
✔ Pull latest images
✔ Restart containers

🔹 GitHub Action Workflow (Full File)

name: CI/CD Pipeline

on:
  push:
    branches:
      - main

jobs:
  build-and-push:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Login to Docker Hub
        uses: docker/login-action@v2
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_PASSWORD }}

      - name: Build Backend Image
        run: docker build -t ${{ secrets.DOCKERHUB_USERNAME }}/mean-backend ./backend

      - name: Build Frontend Image
        run: docker build -t ${{ secrets.DOCKERHUB_USERNAME }}/mean-frontend ./frontend

      - name: Push Backend Image
        run: docker push ${{ secrets.DOCKERHUB_USERNAME }}/mean-backend

      - name: Push Frontend Image
        run: docker push ${{ secrets.DOCKERHUB_USERNAME }}/mean-frontend

  deploy-to-ec2:
    needs: build-and-push
    runs-on: ubuntu-latest
    steps:
      - name: SSH into EC2 & Deploy
        uses: appleboy/ssh-action@v0.1.10
        with:
          host: ${{ secrets.EC2_HOST }}
          username: ubuntu
          key: ${{ secrets.EC2_SSH_KEY }}
          script: |
            cd crud-dd-task-mean-app
            docker-compose pull
            docker-compose down
            docker-compose up -d


6. Screenshots to Include
