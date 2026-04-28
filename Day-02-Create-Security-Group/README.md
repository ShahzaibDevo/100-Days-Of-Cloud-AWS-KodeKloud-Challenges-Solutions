# ☁️ Day 2: Create Security Group — AWS Cloud Migration Lab

> **Series:** Nautilus DevOps — AWS Cloud Migration (Incremental Steps)
> **Region:** `us-east-1`
> **Difficulty:** Beginner
> **Lab Time:** ~15 minutes

---

## 📖 Table of Contents

- [Theory](#-theory)
  - [What is a Security Group?](#what-is-a-security-group)
  - [How Security Groups Work](#how-security-groups-work)
  - [Inbound vs Outbound Rules](#inbound-vs-outbound-rules)
  - [CIDR Notation](#cidr-notation)
  - [Security Group vs NACL](#security-group-vs-network-acl-nacl)
- [Lab Objective](#-lab-objective)
- [Prerequisites](#-prerequisites)
- [Steps: AWS Console (GUI)](#️-steps-aws-console-gui)
- [Verification](#-verification)
- [Key Takeaways](#-key-takeaways)

---

## 📚 Theory

### What is a Security Group?

A **Security Group** in AWS acts as a **virtual firewall** for your EC2 instances and other AWS resources. It controls the traffic that is allowed to reach and leave the resources it is associated with.

Think of it like a **bouncer at a club door** — it checks every incoming and outgoing network packet against a set of rules and decides whether to allow or deny it.

```
Internet
    │
    ▼
┌─────────────────────────────┐
│        Security Group        │  ◄── Virtual Firewall
│  ┌─────────────────────────┐ │
│  │     EC2 Instance        │ │
│  │  (Nautilus App Server)  │ │
│  └─────────────────────────┘ │
└─────────────────────────────┘
```

Security Groups are **stateful** — if you allow inbound traffic, the response is automatically allowed outbound, and vice versa.

---

### How Security Groups Work

| Feature | Details |
|---|---|
| **Type** | Stateful Firewall |
| **Scope** | Instance level (attached to ENI) |
| **Default behavior** | Deny all inbound, Allow all outbound |
| **Rule types** | Allow only (no explicit deny) |
| **Association** | Can be attached to multiple instances |
| **Multiple SGs** | One instance can have up to 5 SGs |

**Key Principle:** Security Groups use **whitelisting** — you only define what is *allowed*. Everything else is automatically denied. There is no way to create a "deny" rule inside a Security Group.

---

### Inbound vs Outbound Rules

```
           INBOUND RULES                     OUTBOUND RULES
           (Ingress)                          (Egress)
         ┌──────────────┐                  ┌──────────────┐
Internet ─► Port 80 (HTTP) ─► EC2          EC2 ──────────► All traffic (default)
         │ Port 22 (SSH)  │                └──────────────┘
         └──────────────┘
```

| Rule Direction | Purpose | Our Lab Config |
|---|---|---|
| **Inbound** | Controls traffic coming INTO the instance | HTTP (80), SSH (22) |
| **Outbound** | Controls traffic going OUT of the instance | All traffic (default) |

---

### CIDR Notation

**CIDR (Classless Inter-Domain Routing)** is a way to define a range of IP addresses.

| CIDR | Meaning | Use Case |
|---|---|---|
| `0.0.0.0/0` | All IPv4 addresses (entire internet) | Public-facing services |
| `10.0.0.0/16` | All IPs in the 10.0.x.x range | Internal VPC traffic |
| `192.168.1.100/32` | A single specific IP address | Restrict SSH to your IP |

> ⚠️ **Security Note:** Using `0.0.0.0/0` for SSH (port 22) in production is **not recommended**. It allows anyone on the internet to attempt an SSH connection. In real-world scenarios, restrict SSH to your office/home IP (`your.ip.address/32`). This lab uses `0.0.0.0/0` purely for simplicity.

---

### Security Group vs Network ACL (NACL)

| Feature | Security Group | Network ACL |
|---|---|---|
| **Level** | Instance level | Subnet level |
| **State** | Stateful | Stateless |
| **Rules** | Allow only | Allow AND Deny |
| **Rule evaluation** | All rules evaluated | Rules evaluated in order (number) |
| **Default** | Deny all inbound | Allow all inbound & outbound |
| **Use case** | Fine-grained instance control | Broad subnet-level protection |

Security Groups and NACLs **work together** as layers of defense (defense-in-depth).

---

## 🎯 Lab Objective

Create a Security Group under the **default VPC** in `us-east-1` with:

| Parameter | Value |
|---|---|
| **Name** | `nautilus-sg` |
| **Description** | `Security group for Nautilus App Servers` |
| **VPC** | Default VPC |
| **Inbound Rule 1** | Type: HTTP \| Port: 80 \| Source: `0.0.0.0/0` |
| **Inbound Rule 2** | Type: SSH \| Port: 22 \| Source: `0.0.0.0/0` |

---

## ✅ Prerequisites

- Access to AWS Console
- IAM user with `ec2:CreateSecurityGroup` and `ec2:AuthorizeSecurityGroupIngress` permissions
- A default VPC must exist in `us-east-1` (present by default in all AWS accounts)

---

## 🖥️ Steps: AWS Console (GUI)

### Step 1 — Log into the AWS Console

1. Open your browser and navigate to:
   ```
   https://511964875546.signin.aws.amazon.com/console?region=us-east-1
   ```
2. Enter your credentials:
   - **IAM Username:** `kk_labs_user_991785`
   - **Password:** `o!FOoN!MC7j7`
3. Click **Sign In**

> ✅ Confirm the region in the top-right corner shows **US East (N. Virginia) us-east-1**

---

### Step 2 — Navigate to Security Groups

1. In the **top search bar**, type `EC2` and press Enter
2. Click **EC2** from the results to open the EC2 Dashboard
3. In the **left navigation panel**, scroll down to the **Network & Security** section
4. Click **Security Groups**

```
EC2 Dashboard
└── Network & Security
    └── Security Groups   ◄── Click here
```

---

### Step 3 — Start Creating the Security Group

1. Click the orange **Create security group** button in the top-right corner

---

### Step 4 — Fill in Basic Details

In the **Basic details** section, enter:

| Field | Value |
|---|---|
| Security group name | `nautilus-sg` |
| Description | `Security group for Nautilus App Servers` |
| VPC | Select the **default VPC** (labeled `default`) |

> 💡 The default VPC will have the tag **default** next to its VPC ID. It is automatically created in every AWS region for your account.

---

### Step 5 — Add Inbound Rule 1 (HTTP)

1. Scroll down to the **Inbound rules** section
2. Click **Add rule**
3. Configure:

| Field | Value |
|---|---|
| Type | `HTTP` |
| Protocol | TCP *(auto-filled)* |
| Port range | `80` *(auto-filled)* |
| Source | Custom → `0.0.0.0/0` |
| Description *(optional)* | `Allow HTTP traffic from internet` |

---

### Step 6 — Add Inbound Rule 2 (SSH)

1. Click **Add rule** again
2. Configure:

| Field | Value |
|---|---|
| Type | `SSH` |
| Protocol | TCP *(auto-filled)* |
| Port range | `22` *(auto-filled)* |
| Source | Custom → `0.0.0.0/0` |
| Description *(optional)* | `Allow SSH access` |

---

### Step 7 — Review Outbound Rules

Leave the **Outbound rules** as default:
- **Type:** All traffic
- **Destination:** `0.0.0.0/0`

This allows the instance to reach the internet for updates, API calls, etc.

---

### Step 8 — Create the Security Group

1. Click the orange **Create security group** button at the bottom
2. You will be redirected to the Security Group detail page
3. Confirm the group was created successfully — you should see a new **Security group ID** like `sg-0abc123def456`

---

### ✅ Verification Checklist

```
[ ] Security group name    →  nautilus-sg
[ ] Description            →  Security group for Nautilus App Servers
[ ] VPC                    →  Default VPC
[ ] Inbound Rule 1         →  TCP | Port 80 | 0.0.0.0/0
[ ] Inbound Rule 2         →  TCP | Port 22 | 0.0.0.0/0
[ ] Region                 →  us-east-1
```

### Expected Console View

```
Security group: nautilus-sg (sg-xxxxxxxxxxxxxxxxx)

Inbound rules:
┌────────┬──────────┬───────┬─────────────┐
│ Type   │ Protocol │ Port  │ Source      │
├────────┼──────────┼───────┼─────────────┤
│ HTTP   │ TCP      │ 80    │ 0.0.0.0/0   │
│ SSH    │ TCP      │ 22    │ 0.0.0.0/0   │
└────────┴──────────┴───────┴─────────────┘

Outbound rules:
┌─────────────┬──────────┬──────┬─────────────┐
│ Type        │ Protocol │ Port │ Destination │
├─────────────┼──────────┼──────┼─────────────┤
│ All traffic │ All      │ All  │ 0.0.0.0/0   │
└─────────────┴──────────┴──────┴─────────────┘
```

---

## 💡 Key Takeaways

- A **Security Group** is a stateful, instance-level virtual firewall in AWS
- Security Groups are **allow-only** — you cannot write explicit deny rules
- **Stateful** means return traffic is automatically permitted without a separate outbound rule
- `0.0.0.0/0` means traffic is allowed from **any IP** on the internet
- Port **80** is used for **HTTP** (unencrypted web traffic)
- Port **22** is used for **SSH** (secure remote terminal access)
- In production, always restrict SSH source to a trusted IP range instead of `0.0.0.0/0`
- Security Groups can be **reused** across multiple EC2 instances — ideal for consistent policy enforcement across your Nautilus App Servers

---
