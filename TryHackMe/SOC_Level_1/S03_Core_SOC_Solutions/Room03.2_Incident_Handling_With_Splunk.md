# Incident Handling Life Cycle 

As a SOC Analyst, our main goal is to:
- Understand how the attacker attacked
- Stop the attack
- Make sure it does not happen again

The Incident Handling Life Cycle has **4 main phases**.

----------------------------------
1) PREPARATION
----------------------------------

This phase is about being **ready before an attack**.

### What happens here:
- Create security policies
- Decide rules and procedures
- Install security tools like:
  SIEM
  EDR
  IDS
  IPS
- Train security staff
- Prepare incident response plans

### Simple meaning:
Be ready **before** the attack happens.

----------------------------------
2) DETECTION AND ANALYSIS
----------------------------------

This phase is about **finding and understanding the attack**.

### What happens here:
- Get alerts from SIEM or EDR
- Analyze logs and alerts
- Find how the attack started
- Identify attacker actions
- Perform threat hunting

### Simple meaning:
Find the attack and understand it deeply.

----------------------------------
3) CONTAINMENT, ERADICATION, RECOVERY
----------------------------------

This phase is about **stopping and fixing the attack**.

### What happens here:
- Stop the attack from spreading
- Isolate infected systems
- Remove malware or backdoors
- Fix vulnerabilities
- Restore normal operations

### Simple meaning:
Stop, clean, and recover.

----------------------------------
4) POST-INCIDENT ACTIVITY (LESSONS LEARNED)
----------------------------------

This phase is about **learning from mistakes**.

### What happens here:
- Find security weaknesses
- Improve security controls
- Add new detection rules
- Update policies
- Train staff again if needed

### Simple meaning:
Learn so it does not happen again.

----------------------------------
ONE LINE REVISION (4 PHASES)
----------------------------------

Prepare → Detect → Fix → Improve

----------------------------------
CYBER KILL CHAIN
----------------------------------

During investigation, we map attacker activity to
the **Cyber Kill Chain**.

### 7 Phases of Cyber Kill Chain:
1. Reconnaissance
2. Weaponization
3. Delivery
4. Exploitation
5. Installation
6. Command & Control
7. Actions on Objectives

### Important Note:
- We do NOT need to follow the order
- One clue can lead to another phase

----------------------------------
SCENARIO OVERVIEW
----------------------------------

Organization:
:contentReference[oaicite:1]{index=1}

Attack Type:
Website defacement

Affected Website:
http://www.imreallynotbatman.com

Attack Result:
Website shows message:
"YOUR SITE HAS BEEN DEFACED"

----------------------------------
OUR ROLE
----------------------------------

We are working as **Security Analysts**.

Our task:
- Investigate the attack
- Find how attacker entered
- Track attacker activity
- Map actions to Cyber Kill Chain

This investigation belongs to:
**Detection and Analysis phase**

----------------------------------
SIEM USED
----------------------------------

SIEM Tool:
:contentReference[oaicite:2]{index=2}

Splunk collects logs from:
- Web server
- Firewall
- IDS
- Host machines

----------------------------------
IMPORTANT LOG SOURCES
----------------------------------

All logs are stored in:
index=botsv1

Key log sources:

- wineventlog  
  Windows event logs

- winRegistry  
  Registry changes

- XmlWinEventLog  
  Sysmon logs (very important)

- fortigate_utm  
  Firewall logs

- iis  
  Web server logs

- Nessus:scan  
  Vulnerability scan results

- Suricata  
  IDS alerts (very important)

- stream:http  
  HTTP network traffic

- stream:dns  
  DNS network traffic

- stream:icmp  
  ICMP traffic

----------------------------------
IMPORTANT TIP
----------------------------------

Use **Data Summary tab** in Splunk to:
- See all hosts
- See log sources
- Understand visibility

----------------------------------
ONE LINE MEMORY TRICK
----------------------------------

Incident Life Cycle:
P D C L
Prepare
Detect
Contain
Learn

----------------------------------
# Reconnaissance & Exploitation Phase

This investigation is done using
:contentReference[oaicite:1]{index=1}
during the **Detection and Analysis** phase.

----------------------------------
RECONNAISSANCE PHASE
----------------------------------

## What is Reconnaissance?

Reconnaissance means **collecting information about the target**.

Attackers try to learn:
- Website details
- Server IP
- Technologies used
- Login pages
- Employee or system information

----------------------------------
FIRST STEP AS AN ANALYST
----------------------------------

We first check:
- Which logs contain web traffic
- Who is sending requests to our website

Target website:
imreallynotbatman.com

----------------------------------
INITIAL SEARCH
----------------------------------

Search Query:
index=botsv1 imreallynotbatman.com

### Why this query?
- index=botsv1 contains all logs
- imreallynotbatman.com finds related events

----------------------------------
LOG SOURCES FOUND
----------------------------------

From the results, we found these log sources:
- Suricata (IDS alerts)
- stream:http (HTTP traffic)
- fortigate_utm (Firewall)
- iis (Web server logs)

----------------------------------
FOCUS ON HTTP TRAFFIC
----------------------------------

To see incoming web requests, we check HTTP logs.

Search Query:
index=botsv1 imreallynotbatman.com sourcetype=stream:http

----------------------------------
SOURCE IP ANALYSIS
----------------------------------

From the field src_ip, we found two IP addresses:
- 40.80.148.42
- 23.22.63.114

The IP **40.80.148.42** generated most traffic.
This looks suspicious.

----------------------------------
CONFIRM SCANNING ACTIVITY
----------------------------------

We checked IDS alerts using Suricata logs.

Search Query:
index=botsv1 imreallynotbatman.com src=40.80.148.42 sourcetype=suricata

### Finding:
- Alerts confirmed scanning behavior
- Scanner used: Acunetix (web vulnerability scanner)

----------------------------------
RECONNAISSANCE CONCLUSION
----------------------------------

- Attacker IP: 40.80.148.42
- Activity: Web scanning
- Target server IP: 192.168.250.70

----------------------------------
EXPLOITATION PHASE
----------------------------------

Now we check if the attacker tried to **break in**.

----------------------------------
COUNT REQUESTS BY IP
----------------------------------

Search Query:
index=botsv1 imreallynotbatman.com sourcetype=stream* 
| stats count(src_ip) as Requests by src_ip 
| sort - Requests

### Result:
Shows which IP sent the most requests.

----------------------------------
FOCUS ON WEB SERVER IP
----------------------------------

Search Query:
index=botsv1 sourcetype=stream:http dest_ip="192.168.250.70"

### Why?
To see all inbound traffic to the web server.

----------------------------------
HTTP METHOD ANALYSIS
----------------------------------

We observed many **POST** requests.

POST requests usually mean:
- Login attempts
- Form submissions

Search Query:
index=botsv1 sourcetype=stream:http 
dest_ip="192.168.250.70" 
http_method=POST

----------------------------------
IMPORTANT FIELDS
----------------------------------

Useful fields found:
- src_ip
- uri
- form_data
- http_user_agent

----------------------------------
JOOMLA ADMIN PANEL FOUND
----------------------------------

Logs showed the word **Joomla**.

This means the website uses:
:contentReference[oaicite:2]{index=2}

Joomla admin login page:
 /joomla/administrator/index.php

----------------------------------
CHECK ADMIN LOGIN TRAFFIC
----------------------------------

Search Query:
index=botsv1 imreallynotbatman.com 
sourcetype=stream:http 
dest_ip="192.168.250.70" 
uri="/joomla/administrator/index.php"

----------------------------------
BRUTE FORCE CONFIRMATION
----------------------------------

POST requests with form data showed:
- Same username: admin
- Many different passwords

This confirms a **brute-force attack**.

----------------------------------
EXTRACT USERNAME & PASSWORD
----------------------------------

Search Query:
index=botsv1 sourcetype=stream:http 
dest_ip="192.168.250.70" 
http_method=POST 
form_data=*username*passwd* 
| table _time uri src_ip dest_ip form_data

----------------------------------
REGEX PASSWORD EXTRACTION
----------------------------------

Regex used to extract passwords:
rex field=form_data "passwd=(?<creds>\w+)"

Search Query:
index=botsv1 sourcetype=stream:http 
dest_ip="192.168.250.70" 
http_method=POST 
form_data=*username*passwd* 
| rex field=form_data "passwd=(?<creds>\w+)" 
| table src_ip creds

----------------------------------
USER AGENT ANALYSIS
----------------------------------

Two user agents found:
- Python script → automated brute force
- Mozilla browser → manual login attempt

Final Table:
_time | src_ip | uri | http_user_agent | creds

----------------------------------
FINAL FINDINGS
----------------------------------

- IP 23.22.63.114  
  Automated brute-force attack using Python

- IP 40.80.148.42  
  One login attempt using browser
  Password tried: batman

----------------------------------
PHASE MAPPING
----------------------------------

- Reconnaissance:
  Scanning using Acunetix

- Exploitation:
  Brute-force on Joomla admin login

----------------------------------
ONE LINE REVISION
----------------------------------

Scan → Find admin page → Brute force → Attempt login

----------------------------------

# Installation & Actions on Objectives 
Investigation is done using
:contentReference[oaicite:1]{index=1}
under the **Detection and Analysis** phase.

----------------------------------
INSTALLATION PHASE
----------------------------------

## What is Installation Phase?

After breaking into a system, the attacker:
- Uploads malware or backdoor
- Installs tools for persistence
- Gains more control of the server

----------------------------------
WHAT WE ALREADY KNOW
----------------------------------

From previous phases:
- Web server was brute-forced
- Attacker got admin access
- Server IP: 192.168.250.70
- Attackers IPs identified earlier

Now we check:
Did attacker upload and execute files?

----------------------------------
SEARCH FOR UPLOADED FILES
----------------------------------

We first look for executable files.

Search Query:
index=botsv1 sourcetype=stream:http dest_ip="192.168.250.70" *.exe

----------------------------------
IMPORTANT FINDING
----------------------------------

Interesting field found:
part_filename{}

Files observed:
- 3791.exe
- agent.php

These files were uploaded to the server.

----------------------------------
CHECK ATTACKER IP
----------------------------------

We clicked on the file name to filter logs.

Search Query:
index=botsv1 sourcetype=stream:http 
dest_ip="192.168.250.70" 
"part_filename{}"="3791.exe"

Field checked:
c_ip (client IP)

This confirms the file came from attacker IP.

----------------------------------
WAS THE FILE EXECUTED?
----------------------------------

Uploading is not enough.
We must check execution.

Search Query:
index=botsv1 "3791.exe"

----------------------------------
HOST-CENTRIC LOG SOURCES FOUND
----------------------------------

Traces found in:
- Sysmon
- WinEventLog
- fortigate_utm

----------------------------------
CONFIRM EXECUTION USING SYSMON
----------------------------------

Sysmon EventCode=1 means:
Process Creation (execution)

Search Query:
index=botsv1 "3791.exe" 
sourcetype="XmlWinEventLog" 
EventCode=1

----------------------------------
INSTALLATION CONFIRMED
----------------------------------

- File 3791.exe was uploaded
- File was executed on server
- Server is fully compromised

----------------------------------
ACTIONS ON OBJECTIVES
----------------------------------

This phase shows:
What attacker actually wanted to do.

In this case:
Website defacement.

----------------------------------
CHECK SURICATA LOGS (INBOUND)
----------------------------------

Search Query:
index=botsv1 dest=192.168.250.70 sourcetype=suricata

Result:
No useful external traffic found.

----------------------------------
CHECK SURICATA LOGS (OUTBOUND)
----------------------------------

Web servers usually do NOT initiate traffic.
So outbound traffic is suspicious.

Search Query:
index=botsv1 src=192.168.250.70 sourcetype=suricata

----------------------------------
SUSPICIOUS OBSERVATION
----------------------------------

Server initiated traffic to:
- 23.22.63.114
- Other external IPs

Large traffic volume seen.

----------------------------------
PIVOT TO DESTINATION IP
----------------------------------

Search Query:
index=botsv1 
src=192.168.250.70 
sourcetype=suricata 
dest_ip=23.22.63.114

----------------------------------
INTERESTING FILE FOUND
----------------------------------

Files seen:
- PHP files
- One JPEG file

Suspicious file:
poisonivy-is-coming-for-you-batman.jpeg

----------------------------------
TRACE THE JPEG FILE
----------------------------------

Search Query:
index=botsv1 
url="/poisonivy-is-coming-for-you-batman.jpeg" 
dest_ip="192.168.250.70" 
| table _time src dest_ip http.hostname url

----------------------------------
FINAL FINDING
----------------------------------

- JPEG downloaded from attacker host
- Attacker host:
  prankglassinebracket.jumpingcrab.com
- Image used to deface website

----------------------------------
PHASE MAPPING
----------------------------------

Installation:
- Upload 3791.exe
- Execute malware

Actions on Objectives:
- Download defacement image
- Replace website content

----------------------------------
ONE LINE REVISION
----------------------------------

Upload malware → Execute → Deface website

----------------------------------

# Command & Control, Weaponization, Delivery – Easy Notes

Investigation is done using
:contentReference[oaicite:1]{index=1}
under the Detection and Analysis phase.

----------------------------------
COMMAND & CONTROL (C2)
----------------------------------

## What is Command & Control?
After compromise, the attacker:
- Connects to an external server
- Sends commands
- Downloads files
- Maintains control

Often done using:
- Dynamic DNS
- Suspicious domains

----------------------------------
FIND C2 COMMUNICATION
----------------------------------

We check network logs for traffic related to the defacement file.

Search Query:
index=botsv1 sourcetype=fortigate_utm "poisonivy-is-coming-for-you-batman.jpeg"

### Why firewall logs?
- Show src IP, dest IP, and URL
- Good for outbound traffic review

----------------------------------
IMPORTANT FINDING
----------------------------------

From firewall logs:
- URL field shows a full domain (FQDN)
- This domain is suspicious and attacker-controlled

----------------------------------
VERIFY USING HTTP LOGS
----------------------------------

Search Query:
index=botsv1 sourcetype=stream:http 
dest_ip=23.22.63.114 
"poisonivy-is-coming-for-you-batman.jpeg" 
src_ip=192.168.250.70

### Conclusion:
- Web server contacted attacker server
- Domain is acting as C2 server

----------------------------------
OPTIONAL DNS CONFIRMATION
----------------------------------

We can also check:
- stream:dns logs
- DNS queries from the server
to confirm Dynamic DNS usage

----------------------------------
WEAPONIZATION PHASE
----------------------------------

## What is Weaponization?
Attackers prepare tools BEFORE attack:
- Malware creation
- Fake or similar domains
- C2 infrastructure

----------------------------------
KNOWN ATTACKER DOMAIN
----------------------------------

Suspicious domain found:
prankglassinebracket.jumpingcrab.com

----------------------------------
OSINT: ROBTEX
----------------------------------

Tool:
:contentReference[oaicite:2]{index=2}

### What we do:
- Search the domain
- Find linked IP addresses
- Find related subdomains

Result:
- Domain resolves to attacker IPs
- Multiple related domains found

----------------------------------
OSINT: ROBTEX IP LOOKUP
----------------------------------

Search IP:
23.22.63.114

Finding:
- IP linked to domains similar to
  Wayne Enterprises brand

----------------------------------
OSINT: VIRUSTOTAL (IP)
----------------------------------

Tool:
:contentReference[oaicite:3]{index=3}

### What we check:
- Relations tab
- Domains linked to the IP

Finding:
- Suspicious domain found:
  www.po1s0n1vy.com

----------------------------------
EXTRA OSINT
----------------------------------

WHOIS lookup:
- whois.domaintools.com
- Used to find registration details

----------------------------------
DELIVERY PHASE
----------------------------------

## What is Delivery?
How malware is delivered:
- Files
- URLs
- Emails
- Web downloads

----------------------------------
THREAT INTEL CORRELATION
----------------------------------

Threat Intel says:
- Attacker group "Poison Ivy"
- Has secondary attack methods

----------------------------------
OSINT: THREATMINER
----------------------------------

Tool:
:contentReference[oaicite:4]{index=4}

Search IP:
23.22.63.114

Finding:
- Multiple files associated
- One malicious file found

MD5 Hash:
c99131e0169171935c5ac32615ed6261

----------------------------------
VIRUSTOTAL (FILE HASH)
----------------------------------

Search the hash on VirusTotal.

Finding:
- Malware metadata
- Detection results
- Behavior summary

----------------------------------
HYBRID ANALYSIS
----------------------------------

Tool:
:contentReference[oaicite:5]{index=5}

What it shows:
- Network communication
- DNS requests
- Contacted hosts
- MITRE ATT&CK mapping
- Malicious indicators
- File behavior details

----------------------------------
FINAL PHASE MAPPING
----------------------------------

Command & Control:
- Server contacted attacker domain

Weaponization:
- Similar domains created
- C2 infrastructure ready

Delivery:
- Malware linked via IP and hash
- Confirmed by OSINT tools

----------------------------------
ONE LINE REVISION
----------------------------------

C2 connect → Prepare malware → Deliver payload

----------------------------------
