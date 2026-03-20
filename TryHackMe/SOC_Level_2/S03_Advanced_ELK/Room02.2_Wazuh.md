# Wazuh 

## 1. What is Wazuh? (Very Simple)
Wazuh is a **security tool** that helps you:
- Detect attacks
- Monitor systems (Windows/Linux)
- Find vulnerabilities
- Analyze logs

👉 Think of Wazuh like a **security guard + CCTV + alarm system** for computers.

---

## 2. Why Wazuh is Powerful
Wazuh is not just one thing, it does many things together:

- Detect suspicious activity (like hacking attempts)
- Check system weaknesses (vulnerabilities)
- Show data in dashboards (graphs, charts)
- Help with compliance (PCI DSS, HIPAA, NIST)

👉 SOC use: You can monitor everything from one place.

---

## 3. How Wazuh Works (Important)

Wazuh uses **Manager + Agent model**

### Simple Idea:
- **Manager (Server)** = Brain 🧠  
- **Agent (Installed on devices)** = Eyes 👀

### Flow:
1. Agent collects logs from system
2. Agent sends logs to Manager
3. Manager analyzes logs
4. Alerts are created

👉 Example:
Laptop → Agent → Wazuh Server → Alert

---

## 4. What Do Agents Do?

Agents are installed on:
- Windows
- Linux

They monitor:
- Login activity
- File changes
- User creation
- System events

👉 SOC Tip: Without agents → No data → No alerts

---

## 5. Vulnerability Detection (Very Important)

Wazuh can:
- Check installed software
- Compare with CVE database
- Find vulnerabilities

👉 Example:
- Vim version is old
- CVE found → Alert generated

👉 Runs:
- First full scan when installed
- Then runs automatically (default: every 5 minutes)

---

## 6. Alerts and Rules (Core Concept)

Wazuh uses **rules** to detect events.

### Example:
- Rule ID: 5710  
- Detects: Login attempt with wrong username (SSH)

### Alert contains:
- Agent IP
- Hostname
- Description
- MITRE technique
- Rule ID
- Log file location

👉 SOC Tip: Always check **rule.id + description**

---

## 7. Example Alert (Simple Understanding)

Someone tries to login with wrong user:

- Username does not exist
- Wazuh detects it
- Creates alert

Mapped to:
- MITRE Technique: Brute Force
- MITRE ID: T1110

👉 Meaning: Possible attacker trying passwords

---

## 8. Where Alerts Are Stored

All alerts are saved in:

/var/ossec/logs/alerts/alerts.log

👉 SOC Work:
- Search logs
- Investigate events
- Correlate attacks

---

## 9. Too Many Alerts Problem

Wazuh can generate **a lot of alerts**

👉 Example:
- 700+ events daily
- Even normal activity can trigger alerts

Why?
- Default rules are very sensitive

👉 SOC Tip:
- Tune rules
- Reduce false positives

---

## 10. Compliance & Frameworks

Wazuh checks systems using:
- MITRE ATT&CK
- NIST
- GDPR

👉 It gives scores for system security

Example:
- How secure your server is
- What needs fixing

---

## 11. Visualization (Dashboards)

Wazuh shows:
- Graphs
- Event trends
- Login activity

👉 Example:
- Total logins: 285
- Filtered suspicious: 79

👉 SOC Tip:
Use dashboards to quickly spot anomalies

---

## 12. Authentication Monitoring

Wazuh tracks:
- Successful logins
- Failed logins

### Example:
- Failed login → High alert
- Successful login → Lower alert

👉 But:
You can change severity based on your needs

---

## 13. Key SOC Learnings

- Logs are everything
- Alerts come from rules
- Always check:
  - Rule ID
  - Description
  - MITRE mapping
- Tune alerts to reduce noise
- Use dashboards for quick analysis

---

## 14. Quick Revision Table

| Component | Meaning |
|----------|--------|
| Manager | Main server (analysis) |
| Agent | Collects logs |
| Rule | Condition for alert |
| Alert | Security event |
| CVE | Known vulnerability |
| MITRE | Attack technique mapping |

---

## Final Simple Line

👉 Wazuh = Tool that collects logs + detects threats + shows alerts  
👉 SOC Analyst = Person who reads those alerts and investigates
---
# Wazuh + Sysmon (Windows Logs) 

---

## 1. What is Happening Here? 

Windows records everything:
- Logins
- Network activity
- File access
- App behavior

👉 This data is stored in **Event Logs**

But default logs are not enough for deep security.

👉 So we use **Sysmon + Wazuh together**

---

## 2. What is Sysmon? (Very Simple)

Sysmon = A tool that gives **detailed system logs**

👉 Think:
- Windows logs = Basic CCTV  
- Sysmon = HD CCTV with zoom 🔍

Sysmon tracks:
- Process start (like PowerShell)
- Network connections
- File creation
- Registry changes

---

## 3. How Sysmon Works

Sysmon uses a **config file (XML)**

👉 This file tells Sysmon:
- What to monitor
- What to ignore

### Example:
We told Sysmon:
- Monitor if **powershell.exe starts**

👉 Meaning:
Whenever PowerShell runs → Event will be created

---

## 4. Important Sysmon Events (SOC Must Know)

| Event ID | Meaning |
|--------|--------|
| 1 | Process Created |
| 3 | Network Connection |
| 5 | Process Ended |
| 7 | DLL Loaded |
| 8 | Remote Thread (Injection) |
| 10 | Process Access |
| 11 | File Created |
| 12-14 | Registry Changes |

👉 SOC Tip:  
**Event ID 1 + 3 = Most important for detection**

---

## 5. Installing Sysmon (Concept Only)

Command used:

Sysmon64.exe -accepteula -i detect_powershell.xml

👉 Meaning:
- Install Sysmon
- Apply config file

---

## 6. Verifying Sysmon

Go to:
- Event Viewer
- Search: Sysmon

👉 You will see logs like:
- PowerShell opened
- Network connections

---

## 7. Where Wazuh Comes In

Sysmon only **collects logs**

👉 Wazuh does:
- Analyze logs
- Create alerts
- Show dashboards

---

## 8. Connecting Sysmon to Wazuh (Very Important)

We tell Wazuh agent:

👉 "Send Sysmon logs to server"

### File:
C:\Program Files (x86)\ossec-agent\ossec.conf

### Add this:

<localfile>
<location>Microsoft-Windows-Sysmon/Operational</location>
<log_format>eventchannel</log_format>
</localfile>

👉 Meaning:
- Take Sysmon logs
- Send to Wazuh manager

---

## 9. Restart Step (Important)

After config:
- Restart Wazuh agent (or system)

👉 Otherwise changes will not apply

---

## 10. Creating Detection Rule in Wazuh

Now Wazuh needs rules to detect attacks.

### Example Rule:

- Detect PowerShell execution
- Raise alert

👉 If:
- powershell.exe runs
- OR .ps1 script runs

Then:
- Alert generated (Level 12 = High)

---

## 11. Rule Logic (Simple)

Condition:
- Sysmon Event ID 1 (process start)

Check:
- Is it PowerShell?

If YES:
👉 Generate alert

---

## 12. Final Flow (Very Important for SOC)

1. User runs PowerShell
2. Sysmon logs event
3. Wazuh agent collects log
4. Sends to Wazuh server
5. Wazuh rule checks it
6. Alert created

👉 This is full detection pipeline

---

## 13. Real SOC Use Case

Example:
- Attacker runs malicious PowerShell script

What happens:
- Sysmon detects it
- Wazuh flags it
- SOC analyst investigates

---

## 14. Key Takeaways (Revision)

- Sysmon = Log generator
- Wazuh = Analyzer + Alert system
- XML = Sysmon config
- Rule = Detection logic
- Event ID = Type of activity

---

## 15. One Line Summary

👉 Sysmon collects deep Windows logs  
👉 Wazuh reads those logs and tells you if something is suspicious  
---
# Wazuh + Linux Logs (Apache + auditd) 
---

## 1. Big Idea 

Linux also creates logs just like Windows.

👉 But:
- Linux logs = raw data
- Wazuh = reads + analyzes + alerts

👉 So flow is:
Linux → Logs → Wazuh Agent → Wazuh Server → Alerts

---

## 2. Log Collector (Very Important)

Wazuh uses **log collector** to:

- Read log files
- Send logs to server

👉 You must tell Wazuh:
"Which log file to monitor"

---

## 3. Example: Apache2 Logs

Apache = Web server  
👉 It stores logs like:
- Errors
- Requests
- Attacks

---

### Config in Agent

File:
/var/ossec/etc/ossec.conf

Add:

<localfile>
  <location>/var/log/example.log</location>
  <log_format>syslog</log_format>
</localfile>

👉 Meaning:
- Read this log file
- Send to Wazuh server

---

## 4. Wazuh Rules (Important Concept)

Wazuh already has **900+ rules**

Location:
/var/ossec/ruleset/rules

Examples:
- Apache
- Docker
- WordPress
- SQL
- Firewall

👉 For Apache:
0250-apache_rules.xml

👉 SOC Tip:
You don’t always need to create rules from scratch

---

## 5. After Config (Important Step)

👉 Restart agent

Otherwise:
- Logs will NOT be sent

---

## 6. What is auditd? (Very Important)

auditd = Linux monitoring tool

👉 It tracks:
- Commands executed
- File access
- User activity

👉 Think:
Sysmon (Windows) = auditd (Linux)

---

## 7. Why auditd is Important for SOC

It helps detect:
- Privilege abuse
- Suspicious commands
- Attacker activity

👉 Example:
- root user runs commands → suspicious

---

## 8. Installing auditd (Concept)

Install:
sudo apt-get install auditd audispd-plugins

Enable:
sudo systemctl enable auditd.service
sudo systemctl start auditd.service

👉 Now auditd is active

---

## 9. Creating auditd Rule (Core Part)

File:
/etc/audit/rules.d/audit.rules

Add rule:

-a exit,always -F arch=b64 -F euid=0 -S execve -k audit-wazuh-c

---

### What this means (Very Simple):

| Part | Meaning |
|------|--------|
| exit,always | Always log event |
| arch=b64 | 64-bit system |
| euid=0 | root user |
| execve | command execution |
| key | label for tracking |

👉 Final meaning:
"Log every command executed by root user"

---

## 10. Apply the Rule

Run:

sudo auditctl -R /etc/audit/rules.d/audit.rules

👉 Meaning:
Reload rules

---

## 11. Where auditd Logs Are Stored

/var/log/audit/audit.log

👉 This file contains:
- Commands run
- Who ran them
- When

---

## 12. Send auditd Logs to Wazuh

Edit:
/var/ossec/etc/ossec.conf

Add:

<localfile>
  <location>/var/log/audit/audit.log</location>
  <log_format>audit</log_format>
</localfile>

👉 Meaning:
- Read audit logs
- Send to Wazuh

---

## 13. Final Flow (Very Important)

1. User runs command (as root)
2. auditd logs it
3. Wazuh agent reads log
4. Sends to server
5. Wazuh rule analyzes
6. Alert created

---

## 14. Real SOC Example

Attacker:
- Gets root access
- Runs commands like:
  - cat /etc/passwd
  - nc (netcat)
  - tcpdump

👉 auditd logs it  
👉 Wazuh detects it  
👉 SOC analyst investigates  

---

## 15. Key Differences (Windows vs Linux)

| Feature | Windows | Linux |
|--------|--------|--------|
| Tool | Sysmon | auditd |
| Logs | Event Viewer | Log files |
| Format | Event channel | Text logs |
| Wazuh role | Same | Same |

---

## 16. Quick Revision

- Log collector = Reads logs
- Apache logs = Web activity
- auditd = Tracks commands
- Rules = Detection logic
- Restart = Required after config

---

## Final One Line

👉 auditd collects Linux activity  
👉 Wazuh reads it and tells you if something is suspicious  
---

# Wazuh API + Reporting 

---

## 1. What is Wazuh API? 

Wazuh API lets you:
- Control Wazuh using commands
- Get data (agents, alerts, status)
- Automate tasks

👉 Think:
Dashboard = Manual work  
API = Automation + scripting 🔧

---

## 2. Authentication 

Before using API:
👉 You must **login**

When you login:
- You get a **TOKEN**

👉 Token = temporary password/session

---

### Step 1: Get Token

TOKEN=$(curl -u USER:PASSWORD -k -X GET "https://SERVER_IP:55000/security/user/authenticate?raw=true")

👉 Meaning:
- Send username + password
- Get token

---

### Step 2: Verify Token

curl -k -X GET "https://SERVER_IP:55000/" -H "Authorization: Bearer $TOKEN"

👉 If success:
- You will see API info (version, hostname, etc.)

---

## 3. How API Requests Work

You use:
- GET → Read data
- POST → Create data
- PUT → Update
- DELETE → Remove

👉 Example:
-X GET

---

## 4. Get Manager Status

curl -k -X GET "https://SERVER_IP:55000/manager/status?pretty=true" -H "Authorization: Bearer $TOKEN"

👉 Shows:
- Which services are running
- Which are stopped

---

## 5. Get Manager Configuration

curl -k -X GET "https://SERVER_IP:55000/manager/configuration?pretty=true&section=global" -H "Authorization: Bearer $TOKEN"

👉 Gives:
- System settings
- Service details

---

### Example Output Meaning

- running → service active ✅  
- stopped → service not working ❌  

👉 SOC Tip:
Always check if critical services are running

---

## 6. Working with Agents

You can query agents using API

### Example:

curl -k -X GET "https://SERVER_IP:55000/agents?pretty=true&status=active" -H "Authorization: Bearer $TOKEN"

👉 Output includes:
- Agent ID
- Name
- Status
- Version

---

👉 SOC Use:
- Check if agent is active
- Detect offline agents

---

## 7. Why API is Useful (SOC View)

- Automate monitoring
- Integrate with scripts/tools
- Build alerts pipelines
- Fetch data quickly

👉 Example:
Python script → Fetch alerts → Send email

---

## 8. Wazuh API Console (UI Method)

Wazuh also gives:
👉 Built-in API console (GUI)

Location:
- Wazuh → Tools → API Console

---

### How to Use:
1. Select query
2. Click run button ▶️

👉 Same commands as curl
👉 Easier for beginners

---

## 9. Reporting Module (Important)

Wazuh can generate reports

👉 Example:
- Last 24 hours alerts
- Security events summary

---

### Steps:

1. Go to:
Modules → Security Events  

2. Generate report

👉 If button disabled:
- No data available
- Change date range

---

## 10. Download Report

Go to:
Wazuh → Management → Reporting

👉 You will see:
- All reports list

Click:
- Download icon → PDF

---

## 11. Sample Data (For Practice)

Wazuh provides demo data

### Steps:
1. Wazuh → Settings  
2. Sample Data  
3. Click "Add Data"

---

👉 After loading:
- More alerts available
- Better practice environment

---

### Important:

Set date range:
👉 Last 7 days (minimum)

Otherwise:
- No data visible

---

## 12. Final Flow (Simple)

1. Authenticate → Get Token  
2. Use Token → Call API  
3. Fetch data (agents, alerts)  
4. Analyze / automate  

---

## 13. Key SOC Takeaways

- API = Automation power
- Token = Required for access
- Use API to:
  - Check agents
  - Check services
  - Pull alerts
- Reporting = Quick summary for analysis

---

## Final One Line

👉 Wazuh API lets you control and monitor everything using commands instead of UI  

