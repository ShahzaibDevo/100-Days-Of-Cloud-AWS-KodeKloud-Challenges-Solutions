# 100 Days of Cloud (AWS) — Day 28   |  Creating a Private Amazon ECR Repository & Pushing Docker Image



##  Lab Overview

In **Day 28**, the goal was to understand how container images are stored securely in AWS using **Amazon Elastic Container Registry (ECR)**.

This lab simulated a real DevOps workflow where a team prepares a containerized application and stores its Docker image in a private registry for deployment.

---

##  Objectives

* Create a **Private Amazon ECR Repository**
* Build a Docker image from an existing Dockerfile
* Authenticate Docker with AWS ECR
* Tag the Docker image properly
* Push the image to ECR with the `latest` tag
* Verify image upload

---

## Core Concept

**Amazon ECR** is AWS's private container registry used to store, manage, and deploy Docker images securely.

### DevOps Workflow

```
Application Code
        ↓
Dockerfile
        ↓
Docker Image Build
        ↓
Amazon ECR Repository
        ↓
Deployment (ECS / EKS / EC2)
```

---

##  Architecture Concept

* Developer builds Docker image
* Image stored inside AWS Private Registry (ECR)
* Deployment services pull image directly from ECR

---

##  Environment Details

| Component       | Value       |
| --------------- | ----------- |
| Region          | us-east-1   |
| Repository Name | xfusion-ecr |
| Repository Type | Private     |
| Encryption      | AES-256     |
| Image Tag       | latest      |

---

##  Application Source

Location on AWS Client:

```
/root/pyapp
```

Files:

```
app.py
Dockerfile
requirements.txt
```

---

## Implementation Steps

### 1️⃣ Create Private ECR Repository

```bash
aws ecr create-repository \
--repository-name xfusion-ecr \
--region us-east-1
```

---

### 2️⃣ Authenticate Docker to ECR

```bash
aws ecr get-login-password --region us-east-1 | \
docker login --username AWS \
--password-stdin <account-id>.dkr.ecr.us-east-1.amazonaws.com
```

---

### 3️⃣ Build Docker Image

```bash
docker build -t xfusion-ecr .
```

---

### 4️⃣ Tag Docker Image

```bash
docker tag xfusion-ecr:latest \
<account-id>.dkr.ecr.us-east-1.amazonaws.com/xfusion-ecr:latest
```

---

### 5️⃣ Push Image to ECR

```bash
docker push \
<account-id>.dkr.ecr.us-east-1.amazonaws.com/xfusion-ecr:latest
```

---

### 6️⃣ Verify Image Upload

```bash
aws ecr list-images --repository-name xfusion-ecr
```

Output confirms image successfully uploaded.

---

## Final Result

* Private ECR repository created
* Docker image built successfully
* Image pushed with `latest` tag
* Repository verified through AWS CLI

---

##  Key Learnings

* ECR works as **private Docker Hub inside AWS**
* Secure authentication between Docker and AWS
* Container images become deployment-ready assets
* Foundation for ECS, EKS, and Kubernetes deployments

---

##  Real DevOps Insight

Modern cloud deployments don't use raw servers — they use **container images stored in registries**.
ECR acts as the **single source of truth** for application images in AWS environments.

---
