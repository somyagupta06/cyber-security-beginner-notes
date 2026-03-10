# Alert Triage Using Elastik
---
# Lab Questions
## Q.1 How many logs are available for analysis within the entire time range?
<img width="1470" height="956" alt="Screenshot 2026-03-10 at 11 52 55 AM" src="https://github.com/user-attachments/assets/b47e8930-6c9a-433c-8c71-b0c4f659fe75" />

## Q.2 What is the field value for the client.ip in the weblogs index?
<img width="1470" height="956" alt="Screenshot 2026-03-10 at 11 52 41 AM" src="https://github.com/user-attachments/assets/bf6e26c1-4ad5-4341-a2a8-021147519de7" />
## Q.3 How many POST requests did the IP address 203.0.113.55 make to proxyLogon.ecp?

<img width="1470" height="956" alt="Screenshot 2026-03-10 at 11 55 43 AM" src="https://github.com/user-attachments/assets/41636ec8-a24e-456c-9ced-9fad37fc42b2" />
## Q.4 Which user.agent paired with the IP address 203.0.113.55 made the POST requests?
<img width="1470" height="956" alt="Screenshot 2026-03-10 at 11 56 20 AM" src="https://github.com/user-attachments/assets/bea17543-e588-4de7-aab7-f986d990f2a3" />
## Q.5 How many logs contain the cmd= query parameter in the url.path field?
<img width="1470" height="956" alt="Screenshot 2026-03-10 at 11 58 51 AM" src="https://github.com/user-attachments/assets/80c71917-2724-40ae-9bd8-386a1776f85e" />
## Q.6 Which command was run utilizing errorEE.aspx on Jul 20, 2025 @ 04:45:50.000?
<img width="1470" height="956" alt="Screenshot 2026-03-10 at 12 06 14 PM" src="https://github.com/user-attachments/assets/3ecbab8f-00f4-4620-8f1b-b30ff22f1b6f" />
## Q.7 What is the winlog.record_id of the Administrator 4624 logon event?
<img width="1470" height="956" alt="Screenshot 2026-03-10 at 12 10 02 PM" src="https://github.com/user-attachments/assets/e353d2a7-da36-478e-b705-0e92c3989ff4" />
## Q.8 What is the process.pid of the Sysmon 1 event that occurred on Jul 20, 2025 @ 05:11:27.996?
<img width="1470" height="956" alt="Screenshot 2026-03-10 at 12 12 01 PM" src="https://github.com/user-attachments/assets/f2aa7539-267c-4cc5-a673-1dbd076a733c" />
## Q.9 What is the winlog.event_id for the new user account being created?
<img width="1470" height="956" alt="Screenshot 2026-03-10 at 12 16 16 PM" src="https://github.com/user-attachments/assets/b2bd214e-3034-4267-a517-f12f68f3caea" />
## Q.10 What is the name of the new user account?
<img width="1470" height="956" alt="Screenshot 2026-03-10 at 12 15 40 PM" src="https://github.com/user-attachments/assets/31d5dd65-0b96-4fc7-bb1b-16ce8ac4dfa8" />
## Q.11 What command does the attacker use to add the new account to the "Remote Desktop Users" group?
<img width="1470" height="956" alt="Screenshot 2026-03-10 at 12 27 39 PM" src="https://github.com/user-attachments/assets/c35bbf11-1ee8-400d-9371-5076071675c2" />
## Q.12 What is the winlog.record_id of the 4732 Security event when the attacker adds the user to the Administrator group?
<img width="1470" height="956" alt="Screenshot 2026-03-10 at 12 29 58 PM" src="https://github.com/user-attachments/assets/1604ee4a-02cd-4e0b-be57-89b25f8d891b" />
## Q.13 What PowerShell command did the attacker run on Jul 20, 2025 @ 05:16:14.628?
<img width="1470" height="956" alt="Screenshot 2026-03-10 at 12 31 32 PM" src="https://github.com/user-attachments/assets/7ff8dc84-fb4d-439c-94f2-d4ba2d92d3d8" />
## Q.14 What is the name of the archive that the attacker creates using the Rar.exe executable?
<img width="1470" height="956" alt="Screenshot 2026-03-10 at 12 36 29 PM" src="https://github.com/user-attachments/assets/28e5d9f8-ee76-49da-80ed-110e65435961" />

---
# SOC Investigation Notes

Imagine you are a **SOC Analyst**.  
Your job is to **watch logs and alerts** and find out if someone is attacking the system.

In this investigation we analyze **web logs, Windows logs, Sysmon logs, and PowerShell logs** to understand what an attacker did.

---

# 1. What is a SOC Analyst?

SOC = Security Operations Center

A SOC analyst:
- Monitors security alerts
- Investigates suspicious activity
- Finds attackers inside systems
- Creates an attack timeline

Think of a SOC analyst like a **security guard for computers**.

---

# 2. Tools Used in This Investigation

We use **Kibana (Elastic SIEM)**.

SIEM means:
Security Information and Event Management

It collects logs from many sources like:

| Source | What it Shows |
|------|------|
Web logs | Website activity |
Windows logs | User logins and system activity |
Sysmon logs | Process activity |
PowerShell logs | Commands executed |

---

# 3. First Step – Looking at Web Logs

First we check **IIS web server logs**.

We filter logs using this query:

_index:weblogs

This shows only **web server requests**.

From the alert we see a suspicious IP:

203.0.113.55

So we search for requests from this IP.

Query:

_index:weblogs and client.ip:203.0.113.55 and http.request.method:POST

Important fields to view:

| Field | Meaning |
|------|------|
client.ip | Attacker IP address |
user.agent | Tool used by attacker |
http.request.method | Request type (GET/POST) |
url.path | Page requested |
http.response.status_code | Server response |

We see requests to:

proxyLogon.ecp

This is related to **ProxyLogon vulnerability** in Microsoft Exchange.

This means the attacker tried to **exploit the web server**.

SOC conclusion:
This alert is a **True Positive**.

---

# 4. Detecting a Web Shell

Next alert shows **GET requests with cmd= parameter**.

Query:

_index:weblogs and client.ip:203.0.113.55 and http.request.method:GET and errorEE.aspx

Example URLs seen in logs:

errorEE.aspx?cmd=whoami  
errorEE.aspx?cmd=ipconfig  

This means a **web shell is installed**.

What is a web shell?

A web shell is a **hidden webpage that lets attackers run commands on the server**.

Example command:

cmd=whoami

This tells the attacker which user account is running.

SOC conclusion:
The attacker has **remote command execution on the server**.

---

# 5. Checking Administrator Login

Next alert says:

Administrator login happened outside business hours.

We check Windows logins.

Important Event ID:

4624

Meaning:
Successful logon.

Query:

@timestamp >= "2025-07-20T05:11:22" and winlog.event_id:4624 and host.name:winserv2019.some.corp and winlog.event_data.TargetUserName:Administrator

Important fields:

| Field | Meaning |
|------|------|
winlog.event_id | Event type |
host.name | Computer name |
TargetUserName | User account |
logon.type | Login method |
IpAddress | Source IP |

Result:

Administrator logged in from:

203.0.113.55

Same attacker IP.

SOC conclusion:
The attacker **logged in as Administrator**.

---

# 6. Checking Processes with Sysmon

Sysmon helps us see **process creation**.

Important Event ID:

1 = Process Creation

Query:

@timestamp >= "2025-07-20T05:11:22" and winlog.event_id:1 and user.name:Administrator

Important fields:

| Field | Meaning |
|------|------|
user.name | User running the process |
process.parent.name | Parent process |
process.command_line | Full command |

Example processes:

explorer.exe  
cmd.exe  

This shows the attacker started **Command Prompt**.

---

# 7. Detecting New User Creation

Next alert says a new account was created.

Query:

@timestamp >= "2025-07-20T05:13:10.000" and winlog.channel:Security and winlog.task:"User Account Management"

Example activity:

A new user account was created.

Typical attacker command:

net user hacker password123 /add

Why attackers do this:

To create a **backdoor account** so they can come back later.

SOC conclusion:
Attacker created a **new user account**.

---

# 8. Privilege Escalation with CMD

Next alert shows suspicious command line activity.

Query:

@timestamp >= "2025-07-20T05:13:15" and process.parent.name:cmd.exe and user.name:Administrator

Example commands:

net user hacker /add  
net localgroup administrators hacker /add  

Meaning:

Step 1: Create new user  
Step 2: Add user to administrators group

SOC conclusion:
Attacker gave **admin privileges to the new account**.

---

# 9. Security Group Changes

We confirm group changes using Event ID:

4732

Meaning:
User added to security group.

Query:

@timestamp >= "2025-07-20T05:13:15" and (winlog.event_id:4732 or process.parent.name:cmd.exe)

This confirms the attacker added the new user to **Administrators group**.

---

# 10. PowerShell Commands

Attackers often run **discovery commands**.

Query:

@timestamp >= "2025-07-20T05:13:15" and event.module:powershell and event.code:4104

Event 4104 means:

PowerShell Script Block Logging.

Commands seen:

whoami  
whoami /priv  

Purpose:

| Command | Meaning |
|------|------|
whoami | Shows current user |
whoami /priv | Shows privileges |

Attackers use these commands to **learn about the system**.

---

# 11. Suspicious Use of RAR Tool

We also search for:

process.name: "Rar.exe"

RAR is a file compression tool.

Attackers sometimes use it to:

- compress stolen data
- prepare files for exfiltration

Example scenario:

Important files → compressed into archive → sent outside network.

SOC conclusion:
Possible **data exfiltration preparation**.

---

# 12. Full Attack Timeline

The attack happened in this order:

1. Attacker exploited Exchange server (ProxyLogon).
2. Web shell was uploaded.
3. Attacker executed commands using web shell.
4. Attacker logged in as Administrator.
5. Command Prompt was used.
6. New user account was created.
7. User added to Administrators group.
8. PowerShell discovery commands executed.
9. Files compressed using RAR.

---

# 13. Final SOC Verdict

This activity is **malicious**.

Reason:

- Exploited web application
- Installed web shell
- Logged in as Administrator
- Created backdoor user
- Escalated privileges
- Ran discovery commands
- Possibly prepared data for exfiltration

Final classification:

TRUE POSITIVE SECURITY INCIDENT

---

# 14. Key Lessons for SOC Analysts

Always check:

1. Source IP addresses
2. Suspicious web requests
3. Login events
4. Process creation
5. User account creation
6. Privilege escalation
7. PowerShell activity
8. Data compression tools

By combining logs from many sources, we can **reconstruct the attacker’s actions step by step**.

