**🌟 Task 19 - Serve Two Static Websites on Apache with Different URL Paths**

**📌 Task Description**

**xFusionCorp Industries** plans to host **two static websites** on **app server 3** in **Stratos Datacenter**. The websites are under development and backups are available on the **jump host**.

**Requirements:**
a. Install **Apache HTTP server** (`httpd`) on **app server 3**
b. Configure Apache to serve on **port 6400**
c. Copy backups of two sites from jump host: `/home/thor/blog` and `/home/thor/cluster` to app server 3 and configure Apache to serve them on URLs:
   - `http://localhost:6400/blog/`
   - `http://localhost:6400/cluster/`
d. Verify **site content accessibility** locally

👉 **Your task:** Set up Apache to serve multiple static websites with different URL paths on a custom port.

💡 **Note:** You can find the infrastructure details by clicking on the **Details of all Users and Servers** button on the top-right section of the page.

---

## 🔹 Step 1: Connect to App Server 3

```bash
ssh banner@stapp03
sudo su -
```

**Purpose**: Connect to App Server 3 as the banner user and switch to root for administrative tasks.

---

## 🔹 Step 2: Check Apache service status (pre-installation)

```bash
systemctl status httpd
```

**Purpose**: Check if Apache is already installed on the server.

**Expected Output**:
```
Unit httpd.service could not be found.
```

**Status**: ❌ **Apache is not installed yet** - proceeding with installation.

---

## 🔹 Step 3: Install Apache HTTP server

```bash
yum install -y httpd
```

**Purpose**: Install Apache HTTP server package on App Server 3.

**For Ubuntu/Debian systems**:
```bash
apt-get update
apt-get install -y apache2
```

**Installation Note**: 📦 This installs Apache with default configuration.

---

## 🔹 Step 4: Configure Apache port

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
Listen 6400
```

**Save and exit**: Press `Esc`, type `:wq`, press `Enter`

---

## 🔹 Step 5: Copy website backups from Jump Host

**From Jump Host, copy the website directories:**

```bash
scp -r /home/thor/blog banner@stapp03:/tmp/
scp -r /home/thor/cluster banner@stapp03:/tmp/
```

**Purpose**: Copy both website directories from jump host to app server 3 temporary location.

**Alternative method** (if on jump host):
```bash
rsync -av /home/thor/blog/ banner@stapp03:/tmp/blog/
rsync -av /home/thor/cluster/ banner@stapp03:/tmp/cluster/
```

---

## 🔹 Step 6: Move websites to Apache document root

**On App Server 3:**

```bash
mv /tmp/blog /var/www/html/
mv /tmp/cluster /var/www/html/
```

**Purpose**: Move website directories to Apache's document root directory.

**Verify the move**:
```bash
ls -la /var/www/html/
```

**Expected Output**:
```
drwxr-xr-x. 2 root root 4096 Mar 15 10:30 blog
drwxr-xr-x. 2 root root 4096 Mar 15 10:30 cluster
```

---

## 🔹 Step 7: Set proper permissions

```bash
chown -R apache:apache /var/www/html/blog /var/www/html/cluster
chmod -R 755 /var/www/html/blog /var/www/html/cluster
```

**Purpose**: Set appropriate ownership and permissions for Apache to serve the websites.

**Permission Details**:
- **Owner**: apache user and group
- **Permissions**: 755 (read, write, execute for owner; read, execute for group and others)

---

## 🔹 Step 8: Create Apache configuration for websites

```bash
vi /etc/httpd/conf.d/websites.conf
```

**Purpose**: Create a dedicated configuration file for the two static websites.

**Configuration Content:**

```apache
# Blog website configuration
Alias /blog "/var/www/html/blog"
<Directory "/var/www/html/blog">
    Options Indexes FollowSymLinks
    AllowOverride None
    Require all granted
</Directory>

# Cluster website configuration  
Alias /cluster "/var/www/html/cluster"
<Directory "/var/www/html/cluster">
    Options Indexes FollowSymLinks
    AllowOverride None
    Require all granted
</Directory>
```

**Configuration Breakdown**:
- **Alias**: Maps URL paths to filesystem directories
- **Directory**: Defines access controls and options for each directory
- **Options Indexes**: Allows directory browsing if no index file exists
- **FollowSymLinks**: Allows following symbolic links
- **Require all granted**: Allows access from all clients

---

## 🔹 Step 9: Verify Apache configuration syntax

```bash
httpd -t
```

**Purpose**: Test Apache configuration syntax before starting the service.

**Expected Output**:
```
Syntax OK
```

**If errors found**: Review and fix configuration files before proceeding.

---

## 🔹 Step 10: Start and enable Apache service

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

---

## 🔹 Step 11: Verify Apache is listening on port 6400

```bash
ss -tulnp | grep :6400
```

**Purpose**: Confirm Apache is listening on the correct port 6400.

**Expected Output**:
```
tcp    LISTEN  0    128    *:6400    *:*    users:(("httpd",pid=1234,fd=4))
```

**Alternative command**:
```bash
netstat -tulnp | grep :6400
```

---

## 🔹 Step 12: Test blog website locally

```bash
curl http://localhost:6400/blog/
```

**Purpose**: Test the blog website accessibility through the configured URL path.

**Expected Output**: HTML content from the blog website files.

**Alternative test**:
```bash
curl -I http://localhost:6400/blog/
```

---

## 🔹 Step 13: Test cluster website locally

```bash
curl http://localhost:6400/cluster/
```

**Purpose**: Test the cluster website accessibility through the configured URL path.

**Expected Output**: HTML content from the cluster website files.

---

## 🔹 Step 14: Verify website content

```bash
# Check if index files exist
ls -la /var/www/html/blog/
ls -la /var/www/html/cluster/

# View content samples
head /var/www/html/blog/index.html
head /var/www/html/cluster/index.html
```

**Purpose**: Verify that website files contain proper content and are accessible.

---

## 🔹 Step 15: Test from external access (optional)

```bash
# Get server IP
ip addr show | grep "inet " | grep -v 127.0.0.1

# Test from jump host or another server
curl http://[SERVER_IP]:6400/blog/
curl http://[SERVER_IP]:6400/cluster/
```

**Purpose**: Test websites from external hosts to verify network accessibility.

---

## 📋 Quick Command Reference

For quick copy-paste, execute on **App Server 3**:

```bash
# Connect and install Apache
ssh banner@stapp03
sudo su -
yum install -y httpd

# Configure Apache port
vi /etc/httpd/conf/httpd.conf
# Change: Listen 80 to Listen 6400

# Copy websites from jump host (run from jump host)
scp -r /home/thor/blog banner@stapp03:/tmp/
scp -r /home/thor/cluster banner@stapp03:/tmp/

# Move and set permissions
mv /tmp/blog /var/www/html/
mv /tmp/cluster /var/www/html/
chown -R apache:apache /var/www/html/blog /var/www/html/cluster
chmod -R 755 /var/www/html/blog /var/www/html/cluster

# Create website configuration
vi /etc/httpd/conf.d/websites.conf
# Add Alias and Directory configurations

# Start Apache
httpd -t
systemctl start httpd && systemctl enable httpd

# Test websites
curl http://localhost:6400/blog/
curl http://localhost:6400/cluster/
```

---

## 💡 Additional Tips

- **Index files**: Ensure each website directory has an `index.html` or `index.php` file
- **Error logs**: Monitor `/var/log/httpd/error_log` for troubleshooting
- **Virtual hosts**: Consider using virtual hosts for more complex configurations
- **Security**: Implement appropriate security headers and access controls for production
- **Performance**: Configure caching and compression for better performance

---

## 🔧 Troubleshooting Common Issues

### **Issue 1: Apache fails to start on port 6400**
**Symptoms**: Service fails to start or port binding errors
**Solution**: Check for port conflicts and configuration syntax
```bash
ss -tulnp | grep :6400
systemctl status httpd -l
httpd -t
```

### **Issue 2: Website content not accessible**
**Symptoms**: 404 errors or permission denied
**Solution**: Verify file permissions and directory structure
```bash
ls -la /var/www/html/
chown -R apache:apache /var/www/html/blog /var/www/html/cluster
chmod -R 755 /var/www/html/blog /var/www/html/cluster
```

### **Issue 3: Alias configuration not working**
**Symptoms**: URLs don't map to correct directories
**Solution**: Check Apache configuration and restart service
```bash
httpd -t
cat /etc/httpd/conf.d/websites.conf
systemctl restart httpd
```

### **Issue 4: SELinux blocking access**
**Symptoms**: Permission denied errors in logs
**Solution**: Set appropriate SELinux contexts
```bash
restorecon -Rv /var/www/html/
setsebool -P httpd_can_network_connect 1
```

---

## 🚨 Task-Specific Challenge & Solution

**🔍 Main Challenge Encountered:**

The primary challenge was **configuring Apache to serve multiple static websites** under different URL paths on a custom port, requiring proper alias configuration and file management across servers.

**💡 Solution Approach:**

1. **Custom Port Configuration**: Modified Apache's main configuration to listen on port 6400 instead of default port 80
2. **File Transfer Management**: Used SCP to copy website directories from jump host to app server 3 via temporary location
3. **Directory Structure Setup**: Moved websites to Apache document root (`/var/www/html/`) and set proper ownership and permissions
4. **Alias Configuration**: Created dedicated configuration file with Alias directives to map URL paths `/blog/` and `/cluster/` to their respective directories
5. **Access Control Configuration**: Implemented proper Directory blocks with appropriate permissions and options for each website
6. **Service Management**: Started and enabled Apache service with configuration validation

**🎯 Key Success Factors:**
- **Port customization** using Listen directive to serve on port 6400 as specified
- **File transfer coordination** between jump host and app server for website content
- **Alias directive usage** to create clean URL mappings for multiple websites
- **Permission management** ensuring Apache can read and serve website files
- **Configuration validation** using `httpd -t` before service startup
- **Local testing** using curl to verify both websites are accessible at their respective URLs

**⚠️ Critical Configuration Details:**
- **Alias syntax** must specify both URL path and filesystem path correctly
- **Directory permissions** require proper apache:apache ownership and 755 permissions
- **Configuration file placement** in `/etc/httpd/conf.d/` for modular configuration management
- **Service restart** required after configuration changes to apply new settings

**🔒 Multi-Site Benefits:**
- **URL path separation** allowing multiple websites on single Apache instance
- **Resource efficiency** using one Apache server for multiple static sites
- **Maintenance simplicity** with separate directories for each website
- **Scalability** easy to add additional websites using same alias pattern

---

## ⚠️ Important Production Notes

🔧 **Multi-Site Management**: Apache configured to serve multiple static websites efficiently on custom port

🔐 **Security Considerations**: Implement proper access controls and security headers for production deployment

📊 **Performance Monitoring**: Monitor Apache performance and resource usage with multiple sites

🛡️ **Content Management**: Establish proper backup and update procedures for static website content