# Day 23: Data Migration Between S3 Buckets Using AWS CLI

## 📋 Objective

Migrate data from existing S3 bucket `datacenter-s3-24359` to new private S3 bucket `datacenter-sync-30951` using AWS CLI.

---

## 🎯 What You'll Do

1. Create new private S3 bucket
2. Migrate data from old bucket to new bucket
3. Verify data consistency
4. Confirm all data transferred

---

## 🛠️ Steps

### **Part 1: Create New S3 Bucket**

### Step 1: Configure AWS CLI (if not done)
```bash
aws configure
```

**Enter:**
- AWS Access Key ID: (from your credentials)
- AWS Secret Access Key: (from your credentials)
- Default region: `us-east-1`
- Default output format: `json`

### Step 2: Create new private S3 bucket
```bash
aws s3api create-bucket \
  --bucket datacenter-sync-30951 \
  --region us-east-1
```

**Output:**
```
{
    "Location": "http://datacenter-sync-30951.s3.amazonaws.com/"
}
```

### Step 3: Block public access
```bash
aws s3api put-public-access-block \
  --bucket datacenter-sync-30951 \
  --public-access-block-configuration \
  "BlockPublicAcls=true,IgnorePublicAcls=true,BlockPublicPolicy=true,RestrictPublicBuckets=true"
```

This makes the bucket **private** (secure)

### Step 4: Verify bucket created
```bash
aws s3 ls | grep datacenter-sync-30951
```

**Output:**
```
2026-04-27 10:00:00 datacenter-sync-30951
```

---

### **Part 2: Migrate Data**

### Step 5: Check source bucket contents
```bash
aws s3 ls s3://datacenter-s3-24359 --recursive
```

**Shows all files in source bucket**

### Step 6: Get count of source objects
```bash
aws s3 ls s3://datacenter-s3-24359 --recursive --summarize
```

**Shows total files and size**

### Step 7: Sync data from source to new bucket
```bash
aws s3 sync s3://datacenter-s3-24359 s3://datacenter-sync-30951
```

**Output:**
```
download: s3://datacenter-s3-24359/file1.txt to ../datacenter-sync-30951/file1.txt
download: s3://datacenter-s3-24359/file2.txt to ../datacenter-sync-30951/file2.txt
download: s3://datacenter-s3-24359/folder/file3.txt to ../datacenter-sync-30951/folder/file3.txt
...
```

**Wait for completion** - Takes time depending on data size

---

### **Part 3: Verify Data Consistency**

### Step 8: Count files in new bucket
```bash
aws s3 ls s3://datacenter-sync-30951 --recursive
```

**Compare with Step 5 output**

### Step 9: Get summary of new bucket
```bash
aws s3 ls s3://datacenter-sync-30951 --recursive --summarize
```

**Example output:**
```
Total Objects: 150
Total Size: 5.2 GB
```

### Step 10: Compare source and destination
```bash
# Source bucket
aws s3 ls s3://datacenter-s3-24359 --recursive --summarize | tail -2

# Destination bucket
aws s3 ls s3://datacenter-sync-30951 --recursive --summarize | tail -2
```

**Both should show same Total Objects and Total Size**

### Step 11: Verify specific file (optional)
```bash
# Check file in source
aws s3 ls s3://datacenter-s3-24359/filename

# Check file in destination
aws s3 ls s3://datacenter-sync-30951/filename
```

**Both should exist and have same size**

### Step 12: Final verification - sync again (dry run)
```bash
aws s3 sync s3://datacenter-s3-24359 s3://datacenter-sync-30951 --dryrun
```

**Output should be empty** - means all files already synced:
```
(dryrun) upload: ../file to s3://... 
```

**If nothing shows = Success! All data migrated.**

---

## ✅ Verification Checklist

- [x] New bucket `datacenter-sync-30951` created
- [x] Bucket is private (public access blocked)
- [x] Source bucket `datacenter-s3-24359` listed
- [x] Data synced from source to new bucket
- [x] Total objects count matches
- [x] Total size matches
- [x] All files present in destination
- [x] Dry run shows no pending transfers

---

## 📝 Quick Reference

| Item | Details |
|------|---------|
| Source bucket | `datacenter-s3-24359` |
| Destination bucket | `datacenter-sync-30951` |
| Region | `us-east-1` |
| Bucket type | Private (public access blocked) |
| Migration method | AWS CLI `s3 sync` |
| Verification | `s3 sync --dryrun` |

---


