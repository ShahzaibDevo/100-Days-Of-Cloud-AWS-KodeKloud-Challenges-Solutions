

#  Day 41: Encrypting and Decrypting Data Using AWS KMS

##  Project Overview

This project demonstrates how to securely encrypt and decrypt sensitive data using
**AWS Key Management Service (KMS)** in a real DevOps workflow.

We create a customer-managed KMS key, encrypt a file using AWS CLI, and decrypt it back to verify data integrity.

---

##  Objectives

* Create a **symmetric KMS key**
* Configure a **key alias**
* Encrypt sensitive data using AWS KMS
* Store encrypted output securely
* Decrypt data using the same key
* Validate original vs decrypted data integrity

---


### 🔄 Workflow

```
Sensitive Data File
        ↓
AWS KMS Encrypt Operation
        ↓
Encrypted Ciphertext (Base64)
        ↓
Stored as EncryptedData.bin
        ↓
AWS KMS Decrypt Operation
        ↓
Original Data Restored & Verified
```

---

##  Prerequisites

* AWS account with IAM permissions
* AWS CLI installed & configured
* Region: `us-east-1`
* Input file:


```bash
/root/SensitiveData.txt
```

---

##  Step-by-Step Implementation

---

### 1️ Verify AWS Identity

```bash
aws sts get-caller-identity
```

---

### 2️ Create KMS Key

```bash
aws kms create-key \
--description "devops-KMS-Key"
```

 Save the returned **KeyId**

---

### Create Key Alias

```bash
aws kms create-alias \
--alias-name alias/devops-KMS-Key \
--target-key-id <KEY_ID>
```

---

###  Encrypt the File

```bash
aws kms encrypt \
--key-id alias/devops-KMS-Key \
--plaintext fileb:///root/SensitiveData.txt \
--output text \
--query CiphertextBlob \
| base64 --decode > /root/EncryptedData.bin
```

---

### 5 Verify Encrypted File

```bash
ls -l /root/
```

---

###  Decrypt the File

```bash
aws kms decrypt \
--ciphertext-blob fileb:///root/EncryptedData.bin \
--output text \
--query Plaintext \
| base64 --decode > /root/DecryptedData.txt
```

---

###  Validate Integrity

```bash
diff /root/SensitiveData.txt /root/DecryptedData.txt
```

✔ No output = Success

---

##  Security Highlights

* Customer Managed Key (CMK)
* IAM-based access control
* Secure encryption/decryption workflow
* No manual key handling
* Audit-ready cloud security design

---

## Key Learnings

* AWS KMS envelope encryption
* Secure CLI-based encryption workflow
* Base64 encoding in cryptographic operations
* Real-world DevOps security implementation
* Data integrity verification techniques

---

##  Final Outcome

✔ KMS Key Created
✔ File Successfully Encrypted
✔ Ciphertext Stored Securely
✔ File Decrypted Successfully
✔ Data Integrity Verified

---


##  Conclusion

This lab demonstrates how DevOps engineers can implement **secure, scalable, and production-grade encryption workflows** using AWS-native services.

---
