# Incident Response (IR) 

## 1. Real Life Example (Home Security)
- Imagine your house is in a dangerous street.
- You buy:
  - Security guard (external defense)
  - CCTV cameras (monitoring)
  - Hidden underground room for valuables (backup plan)
- Even after all this, what if a thief breaks in?
  - You still need a **response plan**:
    - How to catch thief
    - How to save valuables
    - How to reduce losses

👉 Same concept in Digital World = **Incident Response (IR)**



## 2. What is Incident Response?
- **Definition:**  
  A process to **detect, respond, and recover** from cyber security incidents.
- Goal: **Minimize damage + recover fast**


## 3. Why is Incident Response Important?
| Without IR | With IR |
|------------|---------|
| Panic when attack happens | Clear step-by-step plan |
| Data loss, money loss | Loss is minimized |
| No one knows what to do | Team knows responsibilities |
| Reputation damage | Faster recovery, trust maintained |



## 4. Phases of Incident Response (Lifecycle)
Think of it like a **step-by-step journey**:

1. **Preparation**
   - Like installing CCTV, guards, alarms in home
   - In digital: Install antivirus, firewalls, monitoring tools

2. **Identification**
   - Detecting if something suspicious happened  
   - Example: You see thief on CCTV OR system detects unusual login

3. **Containment**
   - Stop attacker from spreading  
   - Example: Lock the thief in one room  
   - Digital: Disconnect infected computer from network

4. **Eradication**
   - Remove the root cause  
   - Example: Catch thief, remove broken lock  
   - Digital: Delete malware, close vulnerability

5. **Recovery**
   - Restore normal operations  
   - Example: Replace broken door, secure home again  
   - Digital: Restore systems from backup, bring network online

6. **Lessons Learned**
   - Analyze: What went wrong? How to improve?  
   - Example: Next time, put stronger locks, better alarm system  
   - Digital: Update policies, improve monitoring



## 5. Key Terms
- **Incident:** Any event that affects confidentiality, integrity, or availability of data.  
- **Incident Response Team (IRT):** Group of people trained to handle incidents.  
- **Playbook:** Step-by-step guide for responding to a specific type of attack (like "what to do if malware attack happens").  



## 6. Short Analogy Table (Home vs Cyber)

| Home Security | Cyber Security |
|---------------|----------------|
| Security guard | Firewall |
| CCTV cameras | Monitoring tools (SIEM, IDS/IPS) |
| Hidden room | Backup & Disaster recovery |
| Thief enters | Hacker attacks |
| Lock thief in one room | Contain infected system |
| Catch thief & fix lock | Remove malware & patch vulnerability |
| Stronger locks next time | Update policies & security patches |



## 7. Summary 
- Incident Response = **"Plan before + Act during + Learn after"**
- 3 Golden Goals:
  1. Detect early
  2. Reduce impact
  3. Recover quickly
---

# Events, Alerts, and Incident Severity 

## 1. Processes and Events
- On your device (Laptop, Mobile, etc.) many **processes** run:
  - **Interactive processes** → Need your action  
    Example: Playing games, Watching videos
  - **Non-Interactive processes** → Run in background  
    Example: Antivirus update, System services

👉 Both types generate **events** (logs of their actions).



## 2. Why Events are Important?
- Devices generate **millions of events** daily.  
- Some events are normal, but some may point to **suspicious activity**.  
- Manually checking = Impossible ❌  
- So, we use **Security Solutions (like SIEM tools)**:
  - Collect events as logs
  - Detect possible harmful activity
  - Trigger **alerts**



## 3. What is an Alert?
- **Alert = Signal from security solution** saying:  
  “Hey! Something unusual happened. Please check.”  

- Not every alert is dangerous.  
- Alerts can be:
  - **False Positive** (Looks dangerous but actually safe)  
  - **True Positive** (Looks dangerous and is actually harmful)  



## 4. Examples
### False Positive
- Security solution: "Large data transfer to external IP detected!"
- Investigation: Found it was **backup to cloud storage** (safe).  
- ✅ Conclusion: False Positive (Not harmful).

### True Positive
- Security solution: "Phishing email detected for one user!"
- Investigation: Found it was **real phishing attempt**.  
- 🚨 Conclusion: True Positive (Harmful).

👉 **True Positives are treated as Incidents.**



## 5. Incident Severity
- When an alert becomes an **incident**, we must decide **priority**.  
- Not all incidents are equal → Some are more dangerous.  

| Severity | Meaning | Example | Priority |
|----------|---------|---------|----------|
| Low | Minor issue, no big risk | A single failed login attempt | Lowest |
| Medium | Some impact but not critical | User clicked a suspicious link but blocked | Medium |
| High | Serious issue, can cause damage | Malware detected on one machine | High |
| Critical | Very dangerous, business at risk | Ransomware spreading in network | Highest |

👉 Rule: Always respond to **Critical first**, then **High**, then **Medium**, and finally **Low**.



## 6. Flow Summary
1. **Processes run** → generate **events**  
2. **Security solution analyzes events** → creates **alerts**  
3. Alerts are checked → False Positive ❌ or True Positive ✅  
4. **True Positive = Incident**  
5. Assign **Severity** → Low / Medium / High / Critical  
6. Respond according to priority
---

# Types of Security Incidents 

## 1. Quick Intro
- Many people call every cyber attack a **hacking attempt**.
- In reality → **Security Incidents have different types**.
- Each type has its **own way of harming** the victim.

👉 Example:  
Earlier we saw **phishing email with malicious attachment** → this is **1 type of incident**.  
But there are many more…



## 2. Main Types of Security Incidents

### (a) Malware Infections
- **What it is:** Malicious software/program installed on system/network/app.  
- **Causes:** Malicious files (documents, text, executables, etc.).  
- **Impact:** Can steal data, damage files, slow system, allow hacker control.  

**Example:**  
- Ransomware encrypting all files.  
- Trojan hiding inside a "free game" app.  



### (b) Security Breaches
- **What it is:** Unauthorized access to confidential data.  
- **Why serious?** Businesses rely on secret data (financial, customer info).  
- **Impact:** Privacy loss, trust issues, legal penalties.  

**Example:**  
- Hacker breaks into company database and steals customer records.  



### (c) Data Leaks
- **What it is:** Confidential information exposed to outsiders.  
- **Difference from Breach:**  
  - Breach = attacker **breaks in**.  
  - Leak = data **flows out** (by mistake or intentional).  
- **Causes:**  
  - Human error (sending file to wrong email).  
  - Misconfiguration (public database not secured).  
- **Impact:** Reputational loss, blackmail, financial damage.  

**Example:**  
- Employee accidentally uploads sensitive report to public server.  

### (d) Insider Attacks
- **What it is:** Attack launched by someone **inside the organization**.  
- **Why dangerous?** Insiders already have access → less effort needed.  
- **Impact:** System-wide infections, stolen secrets, sabotage.  

**Example:**  
- Disgruntled employee plugs USB with malware before quitting.  



### (e) Denial of Service (DoS) Attacks
- **What it is:** Attacker floods system/network with fake requests → real users cannot access.  
- **Pillar of Security Affected:** **Availability**  
- **Impact:** Services go offline, business loss, customer frustration.  

**Example:**  
- E-commerce website attacked with millions of fake login requests → site crashes during sale.  



## 3. Comparison Table

| Incident Type     | Description | Example | Key Risk |
|-------------------|-------------|---------|----------|
| Malware Infection | Malicious software infecting system | Ransomware encrypting files | Data loss, system damage |
| Security Breach   | Unauthorized access to confidential data | Hacker stealing customer DB | Privacy & financial loss |
| Data Leak         | Confidential info accidentally/ intentionally exposed | Employee misconfigures database | Reputation damage |
| Insider Attack    | Attack from within organization | Angry employee spreading malware | High access misuse |
| DoS Attack        | Flooding system with fake requests | Website down due to attack | Service unavailability |



## 4. Severity Depends on Victim
- Same incident ≠ same impact for everyone.  
- Example:  
  - **Company A** → not affected much if their internal report leaks (data useless to outsiders).  
  - **Company B** → huge loss if their main website suffers DoS attack (business fully depends on site).  

👉 Severity = **Relative to business impact**



## 5. Easy Analogy 
- **Malware Infection** = Virus infection in body → spreads if untreated  
- **Security Breach** = Thief enters locked room  
- **Data Leak** = Accidentally leaving private diary on park bench  
- **Insider Attack** = Family member stealing from inside home  
- **DoS Attack** = Crowd blocking shop entrance so customers can’t go inside

---

# Incident Response Frameworks 

## 1. Why Frameworks are Needed?
- Different types of incidents = different handling difficulties.
- A **structured process** is needed to respond effectively.
- **Incident Response Frameworks** provide generic steps → can be applied to any incident.

👉 Two most popular frameworks:
- **SANS** (offers security training & certifications)
- **NIST** (develops standards & guidelines)



## 2. SANS Incident Response Framework (PICERL)

SANS has **6 phases** → easy to remember as **PICERL**.

| Phase          | Explanation | Example |
|----------------|-------------|---------|
| **P – Preparation** | Build resources, teams, and tools before an incident | Employee training on phishing emails |
| **I – Identification** | Detect abnormal activity that may indicate incident | Detect unusual data transfer from host |
| **C – Containment** | Minimize attack impact (stop spreading) | Isolate infected host from network |
| **E – Eradication** | Remove threat completely | Run deep malware scan, delete malicious files |
| **R – Recovery** | Restore affected systems, test, and bring back online | Rebuild host, restore data from backup |
| **L – Lessons Learned** | Analyze incident, document gaps, and improve process | Post-incident review meeting |



## 3. NIST Incident Response Framework
NIST framework is **simpler** with **4 phases**:

1. **Preparation**  
   - Same as SANS: Build team, policies, tools.
2. **Detection & Analysis**  
   - Identify and verify incidents.
3. **Containment, Eradication & Recovery**  
   - Combine SANS’s 3 steps into one phase.
4. **Post-Incident Activity**  
   - Similar to Lessons Learned.



## 4. Comparison Table – SANS vs NIST

| Aspect             | SANS (6 Phases - PICERL) | NIST (4 Phases) |
|--------------------|---------------------------|-----------------|
| **Preparation**    | Yes | Yes |
| **Identification** | Separate phase | Included in Detection & Analysis |
| **Containment**    | Separate phase | Combined |
| **Eradication**    | Separate phase | Combined |
| **Recovery**       | Separate phase | Combined |
| **Lessons Learned**| Yes | Post-Incident Activity |
| **Detail Level**   | More detailed (step-by-step) | More simplified |

👉 Both are **very similar**, just the number of phases is different.



## 5. Incident Response Plan (IRP)
- Organizations create a **formal document** → approved by senior management.
- Defines what to do **before, during, after** an incident.

### Key Components of IRP:
1. **Roles & Responsibilities**  
   - Who will do what during incident.
2. **Incident Response Methodology**  
   - Steps to follow (can be based on SANS/NIST).
3. **Communication Plan**  
   - How to report incident internally & externally.  
   - Includes communication with **stakeholders, law enforcement**.
4. **Escalation Path**  
   - Define **which incidents go to higher authority** (e.g., Critical → report to CISO & legal team).


## 6. Easy Analogy (Fire Drill Example)
- **Preparation** → Fire extinguisher, trained staff, exit plan.  
- **Identification** → Detect smoke/fire.  
- **Containment** → Close doors, stop fire spreading.  
- **Eradication** → Extinguish fire completely.  
- **Recovery** → Repair damaged building.  
- **Lessons Learned** → Meeting after fire to improve safety.

👉 Same process applied to cyber incidents = Incident Response Framework.

---
# Incident Detection Tools & Playbooks 

## 1. Why Tools Are Needed?
- Identification/Detection of incidents (SANS → Identification, NIST → Detection & Analysis) is **hard to do manually**.
- Many security solutions help in **detecting + sometimes responding** to incidents.



## 2. Common Security Solutions

| Tool | Full Form | Role |
|------|-----------|------|
| **SIEM** | Security Information and Event Management | Collects all logs in one place, correlates them, and identifies suspicious activity. |
| **AV** | Antivirus | Detects known malware, regularly scans system for threats. |
| **EDR** | Endpoint Detection & Response | Installed on each system; detects advanced threats; can also **contain & eradicate**. |

👉 Example:  
- **SIEM** = Central CCTV system watching all houses.  
- **AV** = Guard at each house checking only known thieves.  
- **EDR** = Smart guard at each house who can catch thieves AND lock them in.


## 3. After Incident Detection
Once an incident is detected:
1. Investigate the extent of attack.  
2. Take actions to prevent further spread.  
3. Eliminate the root cause.  

👉 Different incidents = different procedures.  
That’s why we need **Playbooks**.



## 4. Playbooks
- **Definition:** Step-by-step **guidelines** to handle a specific type of incident.  
- Purpose: Save time + ensure consistency.  
- Each type of incident (Phishing, Malware, DoS, etc.) has its own playbook.

### Example: Playbook for *Phishing Email*
1. Notify all stakeholders.  
2. Check if email is malicious → analyze header & body.  
3. Analyze any attachments.  
4. Check if anyone opened the attachment.  
5. Isolate infected systems.  
6. Block the sender.  



## 5. Runbooks
- **Definition:** Detailed **execution steps** inside a playbook.  
- More technical + can vary depending on resources.  
- Example:  
  - Playbook says → "Analyze email header"  
  - Runbook says → "Use tool XYZ, run command ABC to extract header details"

👉 **Playbook = What to do**  
👉 **Runbook = How to do it**



## 6. Simple Analogy
- **Playbook** = Football team’s game strategy ("Attack from left wing").  
- **Runbook** = Actual step-by-step moves of each player during that strategy.  
