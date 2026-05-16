#  Day 40: Troubleshooting Internet Accessibility for an EC2-Hosted Application

## Project Overview

This lab focuses on troubleshooting a real-world issue where an application hosted on **Amazon EC2** is not accessible from the internet, even though the application (Nginx) is running correctly.

The root cause lies in the networking layer of **Amazon VPC**, requiring proper diagnosis and fixing of route tables, internet gateway, and subnet configuration.

---

#  Objective

* Diagnose EC2 connectivity issues
* Fix VPC misconfiguration blocking internet access
* Ensure EC2 instance is accessible via public IP
* Validate Nginx web server accessibility on port 80

---

# Architecture Understanding

```id="0i7d8s"
Internet
   ↓
Internet Gateway
   ↓
Route Table (0.0.0.0/0 → IGW)
   ↓
Public Subnet
   ↓
EC2 Instance (Nginx Server)
```

---

#  Prerequisites

* AWS Account (Lab provided credentials)
* EC2 instance running
* Nginx installed
* Region: `us-east-1`
* Public IP assigned to instance

---

#  Step-by-Step Lab Implementation

---

##  Step 1: Verify EC2 Instance

Go to:

**EC2 → Instances**

Check:

```id="ec2check"
Instance: devops-ec2
State: Running
Public IP: Available
```

---

##  Step 2: Check Security Group

Ensure inbound rules allow:

| Type | Port | Source    |
| ---- | ---- | --------- |
| SSH  | 22   | 0.0.0.0/0 |
| HTTP | 80   | 0.0.0.0/0 |

---

##  Step 3: Identify the Issue

Problem observed:

* EC2 instance running ✔
* Nginx running ✔
* Website not accessible 

AWS error:

> Missing active route to Internet Gateway (blackhole route detected)

---

##  Root Cause

* Route table contains invalid or missing route
* Internet Gateway not properly attached or configured
* Traffic cannot reach EC2 instance

---

## 🔹 Step 4: Verify Internet Gateway

Go to:

**VPC → Internet Gateways**

Check:

* IGW exists
* Attached to correct VPC

If not attached:

 Attach to VPC

---

## 🔹 Step 5: Fix Route Table

Go to:

**VPC → Route Tables → devops-rtb**

Edit routes:

Remove broken route 

Add correct route:

```
Destination: 0.0.0.0/0
Target: Internet Gateway (igw-xxxxx)
```

Save changes ✔

---

## 🔹 Step 6: Verify Subnet Association

Ensure EC2 subnet is associated:

* Subnet must be PUBLIC
* Auto-assign public IP enabled

---

## 🔹 Step 7: Validate Nginx Service

Connect to EC2:

```bash id="nginxcheck"
sudo systemctl status nginx
```

If required:

```bash id="nginxstart"
sudo systemctl start nginx
sudo systemctl enable nginx
```

---

## 🔹 Step 8: Test Application

Open browser:

```
http://<EC2-PUBLIC-IP>
```

Example:

```
http://54.237.162.164
```

---

#  Final Result

✔ EC2 instance accessible from internet
✔ Nginx server responding successfully
✔ Route table fixed
✔ Internet Gateway properly configured

---

# 💡 Key Learnings

* EC2 accessibility depends heavily on VPC networking
* Route tables control traffic flow to Internet Gateway
* Blackhole routes cause complete connectivity failure
* Security Group alone is not enough for internet access
* Proper subnet + IGW configuration is critical

---

# 🏁 Conclusion

This lab demonstrates a real-world DevOps troubleshooting scenario where application-level health was correct, but networking misconfiguration blocked external access.

Fixing VPC routing restored full internet connectivity to the EC2-hosted application.

---

# 🏷️ Tags

#AWS #EC2 #VPC #DevOps #CloudComputing #Linux #Networking #100DaysOfCloud #AWSProjects #Troubleshooting #LearningByDoing 🚀

---

If you want next, I can also create:

✅ Day 40 LinkedIn Post
✅ Interview Q&A from this lab
✅ DevOps troubleshooting cheat sheet
✅ Portfolio-ready project diagram

Just tell me 👍
