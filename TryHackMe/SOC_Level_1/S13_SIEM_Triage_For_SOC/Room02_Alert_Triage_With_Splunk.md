# SOC Investigation Notes (Very Simple Version)

## 1. Situation

You are working as a **SOC Level 1 Analyst**.

SOC means **Security Operations Center**.  
Your job is to **monitor security alerts and investigate suspicious activity**.

One alert appears in the SIEM system.

Alert details:

Alert Name: Brute Force Activity Detection  
Time: 17/09/2025 9:00 AM  
Target Host: tryhackme-2404  
Source IP: 10.10.242.248

Your task is simple:

1. Check if the alert is real or false
2. Investigate the logs
3. Decide if the activity is malicious
4. Escalate if needed

---

# 2. Step 1 — First Look at the Alert

When SOC analysts see an alert, they first check **basic information**.

Important fields:

| Field | Meaning |
|-----|-----|
| Source IP | Where the attack is coming from |
| Target Host | Which system is being attacked |
| Time | When the attack happened |

Example:

Source IP = 10.10.242.248

This is a **private IP address**.

Private IP ranges:

10.x.x.x  
172.16.x.x – 172.31.x.x  
192.168.x.x

This means the attacker is **inside the network**, not from the internet.

Possible reasons:

- attacker already inside the company network  
- compromised internal machine  
- insider threat  
- VPN compromise

This is already **suspicious**.

---

# 3. Step 2 — Open Logs in Splunk

SOC analysts investigate alerts using **SIEM tools**.

Here we use **Splunk**.

We search logs in the index called:

index=linux-alert

This means we are looking at **Linux system logs**.

---

# 4. Step 3 — Look for Login Attempts

We want to check **SSH login activity**.

Important SSH events:

| Log Message | Meaning |
|------|------|
| Failed password | Login failed |
| Accepted password | Login successful |
| Invalid user | Username does not exist |

These messages help detect **brute force attacks**.

Search query used:

index="linux-alert" sourcetype="linux_secure" 10.10.242.248
search "Accepted password for" OR "Failed password for" OR "Invalid user"

---

# 5. Step 4 — What Did We Notice?

The logs show many login attempts from the same IP.

Some logs show:

Invalid user admin  
Invalid user guest  
Invalid user test

This means the attacker is **guessing usernames**.

Attackers often do this before brute forcing passwords.

Example:

Step 1 → find valid usernames  
Step 2 → try many passwords

This behavior is **very suspicious**.

---

# 6. Step 5 — Check Which User Was Targeted

SOC analysts now check **which user account received the most login attempts**.

Search query:

index="linux-alert" sourcetype="linux_secure" 10.10.242.248
stats count by username

Result example:

| Username | Login Attempts |
|--------|--------|
| john.smith | 503 |
| admin | 5 |
| guest | 3 |
| test | 2 |

This shows something very important.

The attacker tried **503 times** to login to the account:

john.smith

This is a **strong sign of brute force attack**.

---

# 7. Step 6 — Check if Login Was Successful

Now the most important question:

Did the attacker successfully login?

Search query:

index="linux-alert" sourcetype="linux_secure" 10.10.242.248
search "Accepted password"

Log example:

Accepted password for john.smith

This means the attacker **finally guessed the correct password**.

So the attacker now has **access to the system**.

---

# 8. Step 7 — SOC Decision

SOC analysts classify alerts into 3 types.

| Type | Meaning |
|------|------|
| True Positive | Real attack |
| False Positive | Wrong alert |
| Benign | Normal activity |

In this case we have:

- many failed logins
- invalid users
- targeted account
- successful login

This is clearly a **True Positive**.

A real attack happened.

---

# 9. Step 8 — What Should SOC Level 1 Do?

SOC Level 1 analysts do not handle the full incident.

Their job is to:

- detect suspicious activity
- confirm the alert
- escalate the case

So the analyst must **escalate the incident to SOC Level 2**.

Example escalation report:

Incident Summary:

Multiple brute force login attempts detected from IP 10.10.242.248.

Target user: john.smith  
Total attempts: 503  

A successful login was observed for john.smith.

Activity confirmed as brute force attack.

Incident escalated for further investigation.

---

# 10. What SOC Level 2 Will Investigate

SOC Level 2 will answer important questions.

Example questions:

1. Why is the attacker inside the network?
2. Which system is using the IP 10.10.242.248?
3. What did the attacker do after login?
4. Did the attacker move to other systems?

They will check:

- command history
- privilege escalation
- lateral movement
- persistence techniques

---

# 11. Incident Response Actions

If the attack is confirmed, the incident response team will act.

Containment:

- disable john.smith account
- block attacker IP
- isolate infected machine

Remediation:

- reset passwords
- enable MFA
- fix security gaps

Recovery:

- restore affected systems
- monitor network activity

---

# 12. Key SOC Learning Points

Important indicators of brute force attacks:

- many failed logins
- invalid usernames
- repeated attempts on one account
- successful login after many failures

Common SSH log keywords:

Failed password  
Accepted password  
Invalid user

These are **very important for SOC analysts**.

---

# 13. Simple Story to Remember

Think of it like this.

Someone is trying to enter a locked room.

Step 1  
They guess many names of people who might own the room.

Step 2  
They find the correct name (john.smith).

Step 3  
They try hundreds of keys.

Step 4  
One key works.

Now they enter the room.

This is exactly what happened in this attack.

---
# Lab Questions
## Q.1 How many failed login attempts were made on the user john.smith?
<img width="1470" height="956" alt="Screenshot 2026-03-09 at 9 53 32 PM" src="https://github.com/user-attachments/assets/810b2527-b60f-40af-8e7d-3618d69fbcb1" />

## Q.2 What was the duration of the brute force attack in minutes?
<img width="1470" height="956" alt="Screenshot 2026-03-09 at 9 55 55 PM" src="https://github.com/user-attachments/assets/54c834bb-7ca0-4d35-bd12-54b96f5b8874" />

## Q.3 What username was the attacker able to privilege escalate to?
<img width="1470" height="956" alt="Screenshot 2026-03-09 at 9 56 27 PM" src="https://github.com/user-attachments/assets/0bdadbba-bd0e-490c-9cc0-71b87eab7b6e" />

## Q.4 What is the name of the user account created by the attacker for persistence?
<img width="1470" height="956" alt="Screenshot 2026-03-09 at 9 58 36 PM" src="https://github.com/user-attachments/assets/16eeac23-277d-4aef-b704-a77caa34a513" />



# Final SOC Conclusion

An internal IP attempted multiple SSH logins.

503 brute force attempts targeted the user john.smith.

The attacker successfully logged in.

The alert is a **True Positive brute force attack**.

The incident must be **escalated for further investigation**.
---

# SOC Investigation Notes  
## Windows Scenario – Suspicious Scheduled Task (Very Simple)

Imagine you are a **SOC Level 1 Analyst**.  
Your job is to **investigate alerts and decide if they are malicious or not**.

A new alert appears in the SIEM.

Alert Details

Alert Name: Potential Task Scheduler Persistence Identified  
Time: 30/08/2025 10:06 AM  
Host: WIN-H015  
User: oliver.thompson  
Task Name: AssessmentTaskOne

Your job is to check:

1. Is this activity normal?
2. Or is it a real attack?

---

# 1. Step One – Read the Alert Carefully

Before opening Splunk, SOC analysts first read the alert.

Important fields:

| Field | Meaning |
|------|------|
| Host | Which machine the activity happened on |
| User | Which user performed the action |
| Task Name | The scheduled task created |

This gives **context**.

---

# 2. Step Two – Identify the Host Type

Look at the host name.

Host: WIN-H015

Companies usually follow naming rules.

Example naming patterns:

| Prefix | System Type |
|------|------|
| SRV | Server |
| WEB | Web server |
| SQL | Database server |
| WIN | Workstation |
| HOST | Workstation |

Here the name starts with **WIN**.

So the system is a **user workstation**, not a server.

This is important.

Because servers often run scheduled tasks.  
But **user workstations usually do not**.

So this already looks **slightly suspicious**.

---

# 3. Step Three – Check the User

User: oliver.thompson

SOC analysts check:

- the user's job role
- department
- working hours

In the identity database we find:

Role: **System Engineer**

This means the user is part of the **IT team**.

So creating tasks **could be normal**, but we still need to verify.

---

# 4. Step Four – Investigate in Splunk

Now we open the SIEM (Splunk).

Logs are stored in:

index=win-alert

To find scheduled task creation we search for **Event ID 4698**.

Important Windows Event ID:

| Event ID | Meaning |
|------|------|
| 4698 | Scheduled task created |

Search query:

index="win-alert" EventCode=4698 AssessmentTaskOne

This shows events where the task **AssessmentTaskOne** was created.

---

# 5. Step Five – Look at the Result

The search shows **only one event**.

Important fields:

| Field | Meaning |
|------|------|
| _time | time of event |
| user_name | user who created task |
| host | machine |
| Task_Name | scheduled task name |
| Message | details of the task |

So only **one scheduled task was created**.

Now we investigate the **Message field**.

---

# 6. Step Six – Understand the Task Behavior

Inside the Message field we see multiple sections.

Important sections:

| Section | Meaning |
|------|------|
| Triggers | When the task runs |
| Exec | What command runs |
| Principals | Which user runs it |

---

# 7. Step Seven – Check the Trigger

Trigger information shows:

The task runs **every day at the same time**.

This is strange.

Because this is a **user workstation**, not a server.

Malware often uses scheduled tasks for **persistence**.

Persistence means:

The attacker wants their malware to **run again and again automatically**.

---

# 8. Step Eight – Check What the Task Executes

Inside the **Exec section** we see something very suspicious.

The task runs a command that uses **certutil**.

Certutil is a Windows tool.

Attackers often misuse it to **download malware from the internet**.

Example behavior:

Step 1  
Download file from domain

tryhotme

Step 2  
Save file as

DataCollector.exe

Step 3  
Store it in the Temp folder

Step 4  
Run the file using PowerShell

Start-Process

So the task does this:

1. download malware  
2. save it on the computer  
3. run the malware

This is clearly **malicious behavior**.

---

# 9. Step Nine – Why This Is Persistence

Attackers want their malware to **stay active even after reboot**.

So they create a scheduled task.

Example:

Every day the task runs → malware runs again.

Even if the computer restarts → malware returns.

This technique is called:

**Task Scheduler Persistence**

---

# 10. Step Ten – Additional Investigation

SOC analysts may also check the domain.

Domain used:

tryhotme

They can search it in **Threat Intelligence platforms**.

Example:

VirusTotal  
AbuseIPDB  
ThreatFox

If the domain is linked to attackers, it confirms the threat.

---

# 11. SOC Decision

Now we evaluate the activity.

Evidence found:

- scheduled task created
- runs daily
- downloads executable file
- executes PowerShell command
- uses certutil (common attacker tool)

This is clearly malicious.

Alert classification:

**True Positive**

---

# 12. SOC Level 1 Action

SOC Level 1 analysts must **escalate the incident**.

Example escalation report:

Incident Summary

A scheduled task named AssessmentTaskOne was created on host WIN-H015.

The task downloads a file from an external domain using certutil and executes it using PowerShell.

The task runs daily under user oliver.thompson.

This behavior indicates persistence via Task Scheduler.

The activity is classified as a True Positive.

Escalated to SOC Level 2 for further investigation.

---

# 13. What SOC Level 2 Will Investigate

SOC Level 2 will answer deeper questions.

Example questions:

1. How was the scheduled task created?
2. Was the user account compromised?
3. What malware is rv.exe?
4. Did the attacker move to other machines?

They will check:

- PowerShell logs
- process creation logs
- network connections
- malware analysis

---

# 14. Simple Story to Remember

Imagine a thief enters a house.

Instead of coming again manually every day,

the thief installs a **timer lock** that opens the door every night.

So the thief can always return.

That timer lock is like a **scheduled task**.

It lets the attacker come back anytime.

---
# Lab Questions
## Q.1 What is the ProcessId of the process that created this malicious task?
<img width="1470" height="956" alt="Screenshot 2026-03-09 at 10 41 57 PM" src="https://github.com/user-attachments/assets/786dacd6-d13e-4484-8cf3-a528460e2265" />
## Q.2 What is the name of the parent process for the process that created this malicious task?
<img width="1470" height="956" alt="Screenshot 2026-03-09 at 10 46 35 PM" src="https://github.com/user-attachments/assets/fa26838f-6fae-4c60-9949-d435529aa799" />

## Q.3 Which local group did the attacker enumerate during discovery?

<img width="1470" height="956" alt="Screenshot 2026-03-09 at 10 48 31 PM" src="https://github.com/user-attachments/assets/e1bb4b62-d6f1-452e-9d5d-b90f427896be" />
## Q.4 What is the name of the workstation from which the Threat Actor logged into this host?
<img width="1470" height="956" alt="Screenshot 2026-03-09 at 10 50 48 PM" src="https://github.com/user-attachments/assets/96b8d2ea-4972-4b11-80c0-d35857c0997f" />


# Final SOC Conclusion

A scheduled task was created on workstation WIN-H015.

The task downloads and executes a suspicious executable from an external domain.

This indicates persistence using Windows Task Scheduler.

The activity is confirmed as a **True Positive security incident**.

---
# SOC Investigation Notes  
## Web Shell Attack Investigation (Very Simple)

These notes explain how a **SOC Level 1 Analyst** investigates a possible **web shell attack** on a web server.

---

# 1. Alert Information

Alert Name: Potential Web Shell Upload Detected  
Time: 14/09/2025 09:31 AM  
Website: http://web.trywinme.thm  
Suspicious IP: 171.251.232.40  

SOC analyst must answer one question:

Is this activity **malicious or normal**?

---

# 2. Step 1 — Understand the Alert

Important fields:

| Field | Meaning |
|------|------|
Resource | Website where activity happened |
Suspicious IP | IP address sending requests |

The resource is the **organization's website**.

This means the activity happened on the **web server**.

---

# 3. Step 2 — Check the Suspicious IP

SOC analysts always check suspicious IPs using **Threat Intelligence platforms**.

Example platforms:

AbuseIPDB  
VirusTotal  
GreyNoise  

When the IP **171.251.232.40** was checked in AbuseIPDB:

Result:

The IP was reported **more than 3000 times for malicious activity**.

This means the IP is likely used by attackers.

---

# 4. Step 3 — Investigate Logs in Splunk

Logs are stored in the index:

index=web-alert

Search for activity from the suspicious IP:

index=web-alert 171.251.232.40

Important log fields:

| Field | Meaning |
|------|------|
_time | time of request |
clientip | attacker IP |
useragent | tool or browser used |
uri_path | page accessed |
method | request type |
status | server response |

---

# 5. Step 4 — Detect Brute Force Activity

Logs show many requests with the User-Agent:

Hydra

Hydra is a **password brute force tool** used by attackers.

The target page was:

wp-login.php

This is the **WordPress login page**.

This indicates the attacker attempted a **login brute force attack**.

---

# 6. Step 5 — Look for Other Suspicious Activity

Since the alert is about a **web shell**, the SOC analyst removes Hydra results to focus on other actions.

Search:

index=web-alert 171.251.232.40 useragent!="Mozilla/5.0 (Hydra)"

This shows other requests made by the attacker.

---

# 7. Step 6 — Suspicious PHP File Detected

One request shows a referer containing:

theme-editor.php?file=b374k.php

This is suspicious.

Reason:

Normal website files do not randomly reference unknown PHP files.

The file name **b374k.php** is important.

---

# 8. Step 7 — Identify the Web Shell

SOC analysts search logs related to this file.

Search:

index=web-alert 171.251.232.40 b374k.php

Results show:

Multiple **POST requests** to b374k.php.

POST requests mean the attacker is sending commands to the server.

---

# 9. Step 8 — Research the File

SOC analysts research suspicious files.

Search online:

b374k.php web shell

Result:

b374k.php is a **well-known PHP web shell used by attackers**.

This confirms the attacker is using a web shell.

---

# 10. What is a Web Shell?

A web shell is a **malicious script uploaded to a web server**.

It allows attackers to control the server remotely.

Example attacker actions:

- execute commands
- upload files
- download data
- create backdoors

This allows the attacker to maintain **full control of the server**.

---

# 11. Attack Timeline

The attack likely happened in this order:

Step 1  
Attacker accessed the website from IP 171.251.232.40.

Step 2  
Attacker used Hydra to perform brute force attempts on the login page.

Target page:

wp-login.php

Step 3  
A web shell file appeared on the server.

File name:

b374k.php

Step 4  
Attacker accessed the web shell and sent commands using POST requests.

---

# 12. SOC Decision

Evidence found:

| Evidence | Meaning |
|------|------|
Malicious IP | Known attacker infrastructure |
Hydra brute force | Unauthorized login attempts |
b374k.php file | Known web shell |
POST requests | Commands executed on server |

Conclusion:

This activity is **malicious**.

Classification:

True Positive

---

# 13. SOC Level 1 Action

SOC Level 1 analysts must escalate the incident.

Example escalation summary:

Malicious activity detected from IP 171.251.232.40 targeting the web server.

The attacker performed brute force attempts using Hydra against wp-login.php.

Logs show access to a known web shell file b374k.php with multiple POST requests.

This indicates web shell exploitation.

The incident is classified as True Positive and escalated to SOC Level 2.

---

# 14. Next Investigation Steps

SOC Level 2 analysts will investigate further.

Important questions:

Was the brute force attack successful?

How was the web shell uploaded to the server?

What commands did the attacker run using the web shell?

Did the attacker access sensitive data?

---

# 15. Key SOC Learning Points

Important indicators of web shell attacks:

- suspicious PHP files
- multiple POST requests
- unusual file names
- attacker IP with bad reputation
- brute force tools like Hydra

These indicators help SOC analysts detect **web server compromises**.

---
# Lab Questions
## Q.1 What time did the brute-force activity using Hydra begin?
<img width="1470" height="956" alt="Screenshot 2026-03-09 at 10 59 55 PM" src="https://github.com/user-attachments/assets/38bc60ae-ce8d-47f8-83d6-0340a0b514ce" />

## Q.2 Which user agent did the attacker use when interacting with the web shell?
<img width="1470" height="956" alt="Screenshot 2026-03-09 at 11 01 44 PM" src="https://github.com/user-attachments/assets/d9e38458-867d-46ad-972e-ce8f0d2a5701" />

## Q.3  What was the number of requests made by the attacker to the server via the web shell?
<img width="1470" height="956" alt="Screenshot 2026-03-09 at 11 03 21 PM" src="https://github.com/user-attachments/assets/80efefa4-6796-47c8-875a-4dad32fde9a0" />

---

# Final SOC Conclusion

Malicious IP 171.251.232.40 targeted the web server.

The attacker performed brute force attempts and accessed a known web shell file named b374k.php.

Multiple POST requests indicate command execution through the web shell.

This confirms a **web shell exploitation attack**.

The incident must be escalated for further investigation.
