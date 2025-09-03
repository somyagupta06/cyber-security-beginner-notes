# 🔐 Bandit Level: Bandit28 ➝ Bandit29

## 📂 Command(s) used:
👉 
- mkdir /tmp/bandit28repo
- cd /tmp/bandit28repo
- git clone "ssh://bandit28-git@localhost:2220/home/bandit27-git/repo"
- cd repo
- ls -la
- ls
- cat README


## 📄 Password found:
👉 4pT1t5DENaYuqnqvadYs1oE4QLCdjmJ7


## Notes 
# Bandit28 → Bandit29 Password Extraction Notes


Bandit28 level's password is **not directly in README.md**.  
The file contains only placeholders (`x` or `<TBD>`).  
The actual password is hidden in **Git commit history**.  

This exercise teaches:  
- Git keeps all previous versions of a file.  
- Sensitive data in commits can be recovered even if deleted later.  



## Step 1: Navigate to the repo
```bash
cd /tmp/banditl28/repo
```
## Step 2: Check commit history
```
git log
```
- Shows all commits in the repo.
- Look at commit messages and authors.
## Step 3: View file content in a specific commit
```
git show <commit_id>:README.md
```
- Replace <commit_id> with the commit hash from git log.
Example:
```
git show 710c14a2e43cfd97041924403e00efb00b3a956e:README.md
```
- Output may still show placeholders.
## Step 4: View all changes (diff) in README.md across commits
```
git log -p README.md
```
- Shows line-by-line changes in README.md for every commit.
- Look for the line containing password: → actual password appears in one of the commits.
## Step 5: Extract password quickly
```
git log -p README.md | grep "password"
```
- Filters all lines containing password: across commit history.
- Output shows bandit29's password.
## Step 6: Login to Bandit29
```
 ssh bandit29@bandit.labs.overthewire.org -p 2220
```
- Enter the password extracted from Git history.
- You will successfully log in to level29.
---
# Understanding Git History and Password Extraction

## 1️⃣ Why we needed to check Git commit history

- In Bandit28, the file `README.md` **did not contain the password directly**.  
  It only had a placeholder like `x` or `<TBD>`.  

- However, Git repositories **store the full history of every file**.  
  This means every time a file is committed, Git keeps a snapshot of that file in that commit.

- Therefore, **even if the password was later replaced or hidden in the file**, it still exists in Git's object database.  
  Git does not delete old content unless you rewrite history (e.g., with `git filter-branch`).

- By checking Git history, we can **look at previous versions of the file** and potentially recover the hidden password.  

**Key point:** Git is a version control system, not a secure storage. Anything committed remains accessible via history unless explicitly removed.



## 2️⃣ What we achieved by checking Git history

- Using:
```bash
git log
```
we saw that the repository had 3 commits:
- Initial commit → README.md had placeholder or initial information
- Add missing data → updates to the file
- Fix info leak → last commit which replaced the password with a placeholder
- Then, by using:
```
git show <commit_id>
git log -p README.md
```
we could view previous versions of the README.md file and the differences (diffs) between commits.
- Observations:
   - The line containing password: in previous commits still had the actual password.
   - Even if the latest commit replaced it with a placeholder, the old password was still retrievable.
**Key point:** Git’s versioning allows us to access data that was “deleted” from the current file.
## 3️⃣ Security / Learning Points
- Never commit sensitive information (like passwords, API keys, private keys) to Git.
- Git keeps permanent records of all commits, so even if you remove or change sensitive data later, it can still be accessed through git log, git show, or similar commands.
- Bandit28 demonstrates this principle:
  - The password was hidden in the file → direct reading did not reveal it.
  - The password was still available in Git commit history → using git show or git log -p, it could be extracted.
- This is an important real-world lesson for developers and security engineers. Version control systems are powerful, but careless use can leak secrets.

## 5️⃣ Summary
- Password was not in the current README.md file.
- Git commit history preserved old versions containing the actual password.
- By analyzing commits (git log, git show, git log -p), the password could be recovered.
- Lesson: Be very careful with sensitive information in Git.
