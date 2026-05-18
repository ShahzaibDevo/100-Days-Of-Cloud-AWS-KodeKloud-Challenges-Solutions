
#  Day 42: Building and Managing NoSQL Databases with Amazon DynamoDB

##  Overview

This project is part of my **100 Days of Cloud (AWS) journey**.  
In this lab, I built and managed a simple NoSQL database using **Amazon DynamoDB** to store task data for a To-Do application.

---

## 📖 What is Amazon DynamoDB?

**Amazon DynamoDB** is a fully managed **NoSQL database service** provided by AWS.  
It stores data in **key-value and document format** and is designed for:

- High performance at any scale
- Low latency data access
- Fully serverless architecture
- Automatic scaling and high availability

###  Key Features:
- No server management required
- Schema-less database design
- Built-in security and backup options
- Millisecond-level response time
- Auto scaling for traffic spikes

---

## Project Objective

Create a DynamoDB table to manage a simple **To-Do application**, where each task has:

- A unique Task ID
- Task Description
- Task Status (completed / in-progress)

---

## ⚙️ AWS Services Used

- Amazon DynamoDB

---

## 🧱 Table Design

| Attribute     | Type   | Description              |
|--------------|--------|--------------------------|
| taskId       | String | Primary Key (Unique ID)  |
| description   | String | Task details             |
| status        | String | Task progress status     |

---

##  Step-by-Step Implementation

### Step 1: Create DynamoDB Table
1. Open AWS Management Console
2. Navigate to **DynamoDB**
3. Click **Create table**
4. Enter details:
   - Table name: `nautilus-tasks`
   - Partition key: `taskId` (String)
5. Click **Create table**

---

### Step 2: Add Items to Table

Go to **Explore items → Create item (JSON view)**

####  Task 1
```json
{
  "taskId": "1",
  "description": "Learn DynamoDB",
  "status": "completed"
}
````

#### Task 2

```json
{
  "taskId": "2",
  "description": "Build To-Do App",
  "status": "in-progress"
}
```

---

### Step 3: Verify Data

Run a scan operation in DynamoDB:

```bash

aws dynamodb scan --table-name nautilus-tasks
```

You should see:

* Total Items: 2
* Correct task details stored

---

##  Expected Output

* Table created successfully
* 2 tasks inserted
* Data retrieved using scan operation
* Correct status values verified

---

## 🎯 Key Learnings

* Understanding NoSQL database concepts
* Working with DynamoDB tables and primary keys
* Performing CRUD operations in AWS
* Hands-on experience with serverless databases
* Real-world To-Do app backend design

---

##  Conclusion

This lab helped me understand how AWS DynamoDB simplifies database management by removing server overhead and providing a highly scalable NoSQL solution.

---
