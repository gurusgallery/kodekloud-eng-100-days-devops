**🌟 Task 9 - Fix MariaDB Service Down Issue**

**📌 Task Description**

The **Nautilus application** in **Stratos Datacenter** is unable to connect to the database. The production support team found that the **MariaDB service** is down on the database server.

👉 **Your task:** Diagnose and fix the MariaDB service down issue to restore database connectivity for the Nautilus application.

💡 **Note:** You can find the infrastructure details by clicking on the **Details of all Users and Servers** button on the top-right section of the page.

---

## 🚨 Task-Specific Challenge & Solution

**🔍 Main Challenge Encountered:**

The primary challenge was **diagnosing and resolving MariaDB service failure** caused by missing data directory and uninitialized database, preventing the Nautilus application from connecting to the database server.

**💡 Solution Approach:**

1. **Service Diagnosis**: Used `systemctl status mariadb` to identify service failure and examine detailed error messages
2. **Root Cause Analysis**: Used `journalctl -xeu mariadb.service` to discover missing `/var/lib/mysql` data directory issue
3. **Directory Structure Recovery**: Created missing data directory and applied proper ownership using `chown mysql:mysql`
4. **Database Initialization**: Used `mariadb-install-db` to initialize the database system tables and essential directories
5. **Service Recovery**: Successfully started MariaDB after resolving directory and initialization issues
6. **Connectivity Verification**: Tested database accessibility using MariaDB shell and `SHOW DATABASES` command

**🎯 Key Success Factors:**
- **Systematic troubleshooting** using service logs and status commands to identify root causes
- **Directory permission management** ensuring proper mysql user ownership for data directory
- **Database initialization process** using mariadb-install-db for first-time setup
- **Service persistence configuration** enabling MariaDB for automatic startup on system boot

**⚠️ Critical Learning Points:**
- **Missing data directories** are common causes of database service startup failures
- **Permission ownership** is critical for database service functionality (mysql:mysql)
- **Database initialization** is required for first-time installations or corrupted data directories
- **Service logs** provide essential diagnostic information for troubleshooting database issues

**🔒 Most Common Issues Resolved:**
- **Missing `/var/lib/mysql` directory** preventing MariaDB from accessing its data files
- **Uninitialized database directory** requiring `mariadb-prepare-db-dir` or `mariadb-install-db` setup
- **Improper directory ownership** blocking MariaDB's ability to read/write data files

---

## 🔹 Step 1: Check MariaDB service status

```bash
sudo systemctl status mariadb
```

**Purpose**: Check the current status of the MariaDB service.

**Expected Outputs**:
- 🔴 **Inactive (dead)** - Service is stopped
- 🔴 **Failed** - Service failed to start
- ⚠️ **Active (running)** - Service is running (if so, check connectivity)

---

## 🔹 Step 2: Attempt to start MariaDB service

```bash
sudo systemctl start mariadb
```

**Purpose**: Attempt to start the MariaDB service.

**If successful**: Service should start without errors
**If failed**: Proceed to examine detailed logs in the next step

---

## 🔹 Step 3: View detailed service logs

```bash
sudo journalctl -xeu mariadb.service
```

**Purpose**: View detailed logs for the MariaDB service to identify specific errors.

**Common Error Indicators**:
- Permission denied errors
- Missing data directory
- Database initialization issues
- Configuration file problems

---

## 🔹 Step 4: Check MariaDB data directory

```bash
sudo ls -lah /var/lib/mysql
```

**Purpose**: Check if the MariaDB data directory exists and verify its ownership.

**Expected Directory Contents**:
```
drwxr-xr-x  mysql mysql  4096 Mar 15 10:30 .
drwxr-xr-x  root  root   4096 Mar 15 09:45 ..
-rw-rw----  mysql mysql   56  Mar 15 10:30 auto.cnf
drwx------  mysql mysql  4096 Mar 15 10:30 mysql/
```

**Problem Indicators**:
- ❌ **Directory doesn't exist**
- ❌ **Wrong ownership** (not mysql:mysql)
- ❌ **Empty directory** (uninitialized database)

---

## 🔹 Step 5: Fix missing data directory (if needed)

```bash
sudo mkdir /var/lib/mysql
```

**Purpose**: Create the missing data directory if it doesn't exist.

**When to use**: Only if `/var/lib/mysql` directory is completely missing.

---

## 🔹 Step 6: Fix directory ownership

```bash
sudo chown -R mysql:mysql /var/lib/mysql
```

**Purpose**: Set correct ownership to the mysql user and group for the data directory.

**Important**: 🔐 This ensures MariaDB has proper permissions to read/write its data files.

---

## 🔹 Step 7: Initialize database directory (if uninitialized)

```bash
sudo mariadb-install-db --user=mysql --datadir=/var/lib/mysql
```

**Purpose**: Initialize the MariaDB data directory for first-time setup.

**When to use**: If the data directory exists but is empty or uninitialized.

**Alternative command**:
```bash
sudo /usr/libexec/mariadb-prepare-db-dir mariadb.service
```

---

## 🔹 Step 8: Start and verify MariaDB service

```bash
sudo systemctl start mariadb
sudo systemctl status mariadb
```

**Purpose**: Start and verify the MariaDB service is running properly.

**Expected Output**:
```
● mariadb.service - MariaDB 10.x database server
   Loaded: loaded
   Active: active (running)
   Main PID: 1234 (mysqld)
```

---

## 🔹 Step 9: Enable MariaDB service (optional)

```bash
sudo systemctl enable mariadb
```

**Purpose**: Enable MariaDB to start automatically on system boot.

**Best Practice**: 🔄 Prevents future service down issues after server restarts.

---

## 🔹 Step 10: Test database connectivity

```bash
sudo mysql
```

**Purpose**: Login to the MariaDB shell to verify database functionality.

**Alternative login methods**:
```bash
# Login with specific user (if configured)
mysql -u root -p

# Login with socket authentication
sudo mysql -u root
```

---

## 🔹 Step 11: Verify database accessibility

```sql
SHOW DATABASES;
```

**Purpose**: Check if the databases are accessible and system databases exist.

**Expected Output**:
```
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
+--------------------+
```

**Exit MariaDB shell**:
```sql
EXIT;
```

---

## 📋 Quick Command Reference

For quick copy-paste, here are all commands in sequence:

```bash
# Check service status
sudo systemctl status mariadb

# Start service
sudo systemctl start mariadb

# View service logs
sudo journalctl -xeu mariadb.service

# Check data directory
sudo ls -lah /var/lib/mysql

# Fix missing directory (if needed)
sudo mkdir /var/lib/mysql
sudo chown -R mysql:mysql /var/lib/mysql

# Initialize database (if uninitialized)
sudo mariadb-install-db --user=mysql --datadir=/var/lib/mysql

# Start and verify service
sudo systemctl start mariadb
sudo systemctl status mariadb

# Enable service for auto-start
sudo systemctl enable mariadb

# Test connectivity
sudo mysql
SHOW DATABASES;
EXIT;
```

---

## 🔧 Common Issues Encountered

### **Issue 1: Missing Data Directory**
**Symptoms**: MariaDB fails to start, logs show "Can't create/write to file"
**Root Cause**: The `/var/lib/mysql` directory doesn't exist
**Solution**: Create directory and set proper ownership

### **Issue 2: Uninitialized Database Directory**
**Symptoms**: MariaDB fails to start, logs show "Fatal error: Can't open and lock privilege tables"
**Root Cause**: Database directory exists but is empty/uninitialized
**Solution**: Run `mariadb-install-db` or `mariadb-prepare-db-dir`

### **Issue 3: Permission Issues**
**Symptoms**: MariaDB starts but can't access files, permission denied errors
**Root Cause**: Data directory not owned by `mysql:mysql`
**Solution**: Fix ownership with `chown -R mysql:mysql /var/lib/mysql`

### **Issue 4: Configuration Problems**
**Symptoms**: Service fails with configuration errors
**Root Cause**: Incorrect settings in `/etc/my.cnf` or `/etc/mysql/my.cnf`
**Solution**: Review and fix configuration file syntax

---

## 💡 Additional Tips

- **Check disk space**: `df -h /var/lib/mysql` (MariaDB needs adequate disk space)
- **Review error logs**: `sudo tail -f /var/log/mysqld.log` or `/var/log/mariadb/mariadb.log`
- **Check port availability**: `sudo netstat -tlnp | grep 3306`
- **Verify MySQL socket**: `ls -la /var/run/mysqld/mysqld.sock`
- **Test remote connectivity**: `telnet database_server_ip 3306`

---

## 🚨 Most Common Root Cause

**⚠️ In this specific scenario encountered:**

**Primary Issue**: MariaDB's data directory (`/var/lib/mysql`) was missing and not owned by the mysql user, preventing the server from initializing and starting.

**Secondary Issue**: The database directory was uninitialized, requiring first-time setup using `mariadb-prepare-db-dir` to initialize the system databases.

**Resolution Path**:
1. Create missing `/var/lib/mysql` directory
2. Set proper ownership (`mysql:mysql`)
3. Initialize database with `mariadb-install-db` or `mariadb-prepare-db-dir`
4. Start MariaDB service successfully

---

## ⚠️ Important Security Notes

🔐 **Post-fix security steps**:
- Run `mysql_secure_installation` to secure the installation
- Set strong root password
- Remove test databases and anonymous users
- Configure appropriate user permissions

🛡️ **Production considerations**:
- Enable MariaDB service for automatic startup
- Configure backup procedures
- Monitor disk space and performance
- Implement proper firewall rules