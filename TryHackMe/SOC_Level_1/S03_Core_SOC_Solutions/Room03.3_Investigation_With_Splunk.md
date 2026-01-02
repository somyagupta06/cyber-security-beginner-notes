# Windows Logs Investigation in Splunk 

Investigation is done using
splunk

Role: SOC Analyst  
Goal: Find anomalies and attacker activity in Windows logs

----------------------------------
SCENARIO SUMMARY
----------------------------------

- Johny noticed suspicious activity on Windows machines
- Attacker created backdoor users
- Logs were collected and ingested into Splunk
- Our job is to analyze these logs

----------------------------------
IMPORTANT SETTINGS
----------------------------------

- Index: main
- Source file: splunk_challenge1.json
- Time Range: All Time

----------------------------------
QUESTION 1
----------------------------------
How many events were ingested in index main?

Answer:
- Total events = 12256

----------------------------------
QUESTION 2
----------------------------------
What is the new backdoor username created by attacker?

Windows Event ID:
- 4720 (User account created)

Search Query:
source="splunk_challenge1.json" EventID=4720

Finding:
- New username created: A1berto

Note:
- Subject field shows who created the user
- Here, James initiated the action

----------------------------------
QUESTION 3
----------------------------------
What is the full registry key path for the backdoor user?

Registry change Event:
- Sysmon EventID = 13

Search Query:
source="splunk_challenge1.json" EventID=13 "A1berto"

Answer:
HKLM\SAM\SAM\Domains\Account\Users\Names\A1berto\

----------------------------------
QUESTION 4
----------------------------------
Which user was the attacker trying to impersonate?

Answer:
- Alberto

Reason:
- Username A1berto closely matches Alberto

----------------------------------
QUESTION 5
----------------------------------
What command was used to add the backdoor user remotely?

Relevant Event:
- EventID = 4688 (Process creation)

Command Found:
"C:\windows\System32\Wbem\WMIC.exe" 
/node:WORKSTATION6 
process call create 
"net user /add A1berto paw0rd1"

----------------------------------
QUESTION 6
----------------------------------
How many successful login attempts were made by backdoor user?

Login Event:
- EventID = 4624 (Successful login)

Search Result:
- No events found for user A1berto

Answer:
- 0 successful logins

----------------------------------
QUESTION 7
----------------------------------
Which host executed suspicious PowerShell commands?

Finding:
- Hostname: James.browne

----------------------------------
QUESTION 8
----------------------------------
How many malicious PowerShell events were logged?

PowerShell Event IDs:
- 4104
- 4103

Answer:
- Total events = 74

----------------------------------
QUESTION 9
----------------------------------
What is the full URL contacted by encoded PowerShell script?

Observation:
- PowerShell used Base64 encoded command
- Decoded value revealed IP address
- Script downloaded data from a web server

Decoded URL:
http://10.10.10.5/news.php

Defanged URL:
hxxp[://]10[.]10[.]10[.]5/news[.]php

----------------------------------
FINAL ATTACK SUMMARY
----------------------------------

- Backdoor user created: A1berto
- Registry modified for persistence
- PowerShell used to disable logging
- Encoded script contacted attacker server
- Malware payload downloaded from URL

----------------------------------
ONE LINE REVISION
----------------------------------

Create user → Modify registry → Execute PowerShell → Contact C2

----------------------------------
