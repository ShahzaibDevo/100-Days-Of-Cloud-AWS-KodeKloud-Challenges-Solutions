
# 🚀 Day 39: Hosting a Static Website on AWS S3

## 📌 Project Overview

This project demonstrates how to host a **static website** using **Amazon S3**.
The lab focuses on enabling public access, configuring static website hosting, and deploying an `index.html` file without using any servers.

---

# 🎯 Objective

* Create an S3 bucket
* Enable static website hosting
* Configure public access permissions
* Upload `index.html` file
* Access website via S3 endpoint

---

# 🧠 Architecture (Simple Flow)

```
index.html (Local / AWS Client)
        ↓
S3 Bucket Upload
        ↓
S3 Static Website Hosting
        ↓
Public Internet Access via S3 URL
```

---

# ⚙️ Prerequisites

* AWS Account (Lab credentials provided)
* AWS CLI access (AWS client machine)
* File: `/root/index.html`
* Region: `us-east-1`

---

# 🛠️ Step-by-Step Implementation

---

## ✅ Step 1: Login to AWS Console

Login using provided credentials and select region:

```
us-east-1 (N. Virginia)
```

---

## ✅ Step 2: Create S3 Bucket

Create a bucket with the following name:

```
datacenter-web-969113531
```

⚠️ Important:

* Uncheck **Block all public access**
* Confirm public access warning

---

## ✅ Step 3: Enable Static Website Hosting

Go to:

**Bucket → Properties → Static Website Hosting**

Configure:

* Enable hosting
* Index document:

```
index.html
```

Save changes.

---

## ✅ Step 4: Upload Website File

Upload file from AWS client machine:

```bash
aws s3 cp /root/index.html s3://datacenter-web-969113531/
```

---

## ✅ Step 5: Configure Bucket Policy (Public Access)

Go to:

**Permissions → Bucket Policy**

Add:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadAccess",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::datacenter-web-969113531/*"
    }
  ]
}
```

---

## ✅ Step 6: Get Website URL

Go to:

**Properties → Static Website Hosting**

Copy endpoint:

```
http://datacenter-web-969113531.s3-website-us-east-1.amazonaws.com
```

---

## ✅ Step 7: Verify Website

Open in browser:

```
http://datacenter-web-969113531.s3-website-us-east-1.amazonaws.com
```

Expected Output:

```
Welcome to KKE labs!
```

---

# 📊 Result

✔ S3 bucket created successfully
✔ Static website hosting enabled
✔ index.html uploaded
✔ Public access configured
✔ Website accessible via S3 URL

---

# 💡 Key Learnings

* Hosting static websites using serverless architecture
* Understanding S3 bucket policies
* Public access configuration in AWS
* Real-world use of S3 for frontend hosting
* AWS CLI file deployment

---

# 🚀 Conclusion

This lab demonstrates how **Amazon S3 can be used as a fully serverless web hosting solution**, removing the need for traditional web servers while ensuring scalability and availability.

---

# 🏷️ Tags

#AWS #S3 #DevOps #CloudComputing #StaticWebsite #Serverless #100DaysOfCloud #AWSProjects #LearningByDoing

---

If you want next upgrade, I can also:
✔ Add architecture diagram image
✔ Convert this into portfolio project
✔ Or create interview explanation script

