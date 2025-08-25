# 🔐 Bandit Level: Bandit19 ➝ Bandit20

## 📂 Command(s) used:
👉
- ls -l
- echo "<Bandit20_password>" | nc -l -p 12345 & ./suconnect 12345



## 📄 Password found:
👉 0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO

## 🧠 Notes / What I learned:
👉
# 📘 Linux Notes – Bandit Level & Setuid
``` bash
echo "<Bandit20_password>" | nc -l -p 12345 & ./suconnect 12345
```

## 🔹 Part 1: Commands Used in This Bandit Level

### 1. ls
- Lists files and directories.
- Example: ls -l

### 2. nc (Netcat)
- Create a listener (server): nc -l -p <port>
- Connect to a server: nc localhost <port>

### 3. echo
- Prints text or sends input.
- Example: echo "hello"
- Example with pipe: echo "hello" | nc localhost 12345

### 4. & (Ampersand)
- Runs a command in the background.
- Example: nc -l -p 12345 &

### 5. ./binary
- Runs an executable from the current directory.
- Example: ./suconnect 12345



## 🔹 How These Commands Work Together in the Bandit Level
1. Start listener: nc -l -p 12345  
2. Run binary: ./suconnect 12345  
3. In the listener terminal, paste the Bandit20 password.  
4. If correct → Bandit21 password will be displayed.  

**Shortcut (one line):**  
echo "<Bandit20_password>" | nc -l -p 12345 & ./suconnect 12345  

---

## 🔹 Part 2: Understanding setuid

### 1. What is setuid?
- Special permission in Linux (Set User ID).
- Lets a program run with file owner’s privilege instead of the user running it.

### 2. Example: passwd
- passwd modifies /etc/shadow (root-only file).
- Normal users can’t edit it.
- But passwd has setuid → runs as root → lets you change your password.

Check: ls -l /usr/bin/passwd  
Output shows: -rwsr-xr-x  
(Notice s instead of x → setuid is set)

### 3. How to Set/Remove setuid
- Set: chmod u+s filename  
- Remove: chmod u-s filename  

### 4. Numeric Mode
- 4 = setuid, 2 = setgid, 1 = sticky bit  
- Example: chmod 4755 myfile  

### 5. Security Risks
- If setuid is used on insecure programs, attackers can gain root access.
- That’s why it’s used only for trusted programs (like passwd).
