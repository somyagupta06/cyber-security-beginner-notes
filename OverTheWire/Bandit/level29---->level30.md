# 🔐 Bandit Level: Bandit29 ➝ Bandit30

## 📂 Command(s) used:
👉 
- mkdir /tmp/bandit29repo
- cd /tmp/bandit29repo
- git clone "ssh://bandit29-git@localhost:2220/home/bandit29-git/repo"
- cd repo
- ls -la
- ls
- cat README
<img width="572" height="467" alt="Screenshot 2025-09-04 at 2 24 23 PM" src="https://github.com/user-attachments/assets/2b43840a-5de6-4412-84ec-de2d5f8b02a9" />

<img width="570" height="666" alt="Screenshot 2025-09-04 at 2 24 38 PM" src="https://github.com/user-attachments/assets/d04af03f-cc89-465b-aba7-06a5901cddf0" />


## 📄 Password found:
👉 
qp30ex3VLz5MDG1n91YowTv4Q8l7CDZL

## Notes 
## 🛠️ Commands Used

```bash
# 1. See hidden files (to check .git folder exists)
ls -a

# 2. View commit history
git log

# 3. Show file contents from a specific commit
git show <commit-id>:README.md

# 4. See commit differences (to check what changed)
git log -p

# 5. List all branches (local + remote)
git branch -r

# 6. Checkout remote branch
git checkout origin/<branchname>

# 7. Display README.md file in that branch
cat README.md

# 8. Fetch tags (if needed)
git fetch --tags

# 9. List available tags
git tag

# 10. Show tag details
git show <tagname>
```
## 📖 Detailed Explanation
### 1. Git Repository
- The directory given contains a hidden .git folder.
- That means it is a Git repository, and we can use git commands to explore its history.
### 2. Commit History (git log)
- Each time a developer saves changes, a commit is created.
- git log shows:
  - Commit ID
  - Author
  - Date
  - Commit message
- Using git show <commit-id>:README.md, we can see how the file looked at that point in history.

👉 In this level, commit history only showed username changes and the password placeholder <no passwords in production!>.
### 3. Branches
- A branch is like a separate line of development.
- Developers use branches to test features without affecting the main code.
- The command git branch -r lists remote branches from the original repository.
- Example output:
```
origin/master
origin/dev
origin/sploits-dev
```
👉 The trick in this level is that the password is not in master branch commits, but in another branch (dev or sploits-dev).
### 4. Checking Branches
- With git checkout origin/<branchname>, you move to that branch.
- Then you check README.md:
```
cat README.md
```
- In one of these hidden branches, the actual password for bandit30 is revealed.
### 5. Tags
- A tag is like a bookmark for a specific commit, often used for marking versions (e.g., v1.0, release-2025).
- Tags can also hide secrets.
- Command git fetch --tags && git tag lists them.
- git show <tagname> reveals its content.
- 
👉 In Bandit28, the password was hidden in commit history.

👉 In Bandit29, they increased the difficulty: the password is hidden in a branch (not commits).

👉 In later levels, it may be hidden in tags.

## 🧠 The Trick / Thought Process
- Humans usually check only the main branch (master) and recent commits.
- Security challenge: hide the password in a less obvious place.
- This level forces you to explore branches (and learn that real attackers also search hidden branches/tags in repos for secrets).
## ✅ Key Takeaways
- Commits → Past versions of files. Use git log + git show.
- Branches → Alternative development lines. Use git branch -r + git checkout.
- Tags → Bookmarks for specific commits. Use git tag + git show.
- Always explore history, branches, and tags when auditing repositories for secrets.

