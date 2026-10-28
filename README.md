# Odoo Developer Notes

## GitLab Workflow & QA Server Deployment Guide

GitLab Workflow & Odoo QA Server Deployment Guide
This guide covers submitting a development task through GitLab and updating the Odoo QA server after the appropriate merge and approval.
1. Submit a New Task to GitLab
Step 1 — Update your local master branch
Before starting, make sure your working tree is clean (`git status`). Save or stash unfinished work before switching branches.
```bash
git checkout master
git pull origin master
```
`git checkout master` switches to your local master branch.
`git pull origin master` downloads and integrates changes from remote master.
If your team uses a different starting branch, use that branch instead.
Step 2 — Create and switch to a task branch
```bash
git checkout -b feature/task-name
```
Example:
```bash
git checkout -b fix/shopify-order-notification
```
Use a descriptive name such as `feature/...`, `fix/...`, or `hotfix/...`. This isolates your task from master.
Step 3 — Implement and review changes
Write your code, test it locally, and inspect the changes:
```bash
git status
git diff
```
Step 4 — Stage files
```bash
git add .
```
This stages changes under the current directory. To stage only a particular file:
```bash
git add path/to/file.py
```
Review staged changes with `git diff --staged`. Do not commit secrets, local configuration, or unrelated files.
Step 5 — Commit your work
```bash
git commit -m "Fix Shopify order notification on state change"
```
Use a clear message explaining what changed. A commit records staged changes locally.
Step 6 — Push / publish the branch to GitLab
```bash
git remote -v
git push -u origin fix/shopify-order-notification
```
Check that `origin` points to the intended GitLab repository (particularly if you also have a GitHub remote).
`-u` sets the upstream branch, so later pushes can use `git push`.
In VS Code, Source Control → Publish Branch is an alternative if the correct remote is selected.
Step 7 — Open a GitLab Merge Request (MR)
Open your project in GitLab and select Merge Requests → New Merge Request.
Choose your task branch as the source branch.
Choose QA or master as the target branch, according to your team's instructions.
Write a descriptive title and summary, including testing performed and any Odoo module upgrade required.
Assign the reviewer(s), then create the MR.
Wait for required review, pipeline checks, and approval before merging.
> Creating an MR does not itself merge or deploy code. Do not assume QA has been updated until the correct branch has been merged and deployed.
---
2. Update and Restart the Odoo QA Server
Prerequisites: Confirm the approved changes are merged into the branch deployed to QA, the deployment window is appropriate, and you are authorized to restart the service.
Step 1 — Connect over SSH
```bash
ssh gigate-qa
```
Uses your configured SSH alias to log in to the QA server.
Step 2 — Switch to root
```bash
sudo -E su
```
`sudo` elevates privileges, `-E` requests preservation of the environment (subject to policy), and `su` switches to root. Use this only if the server workflow requires it.
Step 3 — Go to the update scripts
```bash
cd /opt/script/update
```
Step 4 — Run the custom pull script
```bash
./pull.py
```
This runs the project's update script. Its exact behavior depends on its implementation; confirm it updates the intended QA checkout and check its output for failures before restarting.
Step 5 — Restart Odoo
```bash
service odoo restart
```
Restarts the Odoo service to load updated server-side Python code. A restart does not automatically upgrade Odoo modules; changes to fields, views, access rights, or module data may require a separate module upgrade following the team's deployment procedure.
Step 6 — Verify deployment
```bash
service odoo status
```
Confirm Odoo is running, open the QA site, and test the feature. If startup fails, check the server's Odoo logs using the team's normal procedure.
Step 7 — Disconnect
```bash
exit
exit
```
The first command exits the root shell; the second exits the SSH session.
---
Quick Reference
New task
```bash
git checkout master
git pull origin master
git checkout -b feature/task-name
# Implement and test your changes
git status
git diff
git add .
git diff --staged
git commit -m "Describe the change"
git push -u origin feature/task-name
```
Then create a Merge Request in GitLab from your task branch to the team-approved target branch.
QA deployment (after merge and approval)
```bash
ssh gigate-qa
sudo -E su
cd /opt/script/update
./pull.py
service odoo restart
service odoo status
exit
exit
```
Reminder: Check for pull-script errors before restarting and confirm whether the Odoo module needs an upgrade.
