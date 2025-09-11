**🌟 Task 3 - Disable Direct SSH Root Login**

**📌 Task Description**

Following security audits, the **xFusionCorp Industries** security team has rolled out new protocols, including the restriction of direct root SSH login.

👉 **Your task:** Disable direct SSH root login on all app servers within the **Stratos Datacenter**.

💡 **Note:** You can find the infrastructure details by clicking on the **Details of all Users and Servers** button on the top-right section of the page.

---

## 🚨 Task-Specific Challenge & Solution

**🔍 Main Challenge Encountered:**

The primary challenge was **implementing security hardening** by disabling direct SSH root login across multiple app servers while maintaining administrative access and avoiding system lockout.

**💡 Solution Approach:**

1. **Multi-Server Implementation**: Applied consistent security configuration across all app servers (app01, app02, app03)
2. **Configuration File Management**: Modified `/etc/ssh/sshd_config` to change `PermitRootLogin` from default to `no`
3. **Service Management**: Used `sudo systemctl restart sshd` to apply configuration changes without system reboot
4. **Verification Process**: Used `grep -i "permitrootlogin" /etc/ssh/sshd_config` to confirm configuration changes
5. **Safety Precautions**: Maintained alternative access methods before implementing root login restrictions

**🎯 Key Success Factors:**
- **Systematic approach** across multiple servers for consistent security posture
- **Safe configuration changes** with proper testing and verification procedures
- **Service restart management** to apply changes without system downtime
- **Alternative access preservation** to prevent administrative lockout scenarios

**⚠️ Critical Learning Points:**
- **Security hardening** should be applied consistently across infrastructure
- **Configuration backup** is essential before making SSH security changes
- **Alternative access methods** must be maintained when restricting root access
- **Testing procedures** should verify both functionality and security compliance

---

## 🔹 Step 1: Login to each app server

**Servers to configure:** `app01`, `app02`, `app03`

```bash
# Example login commands for each server
ssh user@app01
ssh user@app02  
ssh user@app03
```

**Purpose**: Access each application server in the Stratos Datacenter to apply the SSH configuration changes.

---

## 🔹 Step 2: Open SSH daemon configuration file

```bash
sudo vi /etc/ssh/sshd_config
```

**Purpose**: Open the SSH daemon configuration file for editing.

**What to do**: Find the line containing `PermitRootLogin` and modify it to:
```
PermitRootLogin no
```

**Security Note**: 🔒 This setting disables direct root login via SSH, improving system security.

---

## 🔹 Step 3: Restart SSH service to apply changes

```bash
sudo systemctl restart sshd
```

**Purpose**: Restart the SSH service to apply configuration changes.

**Alternative command** (for older systems):
```bash
sudo service ssh restart
```

---

## 🔹 Step 4: Verify the configuration change

```bash
sudo cat /etc/ssh/sshd_config | grep -i "permitrootlogin"
```

**Purpose**: Verify that root login is disabled by checking the configuration.

**Expected Output**:
```
PermitRootLogin no
```

**Additional verification**:
```bash
sudo sshd -T | grep permitrootlogin
```

---

## 📋 Quick Command Reference

For quick copy-paste, here are all commands in sequence:

```bash
# Open SSH config file
sudo vi /etc/ssh/sshd_config

# Restart SSH service
sudo systemctl restart sshd

# Verify configuration
sudo cat /etc/ssh/sshd_config | grep -i "permitrootlogin"

# Alternative verification
sudo sshd -T | grep permitrootlogin
```

---

## 💡 Additional Tips

- **Before making changes**: `sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.backup`
- **Test SSH connection**: Always test from another terminal before closing current session
- **Alternative editors**: Use `nano` instead of `vi` if preferred: `sudo nano /etc/ssh/sshd_config`
- **Check SSH status**: `sudo systemctl status sshd`
- **Repeat process**: Apply these steps on **all remaining app servers** in Stratos Datacenter

---

## ⚠️ Important Security Reminder

🔐 **Always maintain an alternative access method** (console access, another user with sudo privileges) before disabling root SSH login to avoid getting locked out of the system.