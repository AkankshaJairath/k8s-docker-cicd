
# Kubernetes CI/CD Pipeline with Docker, GitHub Actions, and Datadog Monitoring

This project demonstrates a complete CI/CD pipeline for a containerized application using:

- **GitHub Actions** for automation
- **DockerHub** for image storage
- **k3s Kubernetes Cluster** for deployment
- **Datadog** for monitoring

---

## 📁 Project Structure

```
.
├── .github/workflows/         # GitHub Actions workflows
├── k8s/                       # Kubernetes manifests
│   ├── deployment.yaml
│   ├── service.yaml
├── Dockerfile                 # Docker image for the application
├── app/                       # Application source code
└── README.md
```

---

## ⚙️ CI/CD Pipeline Overview

1. **Code Push to GitHub**
2. **GitHub Actions Workflow Triggered**
3. **Build Docker Image**
4. **Push Image to DockerHub**
5. **Deploy to k3s Kubernetes Cluster**
6. **Expose App via NodePort/LoadBalancer**
7. **Monitor via Datadog**

---

## 🚀 Deployment Steps

### 1. Clone the Repository

```bash
git clone https://github.com/AkankshaJairath/k8s-docker-cicd.git
cd k8s-docker-cicd
```

### 2. Build & Push Docker Image (GitHub Actions)

The GitHub Actions workflow handles:
- Docker image build
- DockerHub login
- Image push

You can find it in `.github/workflows/ci-cd.yml`.

Set the following secrets in your GitHub repository:
- `DOCKERHUB_USERNAME`
- `DOCKERHUB_TOKEN`
- `KUBE_CONFIG` (base64 encoded kubeconfig for k3s)

### 3. Deploy to Kubernetes

```bash
kubectl apply -f k8s/
```

---

## 📦 Docker

```bash
docker build -t your-dockerhub-username/your-app-name .
docker push your-dockerhub-username/your-app-name
```

---


## 📷 Architecture Diagram

![Architecture](k8s-cicd-architecture.io)

---

---

## 🧰 Installation Guide

### 1. Install Docker (on Amazon Linux 2023)

```bash
sudo dnf update -y
sudo dnf install -y docker
sudo systemctl enable --now docker
sudo usermod -aG docker $USER
```

> 🔁 You may need to log out and log back in or run `newgrp docker` for your user to access Docker without `sudo`.

---

### 2. Install Kubernetes (k3s) on Amazon Linux 2023

Amazon Linux 2023 does not support installing kubeadm, kubelet, or kubectl via dnf/yum. The best way is to use **k3s**, which is lightweight and widely used for single-node or small clusters.

```bash
curl -sfL https://get.k3s.io | sh -
```

This installs the Kubernetes server, agent, and a compatible version of `kubectl`.

---

### 3. Use kubectl via k3s

```bash
sudo k3s kubectl get nodes
```

If you prefer to use the standard `kubectl` command:

```bash
mkdir -p ~/.kube
sudo cp /etc/rancher/k3s/k3s.yaml ~/.kube/config
sudo chown $USER:$USER ~/.kube/config

kubectl get nodes
```

---

### 4. (Optional) Install kubectl Standalone

Determine your architecture:

```bash
uname -m
```

- If you see `x86_64`, use `amd64`
- If you see `aarch64`, use `arm64`

#### For x86_64:

```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
```

#### For aarch64:

```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/arm64/kubectl"
```

Then:

```bash
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
kubectl version --client
```

---

