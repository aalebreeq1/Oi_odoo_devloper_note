# Odoo Developer Notes

## 1. GitLab Workflow — Submit a New Task

### Step 1: Get the Latest Changes
Make sure you are working with the latest code from `master`.

```bash
git checkout master
git pull origin master
```

### Step 2: Create and Switch to a New Branch
```bash
git checkout -b feature/task-name
```
Creates a new branch **and automatically switches to it**.

### Step 3: Implement and Stage Changes
After completing and testing your task:

```bash
git status
git add .
```
`git add .` stages your changes for committing.

### Step 4: Commit Changes
```bash
git commit -m "Fix Shopify order notification"
```
Saves your staged changes locally with a meaningful message.

### Step 5: Push Your Branch
```bash
git push -u origin feature/task-name
```
Pushes your branch to GitLab.

Alternatively, use **Publish Branch** in VS Code Source Control.

### Step 6: Create a Merge Request
In GitLab:

1. Open **Merge Requests → New Merge Request**.
2. Source: `feature/task-name`.
3. Target: `QA` or `master` (as instructed).
4. Add a description and assign a reviewer.
5. Submit the MR for approval.

---

## 2. Odoo QA Server — Pull & Restart

After the changes are merged and deployment is approved:

### Step 1: Connect to the Server
```bash
ssh gigate-qa
```
Connects to the QA server through SSH.

### Step 2: Switch to Root
```bash
sudo -E su
```
Switches to root and requests preservation of environment variables.

### Step 3: Navigate to the Update Directory
```bash
cd /opt/script/update
```

### Step 4: Pull the Latest Changes
```bash
./pull.py
```
Runs the project's custom update script. Check for errors before proceeding.

### Step 5: Restart Odoo
```bash
service odoo restart
```
Restarts the Odoo service to load updated code.

### Step 6: Verify the Server
```bash
service odoo status
```
Confirm the service is running and test your changes on QA.

**Note:** Changes to Odoo models, XML views, or security rules may require a module upgrade, not just a restart.

---

## 3. Quick Reference

### Submit a Task
```bash
git checkout master
git pull origin master
git checkout -b feature/task-name
git add .
git commit -m "Task description"
git push -u origin feature/task-name
```
Then create a Merge Request in GitLab.

### Update QA Server
```bash
ssh gigate-qa
sudo -E su
cd /opt/script/update
./pull.py
service odoo restart
service odoo status
```

**Important:** Always verify your branch, review your changes, and follow the team's merge and deployment procedures.
