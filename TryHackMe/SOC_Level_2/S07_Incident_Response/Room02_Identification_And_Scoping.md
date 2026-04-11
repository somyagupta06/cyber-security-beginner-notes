# 🛡️ Identification Phase 

## 🔹 What is Identification?

Identification means:
Finding out that something suspicious or dangerous is happening in the system.

In SOC:
We detect alerts and decide if it is a real security problem or not.

---

## 🔹 Simple Example

A user tries to login 100 times and fails.

SOC Analyst thinks:
- Is this normal?
- Or is someone trying to hack?

👉 This is Identification.

---

## 🔹 What does a SOC Analyst do?

1. Look at alerts
2. Check logs
3. Understand what is happening
4. Decide:
   - Safe (ignore)
   - Dangerous (investigate more)

---

## 🔹 Security Alerts (Very Important)

Alerts are warning messages.

Example:
- Multiple failed logins
- Malware detected
- Unusual login time

👉 Alerts help SOC start investigation.

---

## 🔹 Three Important Things (Triad)

### 1. People
Humans (SOC analysts)
- Understand alerts
- Make decisions

### 2. Process
Rules to follow

Example:
- If brute force → check logs → block IP

### 3. Technology
Tools used in SOC

Examples:
- SIEM (collects logs and gives alerts)
- EDR (checks activity on computers)
- IDS (detects network attacks)

---

## 🔹 SOC Workflow (Step by Step)

### Step 1: Alert comes
System gives warning

### Step 2: Triage
Check:
- Real problem? (True Positive)
- Or mistake? (False Positive)

### Step 3: Investigation
- Check logs
- Check user activity
- Check system behavior

### Step 4: Escalation
If serious → send to higher team

---

## 🔹 True vs False Positive

True Positive:
Real attack

Example:
Hacker trying password many times

False Positive:
Normal activity looks suspicious

Example:
User forgot password

---

## 🔹 Why Speed is Important?

Fast detection = less damage

Slow detection = more damage

---

## 🔹 Role of Employees

Not only SOC team.

Everyone should:
- Report strange activity
- Report suspicious emails

---

## 🔹 Communication

SOC must:
- Inform correct team
- Give clear information

Example:
Malware found → inform IT team

---

## 🔹 After Identification → Next Step

Next step is Scoping.

Questions:
- Which systems are affected?
- How big is the attack?
- What data is at risk?

---

## 🔹 Final Summary (Quick Revision)

- Identification = Detect problem
- Alerts = Starting point
- SOC checks if alert is real
- Follow steps: Alert → Check → Investigate → Escalate
- Work with People + Process + Technology
- Fast response is very important

---

# 🛡️ Scoping Phase

## 🔹 What is Scoping?

Scoping means:
Finding how big the security incident is.

After Identification (problem found),
Scoping tells:
- How many systems are affected
- What data is at risk
- How serious the incident is

---

## 🔹 Simple Example

Malware found on one laptop.

Questions:
- Only one system infected?
- Or many systems?
- Any sensitive data involved?

👉 Answering these = Scoping

---

## 🔹 What SOC Analyst Does in Scoping?

1. Identify affected systems
2. Check data at risk
3. Understand impact

---

## 🔹 Asset Inventory (Very Important)

Asset Inventory = List of all company systems

Includes:
- System name
- IP address
- Operating system
- Owner

### Example Use

Alert:
IP 172.16.1.153 suspicious

Check Asset Inventory:
→ It is a laptop
→ Owner is Derick

👉 Now investigation becomes easy

---

## 🔹 Why Asset Inventory is Useful?

- Quick identification of systems
- Saves time
- Reduces confusion

---

## 🔹 Spreadsheet of Doom (SoD)

SoD = List of known malicious indicators

Also called:
Indicators of Compromise (IoCs)

---

## 🔹 What is inside SoD?

- Malicious IP addresses
- Fake domains
- Suspicious email IDs
- Malware file hashes

---

## 🔹 Example

Alert:
System contacted IP 188.40.75.132

Check SoD:
→ IP is marked as malware hosting

👉 This confirms suspicious activity

---

## 🔹 Why SoD is Important?

- Fast threat detection
- Gives context (phishing, malware, etc.)
- Helps track repeated attacks

---

## 🔹 Asset Inventory + SoD (Together)

SOC uses both together:

1. Alert comes
2. Check SoD → Is it malicious?
3. Check Asset Inventory → Which system?
4. Decide scope → How big is the issue?

---

## 🔹 Why Scoping is Important?

- Helps plan response
- Tells severity of incident

### Example

Small:
1 laptop affected

Big:
Server + multiple systems affected

---

## 🔹 After Scoping

Next step:
Containment and Response

---

## 🔹 Final Quick Revision

- Scoping = Measure the size of incident
- Asset Inventory = System details list
- SoD = Known threat list (IoCs)
- Use both to understand impact
- Helps in fast and correct response

---

# 🔄  Identification + Scoping Feedback Loop

## 🔹 Key Idea

Identification and Scoping are NOT a straight line.

They work in a loop:
Identification ↔ Scoping

This means:
We keep updating our understanding as new information comes.

---

## 🔹 Simple Example

Step 1:
Alert shows 1 system infected

Step 2:
After checking logs → 3 more systems found

👉 Scope changes → investigation updates

---

## 🔹 What is Intelligence-Driven?

Using smart data to improve investigation.

Includes:
- Past incidents
- Logs from systems
- Threat intelligence (known bad IPs, domains)
- Analytics / ML tools

👉 Helps in better and faster decisions

---

## 🔹 Feedback Loop Steps

### 1. Event Notification
Alert or issue is reported

Example:
- Suspicious email
- Malware alert

---

### 2. Documentation
Write down details:
- What happened
- Which system
- Time of incident

👉 Important for tracking and teamwork

---

### 3. Evidence Collection
Collect proof:
- Logs
- Network traffic
- Files

👉 Helps understand what really happened

---

### 4. Artefact Identification
Find important clues from evidence

Examples:
- Malicious IP
- Fake domain
- Suspicious email

👉 These are called IoCs (Indicators of Compromise)

---

### 5. Pivot Point Discovery
Find new leads for investigation

Example:
- One bad IP found
- Same IP seen on other systems

👉 New systems become part of scope

---

### 6. Loop Back
- Update documentation
- Collect more evidence
- Expand scope

👉 Process repeats (loop)

---

## 🔹 Why This Loop is Important?

- Continuous learning
- Better understanding of attack
- Faster response
- More accurate results

---

## 🔹 SOC Mindset

- Investigation is never fixed
- Scope can change anytime
- Always follow new clues

---

## 🔹 Extra Benefits

- Improves security response
- Helps in legal compliance
- Better communication between teams

---

## 🔹 Final Quick Revision

- Not linear → it is a loop
- New data → new scope
- Steps:
  Alert → Document → Collect Evidence → Find Clues → Discover New Leads → Repeat
- Use intelligence (logs + past data) for better decisions
