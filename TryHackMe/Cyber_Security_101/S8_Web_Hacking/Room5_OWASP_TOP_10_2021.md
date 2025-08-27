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

