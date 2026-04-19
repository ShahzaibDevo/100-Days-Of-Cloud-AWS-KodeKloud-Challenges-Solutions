# ☁️ Day 5: Create GP3 Volume — AWS Cloud Migration Lab

-

## 📖 Table of Contents

- [Theory](#-theory)
  - [What is Amazon EBS?](#what-is-amazon-ebs)
  - [What is a GP3 Volume?](#what-is-a-gp3-volume)
  - [EBS Volume Types](#ebs-volume-types)
  - [GP2 vs GP3](#gp2-vs-gp3)
  - [How EBS Works](#how-ebs-works)
- [Lab Objective](#-lab-objective)
- [Prerequisites](#-prerequisites)
- [Steps: AWS Console (GUI)](#️-steps-aws-console-gui)
- [Verification](#-verification)
- [Key Takeaways](#-key-takeaways)
- [Related Labs](#-related-labs)

---

## 📚 Theory

### What is Amazon EBS?

**Amazon EBS (Elastic Block Store)** is a high-performance **block storage service** designed for use with Amazon EC2 instances. Think of it as a **virtual hard drive** that you can attach to your cloud server.

```
💻 EC2 Instance  =  Your Computer
💾 EBS Volume    =  The Hard Drive inside that computer
```

Key characteristics of EBS:

| Feature | Details |
|---|---|
| **Type** | Block storage (like a physical hard disk) |
| **Attachment** | Attached to a single EC2 instance at a time |
| **Persistence** | Data persists even after EC2 is stopped |
| **Scope** | Tied to a single Availability Zone |
| **Scalability** | Can be resized without stopping the instance |

---

### What is a GP3 Volume?

**GP3 (General Purpose SSD v3)** is the **latest generation** of AWS general-purpose SSD volumes. It delivers a consistent baseline performance and is the **recommended default** volume type for most workloads.

```
GP3 = Great Performance + Lower Cost + Full Control
```

**GP3 Baseline Specs:**

| Spec | Value |
|---|---|
| **IOPS** | 3,000 (baseline, free) |
| **Throughput** | 125 MiB/s (baseline, free) |
| **Max IOPS** | 16,000 |
| **Max Throughput** | 1,000 MiB/s |
| **Size Range** | 1 GiB — 16 TiB |
| **Cost** | ~20% cheaper than GP2 |

---

### EBS Volume Types

| Type | Use Case | Description |
|---|---|---|
| **gp3** ✅ | General purpose | Latest gen SSD — best for most workloads |
| **gp2** | General purpose | Previous gen SSD — being phased out |
| **io1 / io2** | High performance | For critical databases needing high IOPS |
| **st1** | Big data | Low-cost HDD for sequential workloads |
| **sc1** | Cold storage | Cheapest HDD for infrequently accessed data |

---

### GP2 vs GP3

| Feature | GP2 | GP3 |
|---|---|---|
| **Baseline IOPS** | 3 IOPS per GiB (burst) | 3,000 fixed (free) |
| **Baseline Throughput** | 128–250 MiB/s | 125 MiB/s (free) |
| **Max IOPS** | 16,000 | 16,000 |
| **Max Throughput** | 250 MiB/s | 1,000 MiB/s |
| **Cost** | Higher | ~20% cheaper ✅ |
| **IOPS/Throughput Control** | Tied to size | Independent control ✅ |
| **Recommended** | ❌ Legacy | ✅ Always choose GP3 |

> 💡 **Bottom Line:** GP3 gives you **more control**, **better performance**, and **lower cost** than GP2. Always choose GP3 for new volumes!

---

### How EBS Works

```
AWS Region: us-east-1
│
└── Availability Zone: us-east-1a
    │
    ├── 💾 nautilus-volume (gp3 | 2 GiB)  ← Created Today!
    │       │
    │       │  attach
    │       ▼
    └── 💻 EC2 Instance (Future)
            │
            └── /dev/xvdf  ← Volume mounted here
```

**EBS Volume Lifecycle:**
```
Create Volume
     │
     ▼
  Available  ──attach──►  In-Use  ──detach──►  Available
     │                                               │
     └──────────────── delete ─────────────────────►┘
```

---

## 🎯 Lab Objective

Create an EBS Volume with the following specifications:

| Parameter | Value |
|---|---|
| **Volume Name** | `nautilus-volume` |
| **Volume Type** | `gp3` |
| **Volume Size** | `2 GiB` |
| **Region** | `us-east-1` |
| **Availability Zone** | `us-east-1a` |

---

## ✅ Prerequisites

- Access to AWS Console
- IAM user with `ec2:CreateVolume` permission
- Region set to `us-east-1`

---

## 🖥️ Steps: AWS Console (GUI)

### Step 1 — Log into the AWS Console

1. Open browser → navigate to the **AWS Console**
2. Enter your **IAM username and password**
3. Click **Sign In**

> ✅ Confirm top-right shows **US East (N. Virginia) — us-east-1**

---

### Step 2 — Navigate to EC2

1. In the **top search bar** → type `EC2`
2. Click **EC2** from results to open EC2 Dashboard

---

### Step 3 — Go to Volumes

1. In the **left navigation panel**
2. Scroll down to **Elastic Block Store** section
3. Click **Volumes**

```
EC2 Dashboard
└── Elastic Block Store
    └── Volumes   ◄── Click here
```

---

### Step 4 — Click "Create Volume"

1. Click the orange **Create volume** button (top-right corner)

---

### Step 5 — Configure Volume Settings

Fill in the following details:

| Field | Value |
|---|---|
| **Volume type** | `gp3` |
| **Size (GiB)** | `2` |
| **Availability Zone** | `us-east-1a` |
| **IOPS** | `3000` *(default — leave as is)* |
| **Throughput (MiB/s)** | `125` *(default — leave as is)* |
| **Snapshot ID** | Leave empty |
| **Encryption** | Leave default |

---

### Step 6 — Add Name Tag

1. Scroll down to the **Tags** section
2. Click **Add tag**
3. Enter:

| Key | Value |
|---|---|
| `Name` | `nautilus-volume` |

---

### Step 7 — Create the Volume

1. Click the orange **Create volume** button at the bottom
2. You will see a **green success banner** ✅
3. You are redirected to the Volumes list page
4. Confirm `nautilus-volume` appears with status **available**

---
