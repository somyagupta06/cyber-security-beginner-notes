# 🛡️ Junior Security Analyst (Tier 1 SOC) 



## 1. Role Basics
- Position: **Junior Security Analyst / Tier 1 SOC Analyst**
- Main work = **Triage Specialist** → triaging & monitoring logs/alerts.
- Environment: Often **24x7 SOC operations**.



## 2. Responsibilities
- Monitor & investigate alerts.
- Configure/manage security tools.
- Develop/implement basic **IDS signatures**.
- Participate in SOC meetings & working groups.
- Create tickets & **escalate incidents** to Tier 2/Team Lead.
- Support SOC processes across **Preparation → Monitoring → Response**.


## 3. Required Qualifications
- **0-2 years** Security Operations experience.
- Basic knowledge of:
  - Networking → OSI Model, TCP/IP Model.
  - Operating Systems → Windows, Linux.
  - Web Applications.
- Scripting/programming skills = **plus**.

### ✅ Desired Certification:
- **CompTIA Security+**


## 4. SOC Tier Model
- **Tier 1 (Junior Analyst)** → Triaging, monitoring, escalating.
- **Tier 2 (Mid Analyst)** → Deeper investigation & response.
- **Tier 3 (Senior/Lead)** → Advanced analysis, threat hunting, IR leadership.

SOC = 24/7 monitoring, investigation, prevention, & response hub.
<img width="1210" height="578" alt="Screenshot 2025-09-25 at 6 57 50 PM" src="https://github.com/user-attachments/assets/2ffea9d1-fbbb-4ed9-af0c-a80e065ada69" />



## 5. Day-to-Day Activities
- Review tickets & alerts at start of shift.
- Monitor network traffic, IDS/IPS alerts, suspicious emails.
- Extract forensics data for analysis.
- Use OSINT (open-source intelligence) for decision making.
- Handle incident response (IR): detection, containment, remediation.
- Ask critical questions:
  - Did attacker exfiltrate data?
  - How much data was lost?
  - Did attacker pivot to other hosts?



## 6. SOC Responsibilities Breakdown

### 🔹 Preparation & Prevention
- Stay updated on latest threats (Twitter, Feedly, CISA alerts).
- Gather threat intelligence (actors, TTPs = tactics, techniques, procedures).
- Maintain defenses:
  - Update firewall signatures.
  - Patch vulnerabilities.
  - Block/allow lists (apps, IPs, emails).

### 🔹 Monitoring & Investigation
- Tools: **SIEM, EDR**.
- Alerts → Prioritised (Critical > High > Medium > Low).
- Junior Analysts = Perform **triaging**.
- Investigation mindset → Always ask **How? When? Why?**
- Drill down into logs, alerts, OSINT.

### 🔹 Response
- Contain compromised hosts (e.g., isolation).
- Terminate malicious processes.
- Delete malicious files.
- Coordinate with higher SOC tiers for escalation.

<img width="869" height="661" alt="Screenshot 2025-09-25 at 6 58 19 PM" src="https://github.com/user-attachments/assets/03456e85-d8f2-4293-9c26-f40e83602952" />


## 7. Key Takeaways
- Junior Analysts = **first line of defense**.
- Work is challenging (multiple log sources, nonstop alerts).
- Exciting & rewarding when incident is **successfully remediated**.
- Career growth path → **Tier 1 → Tier 2 → Tier 3 → Senior roles**.

---
# 🛡️ Security Operations Center (SOC) Overview

---

## 1. SOC Definition
- Centralized 24/7 function in an organization.
- Uses **people, processes, technology** to:
  - Monitor
  - Detect
  - Analyze
  - Respond to cybersecurity incidents
- Acts as **central hub** for IT telemetry (networks, devices, appliances, info stores).
- Correlates every logged event → decide how to act.

---

## 2. SOC Roles & Structure
- **SOC Manager** → Leads SOC.
- **Staff**:
  - SOC Analysts (Tier 1, 2, 3)
  - Threat Hunters
  - Incident Responders
  - Incident Response Managers
- Reports to **CISO → CIO/CEO**
- Architecture: **Hub-and-spoke**
  - Spokes: Vulnerability scanners, GRC systems, IPS, UEBA, EDR, TIP

---

## 3. Key Responsibilities of SOC

### 1️⃣ Take Stock of Resources
- Assets: Devices, processes, applications to protect.
- Defensive tools: SIEM, EDR, SOAR, firewalls, etc.
- Gain full visibility → reduce blind spots.

### 2️⃣ Preparation & Preventative Maintenance
- **Preparation**: Stay updated on threats, trends, innovations.
- **Preventative Maintenance**:
  - Maintain/update systems
  - Update firewall policies
  - Patch vulnerabilities
  - Allowlist/blocklist apps, IPs, emails

### 3️⃣ Continuous Proactive Monitoring
- Tools scan network 24/7 → flag anomalies.
- Tools: SIEM, EDR, SOAR
- Advanced: Behavioral analysis → reduces manual triage

### 4️⃣ Alert Ranking & Management
- Review alerts → discard false positives
- Prioritize based on severity (Critical → Low)
- Triage emerging threats

### 5️⃣ Threat Response
- Isolate endpoints
- Terminate malicious processes
- Delete malicious files
- Minimize business impact

### 6️⃣ Recovery & Remediation
- Restore systems & recover data
- Reconfigure systems / deploy backups
- Return network to pre-incident state

### 7️⃣ Log Management
- Collect, maintain, review logs
- Use logs for:
  - Baseline activity
  - Threat detection
  - Forensics & remediation

### 8️⃣ Root Cause Investigation
- Investigate **what, how, when, why** incidents occurred
- Use logs & data to prevent recurrence

### 9️⃣ Security Refinement & Improvement
- Continuous improvement against evolving threats
- Execute security roadmap
- Conduct red-team / purple-team exercises

### 🔟 Compliance Management
- Ensure compliance with:
  - GDPR, HIPAA, PCI DSS
- Regular audits → reduce legal/reputational risk

---

## 4. Optimizing SOC Operations
- Bridge **operational + data silos** → improve efficiency
- **Integration, automation, orchestration** reduce labor hours
- Use centralized dashboards → integrate threat intelligence
- Support **continuous visibility** & actionable intelligence

---

## 5. Best Practices for SOC Threat Management

### 5.1 Conduct Threat Management Assessment
- Evaluate:
  - Strengths & gaps
  - Risk posture
  - Data collection & usage

### 5.2 Create Threat Management Response Plan
- Core steps:
  - **Discovery** → baseline calculation, normalization, correlation
  - **Triage** → prioritize based on risk & asset value
  - **Analysis** → contextualize alerts
  - **Scoping** → iterative investigation
- Feed cases into incident response process

### 5.3 Determine Right Data
- Valuable data sources:
  - Event data from countermeasures / IT assets
  - IoCs from internal malware analysis
  - IoCs from external threat feeds
  - Host/network/database sensor data
- Ensures actionable & timely response

### 5.4 Review Threat Management Effectiveness
- Benchmark SOC performance
- Use consultants / penetration tests
- Compare against peers → optimize resource allocation
