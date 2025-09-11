# 🌟 Task 1 - Create User with Non-Interactive Shell  

## 📌 Task Description  
To accommodate the **backup agent tool's** specifications, the system admin team at *xFusionCorp Industries* requires the creation of a user with a **non-interactive shell**.  

👉 Your task: Create a user named **rose** with a non-interactive shell on **App Server 2**.  

> 💡 **Note:** You can find the infrastructure details by clicking on the **Details of all Users and Servers** button on the top-right section of the page.

---

## ✅ Solution with Command Purposes  

# Creating User 'rose' - Step by Step Guide

## 🔹 Step 1: Switch to root user for administrative privileges

```bash
sudo su -
```

> **Note**: This command switches you to the root user with full administrative privileges.

---

## 🔹 Step 2: Verify if the user 'rose' already exists

```bash
cat /etc/passwd | grep rose
```

**Expected Result**: 
- ✔️ If **nothing is returned**, the user does not exist yet
- ❌ If user information appears, the user already exists

---

## 🔹 Step 3: Create a new user 'rose' with a non-interactive shell

```bash
sudo useradd rose --shell /sbin/nologin
```

**Security Note**: 🔒 The `/sbin/nologin` shell ensures that the user cannot log in interactively, making it suitable for service accounts.

---

## 🔹 Step 4 (Optional): Verify the user creation

**Option 1** - Check user ID and groups:
```bash
id rose
```

**Option 2** - Check user entry in passwd file:
```bash
getent passwd rose
```

**Expected Output Example**:
```
rose:x:1001:1001::/home/rose:/sbin/nologin
```

---

## 📋 Quick Command Reference

For quick copy-paste, here are all commands in sequence:

```bash
# Switch to root
sudo su -

# Check if user exists
cat /etc/passwd | grep rose

# Create the user
sudo useradd rose --shell /sbin/nologin

# Verify creation
id rose
```

---

## 💡 Additional Tips

- **To delete the user later**: `sudo userdel rose`
- **To delete user and home directory**: `sudo userdel -r rose`
- **To change user shell later**: `sudo usermod -s /bin/bash rose`

🚨 Task-Specific Challenge & Solution

🔍 Main Challenge Encountered:
The primary challenge was creating a user account with a non-interactive shell to prevent direct login access while maintaining the user's functionality for system processes and applications.
💡 Solution Approach:

User Verification: Used cat /etc/passwd | grep rose to check if the user already exists
Secure User Creation: Used sudo useradd rose --shell /sbin/nologin to create user with restricted shell access
Shell Security: The /sbin/nologin shell prevents interactive login while allowing the user to exist for system purposes
Verification Process: Used id rose and getent passwd rose to confirm proper user creation

🎯 Key Success Factors:

Non-interactive shell selection using /sbin/nologin for security compliance
Proper user verification both before and after creation
System integration allowing the user to function for backup agent tools without login access
Security-first approach preventing unauthorized interactive access

⚠️ Critical Learning Points:

Shell types matter for security - /sbin/nologin vs /bin/bash vs /bin/false
User verification is essential before creating accounts to avoid conflicts
System users often require non-interactive shells for security reasons
Backup agents and system processes can function with nologin users