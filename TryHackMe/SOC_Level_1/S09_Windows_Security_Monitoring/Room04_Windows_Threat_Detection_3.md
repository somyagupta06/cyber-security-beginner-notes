# Windows Attack & Detection Notes 

Imagine a computer is like a house.

Attacker = thief  
User = house owner  
SOC Analyst = security guard  

--------------------------------------------

## 1. What is RDP?

RDP (Remote Desktop Protocol) is like remote control of a computer.

If attacker knows the password, they can open the computer from far away.

Problem:
If password is changed or RDP is disabled, attacker loses access.
<img width="1238" height="483" alt="Screenshot 2026-02-21 at 9 21 13 AM" src="https://github.com/user-attachments/assets/b5944d1a-9293-4d15-8459-d1185739246b" />

--------------------------------------------

## 2. What is C2 (Command and Control)?

C2 is like a secret phone between attacker and victim computer.

How it works:

Step 1: Attacker sends phishing file.
Step 2: User opens it.
Step 3: Malware connects back to attacker.
Step 4: Attacker controls system secretly.

Important:
Victim computer calls attacker.
Attacker does not call victim.

That is why it is harder to detect.
<img width="1219" height="471" alt="Screenshot 2026-02-21 at 9 21 23 AM" src="https://github.com/user-attachments/assets/2d9a7cd6-67a3-4e16-82e0-6295296afb99" />

--------------------------------------------

## 3. Sysmon (Very Important for Detection)

Sysmon records what happens in the computer.

You check it in:
Event Viewer → Sysmon → Operational

Important Event IDs:

Event ID 1  = Process Created  
Event ID 3  = Network Connection  
Event ID 11 = File Created  
Event ID 13 = Registry Changed  
Event ID 10 = Process tried to access another process  

--------------------------------------------

## 4. Example Attack (Step by Step)

Example:

PowerShell downloads update.exe  
File saved in AppData  
File executed  
File connects to internet  

Attack chain:

Event 3  → PowerShell connects to IP  
Event 1  → update.exe started  
Event 11 → File created in AppData  

If you see all together → Very suspicious.

--------------------------------------------

## 5. Why AppData Folder is Suspicious?

Normal software installs in:

C:\Program Files

Malware hides in:

C:\Users\Username\AppData\Local
C:\Users\Username\AppData\Roaming
C:\Users\Username\AppData\Temp

Because normal users do not check there.

--------------------------------------------

## 6. What is Persistence?

Persistence means attacker makes permanent access.

Like:
If thief enters house once,
Then makes a duplicate key,
Now even if door lock changes,
Thief can still enter.
<img width="1222" height="361" alt="Screenshot 2026-02-21 at 9 21 43 AM" src="https://github.com/user-attachments/assets/94530ae1-5846-48d7-aead-17aa178162b2" />

--------------------------------------------

## 7. How Attackers Create Persistence?

Method 1: Create new hidden user  
Method 2: Add that user to Administrators  
Method 3: Reset old user password  
Method 4: Add file to Startup  
Method 5: Add registry run key  

--------------------------------------------

## 8. Important Security Event IDs

Security Event 4720 = New user created  
Security Event 4732 = User added to group  
Security Event 4724 = Password reset  
Security Event 4624 = Login success  

If you see:

4720 + 4732 + 4624

That means attacker created admin backdoor.

--------------------------------------------

## 9. How to Investigate Properly

Never check one event alone.

Always check:

Who did it?
At what time?
From which IP?
What happened before?
What happened after?

Think like a story.

--------------------------------------------

## 10. Simple Difference

Normal Activity:
Browser downloads file.
User opens PDF.
No strange network connection.

Suspicious Activity:
PowerShell downloads EXE.
File saved in AppData.
File connects to unknown IP.
New admin user created.

That is attack.

--------------------------------------------

## 11. Final Simple Understanding

Initial Access  = Attacker enters  
Execution       = Malware runs  
Persistence     = Attacker stays permanently  
C2              = Attacker controls remotely  

Your job as SOC Analyst:
Connect the dots.
Do not panic.
Follow the timeline.

--------------------------------------------
# Windows Persistence : Tasks and Services

--------------------------------------------

## 1. What is Persistence?

Persistence means:
Malware stays active even after system restart.

If computer reboots,
malware should still run automatically.

--------------------------------------------

# 2. Windows Service Persistence

## What is a Service?

A service is a program that runs automatically
when Windows starts.

Example:
Antivirus
Windows Update

Attacker can create a fake service
to run malware at startup.

--------------------------------------------

## How Attacker Creates Service

sc.exe create ServiceName binpath= C:\malware.exe start= auto

Meaning:
Windows will run malware.exe automatically
every time system starts.

--------------------------------------------

## How to Detect Malicious Service

### Important Event IDs:

Security Event ID 4697  
System Event ID 7045  
Sysmon Event ID 1 (sc.exe usage)

--------------------------------------------

## What to Check Inside Event

Service Name  
Service File Path  
Service Account  

If file path contains:
AppData
Temp
Downloads
Random folder

→ Suspicious

--------------------------------------------

# 3. Scheduled Task Persistence

## What is a Scheduled Task?

It tells Windows:
Run this program at specific time
or at system startup.

--------------------------------------------

## How Attacker Creates Task

schtasks.exe /create /tn TaskName /tr C:\malware.exe /sc onstart

Meaning:
Malware runs every time system starts.

--------------------------------------------

## How to Detect Malicious Task

### Important Event IDs:

Security Event ID 4698  
Sysmon Event ID 1 (schtasks.exe /create)

--------------------------------------------

## What to Check Inside Event 4698

Task Name  
Command (program path)

If command runs:
C:\malware.exe
AppData\random.exe

→ Suspicious

--------------------------------------------

# 4. Process Tree Logic

For Services:

services.exe  
  → svchost.exe (sometimes)  
      → malware.exe  

For Scheduled Tasks:

services.exe  
  → svchost.exe -s Schedule  
      → malware.exe  

If malware starts from these parents,
it is persistence.

--------------------------------------------

# 5. Investigation Steps (Quick Checklist)

For Service Question:

1. Filter Event ID 4697 or 7045  
2. Find newly created service  
3. Check file path  
4. Match with malware  
5. Write Service Name  

--------------------------------------------

For Scheduled Task Question:

1. Filter Event ID 4698  
2. Check Task Name  
3. Check Command path  
4. Match with malware file  
5. Write Task Name  

--------------------------------------------

# 6. Lab Flag Question Logic

If question asks:

"What flag do you get after running malware?"

Steps:

1. Find malware path from service/task log  
2. Go to that folder  
3. Run the .exe file  
4. Copy flag shown (Example: THM{something})

--------------------------------------------

# 7. Key Difference

Service:
Runs as system service in background.

Scheduled Task:
Runs based on time or startup trigger.

Both survive reboot.

--------------------------------------------

# Final Memory Trick

Service → Event 4697  
Task → Event 4698  

4697 = Service  
4698 = Task  

--------------------------------------------
# Task 5 – Persistence: Run Keys and Startup
(Simple Revision Notes)

---
## 1️⃣ Big Idea


Sometimes malware should run:

✔ Only when a user logs in  
❌ Not when the whole system boots  

For this, attackers use:

1. Startup Folder
2. Run Registry Keys

Both run when USER logs in.


## 2️⃣ Startup Folder Persistence


What is Startup Folder?

It is a special folder.
Any program inside it runs automatically
when the user logs in.

----------------------------------------

- 📂 Path (Per User):

C:\Users\<User>\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup

📂 Path (All Users):

C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Startup

----------------------------------------

- 💀 Attacker Trick:

Copy malware into Startup folder.

Example idea:

copy C:\malware.exe  
C:\Users\Bob\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup\malware.exe

Now:

User logs in  
→ explorer.exe starts  
→ malware.exe runs automatically

----------------------------------------

## 🔍 How to Detect Startup Persistence

✔ Sysmon Event ID 11  
(File created)

Look for:

TargetFilename:
...\Start Menu\Programs\Startup\malware.exe

✔ Process Tree After Reboot

explorer.exe  
   ↓  
cmd.exe (sometimes)  
   ↓  
malware.exe  

----------------------------------------

## ⚠ What Looks Suspicious?

- .exe or .cmd in Startup folder
- File from Downloads copied there
- File created shortly before reboot

## 3️⃣ Run Key Persistence (Registry)
Instead of copying file,
attacker creates a registry entry.

When user logs in,
Windows checks the "Run" key
and runs all programs listed there.

----------------------------------------

- 📌 Run Key Locations

Per User:
HKCU\Software\Microsoft\Windows\CurrentVersion\Run

All Users:
HKLM\Software\Microsoft\Windows\CurrentVersion\Run

----------------------------------------

- 💀 Attacker Trick:

Add registry value pointing to malware.

Example idea:

reg add  
"HKCU\Software\Microsoft\Windows\CurrentVersion\Run"  
/v BadKey  
/t REG_SZ  
/d "C:\malware.exe"

Meaning:

Run Key Name = BadKey  
Program = C:\malware.exe  

----------------------------------------

## 🔍 How to Detect Run Key Persistence

✔ Sysmon Event ID 13  
(Registry value set)

Look at:

TargetObject:
...\Run\BadKey

Details:
C:\malware.exe

✔ After Reboot:

explorer.exe  
   ↓  
malware.exe  

----------------------------------------

## ⚠ What Looks Suspicious?

- Run key pointing to:
  - Public folder
  - Downloads folder
  - AppData
- Weird key name
- Recently created registry entry

## 4️⃣ Startup vs Run Key (Difference)

Startup Folder:
- File is physically copied.
- Easy to see in File Explorer.

Run Key:
- No file copied.
- Only registry value created.
- Harder to notice.

Both:
- Run at user login.
- Do NOT require system reboot.
- Are very common persistence methods.

## 5️⃣ How to Analyze in Lab
Step 1:
Open "Before Reboot" log.

Look for:
- Event ID 11 (file in Startup)
- Event ID 13 (registry change)

Step 2:
Open "After Reboot" log.

Look for:
- Event ID 1 (process create)
- Parent process (usually explorer.exe or cmd.exe)

Step 3:
Check:
- What file runs?
- From which path?
- What is the Run key name?
- What is the Startup file name?
## 6️⃣ Important Event IDs

Event ID 1  → Process Create  
Event ID 11 → File Created  
Event ID 13 → Registry Value Set  

## 7️⃣ Investigation Questions to Ask
✔ Was a file placed in Startup?
✔ Was a Run key created?
✔ What is the value name?
✔ What path does it execute?
✔ Does it run after reboot?
✔ Who created it?

---

🔥 Final Concept

Startup Folder = File-based persistence  
Run Key = Registry-based persistence  

Both are very common.
Both run at user login.
Both are heavily used by malware.

---
# Task 6 – Impact & Threat Detection 

---
## PART 1 – NEED FOR PERSISTENCE (IMPACT SIDE)


🧠 Big Question:
Why do attackers stay inside the system?
Why not steal data and leave?

Because long-term access = bigger damage.

----------------------------------------------------
1️⃣ Add Host to a Botnet
----------------------------------------------------

Attacker infects one machine.

Instead of leaving,
they keep malware running.

Now that machine becomes:

- Remotely controlled
- Part of a botnet
- Used for future attacks

What attacker can do:

- Launch DDoS
- Send spam
- Mine cryptocurrency
- Spread malware

Persistence makes the victim a permanent weapon.

----------------------------------------------------
2️⃣ Long-Term Spying (Espionage)
----------------------------------------------------

Some attacks are not about money.

They are about spying.

Attackers may stay inside:

- For months
- Sometimes more than a year

They monitor:

- Emails
- Internal documents
- Critical infrastructure

If malware dies after reboot,
they lose access.

So persistence = silent long-term surveillance.

----------------------------------------------------
3️⃣ Entry Point to Entire Network
----------------------------------------------------

In companies:

One PC connects to:

- File servers
- Active Directory
- Domain Controller
- Other employee systems

Attacker enters through one machine,
then slowly explores the whole network.

This process can take:

- 20–30 days
- Sometimes months

Without persistence,
they would lose access before completing attack.

----------------------------------------------------
4️⃣ Active Directory & Ransomware
----------------------------------------------------

Active Directory controls:

- Users
- Passwords
- Permissions
- Servers
- Policies

If attacker gains Domain Admin,
they control everything.

Then they can:

- Deploy ransomware to all systems
- Encrypt servers
- Encrypt backups
- Print ransom notes everywhere

This is the IMPACT stage.

----------------------------------------------------
5️⃣ Modern Ransomware (Double Extortion)
----------------------------------------------------

Modern attackers:

1. Steal data
2. Encrypt data
3. Threaten to leak data

Even if company restores backup,
attacker says:

"Pay or we publish your data."

This increases pressure.
## PART 2 – THREAT DETECTION RECAP (THE BIG ATTACK MAP)

Every attack follows stages.

----------------------------------------------------
1️⃣ Initial Access
----------------------------------------------------

How attacker enters:

- Phishing
- RDP
- Exploit
- USB

----------------------------------------------------
2️⃣ Execution
----------------------------------------------------

They run code:

- PowerShell
- cmd.exe
- Malware

Log:
Sysmon Event ID 1

----------------------------------------------------
3️⃣ Persistence
----------------------------------------------------

They survive reboot using:

- Service
- Scheduled Task
- Run Key
- Startup Folder
- Backdoor User

----------------------------------------------------
4️⃣ Privilege Escalation
----------------------------------------------------

They become:

Administrator or Domain Admin.

----------------------------------------------------
5️⃣ Credential Access
----------------------------------------------------

They steal:

- Passwords
- Hashes
- Tokens

----------------------------------------------------
6️⃣ Discovery
----------------------------------------------------

They explore:

- Users
- Servers
- Network layout

----------------------------------------------------
7️⃣ Lateral Movement
----------------------------------------------------

They move from:

One PC → Other PCs → Domain Controller

----------------------------------------------------
8️⃣ Collection
----------------------------------------------------

They gather:

- Documents
- Databases
- Sensitive files

----------------------------------------------------
9️⃣ Command & Control (C2)
----------------------------------------------------

Infected machine talks to attacker server.

Log:
Sysmon Event ID 3

----------------------------------------------------
🔟 Exfiltration
----------------------------------------------------

They send data outside.

----------------------------------------------------
1️⃣1️⃣ Impact
----------------------------------------------------

Final damage:

- Ransomware
- Data destruction
- Financial theft
- Service shutdown

## 🔥 Key Learning
Persistence is not the final goal.

It enables:

- Long-term spying
- Network takeover
- Ransomware deployment

If SOC detects early stages,
they prevent final impact.

