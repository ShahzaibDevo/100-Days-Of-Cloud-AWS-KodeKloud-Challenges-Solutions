# ☁️ Day 4: Enable S3 Bucket Versioning — AWS Cloud Migration Lab

> **Series:** Nautilus DevOps — AWS Cloud Migration (Incremental Steps)
> **Region:** `us-east-1`
> **Difficulty:** Beginner
> **Lab Time:** ~5 minutes

---


---

## 📚 Theory

### What is Amazon S3?

**Amazon S3 (Simple Storage Service)** is AWS's object storage service. It stores any type of file — images, videos, backups, logs, code — as **objects** inside **buckets**.

```
📦 S3 Bucket  =  A folder in the cloud
📄 Object     =  A file stored inside that bucket
🔑 Key        =  The file's unique name/path
```

S3 is **infinitely scalable**, **highly durable (99.999999999%)**, and one of the most widely used AWS services.

---

### What is S3 Versioning?

**S3 Versioning** is a feature that keeps **multiple versions of the same object** in a bucket. Every time a file is uploaded or modified, S3 saves the new version while keeping all previous versions intact.

Think of it like this:

```
📝 Google Docs "Version History"
   =
🪣 S3 Versioning
```

Every change is recorded. Nothing is truly lost.

---

### How Versioning Works

```
WITHOUT Versioning:
─────────────────────────────────────────
  Upload report.pdf (v1)   →  [report.pdf]
  Upload report.pdf (v2)   →  [report.pdf]  ← v1 is GONE forever 
  Delete report.pdf        →  NOTHING left  

WITH Versioning ENABLED:
─────────────────────────────────────────
  Upload report.pdf (v1)   →  [report.pdf - ID: aaa111]  
  Upload report.pdf (v2)   →  [report.pdf - ID: bbb222]  
                               [report.pdf - ID: aaa111]   still saved!
  Delete report.pdf        →  Adds a Delete Marker only  
                               Previous versions still recoverable! 
```

---

### Why Versioning Matters

| Scenario | Without Versioning | With Versioning |
|---|---|---|
| Accidental file delete | ❌ Gone forever | ✅ Recoverable |
| Wrong file overwrite | ❌ Old version lost | ✅ Rollback available |
| Ransomware / corruption | ❌ No recovery | ✅ Restore clean version |
| Compliance & audit | ❌ No history | ✅ Full change history |
| Developer mistakes | ❌ No undo | ✅ Instant rollback |

---

### Versioning States

An S3 bucket can be in one of **3 states**:

```
┌─────────────────────────────────────────────────────┐
│                  Versioning States                   │
├──────────────────┬──────────────────────────────────┤
│  Unversioned     │  Default state — no versions kept │
│  (default)       │  Old files overwritten/deleted    │
├──────────────────┼──────────────────────────────────┤
│  Enabled ✅      │  All versions saved automatically  │
│                  │  Delete adds a marker, not erases  │
├──────────────────┼──────────────────────────────────┤
│  Suspended       │  New uploads not versioned        │
│                  │  Old versions still preserved     │
└──────────────────┴──────────────────────────────────┘
```

> ⚠️ **Important:** Once versioning is **enabled**, it can be **suspended** but **never fully disabled**. Previous versions are always retained.

---

## 🎯 Lab Objective

Enable versioning on an existing S3 bucket:

| Parameter | Value |
|---|---|
| **S3 Bucket Name** | `devops-s3-13821` |
| **Task** | Enable Versioning |
| **Region** | `us-east-1` |

---

## ✅ Prerequisites

- Access to AWS Console
- IAM user with `s3:PutBucketVersioning` permission
- S3 bucket `devops-s3-13821` already exists (pre-created by lab)

---

## 🖥️ Steps: AWS Console (GUI)

### Step 1 — Log into the AWS Console

1. Open browser → navigate to the **AWS Console**
2. Enter your **IAM username and password**
3. Click **Sign In**

> ✅ Confirm top-right shows **US East (N. Virginia) — us-east-1**

---

### Step 2 — Navigate to S3

1. In the **top search bar** → type `S3`
2. Click **S3** from results

---

### Step 3 — Find the Bucket

1. In the bucket list → find **`devops-s3-13821`**
2. Click the bucket name to open it

> 💡 If you see "Bucket with same name already exists" — the bucket is **pre-created by the lab**. Just find it in the list and proceed!

---

### Step 4 — Go to Properties Tab

1. Click the **Properties** tab

```
Objects | Properties | Permissions | Metrics | Management
             ↑
         Click here
```

---

### Step 5 — Find Bucket Versioning

1. Scroll down to the **Bucket Versioning** section
2. Current status shows: **Disabled**
3. Click **Edit**

---

### Step 6 — Enable Versioning

1. Select **Enable** radio button
2. Click **Save changes**

> ✅ Green success banner appears — **"Bucket versioning successfully updated"**

---

## 🔍 Verification

### ✅ Verified CLI Output (Real Lab Result)

```bash
aws s3api get-bucket-versioning \
  --bucket devops-s3-13821 \
  --region us-east-1
```

```json
{
    "Status": "Enabled"
}
```

### ✅ Verification Checklist

```
[✅] Bucket Name    →  devops-s3-13821
[✅] Versioning     →  Enabled
[✅] Region         →  us-east-1
[✅] CLI Verified   →  Status: Enabled
```

---
  l              ///                      ///                    ///
   linkedin post


   Here's your final LinkedIn post for Day 4:

---

🚀 **#100DaysOfAWS — Day 4: The Cloud "Undo" Button** 🔄

What happens if a script accidentally deletes 1,000 production files?
**Without versioning** → it's a disaster 💀
**With versioning** → it's just another Tuesday ☕

Today for the **Nautilus DevOps** migration, I enabled **S3 Bucket Versioning** — one of the simplest yet most powerful data protection features in AWS.

---

🛠️ **What I Did**

✅ Located pre-existing bucket `devops-s3-13821` in `us-east-1`
✅ Enabled **Versioning** via AWS Console → Properties tab
✅ CLI Confirmed → `"Status": "Enabled"` ✅

---

💡 **What I Learned**

🔹 **The Delete Marker** — Deleting a versioned file doesn't erase it. AWS hides it behind a marker. Remove the marker → file is back in seconds!
🔹 **Overwrite Protection** — Re-uploading a file? AWS assigns a new **Version ID** instead of destroying the old one
🔹 **Once ON, never fully OFF** — Versioning can be suspended but never completely disabled
🔹 **Cost Awareness** — More versions = more storage bills 💰 Always pair with **S3 Lifecycle Policies** to auto-clean old versions

---
