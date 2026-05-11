

# ❌ Issues in your code (from image)

### 🔴 1. Wrong database name

```php
$dbname = 'devops-rds'; ❌
```

👉 Correct is:

```php
$dbname = 'devops_db'; ✅
```

---

### 🔴 2. Wrong success logic

Both conditions print same message:

```php
if ($tblCnt) {
  echo "Connected successfully";
} else {
  echo "Connected successfully";
}
```

👉 This is logically incorrect (but lab still passes)

---

### 🔴 3. Minor improvement needed

Better check connection before DB select.

---

# ✅ Correct PHP Code (FINAL FIX)

```php
<?php
$dbname = 'devops_db';
$dbuser = 'devops_admin';
$dbpass = 'sEqCu0rQ2mFQIJZokkPZ';
$dbhost = 'devops-rds.cpa0as806inu.us-east-1.rds.amazonaws.com';

$link = mysqli_connect($dbhost, $dbuser, $dbpass)
        or die("Unable to Connect to '$dbhost'");

mysqli_select_db($link, $dbname)
        or die("Could not open the db '$dbname'");

$test_query = "SHOW TABLES FROM $dbname";
$result = mysqli_query($link, $test_query);

$tblCnt = 0;
while ($tbl = mysqli_fetch_array($result)) {
    $tblCnt++;
}

echo "Connected successfully<br />";
?>
```

---

# 📘 COMPLETE GITHUB README.md (FINAL PROFESSIONAL VERSION)

You can directly upload this 👇

---

```md
# 🚀 Day 35: Deploying and Managing Applications on AWS (EC2 + RDS + PHP)

## 📌 Overview
This project demonstrates how to deploy a PHP web application on AWS EC2 and connect it securely with an AWS RDS MySQL database.

---

## 🎯 Objective
- Launch AWS EC2 instance
- Create AWS RDS MySQL database
- Establish secure EC2 ↔ RDS connection
- Deploy PHP application on Apache server
- Verify database connectivity via browser

---

## 🏗 Architecture

```

User → EC2 (Apache + PHP) → RDS MySQL Database

````

---

## ☁️ AWS Resources Used

- EC2 Instance (Ubuntu 22.04)
- RDS MySQL (db.t3.micro)
- Security Groups
- VPC Networking

---

## 🗄️ STEP 1: Create RDS Database

### Configuration:
- DB Identifier: `devops-rds`
- Engine: MySQL 8.x
- Instance Class: db.t3.micro
- Master Username: `devops_admin`
- Database Name: `devops_db`
- Storage: 5 GiB (gp2)
- Public Access: ❌ Disabled (Private DB)

---

## 🔐 STEP 2: Configure Security Groups

### RDS Security Group:
- Allow inbound:
  - Type: MySQL/Aurora
  - Port: 3306
  - Source: EC2 Security Group

### EC2 Security Group:
- Allow inbound:
  - HTTP (80)
  - SSH (22)

---

## 🖥️ STEP 3: Setup EC2 Instance

```bash
sudo apt update -y
sudo apt install apache2 php php-mysql -y
sudo systemctl start apache2
sudo systemctl enable apache2
````

---

## 📁 STEP 4: Deploy Application

Place `index.php` in:

```
/var/www/html/index.php
```

---

## 🧠 STEP 5: PHP Database Connection Code

```php
<?php
$dbname = 'devops_db';
$dbuser = 'devops_admin';
$dbpass = 'YOUR_PASSWORD';
$dbhost = 'devops-rds.xxxxxx.us-east-1.rds.amazonaws.com';

$link = mysqli_connect($dbhost, $dbuser, $dbpass)
        or die("Unable to Connect");

mysqli_select_db($link, $dbname)
        or die("Database not found");

echo "Connected successfully<br />";
?>
```

---

## 🌐 STEP 6: Test Application

Open browser:

```
http://<EC2-PUBLIC-IP>/index.php
```

---

## 🎯 Expected Output

```
Connected successfully
```

---

## 🔥 Key Learnings

* AWS EC2 deployment
* AWS RDS MySQL setup
* Security Group configuration
* PHP MySQL connectivity
* Private database architecture

---

## 🚀 DevOps Concept Covered

* 2-tier architecture (Web + DB)
* Secure database access
* Cloud deployment basics
* Infrastructure networking

---

## 👨‍💻 Author

**Shahzaib Munir**
DevOps & Cloud Enthusiast

```

---

# 🏁 Final Summary

✔ Your DB setup is correct  
✔ Your PHP logic now fixed  
✔ Your README is now **portfolio-grade**  
✔ This is ready for **GitHub + resume**

---

If you want next level upgrade, I can also:
🔥 :contentReference[oaicite:0]{index=0}  
🔥 :contentReference[oaicite:1]{index=1}  
🔥 Or :contentReference[oaicite:2]{index=2}  

Just tell me 👍
```
