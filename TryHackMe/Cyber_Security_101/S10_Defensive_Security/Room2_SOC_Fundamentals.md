# Introduction to SOC (Security Operations Center)

## 1. Why SOC is Important
- Modern organizations store **critical confidential data** in networks and systems, not physical files.
- Unauthorized access, loss, or modification of this data can cause **huge damage**.
- Threat actors **find and exploit new vulnerabilities** daily.
- Traditional security measures are often **not enough** to prevent all threats.
- Solution: **Dedicated SOC team** to manage and protect security 24/7.



## 2. What is a SOC?
- **SOC** = Security Operations Center
- A **specialized facility/team** that continuously monitors:
  - Network traffic
  - Systems
  - Resources
- Goal: **Identify suspicious activity** and **prevent damage**
- Works **24 hours a day, 7 days a week**

**Example:**  
- Employee clicks a phishing email → SOC detects unusual activity → blocks access before damage occurs



## 3. Learning Objectives in SOC
1. **Building a Baseline for SOC**
   - Know what normal activity looks like in the network
   - Helps in spotting unusual behavior

2. **Detection and Response**
   - Detect suspicious activity or attacks
   - Respond immediately to minimize damage

3. **Role of People, Processes, and Technology**
   | Component  | Role in SOC                                  |
   |------------|---------------------------------------------|
   | People     | Analysts, SOC engineers, incident responders|
   | Processes  | Policies, procedures, incident handling     |
   | Technology | Monitoring tools, firewalls, SIEM, IPS     |

4. **Practical Exercise**
   - Apply monitoring and response methods in a lab environment
   - Example: Detect and respond to an intrusion simulation



**Key Takeaways**
- SOC = heart of modern defensive security
- Constant monitoring + fast response = minimize damage
- Successful SOC = proper mix of **people, processes, and technology**
---
# SOC: Detection & Response and the 3 Pillars

## 1. Main Focus of SOC
- SOC = **Detection + Response**
- Uses **security solutions/tools** to monitor the entire network from a centralized location
- Continuous monitoring is required to detect and respond to any security incident



## 2. Detection in SOC

| Detection Area          | What it Means & Example                                                      |
|-------------------------|----------------------------------------------------------------------------|
| Vulnerabilities         | Weakness in software/hardware attackers can exploit. Example: Windows PCs missing a critical patch |
| Unauthorized Activity   | Someone uses stolen credentials to log in. Example: Employee username/password stolen, SOC detects unusual login location |
| Policy Violations       | Breaking company security rules. Example: Downloading pirated media or sending confidential files insecurely |
| Intrusions              | Unauthorized access to systems/networks. Example: Hacker exploits a web app or a user visits a malicious site |



## 3. Response in SOC
- SOC supports **Incident Response (IR)**
- Steps:
  1. **Minimize impact** of the incident
  2. **Analyze root cause** of the incident
  3. **Assist IR team** in carrying out mitigation steps

**Example:**  
- Malware detected on employee computer → SOC isolates device (containment) → cleans malware (eradication) → helps restore system (recovery)



## 4. The 3 Pillars of SOC
SOC effectiveness depends on **People, Processes, and Technology**:

| Pillar      | Description & Example                                                   |
|------------|--------------------------------------------------------------------------|
| People      | Skilled SOC analysts, incident responders, engineers                     |
| Process     | Security policies, procedures, incident handling workflows               |
| Technology  | Security tools: SIEM, firewalls, IDS/IPS, monitoring solutions           |

- **Coexistence of all three pillars** creates a **mature SOC environment**
- Example: Professional analysts (People) using SIEM tools (Technology) following proper alert escalation workflow (Process) → detects and responds efficiently


## Key Takeaways
- SOC focuses on **detecting threats** and **responding quickly**
- Detection areas: vulnerabilities, unauthorized activity, policy violations, intrusions
- Response = minimize damage + investigate + assist recovery
- **People + Process + Technology** = foundation for a strong, mature SOC

---
# SOC: People and Roles

## 1. Importance of People in SOC
- Even with automated security solutions, **humans are critical** in a SOC.
- Security solutions generate **many alerts**, which can create noise.
- Example:  
  - Imagine a **fire brigade** with centralized alarms all over a city.  
  - Many alarms go off at once → most are false (smoke from cooking) → wasting time.  
- Similarly, **SOC analysts** help filter real threats from irrelevant alerts and ensure **prompt response**.


## 2. SOC Team Roles and Responsibilities

| Role                 | Responsibilities & Example                                                                 |
|----------------------|------------------------------------------------------------------------------------------|
| SOC Analyst (Level 1) | First responders to alerts. Perform **basic triage** to see if alert is harmful. Report through proper channels. |
| SOC Analyst (Level 2) | Handles deeper investigation. Correlates data from multiple sources for proper analysis. |
| SOC Analyst (Level 3) | Experienced analysts. Proactively look for threats, support **incident response**: containment, eradication, recovery. |
| Security Engineer     | Deploys and configures security tools for smooth operation. Example: SIEM, firewalls, IDS/IPS |
| Detection Engineer    | Creates detection rules (logic) for security solutions to identify harmful activity. |
| SOC Manager           | Manages SOC processes, supports team, communicates with **CISO** about security posture |

**Note:** Roles may vary depending on organization size and criticality.



## 3. SOC Analyst Hierarchy Example

SOC Manager
├── SOC Analyst L3
│ ├── SOC Analyst L2
│ │ └── SOC Analyst L1
├── Security Engineer
└── Detection Engineer

**Key Takeaways**
- **People filter alerts** and prevent SOC from being overwhelmed by noise.
- SOC team roles = **tiered approach** from basic alert handling to proactive threat hunting.
- Experience and proper processes allow SOC to **detect and respond efficiently**.
---
# SOC Processes

SOC team roles are important, but **processes** define how they handle alerts and incidents efficiently.  



## 1. Alert Triage
- **Alert triage** = first response to any alert
- Goal: Analyze alert, determine **severity**, and **prioritize** it
- Focuses on answering the **5 Ws**:

### Example Alert
- **Alert:** Malware detected on Host: GEORGE PC

| 5 Ws   | Example Answer |
|--------|----------------|
| What?  | Malicious file detected on the host |
| When?  | Detected at 13:20 on June 5, 2024 |
| Where? | Directory of host "GEORGE PC" |
| Who?   | User George |
| Why?   | File downloaded from pirated software website |

> **Key Point:** Triage helps SOC analysts know **what happened, when, where, who, and why**.  



## 2. Reporting
- Alerts must be **escalated** to higher-level analysts for resolution
- Escalation usually done via **tickets**
- Reports should include:
  - All **5 Ws**
  - Detailed **analysis**
  - **Screenshots** or other evidence
- Example: L1 Analyst detects malware → creates ticket with all details → L2/L3 Analysts take further action



## 3. Incident Response & Forensics
- Some alerts indicate **highly malicious activity**
- **Incident Response (IR)** is initiated for critical incidents
- **Forensics activity** may be required to:
  - Determine **root cause**
  - Analyze **system/network artifacts**
- Example:
  - Malware spreads across network → IR team isolates infected systems → Forensics team analyzes logs and memory → restores systems safely



### Key Takeaways
- Alert triage = foundation of SOC workflow
- **5 Ws** ensure clarity and prioritization
- Reporting = timely escalation with evidence
- Incident response & forensics = handle **critical, severe threats**
---
# SOC Pillar: Technology (Security Solutions)

Having the **right People and Processes** is not enough without **Technology**.  
Technology refers to **security solutions/tools** that help SOC detect and respond to threats efficiently.



## 1. Why Security Solutions are Important
- Networks have **many devices and applications**
- Detecting and responding manually for each device = **huge effort**
- Security solutions **centralize information** and **automate detection & response**
- Minimizes manual effort of SOC team



## 2. Key Security Solutions in SOC

| Solution | Role & Description | Example |
|----------|------------------|---------|
| **SIEM** (Security Information & Event Management) | Collects logs from various devices, applies detection rules, correlates events, alerts SOC team | Detects unusual login from foreign country after correlating multiple logs |
| **EDR** (Endpoint Detection & Response) | Monitors endpoint activities in real-time and historically; allows automated responses | Investigate malware activity on a laptop and isolate it with a few clicks |
| **Firewall** | Network security barrier between internal & external networks; filters unauthorized traffic | Block suspicious IP trying to access internal server |
| **Others** | Antivirus, EPP (Endpoint Protection Platform), IDS/IPS (Intrusion Detection/Prevention), XDR, SOAR | Example: IDS alerts SOC of SQL injection attempt on web server |

**Notes:**
- SIEM = mainly **detection**  
- EDR = **detection + automated response**  
- Firewall = primarily **network traffic control + some detection**  
- Other tools depend on **organization size and threat surface**



### Key Takeaways
- Technology **centralizes monitoring and automates tasks**
- Security solutions reduce **manual effort** and improve SOC efficiency
- Choosing tools = depends on **available resources + threats**  
- Together with **People + Processes**, Technology completes the **SOC pillars**
