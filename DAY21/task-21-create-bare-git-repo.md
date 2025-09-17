**🌟 Task 21 - Create Bare Git Repository on Storage Server**

**📌 Task Description**

The **Nautilus DevOps team** needs to create a **bare Git repository** on the **Storage server** in **Stratos Datacenter** as part of the infrastructure for new application development.

**Requirements:**
- Install **Git package** using yum
- Create a **bare Git repository** at the exact location **`/opt/games.git`**

👉 **Your task:** Set up a centralized bare Git repository for team collaboration and version control.

💡 **Note:** You can find the infrastructure details by clicking on the **Details of all Users and Servers** button on the top-right section of the page.

---

## 🔹 Step 1: Connect to Storage Server

```bash
ssh natasha@ststor01
sudo su -
```

**Purpose**: Connect to the storage server (ststor01) as natasha user and switch to root for administrative tasks.

---

## 🔹 Step 2: Check if Git is already installed

```bash
git --version
```

**Purpose**: Check if Git is already installed on the system.

**Expected Output (if not installed)**:
```
bash: git: command not found
```

**If already installed**: You'll see the Git version number.

---

## 🔹 Step 3: Install Git using yum

```bash
yum install git -y
```

**Purpose**: Install Git version control system using the yum package manager.

**For systems using dnf**:
```bash
dnf install git -y
```

**Installation Note**: This installs Git with all necessary dependencies for repository management.

---

## 🔹 Step 4: Verify Git installation

```bash
git --version
```

**Purpose**: Confirm Git is properly installed and accessible.

**Expected Output**:
```
git version 2.x.x
```

---

## 🔹 Step 5: Create bare Git repository

```bash
git init --bare /opt/games.git
```

**Purpose**: Initialize a bare Git repository at the specified location `/opt/games.git`.

**Expected Output**:
```
Initialized empty Git repository in /opt/games.git/
```

**Bare Repository Explanation**: A bare repository contains only Git metadata without a working directory, making it ideal for central repositories that multiple developers can push to.

---

## 🔹 Step 6: Verify repository creation

```bash
cd /opt/
ls -la
```

**Purpose**: Navigate to `/opt/` directory and list contents to verify the repository was created.

**Expected Output**:
```
drwxr-xr-x. 7 root root  119 Mar 15 10:30 games.git
```

---

## 🔹 Step 7: Examine repository structure

```bash
ls -la games.git/
```

**Purpose**: View the internal structure of the bare Git repository.

**Expected Contents**:
```
drwxr-xr-x. 2 root root   6 Mar 15 10:30 branches
-rw-r--r--. 1 root root  66 Mar 15 10:30 config
-rw-r--r--. 1 root root  73 Mar 15 10:30 description
-rw-r--r--. 1 root root  23 Mar 15 10:30 HEAD
drwxr-xr-x. 2 root root 242 Mar 15 10:30 hooks
drwxr-xr-x. 2 root root  21 Mar 15 10:30 info
drwxr-xr-x. 4 root root  30 Mar 15 10:30 objects
drwxr-xr-x. 4 root root  31 Mar 15 10:30 refs
```

**Repository Components**:
- **HEAD**: Points to current branch
- **config**: Repository configuration
- **objects/**: Git object database
- **refs/**: Branch and tag references
- **hooks/**: Git hooks for automation

---

## 🔹 Step 8: Check repository configuration

```bash
cat games.git/config
```

**Purpose**: View the Git repository configuration to confirm it's set up as a bare repository.

**Expected Output**:
```
[core]
    repositoryformatversion = 0
    filemode = true
    bare = true
```

**Key Setting**: `bare = true` confirms this is a bare repository.

---

## 🔹 Step 9: Set appropriate permissions (optional)

```bash
chown -R natasha:natasha /opt/games.git
chmod -R 755 /opt/games.git
```

**Purpose**: Set proper ownership and permissions if needed for team access.

**Permission Considerations**:
- **Owner**: Set to appropriate user/group for repository access
- **Permissions**: 755 allows read/execute for group and others

---

## 🔹 Step 10: Test repository accessibility

```bash
cd /opt/games.git
git rev-parse --is-bare-repository
```

**Purpose**: Verify the repository is properly configured as a bare repository.

**Expected Output**:
```
true
```

---

## 🔹 Step 11: Display repository path for team reference

```bash
pwd
echo "Bare Git repository created at: $(pwd)"
```

**Purpose**: Confirm and display the full path for team documentation.

**Expected Output**:
```
/opt/games.git
Bare Git repository created at: /opt/games.git
```

---

## 📋 Quick Command Reference

For quick copy-paste, execute on **Storage Server (ststor01)**:

```bash
# Connect to storage server
ssh natasha@ststor01
sudo su -

# Install Git and create bare repository
yum install git -y
git --version
git init --bare /opt/games.git

# Verify creation
cd /opt/
ls -la games.git/
cat games.git/config

# Test repository
cd /opt/games.git
git rev-parse --is-bare-repository
```

---

## 💡 Additional Tips

- **Repository naming**: The `.git` extension is conventional for bare repositories
- **Network access**: Configure SSH or HTTP access for remote team members
- **Backup strategy**: Implement regular backups of the repository
- **Hook scripts**: Use Git hooks for automated testing or deployment
- **Access control**: Set up appropriate file permissions and user groups

---

## 🔧 Troubleshooting Common Issues

### **Issue 1: Git command not found after installation**
**Symptoms**: `bash: git: command not found` after yum install
**Solution**: Verify package installation and refresh shell
```bash
which git
hash -r
source ~/.bashrc
```

### **Issue 2: Permission denied when creating repository**
**Symptoms**: Cannot create repository in `/opt/` directory
**Solution**: Ensure you have proper sudo/root privileges
```bash
sudo su -
# or
sudo git init --bare /opt/games.git
```

### **Issue 3: Repository appears to be regular instead of bare**
**Symptoms**: Repository contains working directory files
**Solution**: Verify the `--bare` flag was used and check config
```bash
cat /opt/games.git/config
# Should show bare = true
```

### **Issue 4: Cannot access repository from other users**
**Symptoms**: Team members cannot clone or access repository
**Solution**: Set proper ownership and permissions
```bash
chown -R git:git /opt/games.git
chmod -R 755 /opt/games.git
```

---

## 🚨 Task-Specific Challenge & Solution

**🔍 Main Challenge Encountered:**

The primary challenge was **setting up a centralized bare Git repository** on a storage server that would serve as the central collaboration point for development teams, requiring proper installation, initialization, and configuration.

**💡 Solution Approach:**

1. **Git Installation**: Used yum package manager to install Git version control system on the storage server
2. **Bare Repository Creation**: Initialized a bare repository using `git init --bare` at the exact specified location `/opt/games.git`
3. **Repository Verification**: Confirmed proper bare repository structure and configuration
4. **Permission Management**: Ensured appropriate access permissions for team collaboration
5. **Configuration Validation**: Verified repository settings and bare repository status

**🎯 Key Success Factors:**
- **Exact path specification** ensuring repository created at `/opt/games.git` as required
- **Bare repository initialization** using `--bare` flag for proper central repository setup
- **Installation verification** confirming Git package installed and accessible
- **Repository structure validation** checking internal Git directory structure
- **Configuration confirmation** verifying `bare = true` in repository config

**⚠️ Critical Configuration Details:**
- **Bare repository requirement** - must use `--bare` flag to create proper central repository
- **Location specification** - exact path `/opt/games.git` as per requirements
- **Repository structure** - bare repositories contain only Git metadata, no working directory
- **Team access** - bare repositories allow multiple developers to push changes safely

**🔒 Repository Benefits:**
- **Central collaboration** - serves as authoritative source for team development
- **Safe pushing** - bare repositories prevent conflicts with working directory
- **Storage efficiency** - contains only Git metadata without workspace files
- **Multi-user support** - designed for concurrent access from multiple developers

---

## ⚠️ Important Production Notes

🔧 **Version Control Hub**: Bare repository configured as central collaboration point for development teams

🔐 **Access Management**: Consider implementing proper user groups and SSH key authentication for team access

📊 **Repository Maintenance**: Plan for regular repository maintenance, cleanup, and backup procedures

🛡️ **Security Considerations**: Implement appropriate network security and access controls for remote repository access.