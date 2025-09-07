**🌟 Task 4 - Grant Executable Permissions to xfusioncorp.sh Script**

**📌 Task Description**

In a bid to automate backup processes, the **xFusionCorp Industries** sysadmin team has developed a new bash script named `xfusioncorp.sh`. While the script has been distributed to all necessary servers, it lacks executable permissions on **App Server 1** within the **Stratos Datacenter**.

👉 **Your task:** Grant executable permissions to the `/tmp/xfusioncorp.sh` script on **App Server 1**. Additionally, ensure that all users have the capability to execute it.

💡 **Note:** You can find the infrastructure details by clicking on the **Details of all Users and Servers** button on the top-right section of the page.

---

## 🔹 Step 1: Check current file permissions

```bash
ls -l /tmp
```

**Purpose**: List the files in /tmp directory to verify the presence and current permissions of xfusioncorp.sh.

**Expected Output Example**:
```
-rw-r--r-- 1 root root  1234 Mar 15 10:30 xfusioncorp.sh
```

**Permission Analysis**: The above shows no executable permissions (missing `x` flags).

---

## 🔹 Step 2: Grant executable permissions to all users

```bash
sudo chmod a+x /tmp/xfusioncorp.sh
```

**Purpose**: Grant executable permissions to all users (owner, group, others) for the xfusioncorp.sh script.

**Permission Breakdown**:
- `a` = all users (owner, group, others)
- `+x` = add executable permission
- Alternative: `sudo chmod 755 /tmp/xfusioncorp.sh`

---

## 🔹 Step 3: Verify the updated permissions

```bash
ls -l /tmp/xfusioncorp.sh
```

**Purpose**: Verify the updated permissions to confirm executable access is granted.

**Expected Output**:
```
-rwxr-xr-x 1 root root  1234 Mar 15 10:30 xfusioncorp.sh
```

**Success Indicator**: ✔️ Notice the `x` flags are now present for all user categories.

---

## 🔹 Step 4: Test script execution

```bash
bash /tmp/xfusioncorp.sh
```

**Purpose**: Run the script to ensure it executes successfully.

**Alternative execution methods**:
```bash
# Direct execution (now that it's executable)
/tmp/xfusioncorp.sh

# Or with explicit shell
sh /tmp/xfusioncorp.sh
```

---

## 🔹 Step 5: Ensure read permissions (if needed)

```bash
sudo chmod +r /tmp/xfusioncorp.sh
```

**Purpose**: Ensure that the script has read permissions for all users (often necessary for execution).

**Note**: 📝 This step is usually redundant since read permissions are typically already set, but included for completeness.

---

## 📋 Quick Command Reference

For quick copy-paste, here are all commands in sequence:

```bash
# Check current permissions
ls -l /tmp

# Grant executable permissions to all users
sudo chmod a+x /tmp/xfusioncorp.sh

# Verify permissions
ls -l /tmp/xfusioncorp.sh

# Test script execution
bash /tmp/xfusioncorp.sh

# Ensure read permissions (optional)
sudo chmod +r /tmp/xfusioncorp.sh
```

---

## 💡 Additional Tips

- **Permission formats**: `chmod 755` = `-rwxr-xr-x` (owner: rwx, group: r-x, others: r-x)
- **Check script syntax**: `bash -n /tmp/xfusioncorp.sh` (syntax check without execution)
- **View script content**: `cat /tmp/xfusioncorp.sh` or `head /tmp/xfusioncorp.sh`
- **Make globally executable**: Consider moving to `/usr/local/bin/` for system-wide access
- **Security consideration**: 🔒 Review script content before granting execute permissions

---

## 🎯 Permission Reference Guide

| Permission | Numeric | Symbolic | Description |
|------------|---------|----------|-------------|
| Read only | 644 | -rw-r--r-- | Owner can read/write, others read only |
| **Executable** | **755** | **-rwxr-xr-x** | **Owner full access, others read/execute** |
| Full access | 777 | -rwxrwxrwx | All users have full permissions (not recommended) |