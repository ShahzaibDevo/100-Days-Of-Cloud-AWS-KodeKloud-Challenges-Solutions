Here is a **complete, clean, GitHub-ready README.md** for your **EKS Lab (Day 43)** with simple explanation + full steps + DevOps style formatting:

---

# ☸️ Day 43 — Amazon EKS Cluster Deployment (nautilus-eks)

🚀 This project demonstrates how to create and configure a **production-style Kubernetes cluster using Amazon EKS** with secure networking, IAM roles, and multi-AZ architecture.

---

# 📌 What is Amazon EKS?

**Amazon Elastic Kubernetes Service (EKS)** is a managed Kubernetes service by AWS that allows you to run Kubernetes without managing the control plane.

✔ AWS manages Kubernetes master nodes
✔ You manage worker nodes and workloads
✔ Highly scalable, secure, and production-ready

---

# 🎯 Lab Objective

Create a secure and scalable Kubernetes cluster with:

* Cluster Name: `nautilus-eks`
* Kubernetes Version: `1.30`
* Private API endpoint
* Default VPC
* Multi-AZ deployment (us-east-1a, 1b, 1c)
* IAM-based access control
* Auto Mode disabled

---

# 🏗️ Architecture Overview

```
User → AWS Console → EKS Cluster (Control Plane)
                          │
                          ├── Private API Endpoint
                          ├── IAM Role (eksClusterRole)
                          └── Worker Nodes (EC2 via Node Group)
```

---

# ⚙️ Step-by-Step Deployment Guide

## 🔹 Step 1: Login to AWS Console

* Open AWS EKS Service
* Select region: **us-east-1**

---

## 🔹 Step 2: Create Cluster

Go to:

👉 Amazon EKS → Create Cluster → Custom Configuration

---

## 🔹 Step 3: Cluster Configuration

Set:

* Cluster Name → `nautilus-eks`
* Kubernetes Version → `1.30`
* Cluster IAM Role → `eksClusterRole`
* Auto Mode → ❌ Disabled

---

## 🔹 Step 4: Networking Configuration

* VPC → Default VPC

* Subnets → Select:

  * us-east-1a
  * us-east-1b
  * us-east-1c

* Cluster Endpoint Access → **Private only**

---

## 🔹 Step 5: Add-ons (Default)

AWS automatically installs:

* CoreDNS
* kube-proxy
* VPC CNI
* Metrics Server

---

## 🔹 Step 6: IAM Roles

### Cluster Role:

* `eksClusterRole`

### Node Role:

* Created using **“Create recommended role”**

---

## 🔹 Step 7: Create Cluster

Click:

👉 **Create**

Wait 10–15 minutes until status becomes:

```
ACTIVE
```

---

# 🔐 IAM Roles Explained

## 🟢 Cluster Role (eksClusterRole)

Used by:

* EKS control plane
* Managing AWS resources

## 🔵 Node Role (Worker Nodes)

Used by:

* EC2 instances
* Running containers
* Pulling images from ECR

---

# ⚠️ Key Challenges Faced

* IAM `iam:PassRole` permission errors
* Confusion between Auto Mode vs Standard EKS
* Role selection mistakes
* Networking configuration understanding

---

# 💡 Key Learnings

✔ IAM is critical in Kubernetes on AWS
✔ Private EKS clusters improve security
✔ Multi-AZ improves high availability
✔ EKS simplifies Kubernetes management
✔ Role-based access is essential in DevOps

---

# 🚀 Result

✔ EKS cluster successfully created
✔ Secure private endpoint configured
✔ Multi-AZ architecture implemented
✔ IAM-based access control enabled
✔ Production-ready Kubernetes foundation ready

---

# 📌 Tech Stack

* AWS EKS
* Kubernetes (1.30)
* IAM (Identity & Access Management)
* VPC Networking
* EC2 (Node Groups)

---

# 🌱 Author

👤 DevOps Learning Journey — Day 43
🚀 Focus: AWS | Kubernetes | DevOps | Cloud Engineering

---

# 📎 License

This project is for learning purposes as part of a DevOps hands-on lab.

---

If you want, I can also create:

✅ GitHub repo folder structure
✅ kubectl deployment (nginx app)
✅ CI/CD pipeline on EKS
✅ Terraform version of this lab

Just tell me 👍
