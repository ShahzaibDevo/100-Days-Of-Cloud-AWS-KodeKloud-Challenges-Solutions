# ☁️ Day 3: Create Subnet — AWS Cloud Migration Lab


### What is a Subnet?

A **Subnet (Sub-Network)** is a **logical subdivision of a VPC's IP address range**. It allows you to partition your Virtual Private Cloud into smaller, isolated segments where you can place AWS resources like EC2 instances, RDS databases, and load balancers.

Think of it like this:

```
🏢 VPC = An entire Office Building
🚪 Subnet = Individual Floors or Rooms inside that building
💻 EC2 Instance = A desk placed inside one of those rooms
```

Every resource in AWS must live inside a **subnet**, which in turn lives inside a **VPC**.

---

### How Subnets Work

```
┌─────────────────────────────────────────────────┐
│           Default VPC (172.31.0.0/16)            │
│                                                   │
│  ┌──────────────────┐  ┌──────────────────────┐  │
│  │  datacenter-subnet│  │   Other Subnet       │  │
│  │  (us-east-1a)    │  │   (us-east-1b)       │  │
│  │                  │  │                      │  │
│  │  [EC2] [RDS]     │  │  [EC2] [Lambda]      │  │
│  └──────────────────┘  └──────────────────────┘  │
│                                                   │
└─────────────────────────────────────────────────┘
```

| Feature | Details |
|---|---|
| **Scope** | Tied to a single Availability Zone (AZ) |
| **IP Range** | A subset of the parent VPC CIDR block |
| **Types** | Public (internet-accessible) or Private (internal only) |
| **Route Table** | Each subnet is associated with a route table |
| **Default Subnet** | AWS auto-creates one subnet per AZ in the default VPC |

---

### Public vs Private Subnet

| Feature | Public Subnet | Private Subnet |
|---|---|---|
| **Internet Access** | Yes (via Internet Gateway) | No direct access |
| **Use Case** | Web servers, Load Balancers | Databases, App servers |
| **Route Table** | Has route to `0.0.0.0/0` via IGW | No IGW route |
| **IP Assignment** | Auto-assigns public IP | Private IP only |
| **Example** | `datacenter-subnet` (this lab) | Backend DB subnet |

---

### CIDR & IP Ranges in Subnets

When you create a subnet, you assign it a **CIDR block** — a portion of the parent VPC's IP range.

**Default VPC CIDR:** `172.31.0.0/16` → gives **65,536 IP addresses**

| Subnet CIDR | Usable IPs | Notes |
|---|---|---|
| `172.31.0.0/20` | 4,091 | Default subnet size AWS creates |
| `172.31.0.0/24` | 251 | Common custom subnet |
| `172.31.0.0/28` | 11 | Small subnet |

> 💡 **Note:** AWS reserves **5 IP addresses** in every subnet (first 4 + last 1), so a `/24` gives 256 − 5 = **251 usable IPs**.

**Reserved IPs Example for `172.31.0.0/24`:**

| IP | Reserved For |
|---|---|
| `172.31.0.0` | Network address |
| `172.31.0.1` | VPC router |
| `172.31.0.2` | DNS server |
| `172.31.0.3` | Future AWS use |
| `172.31.0.255` | Broadcast address |

---

### Subnet vs VPC vs Availability Zone

```
AWS Region: us-east-1
│
├── Availability Zone: us-east-1a
│   └── Subnet: datacenter-subnet  ← Lives HERE
│       └── EC2 Instance
│
├── Availability Zone: us-east-1b
│   └── Subnet: another-subnet
│
└── VPC: Default VPC (172.31.0.0/16)  ← Spans ALL AZs
```

| Component | Scope | Contains |
|---|---|---|
| **Region** | Geographic area | Multiple AZs |
| **VPC** | Spans entire region | Multiple Subnets |
| **Availability Zone** | Single data center | Multiple Subnets |
| **Subnet** | Single AZ only | EC2, RDS, etc. |

---

## 🎯 Lab Objective

Create a Subnet under the **Default VPC** in `us-east-1`:

| Parameter | Value |
|---|---|
| **Subnet Name** | `datacenter-subnet` |
| **VPC** | Default VPC |
| **Region** | `us-east-1` |

---

## ✅ Prerequisites

- Access to AWS Console
- IAM user with `ec2:CreateSubnet` permission
- Default VPC must exist in `us-east-1`

---

## 🖥️ Steps: AWS Console (GUI)

### Step 1 — Log into the AWS Console

1. Open your browser and navigate to:
   ```
   https://471339402762.signin.aws.amazon.com/console?region=us-east-1
   ```
2. Enter your credentials:
   - **IAM Username:** `kk_labs_user_345448`
   - **Password:** `0zlfwOU^AOgC`
3. Click **Sign In**

> ✅ Confirm the region in the top-right corner shows **US East (N. Virginia) us-east-1**

---

### Step 2 — Navigate to Subnets

1. In the **top search bar**, type `VPC` and press Enter
2. Click **VPC** from the results to open the VPC Dashboard
3. In the **left navigation panel**, click **Subnets**

```
VPC Dashboard
└── Virtual Private Cloud
    └── Subnets   ◄── Click here
```

---

### Step 3 — Start Creating the Subnet

1. Click the orange **Create subnet** button in the top-right corner

---

### Step 4 — Select the VPC

In the **VPC ID** dropdown:
- Select the **Default VPC**



---

### Step 5 — Fill in Subnet Details

Scroll down to the **Subnet settings** section and fill in:

| Field | Value |
|---|---|
| Subnet name | `datacenter-subnet` |
| Availability Zone | Select any (e.g. `us-east-1a`) |
| IPv4 subnet CIDR block | Enter a valid range e.g. `172.31.128.0/20` |

> 💡 Make sure the CIDR block you enter **does not overlap** with existing subnets in the VPC. You can check existing subnets on the Subnets list page before creating.

---

### Step 6 — Create the Subnet

1. Click the orange **Create subnet** button at the bottom
2. You will be redirected to the Subnets list page
3. Confirm `datacenter-subnet` appears with status **Available** ✅

---

## 🔍 Verification

After creation, confirm the following in the AWS Console:

Navigate to **VPC → Subnets** and click on `datacenter-subnet`:

### ✅ Verification Checklist

```
[ ] Subnet name       →  datacenter-subnet
[ ] VPC               →  Default VPC (172.31.0.0/16)
[ ] Region            →  us-east-1
[ ] State             →  Available
[ ] CIDR block        →  Valid non-overlapping range
```

### Expected Console View

```
Subnet: datacenter-subnet

┌──────────────────────┬──────────────────────────┐
│ Subnet ID            │ subnet-0xxxxxxxxxxxxxxxxx │
│ State                │ Available                 │
│ VPC                  │ vpc-xxxxxxxxx (default)   │
│ IPv4 CIDR            │ 172.31.128.0/20           │
│ Availability Zone    │ us-east-1a                │
│ Available IPs        │ 4091                      │
└──────────────────────┴──────────────────────────┘
```

---

## 💡 Key Takeaways

- A **Subnet** is a logical partition of a VPC's IP address space
- Every subnet lives inside a **single Availability Zone** — it cannot span multiple AZs
- The **Default VPC** already has default subnets — we created a custom one: `datacenter-subnet`
- AWS **reserves 5 IP addresses** in every subnet, so plan your CIDR accordingly
- **Public subnets** route traffic to an Internet Gateway; **private subnets** do not
- Subnets are the foundation for placing EC2 instances, RDS, and other resources
- Good subnet design = better **security**, **high availability**, and **cost control**

---
