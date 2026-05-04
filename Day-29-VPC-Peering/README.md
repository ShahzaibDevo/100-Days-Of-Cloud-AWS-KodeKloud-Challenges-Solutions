
# 📘 Day 29 — Establishing Secure Communication Between Public and Private VPCs via VPC Peering

## ☁️ Project Overview

In this lab, I implemented **AWS VPC Peering** to enable secure communication between two isolated networks:

* A **Public VPC** containing an internet-accessible EC2 instance
* A **Private VPC** containing a private EC2 instance without public access

The objective was to allow the public EC2 instance to securely communicate with the private EC2 instance using **private networking**.

---

## 🎯 Learning Objectives

* Understand VPC isolation in AWS
* Configure VPC Peering
* Modify Route Tables for cross-VPC communication
* Configure Security Groups for secure traffic
* Verify connectivity between private resources

---

## 🧠 Architecture Concept

```
Public EC2 (Default VPC)
        │
        │  VPC Peering Connection
        │
Private EC2 (Private VPC)
```

✔ No internet exposure required
✔ Secure internal communication
✔ Real-world cloud networking design

---

## 🧱 Existing Infrastructure

### Public Environment

* EC2 Instance: **datacenter-public-ec2**
* VPC: Default VPC
* CIDR: `172.31.0.0/16`

### Private Environment

* VPC: **datacenter-private-vpc**
* CIDR: `10.1.0.0/16`
* Subnet: **datacenter-private-subnet**
* EC2 Instance: **datacenter-private-ec2**

---

# 🚀 Implementation Steps

---

## ✅ Step 1 — Create VPC Peering Connection

Navigate:

```
VPC → Peering Connections → Create Peering Connection
```

Configuration:

* Name: `datacenter-vpc-peering`
* Requester VPC: Default VPC
* Accepter VPC: datacenter-private-vpc

Create the connection.

---

## ✅ Step 2 — Accept Peering Request

After creation:

```
Actions → Accept Request
```

Verify status:

```
Active
```

---

## ✅ Step 3 — Update Route Tables

VPC Peering works only when routes are configured on **both sides**.

---

### 🔹 Default VPC Route Table

Add route:

| Destination | Target                 |
| ----------- | ---------------------- |
| 10.1.0.0/16 | VPC Peering Connection |

---

### 🔹 Private VPC Route Table

Add route:

| Destination   | Target                 |
| ------------- | ---------------------- |
| 172.31.0.0/16 | VPC Peering Connection |

---

## ✅ Step 4 — Configure Security Groups

### Private EC2 Security Group

Add inbound rules:

| Type            | Source        |
| --------------- | ------------- |
| All ICMP - IPv4 | 172.31.0.0/16 |
| SSH             | 172.31.0.0/16 |

This allows communication from the public VPC.

---

## ✅ Step 5 — Configure SSH Access

On AWS Client host:

```bash
cat /root/.ssh/id_rsa.pub
```

Copy the key.

SSH into public EC2:

```bash
ssh ec2-user@PUBLIC-IP
```

Add key:

```bash
mkdir -p ~/.ssh
nano ~/.ssh/authorized_keys
```

Paste public key and save.

---

## ✅ Step 6 — Test Connectivity

From **Public EC2**:

```bash
ping PRIVATE-EC2-PRIVATE-IP
```

Example:

```bash
ping 10.1.1.25
```

---

## ✅ Successful Output

```
64 bytes from 10.1.1.xx
```

This confirms secure communication between VPCs.

---

# 🔐 Key Concepts Learned

### 🟢 VPC

Logical isolated network inside AWS.

### 🟢 VPC Peering

Private connection enabling communication between two VPCs using private IP addresses.

### 🟢 Route Table

Defines where network traffic should be routed.

### 🟢 Security Group

Acts as a firewall controlling instance-level traffic.

---

# 🧩 Real-World Use Case

Organizations commonly use VPC Peering to:

* Separate production and development environments
* Connect backend databases securely
* Enable microservices communication
* Maintain network isolation with controlled access

---

# 🏁 Lab Validation Checklist

✅ Peering connection status = Active
✅ Routes added on both VPCs
✅ Security group allows ICMP traffic
✅ Public EC2 can ping Private EC2

---

# 📚 Key Takeaway

Cloud networking is not just launching servers —
it’s about **secure connectivity design**.

VPC Peering demonstrates how isolated environments can communicate safely without exposing resources to the public internet.

---
