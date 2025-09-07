# 🌟 Task 1 - Create User with Non-Interactive Shell  

## 📌 Task Description  
To accommodate the **backup agent tool's** specifications, the system admin team at *xFusionCorp Industries* requires the creation of a user with a **non-interactive shell**.  

👉 Your task: Create a user named **rose** with a non-interactive shell on **App Server 2**.  

> 💡 **Note:** You can find the infrastructure details by clicking on the **Details of all Users and Servers** button on the top-right section of the page.

---

## ✅ Solution with Command Purposes  

### 🔹 Step 1: Switch to the root user for administrative privileges  
```bash
sudo su -

🔹 Step 2: Verify if the user 'rose' already exists

```bash
cat /etc/passwd | grep rose


✔️ If nothing is returned, the user does not exist yet.

🔹 Step 3: Create a new user 'rose' with a non-interactive shell

```bash
sudo useradd rose --shell /sbin/nologin


🔒 The /sbin/nologin shell ensures that the user cannot log in interactively.

🔹 (Optional) Step 4: Verify the user creation
id rose
# or
getent passwd rose


👀 This confirms the user rose was created successfully.