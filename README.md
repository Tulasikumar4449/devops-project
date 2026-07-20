# Real-Time WebSocket Application Deployment using Docker, Nginx & GitHub Actions

## Project Overview

This project demonstrates the deployment of a real-time WebSocket application in a production-style environment using Docker, Docker Compose, Nginx Reverse Proxy, Azure Virtual Machine, and GitHub Actions CI/CD.

The objective of this assignment was to debug an existing staging application, containerize it, configure reverse proxy networking, automate deployment, and document the complete infrastructure.

---

## Technologies Used

- Azure Virtual Machine (Ubuntu)
- Docker
- Docker Compose
- Nginx
- GitHub Actions
- WebSocket
- Git
- Linux
- SSH

---

# Architecture Diagram

![Architecture](architecture.png)

```
                    User Browser
                         │
                         │
                  Public IP (Azure VM)
                         │
                  Nginx Reverse Proxy
                         │
          ┌──────────────┴──────────────┐
          │                             │
          │                             │
   Frontend Container           Backend Container
                                 │
                                 │
                           WebSocket Server
```

---

# Project Structure

```
devops-project/
│
├── app/
├── frontend/
├── Dockerfile
├── docker-compose.yml
├── nginx.conf
├── README.md
├── architecture.png
└── .github/
    └── workflows/
        └── deploy.yml
```

---

# Docker Container Setup

The application is containerized using Docker.

Containers include:

- Backend Application
- Frontend Application
- Nginx Reverse Proxy

Docker Compose is responsible for:

- Building Docker images
- Creating containers
- Managing Docker networking
- Starting all services together

Application can be started using:

```bash
docker compose up -d --build
```

---

# Docker Networking

Docker Compose automatically creates an isolated bridge network.

All containers communicate using Docker service names.

Example:

```
Nginx
   │
   │
backend:8000
```

This removes the need to hardcode container IP addresses.

---

# Nginx Reverse Proxy

Nginx acts as the single entry point to the application.

Responsibilities include:

- Routing frontend traffic
- Forwarding backend API requests
- Handling WebSocket Upgrade requests
- Reverse proxy configuration

Key configuration:

```nginx
proxy_http_version 1.1;
proxy_set_header Upgrade $http_upgrade;
proxy_set_header Connection "upgrade";
```

---

# WebSocket Flow

Communication Flow

```
Browser

↓

Nginx

↓

Backend

↓

Broadcast Message

↓

Connected Clients
```

Nginx forwards Upgrade requests allowing persistent WebSocket connections between clients and backend.

Multiple users can communicate in real-time.

---

# CI/CD Pipeline

Deployment is fully automated using GitHub Actions.

Pipeline Flow

```
Developer

↓

Git Push

↓

GitHub Actions

↓

SSH Azure VM

↓

Git Pull

↓

Docker Compose Build

↓

Restart Containers

↓

Deployment Completed
```

GitHub Actions performs:

- Connect to Azure VM using SSH
- Pull latest code
- Rebuild Docker containers
- Restart application automatically

---

# Deployment Steps

Clone repository

```bash
git clone https://github.com/Tulasikumar4449/devops-project.git
```

Move into project

```bash
cd devops-project
```

Start containers

```bash
docker compose up -d --build
```

Check running containers

```bash
docker ps
```

Access application

```
http://<Public-IP>
```

---

# Troubleshooting & Issues Fixed

## Issue 1

### Problem

Backend container was inaccessible.

### Root Cause

Incorrect Docker networking configuration.

### Resolution

Configured Docker Compose bridge network and connected services using service names.

---

## Issue 2

### Problem

Nginx returned Bad Gateway.

### Root Cause

Incorrect backend proxy configuration.

### Resolution

Updated proxy_pass configuration in nginx.conf.

---

## Issue 3

### Problem

WebSocket connection failed.

### Root Cause

Upgrade headers missing.

### Resolution

Added:

```nginx
proxy_http_version 1.1;
proxy_set_header Upgrade $http_upgrade;
proxy_set_header Connection "upgrade";
```

---

## Issue 4

### Problem

Application inaccessible externally.

### Root Cause

Azure Network Security Group ports not configured.

### Resolution

Opened required inbound ports and verified Docker port mappings.

---

## Issue 5

### Problem

GitHub Actions deployment failed.

### Root Cause

SSH authentication configuration.

### Resolution

Configured GitHub Secrets:

- SERVER_HOST
- SERVER_USER
- SSH_PRIVATE_KEY

---

## Issue 6

### Problem

Containers stopped after server reboot.

### Root Cause

Restart policy not configured.

### Resolution

Configured:

```yaml
restart: always
```

for each container in docker-compose.yml.

---

# Security Considerations

- SSH key authentication
- Docker container isolation
- Reverse proxy architecture
- GitHub Secrets for deployment credentials

---

# Future Improvements

- HTTPS using Let's Encrypt
- Redis Integration
- Monitoring with Grafana
- Netdata Dashboard
- Kubernetes Deployment
- Terraform Infrastructure as Code
- Load Balancer
- Auto Scaling

---

# Repository Contents

- Dockerfile
- docker-compose.yml
- nginx.conf
- GitHub Actions Workflow
- README.md
- Architecture Diagram

---

# Live Deployment

Azure Virtual Machine

```
Public IP:
<YOUR_PUBLIC_IP>
```

Repository

```
https://github.com/Tulasikumar4449/devops-project
```

---

# Author

Tulasi Kumar

DevOps Assignment
Docker • Nginx • Azure • GitHub Actions • WebSocket
