
#  Day 37 — EC2 to S3 Access using IAM Role (AWS)

##  100 Days of Cloud (DevOps Journey)

This project demonstrates how to securely connect an **EC2 instance with an S3 bucket** using **IAM Role-based access instead of access keys**.

---

#  Objective

To allow an EC2 instance to securely:

* Upload files to S3 
* Read files from S3
* List S3 bucket contents

WITHOUT storing credentials inside the server.

---

#  Theory (Concept Understanding)

##  What is IAM Role?

An **IAM Role** is a secure identity in AWS that provides **temporary permissions** to AWS services like EC2.

 No username
 No password
 No access keys

---

##  Why S3 + EC2 Integration?

**Amazon S3** is used to store files, backups, logs, and application data.

**Amazon EC2** runs applications that need to interact with S3.

---

##  Why IAM Role is Important?

Instead of using risky access keys:

❌ Hardcoded credentials inside server

We use:

✔ IAM Role (secure & temporary access)
✔ IAM Policy (permission rules)

---

##  Architecture Flow

```
Developer (aws-client)
        ↓ SSH
EC2 Instance (xfusion-ec2)
        ↓
IAM Role (xfusion-role)
        ↓
IAM Policy (S3 Permissions)
        ↓
S3 Bucket (xfusion-s3)
```

---

#  Step-by-Step Lab Guide

---

##  Step 1 — Create SSH Key (aws-client)

```bash
ssh-keygen -t rsa
cat ~/.ssh/id_rsa.pub
```

 Copy public key

---

##  Step 2 — Add Key to EC2

SSH into EC2:

```bash
ssh ubuntu@<EC2-PUBLIC-IP>
```

Inside EC2:

```bash
mkdir -p ~/.ssh
nano ~/.ssh/authorized_keys
```

Paste public key

Fix permissions:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

---

##  Step 3 — Create S3 Bucket

Go to AWS Console:

* Bucket name: `xfusion-s3-390964011108`
* Region: `us-east-1`
* Block Public Access: ON (Private)

---

##  Step 4 — Create IAM Policy

Create policy with S3 permissions:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:PutObject",
        "s3:GetObject",
        "s3:ListBucket"
      ],
      "Resource": [
        "arn:aws:s3:::xfusion-s3-390964011108",
        "arn:aws:s3:::xfusion-s3-390964011108/*"
      ]
    }
  ]
}
```

Name:

```
xfusion-policy
```

---

##  Step 5 — Create IAM Role

* Trusted entity: EC2
* Attach policy: `xfusion-policy`
* Role name: `xfusion-role`

---

## Step 6 — Attach Role to EC2

EC2 → Actions → Security → Modify IAM Role

Attach:

```
xfusion-role
```

---

##  Step 7 — SSH into EC2

```bash
ssh -i ~/.ssh/id_rsa ubuntu@<EC2-PUBLIC-IP>
```

---

##  Step 8 — Test S3 Access

Create file:

```bash
echo "Hello from EC2 IAM Role" > test.txt
```

Upload to S3:

```bash
aws s3 cp test.txt s3://xfusion-s3-390964011108/
```

List files:

```bash
aws s3 ls s3://xfusion-s3-390964011108/
```

---

#  Result

✔ EC2 successfully accessed S3
✔ File uploaded successfully
✔ IAM Role authentication working
✔ No access keys used

---

#  Key Learning

* IAM Role = Temporary secure access
* IAM Policy = Permission rules
* EC2 uses role automatically
* S3 stores application data securely

---

#  Final Architecture

```
User → EC2 → IAM Role → IAM Policy → S3
```

---


