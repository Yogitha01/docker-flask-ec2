# Dockerized Flask Application Deployment on AWS EC2

## Project Overview

This project demonstrates deployment of a Dockerized Flask web application on an AWS EC2 Ubuntu instance using Docker containers and Nginx reverse proxy.

The application is containerized using Docker, deployed on AWS EC2, and exposed externally through Nginx.

---

# Architecture

```text
User Request
     ↓
Nginx Reverse Proxy (Port 80)
     ↓
Docker Container (Port 5000)
     ↓
Flask Application
```

---

# Technologies Used

* AWS EC2
* Ubuntu 24.04
* Docker
* Python Flask
* Nginx
* Git & GitHub
* Linux

---

# Features

* Containerized Flask application using Docker
* Reverse proxy setup using Nginx
* Deployment on AWS EC2 Ubuntu instance
* Port mapping and external access configuration
* Docker container lifecycle management
* Application health endpoint
* Basic deployment troubleshooting using Docker and Linux tools

---

# Project Structure

```text
docker-flask-ec2/
├── app.py
├── Dockerfile
├── requirements.txt
├── README.md
└── screenshots/
    ├── app-running.png
    ├── docker-ps.png
    ├── nginx-working.png
```

---

# Application Code

## app.py

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def home():
    return "Dockerized App Running on EC2"

@app.route("/health")
def health():
    return {"status": "ok"}

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```

---

# Dockerfile

```dockerfile
FROM python:3.10-slim

WORKDIR /app

COPY . .

RUN pip install -r requirements.txt

EXPOSE 5000

CMD ["python", "app.py"]
```

---

# Setup Instructions

## 1. Clone Repository

```bash
git clone https://github.com/Yogitha01/docker-flask-ec2.git
cd docker-flask-ec2
```

---

## 2. Build Docker Image

```bash
docker build -t flask-app .
```

---

## 3. Run Docker Container

```bash
docker run -d -p 5000:5000 --name flask-app flask-app
```

---

## 4. Test Application

```bash
curl http://localhost:5000
```

Expected Output:

```text
Dockerized App Running on EC2
```

---

# Nginx Reverse Proxy Configuration

## Install Nginx

```bash
sudo apt install nginx -y
```

---

## Configure Reverse Proxy

Edit:

```bash
sudo nano /etc/nginx/sites-available/default
```

Replace configuration with:

```nginx
server {
    listen 80;

    location / {
        proxy_pass http://localhost:5000;
    }
}
```

Restart Nginx:

```bash
sudo systemctl restart nginx
```

---

# Access Application

```text
http://<EC2_PUBLIC_IP>
```

---

# Docker Commands Used

## Check Running Containers

```bash
docker ps
```

## View Container Logs

```bash
docker logs flask-app
```

## Stop Container

```bash
docker stop flask-app
```

## Remove Container

```bash
docker rm flask-app
```

---

# Troubleshooting

## Common Issues

### 1. Port 80 not accessible

* Verify EC2 Security Group allows HTTP traffic.

### 2. Container not running

Check:

```bash
docker ps -a
```

---

### 3. Application not responding

Check logs:

```bash
docker logs flask-app
```

---

### 4. Nginx configuration errors

Check:

```bash
sudo systemctl status nginx
```

---

# Screenshots

1. EC2 instance running
2. Docker container running (`docker ps`)
3. Flask app accessible in browser
4. Nginx reverse proxy working
5. Terminal showing successful deployment

# Key Learnings

* Docker containerization workflow
* Linux server management
* Reverse proxy setup using Nginx
* Deployment debugging using Docker logs
* AWS EC2 networking and security groups
* Basic production-style deployment architecture

---

# Future Improvements

* Add CI/CD pipeline using GitHub Actions or Jenkins
* Add Docker Compose
* Implement monitoring and logging
* Add rollback deployment strategy
* Deploy using Kubernetes or ECS

