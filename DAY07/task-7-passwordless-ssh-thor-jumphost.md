**🌟 Task 7 - Set Up Password-less SSH Access for User thor on Jump Host**

**📌 Task Description**

The system admins team of **xFusionCorp Industries** has set up some scripts on the jump host that run on regular intervals and perform operations on all app servers in the **Stratos Datacenter**. To make these scripts work properly, we need to ensure that the user **thor** on the jump host has password-less SSH access to all app servers through their respective sudo users (e.g., **tony** for app server 1).

👉 **Your task:** Configure password-less SSH access from user **thor** on jump host to all app servers using their respective sudo users.

💡 **Note:** You can find the infrastructure details by clicking on the **Details of all Users and Servers** button on the top-right section of the page.

---

## 🔹 Step 1: Generate SSH key pair for user thor

**Login as thor user on jump host first:**

```bash
ssh-keygen -t rsa
```

**Purpose**: Generate an RSA SSH key pair if it does not already exist.

**Key Generation Process**:
- **File location**: Accept default (`/home/thor/.ssh/id_rsa`)
- **Passphrase**: Press `Enter` for empty passphrase (required for password-less login)
- **Confirmation**: Press `Enter` again to confirm empty passphrase

**Expected Output**:
```
Generating public/private rsa key pair.
Enter file in which to save the key (/home/thor/.ssh/id_rsa): [Enter]
Enter passphrase (empty for no passphrase): [Enter]
Enter same passphrase again: [Enter]
Your identification has been saved in /home/thor/.ssh/id_rsa.
Your public key has been saved in /home/thor/.ssh/id_rsa.pub.
```

---

## 🔹 Step 2: Copy public key to App Server 1 (stapp01)

```bash
ssh-copy-id tony@stapp01
```

**Purpose**: Copy the public key to the authorized_keys of user **tony** on server **stapp01**.

**What happens**: 📋 This command copies your public key to `/home/tony/.ssh/authorized_keys` on stapp01.

**Note**: You'll be prompted for tony's password this one time.

---

## 🔹 Step 3: Verify password-less login to App Server 1

```bash
ssh tony@stapp01
```

**Purpose**: Verify login to stapp01 as tony does not require password.

**Success Indicator**: ✔️ You should be able to login without entering a password.

**Test and exit**:
```bash
# Test command on remote server
hostname
# Exit back to jump host
exit
```

---

## 🔹 Step 4: Copy public key to App Server 2 (stapp02)

```bash
ssh-copy-id steve@stapp02
```

**Purpose**: Copy the public key to the authorized_keys of user **steve** on server **stapp02**.

---

## 🔹 Step 5: Verify password-less login to App Server 2

```bash
ssh steve@stapp02
```

**Purpose**: Verify login to stapp02 as steve does not require password.

```bash
# Test and exit
hostname
exit
```

---

## 🔹 Step 6: Copy public key to App Server 3 (stapp03)

```bash
ssh-copy-id banner@stapp03
```

**Purpose**: Copy the public key to the authorized_keys of user **banner** on server **stapp03**.

---

## 🔹 Step 7: Verify password-less login to App Server 3

```bash
ssh banner@stapp03
```

**Purpose**: Verify login to stapp03 as banner does not require password.

```bash
# Test and exit
hostname
exit
```

---

## 🔹 Step 8: Final verification of all connections

```bash
# Test all connections in sequence
ssh tony@stapp01 "hostname && whoami"
ssh steve@stapp02 "hostname && whoami"  
ssh banner@stapp03 "hostname && whoami"
```

**Purpose**: Ensure all logins from thor to sudo user accounts on app servers are password-less.

**Expected Output**:
```
stapp01.stratos.xfusioncorp.com
tony
stapp02.stratos.xfusioncorp.com
steve
stapp03.stratos.xfusioncorp.com
banner
```

---

## 📋 Quick Command Reference

For quick copy-paste, here are all commands in sequence:

```bash
# Generate SSH key pair (run as thor on jump host)
ssh-keygen -t rsa

# Copy public keys to all app servers
ssh-copy-id tony@stapp01
ssh-copy-id steve@stapp02
ssh-copy-id banner@stapp03

# Verify password-less access
ssh tony@stapp01
ssh steve@stapp02
ssh banner@stapp03

# Quick test all connections
ssh tony@stapp01 "hostname && whoami"
ssh steve@stapp02 "hostname && whoami"
ssh banner@stapp03 "hostname && whoami"
```

---

## 💡 Additional Tips

- **Check existing keys**: `ls -la ~/.ssh/` (look for id_rsa and id_rsa.pub)
- **View public key**: `cat ~/.ssh/id_rsa.pub`
- **Manual key copy** (alternative): `cat ~/.ssh/id_rsa.pub | ssh user@server "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"`
- **Troubleshooting**: Check SSH logs with `ssh -v user@server`
- **Key permissions**: Ensure `~/.ssh/` is 700 and `~/.ssh/id_rsa` is 600

---

## 🗂️ Server and User Mapping Reference

| App Server | Hostname | Sudo User |
|------------|----------|-----------|
| App Server 1 | stapp01 | tony |
| App Server 2 | stapp02 | steve |
| App Server 3 | stapp03 | banner |

---

## ⚠️ Important Security Notes

🔐 **Empty passphrase**: Required for automated scripts but reduces security

🔒 **Key protection**: Ensure private key (`id_rsa`) has proper permissions (600)

📝 **Script compatibility**: Password-less access enables automated operations from jump host