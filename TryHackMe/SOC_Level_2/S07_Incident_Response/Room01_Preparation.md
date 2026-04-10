# 🛡️ Incident Response 

## 📌 What is Incident Response?
Incident Response means:
Handling cyber attacks in a structured way to reduce damage and recover fast.

SOC team does this work.

---

## ⚡ Event vs Incident

| Term      | Meaning (Simple)                          | Example                      |
|----------|------------------------------------------|------------------------------|
| Event     | Normal activity in system                | User login, email sent       |
| Incident  | Dangerous activity (security problem)    | Hacking, data theft          |

👉 Important:
Not every event is an incident.

---

## 🔄 Incident Response Process (6 Steps)

### 1. Preparation
Be ready before attack happens.

- Setup tools (SIEM, EDR)
- Create rules
- Train team
- Make plans (IRP)

👉 Example:
If login attack happens, system should already detect it.

---

### 2. Identification
Find if something is wrong.

- Check alerts
- Analyze logs
- Decide: real attack or not

👉 Example:
Many failed logins → suspicious → possible attack

---

### 3. Analysis (Scoping)
Understand how big the attack is.

- Which system is affected?
- What data is at risk?
- How attack started?

👉 Goal:
Know full damage

---

### 4. Containment
Stop the attack from spreading.

- Disconnect infected system
- Block attacker IP
- Disable account

👉 Goal:
Control the situation quickly

---

### 5. Eradication
Remove the attack completely.

- Delete malware
- Fix vulnerabilities
- Remove backdoors

👉 Important:
Fix root cause, not just symptoms

---

### 6. Recovery + Lessons Learned

Recovery:
- Restore systems
- Resume work

Lessons:
- What went wrong?
- Improve security
- Update rules

---

## 📄 Incident Response Plan (IRP)

IRP = A document that tells:

- What to do in attack
- Who will do what
- How to communicate

👉 Think of it like:
Emergency guide for cyber attacks

---

## 📘 Playbooks

Playbooks = Step-by-step instructions for specific attacks

Example: Phishing Playbook

1. Check email
2. Analyze link
3. Scan system
4. Block domain

👉 Benefit:
Fast and clear response

---

## 🧠 SOC Analyst Role

SOC analyst should:

- Monitor alerts
- Investigate properly
- Respond quickly
- Write reports

---

## 💡 Simple Real Example

1. Alert: suspicious login
2. Analyst checks logs
3. Confirms attack
4. Blocks user/IP
5. Fixes issue
6. Writes report

👉 This is full incident response cycle

---

## 🔥 Quick Revision

- Event = normal activity
- Incident = security problem
- 6 steps = Prepare → Identify → Analyze → Contain → Eradicate → Recover
- IRP = plan
- Playbook = actions
---
# 🛡️ Preparation Phase 

## 📌 What is Preparation?
Preparation means:
Being ready before a cyber attack happens.

Goal:
- Reduce damage
- Respond fast
- Recover quickly

---

## 👥 Preparing the People

### 🔴 Why People are Important?
Humans are the easiest target for attackers.

Common attacks:
- Phishing emails
- Social engineering

👉 One mistake by user = system compromised

---

### 👨‍💻 CSIRT Team

CSIRT = Cyber Security Incident Response Team

### Team Members:
- SOC analysts (technical)
- Legal team
- Business team
- PR team

👉 Purpose:
Handle cyber attacks properly from all sides

---

### 🔐 Access Control

- CSIRT gets special access (logs, systems)
- Access must be controlled
- Admin should be notified when used

👉 Reason:
Maintain security and accountability

---

### 🎓 Training & Awareness

Train:
- Employees
- SOC team
- Customers

### Training Topics:
- How to detect phishing
- How to report incidents
- Basic security awareness

👉 Example:
Fake phishing email test (simulation)

---

### 🧪 SOC Skills Needed

Incident handlers should know:
- Log analysis
- Forensics tools
- Malware basics
- Honeypots

👉 Goal:
Detect and investigate attacks properly

---

## 📄 Preparing Documentation

### 📌 Why Documentation is Important?

- Acts as evidence
- Helps in future incidents
- Improves security

### Must Have Skills:
- Note-taking
- Attention to detail

---

## 📜 Policies

Policies = Security rules

### Examples:
- No unauthorized access
- Systems are monitored
- Limited privacy

👉 Important:
- Visible to employees
- Approved by legal team

---

## 📞 Communication Plan

Define:
- Who reports incident
- Who responds
- Who informs management/law enforcement

👉 Purpose:
Avoid confusion during attack

---

## 🔗 Chain of Custody

Meaning:
Track who handled evidence and when

### Example:
- Who collected data
- Who analyzed it
- When it was transferred

👉 Important for:
Legal proof and investigation

---

## ⚙️ Response Procedures

Predefined steps to handle incidents

### Examples:
- Malware → isolate system
- Phishing → block domain

👉 Goal:
Fast and organized response

---

## 🧠 Final Summary

Preparation includes:
- People (trained team)
- Policies (rules)
- Documentation (records)
- Communication (clear plan)
- Procedures (ready actions)

👉 Key Idea:# 🛡️ Preparation Phase

## 📌 Big Idea
People and policies are not enough.

SOC also needs:
- Tools
- Systems
- Infrastructure visibility

👉 Goal:
Detect, prevent, and investigate attacks properly

---

## 🖥️ Asset Inventory

### 📌 What is Asset Inventory?
List of all systems and resources in the organisation.

### Examples:
- Servers (Mail, Web, VPN)
- Software applications
- Employee data
- Customer data

---

### 📊 Example Table

| Asset Type | Name      | OS                  | IP Address   |
|------------|----------|---------------------|--------------|
| Mail Server| MailSrvr1| Windows Server 2019 | 192.168.0.2  |
| Web Server | WebSrvr1 | Ubuntu Server 20.04 | 192.168.0.3  |

---

## 🎯 Asset Classification

Not all assets are equal.

### High Value Assets:
- Customer data
- Critical servers
- Intellectual property

👉 Purpose:
- Protect important assets first
- Set priority during incidents

---

## 🌐 Technical Instrumentation

### 📌 Meaning:
Understanding and mapping the full network.

Includes:
- Devices (routers, switches)
- Servers
- Cloud systems
- Applications

👉 Goal:
Full visibility of infrastructure

---

## 🛠️ Security Tools (SOC Tools)

- Anti-malware → detects viruses
- EDR → monitors endpoints
- DLP → prevents data leaks
- IDPS → detects/prevents attacks
- Log collection → records activity

👉 Without logs = no detection

---

## 🌐 Network Segmentation (Subnetting)

### 📌 Meaning:
Divide network into smaller parts

### Example:
- Office network
- Server network
- Guest network

👉 Purpose:
- Limit attack spread
- Better control and security

---

## 📊 Monitoring & Tracking Tools

Example:
- TheHive (incident tracking system)

👉 Used for:
- Case management
- Alert tracking

---

## 🔍 Investigation Capabilities

### 📌 Meaning:
Ability to investigate attacks properly

SOC should be able to:
- Run scripts
- Analyze systems
- Reproduce attacks

---

## 🧪 Forensics

### Types:
- Disk imaging → copy of hard disk
- Memory imaging → capture RAM data

👉 Purpose:
Collect evidence safely

---

## 🔐 Secure Evidence Storage

- Evidence stored securely
- Only CSIRT can access

👉 Important for legal and analysis

---

## 🧠 Sandbox

Safe environment to test suspicious files

👉 Example:
Run malware safely to observe behavior

---

## 🎒 Jump Bag (Incident Response Kit)

### 📌 Meaning:
Ready-to-use toolkit for incident response

### Contents:

#### 📀 Storage:
- USB drives (store evidence)

#### 💻 Forensic Tools:
- FTK Imager
- EnCase
- Sleuth Kit

#### 🌐 Network Tools:
- Network tap (monitor traffic)

#### 🔌 Hardware:
- USB, SATA cables
- Card readers

#### 🛠️ Repair Tools:
- Screwdrivers
- Tweezers

#### 📄 Documents:
- Incident forms
- Playbooks

---

## 🧠 Final Summary

Preparation (Technology Side) includes:

- Asset inventory (know your systems)
- Asset classification (set priority)
- Network visibility (full mapping)
- Security tools (detect attacks)
- Investigation tools (analyze attacks)
- Jump bag (ready toolkit)

👉 Key Idea:
Know your infrastructure and be ready to investigate

Be ready before attack happens
---
# 👁️ Visibility & Logging 

## 📌 What is Visibility?

Visibility means:
Knowing what is happening inside your systems and network.

👉 No visibility = no detection

---

## 🔍 What does Visibility include?

### 1. Logs (Most important)
- Record of system and user activity

### 2. Threat Intelligence
- Information about new attacks and hacker techniques (TTPs)

### 3. Patch Updates
- Software updates to fix vulnerabilities

---

## 🧠 Types of Visibility

### 🔹 Internal Visibility
- Logs from systems
- User activity
- Network events

### 🔹 External Visibility
- New threats in the cyber world
- Latest attack techniques

---

## 🎯 Benefits of Visibility

- Know who did what and when
- Provides evidence (logs)
- Improves security processes
- Helps in compliance
- Detects new threats
- Ensures systems are updated

---

## 📜 Logs (Core of Visibility)

Logs = records of activities in systems

### Examples:
- Login attempts
- File access
- Application activity

---

## 🧠 SIEM (Log Management)

SIEM = Security Information and Event Management

### Functions:
- Collect logs from all systems
- Store logs centrally
- Analyze logs
- Generate alerts

👉 Without SIEM = logs are scattered

---

## 📊 Types of Logs

### 1. Event Logs
- Normal system activities
- Example: login, network traffic

### 2. Audit Logs
- Who did what
- Example: login success/failure

### 3. Error Logs
- System problems
- Example: service crash

### 4. Debug Logs
- Used for testing and troubleshooting

---

## 🌐 Log Sources

### 1. Network Logs
- Routers, switches

### 2. Firewall / VPN Logs
- Allowed and blocked traffic

### 3. System Logs
- OS activities

### 4. Application Logs
- Web apps, databases

---

## ⚙️ Setting up Visibility

CSIRT must ensure:
- Logging is enabled
- Logs are collected properly
- Logs are secured (no tampering)

---

## 🪟 Windows Logging Example

### Problem:
- Event logging was disabled

👉 Result:
No visibility

---

### Solution:
- Check Event Viewer
- Modify registry value:
  - 4 → Disabled
  - 2 → Enabled (auto start)
- Restart system

👉 Result:
Logging enabled

---

## 🧪 Testing Logs

- Run attack simulation (Atomic Red Team)
- Check logs in Event Viewer

👉 If logs appear → system is working

---

## 🔥 Key Concept

Without logs:
- No detection ❌
- No investigation ❌
- No evidence ❌

With logs:
- Detection ✔️
- Investigation ✔️
- Response ✔️

---

## 🧠 Final Summary

Visibility includes:
- Logs (activity tracking)
- Threat intelligence (external awareness)
- Patch updates (security fixes)

👉 Key Idea:
You must see everything happening in your system
