# 🔒 Day 9: Enable Termination Protection for EC2 Instance

---

## 📌 Scenario

The Nautilus DevOps team created an EC2 instance **`xfusion-ec2`** in `us-east-1` as part of an active cloud migration — but forgot to enable **Termination Protection**. Without it, the instance could be accidentally deleted at any point, causing irreversible data loss during the migration window.

**Objective:** Enable Termination Protection on `xfusion-ec2` so it cannot be terminated via Console, CLI, or API.

---

## 📚 Theory

### What is Termination Protection?

Termination Protection is an EC2 instance-level attribute (`disableApiTermination`) that, when set to `true`, blocks any attempt to permanently delete the instance — whether from the AWS Console, AWS CLI, or any SDK.

It acts as a safety lock. The instance can still be stopped or rebooted normally — only the **Terminate** action is blocked until protection is explicitly removed.

### Stop vs Terminate vs Reboot

| Action | What Happens | Reversible? |
|--------|-------------|-------------|
| Reboot | Restarts OS, IP preserved | ✅ Yes |
| Stop | Powers down, EBS volumes kept | ✅ Yes |
| **Terminate** | **Permanently destroys instance + root EBS** | ❌ No |

### How it works under the hood

When a `terminate-instances` request arrives at the EC2 API, the control plane checks the `disableApiTermination` flag before processing:

```
User calls Terminate
        ↓
EC2 API checks disableApiTermination
        ↓
   Value = true  ──►  OperationNotPermitted (blocked)
   Value = false ──►  Instance permanently deleted
```

### Key Facts

| Property | Detail |
|----------|--------|
| Default state | OFF — must be explicitly enabled |
| Applies to | Terminate action only (not Stop or Reboot) |
| IAM permission needed | `ec2:ModifyInstanceAttribute` |
| Cost | Free — no extra charge |
| Scope | Per-instance setting |

---

## 🖥️ Step-by-Step — AWS Console (UI)

### Step 1 — Sign in to AWS Console

Open the console and log in with your IAM credentials:

```
URL:      https://152233226911.signin.aws.amazon.com/console?region=us-east-1
Username: kk_labs_user_745790
Password: WHx14i1bRU%t
```

Confirm the region shows **US East (N. Virginia) — us-east-1** in the top-right corner.

---

### Step 2 — Navigate to EC2

1. Click the **search bar** at the top of the AWS Console
2. Type `EC2`
3. Click **EC2** from the dropdown results

---

### Step 3 — Open Instances list

In the left sidebar of the EC2 Dashboard, click:

```
EC2 Dashboard
  └── Instances
        └── Instances   ← click here
```

---

### Step 4 — Select xfusion-ec2

1. Find the instance named **`xfusion-ec2`** in the list
2. Click the **checkbox** to the left of the instance name
3. The row will highlight — the Details pane below will show:
   ```
   Termination protection: Disabled
   ```

---

### Step 5 — Open Actions → Instance Settings

With the instance selected, click the **Actions** button at the top of the table:

```
Actions
  └── Instance Settings
        └── Change Termination Protection   ← click here
```

---

### Step 6 — Enable and Save

A dialog box appears:

1. Select the **Enable** radio button
2. Click **Save**

---

### Step 7 — Verify in Console

Click on the instance name to open the detail pane → go to the **Details** tab and confirm:

```
Termination protection: Enabled ✅
```

---




## 💡 Key Takeaways

- **Default is OFF** — termination protection must be explicitly enabled on every critical instance
- **Only blocks Terminate** — Stop, Reboot, and OS-level shutdown are unaffected
- **IAM-gated** — only users with `ec2:ModifyInstanceAttribute` can toggle this setting
- **Best practice** — always enable on production, stateful, and migration-critical instances
- **Add to Launch Templates** — automate it so new instances are protected from day one
- **Free feature** — zero cost, zero operational overhead

---
