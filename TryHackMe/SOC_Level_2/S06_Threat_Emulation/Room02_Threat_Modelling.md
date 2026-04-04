# 🔐 Threat Modelling 
## 📌 What is Threat Modelling?

Threat Modelling means:
👉 Thinking like an attacker before the attack happens.

SOC teams use it to:
- Find possible attacks
- Stop attacks early
- Improve detection rules

Simple meaning:
"Find problems before hackers find them"

---

## ⚠️ Threat vs Vulnerability vs Risk

### 🔴 Threat
Something that can cause harm

Examples:
- Hacker
- Malware
- Phishing email

---

### 🟡 Vulnerability
Weakness in the system

Examples:
- Weak password
- Old software
- Open ports

---

### 🟢 Risk
Chance of damage when threat uses vulnerability

Simple formula:
Risk = Threat + Vulnerability

Example:
- Weak password + hacker = Account hacked

---

## 🏠 Easy Example (House)

- Threat = Thief
- Vulnerability = Broken lock
- Risk = Theft

---

## 🔄 Threat Modelling Process (SOC View)

### 1. Define Scope
What system to protect?

Examples:
- Website
- Server
- Database

---

### 2. Identify Assets
Important things to protect

Examples:
- User data
- Passwords
- Payment info

SOC focus:
Important assets = high priority alerts

---

### 3. Identify Threats
What attacks can happen?

Examples:
- Phishing
- SQL Injection
- Brute force attack

---

### 4. Find Vulnerabilities & Risk
Find weak points and decide danger level

SOC work:
- High risk → urgent action
- Low risk → monitor

---

### 5. Apply Security Controls
Stop attacks using tools

Examples:
- Firewall
- MFA (Multi-Factor Authentication)
- SIEM rules
- Antivirus

---

### 6. Monitor & Improve
Check if protection is working

SOC main job:
- Monitor logs
- Investigate alerts
- Respond to incidents

---

## 👥 Teams Involved

### Security Team (SOC)
- Detect attacks
- Respond to incidents

---

### Development Team
- Fix code issues

---

### IT Team
- Manage servers and networks

---

### GRC Team
- Ensure rules and compliance

---

### Business Team
- Decide how much risk is acceptable

---

### End Users
- Can make mistakes (phishing clicks)

---

## 🌳 Attack Tree

Attack Tree = Diagram of how attacker can attack

### Example Goal:
"Steal data"

### Possible ways:
- Phishing
- Exploit bug
- Insider access

SOC use:
- Understand attack steps
- Create detection rules

---

## 🔗 Attack Path

Attack Path = Step-by-step attack journey

Example:
1. User clicks phishing link
2. Password stolen
3. Hacker logs in
4. Moves inside network
5. Steals data

SOC use:
- Track full attack
- Connect multiple alerts

---

## 🚨 Why Threat Modelling is Important for SOC

- Helps predict attacks
- Improves alert accuracy
- Reduces false alerts
- Helps fast response

---

## 💡 Simple Final Line

Threat Modelling helps SOC teams:
👉 "Stop attacks before they become real problems"

---
# 🧠 MITRE ATT&CK Framework 

---

## 📌 What is MITRE ATT&CK?

MITRE ATT&CK is:
👉 A knowledge base of real-world attacker behavior

Simple meaning:
"It shows how attackers perform attacks step by step"

SOC use:
- Understand attacks
- Detect attacks
- Build defense

---

## 🧱 Core Concept

MITRE ATT&CK is divided into:

### 🎯 Tactics (WHY)
Attacker’s goal

Examples:
- Initial Access (enter system)
- Persistence (stay inside)
- Privilege Escalation (gain admin access)
- Exfiltration (steal data)

---

### 🛠️ Techniques (HOW)
Method used to achieve goal

Examples:
- Phishing
- Brute force
- Exploit vulnerability
- Use stolen credentials

---

## 🔥 Easy Difference

| Tactics (Goal) | Techniques (Method) |
|---------------|--------------------|
| Enter system  | Phishing email     |
| Stay inside   | Create backdoor    |
| Steal data    | Download files     |

---

## 📄 MITRE Technique Page (Important)

Each technique page contains:

### 1. Description
What the attack is and how it works

---

### 2. Procedure Examples
Real-world attack usage

---

### 3. Mitigations
How to prevent the attack

---

### 4. Detections (Very Important for SOC)
How to detect the attack

Examples:
- Logs
- Alerts
- Indicators

---

### 5. References
Extra resources

---

## 🔄 Using MITRE in Threat Modelling

After identifying threats, do:

### 👉 Map to MITRE ATT&CK

Steps:
1. Identify threat (example: phishing)
2. Find matching technique in MITRE
3. Study:
   - Description
   - Detection
   - Mitigation

---

## 💻 SOC Practical Use

SOC teams use MITRE to:

- Create SIEM detection rules
- Do threat hunting
- Investigate alerts
- Understand attack chain

---

## 🔗 Attack Path Example

1. Phishing email
2. User gives password
3. Attacker logs in
4. Gains admin access
5. Steals data

SOC:
👉 Monitor each step

---

## 🎯 Use Cases of MITRE ATT&CK

### 1. Identify Attack Paths
Understand how attacker can move inside system

---

### 2. Develop Threat Scenarios
Use real attacker behavior

---

### 3. Prioritize Vulnerabilities
Fix most dangerous issues first

---

## 🚨 Why Important for SOC

- Predict attacker behavior
- Improve detection accuracy
- Reduce false positives
- Faster incident response

---

## 💡 Final Summary

MITRE ATT&CK helps SOC teams to:
👉 "Understand, detect, and stop attacks step by step"
---
# 🧭 ATT&CK Navigator 

---

## 📌 What is ATT&CK Navigator?

ATT&CK Navigator is:
👉 A web-based tool to visualize MITRE ATT&CK

Simple meaning:
"It helps you see, select, and organize attacker techniques"

SOC use:
- Highlight important attacks
- Plan detection
- Understand attack paths

---

## 🧱 Key Idea

MITRE ATT&CK = Knowledge  
ATT&CK Navigator = Visualization tool

---

## 🧾 What is a Layer?

Layer = Working sheet

Used to:
- Select techniques
- Add notes
- Highlight risks

---

## 📊 Types of Layers

### 1. Enterprise ✅ (Most used)
For company systems (servers, cloud, apps)

### 2. Mobile
For phones and tablets

### 3. ICS
For industrial systems (power, water)

---

## 🔍 Searching Techniques

You can search by:
- Technique (phishing, exploit)
- Threat group (APT28, FIN7)
- Software
- Mitigations

---

### Example:
Search: APT28  
👉 All related techniques will be highlighted

---

## ✅ Selecting Techniques

- Click to select techniques
- Multiple selection allowed
- Count shows number of selected items

---

## 🔽 Filtering

Filter techniques based on:

Examples:
- Windows
- Cloud (GCP)
- Office365

SOC use:
👉 Focus only on relevant environment

---

## 🔤 Sorting

- Alphabetical order
- Based on score

Helps in:
👉 Easy viewing and analysis

---

## 🔍 Expand Sub-techniques

- Shows detailed smaller steps
- Gives deeper understanding

---

## 🎨 Annotation (Very Important)

### 🎨 Background Color
Highlight techniques

Example:
- Red = High risk
- Yellow = Medium risk
- Green = Low risk

---

### ⭐ Scoring
Rate importance

Example:
- 5 = Critical
- 1 = Low

---

### 💬 Comment
Add notes

Example:
"This affects our web application"

---

### 🔗 Link
Add external reference

---

### 🏷️ Metadata
Add tags

Example:
- Cloud
- Critical

---

### ❌ Clear Annotation
Remove all notes and highlights

---

## 🔄 Export Feature

Export layer as:
- JSON
- Excel
- SVG

Use:
👉 Save and reuse data

---

## 💻 SOC Practical Workflow

1. Create new layer (Enterprise)
2. Search threat group (APT28, FIN7)
3. Highlight techniques
4. Filter based on environment (cloud/web)
5. Select important techniques
6. Add:
   - Color
   - Score
   - Comments
7. Use data for:
   - Detection rules
   - Security improvements

---

## 🎯 Real Scenario (Bank Example)

### Assets:
- Customer data
- Transactions
- PII

---

### Important Techniques:

1. Exploit Public-Facing Application  
👉 Web app attack

2. Privilege Escalation  
👉 Gain admin access

3. Data from Cloud Storage  
👉 Steal cloud data

4. Network DoS  
👉 Service disruption

---

## 🚨 SOC Action

- Monitor logs for these techniques
- Create SIEM alerts
- Fix vulnerabilities
- Improve security controls

---

## 💡 Why Important for SOC

- Visual understanding of attacks
- Focus on high-risk techniques
- Better detection planning
- Faster response

---

## 🧠 Final Summary

ATT&CK Navigator helps SOC teams to:
👉 "Visualize, prioritize, and defend against attacker techniques"

---
# 🧠 DREAD Framework 

---

## 📌 What is DREAD?

DREAD is:
👉 A risk assessment model used to prioritize vulnerabilities

Simple meaning:
"It helps decide which security problem is most dangerous"

---

## 🎯 Why SOC Uses DREAD?

SOC teams use DREAD to:
- Prioritize vulnerabilities
- Focus on high-risk issues
- Decide what to fix first

---

## 🔤 DREAD Full Form

DREAD has 5 factors:

| Letter | Meaning |
|--------|--------|
| D | Damage |
| R | Reproducibility |
| E | Exploitability |
| A | Affected Users |
| D | Discoverability |

---

## 🧩 DREAD Components (Simple)

### 1️⃣ Damage
How much harm will happen?

Examples:
- Data leak
- System crash
- Reputation loss

---

### 2️⃣ Reproducibility
Can attacker repeat the attack easily?

- Easy → High score
- Difficult → Low score

---

### 3️⃣ Exploitability
How easy is the attack?

- Simple tools → High score
- Complex skills → Low score

---

### 4️⃣ Affected Users
How many users are impacted?

- All users → High score
- Few users → Low score

---

### 5️⃣ Discoverability
How easy to find the vulnerability?

- Publicly known → High score
- Hidden → Low score

---

## ❓ Quick Questions (Easy Way)

- Damage → How bad is it?
- Reproducibility → Can it happen again easily?
- Exploitability → How easy is the attack?
- Affected Users → How many users affected?
- Discoverability → How easy to find?

---

## 🔢 Scoring System

Each factor is rated from:
👉 1 (low) to 10 (high)

---

## 📊 Final Score

Average of all 5 values:

Example:
- D = 10  
- R = 8  
- E = 10  
- A = 10  
- D = 2  

Final Score = (10+8+10+10+2)/5 = 8

👉 Result = High Risk

---

## ⚖️ Risk Levels

| Score | Risk Level |
|------|-----------|
| 8–10 | High 🔴 |
| 5–7  | Medium 🟡 |
| 1–4  | Low 🟢 |

---

## 💻 SOC Practical Use

1. Identify vulnerability
2. Score using DREAD
3. Calculate average
4. Prioritize:
   - High → Fix immediately
   - Medium → Plan fix
   - Low → Monitor

---

## 🎯 Example Cases

### 🔴 Remote Code Execution (RCE)
- Full system access
- Easy exploit  
👉 High Risk

---

### 🟡 IDOR
- Limited data access  
👉 Medium Risk

---

### 🟢 Info Disclosure
- Small data leak  
👉 Low Risk

---

## ⚠️ Important Note

DREAD is:
👉 Subjective (depends on analyst)

To improve accuracy:
- Use clear guidelines
- Discuss with team
- Use examples

---

## 🚨 Why Important for SOC

- Helps prioritize alerts
- Focus on critical threats
- Improve response time
- Better decision making

---

## 💡 Final Summary

DREAD helps SOC teams to:
👉 "Score and prioritize security risks effectively"

---

# 🧠 STRIDE Framework 

---

## 📌 What is STRIDE?

STRIDE is:
👉 A threat modelling framework used to identify different types of attacks

Simple meaning:
"It helps find what kind of attacks are possible in a system"

---

## 🎯 Why SOC Uses STRIDE?

SOC teams use STRIDE to:
- Identify possible threats
- Cover all attack types
- Improve security design

---

## 🔤 STRIDE Full Form

| Letter | Meaning |
|--------|--------|
| S | Spoofing |
| T | Tampering |
| R | Repudiation |
| I | Information Disclosure |
| D | Denial of Service |
| E | Elevation of Privilege |

---

## 🧩 STRIDE Categories (Simple)

### 1️⃣ Spoofing
Fake identity or impersonation

Examples:
- Phishing email
- Stolen credentials login

Breaks:
👉 Authentication

---

### 2️⃣ Tampering
Changing data without permission

Examples:
- Modify database
- Change user password

Breaks:
👉 Integrity

---

### 3️⃣ Repudiation
Denying an action

Examples:
- User denies transaction
- No logs available

Breaks:
👉 Non-repudiation

---

### 4️⃣ Information Disclosure
Sensitive data exposure

Examples:
- Database leak
- Open cloud storage

Breaks:
👉 Confidentiality

---

### 5️⃣ Denial of Service (DoS)
Making system unavailable

Examples:
- Flooding server
- Crashing website

Breaks:
👉 Availability

---

### 6️⃣ Elevation of Privilege
Gaining higher access

Examples:
- User becomes admin
- Exploiting system bug

Breaks:
👉 Authorization

---

## 🔥 Easy Summary Table

| STRIDE | Simple Meaning |
|--------|--------------|
| S | Fake identity |
| T | Change data |
| R | Deny action |
| I | Data leak |
| D | System down |
| E | Become admin |

---

## 🔄 STRIDE in Threat Modelling

### Step 1: Understand System
- Applications
- Servers
- Data flow

---

### Step 2: Apply STRIDE
Ask for each component:
- Can identity be faked?
- Can data be changed?
- Can data leak?

---

### Step 3: Identify Threats
Examples:
- Weak login system
- SQL injection
- Open database

---

### Step 4: Assess Risk
Use DREAD or other methods

---

### Step 5: Apply Security Controls

Examples:
- MFA → prevent spoofing
- Encryption → protect data
- Logging → prevent repudiation
- Firewall → prevent DoS

---

### Step 6: Validate
- Test system
- Perform security checks

---

### Step 7: Continuous Improvement
- Update regularly
- Monitor new threats

---

## 💻 SOC Practical Use

- Map alerts to STRIDE categories
- Improve detection rules
- Perform threat hunting
- Cover all attack types

---

## 🎯 Example (E-commerce System)

| Threat | STRIDE Category |
|--------|----------------|
| Fake login | Spoofing |
| Change payment amount | Tampering |
| Deny transaction | Repudiation |
| Leak customer data | Information Disclosure |
| Website crash | DoS |
| Gain admin access | Elevation of Privilege |

---

## 🚨 Why Important for SOC

- Complete threat coverage
- Better detection planning
- Helps think like attacker
- Improves system security

---

## 💡 Final Summary

STRIDE helps SOC teams to:
👉 "Identify all possible types of security threats in a system"

---
# 🧠 PASTA Framework 

---

## 📌 What is PASTA?

PASTA (Process for Attack Simulation and Threat Analysis) is:
👉 A risk-based threat modelling framework

Simple meaning:
"It is a step-by-step process to find, analyze, and fix security risks"

---

## 🎯 Why SOC Uses PASTA?

SOC teams use PASTA to:
- Understand full attack scenarios
- Identify vulnerabilities
- Prioritize risks
- Improve detection and response

---

## 🧱 Core Idea

PASTA = Complete threat modelling process

It combines:
- Threat identification (like STRIDE)
- Risk analysis (like DREAD)
- Attack simulation

---

## 🔢 7-Step PASTA Process

---

### 1️⃣ Define Objectives
Identify what to protect

Examples:
- Customer data
- Banking system
- Payment platform

---

### 2️⃣ Define Technical Scope
Understand system components

Includes:
- Servers
- Applications
- Databases
- Cloud infrastructure

---

### 3️⃣ Decompose Application
Break system into parts

Examples:
- Login module
- Payment system
- APIs

Focus:
- Entry points
- Trust boundaries
- Data flow

---

### 4️⃣ Analyse Threats
Identify possible attacks

Examples:
- Phishing
- SQL Injection
- DoS

Tools:
- MITRE ATT&CK
- STRIDE

---

### 5️⃣ Vulnerability Analysis
Find weaknesses

Examples:
- Bugs
- Misconfiguration
- Unpatched systems

Methods:
- Vulnerability scanning
- Penetration testing

---

### 6️⃣ Analyse Attacks
Simulate attack scenarios

Examples:
- Login bypass
- Data theft
- Privilege escalation

Tools:
- Attack Trees

---

### 7️⃣ Risk & Impact Analysis
Evaluate and fix risks

Methods:
- Use DREAD scoring
- Prioritize vulnerabilities

Apply controls:
- MFA
- Encryption
- Patching

---

## 🔗 Simple Flow

1. Define what to protect  
2. Understand system  
3. Break system into parts  
4. Identify threats  
5. Find vulnerabilities  
6. Simulate attacks  
7. Fix and reduce risk  

---

## 💻 SOC Practical Use

- Build detection strategies
- Improve incident response
- Perform threat hunting
- Reduce attack surface

---

## 🤝 Teams Involved

- Development Team → Build applications
- Architecture Team → Design systems
- Security Team (SOC) → Detect and defend
- Business Team → Define priorities

---

## 🎯 Example (Online Banking)

### Assets:
- Customer data
- Transactions
- PII

---

### Threats:
- Phishing
- Data theft
- DoS attack

---

### Vulnerabilities:
- Weak authentication
- Misconfigured cloud

---

### Controls:
- MFA
- Encryption
- Monitoring

---

## 🚨 Why Important for SOC

- Complete attack understanding
- Risk-based prioritization
- Better detection planning
- Strong security posture

---

## 💡 Final Summary

PASTA helps SOC teams to:
👉 "Follow a complete process to identify, analyze, and reduce security risks"
