# 📌 SOC Notes – Improving Detection using IOCs & Sigma Rules

## 🔹 1. Problem in SOC 

Many SOC teams use **default detection rules**.

But problem is:

* Too many alerts ❌
* Not useful alerts ❌
* Real threats may be missed ❌

👉 So, default rules are **not enough**.

---

## 🔹 2. What is the Solution?

We use **IOCs (Indicators of Compromise)**

👉 IOC = Evidence that attacker was present

### Examples:

* Malicious domain → bad3xe69connection.io
* Suspicious file → malware.exe
* IP address → attacker server

---

## 🔹 3. Why IOCs are Important?

* They come from **real past attacks**
* Help detect **same attacker again**
* Improve SOC detection accuracy

👉 Simple idea:
"If attacker came once, they can come again"

---

## 🔹 4. IOC Spreadsheet (Very Useful)

SOC teams store IOCs in a file (spreadsheet)

### Why?

* Easy sharing between analysts
* Helps in investigation
* One IOC can lead to more IOCs

👉 Example:

* Found: bad3xe69connection.io
* Then discovered:

  * kind4bad.com
  * nic3connection.io

👉 All become IOCs

---

## 🔹 5. What is Sigma?

Sigma = A **standard rule format** for detection

👉 It helps to:

* Write detection rules easily
* Share rules across tools
* Work in any SIEM (Splunk, ELK, etc.)

---

## 🔹 6. Example Sigma Rule (Simple)

```
title: Executable Download from Suspicious Domains
status: test
description: Detect download of exe files from bad domains

logsource:
  category: proxy

detection:
  selection:
    c-uri-extension:
      - 'exe'
    r-dns:
      - 'bad3xe69connection.io'
      - 'kind4bad.com'
      - 'nic3connection.io'
  condition: selection

fields:
  - c-uri

falsepositives:
  - Unknown

level: medium
```

---

## 🔹 7. What This Rule Means (Very Simple)

👉 Alert if:

* File type = .exe
  AND
* Domain = suspicious

👉 This means:
"User downloaded a dangerous file from a bad website"

---

## 🔹 8. SOC Mindset (Most Important)

Attackers must:

* Bypass ALL security layers

But SOC needs:

* Just ONE detection to catch them ✅

👉 More rules = more protection layers

---

## 🔹 9. Key Takeaways (Revision)

* Default rules are not enough ❌
* Use IOCs from past incidents ✅
* Store IOCs in spreadsheets ✅
* Convert IOCs into Sigma rules ✅
* Add multiple detection layers ✅

---

## 🔹 10. Final Simple Line

"Good SOC does not depend on default rules, it creates its own detection using real attack data."

---
# 📌 SOC Notes – Using Public IOCs & Sigma for 0-Day Attacks

---

## 🔹 1. New Problem (0-Day Vulnerability)

👉 A **0-day vulnerability** means:

* A new attack
* No patch available yet
* Organization is already at risk ❌

👉 Example:

* Follina (MSDT exploit)
* Log4j vulnerability

---

## 🔹 2. Important SOC Lesson

👉 You don’t need to face every attack yourself

You can learn from:

* Security researchers
* Community findings
* Public reports

👉 These provide:
➡️ Public IOCs (Indicators of Compromise)

---

## 🔹 3. What to Do with Public IOCs?

SOC should:

1. Collect public IOCs
2. Convert them into detection rules
3. Deploy them in SIEM

👉 This helps detect:
"New attacks before they hit you"

---

## 🔹 4. Role of Sigma (Again Important)

Sigma helps to:

* Convert IOCs into rules
* Keep rules tool-independent
* Share rules easily

👉 Then convert Sigma → SIEM queries

---

## 🔹 5. What is Uncoder Tool?

Uncoder = Sigma → SIEM converter

👉 It helps to convert Sigma rules into:

* Splunk queries
* Elastic queries

👉 Think of it like:
"Translator between Sigma and SIEM"

---

## 🔹 6. Example 1 – Follina Detection

👉 What it detects:

* Suspicious use of **msdt.exe** (Microsoft tool)

👉 Logic:

* msdt.exe is running
* Command contains suspicious keywords
* Means possible exploit

👉 Simple meaning:
"Office file tried to execute malicious command"

---

## 🔹 7. Example 2 – Log4j Detection

👉 What it detects:

* Java process starts suspicious tools

👉 Example tools:

* powershell
* cmd
* wmic
* rundll32

👉 Logic:
If Java → launches shell → suspicious

👉 Simple meaning:
"Java application behaving like attacker"

---

## 🔹 8. Detection Logic (Very Easy Table)

| Attack Type | Behavior                   |
| ----------- | -------------------------- |
| Follina     | msdt.exe + strange command |
| Log4j       | java.exe → starts shell    |

---

## 🔹 9. Important Warning ⚠️

👉 Sigma → SIEM conversion is NOT perfect

Problems:

* May not work directly ❌
* Needs testing ❌
* Needs tuning ❌

👉 Why?
Every organization has:

* Different logs
* Different environment

---

## 🔹 10. SOC Best Practice

Never do this ❌

* Directly copy rule and deploy

Always do this ✅

* Test rule
* Tune false positives
* Adjust for environment

---

## 🔹 11. Final SOC Mindset

* Use **default rules** ➝ basic layer
* Add **custom rules (IOCs)** ➝ stronger layer
* Add **public threat intel** ➝ advanced layer

👉 More layers = better detection 🔥

---

## 🔹 12. Final One-Line Revision

"Smart SOC learns from global attacks and converts them into local detection."
---
# 📌 SOC Notes – Tripwires & Purple Teaming

---

## 🔹 1. Key Idea 

👉 Use **special / sensitive data** to create alerts

Example:

* Secret file
* Confidential folder
* Important database

👉 If someone accesses it → Alert 🚨

---

## 🔹 2. What is a Tripwire?

👉 Tripwire = A trap for attackers

It alerts when:

* File is opened
* File is changed
* File is deleted

👉 Simple meaning:
"If someone touches this file → something is wrong"

---

## 🔹 3. Types of Tripwires

### ✅ 1. Honeypot

* Fake system or file
* No real use
* Any activity = Suspicious

👉 Example:
Fake admin panel

---

### ✅ 2. Hidden Files / Folders

* Not visible to normal users
* Used to catch:

  * malware
  * worms
  * attackers

👉 Example:
Hidden folder with fake data

---

## 🔹 4. Why Tripwires are Powerful?

* Detect unknown attacks
* Catch attackers early
* No dependency on signatures

👉 Important:
Even if attacker is new → tripwire still works ✅

---

## 🔹 5. Basic Tripwire Setup (Windows)

### Step 1: Enable Logging

* Open Local Security Policy
* Go to:
  Security Settings → Local Policies → Audit Policy

👉 Enable:

* Audit Object Access

  * Success ✅
  * Failure ✅

---

### Step 2: Create File

* Create file: "Secret Document"

---

### Step 3: Enable Auditing

* Right click file → Properties
* Security → Advanced → Auditing
* Add → Select "Everyone"

👉 Select actions:

* Read
* Write
* Delete

---

## 🔹 6. What Happens After Setup?

👉 Every access is logged

Important Event:

* Event ID = 4663

👉 Means:
"Someone accessed the file"

---

## 🔹 7. SOC Use Case

SOC can:

* Monitor Event ID 4663
* Create alert if triggered

👉 Example alert:
"Unauthorized access to Secret Document"

---

## 🔹 8. Important Tip

👉 If many files:

* Put them in one folder
* Monitor folder instead

---

## 🔹 9. What is Purple Teaming?

👉 Combination of:

* Red Team (Attack)
* Blue Team (Defense)

👉 Goal:
Test detection using real attacks

---

## 🔹 10. How Purple Team Works?

1. Simulate attack
2. Check logs
3. See what was detected
4. Improve detection

---

## 🔹 11. Key Questions (Very Important)

After simulation ask:

* What attack techniques were used?
* What did we detect?
* What did we miss?

👉 Missed = Improve detection

---

## 🔹 12. Real Examples

### 🔸 Tempest Room

* Full attack simulation
* Shows logs step-by-step

👉 Goal:
Understand attacker behavior

---

### 🔸 Follina Room

* Focus on vulnerability effects
* Analyze logs + processes

👉 Combine:

* Logs
* IOCs

---

## 🔹 13. Final SOC Learning

* Use tripwires for unknown threats
* Use logs for visibility
* Use purple teaming for improvement

---

## 🔹 14. Final One-Line Revision

"Good SOC not only detects attacks but also tests itself to improve detection."

