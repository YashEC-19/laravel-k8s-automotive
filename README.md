# 🚗 Automotive Laravel App - Kubernetes Deployment

A PHP Laravel application deployed on Kubernetes (Minikube) with Zero Downtime Rolling Updates on AWS EC2.

## 🏗️ Architecture

- **Cloud**: AWS EC2 (t3.small)
- **OS**: Ubuntu 24.04 LTS
- **Container**: Docker
- **Orchestration**: Kubernetes (Minikube)
- **App**: PHP Laravel 12
- **Web Server**: Apache
- **Database**: SQLite

## 🚀 Features

- ✅ Containerized Laravel application
- ✅ Kubernetes deployment with 3 replicas
- ✅ Zero Downtime Rolling Updates
- ✅ Health checks (Liveness & Readiness probes)
- ✅ Resource limits and requests
- ✅ Rollback capability
- ✅ Version control with Git

## 📋 Prerequisites

- AWS EC2 instance (t3.small)
- Docker
- Minikube
- kubectl
- PHP & Composer

## 🛠️ Setup Instructions

### 1. Install Docker
```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo usermod -aG docker $USER
newgrp docker
```

### 2. Install kubectl
```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```

### 3. Install Minikube
```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
minikube start --driver=docker --memory=1800 --cpus=2
```

### 4. Create Laravel Project
```bash
composer create-project laravel/laravel automotive-app
cd automotive-app
```

### 5. Build Docker Image
```bash
eval $(minikube docker-env)
docker build -t automotive-laravel:v1 .
```

### 6. Deploy to Kubernetes
```bash
kubectl apply -f k8s-deployment.yaml
kubectl apply -f k8s-service.yaml
```

### 7. Access Application
```bash
minikube ip
# Access via http://MINIKUBE-IP:30080
```

## 🔄 Rolling Updates

### Deploy New Version
```bash
# Build new image
docker build -t automotive-laravel:v2 .

# Update deployment
kubectl set image deployment/laravel-deployment laravel-app=automotive-laravel:v2

# Monitor rollout
kubectl rollout status deployment/laravel-deployment
```

### Rollback
```bash
# View history
kubectl rollout history deployment/laravel-deployment

# Rollback to previous version
kubectl rollout undo deployment/laravel-deployment

# Rollback to specific version
kubectl rollout undo deployment/laravel-deployment --to-revision=1
```

## 📊 Kubernetes Configuration

### Rolling Update Strategy
```yaml
strategy:
  type: RollingUpdate
  rollingUpdate:
    maxSurge: 1        # One extra pod during update
    maxUnavailable: 1  # One pod can be down during update
```

### Health Checks
```yaml
livenessProbe:   # Restarts unhealthy containers
  httpGet:
    path: /
    port: 80
readinessProbe:  # Removes from service if not ready
  httpGet:
    path: /
    port: 80
```

## 📝 Version History

| Version | Description | Banner Color |
|---------|-------------|--------------|
| v1.0 | Initial deployment | None |
| v2.0 | Rolling update demo | Purple |
| v3.0 | Rolling update demo | Green |

## 🧹 Cleanup
```bash
kubectl delete -f k8s-deployment.yaml
kubectl delete -f k8s-service.yaml
minikube stop
minikube delete
```

## 👨‍💻 Author
- **GitHub**: YashEC-19
