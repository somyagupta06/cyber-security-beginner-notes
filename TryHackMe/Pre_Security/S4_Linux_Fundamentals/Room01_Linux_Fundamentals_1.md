# 🐧 Linux Fundamentals – Part 1 
Welcome to the first step of becoming a **Linux wizard**!  
You’re probably using **Windows** or **Mac**, but Linux is just another operating system — and it's everywhere!

---

# 🌍 Where is Linux Used?

Linux is lightweight, powerful, and open-source.  

---
# 🐧 Linux Basics – Terminal, Navigation, Find, Grep (Super Simple Notes)

Linux is lightweight, fast, and often has **no GUI** (no desktop, no icons).  
So we use the **Terminal** → a text-based interface.

Example terminal:
tryhackme@linux1:~$

---

# 🟦 BASIC COMMANDS

## 1️⃣ echo  
Print (output) text on the screen.

echo Hello
echo "Hello Friend!"

Use quotes when text has spaces.

---

## 2️⃣ whoami  
Shows which user you are logged in as.

whoami

---

# 🟩 Navigating the Filesystem

You must know how to:
- list files  
- move between folders  
- read file contents  
- find where you currently are  

---

## 3️⃣ ls (list files)
Shows files/folders in the current directory.

ls
ls Pictures # list contents of Pictures without going inside

---

## 4️⃣ cd (change directory)
Move into another folder.

cd Pictures
cd /home/ubuntu/Documents

Go back one directory:
cd ..

Go to home folder:
cd ~

---

## 5️⃣ cat (view file contents)
Shows contents of any file.

cat todo.txt
cat folder1/passwords.txt

It’s useful for:
- passwords
- flags
- configs
- notes

---

## 6️⃣ pwd (print working directory)
Shows full path of where you currently are.

pwd

Example:
/home/ubuntu/Documents

---

# 🟦 Searching in Linux (The Powerful Part)

Linux is super efficient because you can search **everywhere** using commands.

---

# 🔍 7️⃣ find – search for files

### Find file by name
find -name passwords.txt

Output:
./folder1/passwords.txt

### Find all .txt files (using wildcard *)
find -name *.txt

This shows:
- folder1/passwords.txt  
- Documents/todo.txt  

Find is SUPER powerful — saves time searching manually.

---

# 🟧 8️⃣ grep – search inside files

If you want to find a specific word, IP, username, error, etc. **inside** a file → use grep.

Example log file has 244 lines:
wc -l access.log

Search for all visits from an IP:
grep "81.143.211.90" access.log

Output shows:
- date  
- request  
- browser  
- path  

Grep is perfect for:
- log analysis  
- debugging  
- finding sensitive info  
- searching many lines fast  

---

# 🟨 SUPER SIMPLE SUMMARY

| Command | Meaning | Example |
|---------|---------|---------|
| `echo` | Print text | echo "Hi" |
| `whoami` | Show username | whoami |
| `ls` | List files | ls Documents |
| `cd` | Change folder | cd Pictures |
| `cat` | Read file | cat notes.txt |
| `pwd` | Show full path | pwd |
| `find` | Search for files | find -name *.txt |
| `grep` | Search inside files | grep "error" log.txt |

---

# 🧠 Why These Commands Matter

These skills let you:
- navigate Linux without GUI  
- inspect files for flags/passwords  
- search through massive file sets  
- read logs like a pro  
- investigate systems quickly  

This is **the foundation of hacking & sysadmin work**.

---
# 🐧 Linux Operators – Super Simple Notes

Linux operators make your commands **more powerful, faster and smarter**.  
Here are the 4 operators you must know:

| Operator | Meaning | Use Case |
|---------|---------|-----------|
| `&` | Run command in background | Long tasks (copy, download) |
| `&&` | Run next command only if previous was successful | Combine steps |
| `>` | Redirect output (overwrite file) | Create or replace file content |
| `>>` | Redirect output (append) | Add text to bottom of file |

---

# 1️⃣ Operator: `&`  (Run in Background)

Normally, when a long command runs, your terminal gets “busy”.  
But with `&`, the command runs **in background**, so you can keep using the terminal.

Example:
cp bigfile.iso backup/ &
This copies a large file **in the background**.

---

# 2️⃣ Operator: `&&` (Run Commands in Sequence)

`command1 && command2`  
→ command2 **only runs** if command1 was successful.

Example:
mkdir test && cd test
Meaning:  
- If folder creates successfully → go inside  
- If folder fails to create → don’t run cd

---

# 3️⃣ Operator: `>`  (Redirect Output – Overwrite)

Takes output of a command and **sends it to a file**.  
If the file exists, it gets **replaced**.

Example: create file "welcome" containing the word "hey":
echo hey > welcome

Check file:
cat welcome
Output: hey

⚠️ Important:  
`>` **overwrites** the file. Old content gone.

---

# 4️⃣ Operator: `>>`  (Redirect Output – Append)

Same as `>` but it **adds** content at the bottom of the file instead of replacing it.

Example:  
We already have:

welcome → contains:
hey

Add "hello" to the bottom:
echo hello >> welcome

Now file becomes:
hey
hello

This is useful for:
- logs  
- note-taking  
- adding new lines to scripts  

---

# ⭐ Quick Visual Summary

| Action | Operator | Example |
|--------|----------|---------|
| Run in background | `&` | `sleep 10 &` |
| Run next command only if success | `&&` | `apt update && apt upgrade` |
| Create/overwrite file | `>` | `echo hi > file.txt` |
| Append to file | `>>` | `echo hi >> file.txt` |

---
