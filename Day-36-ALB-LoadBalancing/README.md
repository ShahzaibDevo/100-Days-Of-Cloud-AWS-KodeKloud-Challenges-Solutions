
#  Day 36 — Load Balancing EC2 Instances with Application Load Balancer (AWS)

##  100 Days of Cloud (AWS Journey)

This project demonstrates how to deploy and manage a **scalable web application architecture** using an **Application Load Balancer (ALB)** with EC2 and Nginx.

---

#  Objective

To deploy an EC2-based web server and configure an **Application Load Balancer (ALB)** with a **Target Group** to distribute traffic and ensure high availability.

---

#  Architecture Diagram (Simple)

```
                Internet
                    │
                    ▼
     ┌─────────────────────────────┐
     │ Application Load Balancer   │
     │        (ALB)                │
     └────────────┬────────────────┘
                  │
                  ▼
        ┌───────────────────┐
        │  Target Group     │
        │  (nautilus-tg)    │
        └────────┬──────────┘
                 │
                 ▼
        ┌───────────────────┐
        │  EC2 Instance     │
        │  (Nginx Server)   │
        └───────────────────┘
```

---

#  AWS Services Used

* **Amazon Web Services**
* **Amazon EC2** → Web server hosting
* **Application Load Balancer** → Traffic distribution
* **Target Group** → Backend routing + health checks
* **Nginx** → Web application server

---

#  Step-by-Step Lab Guide

##  Step 1: Create Security Group (nautilus-sg)

* Open EC2 Dashboard → Security Groups
* Create Security Group:

  * Name: `nautilus-sg`
  * Inbound Rule:

    * HTTP (80)
    * Source: Default Security Group (ALB)

---

##  Step 2: Launch EC2 Instance

* Name: `nautilus-ec2`
* AMI: Ubuntu
* Security Group: `nautilus-sg`

### User Data Script:

```bash
#!/bin/bash
apt update -y
apt install nginx -y
systemctl start nginx
systemctl enable nginx
```

---

##  Step 3: Create Target Group

* Name: `nautilus-tg`
* Type: Instance
* Protocol: HTTP
* Port: 80
* Register EC2 instance (`nautilus-ec2`)

---

##  Step 4: Create Application Load Balancer

* Name: `nautilus-alb`
* Scheme: Internet-facing
* IP Type: IPv4
* Attach Default Security Group
* Select 2 Availability Zones

---

##  Step 5: Configure Listener

* Protocol: HTTP
* Port: 80
* Forward traffic to: `nautilus-tg`

---

## 🔴Step 6: Test Target Health

Go to Target Group:

* Ensure status = **Healthy**

---

##  Step 7: Access Application

Copy ALB DNS:

```
http://<your-alb-dns>
```

Example:

```
http://nautilus-alb-xxxx.us-east-1.elb.amazonaws.com
```

---

#  Expected Result

You should see:

```
Welcome to nginx!
```

---

#  Key Learning

* Load Balancer does NOT send traffic directly to EC2
* Target Groups manage backend servers + health checks
* ALB ensures scalability and high availability
* Nginx confirms successful backend deployment

---

# ⚫ Final Architecture Flow

```
User → ALB → Target Group → EC2 → Nginx Response
```

---

# 🚀 Outcome

✔ Successfully deployed a load-balanced architecture
✔ Understood ALB + Target Group workflow
✔ Gained hands-on AWS deployment experience
✔ Built production-style cloud architecture

---

# 🏷️ Tags

`#AWS` `#CloudComputing` `#DevOps` `#EC2` `#ALB`
`#100DaysOfCloud` `#Nginx` `#CloudEngineer` `#DevOpsJourney`

---

If you want next upgrade, I can also:

🚀 Add **real image architecture diagram (PNG for GitHub)**
🔥 Convert this into **portfolio-level README (with badges + banner)**
📌 Or merge Day 1–36 into a **complete cloud engineer portfolio**

Just tell me 👍
