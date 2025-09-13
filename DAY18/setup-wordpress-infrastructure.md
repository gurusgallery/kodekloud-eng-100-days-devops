**🌟 Task 18 - Set up WordPress Website Infrastructure and Database Access**

**📌 Task Description**

**xFusionCorp Industries** plans to host a **WordPress website** on their **Nautilus infrastructure** in **Stratos Datacenter**. The infrastructure includes a shared directory `/var/www/html` mounted on each app host at `/var/www/html`.

**Requirements:**
a. Install **Apache HTTP server**, **PHP and dependencies** on all app servers
b. Configure **Apache** to serve on **port 8089**
c. Install and configure **MariaDB server** on the database server
d. Create a database **`kodekloud_db9`** and a database user **`kodekloud_joy`** with password **`BruCStnMT5`**, granting all privileges on this database
e. Verify website connectivity to database via the **LBR server's App button**, which should show success message with username

👉 **Your task:** Set up complete LAMP stack infrastructure for WordPress deployment with database connectivity.

💡 **Note:** You can find the infrastructure details by clicking on the **Details of all Users and Servers** button on the top-right section of the page.

---

## 🔹 Step 1: Install LAMP stack on App Server 1

**Connect to App Server 1:**

```bash
ssh tony@stapp01
sudo su -
```

**Purpose**: Connect to the first app server and switch to root for administrative tasks.

---

## 🔹 Step 2: Install Apache and PHP packages

```bash
yum install -y httpd php php-mysqlnd php-fpm
```

**Purpose**: Install Apache HTTP server and PHP with MySQL database connectivity support.

**Package Details**:
- **httpd** - Apache HTTP server
- **php** - PHP scripting language
- **php-mysqlnd** - PHP MySQL Native Driver
- **php-fpm** - PHP FastCGI Process Manager

---

## 🔹 Step 3: Configure Apache port

```bash
vi /etc/httpd/conf/httpd.conf
```

**Purpose**: Edit Apache main configuration file to change the listening port.

**Configuration Change:**

**Find this line:**
```apache
Listen 80
```

**Change to:**
```apache
Listen 8089
```

**Important**: 🎯 Save the file after making the port change.

---

## 🔹 Step 4: Start and enable Apache service

```bash
systemctl start httpd
systemctl enable httpd
systemctl status httpd
```

**Purpose**: Start Apache service, enable auto-start on boot, and verify it's running.

**Expected Output**:
```
● httpd.service - The Apache HTTP Server
   Loaded: loaded
   Active: active (running)
   Main PID: 1234 (httpd)
```

**Verify port binding**:
```bash
ss -tulnp | grep :8089
```

---

## 🔹 Step 5: Repeat installation on App Server 2

**Connect to App Server 2:**

```bash
ssh steve@stapp02
sudo su -
yum install -y httpd php php-mysqlnd php-fpm
vi /etc/httpd/conf/httpd.conf
# Change Listen 80 to Listen 8089
systemctl start httpd
systemctl enable httpd
systemctl status httpd
```

**Purpose**: Configure identical LAMP stack setup on the second app server.

---

## 🔹 Step 6: Repeat installation on App Server 3

**Connect to App Server 3:**

```bash
ssh banner@stapp03
sudo su -
yum install -y httpd php php-mysqlnd php-fpm
vi /etc/httpd/conf/httpd.conf
# Change Listen 80 to Listen 8089
systemctl start httpd
systemctl enable httpd
systemctl status httpd
```

**Purpose**: Configure identical LAMP stack setup on the third app server.

---

## 🔹 Step 7: Install MariaDB on Database Server

**Connect to Database Server:**

```bash
ssh peter@stdb01
sudo su -
```

**Purpose**: Connect to the database server for MariaDB installation and configuration.

---

## 🔹 Step 8: Install MariaDB server

```bash
yum install -y mariadb-server
```

**Purpose**: Install MariaDB database server package.

**Package Installation**: 📦 This installs the complete MariaDB server with administration tools.

---

## 🔹 Step 9: Start and enable MariaDB service

```bash
systemctl start mariadb
systemctl enable mariadb
systemctl status mariadb
```

**Purpose**: Start MariaDB service, enable auto-start on boot, and verify it's running.

**Expected Output**:
```
● mariadb.service - MariaDB 10.x database server
   Loaded: loaded
   Active: active (running)
   Main PID: 1234 (mysqld)
```

---

## 🔹 Step 10: Access MariaDB as root

```bash
mysql -u root
```

**Purpose**: Access MariaDB command prompt as root user for database administration.

**Expected Output**:
```
Welcome to the MariaDB monitor.  Commands end with ; or \g.
MariaDB [(none)]>
```

**Note**: 🔑 Fresh MariaDB installations typically don't have a root password set initially.

---

## 🔹 Step 11: Create WordPress database

```sql
CREATE DATABASE kodekloud_db9;
```

**Purpose**: Create a dedicated database for the WordPress application.

**Expected Output**:
```
Query OK, 1 row affected (0.00 sec)
```

---

## 🔹 Step 12: Create database user with remote access

```sql
CREATE USER 'kodekloud_joy'@'%' IDENTIFIED BY 'BruCStnMT5';
```

**Purpose**: Create a database user that can connect from any host (%) with the specified password.

**Expected Output**:
```
Query OK, 0 rows affected (0.00 sec)
```

**User Details**:
- **Username**: `kodekloud_joy`
- **Host**: `%` (any host)
- **Password**: `BruCStnMT5`

---

## 🔹 Step 13: Grant all privileges on the database

```sql
GRANT ALL PRIVILEGES ON kodekloud_db9.* TO 'kodekloud_joy'@'%';
```

**Purpose**: Grant comprehensive privileges on the WordPress database to the application user.

**Expected Output**:
```
Query OK, 0 rows affected (0.00 sec)
```

**Privileges Granted**: Full control over the `kodekloud_db9` database and all its tables.

---

## 🔹 Step 14: Apply privilege changes

```sql
FLUSH PRIVILEGES;
```

**Purpose**: Reload the grant tables to ensure privilege changes take effect immediately.

**Expected Output**:
```
Query OK, 0 rows affected (0.00 sec)
```

---

## 🔹 Step 15: Exit MariaDB

```sql
EXIT;
```

**Purpose**: Exit the MariaDB command prompt and return to system shell.

---

## 🔹 Step 16: Verify database and user creation

```bash
mysql -u kodekloud_joy -p -h localhost
```

**Purpose**: Test database connectivity using the newly created user account.

**Password Prompt**: Enter `BruCStnMT5` when prompted.

**Test commands in MariaDB**:
```sql
SHOW DATABASES;
USE kodekloud_db9;
SELECT DATABASE();
EXIT;
```

---

## 🔹 Step 17: Configure MariaDB for network connections

```bash
vi /etc/my.cnf
```

**Purpose**: Configure MariaDB to accept connections from app servers.

**Add or modify these settings:**
```ini
[mysqld]
bind-address = 0.0.0.0
```

**Restart MariaDB** (if configuration changes were needed):
```bash
systemctl restart mariadb
```

---

## 🔹 Step 18: Test connectivity from app servers

**From any app server, test database connection:**

```bash
mysql -u kodekloud_joy -p -h stdb01
```

**Purpose**: Verify that app servers can connect to the database server.

**Password**: Enter `BruCStnMT5` when prompted.

**Success Test**:
```sql
SELECT 'Connection successful' AS status;
EXIT;
```

---

## 🔹 Step 19: Create PHP test file for connectivity

**On all app servers, create a test PHP file:**

```bash
cat > /var/www/html/dbtest.php << 'EOF'
<?php
$servername = "stdb01";
$username = "kodekloud_joy";
$password = "BruCStnMT5";
$dbname = "kodekloud_db9";

try {
    $conn = new PDO("mysql:host=$servername;dbname=$dbname", $username, $password);
    $conn->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
    echo "App is able to connect to the database using user kodekloud_joy";
} catch(PDOException $e) {
    echo "Connection failed: " . $e->getMessage();
}
?>
EOF
```

**Purpose**: Create a PHP script to test database connectivity from the web interface.

---

## 🔹 Step 20: Test via LBR App button

**Using the web interface:**

1. **Click the App button** in the lab environment
2. **Navigate to the test page** (if needed: `/dbtest.php`)
3. **Verify the success message**

**Expected Output**:
```
App is able to connect to the database using user kodekloud_joy
```

**Success Indicator**: ✅ **Database connectivity confirmed through web interface**

---

## 📋 Quick Command Reference

**On all app servers (stapp01, stapp02, stapp03):**
```bash
# Install LAMP stack
sudo su -
yum install -y httpd php php-mysqlnd php-fpm

# Configure Apache port
vi /etc/httpd/conf/httpd.conf
# Change: Listen 80 to Listen 8089

# Start services
systemctl start httpd && systemctl enable httpd
systemctl status httpd
```

**On database server (stdb01):**
```bash
# Install and start MariaDB
sudo su -
yum install -y mariadb-server
systemctl start mariadb && systemctl enable mariadb

# Configure database
mysql -u root
CREATE DATABASE kodekloud_db9;
CREATE USER 'kodekloud_joy'@'%' IDENTIFIED BY 'BruCStnMT5';
GRANT ALL PRIVILEGES ON kodekloud_db9.* TO 'kodekloud_joy'@'%';
FLUSH PRIVILEGES;
EXIT;
```

---

## 💡 Additional Tips

- **Firewall configuration**: Ensure port 3306 is open between app servers and database server
- **PHP configuration**: Install additional PHP modules as needed (`php-gd`, `php-xml`, etc.)
- **Security hardening**: Run `mysql_secure_installation` for production environments
- **Performance tuning**: Configure appropriate PHP and MariaDB settings for WordPress
- **SSL configuration**: Consider SSL certificates for production WordPress deployment

---

## 🔧 Troubleshooting Common Issues

### **Issue 1: Apache not starting on port 8089**
**Symptoms**: Service fails to start or port binding errors
**Solution**: Check for port conflicts and configuration syntax
```bash
ss -tulnp | grep :8089
systemctl status httpd -l
```

### **Issue 2: Database connection refused**
**Symptoms**: Can't connect to MariaDB from app servers
**Solution**: Check MariaDB bind-address and firewall settings
```bash
# Check MariaDB configuration
grep bind-address /etc/my.cnf
# Restart MariaDB if changes made
systemctl restart mariadb
```

### **Issue 3: PHP database connectivity issues**
**Symptoms**: PHP can't connect to database
**Solution**: Verify PHP MySQL extension and database credentials
```bash
php -m | grep mysql
# Test connection manually
mysql -u kodekloud_joy -p -h stdb01
```

### **Issue 4: Permission denied for database user**
**Symptoms**: User authentication or privilege errors
**Solution**: Re-create user with proper host specification
```sql
DROP USER 'kodekloud_joy'@'%';
CREATE USER 'kodekloud_joy'@'%' IDENTIFIED BY 'BruCStnMT5';
GRANT ALL PRIVILEGES ON kodekloud_db9.* TO 'kodekloud_joy'@'%';
FLUSH PRIVILEGES;
```

---

## 🚨 Task-Specific Challenge & Solution

**🔍 Main Challenge Encountered:**

The primary challenge was **establishing a complete LAMP stack infrastructure** with proper database connectivity between multiple app servers and a centralized database server, requiring coordinated configuration across multiple systems.

**💡 Solution Approach:**

1. **Multi-Server LAMP Installation**: Deployed Apache and PHP consistently across all three app servers (stapp01, stapp02, stapp03)
2. **Port Configuration Standardization**: Modified Apache configuration on all app servers to use port 8089 instead of default port 80
3. **Database Server Setup**: Installed and configured MariaDB on dedicated database server (stdb01) 
4. **User and Database Creation**: Created WordPress database `kodekloud_db9` and user `kodekloud_joy` with remote access privileges
5. **Network Connectivity**: Configured MariaDB to accept connections from app servers using `%` host specification
6. **Connectivity Verification**: Created PHP test script to verify end-to-end database connectivity through web interface

**🎯 Key Success Factors:**
- **Consistent configuration** across all app servers ensuring uniform LAMP stack deployment
- **Remote database access** using `'kodekloud_joy'@'%'` to allow connections from any app server
- **Network configuration** ensuring MariaDB accepts connections from remote hosts
- **Service coordination** starting and enabling all required services across the infrastructure
- **End-to-end testing** using PHP script accessible through LBR App button for verification

**⚠️ Critical Configuration Details:**
- **Apache port consistency** - all app servers must use port 8089 for load balancer compatibility
- **Database user host specification** - using `%` wildcard for multi-server access
- **MariaDB bind-address** configuration for network accessibility
- **PHP MySQL extensions** installation for database connectivity support

**🔒 Infrastructure Benefits:**
- **High availability** through multiple app servers with shared database
- **Load distribution** capability through standardized app server configuration
- **Centralized data management** with dedicated database server
- **Scalable architecture** supporting WordPress deployment across multiple servers

---

## ⚠️ Important Production Notes

🔧 **Infrastructure Readiness**: Complete LAMP stack deployed across all app servers with database connectivity

🔐 **Security Considerations**: Implement proper database security, firewall rules, and access controls

📊 **Performance Monitoring**: Monitor database connections and app server performance under load

🛡️ **Backup Strategy**: Configure database backups and app server file synchronization for WordPress data