# How Traditional SOC Works 

## 1. What is a SOC?

SOC = Security Operations Center  

It is like a **security control room** of a company.

Just like a school has security guards to watch cameras and protect students,  
a company has a SOC team to watch computers, networks, and data.

Their main job:
- Watch for cyber attacks
- Stop attacks quickly
- Protect company data

A SOC works using:
- People (security analysts)
- Processes (rules and steps)
- Technology (security tools)

---

## 2. Main Work of a Traditional SOC

### 1️⃣ Monitoring and Detection

SOC keeps watching the system all the time.

They mostly use a tool called SIEM  
Example:
- Many failed login attempts
- Login from a strange country

If something looks suspicious → alert is created.

---

### 2️⃣ Recovery and Remediation

If attack happens, SOC acts like a **first aid team**.

They can:
- Isolate infected computer
- Remove malware
- Block bad IP address
- Disable user account

They use tools like:
- EDR
- Firewall
- IAM

---

### 3️⃣ Threat Intelligence

SOC checks outside threat information like:
- Bad IP addresses
- Malicious domains
- Malware file hashes

If threat intelligence says an IP is dangerous → SOC blocks it.

---

### 4️⃣ Communication

SOC talks to:
- IT team
- Management
- Other departments

Example:
- Create ticket for IT team
- Inform management about attack

---

# Problems Faced by Traditional SOC
<img width="640" height="676" alt="Screenshot 2026-02-15 at 11 12 52 PM" src="https://github.com/user-attachments/assets/78871751-ede3-4309-8972-2c3b513fc44d" />

## 1️⃣ Alert Fatigue

Too many alerts come every day.  
Many alerts are false.

Analysts become tired and stressed.

---

## 2️⃣ Too Many Separate Tools

SIEM is separate  
EDR is separate  
Firewall is separate  

Analysts keep switching between tools again and again.

This wastes time.

---

## 3️⃣ Manual Work

Most steps are done manually.

Example:
- Check SIEM
- Then check threat intelligence
- Then disable user
- Then open ticket

Very slow process.

---

## 4️⃣ Talent Shortage

Not enough skilled security people.

Work is more, people are less.

---

# What is SOAR?

SOAR = Security Orchestration, Automation, and Response
<img width="808" height="467" alt="Screenshot 2026-02-15 at 11 13 11 PM" src="https://github.com/user-attachments/assets/b0cefcdb-d7da-4818-a6bf-449758399c05" />

It is a tool that connects all security tools in one place.

With SOAR:
- No need to switch between tools
- Everything works inside one dashboard
- It also manages tickets and cases

Think of SOAR like a **smart manager** that controls all tools together.

---

# 3 Main Powers of SOAR

## 1️⃣ Orchestration (Connecting Everything)

SOAR connects:
- SIEM
- EDR
- Firewall
- IAM
- Ticketing system

It creates predefined steps called **Playbooks**.

Playbook = A fixed step-by-step rule for handling alerts.

Example (VPN brute force case):

1. Receive alert from SIEM  
2. Check user login history  
3. Check IP reputation  
4. If IP is bad → take action  

All steps are already defined.

---

## 2️⃣ Automation (Doing Work Automatically)

SOAR can do steps automatically.

Example:

- Gets alert from SIEM
- Checks IP reputation
- Disables user account
- Opens ticket

No manual clicks needed.

This saves time and reduces stress.

---

## 3️⃣ Response (Taking Action)
<img width="860" height="475" alt="Screenshot 2026-02-15 at 11 13 42 PM" src="https://github.com/user-attachments/assets/c12133e0-768c-47e5-b08c-7061a1e5f95a" />

SOAR can:
- Block IP on firewall
- Disable user in IAM
- Isolate endpoint
- Open ticket

All from one place.

---

# Traditional SOC vs SOAR (

| Feature | Traditional SOC | With SOAR |
|----------|----------------|------------|
| Tools | Separate tools | All connected |
| Work style | Manual | Automated |
| Speed | Slow | Fast |
| Alert handling | Stressful | Organized |
| Documentation | Often missing | Structured case management |

---

# Does SOAR Replace SOC Analysts?

No ❌

SOAR helps analysts, but does not replace them.

Why?

- Complex cases need human thinking
- Humans understand business impact
- Humans create playbooks

SOAR removes repetitive work.  
Analysts handle important decisions.

---

# Final Easy Summary

SOC = Security control room  
Problem = Too many alerts + manual work  
Solution = SOAR  

SOAR:
- Connects all tools
- Automates work
- Responds quickly
- Reduces stress

But humans are still needed.

---

# SOAR Playbooks

## What is a Playbook?

Playbook = A step-by-step instruction list inside SOAR.

It tells SOAR:
"If this happens → do this.
If not → do something else."

SOC analysts create playbooks for common alerts like:
- Phishing
- CVEs
- Brute force attacks

---

# 1️⃣ Phishing Playbook 
<img width="882" height="748" alt="Screenshot 2026-02-15 at 11 14 14 PM" src="https://github.com/user-attachments/assets/e8baf32e-11ea-479c-9de3-c929ce6bcda5" />

Phishing = Fake email trying to steal information.

Problem:
- Analysts manually check URLs
- Analyze attachments
- Check threat intelligence
- Very time-consuming

SOAR solves this using automation.

---

## Phishing Playbook Flow 

Start → Alert received: "Suspicious Email"

Step 1: Create a ticket automatically

Step 2: Check email content  
- Does it contain a URL?  
- Does it contain an attachment?

---

### Case 1: No URL and No Attachment

- Notify the user
- Close ticket

End.

---

### Case 2: Contains URL

SOAR will:
- Extract the URL
- Check URL reputation in Threat Intelligence
- Check if URL is malicious

If URL is safe:
- Close ticket

If URL is malicious:
- Block the URL in firewall
- Remove email from all mailboxes
- Inform affected users
- Escalate case to analyst

---

### Case 3: Contains Attachment

SOAR will:
- Extract attachment
- Scan file in sandbox
- Check file hash in Threat Intelligence

If file is clean:
- Close ticket

If file is malicious:
- Quarantine email
- Remove from inboxes
- Isolate affected endpoints
- Escalate to analyst

---

## Important Point

Most steps are automated.

But final decisions (like confirming real attack impact) are checked by a SOC analyst.

---

# 2️⃣ CVE Patching Playbook 
<img width="931" height="315" alt="Screenshot 2026-02-15 at 11 14 48 PM" src="https://github.com/user-attachments/assets/fd89342c-697f-421b-91cb-0bf5e5aabc7d" />

CVE = Publicly known vulnerability with a CVE number.

Example:
CVE-2024-XXXX

Problem:
- New CVEs come frequently
- Analysts must check if systems are affected
- Patching takes time
- Backlog increases

SOAR helps automate this process.

---

## CVE Patching Playbook Flow (Easy Version)

Start → New CVE Published

Step 1: Collect CVE details  
- Severity score  
- Affected software  
- Risk level  

Step 2: Check internal systems  
- Do we use this software?  
- Are we vulnerable?

---

### Case 1: Not Affected

- Close case  
- Document findings  

End.

---

### Case 2: Affected

Step 3: Check severity score

If severity is low:
- Schedule patch later

If severity is high:
- Create urgent patching ticket

Step 4: Test patch in test environment

If patch works:
- Deploy to production systems

If patch fails:
- Escalate to analyst

---

## Why Analyst is Still Needed?

SOAR automates:
- Checking systems
- Creating tickets
- Testing patches

But analyst:
- Confirms risk
- Approves production patch
- Handles special cases

---

# Phishing vs CVE Playbook (Quick Comparison)

| Feature | Phishing Playbook | CVE Patching Playbook |
|----------|------------------|-----------------------|
| Trigger | Suspicious email | New CVE published |
| Main Goal | Stop malicious email | Fix vulnerability |
| Automation | URL & file scanning | Vulnerability checking |
| Analyst Role | Final verification | Patch approval |

---

# Final Easy Summary

Playbook = Automated decision tree inside SOAR.

Phishing Playbook:
- Check email
- Analyze URL/attachment
- Block if malicious

CVE Playbook:
- Check new vulnerability
- See if company is affected
- Patch safely

SOAR reduces manual work  
But SOC analysts still make important decisions.

---


