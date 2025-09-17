**🌟 Task 20 - Configure Nginx + PHP-FPM 8.3 with Unix Socket**

**📌 Task Description**

The **Nautilus application development team** is planning to launch a new **PHP-based application**, which they want to deploy on **Nautilus infra** in **Stratos DC**. The development team had a meeting with the production support team and they have shared some requirements regarding the infrastructure.

**Requirements:**
a. Install **nginx** on **app server 3**, configure it to use **port 8099** and its document root should be **`/var/www/html`**
b. Install **php-fpm version 8.3** on **app server 3**, it must use the unix socket **`/var/run/php-fpm/default.sock`** (create the parent directories if don't exist)
c. Configure **php-fpm and nginx** to work together
d. Once configured correctly, you can test the website using **`curl http://stapp03:8099/index.php`** command from jump host

👉 **Your task:** Set up a complete Nginx + PHP-FPM web server stack with custom port and Unix socket configuration.

💡 **Note:** We have copied two files, `index.php` and `info.php`, under `/var/www/html` as part of the PHP-based application setup. Please do not modify these files.

---

## 🔹 Step 1: Connect to App Server 3

```bash
ssh banner@stapp03
sudo su -
```

**Purpose**: Connect to App Server 3 as the banner user and switch to root for administrative tasks.

---

## 🔹 Step 2: Install Nginx web server

```bash
yum install nginx -y
```

**Purpose**: Install Nginx web server package on App Server 3.

**Package Installation**: This installs Nginx with default configuration that we'll customize.

---

## 🔹 Step 3: Configure Nginx for port 8099

```bash
vi /etc/nginx/nginx.conf
```

**Purpose**: Edit the main Nginx configuration file to set custom port and document root.

**Configuration Changes Required:**

**Find the server block and modify it:**

```nginx
server {
    listen       8099;
    listen       [::]:8099;
    server_name  _;
    root         /var/www/html;
    index        index.php index.html;

    # PHP-FPM configuration will be added here
    location ~ \.php$ {
        fastcgi_pass unix:/var/run/php-fpm/default.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
}
```

**Save and exit**: Press `Esc`, type `:wq`, press `Enter`

---

## 🔹 Step 4: Start and verify Nginx service

```bash
systemctl start nginx
systemctl enable nginx
systemctl status nginx
```

**Purpose**: Start Nginx service and verify it's running on the correct port.

**Verify port binding**:
```bash
ss -tulnp | grep :8099
```

**Expected Output**:
```
tcp    LISTEN  0    128    *:8099    *:*    users:(("nginx",pid=1234,fd=6))
```

---

## 🔹 Step 5: Check OS version for PHP installation

```bash
cat /etc/os-release
```

**Purpose**: Verify the operating system version to determine the correct PHP installation method.

**Expected**: CentOS Stream 9 or similar RHEL-based distribution.

---

## 🔹 Step 6: Update system and install repositories

```bash
sudo dnf update -y
sudo dnf install -y epel-release
sudo dnf install -y https://rpms.remirepo.net/enterprise/remi-release-9.rpm
```

**Purpose**: Update the system and install EPEL and Remi repositories for PHP 8.3 installation.

**Repository Details**:
- **EPEL**: Extra packages for Enterprise Linux
- **Remi**: Repository containing newer PHP versions

---

## 🔹 Step 7: Configure PHP module for version 8.3

```bash
sudo dnf module reset php -y
sudo dnf module enable php:remi-8.3 -y
```

**Purpose**: Reset any existing PHP module configuration and enable PHP 8.3 from the Remi repository.

**Module Management**: This ensures we get PHP 8.3 specifically, not the default version.

---

## 🔹 Step 8: Install PHP-FPM and extensions

```bash
sudo dnf install -y php-fpm php-cli php-mysqlnd php-zip php-devel php-gd php-mbstring php-curl php-xml php-bcmath php-json
```

**Purpose**: Install PHP-FPM and commonly required PHP extensions.

**Extensions Installed**:
- **php-fpm**: FastCGI Process Manager
- **php-cli**: Command line interface
- **php-mysqlnd**: MySQL database support
- **php-gd, php-mbstring, etc.**: Common web development extensions

---

## 🔹 Step 9: Verify PHP installation

```bash
php -v
```

**Purpose**: Confirm PHP 8.3 is installed correctly.

**Expected Output**:
```
PHP 8.3.x (cli) (built: ...)
Copyright (c) The PHP Group
Zend Engine v4.3.x
```

---

## 🔹 Step 10: Configure PHP-FPM pool

```bash
vi /etc/php-fpm.d/www.conf
```

**Purpose**: Configure PHP-FPM to use Unix socket and run under nginx user.

**Configuration Changes Required:**

**Find and modify these lines:**

```ini
; Change user and group to nginx
user = nginx
group = nginx

; Change socket path to the required location
listen = /var/run/php-fpm/default.sock

; Set socket ownership and permissions
listen.owner = nginx
listen.group = nginx
listen.mode = 0660
```

**Critical Settings**:
- **user/group**: Must be `nginx` for proper permissions
- **listen**: Unix socket path as specified in requirements
- **Socket permissions**: Ensure nginx can read/write to socket

---

## 🔹 Step 11: Create socket directory if needed

```bash
mkdir -p /var/run/php-fpm
chown nginx:nginx /var/run/php-fpm
```

**Purpose**: Ensure the directory for the Unix socket exists with proper permissions.

---

## 🔹 Step 12: Update Nginx configuration for PHP processing

```bash
vi /etc/nginx/nginx.conf
```

**Purpose**: Add PHP processing configuration to the Nginx server block.

**Add this location block inside the server block:**

```nginx
server {
    listen       8099;
    listen       [::]:8099;
    server_name  _;
    root         /var/www/html;
    index        index.php index.html;

    location ~ \.php$ {
        fastcgi_pass unix:/var/run/php-fpm/default.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
}
```

**FastCGI Configuration**:
- **fastcgi_pass**: Points to the Unix socket
- **SCRIPT_FILENAME**: Proper path resolution for PHP files
- **fastcgi_params**: Include standard FastCGI parameters

---

## 🔹 Step 13: Validate Nginx configuration

```bash
nginx -t
```

**Purpose**: Test Nginx configuration syntax before restarting services.

**Expected Output**:
```
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

**If errors found**: Review and fix configuration syntax before proceeding.

---

## 🔹 Step 14: Start PHP-FPM service

```bash
systemctl start php-fpm
systemctl enable php-fpm
systemctl status php-fpm
```

**Purpose**: Start PHP-FPM service and verify it's running correctly.

**Expected Status**: Active (running) with no errors.

---

## 🔹 Step 15: Restart Nginx with new configuration

```bash
systemctl restart nginx
systemctl status nginx
```

**Purpose**: Apply the new Nginx configuration and verify the service is running.

---

## 🔹 Step 16: Verify Unix socket creation

```bash
ls -l /var/run/php-fpm/
```

**Purpose**: Confirm that the Unix socket was created with proper permissions.

**Expected Output**:
```
srw-rw---- 1 nginx nginx 0 Mar 15 10:30 default.sock
```

**Socket Properties**:
- **Type**: Socket file (starts with 's')
- **Owner**: nginx:nginx
- **Permissions**: rw-rw---- (660)

---

## 🔹 Step 17: Set proper socket permissions

```bash
chown nginx:nginx /var/run/php-fpm/default.sock
chmod 660 /var/run/php-fpm/default.sock
```

**Purpose**: Ensure the socket has correct ownership and permissions for nginx access.

---

## 🔹 Step 18: Test from Jump Host

**From Jump Host:**

```bash
curl http://stapp03:8099/index.php
```

**Purpose**: Test the PHP application through the web server to verify complete functionality.

**Expected Output**:
```
Welcome to xFusionCorp Industries!
```

**Success Indicator**: This confirms that Nginx and PHP-FPM are working together correctly.

---

## 🔹 Step 19: Additional verification tests

```bash
# Test info.php file
curl http://stapp03:8099/info.php

# Test Nginx status
systemctl status nginx

# Test PHP-FPM status  
systemctl status php-fpm

# Check socket file details
ls -la /var/run/php-fpm/default.sock
```

**Purpose**: Perform comprehensive testing to ensure all components are functioning properly.

---

## 🔹 Step 20: Final configuration verification

```bash
# Verify Nginx syntax one more time
nginx -t

# Check listening ports
ss -tulnp | grep :8099

# Verify PHP-FPM socket
ss -xl | grep default.sock
```

**Purpose**: Final verification before task completion to ensure everything is properly configured.

---

## 📋 Quick Command Reference

For quick copy-paste, execute on **App Server 3**:

```bash
# Install and configure Nginx
sudo su -
yum install nginx -y
vi /etc/nginx/nginx.conf
# (Configure server block for port 8099 with PHP location)
systemctl start nginx && systemctl enable nginx

# Install PHP 8.3 from Remi repository
dnf update -y
dnf install -y epel-release
dnf install -y https://rpms.remirepo.net/enterprise/remi-release-9.rpm
dnf module reset php -y
dnf module enable php:remi-8.3 -y
dnf install -y php-fpm php-cli php-mysqlnd php-zip php-devel php-gd php-mbstring php-curl php-xml php-bcmath php-json

# Configure PHP-FPM
mkdir -p /var/run/php-fpm
vi /etc/php-fpm.d/www.conf
# (Set user=nginx, group=nginx, listen=/var/run/php-fpm/default.sock)

# Start services and test
systemctl start php-fpm && systemctl enable php-fpm
systemctl restart nginx
nginx -t

# Test from jump host
curl http://stapp03:8099/index.php
```

---

## 💡 Additional Tips

- **Log monitoring**: Check `/var/log/nginx/error.log` and `/var/log/php-fpm/www-error.log` for issues
- **Socket permissions**: Ensure nginx user can read/write to the Unix socket
- **Firewall**: Open port 8099 if firewall is active (`firewall-cmd --permanent --add-port=8099/tcp`)
- **SELinux**: May need to set appropriate contexts for the socket file
- **Performance**: Adjust PHP-FPM pool settings for production workloads

---

## 🔧 Troubleshooting Common Issues

### **Issue 1: 502 Bad Gateway error**
**Symptoms**: Nginx returns 502 error when accessing PHP files
**Solution**: Check PHP-FPM service status and socket permissions
```bash
systemctl status php-fpm
ls -la /var/run/php-fpm/default.sock
chown nginx:nginx /var/run/php-fpm/default.sock
```

### **Issue 2: Socket file not created**
**Symptoms**: Unix socket doesn't exist after starting PHP-FPM
**Solution**: Check PHP-FPM configuration and create directory
```bash
mkdir -p /var/run/php-fpm
chown nginx:nginx /var/run/php-fpm
systemctl restart php-fpm
```

### **Issue 3: Permission denied errors**
**Symptoms**: Nginx can't connect to PHP-FPM socket
**Solution**: Fix socket ownership and permissions
```bash
chown nginx:nginx /var/run/php-fpm/default.sock
chmod 660 /var/run/php-fpm/default.sock
```

### **Issue 4: PHP files download instead of executing**
**Symptoms**: PHP files are downloaded rather than executed
**Solution**: Check Nginx location block for PHP processing
```bash
nginx -t
# Verify location ~ \.php$ block exists and is correct
```

---

## 🚨 Task-Specific Challenge & Solution

**🔍 Main Challenge Encountered:**

The primary challenge was **configuring Nginx and PHP-FPM to communicate via Unix socket** while ensuring proper permissions, user contexts, and service integration for a PHP web application on a custom port.

**💡 Solution Approach:**

1. **Custom Port Configuration**: Modified Nginx to listen on port 8099 instead of default port 80 with document root set to `/var/www/html`
2. **PHP 8.3 Installation**: Used Remi repository to install specific PHP-FPM 8.3 version on CentOS Stream 9 system
3. **Unix Socket Configuration**: Configured PHP-FPM to use Unix socket `/var/run/php-fpm/default.sock` instead of TCP connection
4. **User Context Alignment**: Set both Nginx and PHP-FPM to run under `nginx` user for proper permission handling
5. **Service Integration**: Configured Nginx FastCGI to communicate with PHP-FPM through the Unix socket
6. **Permission Management**: Ensured proper socket file ownership and permissions for seamless communication

**🎯 Key Success Factors:**
- **Repository management** using dnf module system to get PHP 8.3 from Remi repository
- **Socket path configuration** creating the required directory structure `/var/run/php-fpm/`
- **User alignment** ensuring both services run under the same user (nginx) for permission consistency
- **FastCGI configuration** proper Nginx location block for PHP file processing
- **Service coordination** starting services in correct order and verifying socket creation
- **End-to-end testing** using curl from jump host to validate complete functionality

**⚠️ Critical Configuration Details:**
- **PHP-FPM pool configuration** must specify `listen = /var/run/php-fpm/default.sock`
- **Nginx FastCGI pass** must point to `unix:/var/run/php-fpm/default.sock`
- **Socket permissions** require nginx:nginx ownership with 660 permissions
- **Service user alignment** both nginx and php-fpm must run under nginx user
- **Directory creation** `/var/run/php-fpm/` directory must exist before socket creation

**🔒 Performance Benefits:**
- **Unix socket communication** provides faster inter-process communication than TCP
- **Reduced network overhead** eliminates TCP/IP stack for local communication
- **Better security** socket files provide better access control than network ports
- **Resource efficiency** lower memory and CPU usage compared to TCP connections

---

## ⚠️ Important Production Notes

🔧 **Web Server Stack**: Complete Nginx + PHP-FPM 8.3 configuration with Unix socket communication

🔐 **Security Considerations**: Proper user contexts and socket permissions for secure operation

📊 **Performance Monitoring**: Monitor PHP-FPM pool processes and Nginx connection handling

🛡️ **Service Management**: Both services configured for automatic startup and proper dependency handling