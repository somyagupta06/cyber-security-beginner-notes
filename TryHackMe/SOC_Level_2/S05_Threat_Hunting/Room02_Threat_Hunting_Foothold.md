# 🛡️ Initial Access 

## 🌟 What is Initial Access?

Initial Access means:
👉 **How an attacker enters a system for the first time**

Think like this:

* Your house = computer/network
* Door = security
* Thief = attacker

👉 Initial Access = **How thief enters your house**

---

## 🎯 SOC Goal

As a SOC Analyst, your job is:

* Detect **who is trying to enter**
* Check **if they succeeded**
* Stop them **early**

👉 Catch attacker **as soon as they enter**

---

## 🚪 Ways Attackers Enter (Common Methods)

| Method           | What happens                       |
| ---------------- | ---------------------------------- |
| Phishing         | Fake email tricks user             |
| Brute Force      | Guessing passwords again and again |
| Web Exploit      | Breaking website to run code       |
| USB Attack       | Infected pen drive                 |
| Cracked Software | Fake software with malware         |

---

## 🧠 Important Idea

Attacker wants:

* ✅ Account access (username/password)
* ✅ Machine access (run code on system)

---

# 🔍 SCENARIO 1: SSH Brute Force

## 💡 What is happening?

Attacker tries many passwords to login.

---

## 🔎 What SOC looks for

* Many **failed login attempts**
* Same IP address
* Same or different users

---

## 🧪 Detection Query

host.name: jumphost AND event.category: authentication AND system.auth.ssh.event: Failed

---

## 🚨 What it means

* Too many failures = **Brute force attack**

---

## 🔥 Next Step

Check if attacker succeeded:

host.name: jumphost AND system.auth.ssh.event: Accepted

---

## 🎯 SOC Conclusion

* If success found → 🚨 **System compromised**

---

## 🧠 What to do next

* Check **what commands attacker ran**

---

# 🌐 SCENARIO 2: Web Attack (RCE)

## 💡 What is happening?

Attacker is testing website to find weak points.

---

## 🔎 What SOC looks for

* Many **404 errors (page not found)**
* Same IP sending many requests

---

## 🧪 Detection Query

host.name: web01 AND network.protocol: http AND destination.port: 80

---

## 🚨 What it means

* Many 404 = attacker is **guessing URLs (scanning)**

---

## 🔥 Next Step

Check valid pages:

http.response.status_code: (200 OR 301 OR 302)

---

## 💀 Dangerous Finding

* Attacker finds working page
* Sends malicious code
* Runs commands

👉 This is called **Remote Code Execution (RCE)**

---

## 🎯 SOC Conclusion

Attack flow:

1. Scan website
2. Find valid page
3. Exploit
4. Run commands

---

## 🧠 What to do next

* Check **what commands were executed**

---

# 📧 SCENARIO 3: Phishing Attack

## 💡 What is happening?

User clicks email link or downloads file.

---

## 🔎 Case 1: File Download (Chrome)

### 🧪 Query

host.name: WKSTN-* AND process.name: chrome.exe AND winlog.event_id: 11

---

## 🚨 What SOC sees

* Strange files downloaded:

  * chrome.exe
  * update.exe
  * microsoft.hta

👉 These are **suspicious files**

---

## 🧠 Logic

Normal user:
❌ Does NOT download chrome.exe from browser

---

## 🔎 Case 2: Email Attachment (Outlook)

### 🧪 Query

host.name: WKSTN-* AND process.name: OUTLOOK.EXE AND winlog.event_id: 11

---

## 🚨 What SOC sees

* File: Update.zip
* Inside: update.lnk

---

## 💀 Why dangerous?

* .lnk file = shortcut
* Can secretly run commands

👉 Common malware trick

---

## 🧠 What to do next

* Check **what process started after opening file**

---

# 🔁 SOC Investigation Steps (Very Important)

## 🧩 Always follow this:

1. Detect unusual activity
2. Find attacker (IP/User)
3. Check if attack succeeded
4. Check what attacker did next

---

# 💡 Key Points to Remember

* Logs alone ≠ attack
* Pattern = attack

---

# ⚡ One Line Summary

👉 Initial Access = **Detecting attacker entry as early as possible**

---

# 🧠 Final SOC Thinking

Don’t just see alerts ❌
👉 Always ask:

* Who entered?
* How entered?
* Did they succeed?
* What did they do next?

---

✅ That’s real SOC mindset

----

# Lab Questions

## Q.1 What is the attacker's successful authentication timestamp on the Jumphost server?

<img width="1470" height="956" alt="Screenshot 2026-03-30 at 9 24 40 AM" src="https://github.com/user-attachments/assets/5011da4f-d538-41cd-9a97-4f7034b4002b" />



## Q.2 What is the name of the PHP file accessed by the attacker via the cat command after gaining successful code execution on web01?


<img width="1470" height="956" alt="Screenshot 2026-03-30 at 9 28 47 AM" src="https://github.com/user-attachments/assets/c04cccb3-2575-4a5e-92a0-cfe97feeb296" />


## Q.3 What is the name of the unusual process executed within the timeframe of update.lnk execution on WKSTN-2?

<img width="1470" height="956" alt="Screenshot 2026-03-30 at 10 08 21 AM" src="https://github.com/user-attachments/assets/2acb591c-7657-4c74-9672-af6bb5bfe750" />

---
# ⚡ Execution Tactic 

## 🌟 What is Execution?

Execution means:
👉 **Attacker runs commands or programs inside the system**

Think like this:

* Initial Access = attacker entered the system
* Execution = attacker **starts doing actions**

---

## 🎯 SOC Goal

As a SOC Analyst, your job is:

* Detect **what is running inside system**
* Check if it is **normal or malicious**
* Stop attacker **before damage**

---

## 🧠 Important Idea

Attackers usually:
❌ Do NOT install new tools
✅ Use **existing system tools**

👉 This is called:
**LOLBAS (Living off the Land Binaries)**

---

## 🚨 Common Execution Methods

| Technique          | What attacker does             |
| ------------------ | ------------------------------ |
| Command-line tools | Uses cmd.exe or PowerShell     |
| Built-in tools     | Uses certutil, mshta, regsvr32 |
| Scripting tools    | Uses Python, PHP, NodeJS       |

---

# 🔍 SCENARIO 1: Command-Line Tools (CMD / PowerShell)

## 💡 What is happening?

Attacker uses:

* cmd.exe
* powershell.exe

to run commands

---

## 🔎 What SOC looks for

* New process created
* Parent process is suspicious
* Strange commands
* Network connections

---

## 🧪 Detection Query

host.name: WKSTN-* AND winlog.event_id: 1 AND process.name: (cmd.exe OR powershell.exe)

---

## 🚨 Suspicious Signs

* cmd.exe started by:
  👉 C:\Windows\Temp\installer.exe

👉 Temp folder = risky location

---

## 💀 Why dangerous?

* Unknown file → running commands
* Could be attacker control

---

## 🔥 PowerShell Deep Check

### 🧪 Query

host.name: WKSTN-* AND winlog.event_id: 4104

---

## 🧠 What to do

* Remove noise (like Set-StrictMode)
* Focus on important scripts

---

## 🚨 Suspicious Keywords

If you see:

* invoke / iex
* -enc / encoded
* bypass
* download
* webrequest

👉 ⚠️ Possible attack

---

## 💀 Example Finding

* Invoke-Empire detected
  👉 Means attacker using **C2 (remote control tool)**

---

## 🧠 Next Step

* Check what commands were executed
* Check network activity

---

# 🔍 SCENARIO 2: Built-in Tools (LOLBAS Abuse)

## 💡 What is happening?

Attacker uses normal Windows tools for malicious work

---

## 🧪 Detection Query

host.name: WKSTN-* AND winlog.event_id: (1 OR 3) AND (process.name: (mshta.exe OR certutil.exe OR regsvr32.exe) OR process.parent.name: (mshta.exe OR certutil.exe OR regsvr32.exe))

---

## 🚨 Suspicious Tools

| Tool         | Malicious Use          |
| ------------ | ---------------------- |
| certutil.exe | Download malware       |
| regsvr32.exe | Run remote script      |
| mshta.exe    | Execute malicious code |

---

## 💀 SOC Findings

* certutil → downloaded installer.exe (Temp folder)
* regsvr32 → accessed remote file + encoded PowerShell
* mshta → executed encoded PowerShell

---

## 🧠 Why dangerous?

👉 Attacker is using **trusted system tools**
👉 Hard to detect

---

## 🔥 Next Step

* Decode PowerShell commands
* Check child processes
* Check network connections

---

# 🔍 SCENARIO 3: Scripting / Programming Tools

## 💡 What is happening?

Attacker uses:

* Python
* PHP
* NodeJS

to run commands

---

## 🧪 Detection Query

host.name: WKSTN-* AND winlog.event_id: (1 OR 3) AND (process.name: (*python* OR *php* OR *nodejs*) OR process.parent.name: (*python* OR *php* OR *nodejs*))

---

## 🚨 Suspicious Activity

* Python → starts cmd.exe
* Python → connects to external IP

---

## 💀 What it means?

👉 Possible **reverse shell**

👉 Attacker can:

* Run commands remotely
* Control system

---

## 🧠 Investigation Step

1. Get process PID
2. Track child processes

---

## 🧪 Example Query

host.name: WKSTN-* AND winlog.event_id: (1 OR 3) AND process.parent.pid: <PID>

---

## 🔥 Finding

* Python script (dev.py)
* Spawned cmd.exe
* Created more processes

👉 Full attacker control

---

# 🔁 SOC Investigation Steps (Very Important)

## 🧩 Always follow this:

1. Detect unusual process
2. Check parent-child relation
3. Check command executed
4. Check network connection
5. Track further activity

---

# 💡 Key Points to Remember

* Normal tools can be used for attacks
* Parent process matters a lot
* Encoded commands = suspicious
* Temp folder = high risk

---

# ⚡ One Line Summary

👉 Execution = **Attacker is running commands inside system**

---

# 🧠 Final SOC Thinking

Do NOT trust process name ❌
👉 Always check:

* Who started it?
* From where?
* What command?
* What happened next?

---

✅ That is real SOC mindset


---

# Lab Questions
## Q.1 Tracing back the cmd and PowerShell child processes spawned by installer.exe, what is the first command executed via cmd?

<img width="1470" height="956" alt="Screenshot 2026-03-30 at 10 17 08 AM" src="https://github.com/user-attachments/assets/853869e6-46d6-44ac-8905-3cf0534cf7c2" />
## Q.2 Using the process ID of the PowerShell process spawned by mshta.exe, what is the destination IP of the network connections made by this process?

<img width="1470" height="956" alt="Screenshot 2026-03-30 at 10 32 40 AM" src="https://github.com/user-attachments/assets/4c1828e7-ce65-4f68-982d-ba39ce52dec4" />
## Q.3 Following the cmd.exe process spawned by Python, what is the command-line value of the net.exe process?
<img width="1470" height="956" alt="Screenshot 2026-03-30 at 10 42 42 AM" src="https://github.com/user-attachments/assets/f689d2db-99a6-46fd-8b9f-00175ba569b4" />

---
# 🕵️ Defense Evasion 

## 🌟 What is Defense Evasion?

Defense Evasion means:
👉 **Attacker tries to hide their activity**

Think like this:

* Initial Access = attacker enters
* Execution = attacker runs commands
* Defense Evasion = attacker **hides evidence**

---

## 🎯 SOC Goal

As a SOC Analyst, your job is:

* Detect **hidden malicious activity**
* Find **what attacker is trying to hide**
* Continue investigation even if logs are missing

---

## 🧠 Important Idea

Attackers try to:

* Disable security tools
* Delete logs
* Hide inside normal processes
* Confuse analysts

👉 Goal = **Avoid detection**

---

## 🚨 Common Evasion Techniques

| Technique         | What attacker does               |
| ----------------- | -------------------------------- |
| Disable security  | Turns off antivirus              |
| Delete logs       | Removes evidence                 |
| Masquerading      | Uses fake process names          |
| Obfuscation       | Hides code (encoded)             |
| Process Injection | Runs code inside another process |

---

# 🔍 SCENARIO 1: Disabling Security Software

## 💡 What is happening?

Attacker tries to disable Windows Defender

---

## 🧪 Detection Query

host.name: WKSTN-* AND (*DisableRealtimeMonitoring* OR *RemoveDefinitions*)

---

## 🚨 Suspicious Commands

* DisableRealtimeMonitoring → turns off protection
* RemoveDefinitions → deletes virus signatures

---

## 💀 SOC Findings

* installer.exe → disabled Defender
* cmd.exe (from Python) → removed signatures

---

## 🧠 Why dangerous?

👉 No antivirus = attacker free to act

---

## 🔥 Next Step

* Check what attacker executed after disabling security
* Look for new malware or commands

---

# 🔍 SCENARIO 2: Log Deletion

## 💡 What is happening?

Attacker deletes system logs

---

## 🧪 Detection Query

host.name: WKSTN-* AND winlog.event_id: 1102

---

## 🚨 What SOC sees

* Event logs cleared on system

---

## 💀 Why dangerous?

* Logs = evidence
* Without logs → investigation hard

---

## 🧠 SOC Action

* Check **who deleted logs**
* Check **command used**
* Use surrounding logs for context

---

# 🔍 SCENARIO 3: Process Injection

## 💡 What is happening?

Attacker injects code into another process

---

## 🧪 Detection Query

host.name: WKSTN-* AND winlog.event_id: 8

---

## 🚨 SOC Findings

* chrome.exe → injects into explorer.exe

---

## 💀 Why dangerous?

* explorer.exe = trusted process
* Malware hides inside it

---

## 🧠 SOC Observation

* chrome.exe from user Downloads folder
* Not a normal system process

👉 Highly suspicious

---

## 🔥 Next Step

* Trace origin of chrome.exe (phishing)
* Check activity after injection

---

# 🔁 SOC Investigation Steps (Very Important)

## 🧩 Always follow this:

1. Check if security tools are disabled
2. Look for log deletion events
3. Identify suspicious processes
4. Check process relationships
5. Track attacker activity after evasion

---

# 💡 Key Points to Remember

* Attackers try to hide, not stop
* Missing logs = red flag
* Trusted processes can be abused
* Parent-child process is very important

---

# ⚡ One Line Summary

👉 Defense Evasion = **Attacker is hiding from detection**

---

# 🧠 Final SOC Thinking

Do NOT trust what you see ❌
👉 Always think:

* What is missing?
* What is hidden?
* What should be there but is not?

---

✅ That is real SOC mindset



---

# Lab Questions
## Q.1 What is the PID of the cmd.exe process that executed "powershell Set-MpPreference -DisableRealtimeMonitoring $true"?

<img width="1470" height="956" alt="Screenshot 2026-03-30 at 10 46 58 AM" src="https://github.com/user-attachments/assets/b8d16712-c6e1-42ec-ab64-50ef92c60f04" />

## Q.2 What is the PowerShell command-line argument used to clear the event logs of WKSTN-1?

<img width="1470" height="956" alt="Screenshot 2026-03-30 at 10 50 39 AM" src="https://github.com/user-attachments/assets/dd16f744-fed2-4a21-8e48-1f93d20a70af" />



## Q.3 What is the process PID of chrome.exe's target for process injection?

<img width="1470" height="956" alt="Screenshot 2026-03-30 at 10 56 21 AM" src="https://github.com/user-attachments/assets/9ecadddd-8912-4fba-954e-2a023d7ef40a" />

---
# 🔁 Persistence 

## 🌟 What is Persistence?

Persistence means:
👉 **Attacker wants to stay inside the system for a long time**

Think like this:

* Initial Access = attacker enters
* Execution = attacker runs commands
* Defense Evasion = attacker hides
* Persistence = attacker **makes a permanent backdoor**

---

## 🎯 SOC Goal

As a SOC Analyst, your job is:

* Detect **how attacker stays in system**
* Find **hidden auto-start methods**
* Remove attacker’s access completely

---

## 🧠 Important Idea

Attacker wants:
👉 Even if system restarts
👉 Even if user logs out

👉 They should still have access

---

## 🚨 Common Persistence Methods

| Technique             | What attacker does           |
| --------------------- | ---------------------------- |
| Registry modification | Adds auto-start entry        |
| Scheduled tasks       | Runs malware again and again |
| New user account      | Creates hidden admin user    |

---

# 🔍 SCENARIO 1: Scheduled Task Creation

## 💡 What is happening?

Attacker creates a task that runs automatically

---

## 🧪 Detection Query

host.name: WKSTN-* AND (winlog.event_id: 4698 OR (*schtasks* OR *Register-ScheduledTask*))

---

## 🔎 What SOC looks for

* Unknown task name
* Suspicious command
* Runs frequently (every minute)

---

## 🚨 SOC Finding

* Task name: **Windows Update** (fake name)
* Runs PowerShell command
* Runs every minute

---

## 💀 Why dangerous?

👉 Looks normal but actually malicious
👉 Executes attacker command again and again

---

## 🧠 SOC Thinking

👉 “Is task really Windows Update?” ❌
👉 Check:

* Command inside task
* Source of task

---

## 🔥 Next Step

* Trace parent process
* Find who created the task

---

# 🔍 SCENARIO 2: Registry Modification

## 💡 What is happening?

Attacker changes system settings to auto-run malware

---

## 🧪 Detection Query

host.name: WKSTN-* AND winlog.event_id: 13 AND winlog.channel: Microsoft-Windows-Sysmon/Operational AND registry.path: (*CurrentVersion\Run* OR *CurrentVersion\Explorer\User* OR *CurrentVersion\Explorer\Shell*)

---

## 🔎 What SOC looks for

* Changes in startup registry keys
* Unknown file paths
* Suspicious binaries

---

## 🚨 SOC Finding

* Registry Path:
  HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnceEx\0001\Depend\1

* Value:
  C:\Windows\Temp\installer.exe

---

## 💀 Why dangerous?

👉 This means:

* installer.exe will run on every startup

👉 System restart ≠ attacker removed

---

## 🧠 SOC Thinking

👉 “Why is a Temp folder file running at startup?” 🚨

---

## 🔥 Alternative Detection

### 🧪 Query

host.name: WKSTN-* AND winlog.event_id: 13 AND winlog.channel: Microsoft-Windows-Sysmon/Operational AND process.name: (reg.exe OR powershell.exe)

---

## 🧠 Why useful?

👉 Shows:

* Which process changed registry
* Helps find attacker tool

---

# 🔁 SOC Investigation Steps (Very Important)

## 🧩 Always follow this:

1. Look for auto-start mechanisms
2. Check scheduled tasks
3. Check registry changes
4. Identify suspicious file paths
5. Trace parent process
6. Check what happens after restart

---

# 💡 Key Points to Remember

* Normal names can be fake (e.g., Windows Update)
* Temp folder files are suspicious
* Registry changes = long-term access
* Scheduled tasks = repeated execution

---

# ⚡ One Line Summary

👉 Persistence = **Attacker ensures they stay in system**

---

# 🧠 Final SOC Thinking

Do NOT think attack is over ❌
👉 Always ask:

* Will attacker come back after restart?
* Is there any hidden auto-run?
* Is access completely removed?

---

✅ That is real SOC mindset




---


# Lab Questions
## Q.1 What is the name of the parent process of the cmd.exe process that executed the scheduled task creation?
<img width="1470" height="956" alt="Screenshot 2026-03-30 at 11 07 32 AM" src="https://github.com/user-attachments/assets/d54db28d-ef16-4883-b24b-69eb42fbb3f2" />
## Q.2 Using the process ID of malicious reg.exe execution, what is the value of the process command line used to execute the registry modification?


<img width="1470" height="956" alt="Screenshot 2026-03-30 at 12 13 25 PM" src="https://github.com/user-attachments/assets/05a7ef02-a543-4269-b791-36693febf278" />

---
# 📡 Command and Control 

## 🌟 What is Command and Control (C2)?

Command and Control means:
👉 **Attacker is controlling the system remotely**

Think like this:

* Initial Access = attacker enters
* Execution = attacker runs commands
* Persistence = attacker stays
* C2 = attacker **talks to system and gives commands**

---

## 🎯 SOC Goal

As a SOC Analyst, your job is:

* Detect **communication between attacker and system**
* Identify **hidden network connections**
* Stop attacker’s remote control

---

## 🧠 Important Idea

C2 is:
👉 **Two-way communication**

* Attacker → sends commands
* System → sends data back

---

## 🚨 Common C2 Methods

| Technique  | What attacker does                          |
| ---------- | ------------------------------------------- |
| DNS        | Uses DNS queries to send data               |
| Cloud Apps | Uses apps like Discord                      |
| HTTP/HTTPS | Uses web traffic (hidden in normal traffic) |

---

# 🔍 SCENARIO 1: C2 over DNS

## 💡 What is happening?

Attacker uses DNS to communicate

---

## 🔎 What SOC looks for

* Many **unique subdomains**
* Strange query types (TXT, MX, CNAME)
* Long or encoded subdomains

---

## 🧪 Detection Query

network.protocol: dns AND NOT dns.question.name: *arpa

---

## 🚨 SOC Finding

* Domain: **golge.xyz**
* 2000+ unique subdomains
* Hex-like strings

---

## 💀 Why dangerous?

👉 Normal DNS is simple
👉 This looks like **data transfer**

---

## 🧠 Extra Suspicious Signs

* Queries sent to unknown DNS server
* Not using normal company DNS

---

## 🔥 Process Correlation

### 🧪 Query

host.name: WKSTN-1* AND destination.ip: 167.71.198.43 AND destination.port: 53

---

## 🚨 Result

* Process: **nslookup.exe**

---

## 💀 Meaning

👉 DNS used for C2 communication

---

## 🧠 Next Step

* Check parent process
* Check commands executed

---

# 🔍 SCENARIO 2: C2 over Cloud Apps

## 💡 What is happening?

Attacker uses trusted apps like Discord

---

## 🔎 What SOC looks for

* Rare or unusual cloud domains
* Example: discord.gg

---

## 🚨 SOC Finding

* WKSTN-1 connecting to Discord
* Not normal usage

---

## 🔥 Process Correlation

### 🧪 Query

host.name: WKSTN-1* AND *discord.gg*

---

## 🚨 Result

* Process: C:\Windows\Temp\installer.exe

---

## 💀 Why dangerous?

👉 Temp folder file using Discord
👉 Likely malicious

---

## 🔥 Next Step

Check child processes:

host.name: WKSTN-1* AND winlog.event_id: 1 AND process.parent.executable: "C:\Windows\Temp\installer.exe"

---

## 🧠 Finding

* installer.exe → runs cmd.exe

👉 Remote commands confirmed

---

# 🔍 SCENARIO 3: C2 over HTTP/HTTPS

## 💡 What is happening?

Attacker uses web traffic to communicate

---

## 🔎 What SOC looks for

* Many requests to same domain
* High outbound traffic
* Strange endpoints

---

## 🧪 Detection Query

network.protocol: http AND network.direction: egress

---

## 🚨 SOC Finding

* Domain: cdn.golge.xyz
* Many GET requests
* Same endpoints

---

## 💀 Why dangerous?

👉 Continuous communication
👉 Likely active C2 channel

---

## 🔥 Deep Check

### 🧪 Query

host.name: WKSTN-* AND network.protocol: http AND network.direction: egress AND destination.domain: cdn.golge.xyz

---

## 🧠 Observation

* Same endpoints across systems
  👉 Same malware used

---

## 🔥 Process Correlation

### 🧪 Query

host.name: WKSTN-* AND *cdn.golge.xyz*

---

## 🚨 Result

* PowerShell used for connection

---

## 💀 Meaning

👉 Malware using PowerShell for C2

---

# 🔁 SOC Investigation Steps (Very Important)

## 🧩 Always follow this:

1. Identify unusual network traffic
2. Check domain and IP
3. Check frequency of communication
4. Correlate with process
5. Track commands executed

---

# 💡 Key Points to Remember

* Normal protocols can be abused
* DNS can carry hidden data
* Trusted apps can be misused
* Repeated connections = suspicious

---

# ⚡ One Line Summary

👉 Command & Control = **Attacker is remotely controlling the system**

---

# 🧠 Final SOC Thinking

Do NOT trust network traffic ❌
👉 Always ask:

* Why is system talking to this domain?
* Is this normal behavior?
* Which process is making connection?
* What commands are being executed?

---

✅ That is real SOC mindset

---

# Lab Questions
## Q.1 What is the link downloaded using PowerShell to establish the C2 over DNS?
<img width="1470" height="956" alt="Screenshot 2026-03-30 at 5 29 59 PM" src="https://github.com/user-attachments/assets/bfe04841-adef-4f9e-9f94-576ca067baa7" />

## Q.2 After investigating C2 over Discord events, what command is used to download the malicious dev.py python script?
<img width="1470" height="956" alt="Screenshot 2026-03-30 at 5 33 19 PM" src="https://github.com/user-attachments/assets/d6b2204c-812b-4468-ba0f-5ddc0d6fb17d" />


## Q.3 What is the name of the process that is also associated with cdn[.]golge[.]xyz?

<img width="1470" height="956" alt="Screenshot 2026-03-30 at 5 34 44 PM" src="https://github.com/user-attachments/assets/7e31283b-130c-49bf-96cb-886f001cfc82" />

