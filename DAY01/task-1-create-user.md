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
