**🌟 Task 8 - Install Ansible 4.7.0 on Jump Host Using pip3**

**📌 Task Description**

The **Nautilus DevOps team** decided to use Ansible for automation and configuration management due to its simplicity and minimal prerequisites. They want to test using the jump host as the **Ansible controller**.

👉 **Your task:** Install **Ansible version 4.7.0** on the jump host using **pip3 only**. Ensure the Ansible binary is available globally on the system so all users can run Ansible commands.

💡 **Note:** You can find the infrastructure details by clicking on the **Details of all Users and Servers** button on the top-right section of the page.

---

## 🚨 Task-Specific Challenge & Solution

**🔍 Main Challenge Encountered:**

The primary challenge was **installing a specific version of Ansible (4.7.0)** using pip3 while ensuring global system accessibility for all users, requiring precise version management and system-wide installation configuration.

**💡 Solution Approach:**

1. **Pre-Installation Verification**: Checked Python3 and pip3 availability using version commands to ensure installation prerequisites
2. **Specific Version Installation**: Used `sudo pip3 install ansible==4.7.0` to install the exact required version with global privileges
3. **Global Access Configuration**: Applied sudo privileges during installation to place Ansible binaries in system-wide paths (`/usr/local/bin/`)
4. **Version Mapping Understanding**: Recognized that Ansible 4.7.0 corresponds to ansible-core 2.11.12 for proper version verification
5. **Comprehensive Testing**: Verified installation through version checks, component availability, and basic functionality tests

**🎯 Key Success Factors:**
- **Precise version specification** using `ansible==4.7.0` syntax to meet exact requirements
- **Global installation approach** using sudo pip3 for system-wide availability
- **Binary location verification** ensuring ansible commands are accessible from standard PATH locations
- **Multi-component testing** verifying ansible, ansible-playbook, ansible-galaxy, and ansible-vault availability

**⚠️ Critical Learning Points:**
- **Version specificity** is critical for compatibility and reproducible deployments
- **Installation method choice** (pip3 vs package manager) affects version control and global accessibility
- **Ansible version mapping** between package versions and core versions requires understanding
- **Global vs user installation** impacts accessibility for automation scripts and multiple users

---

## 🔹 Step 1: Check Python and pip3 availability

```bash
python3 --version
pip3 --version
```

**Purpose**: Verify that Python3 and pip3 are installed and available on the system.

**Expected Output Example**:
```
Python 3.8.10
pip 20.0.2 from /usr/lib/python3/dist-packages/pip (python 3.8)
```

---

## 🔹 Step 2: Install Ansible 4.7.0 globally using pip3

```bash
sudo pip3 install ansible==4.7.0
```

**Purpose**: Install Ansible version 4.7.0 globally using pip3 with sudo privileges.

**Why sudo**: 🔐 Installing with `sudo` ensures Ansible is available globally for all users on the system.

**Installation Process**: 📦 This will download and install Ansible 4.7.0 and all its dependencies.

---

## 🔹 Step 3: Verify Ansible installation and version

```bash
ansible --version
```

**Purpose**: Verify the installed Ansible version to confirm successful installation.

**Expected Output Example**:
```
ansible [core 2.11.12]
  config file = None
  configured module library = /home/thor/.ansible/plugins/modules:/usr/share/ansible/plugins/modules
  ansible python module location = /usr/local/lib/python3.8/dist-packages/ansible
  executable location = /usr/local/bin/ansible
  python version = 3.8.10 (default, Nov 14 2022, 12:59:47) [GCC 9.4.0] on linux
```

**Key Information**: ✔️ Look for `ansible [core 2.11.12]` which corresponds to Ansible 4.7.0.

---

## 🔹 Step 4: Check Ansible binary location and permissions

```bash
ls -l $(which ansible)
```

**Purpose**: Check the location and permissions of the Ansible binary to ensure it's available globally.

**Expected Output Example**:
```
-rwxr-xr-x 1 root root 227 Mar 15 10:30 /usr/local/bin/ansible
```

**Permission Analysis**: 
- ✔️ **Executable for all users** (`r-x` for group and others)
- ✔️ **Global location** (`/usr/local/bin/`)

---

## 🔹 Step 5: Verify Ansible components are installed

```bash
ansible-playbook --version
ansible-galaxy --version
ansible-vault --version
```

**Purpose**: Verify that all main Ansible components are properly installed and accessible.

**Components**:
- `ansible-playbook` - For running playbooks
- `ansible-galaxy` - For managing roles and collections
- `ansible-vault` - For encrypting sensitive data

---

## 🔹 Step 6: Test Ansible functionality

```bash
ansible localhost -m ping
```

**Purpose**: Test basic Ansible functionality by pinging localhost.

**Expected Output**:
```
localhost | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

---

## 📋 Quick Command Reference

For quick copy-paste, here are all commands in sequence:

```bash
# Check Python and pip3 availability
python3 --version
pip3 --version

# Install Ansible 4.7.0 globally
sudo pip3 install ansible==4.7.0

# Verify installation
ansible --version

# Check binary location and permissions
ls -l $(which ansible)

# Verify components
ansible-playbook --version
ansible-galaxy --version
ansible-vault --version

# Test functionality
ansible localhost -m ping
```

---

## 💡 Additional Tips

- **Check installed packages**: `pip3 list | grep -i ansible`
- **Upgrade pip3** (if needed): `sudo pip3 install --upgrade pip`
- **Uninstall Ansible** (if needed): `sudo pip3 uninstall ansible`
- **Check Ansible path**: `echo $PATH | grep /usr/local/bin`
- **Alternative verification**: `whereis ansible` to see all ansible-related binaries

---

## 🔧 Troubleshooting

**If pip3 is not found:**
```bash
# Install pip3 on CentOS/RHEL
sudo yum install python3-pip

# Install pip3 on Ubuntu/Debian
sudo apt-get install python3-pip
```

**If PATH issues occur:**
```bash
# Add to PATH if needed
export PATH=$PATH:/usr/local/bin
```

---

## 📊 Ansible Version Mapping

| Ansible Package Version | Ansible Core Version | Release Date |
|------------------------|---------------------|--------------|
| 4.7.0 | 2.11.12 | October 2021 |
| 4.8.0 | 2.11.12 | November 2021 |
| 5.0.0 | 2.12.0 | November 2021 |

---

## ⚠️ Important Notes

🎯 **Specific version**: Ensure you install exactly version **4.7.0** as specified

🌐 **Global access**: Using `sudo pip3` ensures all users can run Ansible commands

🔧 **Controller setup**: This configures the jump host as an Ansible controller for managing other servers