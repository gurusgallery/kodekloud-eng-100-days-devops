**🌟 Task 23 - Fork Git Repository Using Gitea Web UI**

**📌 Task Description**

There is a **Git server** utilized by the **Nautilus project teams**. Recently, a new developer named **Jon** joined the team and needs to begin working on a project. To begin, he must **fork an existing Git repository**.

**Requirements:**
- Click on the **Gitea UI button** located on the top bar to access the Gitea page
- Login to Gitea server using username **`jon`** and password **`Jon_pass123`**
- Once logged in, locate the Git repository named **`sarah/story-blog`** and **fork it** under the **jon** user

👉 **Your task:** Use the Gitea web interface to fork an existing repository for collaborative development.

💡 **Note:** For tasks requiring web UI changes, screenshots are necessary for review purposes. Additionally, consider utilizing screen recording software such as loom.com to record and share your task completion process.

---

## 🔹 Step 1: Access Gitea Web Interface

**Action**: Click on the **"Gitea UI"** button located in the top bar of your lab environment.

**Purpose**: This opens the Gitea web interface where you can manage Git repositories through a user-friendly web UI.

**Expected Result**: Gitea login page should load in a new browser tab/window.

---

## 🔹 Step 2: Login to Gitea Server

**Credentials to use:**
- **Username**: `jon`
- **Password**: `Jon_pass123`

**Process**:
1. Enter the username in the username field
2. Enter the password in the password field  
3. Click the **"Sign In"** button

**Purpose**: Authenticate as the new developer Jon to access repositories and perform fork operation.

**Expected Result**: Successfully logged into Gitea dashboard as user Jon.

---

## 🔹 Step 3: Navigate to Source Repository

**Repository Location**: `sarah/story-blog`

**Navigation Methods**:

**Option 1 - Direct Search**:
1. Use the search bar in Gitea interface
2. Search for "sarah/story-blog" or "story-blog"
3. Click on the matching repository from search results

**Option 2 - Browse Users**:
1. Look for "sarah" in the user list or explore section
2. Navigate to sarah's profile
3. Find the "story-blog" repository in sarah's repository list

**Purpose**: Locate the specific repository that needs to be forked.

---

## 🔹 Step 4: Access Repository Fork Option

**Based on the provided screenshots:**

1. **Repository Page**: You should see the repository page showing:
   - Repository name: `sarah/story-blog`
   - Repository statistics (commits, branches, size)
   - Repository files and commit history

2. **Fork Button Location**: Look for the **"Fork"** button, typically located:
   - In the top-right area of the repository page
   - Near other repository actions like "Watch" and "Star"
   - The button should show fork count (e.g., "Fork 0")

**Purpose**: Access the fork functionality to create a personal copy of the repository.

---

## 🔹 Step 5: Initiate Fork Process

**Action**: Click the **"Fork"** button on the repository page.

**Expected Result**: This should open the "New Repository Fork" dialog/page.

**Fork Dialog Contents** (as shown in screenshot):
- **Owner**: Should be set to "jon" (your username)
- **Fork From**: Shows "sarah/story-blog" as the source
- **Repository Name**: Pre-filled with "story-blog"
- **Visibility**: Options for repository privacy
- **Description**: Optional description field

---

## 🔹 Step 6: Configure Fork Settings

**Fork Configuration**:

1. **Owner**: Verify it's set to **"jon"** (should be automatic)
2. **Repository Name**: Keep as **"story-blog"** (default is fine)
3. **Visibility**: Choose appropriate setting (usually keep same as original)
4. **Description**: Optional - can add "story blog" or leave existing description

**Purpose**: Configure the fork to be created under Jon's account with appropriate settings.

---

## 🔹 Step 7: Complete Fork Operation

**Action**: Click the **"Fork Repository"** button (green button as shown in screenshot).

**Purpose**: Execute the fork operation to create a copy of sarah/story-blog under Jon's account.

**Expected Process**:
1. Gitea processes the fork request
2. Creates a new repository: `jon/story-blog`
3. Copies all repository content, history, and branches
4. Redirects to the newly created forked repository

---

## 🔹 Step 8: Verify Successful Fork

**Verification Steps**:

1. **Check Repository URL**: Should now show `jon/story-blog` instead of `sarah/story-blog`
2. **Verify Fork Indicator**: Repository page should show "forked from sarah/story-blog"
3. **Check Repository Contents**: All files should be present (frogs-and-ox.txt, lion-and-mouse.txt, etc.)
4. **Verify Commit History**: All commits from original repository should be preserved

**Purpose**: Confirm that the fork was created successfully with complete repository content.

---

## 🔹 Step 9: Navigate to Dashboard for Final Verification

**Action**: Click on the **"Dashboard"** button/link in Gitea interface.

**Purpose**: Verify that the forked repository appears in Jon's repository list.

**Expected Result**: 
- Dashboard should show `jon/story-blog` in your repositories list
- Repository should be marked as a fork
- Should have same content and structure as the original

---

## 📋 Quick Steps Summary

**Complete workflow for forking repository:**

1. **Access Gitea UI** → Click "Gitea UI" button in lab environment
2. **Login** → Username: `jon`, Password: `Jon_pass123`
3. **Find Repository** → Navigate to `sarah/story-blog`
4. **Click Fork** → Click "Fork" button on repository page
5. **Configure Fork** → Verify settings (Owner: jon, Name: story-blog)
6. **Complete Fork** → Click "Fork Repository" button
7. **Verify Success** → Check new repository at `jon/story-blog`
8. **Dashboard Check** → Verify repository appears in Jon's dashboard

---

## 🔧 Troubleshooting Common Issues

### **Issue 1: Cannot find sarah/story-blog repository**
**Solution**: 
- Try different search terms
- Check if logged in correctly
- Verify repository exists and is accessible

### **Issue 2: Fork button not visible**
**Solution**:
- Ensure you're logged in as Jon
- Check repository permissions
- Refresh the page and try again

### **Issue 3: Fork operation fails**
**Solution**:
- Check if repository name already exists under Jon's account
- Verify sufficient permissions
- Try different repository name if needed

### **Issue 4: Forked repository appears empty**
**Solution**:
- Wait for fork operation to complete
- Refresh the page
- Check if original repository had content

---

## 📸 Screenshot 

![alt text](<100 days of devops fork-1.png>)


![alt text](<100 days of devops fork 2.png>)

---

## 🚨 Task-Specific Challenge & Solution

**🔍 Main Challenge Encountered:**

The primary challenge was **navigating the Gitea web interface to fork a repository** while ensuring proper authentication and verification of the forked repository under the correct user account.

**💡 Solution Approach:**

1. **Web Interface Access**: Used the Gitea UI button in the lab environment to access the Git server web interface
2. **User Authentication**: Logged in as the new developer Jon using provided credentials (jon/Jon_pass123)
3. **Repository Location**: Found the source repository sarah/story-blog through Gitea's repository browsing interface
4. **Fork Operation**: Used Gitea's fork functionality to create a personal copy of the repository under Jon's account
5. **Configuration Management**: Configured fork settings to maintain proper repository name and visibility settings
6. **Verification Process**: Confirmed successful fork through repository URL change and dashboard verification

**🎯 Key Success Factors:**
- **Proper authentication** using correct username and password for Jon account
- **Repository identification** accurately locating sarah/story-blog among available repositories
- **Fork interface navigation** successfully using Gitea's web UI fork functionality
- **Configuration accuracy** ensuring fork created under jon account with correct settings
- **Verification procedures** confirming fork completion through multiple validation methods
- **Documentation compliance** capturing required screenshots for task verification

**⚠️ Critical Web UI Elements:**
- **Gitea UI access** through lab environment button integration
- **Login form** requiring exact credential match for authentication
- **Fork button** location and visibility on repository pages
- **Fork configuration dialog** with owner, name, and visibility settings
- **Repository navigation** showing fork relationship and ownership

**🔒 Repository Fork Benefits:**
- **Independent development** Jon can make changes without affecting original repository
- **Collaboration preparation** enables pull request workflow for contributing back to original
- **Complete history preservation** maintains all commits and branches from original repository
- **Personal workspace** provides dedicated environment for Jon's development work

---

## ⚠️ Important Notes

**Task Completion Requirements**:
- Screenshots required for verification
- Screen recording recommended for process documentation
- Must verify fork appears in Jon's dashboard
- Repository must maintain original content and history

**Web UI Considerations**:
- Interface may vary slightly between Gitea versions
- Ensure browser allows pop-ups for proper fork dialog display
- Clear browser cache if experiencing display issues
- Use modern browser for best Gitea compatibility**🌟 Task 23 - Fork Git Repository Using Gitea Web UI**

**📌 Task Description**

There is a **Git server** utilized by the **Nautilus project teams**. Recently, a new developer named **Jon** joined the team and needs to begin working on a project. To begin, he must **fork an existing Git repository**.

**Requirements:**
- Click on the **Gitea UI button** located on the top bar to access the Gitea page
- Login to Gitea server using username **`jon`** and password **`Jon_pass123`**
- Once logged in, locate the Git repository named **`sarah/story-blog`** and **fork it** under the **jon** user

👉 **Your task:** Use the Gitea web interface to fork an existing repository for collaborative development.

💡 **Note:** For tasks requiring web UI changes, screenshots are necessary for review purposes. Additionally, consider utilizing screen recording software such as loom.com to record and share your task completion process.

---

## 🔹 Step 1: Access Gitea Web Interface

**Action**: Click on the **"Gitea UI"** button located in the top bar of your lab environment.

**Purpose**: This opens the Gitea web interface where you can manage Git repositories through a user-friendly web UI.

**Expected Result**: Gitea login page should load in a new browser tab/window.

---

## 🔹 Step 2: Login to Gitea Server

**Credentials to use:**
- **Username**: `jon`
- **Password**: `Jon_pass123`

**Process**:
1. Enter the username in the username field
2. Enter the password in the password field  
3. Click the **"Sign In"** button

**Purpose**: Authenticate as the new developer Jon to access repositories and perform fork operation.

**Expected Result**: Successfully logged into Gitea dashboard as user Jon.

---

## 🔹 Step 3: Navigate to Source Repository

**Repository Location**: `sarah/story-blog`

**Navigation Methods**:

**Option 1 - Direct Search**:
1. Use the search bar in Gitea interface
2. Search for "sarah/story-blog" or "story-blog"
3. Click on the matching repository from search results

**Option 2 - Browse Users**:
1. Look for "sarah" in the user list or explore section
2. Navigate to sarah's profile
3. Find the "story-blog" repository in sarah's repository list

**Purpose**: Locate the specific repository that needs to be forked.

---

## 🔹 Step 4: Access Repository Fork Option

**Based on the provided screenshots:**

1. **Repository Page**: You should see the repository page showing:
   - Repository name: `sarah/story-blog`
   - Repository statistics (commits, branches, size)
   - Repository files and commit history

2. **Fork Button Location**: Look for the **"Fork"** button, typically located:
   - In the top-right area of the repository page
   - Near other repository actions like "Watch" and "Star"
   - The button should show fork count (e.g., "Fork 0")

**Purpose**: Access the fork functionality to create a personal copy of the repository.

---

## 🔹 Step 5: Initiate Fork Process

**Action**: Click the **"Fork"** button on the repository page.

**Expected Result**: This should open the "New Repository Fork" dialog/page.

**Fork Dialog Contents** (as shown in screenshot):
- **Owner**: Should be set to "jon" (your username)
- **Fork From**: Shows "sarah/story-blog" as the source
- **Repository Name**: Pre-filled with "story-blog"
- **Visibility**: Options for repository privacy
- **Description**: Optional description field

---

## 🔹 Step 6: Configure Fork Settings

**Fork Configuration**:

1. **Owner**: Verify it's set to **"jon"** (should be automatic)
2. **Repository Name**: Keep as **"story-blog"** (default is fine)
3. **Visibility**: Choose appropriate setting (usually keep same as original)
4. **Description**: Optional - can add "story blog" or leave existing description

**Purpose**: Configure the fork to be created under Jon's account with appropriate settings.

---

## 🔹 Step 7: Complete Fork Operation

**Action**: Click the **"Fork Repository"** button (green button as shown in screenshot).

**Purpose**: Execute the fork operation to create a copy of sarah/story-blog under Jon's account.

**Expected Process**:
1. Gitea processes the fork request
2. Creates a new repository: `jon/story-blog`
3. Copies all repository content, history, and branches
4. Redirects to the newly created forked repository

---

## 🔹 Step 8: Verify Successful Fork

**Verification Steps**:

1. **Check Repository URL**: Should now show `jon/story-blog` instead of `sarah/story-blog`
2. **Verify Fork Indicator**: Repository page should show "forked from sarah/story-blog"
3. **Check Repository Contents**: All files should be present (frogs-and-ox.txt, lion-and-mouse.txt, etc.)
4. **Verify Commit History**: All commits from original repository should be preserved

**Purpose**: Confirm that the fork was created successfully with complete repository content.

---

## 🔹 Step 9: Navigate to Dashboard for Final Verification

**Action**: Click on the **"Dashboard"** button/link in Gitea interface.

**Purpose**: Verify that the forked repository appears in Jon's repository list.

**Expected Result**: 
- Dashboard should show `jon/story-blog` in your repositories list
- Repository should be marked as a fork
- Should have same content and structure as the original

---

## 📋 Quick Steps Summary

**Complete workflow for forking repository:**

1. **Access Gitea UI** → Click "Gitea UI" button in lab environment
2. **Login** → Username: `jon`, Password: `Jon_pass123`
3. **Find Repository** → Navigate to `sarah/story-blog`
4. **Click Fork** → Click "Fork" button on repository page
5. **Configure Fork** → Verify settings (Owner: jon, Name: story-blog)
6. **Complete Fork** → Click "Fork Repository" button
7. **Verify Success** → Check new repository at `jon/story-blog`
8. **Dashboard Check** → Verify repository appears in Jon's dashboard

---

## 🔧 Troubleshooting Common Issues

### **Issue 1: Cannot find sarah/story-blog repository**
**Solution**: 
- Try different search terms
- Check if logged in correctly
- Verify repository exists and is accessible

### **Issue 2: Fork button not visible**
**Solution**:
- Ensure you're logged in as Jon
- Check repository permissions
- Refresh the page and try again

### **Issue 3: Fork operation fails**
**Solution**:
- Check if repository name already exists under Jon's account
- Verify sufficient permissions
- Try different repository name if needed

### **Issue 4: Forked repository appears empty**
**Solution**:
- Wait for fork operation to complete
- Refresh the page
- Check if original repository had content

---

## 📸 Screenshot Requirements

**Required Screenshots** (for task verification):

1. **Gitea Login Page** - Before logging in
2. **Successful Login** - Jon's dashboard after login
3. **Source Repository** - sarah/story-blog repository page
4. **Fork Dialog** - New Repository Fork configuration page
5. **Forked Repository** - jon/story-blog after successful fork
6. **Dashboard Verification** - Jon's dashboard showing the forked repository

**Screenshot Tips**:
- Capture full browser window showing URLs
- Ensure important elements are clearly visible
- Take screenshots at each major step
- Include timestamps if possible

---

## 🎬 Screen Recording Suggestion

**Recommended Recording Structure**:
1. **Start**: Show lab environment with Gitea UI button
2. **Access**: Click Gitea UI button and show login page
3. **Login**: Enter credentials and show successful login
4. **Navigate**: Search for and access sarah/story-blog
5. **Fork**: Show fork dialog and configuration
6. **Complete**: Show fork completion and new repository
7. **Verify**: Show dashboard with forked repository

**Tools**: 
- loom.com (as suggested in task)
- Built-in browser recording
- Screen recording software

---

## 🚨 Task-Specific Challenge & Solution

**🔍 Main Challenge Encountered:**

The primary challenge was **navigating the Gitea web interface to fork a repository** while ensuring proper authentication and verification of the forked repository under the correct user account.

**💡 Solution Approach:**

1. **Web Interface Access**: Used the Gitea UI button in the lab environment to access the Git server web interface
2. **User Authentication**: Logged in as the new developer Jon using provided credentials (jon/Jon_pass123)
3. **Repository Location**: Found the source repository sarah/story-blog through Gitea's repository browsing interface
4. **Fork Operation**: Used Gitea's fork functionality to create a personal copy of the repository under Jon's account
5. **Configuration Management**: Configured fork settings to maintain proper repository name and visibility settings
6. **Verification Process**: Confirmed successful fork through repository URL change and dashboard verification

**🎯 Key Success Factors:**
- **Proper authentication** using correct username and password for Jon account
- **Repository identification** accurately locating sarah/story-blog among available repositories
- **Fork interface navigation** successfully using Gitea's web UI fork functionality
- **Configuration accuracy** ensuring fork created under jon account with correct settings
- **Verification procedures** confirming fork completion through multiple validation methods
- **Documentation compliance** capturing required screenshots for task verification

**⚠️ Critical Web UI Elements:**
- **Gitea UI access** through lab environment button integration
- **Login form** requiring exact credential match for authentication
- **Fork button** location and visibility on repository pages
- **Fork configuration dialog** with owner, name, and visibility settings
- **Repository navigation** showing fork relationship and ownership

**🔒 Repository Fork Benefits:**
- **Independent development** Jon can make changes without affecting original repository
- **Collaboration preparation** enables pull request workflow for contributing back to original
- **Complete history preservation** maintains all commits and branches from original repository
- **Personal workspace** provides dedicated environment for Jon's development work

---

## ⚠️ Important Notes

**Task Completion Requirements**:
- Screenshots required for verification
- Screen recording recommended for process documentation
- Must verify fork appears in Jon's dashboard
- Repository must maintain original content and history

**Web UI Considerations**:
- Interface may vary slightly between Gitea versions
- Ensure browser allows pop-ups for proper fork dialog display
- Clear browser cache if experiencing display issues
- Use modern browser for best Gitea compatibility