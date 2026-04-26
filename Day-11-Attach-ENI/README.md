# Day 11: Attach Elastic Network Interface to EC2 Instance

## Introduction

In this lab, we attach an existing Elastic Network Interface (ENI) to an EC2 instance. This is a fundamental AWS networking task that enables advanced network configurations and high availability.

**By the end of this lab, you will:**
- Understand what an Elastic Network Interface is
- Know how to attach an ENI to an EC2 instance
- Verify the attachment status using AWS Console
- Understand device indices for network interfaces

---

## Simple Definition

### What is an ENI?

An **Elastic Network Interface (ENI)** is a virtual network card that provides network connectivity to your EC2 instance.

**Think of it like:**
- 🖥️ A network adapter card in a physical computer
- 🔌 A network port on a server

### What Does "Attach" Mean?

**Attaching** an ENI to an EC2 instance means connecting that virtual network interface to your instance so it can communicate on the network.

### Simple Analogy

```
EC2 Instance (Computer)
│
├─ Primary ENI (Index 0) ← Built-in, Cannot Remove
│  └─ Status: in-use
│  └─ IP: 10.0.1.42
│
└─ Secondary ENI (Index 1) ← We ATTACH this one
   └─ Status: attached
   └─ IP: 10.0.1.50

Result: Instance with 2 Network Interfaces ✓
```

---

## Theory & Concepts

### EC2 Instance Network

Every EC2 instance is created with **at least ONE primary network interface** automatically:

| Property | Details |
|----------|---------|
| **Device Index** | 0 (primary slot) |
| **Status** | Always `in-use` when instance is running |
| **Can be Detached?** | ❌ NO (you cannot remove the primary ENI) |
| **Created** | Automatically with the instance |




## Step-by-Step Guide

### ✅ Step 1: Log in to AWS Management Console

**Goal:** Access your AWS account

**Actions:**
1. Open this URL in your web browser:
   ```
   https://183663605587.signin.aws.amazon.com/console?region=us-east-1
   ```

2. Enter your credentials:
   ```
   Username: kk_labs_user_834636
   Password: 8l^2KL0K9Pm1
   ```

3. Click **Sign in**

**Verification:**
- ✓ AWS Console appears
- ✓ Region shown as **us-east-1** (top-right)

---

### ✅ Step 2: Verify EC2 Instance Status

**Goal:** Confirm that instance `devops-ec2` is running

**Actions:**
1. Click **Services** → **EC2**
2. From left sidebar, click **Instances**
3. Look for instance named `devops-ec2`
4. Verify it is **running** (green indicator)

**What to Check:**

| Field | Expected Value |
|-------|---|
| Instance Name | `devops-ec2` ✓ |
| Instance State | `running` (green) ✓ |
| Status Checks | Passed or In Progress ✓ |

**Note down:**
- Instance ID: `i-020f3aa65f67d1082`

---

### ✅ Step 3: Verify ENI Availability

**Goal:** Confirm that ENI `devops-eni` exists and is available (not attached yet)

**Actions:**
1. From left sidebar, click **Network Interfaces**
2. Search or filter for `devops-eni`
3. Click on it to view details

**What to Check:**

| Field | Expected Value |
|-------|---|
| Interface Name | `devops-eni` ✓ |
| Status | `available` (not attached) ✓ |
| VPC ID | Same as EC2 instance ✓ |
| Subnet ID | Same as EC2 instance ✓ |

**Note down:**
- ENI ID: `eni-0c924d512f96cf767`

---

### ✅ Step 4: Attach ENI to EC2 Instance

**Goal:** Connect the ENI to the EC2 instance with Device Index 1

**Actions:**
1. While viewing `devops-eni` details, click **Actions** button
2. Select **Attach network interface** from dropdown
3. In the dialog:
   ```
   Instance ID: i-020f3aa65f67d1082
   Device Index: 1
   ```
4. Click **Attach**

**What's Happening:**
- AWS is connecting the ENI to the EC2 instance
- Device Index 1 = secondary network interface
- Takes 5-15 seconds to complete

---

### ✅ Step 5: Wait for Attachment

**Goal:** Allow time for the attachment to complete

**Actions:**
1. After clicking Attach, wait 10-15 seconds
2. Status will change from `attaching` to `attached`
3. Refresh the page to see updated status

---

### ✅ Step 6: Verify Attachment Status

**Goal:** Confirm the attachment is complete

**Actions:**
1. View the **Attachment** section on ENI details page
2. Check these values:

| Field | Expected Value |
|-------|---|
| Status | `attached` or `in-use` ✓ |
| Attachment ID | `eni-attach-0c924d512f96cf767` ✓ |
| Instance ID | `i-020f3aa65f67d1082` ✓ |
| Device Index | `1` ✓ |

---
