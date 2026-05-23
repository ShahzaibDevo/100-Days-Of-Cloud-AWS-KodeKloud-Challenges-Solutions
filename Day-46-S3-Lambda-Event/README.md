
#  Day 46: Event-Driven Processing with Amazon S3 and Lambda

---

## 🧠 Objective

Build a **serverless event-driven pipeline** where:

* File upload in S3 triggers Lambda automatically
* Lambda processes the event
* File is copied to another S3 bucket
* Logs are stored in DynamoDB

---

# 🧠 Simple Theory (IMPORTANT)



## 🪣 What is Amazon S3?

Amazon S3 is a scalable object storage service used to store files like images, logs, backups, and application data. It also supports event triggers when files are uploaded.

---

## ⚡ What is AWS Lambda?

AWS Lambda is a serverless compute service that runs code automatically in response to events like file uploads, API calls, or database changes.

---

## 🗄️ What is DynamoDB?

Amazon DynamoDB is a fast, fully managed NoSQL database used to store structured data like logs, metadata, and application records.

---

## 💡 What is Event-Driven Architecture?

It is a system where:

* One service generates an event (S3 upload)
* Another service reacts automatically (Lambda)
* No manual intervention required

---

# 🏗️ Architecture Flow

```text id="flow1"
User Uploads File
        ↓
S3 Public Bucket (datacenter-public-15676)
        ↓ (Event Trigger)
Lambda Function (datacenter-copyfunction)
        ↓
S3 Private Bucket (datacenter-private-18753)
        ↓
DynamoDB (datacenter-S3CopyLogs)
```

---

#  STEP 1 — CREATE PUBLIC S3 BUCKET (UI)

Go to S3 Console → Create bucket

### Settings:

* Bucket name:

```
datacenter-public-15676
```

* Region:

```
us-east-1
```

### Permissions:

❌ Uncheck **Block all public access**
✔ Confirm warning checkbox

👉 Create bucket

---

#  STEP 2 — CREATE PRIVATE S3 BUCKET

* Bucket name:

```
datacenter-private-18753
```

### Permissions:

✔ Keep Block Public Access ENABLED

👉 Create bucket

---

#  STEP 3 — CREATE DYNAMODB TABLE

* Table name:

```
datacenter-S3CopyLogs
```

* Partition key:

```
LogID (String)
```

👉 Create table

---

#  STEP 4 — CREATE IAM ROLE FOR LAMBDA

Create Role → AWS Service → Lambda

Attach policies:

* AmazonS3FullAccess
* AmazonDynamoDBFullAccess
* AWSLambdaBasicExecutionRole

Role name:

```
lambda_execution_role
```

---

#  STEP 5 — CREATE LAMBDA FUNCTION

* Function name:

```
datacenter-copyfunction
```

* Runtime:

```
Python 3.x
```

* Role:

```
lambda_execution_role
```

👉 Create function

---

#  STEP 6 — UPDATE LAMBDA CODE

Open file:

```
/root/lambda-function.py
```

---

## Replace:

### ❌ Old:

```python
REPLACE-WITH-YOUR-DYNAMODB-TABLE
```

### ✅ New:

```python
datacenter-S3CopyLogs
```

---

### ❌ Old:

```python
REPLACE-WITH-YOUR-PRIVATE-BUCKET
```

### ✅ New:

```python
datacenter-private-18753
```

---

👉 Click Deploy

---

#  STEP 7 — ADD S3 TRIGGER

Inside Lambda:

* Add Trigger → S3
* Bucket: `datacenter-public-15676`
* Event: ObjectCreated (PUT)

👉 Enable trigger → Add

---

#  STEP 8 — TEST UPLOAD

```bash
aws s3 cp sample.zip s3://datacenter-public-15676/
```

---

#  STEP 9 — VERIFY OUTPUT

## ✔ Private Bucket

```bash
aws s3 ls s3://datacenter-private-18753/
```

Expected:

```
sample.zip
```

---

## ✔ DynamoDB Logs

```bash
aws dynamodb scan --table-name datacenter-S3CopyLogs
```

---

# 🎯 FINAL RESULT

✔ File uploaded to public S3
✔ Lambda triggered automatically
✔ File copied to private S3
✔ Logs stored in DynamoDB

---

# ⚡ KEY CONCEPTS

* ✔ Serverless Architecture
* ✔ Event-driven System
* ✔ Decoupled Services
* ✔ Auto-scaling Workflow

---
