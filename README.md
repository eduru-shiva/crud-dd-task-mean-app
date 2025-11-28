**Discover Dollar – DevOps Assignment**
Full-Stack MEAN Application – Containerization, Deployment, CI/CD & Nginx Reverse Proxy

This repository contains the complete implementation of the DevOps assignment provided by Discover Dollar.
The task includes containerizing a full-stack MEAN application, deploying it on an AWS EC2 Ubuntu VM, configuring CI/CD using GitHub Actions, and exposing the application using Nginx reverse proxy on port 80.

 **Project Live URL**

Frontend UI: http://51.20.181.28

Backend API: http://51.20.181.28/api

MongoDB: running via Docker container

** Project Structure**

crud-dd-task-mean-app/
│── backend/
│   ├── Dockerfile
│   ├── server.js
│   ├── app/
│   │   ├── config/db.config.js
│   │   ├── routes
│   │   ├── controllers
│   │   └── models
│── frontend/
│   ├── Dockerfile
│   ├── angular.json
│   ├── src/
│── nginx.conf
│── docker-compose.yml
│── .github/workflows/ci-cd.yml
│── README.md

** Task Requirements Completed**

✔ Setup GitHub Repository

Pushed full project source code

Added Dockerfiles, nginx.conf, and docker-compose.yml

Added GitHub Actions workflow for CI/CD

✔ Containerization & Deployment

Created Dockerfile for backend

Created Dockerfile for frontend

Built and pushed images to Docker Hub

edurushiva1418/mean-backend

edurushiva1418/mean-frontend

Used Docker Compose to deploy full MEAN stack on EC2

Exposed:

Backend → port 8080

Frontend → handled by Nginx

Entire application available on port 80

✔ MongoDB Setup

Used official MongoDB Docker image

Mapped volume mongo-data:/data/db

✔ CI/CD Pipeline

Implemented via GitHub Actions:

On every push to main:

Build backend & frontend Docker images

Push images to Docker Hub

SSH into EC2

Pull latest images

Restart Docker Compose services

✔ Nginx Reverse Proxy

Configured nginx.conf to:

Serve Angular frontend

Proxy /api requests to backend at port 8080

Expose the entire application on port 80

** Screenshots**

✔ Working Frontend UI
<img width="1162" alt="UI Screenshot" src="https://github.com/user-attachments/assets/d792dce3-9d48-45f4-b3f1-90ece1bf55ac" />
✔ Docker Images Build & Push
<img width="1619" alt="Docker Push" src="https://github.com/user-attachments/assets/f7225712-24d5-4a00-9335-474310747bc8" />
✔ Docker Compose Deployment
<img width="1503" alt="Docker Compose" src="https://github.com/user-attachments/assets/ec22dc64-efc8-4b1d-9826-b378e52db911" />
✔ CI/CD Workflow – Successful Execution
<img width="1907" alt="CI/CD Success" src="https://github.com/user-attachments/assets/524ffd38-f1ae-4ddb-a506-b5d3f9589de2" />
✔ Nginx & Infrastructure Setup
<img width="1900" alt="Nginx" src="https://github.com/user-attachments/assets/95dfe6f0-3135-43ac-91d6-75919d62b8cf" />
✔ Working Backend API Output
<img width="1882" alt="API Working" src="https://github.com/user-attachments/assets/6a44a20b-deb8-4630-aad4-e914f36776d5" />
⚙️ How to Deploy – Step-by-Step Guide
1️⃣ Clone the repository
git clone https://github.com/eduru-shiva/crud-dd-task-mean-app.git
cd crud-dd-task-mean-app

2️⃣ Build & Push Docker Images

Backend:

docker build -t edurushiva1418/mean-backend ./backend
docker push edurushiva1418/mean-backend


Frontend:

docker build -t edurushiva1418/mean-frontend ./frontend
docker push edurushiva1418/mean-frontend

3️⃣ Start Application on EC2
docker compose up -d

4️⃣ Access Application
http://<EC2_PUBLIC_IP>

⚙️ CI/CD – GitHub Actions Workflow
.github/workflows/ci-cd.yml


Includes:

Build backend image

Build frontend image

Push to Docker Hub

SSH into EC2

Pull latest images

Restart Docker Compose

 Nginx Reverse Proxy Configuration

nginx.conf:

server {
    listen 80;

    location / {
        root /usr/share/nginx/html;
        try_files $uri /index.html;
    }

    location /api/ {
        proxy_pass http://backend:8080/;
    }
}

 Testing the Application
Add tutorial via API
curl -X POST http://51.20.181.28/api/tutorials \
-H "Content-Type: application/json" \
-d '{"title":"Test","description":"Hello","published":true}'

Fetch tutorials
curl http://51.20.181.28/api/tutorials

🏁 Submission

✔ GitHub Repo:
https://github.com/eduru-shiva/crud-dd-task-mean-app

✔ EC2 Public IP:
51.20.181.28

✔ Docker Hub Images:

https://hub.docker.com/r/edurushiva1418/mean-backend

https://hub.docker.com/r/edurushiva1418/mean-frontend

** Completed Successfully**

This assignment covers:

Docker

Docker Compose

CI/CD (GitHub Actions)

Nginx Reverse Proxy

MEAN Stack Deployment

AWS EC2 infrastructure setup

<img width="1162" height="346" alt="Screenshot 2025-11-28 111903" src="https://github.com/user-attachments/assets/d792dce3-9d48-45f4-b3f1-90ece1bf55ac" />
<img width="1619" height="457" alt="Screenshot 2025-11-28 152215" src="https://github.com/user-attachments/assets/f7225712-24d5-4a00-9335-474310747bc8" />
<img width="1503" height="517" alt="Screenshot 2025-11-28 152307" src="https://github.com/user-attachments/assets/ec22dc64-efc8-4b1d-9826-b378e52db911" />
<img width="1907" height="663" alt="Screenshot 2025-11-28 152433" src="https://github.com/user-attachments/assets/524ffd38-f1ae-4ddb-a506-b5d3f9589de2" />
<img width="1900" height="790" alt="Screenshot 2025-11-28 152458" src="https://github.com/user-attachments/assets/95dfe6f0-3135-43ac-91d6-75919d62b8cf" />
<img width="1882" height="701" alt="Screenshot 2025-11-28 152533" src="https://github.com/user-attachments/assets/6a44a20b-deb8-4630-aad4-e914f36776d5" />
