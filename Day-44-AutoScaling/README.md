# ☁️ AWS Auto Scaling + ALB Lab (Day 44)

## 📌 Project Overview

This lab demonstrates how to build a **highly available and scalable web application** using AWS services. The architecture automatically scales EC2 instances based on CPU load and distributes traffic using an Application Load Balancer (ALB).

---

## 🧠 Simple Definitions

### 🔹 Auto Scaling Group (ASG)

A service that automatically adds or removes EC2 instances based on demand to maintain performance and availability.

### 🔹 Application Load Balancer (ALB)

A service that distributes incoming traffic across multiple EC2 instances to ensure no single server is overloaded.

### 🔹 Launch Template

A blueprint that defines configuration for EC2 instances (AMI, instance type, security groups, and startup scripts).

### 🔹 Target Group

A group of EC2 instances registered with the ALB to receive traffic.

### 🔹 User Data Script

A startup script that runs automatically when an EC2 instance launches.

### 🔹 Nginx

A lightweight web server used to serve web pages and test load balancing.

---

## 🏗️ Architecture Flow

User → ALB → Target Group → EC2 Instances (ASG) → Nginx Web Server

---

## ⚙️ Step-by-Step Implementation

## 1️⃣ Create Security Group

* Allow HTTP traffic on port 80
* Source: 0.0.0.0/0

---

## 2️⃣ Create Launch Template

Name: `nautilus-launch-template`

### Configuration:

* AMI: Amazon Linux 2
* Instance Type: t2.micro
* Security Group: Allow HTTP (port 80)

### User Data Script:

```bash
#!/bin/bash
yum update -y
amazon-linux-extras install nginx1 -y
systemctl start nginx
systemctl enable nginx
```

---

## 3️⃣ Create Target Group

* Name: `nautilus-tg`
* Target type: Instances
* Protocol: HTTP
* Port: 80
* Health Check: `/`

---

## 4️⃣ Create Application Load Balancer (ALB)

* Name: `nautilus-alb`
* Scheme: Internet-facing
* Listener: HTTP (port 80)
* Attach Target Group: `nautilus-tg`

---

## 5️⃣ Create Auto Scaling Group (ASG)

Name: `nautilus-asg`

### Configuration:

* Launch Template: `nautilus-launch-template`
* Minimum capacity: 1
* Desired capacity: 1
* Maximum capacity: 2

### Scaling Policy:

* Metric: CPU Utilization
* Target: 50%

---

## 6️⃣ Verify Setup

* EC2 instance should be running
* Target group status: Healthy
* ALB DNS should open Nginx page

---

## 🚨 Common Issues & Fixes

### ❌ 502 Bad Gateway

* Nginx not running
* Instance unhealthy

### ❌ 503 Service Unavailable

* No healthy targets
* Health check failing

### ✅ Fix Commands (if SSH available)

```bash
sudo systemctl restart nginx
sudo systemctl status nginx
```

---

## 🎯 Final Result

A fully automated system where:

* Instances launch automatically
* Traffic is distributed via ALB
* System scales based on CPU usage
* Web server runs without manual setup

---

## 🚀 Key Learning

This lab demonstrates real-world cloud architecture principles:

* High availability
* Fault tolerance
* Auto scaling
* Load balancing

---

## 🏷️ Tags

#AWS #DevOps #CloudComputing #AutoScaling #ALB #EC2 #Nginx #Infrastructure #SRE #100DaysOfCloud
