# SIEM Basics (SOC Analyst Notes – Very Easy English)

## What is SIEM?

SIEM stands for **Security Information and Event Management**.

A **SIEM** is a security system that **collects logs from many devices and shows them in one place** so that a **SOC analyst can detect attacks faster**.

Example SIEM tools:
- Splunk
- IBM QRadar
- Microsoft Sentinel

Think of SIEM like a **security control room** where all cameras (logs) send their video (data).

SOC analysts sit in this control room and watch everything.

---

# 1. Centralisation

## What is Centralisation?

Centralisation means **bringing all logs to one place**.

In an organisation, many systems create logs:

| System | Example |
|------|------|
| Firewall | Network traffic |
| Servers | System activity |
| Workstations | User actions |
| IDS/IPS | Intrusion alerts |
| Applications | App events |

Without SIEM, logs are **stored in different systems**.

With SIEM, all logs are **collected in one dashboard**.

---

## Example (SOC Investigation)

Two SOC analysts receive the same alert.

### Analyst Ted (Using SIEM)

Ted opens SIEM and can see:

- firewall logs
- endpoint logs
- network logs

All in **one dashboard**.

He quickly understands the situation.

### Analyst Emily (No SIEM)

Emily must:

1. Login to firewall
2. Login to endpoint system
3. Login to server logs

She spends **more time collecting data**.

---

### SOC Lesson

Centralisation helps analysts:

- investigate faster
- see the full picture
- reduce investigation time

---

# 2. Correlation

## What is Correlation?

Correlation means **connecting multiple events to understand the full attack story**.

One log alone may not explain the problem.

SOC analysts combine logs from **different sources**.

---

## Example

The SIEM generates an alert:

Alert: Internal network scanning detected  
Source IP: 192.168.1.15

This information alone is not enough.

The analyst correlates logs.

| Log Source | Information Found |
|------|------|
| IDS | Network scan detected |
| Windows Logs | User logged in |
| Sysmon | Process executed |
| DHCP Logs | Device name |

---

### Final Investigation Result

The analyst discovers:

User: John  
Machine: HR-PC  
Tool used: Nmap

Now the analyst decides:

- Is it normal activity?
- Or an attacker scanning the network?

---

### SOC Lesson

Correlation helps analysts:

- connect events
- understand attacks
- reduce false alerts

---

# 3. Historical Events

SIEM stores **past logs for investigation**.

This allows analysts to **look back in time**.

---

## Example

Alert appears:

User logged in from a new country.

The analyst checks **historical logs**.

| Question | Check |
|------|------|
| Did the user login from this location before? | Check past logs |
| Was this IP used earlier? | Check history |
| Is the behaviour normal? | Compare patterns |

---

### Investigation Result

Case 1:

User never logged in from that country.

Result: Suspicious activity.

Case 2:

User often travels and logs in from there.

Result: Normal behaviour.

---

### SOC Lesson

Historical logs help analysts:

- detect hidden attacks
- study user behaviour
- confirm suspicious activity

---

# 4. Log Sources in SIEM

SIEM collects logs from many systems.

The main log sources are:

1. Host logs
2. Network logs
3. Web logs

---

# 4.1 Host-Based Log Sources

Host logs come from **individual computers and servers**.

Examples:

- employee laptops
- servers
- workstations

These logs show **what is happening inside a system**.

---

## Examples of Host Logs

| Log Type | Information |
|------|------|
| Windows Event Logs | login activity |
| Sysmon Logs | process activity |
| Security Logs | authentication events |

---

### Example Event

A suspicious process runs on a machine.

The SOC analyst checks:

- which user started it
- which process executed
- when it happened

Host logs help detect:

- malware execution
- privilege escalation
- suspicious programs

---

# 4.2 Network-Based Log Sources

Network logs come from **network devices**.

Examples:

- Firewalls
- Routers
- IDS
- IPS

These logs show **how systems communicate on the network**.

---

## Example Information from Network Logs

| Information | Example |
|------|------|
| Source IP | 10.0.0.5 |
| Destination IP | 192.168.1.10 |
| Port | 445 |
| Protocol | TCP |

---

### What Analysts Detect

Network logs help detect:

- scanning activity
- lateral movement
- suspicious connections
- data exfiltration

---

# 4.3 Web-Based Log Sources

Web logs come from **web servers and applications**.

Examples:

- Apache
- Nginx
- Web applications

These logs show **user requests to websites**.

---

## Example Web Log Event

Request made to a website:

GET /login.php?id=' OR 1=1 --

This could indicate a **SQL Injection attempt**.

---

### SOC Lesson

Web logs help detect:

- web attacks
- login brute force
- injection attacks

---

# 5. Time Pitfalls in SIEM

A common challenge in SIEM analysis is **time differences**.

Logs may come from systems in **different time zones**.

Examples:

- UTC
- Local time
- Different regional settings

---

## Example Scenario

Analyst local time:

5:00 PM

SIEM log time:

1:00 PM

This does **not mean the log arrived late**.

It simply means the **time zones are different**.

---

### SOC Lesson

Always check:

- SIEM timezone
- log source timezone

Time understanding is important for **correct attack timelines**.

---

# 6. Log Normalisation

Different systems produce logs in **different formats**.

Examples:

| System | Log Format |
|------|------|
| Firewall | plain text |
| Windows | XML |
| Applications | JSON |

Each system uses **different field names**.

---

## Example

Firewall log:

src_ip=10.0.0.5

Windows log:

SourceNetworkAddress: 10.0.0.5

Both show the same information.

---

## After Normalisation

SIEM converts logs into a **common format**.

source_ip = 10.0.0.5

Now analysts can search easily.

---

### SOC Lesson

Normalisation helps analysts:

- search logs faster
- compare events
- correlate multiple sources

---

# Quick Revision (SOC Summary)

## Why SIEM is Important

SIEM helps SOC analysts:

- collect logs in one place
- detect attacks faster
- correlate events
- investigate historical data

---

## Main SIEM Benefits

1. Centralisation  
2. Correlation  
3. Historical analysis  
4. Faster investigations

---

## Main Log Sources

Host Logs  
Network Logs  
Web Logs

---

## Important Concepts

Time zone differences  
Log normalisation
---
# Windows Logs in SIEM (SOC Analyst Notes – Very Easy English)

When a SOC analyst investigates Windows activity in a SIEM, two very important log sources are used.

1. WinEventLogs  
2. Sysmon Logs

Both together give **better visibility of what happened on a system**.

Think of it like this:

WinEventLogs = basic system activity  
Sysmon = detailed activity tracking

When both are used together, investigation becomes much easier.

---

# 1. Sysmon (System Monitor)

Sysmon is a **special Windows monitoring tool** created by Microsoft.

It is **not installed by default**.  
It must be installed and configured manually.

Once installed, Sysmon records **very detailed system activity**.

---

## What Sysmon Can Detect

Sysmon logs can show many important activities.

| Activity | What it means |
|---|---|
| Process creation | Which program started |
| Network connections | Which program connected to the internet |
| File creation | New files created |
| Registry changes | Changes in Windows registry |
| Process injection | One process injecting into another |

This gives SOC analysts **deep visibility into system behaviour**.

---

# Important Sysmon Event Codes

| EventCode | Activity |
|---|---|
| 1 | Process creation |
| 3 | Network connection |
| 7 | Image loaded |
| 11 | File created |
| 13 | Registry modification |

SOC analysts often search these events in SIEM.

---

# Example 1: Malicious Process Execution

A SOC alert says:

Suspicious encoded PowerShell command detected.

Encoded PowerShell commands are often used by attackers to hide malicious commands.

The analyst searches Sysmon logs.

Search example in Splunk:

index=winenv EventCode=1 powershell EncodedCommand

This search looks for:

- EventCode 1 (process creation)
- PowerShell
- EncodedCommand argument

---

## Investigation Result

The analyst discovers the following activity:

| Field | Information |
|---|---|
| Host | WINHOST05 |
| Parent process | cmd.exe |
| Process started | powershell.exe |
| File executed | update_config.js |
| File location | C:\Users\Public |

---

### What happened?

1. A malicious file executed.
2. That file started cmd.exe.
3. cmd.exe launched PowerShell.
4. PowerShell used an encoded command.

This is a **strong indicator of malware execution**.

---

# Example 2: Suspicious Network Connection

After a few minutes, another alert appears.

The alert says a **suspicious network connection** occurred.

The analyst now checks Sysmon network logs.

Search example:

index=winenv EventCode=3 ComputerName=WINHOST05

EventCode 3 shows **network connections created by processes**.

---

## Investigation Result

The analyst finds this information.

| Field | Information |
|---|---|
| Process | PPn423.exe |
| File location | Temp folder |
| Destination IP | 83.222.191.2 |
| Destination Port | 9999 |
| Protocol | TCP |

---

### Why is this suspicious?

Reasons:

1. The process runs from a Temp folder.
2. The file name looks random.
3. The destination port 9999 is unusual.
4. Unknown external IP address.

The analyst should also check the IP in **Threat Intelligence platforms**.

This may indicate **command and control communication**.

---

# 2. WinEventLogs

Windows Event Logs are **default logs created by Windows**.

They record **many system activities automatically**.

Windows actually has **200+ log channels**, but the most important ones are:

| Log Type | Purpose |
|---|---|
| Security | Security related events |
| System | Operating system activity |
| Application | Application events |

SOC analysts mainly focus on **Security and System logs**.

---

# Windows Security Logs

Security logs record **important security events**.

These logs help analysts detect suspicious activity.

---

## What Security Logs Show

| Activity | Example |
|---|---|
| User login attempts | Successful or failed login |
| Account creation | New user account created |
| Account modification | Permissions changed |
| File access | Access to files |
| Policy changes | Security settings modified |

---

# Example: Attacker Creates New Account

A SOC analyst receives a new alert.

Alert: A new user account was created on WINHOST05.

This may mean the attacker is trying to create **persistence**.

Persistence means **keeping access to the system even after reboot**.

---

## Search Example

index=winenv EventCode=4720 OR EventCode=4722

Important event codes:

| EventCode | Meaning |
|---|---|
| 4720 | New user account created |
| 4722 | User account enabled |

---

## Investigation Result

| Field | Information |
|---|---|
| Host | WINHOST05 |
| Created by | ted-admin |
| New account | backup-user |

---

### What this means

The attacker used the **ted-admin account** to create a **backup user account**.

This allows the attacker to **return later to the system**.

This is a **persistence technique**.

The SOC L1 analyst should escalate this to **SOC L2**.

---

# Windows System Logs

System logs record **events related to the operating system**.

They track things like:

- services
- drivers
- system errors
- system startup

These logs help detect **persistence and privilege escalation**.

---

# Important System Log Event Codes

| EventCode | Meaning |
|---|---|
| 7045 | New service created |
| 7036 | Service started or stopped |

Attackers sometimes create **malicious services** to maintain access.

---

# Example: Malicious Service

The analyst investigates the compromised host WINHOST05.

Search example:

index=winenv EventCode=7045 OR EventCode=7036 ComputerName=WINHOST05

---

## Investigation Result

| Field | Information |
|---|---|
| Service name | User Updates |
| Executable | RNSfnsjdf.exe |
| File location | Temp folder |
| Account used | SYSTEM |

---

### Why is this suspicious?

Reasons:

1. The service name looks fake.
2. The executable runs from the Temp folder.
3. The service runs as SYSTEM.
4. The file name looks random.

This suggests the attacker created a **malicious service for privilege escalation**.

---

# Quick Comparison

| Feature | Sysmon | WinEventLogs |
|---|---|---|
| Installation | Must be installed manually | Default in Windows |
| Visibility | Very detailed | Basic system events |
| Network tracking | Yes | Limited |
| Process tracking | Very detailed | Limited |

---

# SOC Analyst Quick Summary

## Sysmon

Used to detect:

- malicious processes
- suspicious network connections
- file creation
- registry changes

Important EventCodes:

1 – Process creation  
3 – Network connection  

---

## WinEventLogs

Used to detect:

- user logins
- account creation
- service creation
- system events

Important EventCodes:

4720 – User created  
4722 – User enabled  
7045 – Service created  
7036 – Service started or stopped

---
# Lab Questions
## Q.1 Which IP address was the connection established with?
<img width="1470" height="956" alt="Screenshot 2026-03-08 at 12 52 13 AM" src="https://github.com/user-attachments/assets/29a6656a-56b9-45c1-811e-fd3184192b61" />
## Q.2 Which process initiated this suspicious connection?
<img width="1470" height="956" alt="Screenshot 2026-03-08 at 12 52 49 AM" src="https://github.com/user-attachments/assets/8c1163af-e9ae-4822-8e85-2d6fd2f611cb" />
## Q.3 What is the MD5 hash of the malicious process from the previous question?
<img width="1470" height="956" alt="Screenshot 2026-03-08 at 1 01 38 AM" src="https://github.com/user-attachments/assets/df1ec168-2ea8-4721-b71e-4e4ba7162506" />
## Q.4 What is the name of the scheduled task that was created on the system?
<img width="1470" height="956" alt="Screenshot 2026-03-08 at 1 02 01 AM" src="https://github.com/user-attachments/assets/ef89845f-eb10-4b1d-a102-7f012920003d" />


# Investigation Flow in SOC

Typical SOC investigation using these logs:

1. Alert detected in SIEM
2. Check Sysmon logs for process activity
3. Check network connections
4. Check Security logs for account activity
5. Check System logs for service creation
6. Build full attack timeline
7. Escalate to SOC L2 if malicious activity confirmed
---

# Linux Logs in SIEM (SOC L1 Analyst Notes – Very Simple)

When analysing **Linux systems in a SIEM**, SOC analysts mainly use two important log sources:

1. **auth.log**
2. **syslog**

Both logs help analysts understand **user activity and system behaviour**.

Simple idea:

auth.log → user login activity  
syslog → system activity

Using both together helps analysts detect attacks.

---

# 1. auth.log (Authentication Logs)

auth.log records **authentication related activities**.

These logs help SOC analysts see **who is accessing the system and how**.

---

## What auth.log Shows

| Activity | Meaning |
|---|---|
| SSH login attempts | Users trying to login |
| Failed logins | Incorrect password attempts |
| Successful logins | Correct login |
| sudo usage | User running admin commands |
| su usage | Switching users (example: user → root) |

These logs help detect **unauthorised access attempts**.

---

# Example 1 – Unusual SSH Login

SOC alert:

Unusual SSH login detected for user **ubuntu**.

The analyst searches **auth.log** in SIEM.

Search idea:

- look for ssh login events
- check failed and successful login attempts

Important log messages:

Accepted password → successful login  
Failed password → login failed

---

## Investigation Result

Logs show:

| Event | Meaning |
|---|---|
| Many failed password attempts | attacker guessing passwords |
| Successful login afterwards | attacker guessed correct password |

This indicates a **Brute Force Attack**.

Brute force attack means:

An attacker tries **many passwords until one works**.

---

### SOC Action

SOC L1 analyst should:

- confirm the activity
- escalate the case to **SOC L2**

---

# Example 2 – Privilege Escalation

After login, attackers usually try to gain **root access**.

Root is the **highest privilege account in Linux**.

Attackers need root access to:

- control the system
- access sensitive files
- install malware

---

## Detecting Privilege Escalation

Analysts search auth.log for **su command activity**.

su command means:

Switch user

Example:

ubuntu → root

---

## Investigation Result

Logs show the attacker switched from:

ubuntu → root

This indicates **privilege escalation**.

However, auth.log only shows **that the switch happened**.

It does not show **how the root password was obtained**.

Other logs would be needed for that.

---

# 2. syslog (System Logs)

syslog records **general system activity**.

These logs help analysts understand **what the system is doing over time**.

---

## What syslog Shows

| Activity | Example |
|---|---|
| Service activity | services starting or stopping |
| System events | errors or system messages |
| Cron jobs | scheduled tasks |
| Background processes | scripts running automatically |

These logs help analysts detect **persistence and abnormal system behaviour**.

---

# Example – Persistence Using Cron

Attackers often create **cron jobs**.

Cron jobs are **scheduled tasks that run automatically**.

Example:

Run a script every 5 minutes.

If an attacker creates such a job, their malware **keeps running automatically**.

---

## Investigation

The analyst searches syslog for:

- cron activity
- suspicious scripts

---

## Investigation Result

Logs show:

A script named:

pnr5433sw.sh

Location:

/tmp directory

Execution pattern:

Runs every **5 minutes via cron job**

---

### Why This is Suspicious

Reasons:

1. File name looks random
2. File runs from `/tmp` directory
3. Script runs repeatedly
4. Created through cron job

This indicates **persistence mechanism**.

Persistence means:

The attacker ensures their access **remains active even after reboot**.

---

# Example – Reverse Shell Activity

Logs also show signs of a **Perl reverse shell**.

A reverse shell allows the system to **connect back to the attacker’s machine**.

Example connection detected:

| Field | Value |
|---|---|
| Destination IP | 10.10.101.12 |
| Port | 9999 |

This may indicate **command and control communication**.

---

# Quick Comparison

| Log Source | What It Shows |
|---|---|
| auth.log | login attempts and privilege usage |
| syslog | system behaviour and background activity |

---

# SOC L1 Investigation Flow

Typical investigation steps:

1. Alert detected in SIEM
2. Check auth.log for login activity
3. Detect brute force attempts
4. Check privilege escalation (su or sudo)
5. Review syslog for persistence activity
6. Identify cron jobs or reverse shells
7. Escalate incident to SOC L2

---

# Quick Revision

## auth.log

Used to detect:

- SSH login attempts
- brute force attacks
- privilege escalation
- sudo and su activity

---

## syslog

Used to detect:

- cron jobs
- system services
- background scripts
- persistence mechanisms

---
# Lab Questions
## Q.1 What was the timestamp of the remote-ssh account creation?
<img width="1470" height="956" alt="Screenshot 2026-03-09 at 1 31 00 PM" src="https://github.com/user-attachments/assets/0e0cfa4e-6b26-4869-a3fe-1337bf637b94" />

## Q.2 Which user successfully escalated their privileges to root prior to the action from the first question?
<img width="1470" height="956" alt="Screenshot 2026-03-09 at 1 31 17 PM" src="https://github.com/user-attachments/assets/2f028f0a-0eb8-4a53-aa80-51f04ba79ff2" />


## Q.3 From which IP address did the user from the previous question successfully log in to the system?
<img width="1470" height="956" alt="Screenshot 2026-03-09 at 1 30 37 PM" src="https://github.com/user-attachments/assets/6a5e0bbb-d6c5-439e-bb22-74ca1c4a0ac9" />

## Q.4 How many failed login attempts occurred prior to this successful login?
<img width="1470" height="956" alt="Screenshot 2026-03-09 at 1 35 10 PM" src="https://github.com/user-attachments/assets/3d7008c5-ede3-4d2a-aae2-6f0abb215e0a" />
## Q.5 Which port is the persistence mechanism configured to connect to?
<img width="1470" height="956" alt="Screenshot 2026-03-09 at 1 36 50 PM" src="https://github.com/user-attachments/assets/7a48c181-f130-4deb-b270-e653356920de" />


---

# Key Security Indicators

Brute force attack  
Privilege escalation  
Cron-based persistence  
Reverse shell connection
---
# Web Logs in SIEM (SOC L1 Analyst Notes – Very Simple)

Many organisations run websites.  
These websites use **web servers** such as:

- Apache
- Nginx

These servers generate **web logs** when users access the website.

SIEM collects these logs so that **SOC analysts can detect web attacks**.

The most useful logs for investigation are:

1. Access Logs
2. Error Logs

---

# 1. Access Logs

Access logs record **all requests made to the website**.

These logs help analysts see:

- who accessed the website
- which page was requested
- how the server responded

---

## Common Fields in Web Access Logs

| Field | Meaning |
|---|---|
| clientip | IP address of the user |
| method | HTTP request type (GET or POST) |
| uri_path | page or resource requested |
| status | server response code |
| user-agent | browser or tool used |

---

# Important HTTP Status Codes

| Status Code | Meaning |
|---|---|
| 200 | request successful |
| 404 | page not found |
| 500 | server error |
| 503 | server overloaded |

SOC analysts use these codes to understand **server behaviour and attacks**.

---

# Example 1 – WordPress Brute Force Attack

SOC alert:

Possible brute force attack on **WordPress login page**.

WordPress login page:

/wp-login.php

Brute force attack means:

An attacker tries **many passwords until one works**.

---

## Investigation Logic

SOC analyst checks:

- POST requests
- login page `/wp-login.php`
- high number of requests from the same IP

Login forms usually send data using **POST requests**.

---

## Investigation Result

Logs show:

| Field | Value |
|---|---|
| Client IP | 167.172.41.141 |
| Requests | 160 requests |
| Target page | /wp-login.php |
| User-Agent | Hydra |

---

## Why This is Suspicious

Hydra is a **popular brute-force tool**.

This indicates:

Possible **automated password attack** on the WordPress login page.

---

# Example 2 – Possible Web Shell

SOC receives another alert about **possible web shell activity**.

---

## What is a Web Shell

A web shell is a **malicious script uploaded to a web server**.

It allows attackers to:

- run commands
- access files
- control the server

Common web shell file types:

| File Type |
|---|
| .php |
| .asp |
| .aspx |
| .jsp |
| .exe |

---

## Investigation Logic

SOC analyst searches for:

- executable script files
- GET or POST requests
- successful responses (status = 200)

Web shells usually receive **multiple suspicious requests**.

---

## Investigation Result

Logs show a suspicious file:

505.php

Example data:

| Field | Value |
|---|---|
| File | 505.php |
| Status | 200 |
| Requests | multiple |
| Client IP | suspicious source |

---

## Why This is Suspicious

Reasons:

- unusual PHP file
- random file name
- multiple requests
- successful execution

This could indicate **web shell activity**.

SOC analyst should investigate the file further.

---

# Example 3 – DDoS Attack

SOC receives an alert about a **possible DDoS attack**.

---

## What is a DDoS Attack

DDoS stands for:

Distributed Denial of Service.

Attackers send **huge numbers of requests** to overload the server.

Result:

- server becomes slow
- website becomes unavailable

---

## Investigation Logic

SOC analyst checks:

- status code = 503
- very high number of requests
- short time window
- source IP patterns

Status code 503 means:

The server is **overloaded or unavailable**.

---

## Investigation Result

Logs show:

| Field | Value |
|---|---|
| Requests | 1.5 million |
| Time window | 10 minutes |
| Status | 503 |
| Target resource | web server |

---

## Conclusion

This pattern strongly indicates a **possible DDoS attack**.

---

# Quick Comparison

| Attack Type | Detection Method |
|---|---|
| Brute Force | many login attempts to login page |
| Web Shell | suspicious script requests |
| DDoS | extremely high request volume |

---

# SOC Investigation Flow (Web Logs)

1. Receive alert in SIEM
2. Check web access logs
3. Identify suspicious IP addresses
4. Analyse request methods (GET / POST)
5. Review status codes
6. Check user-agent strings
7. Detect attack patterns

---

# Quick Revision

## Web Logs Help Detect

- brute force attacks
- web shell activity
- DDoS attacks
- scanning behaviour
- web exploitation attempts

---

## Important Indicators

Many POST requests → possible brute force

Suspicious script files → possible web shell

Huge request count + status 503 → possible DDoS


---
# Lab Questions

## Q.1 Which URI path had the highest number of requests?

<img width="1470" height="956" alt="Screenshot 2026-03-09 at 2 05 44 PM" src="https://github.com/user-attachments/assets/61556390-8d50-4573-bc8e-a3878353f258" />

## Q.2 Which IP address was the source of the activity?
<img width="1470" height="956" alt="Screenshot 2026-03-09 at 2 07 45 PM" src="https://github.com/user-attachments/assets/17952717-a8a2-46fd-9982-ad9466698416" />

## Q.3 How can this activity be classified?
Brute Force

## Q.4 Which tool did the threat actor use?
<img width="1470" height="956" alt="Screenshot 2026-03-09 at 2 09 13 PM" src="https://github.com/user-attachments/assets/0c36e7e4-b87e-4b62-8caf-d4591d8becec" />

