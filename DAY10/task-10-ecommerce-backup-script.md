**🌟 Task 10 - Create Bash Script for Website Backup**

**📌 Task Description**

The production support team of **xFusionCorp Industries** needs a bash script `ecommerce_backup.sh` on **App Server 1** in **Stratos Datacenter** to automate the following tasks:

a. Create a zip archive named `xfusioncorp_ecommerce.zip` of the `/var/www/html/ecommerce` directory
b. Save the archive in `/backup/` on **App Server 1** (temporary storage, cleaned weekly)
c. Copy the archive to **Nautilus Backup Server** (`stbkp01`) in `/backup/` location
d. Ensure the script does not ask for a password while copying, and the relevant server user (e.g., `tony`) can run it
e. Do not use `sudo` inside the script

👉 **Your task:** Create an automated backup script that zips the ecommerce directory and copies it to the backup server without manual intervention.

💡 **Note:** The `zip` package must be installed manually on the server before running the script.

---

## 🔹 Step 1: Install required packages

```bash
sudo yum install zip -y
```

**Purpose**: Install the zip utility required for creating compressed archives.

**For Ubuntu/Debian systems**:
```bash
sudo apt-get install zip -y
```

---

## 🔹 Step 2: Create the backup script directory

```bash
sudo mkdir -p /scripts
sudo mkdir -p /backup
```

**Purpose**: Create necessary directories for script storage and local backup location.

**Permission setup**:
```bash
sudo chown tony:tony /scripts /backup
```

---

## 🔹 Step 3: Create the backup script

```bash
cd /scripts/
vim ecommerce_backup.sh
```

**Create the script with the following content:**

```bash
#!/bin/bash

# eCommerce Backup Script for xFusionCorp Industries
# Creates zip archive and copies to backup server

echo "Starting eCommerce backup process..."

# Create zip archive
echo "Creating zip archive of /var/www/html/ecommerce..."
zip -r /backup/xfusioncorp_ecommerce.zip /var/www/html/ecommerce

# Check if zip command was successful
if [ $? -eq 0 ]; then
    echo "✅ Zip archive created successfully at /backup/xfusioncorp_ecommerce.zip"
    
    # Copy archive to backup server
    echo "📤 Copying archive to backup server (stbkp01)..."
    scp /backup/xfusioncorp_ecommerce.zip clint@stbkp01:/backup/
    
    # Check if scp command was successful
    if [ $? -eq 0 ]; then
        echo "✅ Successfully copied zip archive to backup server."
        echo "📋 Backup process completed successfully!"
    else
        echo "❌ Failed to copy archive to backup server."
        exit 1
    fi
else
    echo "❌ Zip command failed. Archive not created."
    exit 1
fi

echo "🎉 eCommerce backup workflow completed!"
```

**Purpose**: Create a comprehensive backup script with error handling and status messages.

---

## 🔹 Step 4: Make the script executable

```bash
chmod +x /scripts/ecommerce_backup.sh
```

**Purpose**: Grant execute permissions to the backup script.

**Verify permissions**:
```bash
ls -la /scripts/ecommerce_backup.sh
```

---

## 🔹 Step 5: Set up passwordless SSH access

```bash
ssh-keygen -t rsa
```

**Purpose**: Generate SSH key pair for passwordless authentication.

**Key generation options**:
- **File location**: Accept default (`/home/tony/.ssh/id_rsa`)
- **Passphrase**: Press `Enter` for empty passphrase

---

## 🔹 Step 6: Copy public key to backup server

```bash
ssh-copy-id clint@stbkp01
```

**Purpose**: Copy the public key to the backup server for passwordless SSH access.

**Note**: You'll be prompted for `clint` user's password this one time only.

---

## 🔹 Step 7: Test SSH connection to backup server

```bash
ssh clint@stbkp01
```

**Purpose**: Verify passwordless SSH connection is working properly.

**Test commands on backup server**:
```bash
# Check backup directory exists
ls -la /backup/
# Exit back to app server
exit
```

---

## 🔹 Step 8: Run the backup script

```bash
cd /scripts/
bash ecommerce_backup.sh
```

**Purpose**: Execute the backup script and monitor the backup process.

**Expected Output**:
```
Starting eCommerce backup process...
Creating zip archive of /var/www/html/ecommerce...
✅ Zip archive created successfully at /backup/xfusioncorp_ecommerce.zip
📤 Copying archive to backup server (stbkp01)...
✅ Successfully copied zip archive to backup server.
📋 Backup process completed successfully!
🎉 eCommerce backup workflow completed!
```

---

## 🔹 Step 9: Verify backup on remote server

```bash
ssh clint@stbkp01 "ls -la /backup/"
```

**Purpose**: Verify that the backup archive was successfully copied to the backup server.

**Expected Output**:
```
-rw-r--r-- 1 clint clint 12345678 Mar 15 10:30 xfusioncorp_ecommerce.zip
```

---

## 🔹 Step 10: Test script from different location

```bash
# Test running script from home directory
cd ~
/scripts/ecommerce_backup.sh
```

**Purpose**: Ensure the script works regardless of current working directory.

---

## 📋 Quick Command Reference

For quick copy-paste, here are all commands in sequence:

```bash
# Install zip package
sudo yum install zip -y

# Create directories
sudo mkdir -p /scripts /backup
sudo chown tony:tony /scripts /backup

# Create and edit script
cd /scripts/
vim ecommerce_backup.sh

# Make script executable
chmod +x ecommerce_backup.sh

# Setup passwordless SSH
ssh-keygen -t rsa
ssh-copy-id clint@stbkp01

# Test SSH connection
ssh clint@stbkp01
ls -la /backup/
exit

# Run backup script
bash ecommerce_backup.sh

# Verify backup on remote server
ssh clint@stbkp01 "ls -la /backup/"
```

---

## 💡 Additional Tips

- **Schedule with cron**: Add to crontab for automated daily/weekly backups
- **Log rotation**: Implement log files for backup history tracking
- **Cleanup old backups**: Add commands to remove old archives to save space
- **Compression options**: Use `zip -9` for maximum compression if needed
- **Error notifications**: Add email notifications for backup failures

---

## 🔧 Troubleshooting Common Issues

### **Issue 1: Zip command not found**
**Solution**: Install zip package with `sudo yum install zip -y`

### **Issue 2: Permission denied on /backup directory**
**Solution**: Ensure proper ownership `sudo chown tony:tony /backup`

### **Issue 3: SSH password prompts**
**Solution**: Verify SSH key setup with `ssh-copy-id clint@stbkp01`

### **Issue 4: SCP connection refused**
**Solution**: Check if backup server is reachable `ping stbkp01`

---

## 🚨 Task-Specific Challenge & Solution

**🔍 Main Challenge Encountered:**

The primary challenge was ensuring **passwordless SSH authentication** between App Server 1 and the Backup Server (stbkp01) while avoiding the use of `sudo` within the script itself.

**💡 Solution Approach:**

1. **Pre-setup Requirements**: Install zip package and create necessary directories using `sudo` outside the script
2. **SSH Key Authentication**: Generate RSA key pair and copy public key to backup server using `ssh-copy-id`
3. **Script Design**: Create script that relies on pre-configured passwordless SSH rather than embedding credentials
4. **Error Handling**: Implement proper exit codes and status messages for troubleshooting

**🎯 Key Success Factors:**
- **Proper directory permissions** for the `tony` user on both source and destination paths
- **Successful SSH key distribution** to eliminate password prompts during scp operations
- **Script testing** from multiple locations to ensure portability and reliability

---

## ⚠️ Important Production Notes

🔄 **Automation Ready**: Script can be easily integrated into cron jobs for scheduled backups

🔐 **Security Compliant**: Uses SSH key authentication instead of embedded passwords

📦 **Space Efficient**: Creates compressed archives to minimize storage requirements

🛡️ **Error Handling**: Provides clear success/failure feedback for monitoring systems