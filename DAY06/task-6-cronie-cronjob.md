**🌟 Task 6 - Install Cronie and Create Sample Cron Job**

**📌 Task Description**

The **Nautilus system admins team** has prepared scripts to automate several day-to-day tasks. They want them to be deployed on all app servers in **Stratos Datacenter** on a set schedule. Before that, they need to test similar functionality with a sample cron job.

**Steps to perform:**

a. Install **cronie package** on all Nautilus app servers and start **crond service**

b. Add a cron job to run every 5 minutes: `*/5 * * * * echo hello > /tmp/cron_text` for the **root user**

👉 **Your task:** Install cronie and configure the sample cron job on **all Nautilus app servers**.

💡 **Note:** You can find the infrastructure details by clicking on the **Details of all Users and Servers** button on the top-right section of the page.

---

## 🚨 Task-Specific Challenge & Solution

**🔍 Main Challenge Encountered:**

The primary challenge was **implementing automated task scheduling** using cron across multiple servers while ensuring the cronie service was properly installed, configured, and executing scheduled jobs reliably.

**💡 Solution Approach:**

1. **Service Installation**: Installed `cronie` package to provide cron daemon functionality across all Nautilus app servers
2. **Service Management**: Started and enabled `crond` service to ensure automatic startup and continuous operation
3. **Cron Job Configuration**: Created a precise cron expression `*/5 * * * * echo hello > /tmp/cron_text` for 5-minute interval execution
4. **Root User Implementation**: Used `sudo crontab -e` to configure the cron job under root user as specified
5. **Verification Process**: Used `crontab -l` and file system checks to confirm job scheduling and execution

**🎯 Key Success Factors:**
- **Cron syntax precision** ensuring correct time interval specification (*/5 for every 5 minutes)
- **Service persistence** through systemctl enable for automatic startup after reboots
- **Output redirection** to create verifiable execution evidence in `/tmp/cron_text`
- **Multi-server deployment** ensuring consistent automation across infrastructure

**⚠️ Critical Learning Points:**
- **Cron expression syntax** requires precise field positioning for proper scheduling
- **Service dependencies** - crond must be running for cron jobs to execute
- **User context matters** - root crontab vs user crontab have different capabilities
- **Output management** is essential for monitoring and verifying automated task execution

---

## 🔹 Step 1: Check OS version on all servers

```bash
cat /etc/os-release
```

**Purpose**: Check OS version on all servers to ensure compatibility.

**Servers to configure**: Apply these steps on **all Nautilus app servers** (app01, app02, app03).

---

## 🔹 Step 2: Install cronie package

```bash
sudo yum install cronie -y
```

**Purpose**: Install cronie package for cron jobs.

**For Ubuntu/Debian systems** (alternative):
```bash
sudo apt-get install cron -y
```

**Package Details**: 📦 Cronie provides the cron daemon and utilities for scheduling tasks.

---

## 🔹 Step 3: Check crond service status

```bash
sudo systemctl status crond
```

**Purpose**: Check status of crond service.

**Expected Status**: 
- ✅ **Active (running)** - Service is running properly
- ❌ **Inactive (dead)** - Service needs to be started

---

## 🔹 Step 4: Start the crond service

```bash
sudo systemctl start crond
```

**Purpose**: Start the crond service if not running.

**Additional service commands**:
```bash
# Enable service to start on boot
sudo systemctl enable crond

# Check service status again
sudo systemctl status crond
```

---

## 🔹 Step 5: Open root user's crontab for editing

```bash
sudo crontab -e
```

**Purpose**: Open root user's crontab for editing.

**Editor Note**: 📝 This will open the default editor (usually vi/nano). If first time, it may ask you to choose an editor.

---

## 🔹 Step 6: Add the cron job entry

**Add the following line to the crontab:**

```bash
*/5 * * * * echo hello > /tmp/cron_text
```

**Purpose**: Schedule a job to run every 5 minutes that writes "hello" to `/tmp/cron_text`.

**Cron Format Explanation**:
```
*/5  *  *  *  *  command
 │   │  │  │  │
 │   │  │  │  └─── Day of week (0-7, Sunday = 0 or 7)
 │   │  │  └────── Month (1-12)
 │   │  └───────── Day of month (1-31)
 │   └──────────── Hour (0-23)
 └─────────────── Minute (*/5 = every 5 minutes)
```

**Save and exit**: 
- **In vi**: Press `Esc`, type `:wq`, press `Enter`
- **In nano**: Press `Ctrl+X`, then `Y`, then `Enter`

---

## 🔹 Step 7: Verify the cron job is scheduled

```bash
sudo crontab -l
```

**Purpose**: Verify the cron job is scheduled correctly.

**Expected Output**:
```
*/5 * * * * echo hello > /tmp/cron_text
```

---

## 🔹 Step 8: Check the cron job output

```bash
cd /tmp
ls -l cron_text
```

**Purpose**: Check the cron job output file and permissions.

**Wait time**: ⏱️ You may need to wait up to 5 minutes for the first execution.

**Expected Output** (after job runs):
```bash
# Check if file exists and view contents
cat /tmp/cron_text
# Should display: hello
```

---

## 📋 Quick Command Reference

For quick copy-paste, here are all commands in sequence:

```bash
# Check OS version
cat /etc/os-release

# Install cronie package
sudo yum install cronie -y

# Check and start crond service
sudo systemctl status crond
sudo systemctl start crond
sudo systemctl enable crond

# Edit crontab (add: */5 * * * * echo hello > /tmp/cron_text)
sudo crontab -e

# Verify cron job
sudo crontab -l

# Check output file
ls -l /tmp/cron_text
cat /tmp/cron_text
```

---

## 💡 Additional Tips

- **View cron logs**: `sudo tail -f /var/log/cron`
- **Remove all cron jobs**: `sudo crontab -r`
- **Edit specific user's crontab**: `sudo crontab -u username -e`
- **Test cron syntax**: Use online cron expression validators
- **Common cron examples**:
  - Every minute: `* * * * *`
  - Daily at midnight: `0 0 * * *`
  - Weekly on Sunday: `0 0 * * 0`

---

## ⚠️ Important Reminders

🔄 **Repeat on all servers**: Apply these steps on **all Nautilus app servers** in the Stratos Datacenter

📝 **Cron job testing**: Wait 5 minutes after setup to verify the job executes successfully

🔒 **Service persistence**: Enable crond service to start automatically on boot