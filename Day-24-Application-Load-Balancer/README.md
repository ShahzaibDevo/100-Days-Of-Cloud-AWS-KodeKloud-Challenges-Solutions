
# ☁️ Day 24: Application Load Balancer (ALB) with EC2 + Nginx

##  Objective

Deploy an **Application Load Balancer (ALB)** in front of an EC2 instance running **Nginx**, and configure secure and scalable traffic routing.

---

# 📘 Core Concepts

## ⚖️ What is an Application Load Balancer (ALB)?

An **Application Load Balancer (ALB)** is a Layer 7 (HTTP/HTTPS) load balancing service in AWS.

### Key Features:
- Distributes incoming traffic across multiple targets
- Works at **application layer (HTTP/HTTPS)**
- Supports path-based routing
- Provides high availability and scalability

### Simple Definition:
> ALB is a traffic manager that forwards user requests to healthy backend servers.

---

##  What is a Target Group?

A **Target Group** is a logical group of backend resources (usually EC2 instances) that receive traffic from the ALB.

### Key Features:
- Contains EC2 instances or IP addresses
- Defines **port and protocol (e.g., HTTP:80)**
- Performs health checks
- Ensures traffic goes only to healthy instances

### Simple Definition:
> Target Group = Set of servers that ALB sends traffic to.

---

## 🏗️ Architecture Flow

```

Internet User
↓
Application Load Balancer (xfusion-alb)
↓
Target Group (xfusion-tg)
↓
EC2 Instance (xfusion-ec2)
↓
Nginx Web Server

```id="arch_flow"

---

#  STEP 1 — AWS Console Login

- Login using provided credentials
- Select region: **us-east-1**

---

# STEP 2 — Verify EC2 Instance

Go to:

EC2 → Instances

Check:
- Instance Name: `xfusion-ec2`
- State: Running
- Nginx installed

---

#  STEP 3 — Create Security Group (ALB)

EC2 → Security Groups → Create

### Configuration:
- Name: `xfusion-sg`
- Allow HTTP access

### Inbound Rule:

| Type | Port | Source |
|------|------|--------|
| HTTP | 80   | 0.0.0.0/0 |

---

# STEP 4 — Update EC2 Security Group

Add rule:

| Type | Port | Source |
|------|------|--------|
| HTTP | 80   | xfusion-sg |

👉 This allows ALB to communicate with EC2

---

#  STEP 5 — Create Target Group

Go to:

EC2 → Target Groups → Create

### Configuration:
- Name: `xfusion-tg`
- Type: Instance
- Protocol: HTTP
- Port: 80
- VPC: Same as EC2

### Register Target:
- Select `xfusion-ec2`
- Add as pending target
- Create target group

---

#  STEP 6 — Create Application Load Balancer

Go to:

EC2 → Load Balancers → Create

### Configuration:
- Name: `xfusion-alb`
- Type: Application Load Balancer
- Scheme: Internet-facing
- IP type: IPv4

### Network Mapping:
- Select VPC
- Select 2 Availability Zones

### Security Group:
- Attach `xfusion-sg`

### Listener:
- HTTP: 80 → Forward to `xfusion-tg`

Create Load Balancer

---

#  STEP 7 — Wait for ALB Status

Ensure status:

```

Active

```id="status_active"

Copy DNS:

```

[http://xfusion-alb-xxxx.us-east-1.elb.amazonaws.com](http://xfusion-alb-xxxx.us-east-1.elb.amazonaws.com)

```

---

# 🌐 STEP 8 — Test Application

Open ALB DNS in browser:

Expected output:

```

Welcome to nginx!

```

---

# ✅ Final Verification Checklist

✔ EC2 running  
✔ Nginx installed  
✔ Target group created  
✔ ALB active  
✔ Security group configured  
✔ Traffic routing working  

---

# 🚀 Final Result

You successfully deployed a **highly available AWS architecture** using:

- Application Load Balancer
- Target Group
- EC2 Instance
- Nginx Web Server

---

# 🧠 Key Takeaway

- ALB distributes traffic intelligently
- Target Group manages backend servers
- Security Groups control access flow
- Health checks ensure reliability
```

---
