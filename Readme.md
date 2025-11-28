# 🌐 MEAN DevOps Task – Full Deployment with Docker, Nginx & GitHub Actions CI/CD

This project is a fully containerized **MEAN (MongoDB, Express, Angular, Node.js)** application deployed on an **Ubuntu cloud VM** using:

- Docker & Docker Compose  
- Nginx Reverse Proxy  
- GitHub Actions CI/CD  
- Docker Hub  
- AWS EC2 (Ubuntu 22.04)

The project includes automatic builds, image pushes, and server redeployment whenever new code is pushed to GitHub.

---

## 📁 Project Structure

```
crud-dd-task-mean-app/
├── backend/              # Node.js + Express API (REST)
│   └── Dockerfile
├── frontend/             # Angular 15 app served with Nginx
│   └── Dockerfile
├── nginx/                # Reverse proxy configuration
│   └── default.conf
├── docker-compose.yml    # Production multi-container setup
└── .github/workflows/    # CI/CD pipeline
    └── deploy.yml
```



---

# 🐳 Docker Setup

## Backend
- Node.js + Express
- Exposes port **8080**
- Connects to MongoDB through environment variables

## Frontend
- Angular 15
- Built in production mode
- Served using Nginx (port **80**)

## Nginx Reverse Proxy
Handles two functions:

1. Serves Angular frontend  
2. Routes API requests:/api → backend:8080


## MongoDB
Official `mongo:6` Docker image with persistent volume:


---

# 🚀 Deployment on Ubuntu VM

### 1️⃣ Launch EC2 Instance (AWS)
- AMI: **Ubuntu Server 22.04 LTS**
- Type: **t2.micro**
- Open inbound ports:
  - **22 (SSH)**
  - **80 (HTTP)**

### 2️⃣ SSH into server
ssh -i "your-key.pem" ubuntu@<public-ip

# Install Docker & Compose
sudo apt update -y
sudo apt install -y ca-certificates curl gnupg git
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo \
"deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
https://download.docker.com/linux/ubuntu \
$(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update -y
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin


# Clone project
sudo mkdir -p /opt/mean-app
sudo chown $USER:$USER /opt/mean-app
cd /opt/mean-app
git clone https://github.com/<your-username>/crud-dd-task-mean-app.git .

# Deploy using Docker Compose
docker compose pull
docker compose up -d

# Access Application
http://13.60.34.15/

### 🔄 CI/CD Pipeline (GitHub Actions)
The entire project is automated using GitHub Actions.

Pipeline steps:
Triggered when code is pushed to main branch

Builds backend & frontend Docker images

Pushes images to Docker Hub

SSH into the VM

Pulls latest images

Restarts containers automatically

Workflow File:
.github/workflows/deploy.yml

🔐 Required GitHub Secrets
Secret Name	Description
DOCKERHUB_USERNAME	Docker Hub username
DOCKERHUB_TOKEN	Docker Hub Access Token (read/write)
SSH_HOST	13.60.34.15
SSH_USER	ubuntu
SSH_KEY	  .pem key

# 📸 Screenshots
### Github actions
![WhatsApp Image 2025-11-28 at 16 20 19_7a8f5119](https://github.com/user-attachments/assets/4ad3e4fa-d575-45a3-a0d7-f58cbfe8fca7)
### Docker Hub images and repositories
<img width="1169" height="791" alt="image" src="https://github.com/user-attachments/assets/c4ed389c-ecbe-4377-9418-3d41696cd08e" />
<img width="1090" height="701" alt="image" src="https://github.com/user-attachments/assets/184670d4-a0bf-4da9-8ed6-e3c68eebf2bd" />
###  Running EC2 instance
<img width="1909" height="829" alt="image" src="https://github.com/user-attachments/assets/7ddd9211-ea32-4e8b-a6d8-19f18d6105e0" />
###  Deployment inside VM
<img width="1859" height="799" alt="image" src="https://github.com/user-attachments/assets/73a0bc09-9203-42ca-b144-05ea1d03085d" />
###  Application running on public IP
<img width="1919" height="774" alt="image" src="https://github.com/user-attachments/assets/0f284681-bb13-48bc-ab68-b80d1bc44fc2" />
###  Github Secrets
<img width="1840" height="819" alt="image" src="https://github.com/user-attachments/assets/d49355b3-62ac-4e16-b15a-caec7bf13fa0" />







