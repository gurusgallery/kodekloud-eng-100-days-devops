**🌟 Task 5 - Install and Disable SELinux Temporarily**

**📌 Task Description**

Following a security audit, the **xFusionCorp Industries** security team has opted to enhance application and server security with SELinux. To initiate testing, the following requirements have been established for **App Server 2** in the **Stratos Datacenter**:

- Install the required SELinux packages
- Permanently disable SELinux for the time being; it will be re-enabled after necessary configuration changes
- No need to reboot the server, as a scheduled maintenance reboot is already planned for tonight
- Disregard the current status of SELinux via the command line; the final status after the reboot should be disabled

👉 **Your task:** Install SELinux packages and configure permanent disabling on **App Server 2**.

💡 **Note:** You can find the infrastructure details by clicking on the **Details of all Users and Servers** button on the top-right section of the page.

---

## 🚨 Task-Specific Challenge & Solution

**🔍 Main Challenge Encountered:**

The primary challenge was **installing SELinux packages while simultaneously disabling SELinux** for testing purposes, requiring careful configuration management to avoid conflicts between installation and operational requirements.

**💡 Solution Approach:**

1. **System Compatibility**: Verified OS version using `cat /etc/os-release` to ensure SELinux package compatibility
2. **Package Installation**: Installed required SELinux packages (`selinux-policy`, `libselinux-utils`) for future testing needs
3. **Configuration Management**: Modified `/etc/selinux/config` to set `SELINUX=disabled` for current testing phase
4. **Reboot Planning**: Coordinated with planned maintenance window to avoid immediate system restart
5. **Status Verification**: Used configuration file checks to confirm proper disable settings

**🎯 Key Success Factors:**
- **Package preparation** for future SELinux enablement while meeting current testing requirements
- **Configuration file management** to control SELinux behavior across system reboots
- **Maintenance window coordination** to apply changes during scheduled downtime
- **Verification procedures** to ensure configuration changes are properly saved

**⚠️ Critical Learning Points:**
- **SELinux installation** doesn't automatically enable enforcement - configuration controls behavior
- **Configuration persistence** requires proper `/etc/selinux/config` file modification
- **Reboot dependency** for SELinux mode changes to take full effect
- **Testing phases** may require temporary security policy adjustments

---

## 🔹 Step 1: Check OS version compatibility

```bash
cat /etc/os-release
```

**Purpose**: Check the OS version to confirm it's compatible with SELinux packages.

**Expected Output Example**:
```
NAME="CentOS Linux"
VERSION="7 (Core)"
ID="centos"
```

**Compatibility Note**: 📋 SELinux is supported on RHEL, CentOS, Fedora, and similar distributions.

---

## 🔹 Step 2: Install required SELinux packages

```bash
sudo yum install -y selinux-policy libselinux-utils
```

**Purpose**: Install required SELinux packages using the package manager.

**Package Details**:
- `selinux-policy` - Core SELinux policy files
- `libselinux-utils` - SELinux utility programs

**For Ubuntu/Debian systems** (alternative):
```bash
sudo apt-get install -y selinux-policy-default selinux-utils
```

---

## 🔹 Step 3: Check current SELinux status (optional)

```bash
sestatus
```

**Purpose**: View current SELinux status before making changes.

**Possible Outputs**:
```
SELinux status:                 enabled
SELinuxfs mount:                /sys/fs/selinux
SELinux root directory:         /etc/selinux
Loaded policy name:             targeted
Current mode:                   enforcing
```

---

## 🔹 Step 4: Open SELinux configuration file

```bash
sudo vi /etc/selinux/config
```

**Purpose**: Open the SELinux configuration file for editing.

**Alternative editors**:
```bash
sudo nano /etc/selinux/config
sudo gedit /etc/selinux/config
```

---

## 🔹 Step 5: Configure permanent SELinux disable

**What you'll see in the file:**
```bash
# This file controls the state of SELinux on the system.
SELINUX=enforcing
SELINUXTYPE=targeted
```

**Modify the configuration file to change:**

From: `SELINUX=enforcing`  
To: `SELINUX=disabled`

**Purpose**: This will disable SELinux permanently after the next reboot.

**Configuration Options**:
- `SELINUX=enforcing` - SELinux security policy is enforced (current default)
- `SELINUX=permissive` - SELinux prints warnings instead of enforcing
- `SELINUX=disabled` - No SELinux policy is loaded (target setting)

**Important**: 🔄 Changes take effect after the planned maintenance reboot tonight.

---

## 🔹 Step 6: Verify configuration changes

```bash
cat /etc/selinux/config | grep SELINUX=
```

**Purpose**: Verify that the configuration has been updated correctly.

**Expected Output**:
```
SELINUX=disabled
```

---

## 📋 Quick Command Reference

For quick copy-paste, here are all commands in sequence:

```bash
# Check OS compatibility
cat /etc/os-release

# Install SELinux packages
sudo yum install -y selinux-policy libselinux-utils

# Check current status (optional)
sestatus

# Open config file for editing
sudo vi /etc/selinux/config

# Verify configuration
cat /etc/selinux/config | grep SELINUX=
```

---

## 💡 Additional Tips

- **Backup config before changes**: `sudo cp /etc/selinux/config /etc/selinux/config.backup`
- **Check package installation**: `rpm -qa | grep selinux`
- **Alternative package manager**: Use `dnf` instead of `yum` on newer RHEL/CentOS versions
- **Temporary disable** (without reboot): `sudo setenforce 0` (not required for this task)
- **Re-enable later**: Change `SELINUX=disabled` to `SELINUX=enforcing` in config file

---

## ⚠️ Important Reminders

🔧 **No reboot required now** - The scheduled maintenance reboot tonight will apply the changes

🔒 **Security consideration** - SELinux will be re-enabled after necessary configuration changes

📅 **Timeline** - Changes become effective after the planned maintenance reboot