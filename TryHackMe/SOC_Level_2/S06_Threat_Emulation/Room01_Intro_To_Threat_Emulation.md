# 🛡️ Threat Emulation 

## 🔴🔵 Red Team vs Blue Team

* **Red Team** = acts like attacker (tries to break into system)
* **Blue Team** = defender (detects and stops attacks)

👉 Problem:

* They don’t always share information
* Security gaps stay hidden

👉 Solution:

* Work together → called **Threat Emulation / Purple Teaming**

---

## 🎯 What is Threat Emulation?

👉 Simple meaning:

Threat emulation = acting like a real hacker in a safe environment to test security

* No real damage
* But attack feels real
* Helps SOC understand weaknesses

---

## 💡 Why do we use Threat Emulation?

It helps SOC to:

* Check if detection is working
* Test response speed
* Find hidden gaps

👉 It also breaks false beliefs like:

* “We installed all patches → we are safe”
* “We have firewall → nothing can happen”

➡️ Reality: attacks can still happen

---

## ⚠️ Types of Security Testing

| Type                     | What it does        | Limitation                   |
| ------------------------ | ------------------- | ---------------------------- |
| Vulnerability Assessment | Finds weaknesses    | Does NOT exploit             |
| Penetration Testing      | Exploits weaknesses | Limited scope                |
| Red Teaming              | Simulates attacker  | Not always full attack cycle |

👉 Problem:

* Mostly stops at initial access
* Not full real-world attack

---

## 🚀 Threat Emulation vs Simulation

| Feature                     | Threat Emulation | Simulation |
| --------------------------- | ---------------- | ---------- |
| Based on real attacker      | ✅ Yes            | ❌ No       |
| Uses real techniques (TTPs) | ✅ Yes            | ⚠️ Generic |
| Feels realistic             | ✅ High           | ❌ Medium   |

👉 Example:

* Emulation = attack like a real APT group
* Simulation = run pre-made attack scripts

---

## 🧠 Key Concepts (Important for SOC)

### 1. Real-world Based

* Uses real attack data (MITRE ATT&CK)
* Based on real breaches

---

### 2. Behaviour-focused

* Detect actions, not just signatures

👉 Example:

* Detect unusual PowerShell usage

---

### 3. Transparency

* Red & Blue teams share everything

---

### 4. Collaboration

* Work together (Purple Teaming)

---

### 5. Repeatable

* Same attack can be tested again
* Helps improve detection over time

---

## 🎯 SOC Use Cases

### 1. Assessment

* Check logs, alerts, detection

### 2. Improvement

* Fix gaps in tools and rules

### 3. Training

* SOC learns real attack patterns

---

## 🧩 Simple Example (SOC View)

1. Red Team sends phishing email
2. User clicks link
3. Malware runs
4. Attacker moves inside network

👉 SOC (Blue Team):

* Did alert trigger?
* Was attack detected?
* How fast was response?

➡️ If failed → improve rules

---

## 📌 Final Quick Revision

* Threat Emulation = real-like attack testing
* Helps SOC improve detection & response
* Better than pentest because:

  * Covers full attack cycle
  * Uses real attacker behavior
  * Encourages team collaboration

---

✔️ Goal: Be ready before real attacker comes
---
# 🛡️ Threat Emulation Methodologies 

## 🎯 What are Threat Emulation Methodologies?

Threat emulation methodologies = structured steps and plans used to simulate real attacks

👉 Goal:

* Find security weaknesses
* Test detection and response
* Improve SOC performance

---

## 🧠 Key Idea

* Every attacker is different
* But attackers follow patterns (steps)

➡️ These methodologies help us copy those patterns safely

---

# 🔥 1. MITRE ATT&CK Framework

## 📌 What is it?

A knowledge base of real attacker techniques (TTPs)

👉 Simple:
“List of what hackers do in real attacks”

---

## 🧩 How it works

* Divided into **tactics** (goals of attacker)
* Each tactic has **techniques** (how attacker does it)

---

## 🧠 Example Flow

* Reconnaissance → gather info
* Initial Access → enter system
* Lateral Movement → move inside network
* Exfiltration → steal data

---

## 🎯 SOC Use

* Map alerts to attacker behavior
* Improve detection rules
* Help in reporting

---

# 🧪 2. Atomic Testing (Atomic Red Team)

## 📌 What is it?

Small, simple tests to check security controls

👉 Not full attack, only one action at a time

---

## 🧠 Examples

* Test PowerShell misuse detection
* Test credential dumping detection

---

## 🎯 SOC Use

* Quick testing
* Easy to identify gaps
* Helps tune alerts

---

# 🇪🇺 3. TIBER-EU Framework

## 📌 What is it?

A structured framework for full attack simulation

---

## 🔄 3 Phases

### 1. Preparation Phase

* Define scope
* Select teams
* Get approvals

---

### 2. Testing Phase

* Red Team performs attack
* Blue Team detects and responds

---

### 3. Closure Phase

* Create reports
* Identify weaknesses
* Suggest improvements

---

## 🎯 SOC Use

* Test full attack lifecycle
* Evaluate real detection capability
* Improve response process

---

# 📚 4. CTID Adversary Emulation Library

## 📌 What is it?

A collection of ready-made attack plans

---

## 🔥 Types

### 1. Full Emulation

* Complete attack (start to end)
* Example: phishing → access → data theft

---

### 2. Micro Emulation

* Small specific actions
* Example: process injection, file access

---

## 🎯 SOC Use

* Saves time
* Real-world scenarios
* Improves detection coverage

---

# 📊 Quick Comparison

| Method         | Simple Meaning   | SOC Benefit              |
| -------------- | ---------------- | ------------------------ |
| MITRE ATT&CK   | Hacker playbook  | Better detection mapping |
| Atomic Testing | Small tests      | Quick gap finding        |
| TIBER-EU       | Full attack plan | End-to-end testing       |
| CTID Library   | Ready scenarios  | Faster emulation         |

---

# 🧠 Final Revision Points

* Methodologies = structured attack simulation methods
* Based on real attacker behavior
* Help SOC detect and respond better
* Can be combined for better results

---

✔️ Goal: Practice attacks before real attackers come
✔️ Focus: Detection, response, improvement
--
# 🛡️ Adversary Emulation Process 

## 🎯 Scenario Overview

* Company: VASEPY Corp (Retail)
* Risk: Credit card fraud + ransomware
* Goal: Test security using real attacker behavior

👉 Role: Threat Emulation Engineer
➡️ Plan and execute realistic attack simulation

---

# 🔄 Emulation Process (5 Steps)

1. Define Objectives
2. Research Adversary
3. Plan Engagement
4. Conduct Emulation
5. Report Results

👉 This process is **iterative** (repeat and improve)

---

# 🎯 1. Define Objectives

## 📌 What is this?

Decide what you want to test

---

## 🧠 Example (VASEPY)

* Test protection of credit card data
* Test ransomware defence

---

## 🎯 SOC Focus

* Are alerts generated?
* Is sensitive data protected?
* Is detection working properly?

---

## ✔️ Key Points

* Objectives must be clear
* Must be measurable
* Scope must be defined

---

# 🔍 2. Research Adversary

## 📌 What is this?

Understand which attacker is relevant

---

## 🧩 2.1 Information Gathering

Collect data from:

* Threat intelligence reports
* Past incidents
* Internal teams:

  * SOC
  * CTI team
  * System admins

---

## 🧠 VASEPY Insight

* Retail company → attackers want money
* Target = POS systems, credit card data

---

## 🧠 Shortlisted Attackers

* FIN6
* FIN7
* FIN8

---

# 🎯 2.2 Select Adversary

## 📌 Choose one attacker

---

## 🧠 Selection Factors

| Factor     | Meaning                      |
| ---------- | ---------------------------- |
| Relevance  | Matches business target      |
| CTI Data   | Enough information available |
| Complexity | Can we simulate it?          |
| Resources  | Time, tools, team available  |

---

👉 Selected: **FIN7**
(reason: targets retail companies)

---

# ⚙️ 2.3 Select TTPs

## 📌 What is this?

Choose attacker techniques to simulate

---

## 🧠 FIN7 TTPs

* Spear phishing
* Social engineering
* Watering hole attack

---

## 🎯 SOC Focus

* Can we detect phishing?
* Do users fall for attack?
* Are alerts triggered?

---

# 🧩 2.4 Construct TTP Outline

## 📌 What is this?

Create detailed attack plan

---

## 📋 Includes

* Attack steps
* Scope
* Rules of engagement
* Tools used

---

## 🧠 Example Flow

1. Send phishing email
2. User clicks link
3. Malware executes
4. Attacker gains access

---

# ⚠️ Important Note

* Attacker behavior changes over time
* Always update TTPs using latest threat intelligence

---

# 🚀 3. Planning the Engagement

## 📌 What happens here?

* Finalize attack plan
* Prepare tools and environment
* Assign roles

---

# ⚔️ 4. Conducting Emulation

## 📌 Execution Phase

* Red Team performs attack
* Blue Team monitors and detects

---

## 🎯 SOC Focus

* Alert generation
* Detection accuracy
* Response time

---

# 📊 5. Concluding & Reporting

## 📌 Final Step

* Document findings
* Identify gaps
* Suggest improvements

---

## 📋 Report Includes

* What attack was done
* What was detected / missed
* Recommendations

---

# 🧠 Final Quick Revision

* Emulation = real-world attack testing
* Process = 5 steps (objective → report)
* Based on real attacker (FIN7 example)
* Focus = detection + response improvement

---

✔️ Goal: Prepare SOC before real attack happens
✔️ Outcome: Better security, faster response
---
# 🛡️ Threat Emulation (Planning & Execution) – SOC Notes (Easy English)

---

# 🎯 3. Planning the Threat Emulation Engagement

## 📌 What is this?

Planning = preparing everything before running the attack

👉 Why important?

* Prevent data loss
* Avoid system crash
* Keep business safe

---

## 🧠 Key Idea

Attack is **real-like**, so planning must be **safe and controlled**

---

# 📑 3.1 Threat Emulation Plan

## 📌 What is it?

A step-by-step document for executing the attack

👉 Simple:
“Full attack script written before execution”

---

## 📋 Components of Plan

### 🎯 1. Objectives

* What you want to test
* Example: ransomware detection

---

### 📦 2. Scope

* Which systems, users, departments are included

---

### ⏰ 3. Schedule

* When attack will happen
* Duration of activity

---

### ⚖️ 4. Rules of Engagement

* What is allowed / not allowed
* Keep attack safe

---

### ✅ 5. Permission to Execute

* Official approval required
* Prevents legal issues

---

### 📞 6. Communication Plan

* How teams will communicate
* Who reports to whom
* Emergency contact plan

---

# ⚔️ 4. Conducting the Emulation

## 📌 What is this?

Executing the planned attack using selected TTPs

---

## 🧠 Key Points

* Must be controlled
* Must be safe
* Requires skilled team

---

# 🧪 Emulation Lab Setup

## 📌 Required Components

### 1. Attack Platform

* System used to launch attack

---

### 2. Test Systems

* Target machines where attack runs

---

### 3. Analysis Platform

* Collect logs and analyse results

---

# ⚙️ 4.1 Planning the Deployment

## 📌 What is this?

Preparing how attack will be executed

---

## 🧠 Example (FIN7)

* Use phishing email
* Send malicious file (RTF/DOCX)

---

## 🎯 SOC Focus

* Can phishing be detected?
* Are users protected?

---

# 💻 4.2 Implementation of TTP

## 📌 What is this?

Actual execution of attack techniques

---

## 🧠 Example Flow

1. Send phishing email
2. User opens file
3. Malware executes
4. Attacker gains access

---

## ✔️ Key Idea

* Follow attacker behavior
* Execute safely

---

# 🛡️ 4.3 Detection & Mitigation

## 📌 What is this?

Blue Team detects and stops attack

---

## 🎯 SOC Tasks

* Monitor logs
* Generate alerts
* Investigate activity

---

## 🧠 Example

* Suspicious file detected
* Alert triggered
* SOC responds

---

## 🔧 Mitigation Actions

* Block malicious file
* Isolate infected system
* Remove malware

---

## 📌 MITRE Role

* Provides detection techniques
* Suggests mitigation strategies

---

# 🧠 Final Quick Revision

* Planning ensures safe attack execution
* Threat Emulation Plan = full attack blueprint
* Execution = controlled attack simulation
* SOC role = detect, analyse, respond

---

✔️ Goal: Test before real attack happens
✔️ Focus: Detection + Response + Improvement
---
# 🛡️ Threat Emulation (Observation & Reporting) 

---

# 👀 5. Observe Results

## 📌 What is this?

Observe results = check what happened after the attack

---

## 🧠 Who does this?

* Blue Team / SOC Team

---

## 🎯 SOC Tasks

* Analyse logs
* Check system events
* Monitor network traffic

---

## 🧩 Example Flow

1. Phishing email sent
2. User opened file
3. Malware executed

👉 SOC checks:

* Was alert generated?
* Was activity detected?
* Is there evidence in logs?

---

## 🔍 Detection Tools

* SIEM tools
* Detection rules
* Signatures

---

## 🧠 YARA Rules (Important)

👉 What is YARA?

Rule-based system to detect malware

---

## 📌 Example

* Detect malicious file (pillowMint.exe)
* Match file patterns or behavior

---

## 🎯 Outcome of Observation

| Result     | Meaning                     |
| ---------- | --------------------------- |
| Successful | Attack worked, not detected |
| Detected   | SOC identified attack       |
| Blocked    | Attack stopped              |

---

# 🧾 6. Document & Report Findings

## 📌 What is this?

Write and report everything observed during emulation

---

## 🧠 Why important?

* Shows security effectiveness
* Helps management understand risk
* Supports improvement

---

## 📋 Report Includes

### 1. Execution Details

* What attack was performed
* How it was executed

---

### 2. Results

* Detected or missed
* Successful or blocked

---

### 3. Impact

* Effect on system
* Data risk level

---

### 4. Recommendations

* Improve detection rules
* Enhance monitoring
* Provide user training

---

## 🧩 Example

* Phishing attack succeeded
* User clicked link
* Detection was delayed

➡️ Recommendation:

* Improve email security
* Train users

---

# 🧠 Final Quick Revision

* Observe = SOC checks detection and logs
* Report = document findings and improvements
* Use tools like SIEM and YARA
* Focus = detection, response, improvement

---

✔️ Goal: Learn from test and improve security
✔️ Outcome: Stronger SOC and faster detection
