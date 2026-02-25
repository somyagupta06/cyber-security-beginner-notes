# Discovery Phase 

## 1. What is Discovery?

Discovery is the phase where an attacker
collects information after gaining access
to a Linux system.

Goal:
Understand the system before continuing the attack.

<img width="1322" height="237" alt="Screenshot 2026-02-24 at 11 10 23 AM" src="https://github.com/user-attachments/assets/a5586dc3-06f3-4f29-81a7-d94c6feff711" />

---

## 2. Typical Attack Flow

Stage 1:
Botnet gains initial access automatically.

Stage 2:
Human attacker connects to the system.

Stage 3:
Attacker manually performs actions.

Discovery usually starts in Stage 2.


---

## 3. Why Attackers Perform Discovery?

Attackers want to know:

- Where am I?
- Which OS is running?
- Which user am I?
- Do I have admin access?
- What services are running?


---

## 4. OS and Filesystem Discovery

Purpose:
Identify system details.

Common commands check:
- current directory
- operating system
- hostname
- environment information


---

## 5. User and Group Discovery

Purpose:
Understand user accounts and privileges.

Attackers look for:
- current logged-in user
- admin or sudo users
- login history
- available accounts


---

## 6. Process and Network Discovery

Purpose:
Find running services and network access.

Attackers check:
- running processes
- open ports
- active connections
- internal network routes


---

## 7. Cloud or Sandbox Discovery

Purpose:
Detect virtual machines or security monitoring.

Attackers verify:
- virtual environment
- security tools
- sandbox detection


---

## 8. Important Command: whoami

whoami shows the current user identity.

Example output:
root
ubuntu
www-data

Attackers often execute this command
immediately after gaining access.


---

## 9. SOC Detection Tip

Legitimate applications rarely execute whoami.

Frequent whoami execution may indicate
attacker activity.

SOC teams often create alerts for this command.


---

## 10. When Discovery May Be Skipped

Discovery may not occur if:

- attacker already knows the target
- automated malware installs directly
- cryptominer deployment attack


---
## Lab Questions
### Q.1 Run systemd-detect-virt to detect the system's cloud. What is the command's output you discovered?
<img width="450" height="49" alt="Screenshot 2026-02-24 at 11 08 38 AM" src="https://github.com/user-attachments/assets/402974a6-87fc-4285-8f7d-739f97184653" />

### Q.2 Now run ps aux and look for EDR or antivirus processes. What is the full path to the detected antimalware binary?
<img width="1470" height="956" alt="Screenshot 2026-02-24 at 11 09 28 AM" src="https://github.com/user-attachments/assets/855e8079-3538-4515-a834-7e2eaa6aa613" />


---
## 11. One Line Summary

Discovery phase helps attackers understand
a compromised Linux system before performing
further malicious actions.

---
# Specialized Discovery (Linux SOC Notes)

## 1. What is Specialized Discovery?

After basic discovery, attackers run
goal-focused commands to achieve
specific objectives.

Purpose:
Collect useful data or prepare next attack stage.


---

## 2. Types of Specialized Discovery

### A. Credential and Data Theft

Goal:
Find passwords and sensitive information.

Attackers search for:
- saved credentials
- private SSH keys
- environment files
- command history

Common activity:
Searching user directories and secret files.


---

### B. Crypto Mining Preparation

Goal:
Check if system is suitable for cryptocurrency mining.

Attackers analyze:
- CPU information
- memory usage
- system performance

If system resources are strong,
crypto miner may be installed.


---

### C. Internal Network Scanning

Goal:
Find additional victims inside network.

Attackers attempt to:
- ping internal systems
- scan IP ranges
- locate open services like SSH

This enables lateral movement.


---

## 3. Detecting Specialized Discovery

Security tools like auditd or SIEM
can log command executions.

Analysts search logs for
Discovery-related commands.

<img width="1336" height="331" alt="Screenshot 2026-02-24 at 11 39 00 AM" src="https://github.com/user-attachments/assets/87310c6c-ca00-464c-99b6-08384f81c20d" />

---

## 4. Main Detection Challenge

Same commands can be executed by:

- attackers
- IT administrators
- legitimate services

Command alone is not proof of attack.


---

## 5. Importance of Context

Behavior becomes suspicious when:

- web server runs discovery commands
- unknown scripts search for secrets
- user suddenly scans the network


---

## 6. Using Process Tree for Detection

Step 1:
Identify suspicious command.

Step 2:
Check parent process using PID.

Step 3:
Trace command origin.

Step 4:
Review other child processes.


---

## 7. Example Investigation

Discovery command:
whoami

Parent process:
bash /tmp/lp.sh

Further activity:
find /home *secret*

Conclusion:
Malicious script performing credential discovery.


---

## 8. SOC Detection Logic

Suspicious command
        ↓
Trace parent process
        ↓
Identify script or application
        ↓
Confirm attacker objective


---
## Lab Questions
### Q.1 What is the path of the script that initiated the "hostname" command?

<img width="1450" height="465" alt="Screenshot 2026-02-24 at 11 27 14 AM" src="https://github.com/user-attachments/assets/912a9565-f103-4002-acbe-6b35406f6a32" />
### Q.2 What was the last Discovery command launched by the script?

ausearch -i --ppid 3771
<img width="1470" height="956" alt="Screenshot 2026-02-24 at 11 31 06 AM" src="https://github.com/user-attachments/assets/ef8dd4be-0e5b-4e05-9a6b-86a86a6acec2" />

### Q.3 Looking at the script content, what's the email of the script author?
<img width="1470" height="956" alt="Screenshot 2026-02-24 at 11 37 33 AM" src="https://github.com/user-attachments/assets/ddd1952c-9875-44e0-b92c-1e5f5a0c48f8" />

## 9. One Line Summary

Specialized Discovery helps attackers
search for credentials, system resources,
or new victims after gaining access.

---
# Hack and Forget Attacks (Linux SOC Notes)

## 1. What are Hack and Forget Attacks?

Hack and Forget attacks are large-scale attacks
focused on quick profit.

Attackers:
- gain access
- install malware
- leave the system

No long-term interaction required.


---

## 2. Typical Attack Flow

Scan Internet
      ↓
Find vulnerable system
      ↓
Gain access (SSH or service exploit)
      ↓
Perform quick discovery
      ↓
Install malware
      ↓
Attacker leaves


---

## 3. Main Attacker Goals

### A. Cryptominer Installation

Purpose:
Use victim CPU or GPU to mine cryptocurrency.

Indicators:
- high CPU usage
- slow system performance


---

### B. Botnet Enrollment

Purpose:
Add system to botnet networks
(e.g., Mirai or Mozi).

Used for:
- DDoS attacks
- scanning new victims
- spreading malware


---

### C. Proxy Usage

Purpose:
Use victim system to:
- send phishing emails
- host malware
- hide attacker traffic


---

## 4. Ingress Tool Transfer

Ingress Tool Transfer means
downloading or transferring malware
into the victim system.


---

## 5. Common File Transfer Methods

### Wget
Downloads files directly from websites.

Used to download malware or miners.


### Curl
Requests web content and saves files.

Often used to download backdoors.


### SSH File Transfer (SCP / SFTP)
Transfers files between systems using SSH.


---

## 6. File Transfer Scenarios

### Option 1: Attacker Connects to Victim

Attacker uploads file via SSH.

Detection:
Check SSH login events in:

/var/log/auth.log


---

### Option 2: Victim Connects to Attacker

Victim downloads file from attacker.

Detection:
Look for file transfer commands
in auditd process logs.


---

## 7. Additional Detection Methods

### Network Traffic Indicators
- downloads from suspicious IPs
- unknown domains
- public hosting services


### File System Indicators
- new files in /tmp or /var/tmp
- suspicious filenames
- exploit or shell files


### Antivirus / EDR Alerts
Security tools may detect
malicious files or processes.


---

## 8. Important SOC Concept

If malware appears without
visible download commands,
check for suspicious SSH logins.


---
## Lab Questions
### Q.1 From which domain was the Elastic agent downloaded?
<img width="1465" height="494" alt="Screenshot 2026-02-24 at 11 48 46 AM" src="https://github.com/user-attachments/assets/cd39f9f6-bbc5-4f07-8db0-7b132adccde0" />

### Q.2 What is the full path to the downloaded "helper.sh" script?
<img width="1470" height="273" alt="Screenshot 2026-02-24 at 11 49 45 AM" src="https://github.com/user-attachments/assets/67d7bc2b-83df-458e-a817-375306d8d5f3" />

### Q.3 Which of the downloaded files is more likely to be malicious: The one downloaded with curl or wget?
---
## 9. One Line Summary

Hack and Forget attacks quickly install
malware such as cryptominers or botnet agents
and abandon the compromised Linux system.

--- 
# Dota3 Malware Analysis (Linux SOC Notes)

## 1. What is Dota3 Malware?

Dota3 is a cryptominer malware
that infects Linux systems using weak SSH passwords.

Goal:
Use victim system resources to mine cryptocurrency.


---

## 2. Initial Access

Attack Steps:

1. Botnet scans Internet for open SSH servers.
2. Targets root user accounts.
3. Performs password brute-force attacks.
4. Weak password guessed.
5. Attacker logs in via SSH.


---

## 3. Discovery Phase

After login, attacker gathers system information.

Main objective:
Check if system is suitable for crypto mining.

Typical checks:
- CPU information
- RAM size
- system architecture
- active users
- scheduled tasks


SOC Indicator:
Multiple discovery commands executed quickly.


---

## 4. Persistence Phase

Attacker ensures long-term access.

### Password Change
Attacker changes password of compromised user
to prevent access by others.

### SSH Key Replacement
Steps performed:
- delete existing .ssh directory
- create new .ssh folder
- add attacker SSH key
- restrict permissions

Result:
System owner locked out,
attacker keeps permanent access.


---

## 5. Unique Malware Indicator

String used in attack:

mdrfckr

Can be used as detection signature.


---

## 6. Detection Using Auth Logs

Check successful SSH logins:

/var/log/auth.log

Look for:
- password-based login
- external IP addresses
- unknown login sources


---

## 7. Detection Using Auditd Logs

Search for execution of discovery commands:

Examples:
uname
lscpu
free
crontab
whoami

Trace command origin using process tree.


---

## 8. Attack Timeline

SSH brute force
        ↓
Successful login
        ↓
System discovery
        ↓
Password changed
        ↓
SSH keys replaced
        ↓
Cryptominer activity


---
## Lab Questions
### Q.1 Which IP address managed to brute-force the exposed SSH?
<img width="1389" height="184" alt="Screenshot 2026-02-24 at 12 05 49 PM" src="https://github.com/user-attachments/assets/7c72270d-e815-497d-af07-bf12ffc871b0" />

### Q.2 Which command did the attacker use to list the last logged-in users?
<img width="1470" height="956" alt="Screenshot 2026-02-24 at 12 10 08 PM" src="https://github.com/user-attachments/assets/f18ee076-7fbf-4063-8ece-617d20e619d7" />

### Q.3 Which three EDR processes did the attacker look for with "egrep"?

## 9. One Line Summary

Dota3 malware gains access through weak SSH passwords,
performs system discovery, establishes persistence,
and uses the Linux system for cryptocurrency mining.
---
# Dota3 Cryptominer Setup (Linux SOC Notes)

## 1. Attack Stage Overview

After gaining access and persistence,
attackers install malware to generate profit.

Main goal:
Install cryptominer and spread infection.


---

## 2. Malware Transfer (Ingress Tool Transfer)

Attackers already have SSH access.

They upload malware using SCP:

scp dota3.tar.gz ubuntu@victim:/tmp

Malware archive is transferred
to the /tmp directory.


---

## 3. Why /tmp Directory?

Attackers prefer /tmp because:

- used for temporary files
- rarely monitored
- easy to hide malware
- files may avoid attention


---

## 4. Hidden Malware Directory Creation

Attackers create hidden folders:

/tmp/.X26-unix

Characteristics:
- starts with "."
- looks like system directory
- hides malicious activity


---

## 5. Malware Extraction

Archive is unpacked:

tar xf dota3.tar.gz

Final malware location:

/tmp/.X26-unix/.rsync/c

Directory names mimic legitimate services.


---

## 6. Executed Malware Components

### A. tsm (Network Scanner)

Purpose:
Scan internal networks for new victims.

Targets:
192.168.* network  
172.16.* network

Used for lateral movement.


---

### B. initall (Cryptominer)

Purpose:
Run XMRig cryptominer.

Effect:
- high CPU usage
- resource abuse
- cryptocurrency mining


---

## 7. Use of nohup Command

Malware executed using nohup.

Meaning:
Processes continue running
even after SSH session ends.

Ensures persistent background execution.


---

## 8. Attack Completion Flow

SSH access gained
        ↓
Malware uploaded
        ↓
Hidden folder created
        ↓
Malware extracted
        ↓
Network scanner executed
        ↓
Cryptominer started


---

## 9. Detection Indicators

### Auditd Logs
- hidden folders in /tmp
- dota3.tar.gz creation
- nohup command execution


### Network Traffic
- SSH scans across internal networks
- abnormal connection attempts


### EDR Alerts
- detection of XMrig cryptominer binaries


---
## Lab Questions
### Q.2 What was the full command line of the cryptominer launch?
<img width="1458" height="557" alt="Screenshot 2026-02-25 at 6 06 41 AM" src="https://github.com/user-attachments/assets/5a86aaf5-2b6a-4eee-a640-9df7c6e1e5ca" />


## 10. One Line Summary

Dota3 attackers upload malware via SSH,
hide it in temporary directories,
scan internal networks,
and run a background cryptominer
to generate profit.
