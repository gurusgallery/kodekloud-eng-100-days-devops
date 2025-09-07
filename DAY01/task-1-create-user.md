# Task 1 - Create User with Non-Interactive Shell
## Task Description
To accommodate the backup agent tool's specifications, the system admin team at xFusionCorp Industries requires the creation of a user with a non-interactive shell. Here's your task:
Create a user named **rose** with a non-interactive shell on App Server 2.
Note: You can find the infrastructure details by clicking on the Details of all Users and Servers button on the top-right section of the page.
---
## Solution with Command Purposes

### Step 1: Switch to the root user for administrative privileges.
```bash
sudo su -

Step 2: Verify if the user 'rose' already exists in the system.


Bash

cat /etc/passwd | grep rose

Step 3: Create a new user 'rose' with a non-interactive shell set to /sbin/nologin.

Bash

sudo useradd rose --shell /sbin/nologin