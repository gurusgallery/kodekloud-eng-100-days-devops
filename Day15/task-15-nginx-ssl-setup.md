**🌟 Task 15 - Install and Configure Nginx with SSL on App Server 3**

**📌 Task Description**

The system admins need to prepare **App Server 3** for application deployment with the following requirements:

1. Install and configure **Nginx** on **App Server 3**
2. Move the self-signed SSL certificate and key from `/tmp/nautilus.crt` and `/tmp/nautilus.key` to appropriate locations and deploy them in Nginx
3. Create an `index.html` file under Nginx document root with content **"Welcome!"**
4. Verify accessibility from jump host using `curl -Ik https://<app-server-ip>/`

👉 **Your task:** Set up Nginx with SSL/TLS encryption using provided certificates and create a basic welcome page.

💡 **Note:** You can find the infrastructure details by clicking on the **Details of all Users and Servers** button on the top-right section of the page.

---

## 🔹 Step 1: Connect to App Server 3

```bash
ssh banner@stapp03
sudo su -
```

**Purpose**: Connect to App Server 3 as the banner user and switch to root for administrative tasks.

---

## 🔹 Step 2: Verify SSL certificate files

```bash
cd /tmp
ls -la
```

**Purpose**: Verify that the SSL certificate and key files are present in the temporary directory.

**Expected Files**:
```
-rw-r--r-- 1 root root 1234 Mar 15 10:30 nautilus.crt
-rw------- 1 root root 5678 Mar 15 10:30 nautilus.key
```

**File Verification**:
```bash
file nautilus.crt nautilus.key
```

---

## 🔹 Step 3: Check OS version and install Nginx

```bash
cat /etc/os-release
yum install nginx -y
```

**Purpose**: Check the operating system version and install Nginx web server.

**For Ubuntu/Debian systems**:
```bash
apt-get update
apt-get install nginx -y
```

**Package Installation**: 📦 This installs Nginx with default configuration and SSL module support.

---

## 🔹 Step 4: Start and verify Nginx service

```bash
systemctl start nginx
systemctl status nginx
```

**Purpose**: Start the Nginx service and verify it's running properly.

**Expected Output**:
```
● nginx.service - The nginx HTTP and reverse proxy server
   Loaded: loaded
   Active: active (running)
   Main PID: 1234 (nginx)
```

**Enable for auto-start**:
```bash
systemctl enable nginx
```

---

## 🔹 Step 5: Move SSL certificate files to standard locations

```bash
mv /tmp/nautilus.crt /etc/pki/tls/certs/
mv /tmp/nautilus.key /etc/pki/tls/private/
```

**Purpose**: Move SSL certificate and key files to their standard system locations for security and organization.

**Directory Creation** (if needed):
```bash
mkdir -p /etc/pki/tls/certs /etc/pki/tls/private
```

**Verify file placement**:
```bash
ls -l /etc/pki/tls/certs/ | grep nautilus.crt
ls -l /etc/pki/tls/private/ | grep nautilus.key
```

---

## 🔹 Step 6: Set proper permissions on SSL files

```bash
chmod 644 /etc/pki/tls/certs/nautilus.crt
chmod 600 /etc/pki/tls/private/nautilus.key
```

**Purpose**: Set appropriate permissions for SSL files - certificate readable, private key secure.

**Security Note**: 🔐 Private key should only be readable by root (600 permissions).

---

## 🔹 Step 7: Configure Nginx for SSL

```bash
vi /etc/nginx/nginx.conf
```

**Purpose**: Edit Nginx configuration to enable SSL/TLS server block with the SSL certificates.

**Configuration Changes Required**:

**Find the commented SSL server block and uncomment/modify it:**

```nginx
# HTTPS server configuration
server {
    listen       443 ssl http2 default_server;
    listen       [::]:443 ssl http2 default_server;
    server_name  _;
    root         /usr/share/nginx/html;

    # SSL Configuration
    ssl_certificate "/etc/pki/tls/certs/nautilus.crt";
    ssl_certificate_key "/etc/pki/tls/private/nautilus.key";
    ssl_session_cache shared:SSL:1m;
    ssl_session_timeout  10m;
    ssl_ciphers PROFILE=SYSTEM;
    ssl_prefer_server_ciphers on;

    # Load configuration files for the default server block.
    include /etc/nginx/default.d/*.conf;

    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
    }

    error_page   404              /404.html;
    location = /40x.html {
    }

    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
    }
}
```

**Critical Configuration Points**:
- **Listen on 443**: SSL/HTTPS port
- **SSL Certificate Path**: `/etc/pki/tls/certs/nautilus.crt`
- **SSL Key Path**: `/etc/pki/tls/private/nautilus.key`
- **Document Root**: `/usr/share/nginx/html`

---

## 🔹 Step 8: Verify Nginx configuration syntax

```bash
nginx -t
```

**Purpose**: Test Nginx configuration syntax before restarting the service.

**Expected Output**:
```
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

**If errors found**: Review and fix configuration file syntax before proceeding.

---

## 🔹 Step 9: Restart and verify Nginx service

```bash
systemctl restart nginx
systemctl status nginx
```

**Purpose**: Restart Nginx service with the new SSL configuration and verify it's running properly.

**Expected Status**: ✅ **Active (running)** with SSL configuration loaded.

**Check listening ports**:
```bash
netstat -tulnp | grep nginx
```

**Expected Output**:
```
tcp        0      0 0.0.0.0:80          0.0.0.0:*         LISTEN      1234/nginx
tcp        0      0 0.0.0.0:443         0.0.0.0:*         LISTEN      1234/nginx
```

---

## 🔹 Step 10: Create the welcome page

```bash
cd /usr/share/nginx/html/
rm -f index.html
echo "Welcome!" > index.html
```

**Purpose**: Create the required index.html file under Nginx document root with the specified content.

**Verify file creation**:
```bash
cat index.html
ls -la index.html
```

**Expected Output**:
```
Welcome!
```

**Set proper permissions**:
```bash
chmod 644 index.html
chown nginx:nginx index.html
```

---

## 🔹 Step 11: Test local SSL connectivity

```bash
curl -Ik https://localhost/
```

**Purpose**: Test SSL connectivity locally on the server to verify basic functionality.

**Expected Output**:
```
HTTP/1.1 200 OK
Server: nginx/1.20.1
Date: ...
Content-Type: text/html
Content-Length: 8
```

**Alternative local test**:
```bash
curl -k https://127.0.0.1/
```

---

## 🔹 Step 12: Get server IP address

```bash
ip addr show | grep "inet " | grep -v 127.0.0.1
```

**Purpose**: Get the actual IP address of App Server 3 for external testing.

**Expected Output Example**:
```
inet 172.16.238.12/24 brd 172.16.238.255 scope global eth0
```

**Server IP**: `172.16.238.12`

---

## 🔹 Step 13: Test from Jump Host

**From Jump Host:**

```bash
curl -Ik https://172.16.238.12/
```

**Purpose**: Test HTTPS accessibility from Jump Host to verify external connectivity and SSL configuration.

**Expected Output**:
```
HTTP/1.1 200 OK
Server: nginx/1.20.1
Date: Wed, 15 Mar 2023 15:30:00 GMT
Content-Type: text/html
Content-Length: 8
Last-Modified: Wed, 15 Mar 2023 15:25:00 GMT
Connection: keep-alive
ETag: "64116c5c-8"
Accept-Ranges: bytes
```

**Success Indicators**:
- ✅ **HTTP/1.1 200 OK** status
- ✅ **Server: nginx** header
- ✅ **SSL connection established** (no certificate errors with -k flag)

---

## 🔹 Step 14: Additional verification tests

```bash
# Test content retrieval
curl -k https://172.16.238.12/

# Test SSL certificate details
openssl s_client -connect 172.16.238.12:443 -servername 172.16.238.12 < /dev/null

# Test different curl options
curl -Iv https://172.16.238.12/
```

**Purpose**: Perform comprehensive testing to ensure all requirements are met.

---

## 📋 Quick Command Reference

For quick copy-paste, execute on **App Server 3**:

```bash
# Connect and setup
ssh banner@stapp03
sudo su -

# Verify files and install Nginx
cd /tmp && ls -la
yum install nginx -y
systemctl start nginx && systemctl enable nginx
systemctl status nginx

# Move and secure SSL files
mv /tmp/nautilus.crt /etc/pki/tls/certs/
mv /tmp/nautilus.key /etc/pki/tls/private/
chmod 644 /etc/pki/tls/certs/nautilus.crt
chmod 600 /etc/pki/tls/private/nautilus.key

# Configure Nginx SSL
vi /etc/nginx/nginx.conf
# (Enable and configure SSL server block)

# Test and restart
nginx -t
systemctl restart nginx
systemctl status nginx

# Create welcome page
cd /usr/share/nginx/html/
echo "Welcome!" > index.html
chmod 644 index.html

# Test locally
curl -Ik https://localhost/
```

**Testing from Jump Host:**
```bash
curl -Ik https://172.16.238.12/
```

---

## 💡 Additional Tips

- **Certificate validation**: Use `openssl x509 -in /etc/pki/tls/certs/nautilus.crt -text -noout` to view certificate details
- **SSL cipher testing**: Use `nmap --script ssl-enum-ciphers -p 443 172.16.238.12` to test SSL configuration
- **Log monitoring**: Check `/var/log/nginx/error.log` for SSL-related errors
- **Firewall considerations**: Ensure port 443 is open if firewall is active
- **SELinux context**: Set appropriate SELinux context for SSL files if needed

---

## 🔧 Troubleshooting Common Issues

### **Issue 1: SSL certificate permission errors**
**Symptoms**: Nginx fails to start, SSL certificate errors in logs
**Solution**: Ensure proper permissions and ownership
```bash
chmod 644 /etc/pki/tls/certs/nautilus.crt
chmod 600 /etc/pki/tls/private/nautilus.key
chown root:root /etc/pki/tls/certs/nautilus.crt /etc/pki/tls/private/nautilus.key
```

### **Issue 2: Nginx configuration syntax errors**
**Symptoms**: `nginx -t` shows syntax errors
**Solution**: Check for missing brackets, semicolons, and proper indentation
```bash
nginx -t
vi /etc/nginx/nginx.conf
# Fix syntax errors reported
```

### **Issue 3: Port 443 not accessible**
**Symptoms**: curl from jump host fails with connection refused
**Solution**: Check firewall and service status
```bash
systemctl status nginx
netstat -tulnp | grep :443
firewall-cmd --list-ports
firewall-cmd --permanent --add-port=443/tcp
firewall-cmd --reload
```

### **Issue 4: SELinux blocking SSL certificate access**
**Symptoms**: Nginx starts but SSL handshake fails
**Solution**: Set appropriate SELinux context
```bash
restorecon -Rv /etc/pki/tls/
setsebool -P httpd_can_network_connect 1
```

---

## 🚨 Task-Specific Challenge & Solution

**🔍 Main Challenges Encountered:**

1. **SSL Configuration**: Properly configuring Nginx to use self-signed SSL certificates with correct file paths and permissions
2. **Certificate Management**: Moving SSL files to appropriate system locations while maintaining security
3. **Service Integration**: Ensuring Nginx starts successfully with SSL configuration and serves content properly

**💡 Solution Approach:**

1. **Systematic File Management**: Moved SSL certificate and private key to standard system locations (`/etc/pki/tls/certs/` and `/etc/pki/tls/private/`)
2. **Security-First Permissions**: Applied appropriate permissions (644 for certificate, 600 for private key) to maintain security
3. **Nginx SSL Configuration**: Enabled the SSL server block in `/etc/nginx/nginx.conf` with proper certificate paths
4. **Configuration Validation**: Used `nginx -t` to verify syntax before service restart
5. **Content Deployment**: Created the required index.html file in the correct document root location
6. **Comprehensive Testing**: Verified functionality both locally and from the Jump Host using curl with SSL options

**🎯 Key Success Factors:**
- **Proper SSL file placement** in standard system directories for organization and security
- **Correct Nginx configuration** with accurate certificate paths and SSL directives
- **Permission management** ensuring certificate accessibility while protecting private key
- **Syntax validation** before service restart to prevent configuration errors
- **Multi-point testing** from both local and remote hosts to ensure complete functionality

**⚠️ Critical Configuration Details:**
- **SSL server block** must be properly uncommented and configured in nginx.conf
- **Certificate paths** must exactly match the file locations in the configuration
- **Document root** must be set correctly for the index.html file to be served
- **Self-signed certificates** require the `-k` flag in curl for testing (insecure flag)

**🔒 Security Implementation:**
- **Private key protection** with 600 permissions (root access only)
- **Certificate accessibility** with 644 permissions for service access
- **Standard system locations** for SSL files following best practices
- **SSL/TLS encryption** for secure web communication

---

## ⚠️ Important Production Notes

🔧 **SSL Certificate Management**: In production, use certificates from trusted Certificate Authorities instead of self-signed certificates

🔐 **Security Hardening**: Implement additional SSL security measures like HSTS, OCSP stapling, and strong cipher suites

📊 **Monitoring**: Set up monitoring for SSL certificate expiration and renewal processes

🛡️ **Backup Strategy**: Regularly backup SSL certificates and private keys in secure locations