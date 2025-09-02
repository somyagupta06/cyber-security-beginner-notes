# 🔐 Bandit Level: Bandit27 ➝ Bandit28

## 📂 Command(s) used:
👉 
- mkdir /tmp/bandit27repo
- cd /tmp/bandit27repo
- git clone "ssh://bandit27-git@localhost:2220/home/bandit27-git/repo"
- cd repo
- ls -la
- ls
- cat README

<img width="774" height="612" alt="Screenshot 2025-09-03 at 12 15 42 AM" src="https://github.com/user-attachments/assets/78985cf1-79f7-49a3-b6de-c19fd981b9b8" />


## 📄 Password found:
👉 
Yz9IpL0sBcCeuG7m9uQFt8ZNpS4HZRcN

## Notes 
## 🎯 Goal
Clone the remote Git repository located at:
ssh://bandit27-git@localhost:2220/home/bandit27-git/repo
and find the password for the next level.

---

## 🛠️ Step-by-Step Commands

1. **Check current directory**
```
pwd
```
2. **Create a writable directory in /tmp**
```
mkdir /tmp/bandit27repo
```
3. **Move into that directory**
```
cd /tmp/bandit27repo
```
4. **Clone the repository using SSH and port 2220**
```
git clone ssh://bandit27-git@localhost:2220/home/bandit27-git/repo
```
5. **Move into the cloned repository**
```
cd repo
```
6. **List all files**
```
ls -la
```
7. **Read the README or any important file**
```
cat README
```
8. **Check git history (sometimes password is hidden in commits)**
```
git log
```
9. **Show details of a commit**
```
git show <commit_hash>
```
10. **Check all branches**
```
git branch -a
```
11. **Switch to another branch if needed**
```
git checkout <branch_name>
```
---

# 📘 Understanding Git (Basics)

## 🔹 What is Git?
- Git is a **distributed version control system**.  
- It helps developers track changes in code, collaborate, and manage project versions.  
- Used in almost all software development projects.

---

## 🔹 Basic Git Workflow
1. Make changes to files.  
2. Stage changes (`git add`).  
3. Commit changes (`git commit`).  
4. Push changes to remote repo (`git push`).  
5. Pull updates from remote repo (`git pull`).  

---

## 🔹 Common Git Commands

### Clone a repository
```
git clone <repo_url>
```
### Check status of working directory
```
git status
```
### Add files to staging area
```
git add <file>
git add .
```
### Commit changes
```
git commit -m "your message"
```
### Push changes to remote repo
```
git push origin <branch_name>
```
### Pull updates from remote repo
```
git pull
```
### View commit history
```
git log
```
### Show details of a commit
```
git show <commit_hash>
```
### List all branches
```
git branch -a
```
### Switch branch
```
git checkout <branch_name>
```
### Create new branch
```
git branch <branch_name>
```
### Merge branch
```
git merge <branch_name>
```
---

# 📌 Linking Back to Bandit 27 → 28
- In this level, you practiced cloning a Git repo using SSH with a custom port (`2220`).  
- Then you explored the repo using `git log`, `git show`, and checked files to find the hidden password.  
- This exercise connects directly with real-world Git usage in cybersecurity (finding secrets, old commits, hidden files).  

---
