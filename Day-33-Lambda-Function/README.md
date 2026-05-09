
# Day 33 — Create a Lambda Function (AWS Lambda)

## 📘 Lab Overview

The Nautilus DevOps Team started adopting **Serverless Architecture** to reduce infrastructure management and improve scalability.

In this lab, we created a simple **AWS Lambda function** that returns a custom greeting message to demonstrate serverless execution.

---

## What is AWS Lambda? 

**AWS Lambda** is a serverless compute service that allows you to run code without managing servers.

 No server provisioning  
 Automatic scaling  
 Pay only for execution time  
Event-driven execution  

You only upload code — AWS handles the infrastructure.

---

##  Lab Objectives

- Create Lambda Function → `xfusion-lambda`
- Runtime → **Python**
- Output Message → `Welcome to KKE AWS Labs!`
- Return Status Code → **200**
- Create IAM Role → `lambda_execution_role`
- Deploy using **AWS Console**

---

##  Step-by-Step Implementation

---

### Step 1 — Login to AWS Console
1. Open AWS Console.
2. Select Region: **us-east-1 (N. Virginia)**.
3. Search **Lambda** service.

---

### Step 2 — Create IAM Role

1. Go to **IAM → Roles**.
2. Click **Create Role**.
3. Choose:
   - Trusted entity → **AWS Service**
   - Use case → **Lambda**
4. Attach policy:
   - `AWSLambdaBasicExecutionRole`
5. Role Name:
```

lambda_execution_role

```
6. Click **Create Role**.

---

### Step 3 — Create Lambda Function

1. Open **Lambda → Functions**.
2. Click **Create Function**.
3. Choose:

- Author from scratch
- Function name:
```

xfusion-lambda

```
- Runtime:
```

Python 3.x

```
- Permissions:
```

Use existing role → lambda_execution_role

````

4. Click **Create Function**.

---

###  Step 4 — Add Function Code

Go to **Code** section and replace code with:

```python
def lambda_handler(event, context):
    return {
        "statusCode": 200,
        "body": "Welcome to KKE AWS Labs!"
    }
````

Click **Deploy**.

---

###  Step 5 — Create Test Event

1. Click **Test**.
2. Configure test event:

```
test-event
```

3. Save event.

---

###  Step 6 — Run Lambda Function

Click **Test** again.

---

###  Expected Output

You should see:

```json
{
  "statusCode": 200,
  "body": "Welcome to KKE AWS Labs!"
}
```

Execution status:

```
Executing function: succeeded 
```

---

## 🔎 Verification Checklist

✔ Lambda function created
✔ IAM execution role attached
✔ Code deployed successfully
✔ Test event executed
✔ Status Code = 200
✔ Greeting message returned

Lab Status → **Completed **

---

## Key Learning

* Serverless removes server management
* Lambda executes code on demand
* IAM roles control permissions securely
* CloudWatch automatically stores logs
* Ideal for APIs, automation, and DevOps workflows

---

## 🌍 Real-World Use Cases

* Backend APIs
* Automation scripts
* CI/CD triggers
* Image processing
* Scheduled jobs
* Event-driven microservices

---

## 📅 100 Days of Cloud — Progress

**Day 33 Complete ✅**
Learning serverless architecture step by step through hands-on AWS labs.

---

⭐ If this helped you, give the repo a star!

```
```
