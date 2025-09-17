**🌟 Task 22 - Clone Git Repository on Storage Server**

**📌 Task Description**

The **Nautilus development team** requires a copy of an existing Git repository on the **Storage Server** in **Stratos Datacenter**.

**Requirements:**
- The repository located at **`/opt/cluster.git`** must be cloned
- Clone the repository into **`/usr/src/kodekloudrepos/cluster`** directory
- Perform the task as user **`natasha`**
- Do not modify repository or directory permissions

👉 **Your task:** Clone an existing Git repository to a specific location while maintaining proper permissions and user context.

💡 **Note:** You can find the infrastructure details by clicking on the **Details of all Users and Servers** button on the top-right section of the page.

---

## 🔹 Step 1: Connect to Storage Server

```bash
ssh natasha@ststor01
```

**Purpose**: Connect to the storage server (ststor01) as the natasha user.

**Important**: Do not switch to root user as the task must be performed as user natasha.

---

## 🔹 Step 2: Verify source repository exists

```bash
cd /opt/
ls -la
```

**Purpose**: Navigate to `/opt/` directory and verify the presence of `cluster.git` repository.

**Expected Output**:
```
drwxr-xr-x. 7 root root  119 Mar 15 10:30 cluster.git
```

**Verification**: Ensure `cluster.git` directory exists and is accessible.

---

## 🔹 Step 3: Check source repository structure

```bash
ls -la cluster.git/
```

**Purpose**: Examine the source repository structure to confirm it's a valid Git repository.

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

---

## 🔹 Step 4: Check if target directory exists

```bash
ls -ld /usr/src/kodekloudrepos/ 2>/dev/null || echo "Directory does not exist"
```

**Purpose**: Check if the parent directory for the clone destination exists.

**If directory doesn't exist**: We'll need to create it during the clone process.

---

## 🔹 Step 5: Create parent directory if needed

```bash
mkdir -p /usr/src/kodekloudrepos/
```

**Purpose**: Create the parent directory structure if it doesn't exist.

**Note**: The `-p` flag creates parent directories as needed.

---

## 🔹 Step 6: Clone the repository

```bash
git clone /opt/cluster.git /usr/src/kodekloudrepos/cluster
```

**Purpose**: Clone the repository from `/opt/cluster.git` to the specified destination directory.

**Expected Output**:
```
Cloning into '/usr/src/kodekloudrepos/cluster'...
done.
```

**Clone Process**: This creates a full working copy of the repository with complete history.

---

## 🔹 Step 7: Verify successful clone

```bash
ls -ld /usr/src/kodekloudrepos/cluster
```

**Purpose**: Confirm the target directory was created successfully.

**Expected Output**:
```
drwxr-xr-x. 3 natasha natasha 4096 Mar 15 10:30 /usr/src/kodekloudrepos/cluster
```

**Ownership**: Directory should be owned by natasha user as expected.

---

## 🔹 Step 8: Check cloned repository contents

```bash
ls -la /usr/src/kodekloudrepos/cluster
```

**Purpose**: List the contents of the cloned repository to verify successful clone.

**Expected Contents**:
```
drwxr-xr-x. 3 natasha natasha 4096 Mar 15 10:30 .
drwxr-xr-x. 3 natasha natasha 4096 Mar 15 10:30 ..
drwxr-xr-x. 8 natasha natasha 4096 Mar 15 10:30 .git
-rw-r--r--. 1 natasha natasha   XX Mar 15 10:30 [repository files]
```

**Key Elements**:
- `.git/` directory containing repository metadata
- Any files that were in the original repository

---

## 🔹 Step 9: Navigate to cloned repository

```bash
cd /usr/src/kodekloudrepos/cluster
```

**Purpose**: Change to the cloned repository directory for verification commands.

---

## 🔹 Step 10: Verify Git repository status

```bash
git status
```

**Purpose**: Confirm the directory is a valid Git working directory.

**Expected Output**:
```
On branch master
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean
```

---

## 🔹 Step 11: Check remote configuration

```bash
git remote -v
```

**Purpose**: Verify the remote URL is set correctly to the source repository.

**Expected Output**:
```
origin  /opt/cluster.git (fetch)
origin  /opt/cluster.git (push)
```

**Remote Configuration**: The origin remote points to the original repository location.

---

## 🔹 Step 12: Verify repository history

```bash
git log --oneline
```

**Purpose**: Check that the repository history was cloned successfully.

**Expected Output**: List of commits from the original repository (if any exist).

---

## 🔹 Step 13: Check current working directory

```bash
pwd
```

**Purpose**: Confirm current location for final verification.

**Expected Output**:
```
/usr/src/kodekloudrepos/cluster
```

---

## 📋 Quick Command Reference

For quick copy-paste, execute as **natasha user** on **Storage Server (ststor01)**:

```bash
# Connect to storage server as natasha
ssh natasha@ststor01

# Verify source repository
cd /opt/
ls -la cluster.git/

# Create target directory and clone
mkdir -p /usr/src/kodekloudrepos/
git clone /opt/cluster.git /usr/src/kodekloudrepos/cluster

# Verify clone success
ls -ld /usr/src/kodekloudrepos/cluster
cd /usr/src/kodekloudrepos/cluster
git status
git remote -v
```

---

## 💡 Additional Tips

- **Local repository**: This creates a local clone with full Git functionality
- **Remote tracking**: The cloned repository maintains connection to the original
- **User permissions**: Repository files will be owned by natasha user
- **Working directory**: Unlike bare repositories, this includes working files
- **Git operations**: You can perform commits, branches, and other Git operations

---

## 🔧 Troubleshooting Common Issues

### **Issue 1: Permission denied when accessing source repository**
**Symptoms**: Cannot read from `/opt/cluster.git`
**Solution**: Verify natasha user has read access to the source repository
```bash
ls -ld /opt/cluster.git
# Check if natasha can read the repository
```

### **Issue 2: Cannot create target directory**
**Symptoms**: Permission denied when creating `/usr/src/kodekloudrepos/`
**Solution**: Check directory permissions and parent directory access
```bash
ls -ld /usr/src/
mkdir -p /usr/src/kodekloudrepos/
```

### **Issue 3: Git clone fails with "not a git repository"**
**Symptoms**: Source directory is not recognized as Git repository
**Solution**: Verify the source is a valid Git repository
```bash
ls -la /opt/cluster.git/
cat /opt/cluster.git/HEAD
```

### **Issue 4: Clone destination already exists**
**Symptoms**: Directory `/usr/src/kodekloudrepos/cluster` already exists
**Solution**: Remove existing directory or choose different name
```bash
rm -rf /usr/src/kodekloudrepos/cluster
# Then retry the clone operation
```

---

## 🚨 Task-Specific Challenge & Solution

**🔍 Main Challenge Encountered:**

The primary challenge was **cloning an existing Git repository while maintaining proper user context and permissions** without making unauthorized modifications to the system or repository structure.

**💡 Solution Approach:**

1. **User Context Maintenance**: Performed all operations as the natasha user without escalating to root privileges
2. **Source Repository Verification**: Confirmed the existence and accessibility of the source repository at `/opt/cluster.git`
3. **Directory Structure Creation**: Created necessary parent directories for the target location `/usr/src/kodekloudrepos/cluster`
4. **Repository Cloning**: Used `git clone` command to create a complete working copy with full Git functionality
5. **Clone Verification**: Validated successful cloning through directory listing, Git status, and remote configuration checks
6. **Permission Preservation**: Maintained original file permissions and ownership as required

**🎯 Key Success Factors:**
- **User context adherence** performing all operations as natasha user without privilege escalation
- **Path specification accuracy** ensuring exact clone destination `/usr/src/kodekloudrepos/cluster`
- **Repository integrity** maintaining complete Git history and metadata during clone
- **Remote configuration** preserving connection to original repository location
- **Permission compliance** not modifying any existing repository or directory permissions
- **Verification procedures** confirming successful clone through multiple validation methods

**⚠️ Critical Configuration Details:**
- **Source repository path** - `/opt/cluster.git` must exist and be accessible
- **Target directory creation** - parent directories created as needed for destination path
- **Git clone behavior** - creates working directory with `.git` metadata folder
- **Remote tracking** - automatically sets up origin remote pointing to source repository
- **User ownership** - cloned files owned by the user performing the clone (natasha)

**🔒 Repository Management Benefits:**
- **Complete working copy** - full Git repository with working directory for development
- **History preservation** - maintains complete commit history from original repository
- **Remote connectivity** - can push/pull changes to/from original repository
- **Independent operation** - can work offline with full Git functionality

---

## ⚠️ Important Production Notes

🔧 **Repository Copy**: Complete Git clone with working directory and full version control capabilities

🔐 **User Context**: All operations performed under natasha user maintaining proper access control

📊 **Version Control**: Cloned repository maintains connection to original source for collaboration

🛡️ **Permission Management**: Original permissions preserved as specified in requirements