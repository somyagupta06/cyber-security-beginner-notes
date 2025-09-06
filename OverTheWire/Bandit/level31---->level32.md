# 🔐 Bandit Level: Bandit30 ➝ Bandit31

## 📂 Command(s) used:
👉 
- mkdir /tmp/bandit31repo
- cd /tmp/bandit31repo
- git clone "ssh://bandit31-git@localhost:2220/home/bandit31-git/repo"
- cd repo
- ls
- cat README.md
- echo "May I come in?" > key.txt
- git add -f key.txt
- git commit -m "Added key.txt file"
- git push origin master
<img width="574" height="742" alt="Screenshot 2025-09-06 at 12 29 25 PM" src="https://github.com/user-attachments/assets/690cf700-d988-4d53-a149-e24458c7d672" />


<img width="579" height="634" alt="Screenshot 2025-09-06 at 12 29 38 PM" src="https://github.com/user-attachments/assets/b1953c96-aa4e-4c5c-840f-9787f5cbaac1" />


## 📄 Password found:
👉 
3O9RfhqyAlVBEZpVb6LYStshZoqoSx5K

## NOTES

##🔹 COMMANDS USED
```
# 1. Go inside the repo (already given)
cd /tmp/gitrepo31/repo

# 2. Create the required file with given content
echo "May I come in?" > key.txt

# 3. Try adding file to git (will be ignored because of .gitignore)
git add key.txt

# 4. Force add file (bypassing .gitignore)
git add -f key.txt

# 5. Commit the file
git commit -m "Added key.txt file"

# 6. Push to the remote master branch
git push origin master

# 7. Server validates and gives password for next level
# Password shown: 3O9RfhqyAlVBEZpVb6LYStshZoqoSx5K
```
## 🔹 Git Push Detailed Notes
### 📌 What git push does
- Sends commits from local repository → remote repository.
- Syntax:
```
git push <remote-name> <branch-name>
```
### 📌 Basic Workflow
```
# Initialize repo
git init

# Clone existing repo
git clone <repo-url>

# Stage files
git add <filename>
git add .   # add everything

# Commit changes
git commit -m "message"

# Add remote
git remote add origin <repo-url>

# Push to remote branch
git push origin main
git push origin master
```
### 📌 First Push
```
git push -u origin main
```
-u sets upstream (future pushes need only git push).
### 📌 Normal Push
```
git push
```
Works if upstream already set.
### 📌 Push New Branch
```
git checkout -b new-feature
git push -u origin new-feature
```
### 📌 Force Push (⚠️ dangerous)
```
git push origin main --force
```
Overwrites remote history.
### 📌 Push All Branches
```
git push --all origin
```
### 📌 Push Tags
```
git push origin --tags
```
### 📌 Fix Rejected Push (remote ahead)
```
git pull origin main --rebase
git push origin main
```
### 📌 Rename / Change Upstream Branch
```
git branch -M main
git push -u origin main
```
### 📌 Push with Authentication (GitHub/GitLab)
- Using token:
```
git push https://<token>@github.com/username/repo.git
```
- Using SSH:
```
git remote set-url origin git@github.com:username/repo.git
git push origin main
```
## 🔹 Summary Table
|Case	|Command|
|-----|-----|
First push|	git push -u origin main
Normal push	|git push
New branch push	|git push -u origin branch-name
Force push	|git push --force origin branch-name
Push all branches	|git push --all origin
Push tags|	git push origin --tags
Fix rejected push	|git pull --rebase origin main && git push
Rename branch + push	|git branch -M main && git push -u origin main
