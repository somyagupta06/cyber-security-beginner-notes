# SIEM  

## 1) What is SIEM?
SIEM = Security Information and Event Management.
In one line: SIEM collects logs from many devices, puts them in one place, and helps find bad or strange activity by linking (correlating) events.

Think of SIEM like a CCTV control room for your whole network: many cameras (devices) send video (logs) to one place where a person (or software) watches and raises an alarm if something odd happens.

---

## 2) Why do we need SIEM? 
- Devices produce many logs every second. Checking each device one-by-one is very slow.
- SIEM centralizes logs so we can search, analyze, and respond faster.
- It automatically alerts when patterns look suspicious.
- Keeps history so we can investigate incidents after they happen.



## 3) Types of log sources (two groups)

HOST-CENTRIC LOGS (inside a computer)
- Examples: Windows Event Logs, Sysmon, Osquery
- Useful events:
  - User logged in or failed login
  - A program (process) started
  - File opened or deleted
  - PowerShell command executed
  - Registry change

NETWORK-CENTRIC LOGS (communication between machines)
- Examples: Firewall logs, Proxy/HTTP logs, VPN logs, SSH logs
- Useful events:
  - SSH connection to a server
  - Website visited (HTTP/HTTPS)
  - File transferred by FTP
  - User connected to company via VPN
  - Peer-to-peer file share activity

Simple comparison:
Host logs focus on "what happened on this machine".
Network logs focus on "who talked with whom and what data moved".



## 4) What SIEM does (features)
- Real-time log ingestion: collects logs as they appear
- Alerting: creates alerts for suspicious patterns
- Correlation: connects multiple events to detect bigger attacks (example below)
- Search & investigation: find past events quickly
- Dashboards: visual charts and reports
- Retention: store logs for days/months for compliance or investigation
- Response: helps block or contain threats (sometimes integrated with other tools)



## 5) Example: correlation (simple story)
Situation:
- A user has many failed logins (5 failed) then a success.
- Immediately after, a new remote PowerShell runs and copies a file to an external IP.

Alone, each event may look small. SIEM can correlate:
1) failed_login x5 -> raise medium risk
2) successful_login from same account -> join event 1
3) remote PowerShell + file transfer -> join events 1+2
Result: SIEM creates a single alert "possible account takeover and data exfiltration".



## 6) Simple text table (visual)
Log Type        | Example Event                     | Why useful
--------------- | ----------------------------------| -----------------------
Host            | Process started (cmd.exe)         | Shows code execution
Host            | File deleted                      | Shows tampering
Network         | HTTP request to unknown domain    | Might be data exfiltration
Network         | VPN login from new country        | Possible credential misuse



## 7) How SIEM helps a small team (steps)
1) Collect logs from endpoints, servers, firewall, web server.
2) Normalize logs (make fields consistent).
3) Create rules (e.g., many failed logins -> alert).
4) SIEM alerts analysts. Analysts investigate with single view.
5) If confirmed, respond (disable account, block IP).
6) Store evidence for future audits.



## 8) Very simple example of a detection rule (plain text)
Rule name: Multiple failed logins then success
Conditions:
- If user has 5 or more failed login events within 10 minutes
AND
- A successful login for same user within next 5 minutes
Then:
- Generate HIGH priority alert: "Possible brute-force then success"

## 9) Quick tips 
- Start by collecting a few logs: Windows Event Logs and Firewall logs.
- Learn to search logs (find by username, IP, or time).
- Build small correlation rules and test with fake events.
- Use dashboards to see trends (e.g., top source IPs, failed logins over time).



## 10) Short summary
SIEM centralizes all logs, finds suspicious patterns by correlating events, alerts you quickly, helps investigate incidents, and stores evidence. It gives visibility across hosts and networks so teams can detect and respond to threats much faster.

---
# Network Devices and Logs

Every device in a network creates logs whenever something happens.  
Examples:  
- A user opens a website  
- Someone connects with SSH  
- Someone logs into Windows or Linux machine  

These logs are later collected by SIEM for monitoring.  
Let’s see device-wise:


## 1) Windows Machine
- Windows records every event in **Event Viewer**.
- Each activity has a unique **Event ID**.  
  (Example: Logon event has ID 4624, failed logon has 4625.)
- Analysts use Event Viewer to check logs.  
- In enterprise, these logs are forwarded to SIEM.  

**Why useful?**  
- Easy to track user login/logout, file access, process execution.



## 2) Linux Workstation
- Linux saves logs in different files under `/var/log/`.  
Some common ones:  

Log File Path | Purpose
------------- | ----------------------------------------
/var/log/httpd | Web server request/response + errors
/var/log/cron  | Cron job execution logs
/var/log/auth.log or /var/log/secure | Authentication (login, sudo usage, etc.)
/var/log/kern  | Kernel-related system events

**Example cron log entry:**
```
May 28 13:04:20 ebr crond[2843]: /usr/sbin/crond 4.4 started with loglevel notice
Jun 13 07:46:22 ebr crond[3592]: unable to exec /usr/sbin/sendmail: cron output for user root

```

## 3) Web Server
- Web servers (like Apache) store request/response logs.  
- In Linux: `/var/log/apache/` or `/var/log/httpd/`.

**Example Apache log entry:**
```
192.168.21.200 - - [21/Mar/2022:10:17:10 -0300] "GET /cgi-bin/try/ HTTP/1.0" 200 3395
127.0.0.1 - - [21/Mar/2022:10:22:04 -0300] "GET / HTTP/1.0" 200 2216
```
**Why useful?**  
- Helps detect suspicious requests (e.g., directory traversal, brute force, SQL injection attempts).



## 4) Log Ingestion in SIEM
"Log ingestion" = how SIEM collects logs from devices.  
Different SIEM tools (Splunk, ELK, QRadar, etc.) have their own methods.  
But common ways are:

### a) Agent / Forwarder
- A lightweight agent is installed on endpoint.
- Example: **Splunk Forwarder**.
- Captures logs locally and sends them to SIEM server.

### b) Syslog
- Old + standard protocol.
- Devices (servers, firewalls, routers) send logs via syslog in real time to SIEM.

### c) Manual Upload
- Some SIEMs (Splunk, ELK) allow uploading offline log files.
- Useful for quick one-time analysis.

### d) Port-Forwarding
- SIEM listens on a specific port.
- Endpoints forward logs directly to that port.



## 5) Why Ingestion is Important
- Without ingestion, SIEM has nothing to analyze.
- Ingestion makes logs centralized, normalized (same format), and searchable.
- Analysts can then:
  - Detect suspicious activity in real-time
  - Create correlation rules
  - Build dashboards and reports
  - Investigate past incidents



## 6) Simple Comparison Table

Device / Source | Example Logs | Why it matters
--------------- | ------------ | -----------------------
Windows Machine | Event ID 4625 (failed login) | Detect brute-force attempts
Linux Workstation | `/var/log/auth.log` | Detect failed SSH attempts
Web Server | Apache access log | Detect web attack patterns
Firewall / Router | Connection logs | Detect abnormal external access
Database | Query logs | Detect suspicious data access



## 7) Summary
- Each network device generates logs (Windows, Linux, Web Server, etc.).
- Logs are collected into SIEM using **agents, syslog, manual upload, or port-forwarding**.
- Once logs are in SIEM, they are normalized and analyzed.
- SIEM helps in real-time monitoring, alerting, and incident response.
---

# SIEM Capabilities and SOC Analyst Responsibilities

## 1) Role of SIEM in Cybersecurity
- SIEM correlates logs from multiple devices.
- When suspicious activity or threshold is detected → it **raises an alert**.
- Analysts then investigate and take actions.
- SIEM = gives **visibility** + **early detection** + **protection**.

Think of SIEM like an **alarm system**:  
- Sensors (logs) are everywhere.  
- SIEM analyzes signals.  
- If something unusual happens, it rings the alarm.  



## 2) SIEM Capabilities
Some key abilities of SIEM:

1. **Correlation**  
   - Connects events from different log sources.  
   - Example: Failed logins (Windows) + strange network connection (Firewall) → Possible attack.

2. **Visibility**  
   - Covers both host activities (inside machines) and network activities (communications).  
   - Ensures attackers cannot hide easily.

3. **Threat Investigation**  
   - Helps analysts dive deep into suspicious activity.  
   - Past logs can be checked for related evidence.

4. **Threat Hunting**  
   - Analysts can actively search for hidden threats not caught by rules.  
   - Example: Searching logs for rare processes or connections.



## 3) SIEM inside SOC
- **SOC (Security Operations Center)** = team + tools + processes for defending network.
- SIEM is a **core component** of SOC.
- Flow inside SOC:
  1) SIEM collects logs
  2) Matches logs against rules
  3) Raises alerts
  4) Analysts investigate & respond


## 4) SOC Analyst Responsibilities
SOC analysts use SIEM every day. Their main tasks are:

1. **Monitoring & Investigating**
   - Watch dashboards and alerts
   - Investigate suspicious activity

2. **Identify False Positives**
   - Some alerts are not real threats (false alarms)
   - Analyst must confirm if it’s real or not

3. **Rule Tuning**
   - Adjust or refine SIEM rules
   - Reduce noise (unnecessary alerts)

4. **Reporting & Compliance**
   - Prepare reports of incidents and findings
   - Ensure logs meet compliance requirements (e.g., ISO, GDPR, PCI-DSS)

5. **Identify Blind Spots**
   - Find areas with no log coverage (e.g., missing device logs)
   - Ensure complete visibility across network



## 5) Quick Comparison Table

Feature / Responsibility | Explanation | Example
------------------------ | ----------- | -----------------------------
SIEM Correlation         | Connects multiple events | Failed logins + VPN login → suspicious
SIEM Visibility          | Shows both host & network | Process execution + unusual IP traffic
SOC Monitoring           | 24/7 watch over alerts | Analyst checks SIEM dashboards
SOC False Positive Check | Verify real vs fake alerts | Firewall block = not an attack
SOC Rule Tuning          | Adjust detection rules | Change failed login threshold from 3 to 5



## 6) Short Summary
- SIEM = brain of SOC, detects suspicious patterns, raises alerts.  
- Capabilities: correlation, visibility, threat investigation, threat hunting.  
- SOC Analysts = operators of SIEM, ensuring real threats are caught and false alarms reduced.  
- Together, SIEM + SOC analysts = strong defense against cyber attacks.  
---

# How SIEM Works 

## 1) Log Ingestion
- SIEM collects **security-related logs** from endpoints, servers, and network devices.
- Logs come into SIEM via:
  - **Agents / Forwarders**
  - **Syslog / Port Forwarding**
  - **Manual Upload**
- Once logs are ingested → SIEM **normalizes** them (same format) → then checks them against **rules**.



## 2) Dashboards
- Dashboards = the **visual summary** of what SIEM is seeing.
- Analysts spend most of their time looking at dashboards.
- SIEM provides **default dashboards** + option to create **custom dashboards**.

**Common Dashboard Information:**
- Alert highlights (important alerts of the day)
- System notifications
- Health alerts (server / SIEM health status)
- Failed login attempts
- Total events ingested count
- Rules triggered (which detection rules fired)
- Top domains visited
- Suspicious IP addresses or traffic spikes

**Why dashboards matter?**
- They turn raw logs into **actionable insights**.
- Helps analysts detect trends and quickly investigate.



## 3) Correlation Rules
- Rules = **logical conditions** applied to logs.
- When conditions are met → SIEM **raises an alert**.
- These rules allow **timely detection** of suspicious activity.

**Examples of Correlation Rules:**
1. User has **5 failed login attempts** within 10 seconds → Alert "Multiple Failed Logins".
2. A successful login happens **after multiple failed logins** → Alert "Suspicious Successful Login".
3. A USB is plugged in (when policy says USB not allowed) → Alert "USB Inserted".
4. Outbound traffic > 25 MB → Alert "Possible Data Exfiltration".



## 4) Rule Creation — Use Cases

### Use Case 1: Log Clearing
- **Why important?** Attackers often clear logs after hacking to hide their tracks.
- Windows Event ID `104` = Log Cleared.
- **Rule:**  
  If Log Source = WinEventLog **AND** EventID = 104  
  → Trigger Alert: **"Event Log Cleared"**



### Use Case 2: Suspicious Command Execution (whoami)
- Attackers use `whoami` after privilege escalation.
- Event ID `4688` = New process creation in Windows.
- **Rule:**  
  If Log Source = WinEventLog **AND** EventCode = 4688 **AND** NewProcessName contains "whoami"  
  → Trigger Alert: **"WHOAMI Command Execution Detected"**



## 5) Importance of Normalized Logs
- Normalization = making all logs follow the same field-value structure.
- Example fields: `EventID`, `NewProcessName`, `SourceIP`, `DestinationPort`.
- Rules depend on field-value pairs → without normalization, correlation won’t work.



## 6) Alert Investigation
When a rule triggers an alert, analysts investigate it step by step:

### Investigation Flow:
1. **Check the dashboard** → See alert details.
2. **Review associated logs/flows** → Which rule condition was met?
3. **Decide outcome:**
   - **False Positive**: Not malicious.  
     → Fix by **tuning rule** to avoid noise in future.
   - **True Positive**: Real threat.  
     → Perform deeper investigation.

### Actions after Confirmed Threat:
- Contact the **asset owner** to check activity.
- **Isolate infected host** from network.
- **Block suspicious IP address** or domain.
- Escalate incident to higher-level team if needed.
- Document case for compliance and reporting.



## 7) Summary
- SIEM ingests logs → normalizes them → applies correlation rules.  
- Dashboards provide a summarized, real-time view of network activity.  
- Correlation rules detect suspicious behavior (e.g., failed logins, log clearing, unusual traffic).  
- Analysts investigate alerts, reduce false positives, and take action when a true attack is detected.  
- SIEM + Analysts = faster detection, better visibility, and timely response.  

