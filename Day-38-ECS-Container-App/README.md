

#  Day 38: Deploying Containerized Applications with Amazon ECS (Fargate)

##  Project Overview

This project demonstrates how to deploy a **containerized application** on **Amazon Elastic Container Service (ECS)** using **AWS Fargate (serverless containers)**.
The lab covers end-to-end deployment including container setup, ECS task definition, networking, and public access via IP address.

---

#  Objective

* Deploy a Docker container on AWS without managing servers
* Use ECS Fargate for serverless container orchestration
* Expose application using a public IP
* Understand ECS networking, task definitions, and security groups

---

#  Theory (Important Concepts)

## 📦 Amazon ECS (Elastic Container Service)

A fully managed container orchestration service that helps run Docker containers at scale without managing EC2 instances.

---

##  AWS Fargate

A serverless compute engine for containers where:

* No EC2 management required
* AWS handles scaling, provisioning, and infrastructure
* You only define CPU & memory

---

##  Task Definition

A blueprint for running containers in ECS:

* Docker image location (ECR)
* CPU & memory allocation
* Port mappings
* Execution roles

---

##  Networking (awsvpc Mode)

Each task gets:

* Its own private IP
* Optional public IP for internet access
* Controlled via Security Groups

---

##  Security Groups

Virtual firewall controlling inbound/outbound traffic:

* Must allow HTTP/port access to reach application
* Example: Port 80 or 3000 open to `0.0.0.0/0`

---

## Amazon ECR

A private Docker registry used to store and manage container images.

---

#  Lab Architecture Flow

```
Docker Image → Amazon ECR → ECS Task Definition → Fargate Service → Public IP Access
```

---

#  Step-by-Step Lab Implementation

##  Step 1: Create Docker Image

Build application image locally:

```bash
docker build -t devops-app .
```

---

##  Step 2: Push Image to Amazon ECR

* Create ECR repository
* Authenticate Docker
* Push image:

```bash
docker tag devops-app:latest <account-id>.dkr.ecr.region.amazonaws.com/devops-ecr:latest
docker push <account-id>.dkr.ecr.region.amazonaws.com/devops-ecr:latest
```

---

## Step 3: Create ECS Cluster

* Select **Fargate cluster**
* Name: `devops-cluster`

---

## ✅ Step 4: Create Task Definition

Configure:

* Launch type: Fargate
* CPU: 1 vCPU
* Memory: 3 GB
* Container port: (e.g. 80 / 3000)
* Image: ECR image URL

---

## ✅ Step 5: Create ECS Service

* Attach task definition
* Choose cluster
* Enable public IP
* Select subnet and security group

---

## ✅ Step 6: Configure Security Group

Allow inbound traffic:

* Type: HTTP
* Port: 80 (or app port)
* Source: `0.0.0.0/0`

---

## ✅ Step 7: Access Application

After deployment, access via:

```
http://<public-ip>
```

Example:

```
http://18.212.220.132
```

---

# 📊 Result

* ECS Task: Running ✔
* Public IP assigned ✔
* Application accessible via browser ✔
* Container deployed successfully on AWS Fargate ✔

---

# 💡 Key Learnings

✔ How ECS simplifies container orchestration
✔ Importance of task definitions in deployment
✔ Role of security groups in application access
✔ Serverless benefits of AWS Fargate
✔ Real-world container deployment workflow

---

# 🚀 Conclusion

This lab successfully demonstrates deploying a fully containerized application on AWS using ECS Fargate, achieving a serverless, scalable, and production-ready architecture.

---

# 🏷️ Tags

#AWS #ECS #Fargate #DevOps #Docker #CloudComputing #ECR #KubernetesAlternative #100DaysOfCloud #CI_CD #LearningByDoing

---

If you want next level upgrade, I can also:
✅ Convert this into a **GitHub project with folder structure**
✅ Add **architecture diagram (image)**
✅ Or make it a **portfolio-ready DevOps project (very impressive for jobs)**
