# 🛡️ Containment & Threat Intelligence 
## 🔹 What is Containment?

Containment means:
👉 Stop the attack from spreading
👉 Reduce damage
👉 Keep evidence safe

In SOC:
When an alert comes, we do NOT panic.
We first try to **control the situation**.

---

## 🔹 Why Containment is Important?

* Stops attacker from spreading in network
* Protects other systems
* Helps in investigation (forensics)
* Helps to restore normal work faster

---

## 🔹 What SOC Does in Containment Phase?

1. Detect alert (from SIEM like Wazuh)
2. Investigate the system
3. Collect evidence
4. Create IOCs
5. Decide containment strategy

---

## 🔹 What is Threat Intelligence?

Threat Intelligence means:
👉 Information about attacker

Example:

* IP address
* File hash
* Domain name
* Attacker behavior

SOC uses this to:

* Understand attacker
* Stop future attacks
* Link with old attacks

---

## 🔹 IOC (Indicator of Compromise)

IOC = Proof that system is infected

Examples:

* File hash (like SHA256)
* Malicious IP
* Suspicious domain

👉 If IOC is found → system is compromised

---

## 🔹 IOA (Indicator of Attack)

IOA = Suspicious behavior

Examples:

* Multiple failed logins
* Strange file execution
* Unusual network activity

👉 IOA shows attack is happening

---

## 🔹 How SOC Collects Evidence?

From tools like:

* IDS (Intrusion Detection System)
* SIEM (like ELK Stack)

Example:

* A file "dropper.exe" downloaded
* SOC takes its hash
* Now any system with this hash = infected

---

## 🔹 Containment Strategies

### 🟥 1. Full Isolation

Meaning:
👉 Completely disconnect system

How:

* Remove network access
* Block device

Pros:

* Stops attack quickly
* Safe

Cons:

* Attacker gets alert
* May rush to cause damage

---

### 🟨 2. Controlled Isolation

Meaning:
👉 Monitor attacker without alerting them

How:

* Keep system running
* Watch attacker activity
* Block only dangerous actions

Pros:

* Learn attacker behavior
* Collect more intelligence

Cons:

* Risky
* Needs continuous monitoring

---

## 🔹 How SOC Chooses Strategy?

SOC asks:

* How dangerous is attack?
* Do we understand attacker?
* Do we have resources to monitor?

Decision:

| Situation              | Action               |
| ---------------------- | -------------------- |
| High risk (ransomware) | Full Isolation       |
| Low/unknown attack     | Controlled Isolation |

---

## 🔹 Simple SOC Flow

Alert → Investigate → Collect IOC → Use Threat Intelligence
→ Choose Strategy → Contain → Recover

---

## 🔹 Key Points to Remember

* Containment = Damage control
* Threat Intelligence = Understanding attacker
* IOC = Evidence
* IOA = Behavior
* Isolation = Fast but visible
* Controlled Isolation = Slow but smart

---

## ✅ Final Simple Example

* User downloads malicious file
* SOC gets alert
* SOC finds file hash
* Checks other systems
* Finds infection

Now:

* If attack is dangerous → isolate system
* If not sure → monitor attacker

---

👉 SOC goal:
**Stop attack + Understand attacker + Protect network**

---

# 🛡️ Threat Intelligence & TTPs 

## 🔹 What is Threat Intelligence?

Threat Intelligence = Information about attacker

Examples:

* IP Address
* File Hash
* Domain
* File Name
* Attack behavior

---

## 🔹 Why SOC Uses Threat Intelligence?

* Understand attacker
* Predict next steps
* Prevent future attacks
* Improve security alerts

---

## 🔹 Types of Threat Intelligence

| Type       | Meaning           |
| ---------- | ----------------- |
| IP Address | Attacker location |
| File Hash  | Malware identity  |
| Domain     | Malicious website |
| File Name  | Tool used         |
| TTPs       | Attack behavior   |

---

## 🔹 TTPs (Very Important)

TTP = Tactics + Techniques + Procedures

---

### 🔸 Tactics (WHY)

* Goal of attacker

Examples:

* Data theft
* System damage
* Financial gain

---

### 🔸 Techniques (HOW)

* Method used in attack

Examples:

* Phishing
* Malware
* Brute force

---

### 🔸 Procedures (FLOW)

* Step-by-step attack chain

Example:
Phishing → Credential Theft → Login → Lateral Movement → Data Exfiltration

---

## 🔹 Threat Intelligence Platforms

* OpenCTI
* Public threat feeds

Use:

* Share attack data
* Compare with alerts
* Identify known attackers

---

## 🔹 SOC Workflow Using Threat Intelligence

Alert → Investigate → Collect IOC
→ Check Threat Intelligence → Identify attacker
→ Predict next step → Apply containment

---

## 🔹 Important Concepts

### Balance

* Fast containment = less intelligence
* Slow containment = more intelligence but risky

---

### Whack-a-Mole Problem

Fixing one system is not enough
Attacker may still exist in other systems

---

### Continuous Learning

* Improve detection rules
* Reduce SIEM noise
* Learn from past incidents

---

## 🔹 Key Points

* Threat Intelligence = attacker knowledge
* TTP = attacker behavior
* Helps in prediction
* Improves containment strategy
* Ongoing process

---

## ✅ Final Goal of SOC

Understand attacker → Predict behavior → Stop attack efficiently
