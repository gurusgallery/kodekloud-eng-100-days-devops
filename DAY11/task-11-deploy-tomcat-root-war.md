**🌟 Task 11 - Install and Configure Tomcat Server with ROOT.war Deployment**

**📌 Task Description**

The **Nautilus application development team** finished beta for a Java app and decided to deploy it on **App Server 3** in **Stratos Datacenter** using **Tomcat** as the application server.

**Requirements:**
- Install Tomcat server on **App Server 3**
- Configure Tomcat to run on **port 8088**
- Deploy the `ROOT.war` file located on Jump host `/tmp` to the Tomcat server
- Verify the webpage at `http://stapp03:8088` returns the expected HTML

👉 **Your task:** Set up Tomcat server with custom port configuration and deploy the Java web application.

💡 **Note:** You can find the infrastructure details by clicking on the **Details of all Users and Servers** button on the top-right section of the page.

---

## 🔹 Step 1: Locate and transfer WAR file from Jump Host

**On Jump Host:**

```bash
cd /tmp
ls -l
```

**Purpose**: Navigate to the tmp directory and verify the ROOT.war file exists.

**Expected Output**:
```
-rw-r--r-- 1 root root 12345 Mar 15 10:30 ROOT.war
```

---

## 🔹 Step 2: Copy WAR file to App Server 3

```bash
scp ROOT.war banner@stapp03:/tmp
```

**Purpose**: Copy ROOT.war from Jump Host to App Server 3 using secure copy.

**Note**: 📦 This transfers the web application package to the target server for deployment.

---

## 🔹 Step 3: Login to App Server 3 and verify file transfer

**On App Server 3 (login as banner):**

```bash
cd /tmp
ls -la
```

**Purpose**: Verify that the ROOT.war file was successfully transferred to App Server 3.

**Expected Output**:
```
-rw-r--r-- 1 banner banner 12345 Mar 15 10:30 ROOT.war
```

---

## 🔹 Step 4: Check OS version and install Tomcat

```bash
cat /etc/os-release
sudo yum install tomcat -y
```

**Purpose**: Check the operating system version and install Tomcat server package.

**For Ubuntu/Debian systems**:
```bash
sudo apt-get update
sudo apt-get install tomcat9 -y
```

**Package Installation**: 📦 This installs Tomcat server with default configuration.

---

## 🔹 Step 5: Check initial Tomcat service status

```bash
sudo systemctl status tomcat
```

**Purpose**: Check the current status of Tomcat service (likely inactive after fresh installation).

**Expected Status**: 
- ⭕ **Inactive (dead)** - Service not started yet
- 🔴 **Failed** - Service failed to start (configuration issue)

---

## 🔹 Step 6: Navigate to Tomcat configuration directory

```bash
cd /etc/tomcat/
ls -l
```

**Purpose**: Navigate to Tomcat configuration directory and list configuration files.

**Key Files**:
- `server.xml` - Main server configuration
- `tomcat-users.xml` - User authentication
- `web.xml` - Web application configuration

---

## 🔹 Step 7: Examine current server configuration

```bash
cat server.xml
```

**Purpose**: View the current Tomcat server configuration to locate the Connector port setting.

**Look for**:
```xml
<Connector port="8080" protocol="HTTP/1.1"
           connectionTimeout="20000"
           redirectPort="8443" />
```

---

## 🔹 Step 8: Edit server configuration for port 8088

```bash
sudo vi server.xml
```

**Purpose**: Edit the server.xml file to change Tomcat's HTTP port from default to 8088.

**Configuration Change**:

**Find this line:**
```xml
<Connector port="8080" protocol="HTTP/1.1"
```

**Change to:**
```xml
<Connector port="8088" protocol="HTTP/1.1"
```

**Save and exit**: 
- **In vi**: Press `Esc`, type `:wq`, press `Enter`
- **In nano**: Press `Ctrl+X`, then `Y`, then `Enter`

---

## 🔹 Step 9: Deploy the ROOT.war application

```bash
sudo cp /tmp/ROOT.war /usr/share/tomcat/webapps/
```

**Purpose**: Copy the ROOT.war file to Tomcat's webapps directory for deployment.

**Deployment Location**: 📁 `/usr/share/tomcat/webapps/` is the default deployment directory.

**Verify copy**:
```bash
ls -la /usr/share/tomcat/webapps/
```

---

## 🔹 Step 10: Start and verify Tomcat service

```bash
sudo systemctl start tomcat
sudo systemctl status tomcat
```

**Purpose**: Start the Tomcat service and verify it's running properly.

**Expected Status**:
```
● tomcat.service - Apache Tomcat Web Application Container
   Loaded: loaded
   Active: active (running)
   Main PID: 1234 (java)
```

**Enable for auto-start**:
```bash
sudo systemctl enable tomcat
```

---

## 🔹 Step 11: Verify port configuration

```bash
sudo netstat -tlnp | grep 8088
```

**Purpose**: Confirm that Tomcat is listening on the correct port 8088.

**Expected Output**:
```
tcp6    0    0 :::8088    :::*    LISTEN    1234/java
```

**Alternative command**:
```bash
sudo ss -tlnp | grep 8088
```

---

## 🔹 Step 12: Test web application deployment

```bash
curl http://stapp03:8088
```

**Purpose**: Test the deployed web application by making an HTTP request to the server.

**Expected HTML Output**:
```html
<html>
<head>
    <title>SampleWebApp</title>
    <meta charset="UTF-8">
    <!-- ... -->
</head>
<body>
    <h2>Welcome to xFusionCorp Industries!</h2>
    <br>
</body>
</html>
```

**Success Indicator**: ✅ This confirms Tomcat is running on port 8088 and serving the ROOT web application.

---

## 🔹 Step 13: Additional verification tests

```bash
# Check Tomcat logs
sudo tail -f /var/log/tomcat/catalina.out

# Check if WAR file was extracted
ls -la /usr/share/tomcat/webapps/ROOT/

# Test from different location
curl http://localhost:8088
```

**Purpose**: Perform additional checks to ensure complete deployment success.

---

## 📋 Quick Command Reference

For quick copy-paste, here are all commands in sequence:

```bash
# On Jump Host - Transfer WAR file
cd /tmp
ls -l
scp ROOT.war banner@stapp03:/tmp

# On App Server 3 - Install and configure
cd /tmp
ls -la
cat /etc/os-release
sudo yum install tomcat -y
sudo systemctl status tomcat

# Configure port 8088
cd /etc/tomcat/
sudo vi server.xml
# (Change port from 8080 to 8088)

# Deploy application
sudo cp /tmp/ROOT.war /usr/share/tomcat/webapps/
sudo systemctl start tomcat
sudo systemctl enable tomcat
sudo systemctl status tomcat

# Verify deployment
sudo netstat -tlnp | grep 8088
curl http://stapp03:8088
```

---

## 💡 Additional Tips

- **Check Tomcat version**: `sudo /usr/share/tomcat/bin/version.sh`
- **Monitor deployment**: `sudo tail -f /var/log/tomcat/catalina.out`
- **Firewall configuration**: `sudo firewall-cmd --permanent --add-port=8088/tcp`
- **Memory tuning**: Edit `/etc/tomcat/tomcat.conf` for heap size adjustments
- **Manager app access**: Configure `tomcat-users.xml` for web management

---

## 🔧 Troubleshooting Common Issues

### **Issue 1: Tomcat fails to start**
**Symptoms**: Service status shows "failed" or "inactive"
**Solution**: Check `/var/log/tomcat/catalina.out` for error messages
**Common cause**: Port already in use or configuration syntax error

### **Issue 2: Port 8088 not accessible**
**Symptoms**: `curl` returns connection refused
**Solution**: Verify port configuration and firewall settings
```bash
sudo firewall-cmd --list-ports
sudo firewall-cmd --permanent --add-port=8088/tcp
sudo firewall-cmd --reload
```

### **Issue 3: WAR file not deploying**
**Symptoms**: ROOT directory not created in webapps
**Solution**: Check file permissions and Tomcat logs
```bash
sudo chown tomcat:tomcat /usr/share/tomcat/webapps/ROOT.war
sudo systemctl restart tomcat
```

### **Issue 4: Java not found**
**Symptoms**: Tomcat fails with "java command not found"
**Solution**: Install Java JDK/JRE
```bash
sudo yum install java-1.8.0-openjdk -y
```

---

## 🚨 Task-Specific Challenge & Solution

**🔍 Main Challenge Encountered:**

The primary challenge was **configuring Tomcat to run on a non-standard port (8088)** instead of the default port 8080, while ensuring the web application deploys correctly and is accessible.

**💡 Solution Approach:**

1. **Port Configuration**: Modified the `server.xml` file to change the HTTP Connector port from 8080 to 8088
2. **Proper WAR Deployment**: Ensured the ROOT.war file was placed in the correct webapps directory (`/usr/share/tomcat/webapps/`)
3. **Service Management**: Used systemctl to properly start and enable the Tomcat service
4. **Verification Process**: Used curl to test the application response and netstat to confirm port binding

**🎯 Key Success Factors:**
- **Correct server.xml editing** to change the Connector port configuration
- **Proper file placement** of ROOT.war in the webapps directory for automatic extraction
- **Service restart** after configuration changes to apply the new port setting
- **Network verification** to ensure Tomcat was listening on the correct port

**⚠️ Critical Configuration Detail:**
The ROOT.war deployment creates the root context ("/") of the web application, making it accessible directly at `http://stapp03:8088` without additional path segments.

---

## ⚠️ Important Production Notes

🔧 **Service Management**: Always enable Tomcat service for automatic startup on server reboot

🔐 **Security Considerations**: Configure proper firewall rules for port 8088 access

📊 **Monitoring**: Regularly check Tomcat logs for application performance and errors

🛡️ **Backup**: Keep backup copies of server.xml before making configuration changes