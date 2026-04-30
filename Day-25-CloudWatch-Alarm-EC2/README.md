# ☁️ Day 25: Setting Up an EC2 Instance and CloudWatch Alarm (AWS Lab)

## 🎯 Lab Objective

In this lab, we set up an **EC2 instance** and configured a **CloudWatch Alarm** to monitor CPU utilization. The alarm triggers when CPU usage exceeds **90% for 5 minutes**, and sends a notification using **SNS topic**.

---

## What is AWS CloudWatch?

AWS CloudWatch is a monitoring and observability service used to track the performance of AWS resources and applications in real time.

It collects metrics, logs, and events so you can understand what is happening inside your cloud environment.

## 📌 What is this Lab About?

This lab demonstrates **real-time monitoring and alerting in AWS** using:

* EC2 (Virtual Server)
* CloudWatch (Monitoring Service)
* SNS (Notification Service)

👉 Goal: Automatically detect high CPU usage and alert DevOps teams.

---

## 🧭 STEP-BY-STEP LAB GUIDE

### 🟢 STEP 1 — Login to AWS Console

* Open AWS Console URL
* Login using provided credentials
* Select region: **us-east-1**

---

### 🟢 STEP 2 — Launch EC2 Instance

Go to:
👉 EC2 → Instances → Launch Instance

Configure:

* Name: `datacenter-ec2`
* AMI: Ubuntu Server
* Instance Type: `t2.micro`
* Key Pair: (optional / lab default)
* Network: default VPC

Click:
👉 **Launch Instance**

---

### 🟢 STEP 3 — Verify EC2 Instance

Go to:
👉 EC2 → Instances

Check:

* Instance State: **Running**
* Status Checks: **2/2 passed**

---

### 🟢 STEP 4 — Open CloudWatch Service

Go to:
👉 AWS Console → CloudWatch

Navigate:
👉 Alarms → Create Alarm

---

### 🟢 STEP 5 — Select Metric

Choose:

* Namespace: `AWS/EC2`
* Metric Name: `CPUUtilization`
* Instance: `datacenter-ec2`
* Statistic: `Average`
* Period: `5 minutes`

Click:
👉 Select Metric

---

### 🟢 STEP 6 — Define Conditions

Set:

* Threshold Type: **Static**
* Condition: `CPU >= 90`
* Evaluation Period: `1 out of 1 datapoint (5 min)`

👉 This means alarm triggers if CPU crosses 90% once in 5 minutes

---

### 🟢 STEP 7 — Configure Actions (SNS)

Set Alarm Action:

* State: **In Alarm**
* Action Type: **Send Notification**
* SNS Topic: `datacenter-sns-topic`

👉 This sends email/alert when CPU is high

---

### 🟢 STEP 8 — Add Alarm Details

* Alarm Name: `datacenter-alarm`
* Description: CPU monitoring alarm for EC2 instance

Click:
👉 **Next → Create Alarm**

---

### 🟢 STEP 9 — Generate CPU Load (Testing)

Login to EC2 and run:

```bash
stress --cpu 2 --timeout 300
```

👉 This increases CPU usage to test alarm

---

### 🟢 STEP 10 — Verify Alarm State

Go to:
👉 CloudWatch → Alarms

You will see:

* Insufficient Data → OK → In Alarm

---

## 📊 Simple Architecture Diagram

```
        +----------------------+
        |   AWS CloudWatch     |
        |  (CPU Monitoring)    |
        +----------+-----------+
                   |
                   v
        +----------------------+
        |   CloudWatch Alarm   |
        | CPU >= 90% (5 min)  |
        +----------+-----------+
                   |
                   v
        +----------------------+
        |     SNS Topic        |
        |  datacenter-sns      |
        +----------+-----------+
                   |
                   v
        +----------------------+
        | Email / Notification |
        +----------------------+

        +----------------------+
        |   EC2 Instance       |
        |  datacenter-ec2      |
        |  (CPU Stress Test)   |
        +----------------------+
```

---

## 🧠 Key Concepts

### 🔹 EC2 (Elastic Compute Cloud)

A virtual server in AWS used to run applications.

### 🔹 CloudWatch

A monitoring service that tracks AWS resources like CPU, memory, and network.

### 🔹 CloudWatch Alarm

A rule that triggers actions when metrics cross a threshold.

### 🔹 SNS (Simple Notification Service)

A messaging service used to send alerts (email/SMS/app notifications).

---

## 🚀 Real-World Use Case

In production environments:

* If CPU usage increases due to high traffic
* CloudWatch detects it
* Alarm triggers automatically
* SNS notifies DevOps team
* Auto Scaling may add new servers

👉 This ensures **zero downtime systems**.

---
