Internet
   ↓
Nginx (Port 80)
   ↓
Angular Frontend (Docker Container)
   ↓
Node.js Backend (Docker Container)
   ↓
MongoDB (Docker Container)
🛠 Technologies Used

Angular 15

Node.js + Express

MongoDB

Docker

Docker Compose

Nginx

GitHub Actions

AWS EC2 (Ubuntu 24.04)

🐳 Dockerization
Backend

Base Image: Node (Alpine)

Dependencies installed using npm ci

MongoDB connection uses environment variable:

MONGO_URL=mongodb://mongo:27017/db

Server runs on port 8080 (internally)

Location:

backend/Dockerfile
Frontend

Multi-stage Docker build:

Stage 1 – Build

Node image

Install dependencies

Angular production build

Stage 2 – Serve

Nginx image

Copy built files to /usr/share/nginx/html

Expose port 80

Location:

frontend/Dockerfile
🗄 Database Setup

MongoDB uses the official Docker image:

mongo:7

Configured in:

docker-compose.yml

MongoDB runs inside Docker network and is not publicly exposed.

📦 Docker Compose

Services:

mongo

backend

frontend

nginx

To run locally:

docker-compose up -d
🌐 Nginx Reverse Proxy

Configuration file:

nginx/nginx.conf

Routing rules:

/ → Angular frontend

/api → Node backend

Application is accessible via:

http://3.109.157.238
☁️ Cloud Deployment (AWS EC2)
Instance Details

Ubuntu 24.04

t2.micro

Region: ap-south-1

Public IP: 3.109.157.238

Security Group Configuration

Inbound Rules:

SSH (22) → 0.0.0.0/0

HTTP (80) → 0.0.0.0/0

Deployment Steps on EC2
git clone https://github.com/asmit990/dd-task.git
cd dd-task
docker-compose pull
docker-compose up -d

After opening port 80 in the security group, the application became publicly accessible.

🔁 CI/CD Pipeline

Workflow file:

.github/workflows/deploy.yml
Pipeline Flow (On Push to main)

Checkout repository

Setup Docker Buildx

Build multi-architecture images (linux/amd64, linux/arm64)

Push images to Docker Hub:

asmit990/backend-app:latest

asmit990/angular-15-crud:latest

SSH into EC2

Pull latest images

Restart containers using Docker Compose

Deployment is fully automated.

📦 Docker Hub Images

https://hub.docker.com/r/asmit990/backend-app

https://hub.docker.com/r/asmit990/angular-15-crud

Images support both:

amd64 (EC2)

arm64 (Apple Silicon)

📸 Screenshots & Proof
1️⃣ GitHub Actions – Successful CI/CD Execution

2️⃣ Docker Hub – Images Built & Pushed

3️⃣ EC2 Instance Running

4️⃣ Docker Containers Running on VM

Command used:

docker ps

5️⃣ Application Running via Public IP

Accessible at:

http://3.109.157.238

6️⃣ Nginx Reverse Proxy Configuration

🔒 Security Considerations

MongoDB is not publicly exposed.

Only required ports (22 and 80) are open.

Docker Hub credentials stored securely in GitHub Secrets.

SSH private key stored securely in GitHub Secrets.

Database connection uses environment variables.

✅ Final Result

The DD Task application is:

Fully containerized

Running on AWS EC2

Served through Nginx reverse proxy

Connected to MongoDB via Docker network

Automatically deployed through CI/CD

Publicly accessible via the EC2 public IP
