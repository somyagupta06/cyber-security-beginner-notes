# 🔐 Bandit Level: Bandit0 ➝ Bandit1



### 📂 Command(s) used: 

👉 
   - ssh bandit0@bandit.labs.org -p 2220      
   - Ls
   - Cat

---
   
### 📄 Password found:


👉

  ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If

---

## 🧠 Notes / What I learned:
👉 
## SSH

### 1️⃣ What is SSH ?

SSH stands for Secure Shell.

It's a secure way to access another computer (remote machine) over the internet or a local network — especially used in hacking labs, servers, or Linux systems you can’t physically touch.


Example: You log in to a TryHackMe or OverTheWire machine using SSH from your own computer.

### 2️⃣ Basic SSH Command Syntax

``` bash
ssh username@host -p port
```

- **ssh**	--           Command to start a secure connection
- **username**	--       The user you’re logging in as
- **@host**	 --         The IP address or domain name of the remote system
- **-p**	  --           Port number (optional if it's not the default 22)

### 3️⃣ Entering the Password

When you run the ssh command, the terminal will ask for a password:
``` bash
bandit0@bandit.labs.overthewire.org's password:
```
- Just type the password and press Enter.
- The password won’t be visible (no dots or stars) — this is normal.
---
## ls

### 1️⃣ What is ls Command ?

– List Files & Folders
**Purpose:** Shows files and folders in the current directory.

### 2️⃣ Basic ls Command Syntax
``` bash
ls [options] [directory]
```
---
## cat

### 1️⃣ What is cat Command ?

- View File Content
**Purpose:** Reads and shows the content of a file in the terminal.

### 2️⃣ Basic cat Command Syntax
``` bash
cat [options] filename
```
