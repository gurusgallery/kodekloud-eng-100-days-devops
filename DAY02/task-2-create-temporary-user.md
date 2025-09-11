**🌟 Task 2 - Create Temporary User with Expiry Date**

**📌 Task Description**

As part of the temporary assignment to the **Nautilus project**, a developer named ravi requires access for a limited duration. To ensure smooth access management, a temporary user account with an expiry date is needed.

👉 Your task: Create a user named **ravi** on **App Server 3** in *Stratos Datacenter*. Set the expiry date to **2024-03-28**, ensuring the user is created in lowercase as per standard protocol.

💡 **Note:** You can find the infrastructure details by clicking on the **Details of all Users and Servers** button on the top-right section of the page.

---

## 🚨 Task-Specific Challenge & Solution

**🔍 Main Challenge Encountered:**

The primary challenge was creating a **temporary user account with an automatic expiry date** to ensure proper access management and security compliance for time-limited project assignments.

**💡 Solution Approach:**

1. **User Existence Check**: Used `cat /etc/passwd | grep ravi` to verify no conflicting user accounts
2. **Expiry Date Implementation**: Used `sudo useradd -e 2024-03-28 ravi` to set automatic account expiration
3. **Date Format Compliance**: Ensured proper YYYY-MM-DD format for the expiry date parameter
4. **Verification Process**: Used `sudo chage -l ravi` to display and confirm account aging information including expiry date

**🎯 Key Success Factors:**
- **Automated expiry management** using the `-e` flag to prevent manual oversight
- **Proper date formatting** in YYYY-MM-DD format for system compatibility
- **Account aging verification** to ensure expiry settings are correctly applied
- **Security compliance** for temporary project assignments with defined end dates

**⚠️ Critical Learning Points:**
- **Account expiry automation** prevents security risks from forgotten temporary accounts
- **Date format precision** is critical for proper system interpretation
- **chage command utility** provides comprehensive account aging information
- **Temporary access management** requires both creation and expiry date planning

---

## 🔹 Step 1: Check if user 'ravi' already exists

```bash
cat /etc/passwd | grep ravi
```

**Purpose**: Check if the user 'ravi' already exists on the system.

**Expected Result**: 
- ✔️ If **nothing is returned**, the user does not exist yet
- ❌ If user information appears, the user already exists

---

## 🔹 Step 2: Create user 'ravi' with expiry date

```bash
sudo useradd -e 2024-03-28 ravi
```

**Purpose**: Create a new user 'ravi' with an expiry date set to March 28, 2024.

**Note**: 🗓️ The `-e` flag sets the account expiration date in YYYY-MM-DD format.

---

## 🔹 Step 3: Verify user creation and expiry date

```bash
sudo chage -l ravi
```

**Purpose**: Display the user account aging information including the expiry date to verify that it was set correctly.

**Expected Output Example**:
```
Last password change                                    : never
Password expires                                        : never
Password inactive                                       : never
Account expires                                         : Mar 28, 2024
Minimum number of days between password change          : 0
Maximum number of days between password change          : 99999
Number of days of warning before password expires       : 7
```

---

## 📋 Quick Command Reference

For quick copy-paste, here are all commands in sequence:

```bash
# Check if user exists
cat /etc/passwd | grep ravi

# Create user with expiry date
sudo useradd -e 2024-03-28 ravi

# Verify user creation and expiry
sudo chage -l ravi
```

---

## 💡 Additional Tips

- **To check user details**: `id ravi`
- **To modify expiry date**: `sudo chage -E 2024-12-31 ravi`
- **To remove expiry date**: `sudo chage -E -1 ravi`
- **To delete the user later**: `sudo userdel ravi`