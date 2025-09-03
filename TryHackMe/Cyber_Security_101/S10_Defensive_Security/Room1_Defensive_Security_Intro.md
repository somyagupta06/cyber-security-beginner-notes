# Defensive Security (Blue Team)

## 1. What is Defensive Security?
- Defensive security is all about **protecting systems** from attacks.
- Main goals:
  1. **Prevent intrusions** (stop attacks before they happen)
  2. **Detect intrusions** and **respond properly** if they happen

**Example:**
- Offensive security: Hacker tries to break into a bank system
- Defensive security: Bank sets firewalls, monitors logs, trains staff to spot phishing, so hacker cannot succeed



## 2. Key Players
| Role         | What They Do                                      |
|--------------|--------------------------------------------------|
| Blue Team    | Focuses on defense, protects and monitors       |
| SOC Team     | Security Operations Center – monitors network continuously |
| DFIR Team    | Digital Forensics & Incident Response – investigates and responds to breaches |
| Malware Analyst | Analyzes malicious software to understand and prevent attacks |



## 3. Defensive Security Tasks

### 3.1 User Cyber Security Awareness
- **Train users** to avoid phishing, weak passwords, social engineering
- **Example:** Employee sees suspicious email → reports it → attack blocked

### 3.2 Documenting & Managing Assets
- Know all **computers, servers, devices** in your network
- **Example Table:**
| Device Name | IP Address    | Owner      | Purpose         |
|-------------|-------------|------------|----------------|
| Laptop-01   | 192.168.1.10 | Alice      | Employee laptop |
| Server-01   | 192.168.1.20 | IT Dept    | Web server      |

### 3.3 Updating & Patching Systems
- **Install software updates** regularly to fix known vulnerabilities
- **Example:** Patch Windows server → prevents hackers from exploiting old bugs

### 3.4 Preventative Security Devices
- **Firewall:** Controls incoming and outgoing network traffic
  - Example: Only allow HTTP (80) and HTTPS (443), block everything else
- **IPS (Intrusion Prevention System):** Blocks known attack patterns
  - Example: Detects SQL injection attack → blocks the request

### 3.5 Logging & Monitoring
- Record system and network activities for detection
- **Example:** 
  - New unknown device connects → alert sent to SOC team
  - Multiple failed login attempts → trigger an alarm



## 4. Other Important Topics

### 4.1 Security Operations Center (SOC)
- Central team monitoring security continuously
- Detects suspicious activities, responds to incidents

### 4.2 Threat Intelligence
- Collects information about current threats and vulnerabilities
- Helps predict and prevent attacks
- **Example:** SOC learns about new ransomware → updates IPS rules

### 4.3 Digital Forensics & Incident Response (DFIR)
- Investigates security incidents
- Steps:
  1. Identify incident
  2. Contain it
  3. Analyze logs & devices
  4. Eradicate threat
  5. Recover systems

### 4.4 Malware Analysis
- Study malicious software to understand its behavior
- Helps create signatures for IPS/firewalls
- **Example:** Malware encrypts files → analysts study it → SOC creates detection rules



## Summary Table: Offensive vs Defensive Security
| Feature                | Offensive Security (Red Team)      | Defensive Security (Blue Team)    |
|------------------------|----------------------------------|---------------------------------|
| Goal                   | Find vulnerabilities & exploit   | Protect, detect & respond       |
| Techniques             | Hacking, pen-testing, social engineering | Firewalls, IPS, monitoring, patching |
| Teams                  | Red Team                          | Blue Team, SOC, DFIR, Malware Analysis |
| Approach               | Attack                            | Defense                          |


**Key Takeaways:**
- Defensive security is proactive **and reactive**
- Always keep systems updated, monitored, and secure
- Users are the first line of defense
- SOC + DFIR + Threat Intelligence = backbone of strong defense
---
# Defensive Security: SOC & DFIR Notes

## 1. Security Operations Center (SOC)
A SOC is a **team of cybersecurity professionals** that monitors a network and systems to detect **malicious events**.

### Main SOC Focus Areas
| Area                  | What it Means & Example                                                                 |
|-----------------------|---------------------------------------------------------------------------------------|
| Vulnerabilities       | Weakness in a system. Example: Missing Windows patch → hacker can exploit it        |
| Policy Violations     | Breaking rules for network protection. Example: Uploading confidential data online   |
| Unauthorized Activity | Stolen credentials used to log in. Example: Hacker logs in using stolen username/password |
| Network Intrusions    | Attackers gaining access through malicious links or public server vulnerabilities    |

### Key SOC Task: Threat Intelligence
- **Threat Intelligence** = info about current/potential enemies (threats)
- Purpose: Prepare company defense (threat-informed defense)
- **Threats depend on company type**:
  - Mobile operator: attackers want customer data
  - Petroleum refinery: attackers may want to stop production
- **Adversary examples**: nation-state hackers, ransomware gangs

#### Threat Intelligence Process
1. **Data Collection**: Gather logs from local systems + public sources (forums, websites)
2. **Data Processing**: Arrange info in a usable format
3. **Analysis**: Understand attackers, their goals, and recommend steps

**Outcome**: Know attacker tactics, predict their moves, and prepare response strategies.



## 2. Digital Forensics & Incident Response (DFIR)

### 2.1 Digital Forensics
- Use science to **investigate cyber crimes** and gather evidence
- Focus areas:
| Focus Area     | What It Tells Us                                                   |
|----------------|------------------------------------------------------------------|
| File System    | Installed programs, created/deleted files, partially overwritten files |
| System Memory  | Malware running in RAM (not saved to disk)                        |
| System Logs    | What happened on computers (user actions, software activity)     |
| Network Logs   | What happened on network (packets, intrusions)                   |



### 2.2 Incident Response (IR)
- **Incident** = data breach, attack, policy violation, misconfiguration
- Goal: Reduce damage + recover quickly
- **4 Major Phases**:
1. **Preparation**: Train team + put preventive measures
2. **Detection & Analysis**: Identify incident + assess severity
3. **Containment, Eradication, Recovery**:
   - Contain: Stop attack spreading
   - Eradicate: Remove malware/attacker access
   - Recover: Restore systems
4. **Post-Incident Activity**: Report + lessons learned to prevent future incidents

**Example:** System infected with virus → contain virus → clean files → restore system → write report



### 2.3 Malware Analysis
- **Malware** = malicious software (virus, trojan, ransomware)
- Types:
| Malware Type | Description & Example |
|--------------|--------------------|
| Virus        | Attaches to program, spreads, deletes/modifies files. Example: slows computer |
| Trojan Horse | Appears useful but has hidden malicious code. Example: fake video player giving attacker control |
| Ransomware   | Encrypts files, demands payment for decryption key |

#### Malware Analysis Methods
1. **Static Analysis**
   - Study malware **without running it**
   - Requires knowledge of assembly/program code
2. **Dynamic Analysis**
   - Run malware in **controlled environment** (sandbox)
   - Observe behavior: what files it changes, network connections it makes, etc.



## Summary Table: SOC vs DFIR
| Aspect                  | SOC                                  | DFIR                                    |
|-------------------------|-------------------------------------|----------------------------------------|
| Focus                   | Monitor & detect threats            | Investigate & respond to incidents     |
| Main Task               | Threat Intelligence                 | Digital Forensics, Incident Response, Malware Analysis |
| Goal                    | Prevent attacks & predict threats   | Reduce damage, recover systems         |
| Tools                   | Logs, SIEM, network monitoring      | Forensic tools, sandboxes, malware analyzers |



**Key Takeaways**
- SOC = active monitoring + threat intelligence
- DFIR = analyze incidents + respond + learn from them
- Malware analysis = understand attacker tools to prevent future attacks
- Together, SOC + DFIR + Malware Analysis = strong defense strategy
