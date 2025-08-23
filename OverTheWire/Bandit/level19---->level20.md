# 🔐 Bandit Level: Bandit19 ➝ Bandit20

## 📂 Command(s) used:
👉
- cat ./bandit20-do
- ./bandit20-do
- ./bandit20-do id
- ./bandit20-do cat /etc/bandit_pass/bandit20

<img width="361" height="46" alt="Screenshot 2025-08-23 at 8 05 08 PM" src="https://github.com/user-attachments/assets/04213b14-b706-4f3f-81f4-b9a78c30222e" />

<img width="565" height="120" alt="Screenshot 2025-08-23 at 8 06 05 PM" src="https://github.com/user-attachments/assets/da8fda5c-74d1-4711-8dfc-40a3d2b68e22" />


## 📄 Password found:
👉 0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO

## 🧠 Notes / What I learned:
 # Bandit Level 19 → 20 (OverTheWire)

---

## 📝 Problem Statement

- You are logged in as `bandit19`.
- You need to get the password for `bandit20`.
- In your home directory, there is a file/program called `bandit20-do`.

---

## 🔑 Key Concepts Involved

1. **SUID (Set User ID)**
   - If a file has the `s` (SUID bit) set, then when someone runs the program,  
     it runs with the **permissions of the file’s owner**, not the user who runs it.
   - Example: If a file is owned by `root` and has SUID, anyone who executes it will run it as `root`.

   - Example permissions output:
     ```
     -rwsr-xr-x  1 root root 12345 Aug 23 bandit20-do
         ^
         s = SUID set
     ```

2. **The File `bandit20-do`**
   - This is a helper binary.
   - Owner = `bandit20`
   - It has the **SUID bit set**.
   - Meaning: whenever you run it, it will actually run as `bandit20`.

3. **Target**
   - Bandit20’s password is stored in:
     ```
     /etc/bandit_pass/bandit20
     ```
   - Normal `cat` cannot read it because `bandit19` doesn’t have permission.
   - But with `bandit20-do` (which runs as `bandit20`), you can read it.

---

## 🖥️ Step by Step Process

1. List files in your home directory  
   `ls -l`

2. Check the owner and permissions of `bandit20-do`  
   `ls -l bandit20-do`

   Example output:
-rwsr-x--- 1 bandit20 bandit19 12345 Aug 23 20:00 bandit20-do
- `rws` → owner permissions (with SUID set, `s`)
- owner = `bandit20`
- group = `bandit19`
- So: when you run this file, it executes as **bandit20**.

3. Run the file directly to see instructions  
`./bandit20-do`

It will tell you: *“Run a command as another user.”*

4. Use the file to read Bandit20’s password  
`./bandit20-do cat /etc/bandit_pass/bandit20`

This will print the Bandit20 password.

5. Login as bandit20 with the found password  
`ssh bandit20@bandit.labs.overthewire.org -p 2220`

---

## ⚡ Quick Recap

- `bandit20-do` is a **SUID binary** owned by `bandit20`.
- Running `./bandit20-do <command>` means the `<command>` runs as `bandit20`.
- Bandit20’s password is inside `/etc/bandit_pass/bandit20`.
- Final working command:  
`./bandit20-do cat /etc/bandit_pass/bandit20`

---

## 📌 Important Takeaways

- **SUID bit** allows programs to run with the **file owner’s privileges**.
- Useful for tasks where a normal user cannot access a file or resource.
- Dangerous if used carelessly (security risks).
- In Bandit, this mechanism is used for moving safely from one level to the next.
---
# 🔐 Linux File Permissions + Special Permissions (SUID, SGID, Sticky Bit)

---

## 🔑 Basics – Normal File Permissions

In Unix/Linux, every file or directory has **3 types of permissions**:

- **r (read)** → can read the file
- **w (write)** → can modify the file
- **x (execute)** → can run the file (if it’s a program/script)

And these permissions apply separately to 3 categories of users:

1. **Owner** → the actual creator/owner of the file  
2. **Group** → team members who belong to the same group  
3. **Others** → everyone else  

Example (`ls -l file.txt`):  
-rwxr-x---
- `rwx` → owner can read/write/execute  
- `r-x` → group can read and execute  
- `---` → others have no permission  

---

## 👑 Special Permissions (Setuid, Setgid, Sticky bit)

Normal permissions are enough for most tasks.  
But some **extra powerful tasks** need **special flags**:  

- **SUID (Set User ID)**  
- **SGID (Set Group ID)**  
- **Sticky Bit**

---

### 1. SUID (Set User ID)

📌 Meaning: When a user runs an executable file with SUID, the program runs with the **owner’s privileges**, not the user’s.  

👉 Example:  
`/usr/bin/passwd` is used to change a user’s password.  
- It edits `/etc/passwd` and `/etc/shadow`, which only `root` can modify.  
- To allow normal users to run it, the file has **SUID set**.  
- This way, when "Thompson" or "Raju" runs `passwd`, it executes as **root**.  

⚡ Command to check:  
`stat -c "%a %U:%G %n" /usr/bin/passwd`

Output:  
4701 root:root /usr/bin/passwd

🔎 Breakdown:  
- `4` → SUID set  
- `7` → owner has `rwx`  
- `0` → group has no permissions  
- `1` → others can execute  

So when Thompson runs `passwd`, it uses **root’s power** to update the password.

---

### 2. SGID (Set Group ID)

📌 Meaning: When SGID is set on a **directory**, any new file/folder created inside will **inherit the group** of the parent directory, instead of the creator’s primary group.  

👉 Example:  
Create a directory `music` owned by group `engineers`:  
`chmod 2770 music`

- Now if `torvalds` (who belongs to group `engineers`) creates a file inside,  
  the new file’s group will also be `engineers`.  

⚡ Command to check:  
`stat -c "%a %U:%G %n" ./music/`

Output:  
2770 root:engineers ./music/

Then if `torvalds` makes a folder:  
`mkdir ./music/electronic`  
Check:  
`stat -c "%U:%G %n" ./music/electronic/`

Output:  
torvalds:engineers ./music/electronic/

👉 Useful for **collaboration** among groups.

---

### 3. Sticky Bit

📌 Meaning: If Sticky Bit is set on a directory, only the **file owner** can delete/rename/move their files, even if everyone has write access to the directory.  

👉 Example:  
The `/tmp` folder (shared by all users).  
- Without Sticky Bit: any user could delete another user’s file.  
- With Sticky Bit: only the file creator can delete their file.  

Command:  
`chmod 1770 videogames`

- If `torvalds` creates `tekken` file,  
- and `wozniak` (same group) tries to delete:  
`rm tekken`

Output:  
rm: cannot remove ‘tekken’: Operation not permitted

👉 Sticky Bit **protected the file**.

---

### 4. Sticky Bit + SGID Combo

📌 If both are set (example: `chmod 3171 blog`):  

- SGID → new files inherit the directory’s group  
- Sticky Bit → only the file owner can delete/rename/move their files  
- BUT if file permissions allow editing, other group members can still edit (not delete).  

---

## 🤯 Security Considerations

- **SUID/SGID are very powerful** → can be dangerous.  
- If a vulnerable program has SUID set (e.g., buffer overflow exploit), attacker may get **root access**.  
- For this reason:  
  - SUID doesn’t work on scripts (bash/python), only on binaries.  

---

## ✅ Quick Recap (easy numbers)

- **SUID = 4XXX** → Program runs as **file owner** (e.g., root)  
- **SGID = 2XXX** → New files inherit **directory’s group**  
- **Sticky Bit = 1XXX** → Only file owner can delete/rename their files  

---
