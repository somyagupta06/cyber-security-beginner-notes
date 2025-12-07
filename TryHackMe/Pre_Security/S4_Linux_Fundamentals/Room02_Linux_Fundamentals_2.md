# 🐧 SSH, Command Arguments & Filesystem Operations 

---

# 🔐 1. What is SSH?

**SSH (Secure Shell)** is a protocol that lets you **control another device remotely** using the terminal — safely and securely.

### How SSH Works:
- You type a command (human-readable text)
- SSH **encrypts it**
- It travels across the internet safely
- Remote machine **decrypts** and executes it  
- Same happens for data coming back

So:
✔ You can run commands on another computer  
✔ No one can read your data in the middle  

---

# 🏳️ 2. Commands + Arguments (Flags)

Most Linux commands accept **arguments** (extra options) to change their behavior.

Example:
ls # default behavior
ls -a # show hidden files

Arguments usually start with:
- `-` (short flag) e.g., `-a`
- `--` (long flag) e.g., `--all`

---

# 👀 3. Hidden Files in Linux

Any file starting with **"."** is hidden.

Example:
ls
folder1

Show hidden stuff:
ls -a
.hiddenfolder folder1

Hidden files are often used for:
- config files
- system files
- history files

---

# 📘 4. Getting Help (ls --help)

Most commands support:
--help

This prints useful flags and examples.

Example:
ls --help

Shows options like:
- `-a` → show all files  
- `-l` → long listing  
- `-d` → show directory itself, not contents  

---

# 📚 5. man Pages (Manual Pages)

`man` gives full documentation for any command.

man ls
man ssh
man grep

Inside man:
- press `q` to quit  
- press `/word` to search  

---

# 🗂️ 6. Creating Files and Folders

### touch → create empty file
touch note

### mkdir → create folder
mkdir mydirectory

---

# 🧹 7. Removing Files and Folders

### Remove file:
rm note

### Remove directory (recursively):
rm -R mydirectory

⚠️ WARNING  
`rm -R` is dangerous — it deletes permanently.

---

# 📄 8. Copying and Moving Files

### cp → Copy a file
Syntax:
cp oldfile newfile

Example:
cp note note2

### mv → Move OR Rename a file
mv note2 note3 # renames note2 → note3
mv file.txt folder/ # moves file

---

# 📑 9. file Command — Detect File Type

Even if a file has no extension, Linux can detect its type.

file note

Example output:
note: ASCII text

Useful for:
- binaries
- images
- scripts
- unknown files

---

# ⭐ FINAL QUICK SUMMARY

| Command | Purpose |
|---------|----------|
| ssh user@host | remote secure login |
| ls / ls -a | view files / show hidden files |
| ls --help | quick guide |
| man cmd | full documentation |
| touch | create file |
| mkdir | create directory |
| rm / rm -R | delete file / delete folder |
| cp | copy file |
| mv | move / rename file |
| file | detect file type |

---

# 🔐 Linux Permissions, Users, Groups & Switching Users 

---

# ⭐ Why Can't Every User Access Every File?

Linux is very strict about **who can open, edit, or run** a file.  
This is for **security** — so users cannot mess with each other’s stuff.

To check file permissions, we use:

ls -l

Example:
-rw-r--r-- 1 cmnatic cmnatic 0 Feb 19 10:37 file1
-rw-r--r-- 8 cmnatic cmnatic 0 Feb 19 10:37 file2

We only care about the **first 3 columns**:

| Column | Meaning |
|--------|----------|
| 1 | File permissions |
| 2 | Owner (User who owns the file) |
| 3 | Group (Group that can also access file) |

---

# 🔎 Understanding Permissions (r, w, x)

A file or folder can allow 3 types of actions:

| Symbol | Meaning | Simple Explanation |
|--------|----------|--------------------|
| r | Read | Can view file |
| w | Write | Can edit/delete file |
| x | Execute | Can run file like a program |

Example permissions:
-rw-r--r--

Breakdown:

| Part | Who? | What they can do |
|------|-------|----------------------|
| rw- | Owner | read + write |
| r-- | Group | read only |
| r-- | Others | read only |

---

# 👥 Users vs Groups 

Linux has:

### ✔ Users  
Ek individual account — jaise `cmnatic`, `user1`.

### ✔ Groups  
Ek team jisme multiple users ho sakte hain.  
A group can have permissions **separate** from the file owner.

### Real-life Example:
- A web server runs under user **www-data**
- A customer uploads their files as **customer1**
- Customer must not gain access to other customers' files
- But web server still needs permission to read files  

So Linux uses **user + group permissions** to control who can access what.

---

# 🔄 Switching Between Users (su Command)

Sometimes you need to become another user.  
Linux allows this with:

su username

To switch users, you must know:

1️⃣ The **username**  
2️⃣ The **password** of that user  
(unless you're root)

### Example:
su user2
Password:
user2@linux2:/home/tryhackme$

⚠ Notice:  
Even after switching, you still remain in the **old user's home directory**.

---

# 🎯 su -l (or --login)

`su -l username` creates a **real login shell** for the new user.  
This means:

✔ You get new user’s environment variables  
✔ You start inside new user’s home directory  
✔ More similar to a fresh login  

Example:
su -l user2
Password:
user2@linux2:~$ pwd
/home/user2

Now you start from `/home/user2` — correct home directory.

---

# 🌟 Summary Table

| Command | Purpose |
|----------|----------|
| ls -l | See permissions, owner, group |
| r, w, x | Read, Write, Execute |
| su user | Switch user (but stay in old directory) |
| su -l user | Proper login shell, new environment, new home directory |
| owner | The user who owns the file |
| group | A team that can also get permissions |

---

# 🧠 Quick Memory Trick

- **User** = individual  
- **Group** = team  
- **Others** = everyone else  

Permissions apply like:
[Owner] [Group] [Others]
rw- r-- r--

---

# 📁 Important Linux Directories
In Linux, the top-level of the filesystem is `/` which is called the **root directory**.  
Inside `/` there are many important folders.  
In this lesson we focus on:

- `/etc`
- `/var`
- `/root`
- `/tmp`

Let's break them down super simply.

---

# 🗂️ 1. /etc — “System Settings Folder”

Think of **/etc** as the **control room** of Linux.  
Here the system keeps **configuration files** that tell Linux how to behave.

### What lives in /etc?

| File | Meaning |
|------|---------|
| `sudoers` | Defines which users can run `sudo` (admin commands) |
| `passwd` | List of system users (but NOT passwords!) |
| `shadow` | Encrypted passwords (sha512 hash) |
| Many more | Network configs, service configs, system rules |

Example:
/etc/passwd
/etc/shadow
/etc/sudoers

### Why is /etc important?

Because ANYTHING related to:
✔ users  
✔ passwords  
✔ sudo permissions  
✔ services  
✔ system configuration  

…is stored here.

---

# 📁 2. /var — “Variable Data Folder”

**var = variable data**  
Meaning: Things that **change often** are stored here.

### What lives in /var?

| Folder | Meaning |
|--------|---------|
| `/var/log` | logs from system + applications |
| `/var/backups` | system backups |
| `/var/tmp` | temp data used by applications |
| `/var/opt` | extra software data |

Example:
/var/log/auth.log # login attempts
/var/log/syslog # system messages

### Why important?

Because logs help you understand:
✔ what the system is doing  
✔ who logged in  
✔ which service failed  
✔ what errors occurred  

---

# 👑 3. /root — “Root User’s Home Folder”

This is **not** the same as `/root directory` (the top-level `/`).  
This folder is the **home directory of the root user**.

Example:
/root/myfile
/root/passwords.xlsx

### Key point:
Only **root** can access this folder.  
Normal users cannot even list files here.

So it’s like the **private bedroom of the root user**.

---

# 🧹 4. /tmp — “Temporary Folder”

`tmp = temporary`

This folder is for files needed **for a short time** only.

### Important rules:

✔ ANY user can read/write here  
✔ Files are **not important**  
✔ Folder is deleted automatically after reboot  

Example usage:
- store scripts during pentesting  
- temporary downloads  
- session files  
- program cache

Example files:
/tmp/todelete
/tmp/trash.txt
/tmp/rubbish.bin

### Why useful for hackers/pentesters?

Because:
- you can upload your scripts here  
- you don’t need special permissions  
- system normally ignores this folder  
- gets wiped automatically (good for covering tracks 😉)

---

# 🎯 Summary Table

| Directory | Purpose | Who can access? |
|-----------|----------|--------------------|
| **/etc** | System configuration, users, passwords, services | Mostly root |
| **/var** | Logs, backups, service data | Everyone (but some files root-only) |
| **/root** | Root user’s home folder | Only root |
| **/tmp** | Temporary files, wiped on reboot | All users |

---

# 🧠 Memory Trick

- **/etc** → “Everything to configure”  
- **/var** → “Variable changing data”  
- **/root** → “Root’s bedroom”  
- **/tmp** → “Temporary trash bin"
