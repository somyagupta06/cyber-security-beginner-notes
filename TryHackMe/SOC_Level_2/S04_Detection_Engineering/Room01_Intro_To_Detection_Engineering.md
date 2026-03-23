# Detection Engineering

## What is Detection Engineering?

Detection Engineering means:

* Creating rules to detect attacks
* Improving those rules over time

In SOC:

* Logs come → rules check them → alerts are generated → analyst investigates

👉 If detection is weak → SOC cannot catch attackers

---

## Types of Detection

There are 2 main categories:

### 1. Environment-Based Detection

Focus: "What is normal in our system?"

* Configuration Detection
* Modelling (Baseline)

---

### 2. Threat-Based Detection

Focus: "What attacker is doing?"

* Indicator Detection
* Behaviour Detection

---

## 1. Configuration Detection

### Idea:

Check if system settings changed from normal

### Examples:

* New admin user created
* Firewall rule changed
* RDP enabled

### SOC Use:

* Easy to create detections
* Good for basic monitoring

### Pros:

* Simple
* Easy to implement

### Cons:

* Many false alerts
* Hard in dynamic environments

---

## 2. Modelling (Baseline Detection)

### Idea:

Define normal behavior → detect anything unusual

### Examples:

* User logs in daily at 9 AM
* Suddenly logs in at 3 AM → suspicious

### SOC Use:

* Detect unknown attacks
* Useful for insider threats

### Pros:

* Can detect new attacks

### Cons:

* No clear context
* Needs good understanding of system
* Can include attacker behavior in baseline

---

## 3. Indicator Detection (IOC)

### Idea:

Detect known bad things

### Examples:

* Malicious IP address
* Malware file hash
* Suspicious domain

### SOC Use:

* Quick detection using threat intelligence

### Pros:

* Fast and easy

### Cons:

* Attacker can change easily
* Works only for known threats

---

## 4. Threat Behaviour Detection (Most Important)

### Idea:

Detect what attacker is doing (behavior)

### Examples:

* Word → PowerShell (suspicious)
* User becomes admin suddenly

### SOC Use:

* Strong detection
* Used in real SOC environments

### Pros:

* Hard for attacker to bypass
* Low false positives

### Cons:

* Hard to build
* Needs more data

---

## Comparison Table

| Type          | Focus            | Strength               | Weakness          |
| ------------- | ---------------- | ---------------------- | ----------------- |
| Configuration | System changes   | Easy                   | Many false alerts |
| Modelling     | Behavior change  | Detect unknown attacks | No context        |
| Indicator     | Known threats    | Fast                   | Easily bypassed   |
| Behaviour     | Attacker actions | Strong                 | Hard to build     |

---

## Detection as Code (DaC)

### Idea:

Write detection rules like software code

---

## Why needed?

Normal problem:

* No tracking of rule changes
* No testing
* No standard process

---

## With Detection as Code:

### 1. Version Control

* Track changes in rules
* Example: using Git

---

### 2. Automation

* Automatically test detections
* Automatically deploy rules

---

### 3. Reusable Rules

* Write once → use many times

---

### 4. Team Work

* SOC team works together easily

---

## Tools Used:

* Sigma → write detection rules
* YARA → detect malware
* Git → store and manage rules

---

## SOC Workflow (Simple)

1. Collect logs (Windows, Network, etc.)
2. Send logs to SIEM
3. Apply detection rules
4. Generate alerts
5. Analyst investigates

---

## Final Key Points (Important for Revision)

* Detection Engineering = building + improving detections
* Logs are required → no logs = no detection
* Best detection = behaviour-based
* Indicators are useful but weak
* Detection is continuous process
* Detection as Code makes detections better and scalable

---
# Detection Gap Analysis

---

## What is Detection Gap Analysis?

Detection Gap Analysis means:

* Finding what attacks we **cannot detect**
* Improving detection where it is missing

👉 Simple:
"What attacker can do" vs "What we can detect"

---

## Step 1: Threat Modelling

### Goal:

Find possible attack areas

---

### Two Ways:

#### 1. Reactive Approach

* Learn from past attacks
* Check what was missed

Example:

* Attack happened
* No alert for privilege escalation → gap found

---

#### 2. Proactive Approach

* Think before attack happens
* Use frameworks like:

  * MITRE ATT&CK

---

Example:

* ATT&CK says attacker can use PowerShell
* Check: Do we detect PowerShell misuse?

If NO → detection gap

---

## Step 2: Data Source Identification

### Goal:

Find what data (logs) we have

---

### Important Rule:

👉 No logs = No detection

---

### Common Data Sources:

* Windows Event Logs
* Sysmon
* Firewall logs
* DNS logs
* EDR logs

---

### SOC Task:

* Check available logs
* Identify missing logs

---

## Step 3: Baseline Creation

### Goal:

Define normal behavior

---

### Example:

* Normal login: 9 AM
* Suspicious login: 3 AM

---

### Types of Baseline:

#### 1. High-Level Baseline

* General rules
* Example:

  * Only admins can install software

---

#### 2. Technical Baseline

* System-level rules

Examples:

* OS hardening settings
* Network activity rules
* IAM (user access rules)

---

### SOC Importance:

* Helps reduce false alerts
* Helps detect anomalies

---

## Step 4: Log Collection

### Goal:

Collect all important logs

---

### Tools:

* Sysmon → host logs
* Splunk → log analysis
* ELK Stack → log storage

---

### Flow:

1. Logs generated
2. Sent to SIEM
3. Stored and analysed

---

### Problems:

* Logs missing
* Logging disabled
* Too much noise

---

## Step 5: Rule Writing

### Goal:

Create detection rules

---

### Types of Rules:

* Network → Snort
* File → YARA
* Logs → Sigma

---

### Example (Sigma Rule Idea):

* Detect suspicious PowerShell
* Detect admin group changes

---

### SOC Focus:

* Detect abnormal behavior
* Reduce false positives

---

## Step 6: Deployment, Automation & Tuning

### Goal:

Use detections in real environment

---

### Process:

1. Deploy rule in SIEM
2. Monitor alerts
3. Tune detection

---

### Example:

Problem:

* Too many alerts

Solution:

* Add filters
* Exclude trusted users

---

### Automation:

* Auto alert handling
* Playbooks for response

---

## Important Concept

👉 Detection is NOT one-time work

It is:

* Continuous
* Always improving

---

## Full Detection Lifecycle

1. Threat modelling
2. Data source check
3. Baseline creation
4. Log collection
5. Rule writing
6. Deployment & tuning

---

## Final Key Points (Revision)

* Detection gaps = missing detections
* Use ATT&CK for proactive detection
* Logs are critical
* Baseline helps reduce noise
* Detection rules must be tested
* Continuous improvement is required

---
# MITRE ATT&CK, CAR, Pyramid of Pain & Kill Chain 

---

## 1. MITRE ATT&CK Framework

### What is it?

* A knowledge base of attacker behavior
* Shows how attackers perform attacks

Use:
👉 Helps SOC analysts understand **what attacker does**

---

### Structure:

* **Tactic** → attacker goal
* **Technique** → how attacker does it

---

### Example:

* Tactic: Execution
* Technique: PowerShell

---

### SOC Use:

* Map alerts to attacker behavior
* Use in detection gap analysis
* Improve detection coverage

---

### Key Point:

👉 ATT&CK answers:
**"What techniques can attacker use?"**

---

## 2. CAR (Cyber Analytics Repository)

### What is it?

* A collection of detection ideas
* Based on ATT&CK techniques

---

### Simple Idea:

* ATT&CK → what attacker does
* CAR → how to detect it

---

### Example:

ATT&CK:

* PowerShell abuse

CAR:

* Detect encoded commands
* Detect unusual execution

---

### SOC Use:

* Helps write detection rules
* Saves time in detection design

---

### Key Point:

👉 CAR answers:
**"How can we detect this attack?"**

---

## 3. Pyramid of Pain

### What is it?

* Shows how difficult it is for attacker to change something

---

### Levels (Bottom → Top):

| Level     | Example             | Difficulty for Attacker |
| --------- | ------------------- | ----------------------- |
| Hash      | File hash           | Easy                    |
| IP        | IP address          | Easy                    |
| Domain    | Domain name         | Medium                  |
| Artefacts | File path, registry | Hard                    |
| Tools     | Malware tools       | Hard                    |
| TTPs      | Behaviour           | Very Hard               |

---

### SOC Meaning:

* Lower levels → weak detection
* Higher levels → strong detection

---

### Example:

* Detect IP → attacker changes IP easily
* Detect behaviour → attacker struggles

---

### Key Point:

👉 Best detection = **Behaviour (TTPs)**

---

## 4. Cyber Kill Chain

### What is it?

* Shows steps of an attack

---

### 7 Phases:

1. Reconnaissance → attacker collects info
2. Weaponisation → prepares malware
3. Delivery → sends attack (email, link)
4. Exploitation → uses vulnerability
5. Installation → installs malware
6. Command & Control → connects to attacker
7. Actions on Objectives → data theft / damage

---

### SOC Use:

* Detect attacks at each stage
* Plan detection strategy

---

### Example:

* Delivery → detect phishing
* Execution → detect PowerShell
* C2 → detect suspicious traffic

---

### Key Point:

👉 Kill Chain answers:
**"Where in the attack are we detecting?"**

---

## 5. Unified Kill Chain

### What is it?

* Advanced version of Kill Chain
* Combines ATT&CK + Kill Chain

---

### Features:

* More detailed (18 phases)
* Covers full attack lifecycle

---

### SOC Use:

* Better detection coverage
* Used in advanced environments

---

## Final Comparison

| Framework       | Purpose                     |
| --------------- | --------------------------- |
| ATT&CK          | What attacker does          |
| CAR             | How to detect it            |
| Pyramid of Pain | What is strongest detection |
| Kill Chain      | When attack happens         |

---

## Final SOC Understanding

* Use ATT&CK → identify attack techniques
* Use CAR → build detection
* Use Pyramid → focus on strong detection
* Use Kill Chain → cover all attack stages

---

## Quick Revision Points

* ATT&CK = techniques
* CAR = detection ideas
* Pyramid = detection strength
* Kill Chain = attack stages
* Behaviour detection is strongest

---
# ADS Framework + DML Model 

---

# 1. Alerting and Detection Strategy (ADS Framework)

## What is ADS?

* A structured way to write detection rules
* Helps reduce alert fatigue (too many useless alerts)

👉 Simple:
"Do not write random detections — follow a proper structure"

---

## ADS Stages (Easy Explanation)

---

### 1. Goal

👉 What do you want to detect?

Example:

* Detect changes in admin groups

---

### 2. Categorisation

👉 Map detection to:
MITRE ATT&CK

Example:

* Privilege Escalation → Account Manipulation

---

### 3. Strategy Abstract

👉 High-level detection logic

Example:

* Monitor group membership changes
* Alert when admin group modified

---

### 4. Technical Context

👉 Environment details

Example:

* Active Directory
* Windows Security Logs
* Domain Controller

---

### 5. Blind Spots & Assumptions

👉 When detection may fail

Examples:

* Logs not enabled
* Logs not sent to SIEM
* DC compromised

---

### 6. False Positives

👉 Legit cases that trigger alerts

Examples:

* IT admin adds user
* Scheduled admin changes

---

### 7. Validation

👉 Test your detection

Example:

* Add user to admin group
* Check if alert triggers

---

### 8. Priority

👉 Alert severity

Example:

* Admin access → High

---

### 9. Response

👉 What analyst should do

Example:

* Check who made change
* Verify user
* Remove access if needed

---

# 2. AD Scenario (IMPORTANT 🔥)

## Problem:

Detect changes in:

* Privileged groups
* Admin accounts

---

## Full ADS Example (Ready to Use)

---

### Goal

Detect when a user is added or removed from privileged groups in Active Directory.

---

### Categorisation

Privilege Escalation → Account Manipulation

---

### Strategy Abstract

* Monitor Windows Security Events
* Detect group membership changes
* Alert on admin group modifications

---

### Technical Context

* Active Directory environment
* Domain Controllers
* Windows Event Logs (Security)

Important Event IDs:

* 4728 → user added to group
* 4732 → user added to local admin group
* 4729 / 4733 → user removed

---

### Blind Spots & Assumptions

* Logging must be enabled
* Logs must reach SIEM
* No tampering on domain controller

---

### False Positives

* Legit admin activity
* IT maintenance changes

---

### Validation

* Add test user to admin group
* Confirm alert is generated

Example:
Add-ADGroupMember -Identity "Administrators" -Members "testuser"

---

### Priority

High

---

### Response

* Identify who added the user
* Check source system
* Verify if authorized
* Remove unauthorized access
* Investigate further activity

---

# 3. Detection Maturity Level (DML Model)

## What is DML?

* Measures how advanced your detection is

👉 Simple:
"How smart is your detection?"

---

## Levels (Easy Version)

| Level | Meaning                   |
| ----- | ------------------------- |
| DML-0 | No detection              |
| DML-1 | IP, domain (basic)        |
| DML-2 | Logs, artefacts           |
| DML-3 | Tools (e.g., Mimikatz)    |
| DML-4 | Attack sequence           |
| DML-5 | Techniques (ATT&CK level) |
| DML-6 | Tactics                   |
| DML-7 | Strategy                  |
| DML-8 | Goals                     |

---

## SOC Reality

* Most companies → DML 1–2
* Good SOC → DML 3–4
* Advanced SOC → DML 5+

---

## Example (Same Attack)

Attack: Credential Dumping

| Level | Detection                    |
| ----- | ---------------------------- |
| DML-1 | Known IP                     |
| DML-2 | LSASS access                 |
| DML-3 | Mimikatz detected            |
| DML-5 | Credential dumping behaviour |

---

👉 Best detection = DML-5 (behaviour)

---

# 4. Final Key Points (Revision)

* ADS = how to write detection properly
* DML = how strong your detection is
* Always test detections (validation)
* Admin access detection = HIGH priority
* Behaviour detection is strongest

---

# Final Understanding

👉 Good Detection =
ADS structure + High DML level

---
