Here is a **clean professional GitHub README** for your **Day 27 Lab** with:

✅ Step-by-step guide
✅ Simple definitions
✅ Architecture explanation
✅ Commands
✅ Real DevOps concepts

You can directly upload this to GitHub.

---

# ☁️ 100 Days of Cloud (AWS) — Day 27

# Configuring a Public VPC with an EC2 Instance for Internet Access

---

## 📌 Lab Objective

The goal of this lab was to build a **public AWS network infrastructure** where an EC2 instance is accessible from the internet using SSH.

This setup represents the **foundation of real-world cloud environments** used by DevOps teams.

---

# 🧠 Key Concepts (Simple Definitions)

## 🌐 VPC (Virtual Private Cloud)

A **VPC** is a private network inside AWS where you launch and manage cloud resources securely.

👉 Like your company's private data center in the cloud.

---

## 🧩 Subnet

A **Subnet** divides a VPC into smaller networks.

* Public Subnet → Internet accessible
* Private Subnet → Internal only

---

## 🌍 Public Subnet

A subnet that allows resources to receive **public IP addresses** and communicate with the internet.

---

## 🚪 Internet Gateway (IGW)

An **Internet Gateway** connects the VPC to the public internet.

Without IGW → No internet access.

---

## 🔐 Security Group

A virtual firewall controlling inbound and outbound traffic.

Example:

* Allow SSH → Port 22
* Allow HTTP → Port 80

---

## 🖥️ EC2 Instance

A virtual server running in AWS used to host applications.

---

## 🔑 Key Pair

Secure authentication method used instead of passwords for SSH login.

---

# 🏗️ Architecture Overview

```
Your Laptop
     ↓
Internet
     ↓
Internet Gateway
     ↓
Public VPC
     ↓
Public Subnet
     ↓
EC2 Instance (Public IP Enabled)
```

---

# 🛠️ Lab Implementation — Step by Step

---

## ✅ Step 1: Create Public VPC

1. Open AWS Console → VPC
2. Click **Create VPC**

Configuration:

* Name: `nautilus-pub-vpc`
* IPv4 CIDR: `10.0.0.0/16`

Create VPC.

---

## ✅ Step 2: Create Public Subnet

1. Go to **Subnets**
2. Click **Create Subnet**

Configuration:

* Name: `nautilus-pub-subnet`
* VPC: `nautilus-pub-vpc`
* CIDR: `10.0.1.0/24`
* AZ: us-east-1a

Enable:

✅ Auto-assign Public IP

---

## ✅ Step 3: Create Internet Gateway

1. Open **Internet Gateway**
2. Create:

Name:

```
nautilus-igw
```

Attach to:

```
nautilus-pub-vpc
```

---

## ✅ Step 4: Configure Route Table

1. Open **Route Tables**
2. Select VPC Route Table
3. Add Route:

```
Destination: 0.0.0.0/0
Target: Internet Gateway
```

Associate route table with:

```
nautilus-pub-subnet
```

👉 This makes subnet public.

---

## ✅ Step 5: Launch EC2 Instance

Open EC2 → Launch Instance.

Configuration:

* Name: `nautilus-pub-ec2`
* AMI: Ubuntu
* Instance Type: t2.micro
* VPC: `nautilus-pub-vpc`
* Subnet: `nautilus-pub-subnet`
* Auto Public IP: Enabled

---

## ✅ Step 6: Create Security Group

Security Group Name:

```
nautilus-pub-sg
```

Inbound Rule:

| Type | Port | Source    |
| ---- | ---- | --------- |
| SSH  | 22   | 0.0.0.0/0 |

---

## ✅ Step 7: Create / Select Key Pair

Create:

```
sahhzh_n.pem
```

Download key file.

---

## ✅ Step 8: Connect via SSH

Move to key location:

```bash
cd Pictures
```

Set permission:

```bash
chmod 400 sahhzh_n.pem
```

Connect:

```bash
ssh -i sahhzh_n.pem ubuntu@<PUBLIC-IP>
```

---

## ✅ Successful Output

```
Welcome to Ubuntu LTS
ubuntu@ip-10-0-1-230:~$
```

✔ Instance reachable from internet.
