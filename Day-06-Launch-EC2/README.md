## 🚀 Day 6 of 90 Days DevOps Challenge — Launch EC2 Instance on AWS

---

### 📌 Task Overview
Launched an EC2 instance on AWS Console as part of incremental cloud infrastructure migration for the Nautilus DevOps team.

---

### 🎯 Requirements
- ✅ Instance Name: `devops-ec2`
- ✅ AMI: Amazon Linux 2023
- ✅ Instance Type: `t2.micro`
- ✅ Key Pair: `devops-kp` (RSA)
- ✅ Security Group: `default`
- ✅ Region: `us-east-1`

---

### 🖥️ Steps Performed via AWS Console

**Step 1 — Login to AWS Console**
> Navigated to AWS Management Console and selected region `us-east-1`

**Step 2 — Navigate to EC2**
> Searched for **EC2** in the search bar → Clicked **Launch Instance**

**Step 3 — Set Instance Name**
> Entered name as `devops-ec2`

**Step 4 — Choose AMI**
> Selected **Amazon Linux 2023 AMI** (Free Tier eligible) from the Quick Start list

**Step 5 — Select Instance Type**
> Chose `t2.micro` (Free Tier eligible)

**Step 6 — Create Key Pair**
> Clicked **Create new key pair**
> - Name: `devops-kp`
> - Type: `RSA`
> - Format: `.pem`
> - Clicked **Create key pair** → Downloaded the `.pem` file

**Step 7 — Configure Security Group**
> Under Network Settings → Selected **"Select existing security group"**
> Chose the `default` security group

**Step 8 — Launch Instance**
> Reviewed all settings → Clicked **Launch Instance** ✅

---

### ✅ Verification Output

```
-------------------------------------------------------------
|                     DescribeInstances                     |
+----------------------+------------+----------+------------+
|          ID          |  KeyName   |  State   |   Type     |
+----------------------+------------+----------+------------+
|  i-0a46e49301a2845e7 |  devops-kp |  running |  t2.micro  |
+----------------------+------------+----------+------------+
```

---

### 💡 Key Learnings

- 🔑 **Key Pairs** — RSA key pairs provide secure SSH access to EC2 instances. The `.pem` file must be stored safely — AWS will never show it again
- 🛡️ **Security Groups** — Virtual firewalls that control inbound and outbound traffic to your EC2 instance
- 🖥️ **Instance Types** — `t2.micro` is Free Tier eligible, perfect for lightweight workloads and learning environments
- 🐧 **Amazon Linux AMI** — AWS-optimized Linux distribution with pre-installed AWS tools and regular security updates
- ☁️ **EC2 Fundamentals** — Understanding how compute resources are provisioned in the cloud is a core DevOps skill

---

