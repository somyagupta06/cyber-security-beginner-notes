# **🛡️ Offensive Security**


> **"To outsmart a hacker, you need to think like one!"**  


---

## **Overview**

Offensive Security involves --
- Breaking into computer systems and understanding attacker mindset. 
- Exploiting software bugs. 
- Identifying loopholes in applications to gain unauthorized access. 
---
## Goals

- Understand hacker tactics and methodologies.
- Improve system defense strategies.
  
---

## Key Concept

### ⚡ Vulnerability 

→ *कमजोरी (Weakness)*  

A vulnerability is a weakness in a system that attackers can exploit to gain unauthorized access.



### ⚡ Types of Vulnerabilities

1. SQL Injection
   - Allows attackers to inject malicious code into database queries.  
   - Can lead to unauthorized access and extraction of confidential data.

2. Cross-Site Scripting (XSS)  
   - Injecting malicious JavaScript into web applications.  
   - Can manipulate website behavior or steal user data.

3. Broken Authentication 
   - Weak login systems enable password guessing or session hijacking.  
   - Can compromise user accounts and sensitive data.

4. Insecure Password Storage 
   - Storing passwords in plain text without hashing.  
   - Makes it easy for attackers to retrieve and misuse credentials.

5. Security Misconfiguration
   - When server, application, or database settings are **not configured properly**.

6. Outdated Components
   - When an old library or older version of software is in use which has a **known vulnerability**.

7. Sensitive Data Exposure
   - When confidential info (like passwords, credit cards, etc.) is **not stored securely**.

8. Path Traversal
   - When attackers get access to insights of the **file system**.

---

### 🛠️ Tools for Finding Vulnerabilities

- **Nmap** – To find open ports and running services.  
- **Nikto** – Web vulnerability scanner.
- **Burp Suite** – Web application testing.  
- **OWASP ZAP** – XSS / SQLi detection.
- **Whatweb / Wappalyzer** – Tech stack detection.

---

### 🔹 Finding Hidden Features in Applications

One of the easiest ways to try hacking an application is by **finding hidden features**, e.g., via URLs.

### 🔧 DIRB
 Tool used to find hidden URLs.
1. FULL FORM -- Directory Buster
2. Other Similar Tools--
   - gobuster
   - dirbuster
   - ffuf
3. Used for--
   - Finding hidden admin panels.
   - Finding backup files.
   - Finding sensitive endpoints of web applications.
     
 **Usage:**
```bash
dirb http://example.com
```
---
## ➡️ NOTE
  RED Teams and penetration testers specialize in these offensive techniques.



