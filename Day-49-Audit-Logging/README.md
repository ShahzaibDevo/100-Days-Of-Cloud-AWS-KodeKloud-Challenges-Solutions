
# Day 49: Centralized Audit Logging with VPC Peering (AWS DevOps Project)

## 🚀 Overview

This project demonstrates a real-world **centralized logging architecture on AWS** using:

- VPC Peering
- EC2 (Private + Public architecture)
- IAM Roles
- S3 Storage
- Linux Cron Jobs
- SCP + AWS CLI automation

The goal is to securely collect logs from a **private EC2 instance**, transfer them through a **public EC2 (bastion/relay)**, and store them in an **S3 bucket**.

---

## 🏗️ Architecture

```

Private VPC (datacenter-priv-vpc)
│
│  VPC Peering
▼
Public VPC (datacenter-pub-vpc)
│
│  SCP (cron job)
▼
Public EC2 (datacenter-pub-ec2)
│
│  AWS CLI (cron job)
▼
S3 Bucket (datacenter-s3-logs-27578)

```

---

## ⚙️ Infrastructure Setup

### 🟢 Private VPC (Pre-Existing)
- VPC: `datacenter-priv-vpc`
- Subnet: `datacenter-priv-subnet`
- EC2: `datacenter-priv-ec2 (Ubuntu)`

---

### 🟢 Public VPC (Created)
- VPC: `datacenter-pub-vpc (10.20.0.0/16)`
- Subnet: `datacenter-pub-subnet (10.20.1.0/24)`
- Internet Gateway attached
- Route table configured for internet access

---

### 🟢 Public EC2 Instance
- Name: `datacenter-pub-ec2`
- Ubuntu AMI
- Same key pair as private EC2
- Public IP enabled

---

### 🟢 S3 Bucket
- Bucket: `datacenter-s3-logs-27578`
- Block public access enabled
- Stores centralized logs

---

### 🟢 IAM Role
- Role: `datacenter-s3-role`
- Policy: `AmazonS3FullAccess`
- Attached to public EC2 instance

---

## 🔗 VPC Peering Setup

Peering connection created:

```

datacenter-vpc-peering

````

Route tables updated:

- Private VPC → Public VPC CIDR via peering
- Public VPC → Private VPC CIDR via peering

---

## 🔐 SSH Access Flow

### Connect to Public EC2:
```bash
ssh -i datacenter-key.pem ubuntu@<public-ec2-ip>
````

### Connect to Private EC2 via Public:

```bash
ssh ubuntu@<private-ec2-ip>
```

or via jump host:

```bash
ssh -i datacenter-key.pem ubuntu@<private-ec2-ip> -J ubuntu@<public-ec2-ip>
```

---

## 📤 Log Transfer Automation

### Private EC2 → Public EC2 (SCP via cron)

```bash
* * * * * scp /var/log/boots.log ubuntu@10.20.1.86:~/boot/boots.log
```

---

### Public EC2 → S3 (AWS CLI via cron)

Install AWS CLI:

```bash
sudo apt install unzip -y
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o awscliv2.zip
unzip awscliv2.zip
sudo ./aws/install
```

Cron job:

```bash
* * * * * aws s3 cp ~/boot/boots.log s3://datacenter-s3-logs-27578/datacenter-priv-vpc/boot/boots.log
```

---

## 📦 Final S3 Structure

```
datacenter-s3-logs-27578/
└── datacenter-priv-vpc/
    └── boot/
        └── boots.log
```

---

## 🎯 Key Skills Demonstrated

* AWS VPC Peering
* EC2 Networking (Public & Private)
* IAM Role-based Access Control
* S3 Storage Management
* Linux Automation (Cron Jobs)
* Secure File Transfer (SCP)
* AWS CLI Automation

---
