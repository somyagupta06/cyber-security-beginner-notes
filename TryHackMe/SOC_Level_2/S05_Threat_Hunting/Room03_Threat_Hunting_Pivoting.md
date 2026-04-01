# 🔍 Discovery 

## 🌟 What is Discovery?

Discovery means:
👉 **Attacker collects information about system and network**

Think like this:

* Initial Access = attacker enters
* Execution = attacker runs commands
* Discovery = attacker **learns everything about system**

---

## 🎯 SOC Goal

As a SOC Analyst, your job is:

* Detect **information gathering activity**
* Identify **unusual commands and scans**
* Stop attacker before next stage

---

## 🧠 Important Idea

Attacker wants to know:

* Who is logged in
* System details
* Network machines
* Domain users and permissions

👉 Goal = **Prepare for next attack**

---

## 🚨 Common Discovery Methods

| Technique    | What attacker does             |
| ------------ | ------------------------------ |
| User Recon   | Checks users and privileges    |
| Host Recon   | Checks system details          |
| Network Scan | Finds other machines and ports |
| Domain Recon | Collects AD information        |

---

# 🔍 SCENARIO 1: Host Reconnaissance

## 💡 What is happening?

Attacker runs commands to collect system info

---

## 🧪 Detection Query

winlog.event_id: 1 AND process.name: (whoami.exe OR hostname.exe OR net.exe OR systeminfo.exe OR ipconfig.exe OR netstat.exe OR tasklist.exe)

---

## 🔎 What SOC looks for

* Many commands executed
* Same user running all commands
* Commands executed quickly

---

## 🚨 SOC Finding

* User: bill.hawkins
* Many commands executed
* All via cmd.exe

---

## 🧠 Suspicion

👉 Commands are normal
👉 BUT too many together = suspicious

---

## 🔥 Deep Investigation

* cmd.exe spawned by PowerShell
* PowerShell contains:

  * IEX
  * downloadstring
  * bypass
  * hidden

---

## 💀 Meaning

👉 Malicious script running commands

---

## 🧠 Next Step

* Check PowerShell activity
* Check child processes

---

# 🔍 SCENARIO 2: Internal Network Scanning

## 💡 What is happening?

Attacker scans internal network

---

## 🧪 Detection Query

source.ip: 10.0.0.0/8 AND destination.ip: 10.0.0.0/8 AND destination.port < 1024

---

## 🔎 What SOC looks for

* Many connections
* Many ports scanned
* Same source and target

---

## 🚨 SOC Finding

* WKSTN-2 scanned 1000+ ports
* Target: INTSRV01

---

## 💀 Why dangerous?

👉 Normal user does NOT scan ports

---

## 🔥 Process Correlation

### 🧪 Query

winlog.event_id: 3 AND source.ip: 10.10.x.x AND destination.port < 1024

---

## 🚨 Result

* Process: n.exe

---

## 🧠 Meaning

👉 Port scanning tool used

---

## 🧠 Next Step

* Check parent process
* Check next connections

---

# 🔍 SCENARIO 3: Active Directory Discovery

## 💡 What is happening?

Attacker collects domain information

---

## 🧪 Detection Query

winlog.event_id: 3 AND destination.port: (389 OR 636) AND NOT process.name: mmc.exe

---

## 🔎 What SOC looks for

* LDAP connections
* Unusual processes
* Non-admin activity

---

## 🚨 SOC Finding

* Process: SharpHound.exe

---

## 💀 Why dangerous?

👉 Tool used for AD reconnaissance

---

## 🧠 Meaning

👉 Attacker mapping:

* Users
* Groups
* Permissions

---

## 🔥 Next Step

* Check how tool was executed
* Check next attacker actions

---

# 🔁 SOC Investigation Steps (Very Important)

## 🧩 Always follow this:

1. Detect unusual commands
2. Check frequency of execution
3. Identify parent process
4. Detect network scanning
5. Correlate with processes
6. Check domain activity

---

# 💡 Key Points to Remember

* Normal commands can be abused
* Many commands together = suspicious
* Internal scanning is a big red flag
* AD tools indicate deeper attack

---

# ⚡ One Line Summary

👉 Discovery = **Attacker is gathering information for next attack**

---

# 🧠 Final SOC Thinking

Do NOT ignore normal commands ❌
👉 Always ask:

* Why so many commands?
* Who is running them?
* What is the purpose?
* What will attacker do next?

---

✅ That is real SOC mindset
---
# Lab Questions
## Q.1 What is the name of the account seen to be executing host enumeration commands on DC01?
<img width="1470" height="956" alt="Screenshot 2026-04-01 at 9 28 03 AM" src="https://github.com/user-attachments/assets/2761489f-7f11-423d-ad0e-fa33f578b68f" />
## Q.2 Following the port scanning activity investigation, what is the parent process of n.exe? 
<img width="1470" height="956" alt="Screenshot 2026-04-01 at 9 36 42 AM" src="https://github.com/user-attachments/assets/e45287f1-345e-4150-a002-7e7ce7bff383" />
## Q.3 What is the full command-line value of the SharpHound.exe process?
<img width="1470" height="956" alt="Screenshot 2026-04-01 at 9 39 24 AM" src="https://github.com/user-attachments/assets/8987f02c-1bad-4d48-b576-5f7a0be5dbfd" />

---
# ⬆️ Privilege Escalation 

## 🌟 What is Privilege Escalation?

Privilege Escalation means:
👉 **Attacker increases their access level in the system**

Think like this:

* Initial Access = attacker enters
* Execution = attacker runs commands
* Discovery = attacker gathers info
* Privilege Escalation = attacker **becomes more powerful**

---

## 🎯 SOC Goal

As a SOC Analyst, your job is:

* Detect **sudden increase in privileges**
* Identify **how attacker gained higher access**
* Stop attacker before full system control

---

## 🧠 Important Idea

System has levels:

| Level  | Access     |
| ------ | ---------- |
| User   | Low        |
| Admin  | Medium     |
| SYSTEM | Highest 💀 |

👉 Attacker goal:
👉 **Reach SYSTEM level**

---

## 🚨 Common Techniques

| Technique             | What attacker does        |
| --------------------- | ------------------------- |
| Exploit vulnerability | Uses system weakness      |
| Use valid accounts    | Logs in as admin          |
| Abuse permissions     | Uses misconfigured access |
| Service abuse         | Modifies system services  |

---

# 🔍 SCENARIO 1: SeImpersonatePrivilege Abuse

## 💡 What is happening?

Attacker uses special privilege to act as SYSTEM

---

## 🧪 Detection Query

winlog.event_id: 1 AND user.name: SYSTEM AND NOT winlog.event_data.ParentUser: "NT AUTHORITY\SYSTEM"

---

## 🔎 What SOC looks for

* SYSTEM process created
* Parent user is NOT SYSTEM

---

## 🚨 SOC Finding

* User: IIS APPPOOL\DefaultAppPool
* Process: spoofer.exe
* Result: SYSTEM process created

---

## 💀 Why dangerous?

👉 Low user → SYSTEM access

---

## 🔍 Deep Analysis

* spoofer.exe → spawned by PowerShell
* PowerShell contains:

  * encoded command
  * hidden execution
  * bypass

---

## 💀 Tool Identified

* spoofer.exe = PrintSpoofer

👉 Known privilege escalation tool

---

## 🧠 Meaning

👉 Attacker abused SeImpersonatePrivilege
👉 Gained SYSTEM access

---

## 🔥 Next Step

* Check what attacker did as SYSTEM
* Trace events before escalation

---

# 🔍 SCENARIO 2: Service Permission Abuse

## 💡 What is happening?

Attacker modifies a service to run malicious file

---

## 🧪 Detection Query

winlog.event_id: 13 AND registry.path: *HKLM\System\CurrentControlSet\Services\*\ImagePath*

---

## 🔎 What SOC looks for

* Service path modification
* Suspicious file paths
* Non-system locations

---

## 🚨 SOC Finding

* Service: SNMPTRAP
* New path: C:\Users\bill.hawkins\Documents\update.exe

---

## 💀 Why dangerous?

👉 Service runs as SYSTEM
👉 Malicious file runs with SYSTEM privileges

---

## 🔍 Deep Investigation

### Step 1: Modification Command

sc.exe config SNMPTRAP binPath= C:\Users\bill.hawkins\Documents\update.exe

* User: bill.hawkins (low privilege)

---

### Step 2: Service Started

sc.exe start SNMPTRAP

---

### Step 3: Result

* update.exe executed
* User: SYSTEM

---

## 💀 Meaning

👉 Attacker hijacked service
👉 Achieved SYSTEM access

---

## 🧠 Next Step

* Check activity after SYSTEM access
* Trace parent process of sc.exe

---

# 🔁 SOC Investigation Steps (Very Important)

## 🧩 Always follow this:

1. Detect SYSTEM-level processes
2. Check parent user
3. Identify suspicious binaries
4. Check service modifications
5. Correlate registry changes
6. Track activity after escalation

---

# 💡 Key Points to Remember

* SYSTEM access = full control
* Parent-child relation is critical
* Services can be abused easily
* User folder binaries are suspicious
* Encoded PowerShell = red flag

---

# ⚡ One Line Summary

👉 Privilege Escalation = **Attacker gains higher access**

---

# 🧠 Final SOC Thinking

Do NOT trust privilege level ❌
👉 Always ask:

* How did this process get SYSTEM access?
* Was it normal or suspicious?
* What was the parent process?
* What happened after escalation?

---

✅ That is real SOC mindset

---


# Lab Questions
## Q.1 What is the full command-line value of the process spawned by spoofer.exe?
<img width="1470" height="956" alt="Screenshot 2026-04-01 at 9 49 47 AM" src="https://github.com/user-attachments/assets/c615f4b5-9cec-4681-aa6e-ce889d15dcf2" />
## Q.2 What is the name of the other service that was abused besides SNMPTRAP?
<img width="1470" height="956" alt="Screenshot 2026-04-01 at 9 55 37 AM" src="https://github.com/user-attachments/assets/c04da0ca-9988-4721-9d8c-db03ca6debe7" />
## Q.3 What is the MD5 hash of the update.exe binary?
<img width="1470" height="956" alt="Screenshot 2026-04-01 at 9 57 02 AM" src="https://github.com/user-attachments/assets/0571503f-f3f3-4151-9878-9c4de0885c3d" />

---
# 🔑 Credential Access 

## 🌟 What is Credential Access?

Credential Access means:
👉 **Attacker steals usernames and passwords (or hashes)**

Think like this:

* Initial Access = attacker enters
* Execution = attacker runs commands
* Privilege Escalation = attacker becomes powerful
* Credential Access = attacker **steals login keys**

---

## 🎯 SOC Goal

As a SOC Analyst, your job is:

* Detect **credential theft attempts**
* Identify **how attacker got passwords**
* Stop attacker from using stolen accounts

---

## 🧠 Important Idea

Credentials = **keys to system**

👉 If attacker gets credentials:

* Can login as user
* Can become admin
* Can move to other systems

---

## 🚨 Common Techniques

| Technique   | What attacker does             |
| ----------- | ------------------------------ |
| Memory dump | Steals credentials from memory |
| File search | Finds passwords in files       |
| Domain dump | Steals domain credentials      |
| Brute force | Guesses passwords              |

---

# 🔍 SCENARIO 1: LSASS Credential Dumping

## 💡 What is happening?

Attacker extracts credentials from lsass.exe

---

## 🧪 Detection Query (Mimikatz)

winlog.event_id: 1 AND process.command_line: (*mimikatz* OR *DumpCreds* OR *privilege::debug* OR *sekurlsa::*)

---

## 🔎 What SOC looks for

* Mimikatz usage
* Suspicious PowerShell scripts
* Credential dumping commands

---

## 🚨 SOC Finding

* Tool: Invoke-Mimikatz.ps1
* Downloaded from internet

---

## 💀 Why dangerous?

👉 LSASS contains passwords
👉 Dump = credentials stolen

---

## 🔥 Alternative Detection (Dump File)

### 🧪 Query

winlog.event_id: 11 AND file.path: *lsass.DMP

---

## 🚨 SOC Finding

* File: lsass.DMP
* Location: Temp folder

---

## 💀 Meaning

👉 LSASS memory copied
👉 Credentials can be extracted

---

## 🧠 Next Step

* Check usage of stolen credentials
* Trace activity before dumping

---

# 🔍 SCENARIO 2: Domain Credential Dump (DCSync)

## 💡 What is happening?

Attacker dumps domain credentials from Domain Controller

---

## 🧪 Detection Query

winlog.event_id: 4662 AND winlog.event_data.AccessMask: 0x100 AND winlog.event_data.Properties: (*1131f6aa* OR *1131f6ad* OR *9923a32a* OR *89e95b76*)

---

## 🔎 What SOC looks for

* Unusual replication requests
* High-privileged account usage
* Domain controller access

---

## 🚨 SOC Finding

* Account: backupadm
* Performed DCSync

---

## 💀 Why dangerous?

👉 Attacker gets all domain password hashes

---

## 🧠 Meaning

👉 Account compromised
👉 Domain fully exposed

---

## 🔥 Next Step

* Check processes run by account
* Trace how account was compromised

---

# 🔍 SCENARIO 3: Brute Force Attack

## 💡 What is happening?

Attacker guesses password

---

## 🧪 Detection Query (Failed Logins)

winlog.event_id: 4625

---

## 🔎 What SOC looks for

* Many failed login attempts
* Same user or same IP
* Short time period

---

## 🚨 SOC Finding

* User: jade.burke
* Many failed logins

---

## 🔥 Check Success

### 🧪 Query

winlog.event_id: 4624 AND user.name: jade.burke

---

## 🚨 Result

* Successful login found

---

## 💀 Meaning

👉 Password guessed successfully

---

## 🔥 Next Step

### 🧪 Query

host.name: WKSTN-1* AND winlog.event_id: 1 AND user.name: jade.burke

---

## 🧠 Finding

* User executed suspicious commands

👉 Account used by attacker

---

# 🔁 SOC Investigation Steps (Very Important)

## 🧩 Always follow this:

1. Detect credential dumping
2. Check LSASS access
3. Look for dump files
4. Monitor domain access
5. Identify brute-force patterns
6. Track usage of stolen accounts

---

# 💡 Key Points to Remember

* LSASS = high-value target
* Dump file = strong indicator
* Domain dump = full compromise
* Failed logins + success = brute force
* Stolen credentials used quickly

---

# ⚡ One Line Summary

👉 Credential Access = **Attacker steals login credentials**

---

# 🧠 Final SOC Thinking

Do NOT trust login activity ❌
👉 Always ask:

* How did user get access?
* Was password stolen or guessed?
* What happened after login?
* Is this normal behavior?

---

✅ That is real SOC mindset

---
# Lab Questions
## Q.1 What is the name of the process that created the lsass.DMP file?
<img width="1470" height="956" alt="Screenshot 2026-04-01 at 10 06 20 AM" src="https://github.com/user-attachments/assets/84906365-1e03-448c-b7a2-45985ef698b6" />
## Q.2 Out of the four GUIDs used to hunt DCSync, what is the value of the GUID seen in the existing logs?
<img width="1470" height="956" alt="Screenshot 2026-04-01 at 10 10 05 AM" src="https://github.com/user-attachments/assets/82addf13-fd77-4221-a6d7-4ffb19c569ee" />
## Q.3 What is the name of the first process spawned by jade.burke on WKSTN-1?
<img width="1470" height="956" alt="Screenshot 2026-04-01 at 10 15 18 AM" src="https://github.com/user-attachments/assets/96f2fe7d-8725-49fc-865e-f8ebb528825d" />
---
# 🔄 Lateral Movement 

## 🌟 What is Lateral Movement?

Lateral Movement means:
👉 **Attacker moves from one system to another inside the network**

Think like this:

* Initial Access = attacker enters one system
* Credential Access = attacker gets passwords
* Lateral Movement = attacker **spreads to other systems**

---

## 🎯 SOC Goal

As a SOC Analyst, your job is:

* Detect **unusual internal access**
* Identify **remote logins between systems**
* Stop attacker from spreading in network

---

## 🧠 Important Idea

Attacker uses:

* Valid credentials
* Admin tools
* Remote services

👉 Goal = **Control multiple systems**

---

## 🚨 Common Techniques

| Technique      | What attacker does             |
| -------------- | ------------------------------ |
| WMI            | Runs commands on remote system |
| PsExec / Tools | Uses admin tools               |
| Pass-the-Hash  | Uses password hash             |
| Remote login   | Access other machines          |

---

# 🔍 SCENARIO 1: Lateral Movement via WMI

## 💡 What is happening?

Attacker uses WMI to run commands on another system

---

## 🧪 Detection Query

winlog.event_id: 1 AND process.parent.name: WmiPrvSE.exe

---

## 🔎 What SOC looks for

* WmiPrvSE.exe spawning processes
* cmd.exe or powershell.exe as child
* Unusual commands

---

## 🚨 SOC Finding

* WmiPrvSE.exe → cmd.exe
* User: clifford.miller
* Host: WKSTN-1

---

## 💀 Suspicious Pattern

cmd.exe /Q /c ... 1> \127.0.0.1\ADMIN$...

👉 Known pattern of **wmiexec.py (Impacket tool)**

---

## 🧠 Meaning

👉 Remote command execution
👉 Lateral movement in progress

---

## 🔥 Authentication Check

* Event ID: 4624
* Logon Type: 3 (Network)
* Source IP: 10.10.184.105

---

## 💀 Meaning

👉 Attack coming from another compromised system

---

# 🔍 SCENARIO 2: Pass-the-Hash (PtH)

## 💡 What is happening?

Attacker uses password hash instead of password

---

## 🧪 Detection Query

winlog.event_id: 4624 AND winlog.event_data.LogonType: 3 AND winlog.event_data.LogonProcessName: *NtLmSsp* AND winlog.event_data.KeyLength: 0

---

## 🔎 What SOC looks for

* Successful login
* Network logon
* No password used (KeyLength: 0)

---

## 🚨 SOC Finding

* User: clifford.miller
* Source: 10.10.184.105

---

## 💀 Why dangerous?

👉 Attacker logged in without password

---

## 🔥 Next Step

Check processes after login:

winlog.event_id: 1 AND user.name: clifford.miller

---

## 🧠 Finding

* cmd.exe executed
* Same pattern as WMI attack

---

## 💀 Meaning

👉 Pass-the-Hash + WMI used together

---

# 🔁 SOC Investigation Steps (Very Important)

## 🧩 Always follow this:

1. Detect unusual internal logins
2. Check logon type (Network = suspicious)
3. Identify source IP
4. Detect WMI or remote execution
5. Check process activity after login
6. Correlate multiple events

---

# 💡 Key Points to Remember

* Internal traffic is not always safe
* Same user across multiple systems = red flag
* WmiPrvSE.exe spawning cmd = suspicious
* KeyLength 0 = Pass-the-Hash
* One compromised system can infect others

---

# ⚡ One Line Summary

👉 Lateral Movement = **Attacker spreads inside network**

---

# 🧠 Final SOC Thinking

Do NOT trust internal activity ❌
👉 Always ask:

* Why is this user accessing another system?
* Is this normal behavior?
* Where did the login come from?
* What happened after login?

---

✅ That is real SOC mindset


---

# Lab Questions
## Q.1 What is the name of the account that also used WMIExec on WKSTN-1 aside from clifford.miller?

<img width="1470" height="956" alt="Screenshot 2026-04-01 at 10 33 56 AM" src="https://github.com/user-attachments/assets/84111d7b-d1df-4b4b-b6ce-144add4dd093" />

## Q.2 Excluding the false positive account, how many events were generated by potential Pass-the-Hash authentications? 
To find the number of events generated by potential Pass-the-Hash authentications, you should run a KQL query in Kibana to filter for the relevant authentication events. Use the following query:

winlog.event_id: 4624 AND winlog.event_data.LogonType: 3 AND winlog.event_data.LogonProcessName: *NtLmSsp* AND winlog.event_data.KeyLength: 0.

This will help you identify the events related to Pass-the-Hash authentications. After running the query, check the total count of results excluding any entries for ANONYMOUS LOGON, which are known false positives.

## Q.3 Excluding the executions of the cd command, what is the full command-line value of the subsequent process spawned after the first successful PtH authentication of clifford.miller?

















