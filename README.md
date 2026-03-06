# Nginx CI/CD GitOps Pipeline with Kubernetes, ArgoCD, and GitHub Actions

## Overview

This project demonstrates a  **Simple CI/CD GitOps pipeline** for deploying a containerized application to Kubernetes.

The application is a simple **Nginx web server** containerized with Docker and deployed to a **Kubernetes cluster running on Kind**.  
The pipeline uses **GitHub Actions for Continuous Integration (CI)** and **ArgoCD for Continuous Delivery (CD)**.

The goal of this project is to demonstrate how modern DevOps tools can be combined to automate application deployment.

---

## Architecture

```
Developer
   │
   ▼
GitHub Repository
   │
   ▼
GitHub Actions (CI)
   │
   ├── Build Docker Image
   └── Push Image to DockerHub
   │
   ▼
ArgoCD (GitOps CD)
   │
   ▼
Kubernetes Cluster (Kind)
   │
   ▼
Running Nginx Application
```

---

## Technologies Used

- **Docker** – Containerization
- **Kubernetes** – Container orchestration
- **Kind** – Local Kubernetes cluster
- **ArgoCD** – GitOps continuous delivery
- **GitHub Actions** – CI pipeline
- **DockerHub** – Container registry
- **Nginx** – Sample web application

---

## Repository Structure

```
nginx-argocd-demo
│
├── Dockerfile
├── index.html
│
├── k8s
│   ├── deployment.yaml
│   └── service.yaml
│
└── .github
    └── workflows
        └── docker-build.yml
```

---

## How the CI/CD Pipeline Works

1. Developer pushes code to GitHub.
2. GitHub Actions triggers the CI pipeline.
3. The pipeline builds the Docker image.
4. The image is pushed to DockerHub.
5. ArgoCD monitors the repository for changes.
6. ArgoCD synchronizes the Kubernetes cluster with the repository.
7. Kubernetes pulls the latest container image and deploys the application.

---

## Prerequisites

Make sure the following tools are installed:

- Docker
- kubectl
- Kind
- Git
- ArgoCD CLI (optional)

---

## Setup Instructions

### 1. Create a Kind Cluster

```bash
kind create cluster --name argocd-cicd
```

---

### 2. Install ArgoCD

```bash
kubectl create namespace argocd

kubectl apply -n argocd \
-f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

---

### 3. Access ArgoCD UI

```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

Open in browser:

```
https://localhost:8080
```

Get the admin password:

```bash
kubectl get secret argocd-initial-admin-secret -n argocd \
-o jsonpath="{.data.password}" | base64 -d
```

---

### 4. Deploy the Application with ArgoCD

Create a new application in the **ArgoCD UI** and connect it to the GitHub repository.

Use the `k8s/` directory as the application path.

ArgoCD will automatically deploy the Kubernetes manifests.

---

### 5. Access the Application

Forward the Kubernetes service port:

```bash
kubectl port-forward svc/nginx-cicd-service 8080:80
```

Open the application in your browser:

```
http://localhost:8080
```

---

## Docker Image

Example container image used in this project:

```
shauyeb009/nginx-cicd-demo
```

---

## CI Workflow

The CI pipeline is defined in:

```
.github/workflows/docker-build.yml
```

This workflow automatically:

- Builds the Docker image
- Pushes the image to DockerHub

---

## DevOps Concepts Demonstrated

- CI/CD pipeline automation
- GitOps workflow
- Kubernetes deployments
- Docker containerization
- Continuous delivery with ArgoCD

---

## Future Improvements

- Helm chart deployment
- Ingress controller configuration
- Automatic image updates using ArgoCD Image Updater
- Monitoring with Prometheus and Grafana
- Multi-environment deployments (dev/staging/prod)

---

## Author

**Shauyeb**

DevOps & Cloud Enthusiast
