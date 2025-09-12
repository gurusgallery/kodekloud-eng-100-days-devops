**🌟 Task 12 - Troubleshoot and Fix Apache Service Connectivity on Port 3002**

**📌 Task Description**

The monitoring tool reported that **Apache service** on **App Server 1** in **Stratos Datacenter** is not reachable on **port 3002**. Possible issues include the service being down, firewall blocking, or other network issues.

👉 **Your task:** Use network troubleshooting tools such as `telnet`, `netstat`, `iptables`, etc., to identify and fix the problem. Confirm Apache is reachable from the jump host without compromising security.

💡 **Note:** You can find the infrastructure details by clicking on the **Details of all Users and Servers** button on the top-right section of the page.

---

## 🔹 Step 1: Test connectivity from Jump Host

**From Jump Host:**

```bash
telnet stapp01 3002
```

**Purpose**: Test if Apache service on App Server 1 is reachable on port 3002.

**Expected Problem Output**:
```
Trying [IP_ADDRESS]...
telnet: connect to address [IP_ADDRESS]: No route to host
```

**Status**: ❌ **Connection failed - No route to host**

---

## 🔹 Step 2: Test other servers for comparison

```bash
telnet stapp02 3002
telnet stapp03 3002
```

**Purpose**: Verify if the issue is isolated to App Server 1 or affects all servers.

**Expected Results**:
- **stapp02**: ✅ Connection successful
- **stapp03**: ✅ Connection successful

**Conclusion**: 🎯 Issue is **isolated to stapp01** only.

---

## 🔹 Step 3: Login to App Server 1 for diagnosis

**On App Server 1 (login as tony user):**

```bash
sudo su -
```

**Purpose**: Switch to root user for administrative troubleshooting.

---

## 🔹 Step 4: Check Apache service status

```bash
systemctl status httpd
```

**Purpose**: Check the current status of Apache HTTP service.

**Problem Output**:
```
● httpd.service - The Apache HTTP Server
   Loaded: loaded
   Active: failed (Result: exit-code)
   Main PID: 1234 (code=exited, status=1/FAILURE)
```

**Status**: ❌ **Service failed due to port conflict**

---

## 🔹 Step 5: Identify process using port 3002

```bash
netstat -lntp | grep 3002
```

**Purpose**: Identify which process is currently using port 3002.

**Problem Output**:
```
tcp    0    0 127.0.0.1:3002    0.0.0.0:*    LISTEN    447/sendmail
```

**Issue Found**: 🔍 **Port 3002 is being used by sendmail (PID 447)**

**Alternative command**:
```bash
ss -lntp | grep 3002
lsof -i :3002
```

---

## 🔹 Step 6: Terminate the conflicting process

```bash
kill 447
```

**Purpose**: Terminate the sendmail process that's occupying port 3002.

**Verify port is free**:
```bash
netstat -lntp | grep 3002
```

**Expected Result**: No output (port is now free)

**Status**: ✅ **Port 3002 is now available**

---

## 🔹 Step 7: Restart Apache service

```bash
systemctl restart httpd
systemctl status httpd
```

**Purpose**: Restart Apache service now that port 3002 is available.

**Expected Output**:
```
● httpd.service - The Apache HTTP Server
   Loaded: loaded
   Active: active (running)
   Main PID: 1456 (httpd)
```

**Status**: ✅ **Apache service is now running**

---

## 🔹 Step 8: Verify Apache is listening on correct port

```bash
netstat -lntp | grep httpd
```

**Purpose**: Confirm Apache is listening on port 3002.

**Expected Output**:
```
tcp6    0    0 :::3002    :::*    LISTEN    1456/httpd
```

---

## 🔹 Step 9: Test connectivity from Jump Host again

**From Jump Host:**

```bash
telnet stapp01 3002
```

**Purpose**: Test if Apache is now reachable from external host.

**Problem Output**:
```
Trying [IP_ADDRESS]...
telnet: connect to address [IP_ADDRESS]: No route to host
```

**Status**: ❌ **Still blocked - Firewall issue suspected**

---

## 🔹 Step 10: Check firewall rules on App Server 1

**Back on App Server 1:**

```bash
iptables -L -n
```

**Purpose**: Examine current firewall rules to identify blocking rules.

**Problem Output**:
```
Chain INPUT (policy ACCEPT)
target     prot opt source               destination
REJECT     tcp  --  0.0.0.0/0            0.0.0.0/0            tcp dpt:3002 reject-with icmp-port-unreachable
```

**Issue Found**: 🔍 **Firewall REJECT rule is blocking port 3002**

---

## 🔹 Step 11: Add firewall rule to allow port 3002

```bash
sudo iptables -I INPUT 1 -p tcp --dport 3002 -j ACCEPT
```

**Purpose**: Insert a rule at the top of INPUT chain to allow incoming connections on port 3002.

**Command Breakdown**:
- `-I INPUT 1` - Insert at position 1 (highest priority)
- `-p tcp` - Protocol TCP
- `--dport 3002` - Destination port 3002
- `-j ACCEPT` - Jump to ACCEPT target

---

## 🔹 Step 12: Verify firewall rule was added

```bash
iptables -L -n
```

**Purpose**: Confirm the ACCEPT rule was added and takes precedence over REJECT rule.

**Expected Output**:
```
Chain INPUT (policy ACCEPT)
target     prot opt source               destination
ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0            tcp dpt:3002
REJECT     tcp  --  0.0.0.0/0            0.0.0.0/0            tcp dpt:3002 reject-with icmp-port-unreachable
```

**Status**: ✅ **ACCEPT rule is now at position 1 (higher priority)**

---

## 🔹 Step 13: Final connectivity testing from Jump Host

**From Jump Host:**

```bash
telnet stapp01 3002
```

**Purpose**: Test telnet connectivity to verify firewall fix.

**Expected Success Output**:
```
Trying [IP_ADDRESS]...
Connected to stapp01.
Escape character is '^]'.
```

**HTTP test**:
```bash
curl http://stapp01:3002
```

**Expected Result**: Apache default page or configured content

**Status**: ✅ **Both telnet and HTTP connections successful**

---

## 📋 Quick Command Reference

For quick copy-paste, here are all commands in sequence:

```bash
# From Jump Host - Initial testing
telnet stapp01 3002
telnet stapp02 3002
telnet stapp03 3002

# On App Server 1 - Troubleshooting
sudo su -
systemctl status httpd
netstat -lntp | grep 3002

# Kill conflicting process
kill 447
netstat -lntp | grep 3002

# Restart Apache
systemctl restart httpd
systemctl status httpd

# Check firewall
iptables -L -n

# Fix firewall rule
sudo iptables -I INPUT 1 -p tcp --dport 3002 -j ACCEPT
iptables -L -n

# Final testing from Jump Host
telnet stapp01 3002
curl http://stapp01:3002
```

---

## 💡 Additional Tips

- **Save firewall rules**: `sudo iptables-save > /etc/iptables.rules`
- **Check Apache config**: `sudo httpd -t` (syntax check)
- **View Apache logs**: `sudo tail -f /var/log/httpd/error_log`
- **Alternative kill command**: `sudo pkill -f sendmail`
- **Permanent firewall**: Use `firewall-cmd` for persistent rules

---

## 🔧 Troubleshooting Common Issues

### **Issue 1: Process keeps restarting**
**Symptoms**: Process returns after being killed
**Solution**: Disable the service permanently
```bash
sudo systemctl stop sendmail
sudo systemctl disable sendmail
```

### **Issue 2: Firewall rules don't persist**
**Symptoms**: Rules disappear after reboot
**Solution**: Make rules persistent
```bash
sudo iptables-save > /etc/sysconfig/iptables
sudo systemctl enable iptables
```

### **Issue 3: Apache config errors**
**Symptoms**: Service fails to start after restart
**Solution**: Check configuration syntax
```bash
sudo httpd -t
sudo tail /var/log/httpd/error_log
```

### **Issue 4: SELinux blocking connections**
**Symptoms**: Service runs but connections still fail
**Solution**: Check SELinux status and policy
```bash
getenforce
sudo semanage port -l | grep http
sudo semanage port -a -t http_port_t -p tcp 3002
```

---

## 🚨 Task-Specific Challenge & Solution

**🔍 Main Challenges Encountered:**

1. **Port Conflict Issue**: Port 3002 was being used by sendmail process (PID 447), preventing Apache from binding to the port
2. **Firewall Blocking**: Even after resolving the port conflict, firewall rules were blocking incoming connections on port 3002

**💡 Solution Approach:**

1. **Systematic Diagnosis**: Used comparative testing (stapp02, stapp03) to isolate the problem to stapp01
2. **Process Identification**: Used `netstat -lntp | grep 3002` to identify the conflicting sendmail process
3. **Process Termination**: Killed the sendmail process to free port 3002
4. **Service Recovery**: Restarted Apache which successfully bound to port 3002
5. **Firewall Analysis**: Used `iptables -L -n` to identify blocking rules
6. **Rule Prioritization**: Added ACCEPT rule at position 1 to override the REJECT rule

**🎯 Key Success Factors:**
- **Layered troubleshooting approach**: Addressed both service-level and network-level issues
- **Proper rule insertion**: Used `-I INPUT 1` to ensure ACCEPT rule takes precedence
- **Comprehensive verification**: Tested both telnet and HTTP connectivity to confirm full resolution
- **Non-destructive approach**: Added rules without removing existing security policies

**⚠️ Critical Learning Points:**
- **Process conflicts** can prevent services from starting even when configuration is correct
- **Firewall rule order matters** - ACCEPT rules must come before REJECT rules
- **Multiple issues can compound** - solving one problem may reveal another underlying issue

---

## ⚠️ Important Production Notes

🔧 **Service Management**: Always check for process conflicts before assuming configuration issues

🔐 **Security Considerations**: Adding firewall rules should be done carefully to maintain security

📊 **Monitoring**: Implement proper monitoring to detect similar issues proactively

🛡️ **Documentation**: Keep track of firewall changes for security auditing purposes