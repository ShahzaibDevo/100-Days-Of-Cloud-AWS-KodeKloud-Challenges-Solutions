Here is a **complete GitHub README file** you can directly upload to your repo ✅
(Simple + Professional + Recruiter Friendly)

---

# 📘 Day 26 — EC2 Web Server Setup with Nginx (AWS)

## ☁️ Project Overview

In this lab, I configured an **AWS EC2 instance as a public web server** using **Nginx**.
The goal was to automatically deploy a ready-to-use web server during instance launch using **User Data automation**.

This simulates how DevOps engineers prepare infrastructure for real production deployments.

---

## 🧠 Learning Objectives

* Launch EC2 instance in AWS
* Use Ubuntu AMI
* Configure User Data automation
* Install & start Nginx automatically
* Configure Security Groups
* Make server accessible from Internet

---

## 🏗️ Architecture

```
User Browser
      ↓
Internet
      ↓
AWS Public IP
      ↓
Security Group (Port 80 Allowed)
      ↓
EC2 Instance (Ubuntu)
      ↓
Nginx Web Server
      ↓
Website Response
```

---

## ⚙️ Step 1 — Launch EC2 Instance

1. Open **AWS Console**
2. Go to **EC2 Dashboard**
3. Click **Launch Instance**

### Instance Configuration

| Setting       | Value            |
| ------------- | ---------------- |
| Instance Name | devops-ec2       |
| AMI           | Ubuntu Server    |
| Instance Type | t2.micro         |
| Key Pair      | Create or Select |
| Region        | us-east-1        |

---

## ⚙️ Step 2 — Configure Security Group

Allow HTTP traffic from the internet.

Add inbound rule:

| Type | Protocol | Port | Source    |
| ---- | -------- | ---- | --------- |
| HTTP | TCP      | 80   | 0.0.0.0/0 |

✅ This allows public access to the web server.

---

## ⚙️ Step 3 — Add User Data Script (Automation)

Scroll to **Advanced Details → User Data**

Paste:

```bash
#!/bin/bash
apt update -y
apt install nginx -y
systemctl start nginx
systemctl enable nginx
```

### What This Script Does

* Updates packages
* Installs Nginx
* Starts web server
* Enables auto-start on reboot

👉 Server becomes ready automatically after launch.

---

## ⚙️ Step 4 — Launch Instance

Click:

```
Launch Instance
```

Wait until instance state becomes:

```
Running
```

---

## 🌐 Step 5 — Access Website

1. Copy **Public IPv4 Address**
2. Open browser:

```
http://<Public-IP>
```

You should see:

```
Welcome to nginx!
```

✅ Web server successfully deployed.

---

## 🔍 Verification (DevOps Practice)

Connect via SSH:

```bash
ssh ubuntu@Public-IP
```

Check service:

```bash
sudo systemctl status nginx
```

Expected result:

```
Active: running
```

---

## 🧠 Theory Concepts

### ✅ What is EC2?

Amazon EC2 is a virtual machine running inside AWS cloud infrastructure.

---

### ✅ What is Nginx?

Nginx is a high-performance web server used for:

* Website hosting
* Reverse proxy
* Load balancing
* API gateway

---

### ✅ What is User Data?

User Data allows automation during server boot.

DevOps teams use it for:

* Application installation
* Configuration setup
* Automated deployments

---

### ✅ What is Security Group?

Security Group acts as a **virtual firewall** controlling traffic to EC2 instances.

---
