# Day 31: Configuring a Private RDS Instance for Application Development (AWS)




## 📌 Overview

In this lab, I deployed a **private Amazon RDS MySQL instance** for application development.
The goal was to understand how real-world databases are securely deployed inside a VPC — **without public internet access** — following the same architectural patterns used in production systems.

> 💡 **Key insight:** Database security is not an optional setting — it's an architectural decision.  
> Private databases reduce attack surfaces, protect application data, and reflect real-world cloud best practices.

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────┐
│               AWS Cloud (us-east-1)             │
│                                                  │
│  ┌──────────────────────────────────────────┐   │
│  │         VPC (Default VPC)                │   │
│  │                                          │   │
│  │  ┌─────────────┐      ┌──────────────┐  │   │
│  │  │  App Server  │────▶│  nautilus-rds│  │   │
│  │  │  (EC2)       │     │  MySQL 8.4.x │  │   │
│  │  │              │     │  db.t3.micro  │  │   │
│  │  └─────────────┘      └──────────────┘  │   │
│  │         ↑                    ↑           │   │
│  │   Port 3306 only      Private Subnet     │   │
│  │   (Security Group)    No Public IP       │   │
│  └──────────────────────────────────────────┘   │
│                                                  │
│  ❌ Internet cannot reach the database directly  │
└─────────────────────────────────────────────────┘
```

---

##  Theory: What is RDS?

Amazon RDS (Relational Database Service) is a **managed database service** by AWS that handles:

| Feature | Benefit |
|---|---|
| Automated backups | No manual snapshot management |
| Storage autoscaling | Grows with your data automatically |
| Multi-AZ option | High availability and failover |
| Security groups | Fine-grained access control |
| Managed patching | OS and engine updates handled by AWS |

### Why Private RDS?

In production systems:
- Databases are **never** exposed to the public internet
- They live inside a **private subnet** within the VPC
- Only trusted application servers can communicate with them on **port 3306**

This architecture reduces attack surface and protects sensitive user data.

---

##  Objective

- [x] Create a private RDS MySQL instance named `nautilus-rds`
- [x] Use AWS Free Tier (`db.t3.micro`)
- [x] Configure Full Configuration creation method
- [x] Enable storage autoscaling with 50 GB threshold
- [x] Disable public access (private VPC deployment)
- [x] Verify instance reaches `Available` status

---

## 🪜 Step-by-Step Lab

### Step 1: Login to AWS Console
- Open: `https://844643069394.signin.aws.amazon.com/console`
- Region: **us-east-1 (N. Virginia)**

---

### Step 2: Open RDS Service
- Search **RDS** in the AWS Console search bar
- Click **Databases** → **Create database**

---

### Step 3: Choose Database Settings

| Setting | Value |
|---|---|
| Creation method | Standard Create (Full Configuration) |
| Engine | MySQL |
| Version | 8.4.x (latest patch) |
| Template | **Free Tier** |

---

### Step 4: DB Instance Configuration

| Setting | Value |
|---|---|
| DB Identifier | `nautilus-rds` |
| Instance Class | `db.t3.micro` |
| Master Username | `admin` |
| Master Password | *(set a strong password)* |

---

### Step 5: Storage Configuration

| Setting | Value |
|---|---|
| Storage Type | General Purpose SSD (gp2) |
| Allocated Storage | 20 GiB *(Free Tier default)* |
| Storage Autoscaling | ✅ **Enabled** |
| Maximum Threshold | **50 GB** |

>  **Important:** Set allocated storage to **20 GiB** (not 50). The 50 GB is the *autoscaling threshold*, not the starting size.

---

### Step 6: Connectivity Configuration

| Setting | Value |
|---|---|
| VPC | Default VPC |
| Subnet Group | default |
| Public Access | ❌ **No** |
| Availability Zone | No preference |

> 👉 Setting **Public Access: No** is what makes this a private RDS instance.

---

### Step 7: Security Group
- Allow inbound access only from your application EC2 instance
- Protocol: **TCP** | Port: **3306** | Source: EC2 Security Group ID

---

### Step 8: Create & Verify
- Click **Create Database**
- Wait for status: **Available** *(takes ~5–10 minutes)*

---

## 💻 AWS CLI Method

> If the AWS Console is blocked by an organizational policy, use the CLI instead.

```bash
aws rds create-db-instance \
  --db-instance-identifier nautilus-rds \
  --db-instance-class db.t3.micro \
  --engine mysql \
  --engine-version 8.4.4 \
  --master-username admin \
  --master-user-password YourStrongPassword123! \
  --allocated-storage 20 \
  --storage-type gp2 \
  --max-allocated-storage 50 \
  --no-publicly-accessible \
  --region us-east-1
```

**Check instance status:**
```bash
aws rds describe-db-instances \
  --db-instance-identifier nautilus-rds \
  --query 'DBInstances[0].DBInstanceStatus' \
  --region us-east-1
```

Keep running until output returns:
```
"available"
```

---

## 🧠Key Learnings

1. **Private by design** — databases should never be internet-facing in any environment
2. **Allocated storage ≠ autoscaling threshold** — two different settings with different purposes
3. **SCPs are organization-level guardrails** — they override IAM policies and apply to all users
4. **CLI is a fallback superpower** — when console is blocked, the CLI often still works
5. **Free Tier limits matter** — staying within 20 GiB allocated storage avoids unexpected charges
6. **Security groups are the last line of defense** — restrict port 3306 to trusted sources only

---

## 🚀 Real-World Use Case

This private RDS architecture is used in:

| Use Case | Why Private RDS? |
|---|---|
| Backend REST APIs | App server connects privately; database never exposed |
| Microservices | Each service talks to DB via internal VPC routing |
| Enterprise systems | Compliance requires databases off the public internet |
| SaaS applications | Multi-tenant data isolation within private networking |

---
