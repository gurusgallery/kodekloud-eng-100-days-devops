**🌟 Task 17 - Setup PostgreSQL Database and User for Nautilus Application**

**📌 Task Description**

The **Nautilus application team** plans to deploy a new app using **PostgreSQL database** on **Nautilus infrastructure** in **Stratos Datacenter**.

The **PostgreSQL database server** is already installed on the **database server** (stdb01).

**Requirements:**
a. Create database user **`kodekloud_gem`** with password **`GyQkFRVNr3`**
b. Create database **`kodekloud_db1`** and grant **all privileges** on it to the user

👉 **Your task:** Configure PostgreSQL database and user for the new Nautilus application deployment.

💡 **Note:** Do not restart the PostgreSQL service.

---

## 🔹 Step 1: Connect to the database server

```bash
ssh peter@stdb01
sudo su -
```

**Purpose**: Connect to the database server (stdb01) as peter user and switch to root for administrative access.

---

## 🔹 Step 2: Check PostgreSQL service status

```bash
systemctl status postgresql
```

**Purpose**: Verify that PostgreSQL service is running on the database server.

**Expected Output**:
```
● postgresql.service - PostgreSQL database server
   Loaded: loaded
   Active: active (running)
   Main PID: 1234 (postgres)
```

**Status**: ✅ **PostgreSQL is already installed and running**

---

## 🔹 Step 3: Switch to PostgreSQL system user

```bash
su - postgres
```

**Purpose**: Switch to the postgres system user to access PostgreSQL administrative functions.

**User Context**: 🔑 The postgres user has full administrative privileges for database operations.

---

## 🔹 Step 4: Access PostgreSQL command prompt

```bash
psql
```

**Purpose**: Launch the PostgreSQL interactive terminal for database administration.

**Expected Output**:
```
psql (12.x)
Type "help" for help.

postgres=#
```

**Prompt Indicator**: `postgres=#` shows superuser access to PostgreSQL.

---

## 🔹 Step 5: Create database user with password

```sql
CREATE USER kodekloud_gem WITH PASSWORD 'GyQkFRVNr3';
```

**Purpose**: Create a new database user with the specified username and password.

**Expected Output**:
```
CREATE ROLE
```

**Command Breakdown**:
- `CREATE USER` - Creates a new database user/role
- `kodekloud_gem` - Username for the application
- `WITH PASSWORD` - Specifies password authentication
- `'GyQkFRVNr3'` - Password for the user (in single quotes)

---

## 🔹 Step 6: Create application database

```sql
CREATE DATABASE kodekloud_db1;
```

**Purpose**: Create a new database for the Nautilus application.

**Expected Output**:
```
CREATE DATABASE
```

**Database Creation**: 📊 Creates an empty database ready for application data.

---

## 🔹 Step 7: Grant all privileges to the user

```sql
GRANT ALL PRIVILEGES ON DATABASE kodekloud_db1 TO kodekloud_gem;
```

**Purpose**: Grant comprehensive database privileges to the application user.

**Expected Output**:
```
GRANT
```

**Privileges Granted**:
- **CONNECT** - Connect to the database
- **CREATE** - Create schemas and objects
- **TEMPORARY** - Create temporary tables
- **ALL** - Full control over the database

---

## 🔹 Step 8: Verify user creation

```sql
\du
```

**Purpose**: Display all database users/roles to verify the new user was created.

**Expected Output**:
```
                                   List of roles
 Role name     |                         Attributes                         | Member of 
---------------+------------------------------------------------------------+-----------
 kodekloud_gem |                                                            | {}
 postgres      | Superuser, Create role, Create DB, Replication, Bypass RLS | {}
```

---

## 🔹 Step 9: List databases to verify creation

```sql
\l
```

**Purpose**: List all databases to confirm the new database was created.

**Expected Output**:
```
                                  List of databases
      Name      |  Owner   | Encoding |   Collate   |    Ctype    |   Access privileges   
----------------+----------+----------+-------------+-------------+-----------------------
 kodekloud_db1  | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =Tc/postgres         +
                |          |          |             |             | kodekloud_gem=CTc/postgres
 postgres       | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | 
```

**Privilege Indicator**: Shows `kodekloud_gem=CTc/postgres` confirming granted privileges.

---

## 🔹 Step 10: Exit PostgreSQL prompt

```sql
\q
```

**Purpose**: Exit the PostgreSQL interactive terminal.

**Return to**: postgres user shell prompt.

---

## 🔹 Step 11: Exit postgres user

```bash
exit
```

**Purpose**: Return to root user context from postgres user.

**User Context**: Back to root user for system administration.

---

## 🔹 Step 12: Test database connection as new user

```bash
psql -U kodekloud_gem -d kodekloud_db1 -h 127.0.0.1 -W
```

**Purpose**: Test connection to the new database using the newly created user.

**Command Breakdown**:
- `-U kodekloud_gem` - Connect as the specified user
- `-d kodekloud_db1` - Connect to the specified database
- `-h 127.0.0.1` - Connect via TCP/IP (localhost)
- `-W` - Force password prompt

**Password Prompt**:
```
Password for user kodekloud_gem: [Enter: GyQkFRVNr3]
```

---

## 🔹 Step 13: Verify database connection

```sql
SELECT current_database();
```

**Purpose**: Verify successful connection and confirm the current database context.

**Expected Output**:
```
 current_database 
------------------
 kodekloud_db1
(1 row)
```

**Success Indicator**: ✅ **Connection successful and database context confirmed**.

---

## 🔹 Step 14: Test user privileges

```sql
SELECT current_user;
```

**Purpose**: Confirm the current user context in the database session.

**Expected Output**:
```
 current_user  
---------------
 kodekloud_gem
(1 row)
```

**Additional privilege tests**:
```sql
-- Test table creation privilege
CREATE TABLE test_table (id SERIAL PRIMARY KEY, name VARCHAR(50));

-- List tables to verify creation
\dt

-- Drop test table
DROP TABLE test_table;
```

---

## 🔹 Step 15: Exit test connection

```sql
\q
```

**Purpose**: Exit the test database connection.

**Final Status**: ✅ **Database and user setup completed successfully**.

---

## 📋 Quick Command Reference

For quick copy-paste, execute on **Database Server (stdb01)**:

```bash
# Connect to database server
ssh peter@stdb01
sudo su -

# Check PostgreSQL status
systemctl status postgresql

# Access PostgreSQL as admin
su - postgres
psql

# Create user and database
CREATE USER kodekloud_gem WITH PASSWORD 'GyQkFRVNr3';
CREATE DATABASE kodekloud_db1;
GRANT ALL PRIVILEGES ON DATABASE kodekloud_db1 TO kodekloud_gem;

# Verify setup
\du
\l
\q

# Test connection
exit
psql -U kodekloud_gem -d kodekloud_db1 -h 127.0.0.1 -W
# Enter password: GyQkFRVNr3

# Verify connection
SELECT current_database();
SELECT current_user;
\q
```

---

## 💡 Additional Tips

- **Password security**: Store database credentials securely in production environments
- **Connection methods**: Use `peer` authentication for local connections, `md5` for network connections
- **User attributes**: Add `CREATEDB` or `CREATEROLE` attributes if needed for specific applications
- **Schema permissions**: Consider granting schema-level permissions for more granular control
- **Backup planning**: Set up regular database backups for the new application database

---

## 🔧 Troubleshooting Common Issues

### **Issue 1: PostgreSQL service not running**
**Symptoms**: Connection refused or service unavailable
**Solution**: Start and enable PostgreSQL service
```bash
sudo systemctl start postgresql
sudo systemctl enable postgresql
sudo systemctl status postgresql
```

### **Issue 2: Authentication failed for user**
**Symptoms**: Password authentication fails
**Solution**: Verify pg_hba.conf settings and user creation
```bash
# Check authentication configuration
sudo cat /var/lib/pgsql/data/pg_hba.conf
# Ensure md5 authentication is enabled for the user
```

### **Issue 3: Database connection refused**
**Symptoms**: Connection to database fails
**Solution**: Check PostgreSQL listening configuration
```bash
# Verify PostgreSQL is listening on correct addresses
sudo netstat -tlnp | grep postgres
# Check postgresql.conf for listen_addresses setting
```

### **Issue 4: Permission denied errors**
**Symptoms**: User cannot perform operations on database
**Solution**: Re-grant privileges or check database ownership
```sql
-- Re-grant all privileges
GRANT ALL PRIVILEGES ON DATABASE kodekloud_db1 TO kodekloud_gem;
-- Grant schema permissions if needed
GRANT ALL ON SCHEMA public TO kodekloud_gem;
```

---

## 🚨 Task-Specific Challenge & Solution

**🔍 Main Challenge Encountered:**

The primary challenge was **configuring PostgreSQL database and user access** for a new application deployment while ensuring proper privilege management and connection authentication without disrupting the existing PostgreSQL service.

**💡 Solution Approach:**

1. **Service Verification**: Confirmed PostgreSQL service status before making any configuration changes
2. **Administrative Access**: Used the postgres system user to access database administration functions
3. **User Management**: Created application-specific user `kodekloud_gem` with secure password authentication
4. **Database Creation**: Established dedicated database `kodekloud_db1` for application isolation
5. **Privilege Assignment**: Granted comprehensive database privileges using `GRANT ALL PRIVILEGES` for full application control
6. **Connection Testing**: Verified setup using TCP/IP connection with password authentication to simulate application connectivity

**🎯 Key Success Factors:**
- **System user context** switching to postgres user for administrative database access
- **Proper SQL syntax** using CREATE USER, CREATE DATABASE, and GRANT statements correctly
- **Authentication method** using password-based authentication for application connectivity
- **Privilege verification** using `\du` and `\l` commands to confirm user and database creation
- **Connection testing** with realistic application connection parameters

**⚠️ Critical Configuration Details:**
- **Password specification** using single quotes in CREATE USER statement
- **TCP/IP connection testing** using `-h 127.0.0.1` to simulate network connectivity
- **Privilege scope** granting database-level privileges for comprehensive application access
- **Service preservation** avoiding service restart while maintaining configuration integrity

**🔒 Security Considerations:**
- **Password complexity** using secure password for database user authentication
- **Privilege isolation** creating dedicated user instead of using superuser privileges
- **Database separation** using dedicated database for application data isolation
- **Connection method** using password authentication for secure application connectivity

---

## ⚠️ Important Production Notes

🔧 **Application Readiness**: Database and user are now configured for Nautilus application deployment

🔐 **Security Management**: Implement proper password policies and regular credential rotation

📊 **Performance Monitoring**: Set up database performance monitoring for the new application

🛡️ **Backup Strategy**: Configure automated backups for the kodekloud_db1 database before production use