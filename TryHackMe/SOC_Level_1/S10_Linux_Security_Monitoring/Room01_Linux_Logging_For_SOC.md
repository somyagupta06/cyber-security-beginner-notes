# Linux Logging Basics (SOC Analyst Notes)

## 1. What is Logging?

Logging means:

A computer writes down everything it does in files.

Example:
- User login
- System start
- Program running
- Errors
- Scheduled tasks

Just like a school diary records daily activities,
Linux keeps a diary called **logs**.

--------------------------------------------------

## 2. Windows vs Linux Logs

Windows:
- Uses Event Viewer
- Structured logs
- Event IDs available
- GUI based

Linux:
- Logs stored as simple text files
- No Event IDs
- Readable using normal text view
- Mostly command-line based

Important Idea:
Linux logs = normal text files

--------------------------------------------------

## 3. Where are Linux Logs Stored?

Most logs are stored inside:

/var/log

Think of it as:
Main folder containing system history.

--------------------------------------------------

## 4. Important Log Files

syslog
→ General system activity

auth.log
→ User login and authentication

kern.log
→ Kernel (core system) events

dpkg.log
→ Software installation history

Rule:
If you don't know where to look,
start with syslog.

--------------------------------------------------

## 5. Understanding a Log Line

Example log:

2025-08-13 14:05:01 server CRON command executed

Breakdown:

Time → When event happened
Host → Machine name
Process → Who performed action
Message → What happened

SOC analysts mainly check:
Process + Action

--------------------------------------------------

## 6. Problem: Too Many Logs

A system can have thousands of log lines.

Reading everything is impossible.

So analysts FILTER logs.

--------------------------------------------------

## 7. Log Filtering Idea

Instead of reading all logs,
search only important words.

Example searches:

search CRON
search login
search ssh
search sudo
search session

This helps find useful events quickly.

--------------------------------------------------

## 8. Finding Unknown Logs (Investigation Trick)

Situation:
You want login activity
but don't know file location.

Solution:
Search keywords inside all logs.

Common investigation words:

login
auth
session
user

This method is called:
Log Hunting

--------------------------------------------------

## 9. Time Synchronization Concept

Computers must have correct time.

They contact special servers called:

Time Servers (NTP Servers)

Service responsible:
systemd-timesyncd

When syncing happens,
logs record the contacted server domain.

Example:
time.google.com

That domain becomes the answer.

--------------------------------------------------

## 10. How to Solve Log Questions (SOC Method)

Step 1:
Read question carefully.

Step 2:
Find important keyword.

Step 3:
Guess related service.

Step 4:
Check correct log file.

Step 5:
Search keyword.

Step 6:
Extract required information.

Q.1 Which time server domain did the VM contact to sync its time?

1. We have keywords to detect:
- time
- server
- domain
- sync
indicating NTP (Network Time Protocol)

2. We have to think which service is related to time sync..
- In Linux time sync is usually done by-->
   - systemd-timesyncd
   - ntpd
   - chronyd

3. Now using commands:
   - grep timesync /var/log/syslog
   
<img width="1470" height="956" alt="Screenshot 2026-02-22 at 8 41 37 AM" src="https://github.com/user-attachments/assets/fca33791-ecd7-45c0-8272-20262e19c32e" />

Q.2 What is the kernel message from Yama in /var/log/syslog?
<img width="1437" height="263" alt="Screenshot 2026-02-22 at 8 42 54 AM" src="https://github.com/user-attachments/assets/f6fdf51c-c39d-4841-9445-84cf4ee1c214" />

--------------------------------------------------

## 11. Keyword Mapping (Very Important)

login → auth.log  
ssh → ssh logs  
cron → scheduled tasks  
time sync → timesync / ntp  
sudo → privilege usage  

--------------------------------------------------

## 12. Important Reality About Linux Logs

Linux systems are different.

Different distributions may:
- Change log format
- Change location
- Disable logs
- Rename files

So analysts must adapt.

--------------------------------------------------

## 13. SOC Analyst Mindset

Beginner:
Reads logs.

SOC Analyst:
Searches logs with purpose.

Goal:
Find suspicious activity fast.

--------------------------------------------------

## Quick Revision Summary

Logs = system diary  
/var/log = log storage  
syslog = general events  
auth.log = login activity  
Filtering = searching keywords  
Time sync = contact with time server  

Always think:
"What activity happened?"
Then search related keyword.

---
# Linux Authentication Logs (SOC Analyst Notes)

--------------------------------------------------

## 1. What is auth.log?

auth.log is one of the MOST IMPORTANT Linux log files.

Location:
/var/log/auth.log

(RHEL systems use /var/log/secure)

This file records:

- User logins
- Logouts
- SSH access
- sudo usage
- User creation & deletion
- Password changes
- Privilege escalation

Simple Meaning:
auth.log shows WHO accessed the system and WHAT permissions they used.

--------------------------------------------------

## 2. Log Structure

Example log contains:

Time → When event happened  
Hostname → Machine name  
Service → Program creating event  
Message → Actual activity  

Example idea:

Time | System | Service | Action

SOC analysts mainly focus on:
Service + Action

--------------------------------------------------

## 3. Login and Logout Events

Important keywords:

session opened  
session closed  

Meaning:

session opened → User logged IN  
session closed → User logged OUT  

Example meaning:
session opened for user bob  
= Bob logged into system

--------------------------------------------------

## 4. Types of Logins

### Local Login
User logs in directly using keyboard.

Log contains:
login:session

Meaning:
Physical machine login.

--------------------------------------------------

### Remote Login (SSH)

Log contains:
sshd:session

Meaning:
User logged in remotely over network.

Very important for SOC monitoring.

Attackers commonly use SSH access.

--------------------------------------------------

## 5. Automated Logins (CRON)

Example keyword:
cron:session

Meaning:
System automatically executed scheduled task.

Examples:
- backups
- updates
- scripts
- malware persistence

--------------------------------------------------

## 6. SUDO Sessions (Privilege Escalation)

Example:
sudo:session opened for user root by carol

Meaning:
Normal user gained root (admin) access.

Important Concept:
User → Admin privilege escalation

SOC analysts always monitor sudo usage.

--------------------------------------------------

## 7. SSH Authentication Events

SSH logs are also stored inside auth.log.

Important keywords:

Failed password  
Accepted password  
Accepted publickey  

--------------------------------------------------

### Failed Login

Failed password for root from IP

Meaning:
Login attempt failed.

Possible brute-force attack.

--------------------------------------------------

### Successful Login

Accepted publickey for bob from IP

Meaning:
Login successful.

Check carefully:
- Username
- Source IP
- Authentication method

--------------------------------------------------

## 8. User Management Events

Linux records account changes.

Important keywords:

useradd → New user created  
userdel → User removed  
usermod → User modified  
passwd → Password changed  

--------------------------------------------------

### Security Risk Example

add 'backdoor' to group sudo

Meaning:
User received admin privileges.

This is a major security alert.

--------------------------------------------------

## 9. Commands Executed Using SUDO

auth.log can record commands run with sudo.

Keyword:
COMMAND=

Example:
COMMAND=/usr/bin/systemctl stop edr

Meaning:
User stopped security software.

Highly suspicious activity.

--------------------------------------------------

## 10. SOC Investigation Workflow

When investigating auth.log:

Step 1 → Check login activity  
Step 2 → Look for failed attempts  
Step 3 → Identify remote access (SSH)  
Step 4 → Check sudo usage  
Step 5 → Detect new users  
Step 6 → Review executed commands  

--------------------------------------------------

## 11. Common Investigation Keywords

login  
session  
sshd  
sudo  
useradd  
userdel  
passwd  
COMMAND  

--------------------------------------------------

## 12. SOC Analyst Golden Rule

auth.log answers:

Who accessed system?  
From where?  
Was access successful?  
Did user become admin?  
What actions were performed?

--------------------------------------------------
## Lab Questions
### Q.1 Which IP address failed to log in on multiple users via SSH?
<img width="1457" height="237" alt="Screenshot 2026-02-22 at 8 59 01 AM" src="https://github.com/user-attachments/assets/0660435c-66c6-479d-9b69-4a657475b265" />

### Q.2 Which user was created and added to the "sudo" group?
<img width="1464" height="116" alt="Screenshot 2026-02-22 at 8 59 37 AM" src="https://github.com/user-attachments/assets/5548c2a2-1fb9-4f55-b1de-3bcc6183ba08" />
---
## Quick Revision

auth.log = Authentication history

session opened → login  
session closed → logout  
sshd → remote login  
sudo → admin access  
useradd → new account  
COMMAND → executed action

--------------------------------------------------

# Linux Generic Logs & Bash History (SOC Analyst Notes)

--------------------------------------------------

## 1. Generic System Logs

Linux records many system activities
across different log files stored inside:

/var/log

These logs track system behavior,
services, software activity, and errors.

--------------------------------------------------

## 2. Important Generic Log Files

### kern.log

Location:
/var/log/kern.log

Purpose:
Stores kernel (core system) messages.

Contains:
- Hardware events
- Driver issues
- System errors
- Device activity

Used mainly in:
Advanced investigations and DFIR.

--------------------------------------------------

### syslog (Most Common)

Location:
/var/log/syslog
(or /var/log/messages)

Purpose:
General system activity log.

Contains:
- Service activity
- Background processes
- Cron jobs
- System events

SOC Tip:
If unsure where to start,
check syslog first.

--------------------------------------------------

### Package Manager Logs (Debian/Ubuntu)

Locations:
/var/log/dpkg.log  
/var/log/apt/

Records:
- Software installation
- Updates
- Package removal

Security Use:
Detect attacker-installed tools.

--------------------------------------------------

### Package Manager Logs (RHEL/CentOS)

Locations:
/var/log/yum.log  
/var/log/dnf.log  

Same purpose as dpkg logs,
but used in RHEL-based systems.

--------------------------------------------------

## 3. SOC Usage of Generic Logs

These logs are useful but often:

- Large
- Noisy
- Hard to analyze quickly

Daily SOC monitoring focuses more on:
authentication and application logs.

Generic logs are commonly used in DFIR.

--------------------------------------------------

## 4. Application-Specific Logs

Each application maintains its own logs.

Examples:

Web Server Logs  
Database Logs  
Mail Server Logs  
Container Logs  

These logs help analysts monitor
specific services.

--------------------------------------------------

## 5. Example: Nginx Web Access Logs

Location:
/var/log/nginx/access.log

Each log line represents
one web request.

Example meaning:

IP Address → Visitor  
Time → Request time  
Method → GET or POST  
Page → Requested resource  
Status Code → Server response

--------------------------------------------------

## 6. HTTP Status Codes

200 → Request successful  
302 → Redirected  
403 → Access denied  

SOC Alert Example:

Request to:
/admin with 403

Meaning:
Someone attempted restricted access.

Possible attacker probing.

--------------------------------------------------

## 7. Bash History

Bash history records commands
executed by users in terminal.

Stored per user at:

/home/username/.bash_history

Example:
/home/ubuntu/.bash_history

--------------------------------------------------

## 8. Viewing Command History

Commands executed earlier
can be viewed using:

history

or by opening:

.bash_history file

--------------------------------------------------

## 9. Why Bash History is Useful?

It helps identify:

- Commands executed
- File access
- Privilege escalation
- Suspicious activity
- Flags in CTF challenges

Example commands:

sudo su  
cat flag.txt  
nano ssh_config  

--------------------------------------------------

## 10. How Bash History Works

Commands are saved:

✔ During session (memory)  
✔ Written to file after logout  

--------------------------------------------------

## 11. Bash History Limitations

Bash history is NOT fully reliable.

Attackers can bypass logging.

--------------------------------------------------

### Method 1: Leading Space

Commands starting with space
may not be saved.

Example:
(space) malicious command

--------------------------------------------------

### Method 2: Script Execution

Attacker runs commands inside script.

History shows only script name,
not internal commands.

--------------------------------------------------

### Method 3: Different Shell

Using another shell like:

/bin/sh

Commands may not be recorded.

--------------------------------------------------

## 12. SOC Investigation Rule

Never rely only on bash history.

Always correlate with:

auth.log  
syslog  
application logs  

--------------------------------------------------

## 13. Linux Log Investigation Sources

auth.log → User authentication  
syslog → System activity  
kern.log → Kernel events  
package logs → Software installs  
app logs → Service activity  
bash history → User commands  

--------------------------------------------------
## Lab Questions

### Q.1 According to the VM's package manager logs, which version of unzip was installed on the system?
<img width="1019" height="161" alt="Screenshot 2026-02-22 at 9 13 13 AM" src="https://github.com/user-attachments/assets/820a5090-80de-4ce8-9ca5-9bedc362d74b" />
### Q.2 What is the flag you see in one of the users' bash history?

--
## Quick Revision

/var/log = main log directory  
syslog = general system events  
kern.log = kernel activity  
package logs = software installs  
app logs = service monitoring  
.bash_history = user commands  

SOC analysts combine multiple logs
to understand attacker activity.

--------------------------------------------------

# Linux Runtime Monitoring & System Calls 

--------------------------------------------------

## 1. Problem with Normal Linux Logs

Earlier logs like:

auth.log  
syslog  
bash history  

can show:

✔ Login activity  
✔ Commands executed  
✔ System events  

But they CANNOT reliably answer:

- Which program was launched?
- Who deleted a file?
- When was a folder removed?
- Which process accessed network?

Reason:
Linux does NOT log runtime activity by default.

--------------------------------------------------

## 2. What are Runtime Events?

Runtime events are activities happening
while the system is running.

Examples:

- Process creation
- Program execution
- File deletion
- File modification
- Network connections

These events happen continuously
but are not logged automatically.

--------------------------------------------------

## 3. Same Problem Exists in Windows

Windows also does not log runtime activity
by default.

Solution in Windows:
Sysmon tool.

Linux uses a similar approach
with runtime monitoring tools.

--------------------------------------------------

## 4. Logging Visibility Concept

Monitoring only auth.log
→ Very limited visibility

Adding web logs + bash history
→ Better visibility

Ignoring runtime events
→ Important attacks remain hidden

SOC Rule:
More visibility = better threat detection.
<img width="900" height="318" alt="Screenshot 2026-02-22 at 9 37 34 AM" src="https://github.com/user-attachments/assets/42070e2b-5bf5-4d74-bc42-4d1b59daa3ed" />


--------------------------------------------------

## 5. What are System Calls?

System Calls are requests made
by programs to the operating system.

Whenever a program needs OS help,
it performs a system call.

Examples:

Open file  
Create process  
Access hardware  
Use network  
Execute program  
<img width="1158" height="221" alt="Screenshot 2026-02-22 at 9 37 19 AM" src="https://github.com/user-attachments/assets/3b24f648-3eba-4e53-bd1a-bd9fb793505d" />

--------------------------------------------------

## 6. Example of System Call Flow

Step 1:
User starts a program.

Step 2:
Program sends request to Kernel.

Step 3:
Kernel accesses hardware/resources.

Step 4:
Kernel returns result to program.

This communication happens
through system calls.

--------------------------------------------------

## 7. Important System Call Example

execve

Purpose:
Executes a program or command.

Example:
Running any application or script
uses execve internally.

--------------------------------------------------

## 8. Why System Calls Matter in SOC?

Every important action requires
a system call.

Attackers cannot easily avoid them.

So monitoring system calls allows:

✔ Process monitoring  
✔ File activity tracking  
✔ Execution detection  
✔ Malware behavior detection  

--------------------------------------------------

## 9. How Security Tools Use System Calls

Modern tools monitor system calls
and convert them into readable logs.

Examples:

EDR tools  
Runtime monitoring tools  
Audit frameworks  

These tools observe system behavior
in real time.

--------------------------------------------------

## 10. Runtime Monitoring Advantage

Normal Logs:
Show past activity.

Runtime Monitoring:
Shows LIVE system behavior.

This helps detect:

- Malware execution
- Unauthorized programs
- File deletion attacks
- Suspicious processes

--------------------------------------------------

## 11. Linux Runtime Monitoring Tool

Common tool:
auditd

auditd monitors selected
system calls and records events.

Used for:
Security monitoring
Forensics investigation
Compliance logging

--------------------------------------------------

## 12. SOC Analyst Key Idea

Logs show history.

System Calls show behavior.

Monitoring system calls
provides deep system visibility.

--------------------------------------------------

## Quick Revision

Runtime events = live system actions  
Default Linux logs miss runtime activity  
System calls = program requests to kernel  
execve = program execution call  
EDR tools monitor system calls  
auditd enables runtime monitoring  

--------------------------------------------------
# Linux Auditd (Audit Daemon) - SOC Runtime Monitoring Notes

--------------------------------------------------

## 1. Why Do We Need Auditd?

Normal Linux logs like:

auth.log  
syslog  
bash history  

can show:

✔ User logins  
✔ Commands used  
✔ System activity  

But they CANNOT show reliably:

- Which program was executed
- Who deleted files
- Which process started malware
- Network activity by programs

These are called Runtime Events.

Linux does NOT log runtime events by default.

Solution:
Auditd (Audit Daemon)

--------------------------------------------------

## 2. What is Auditd?

Auditd is a built-in Linux security tool
used for runtime monitoring.

It records system activity in real time.

Think:

auth.log → Entry register  
auditd → CCTV camera

Auditd monitors system behavior LIVE.
<img width="1360" height="331" alt="Screenshot 2026-02-22 at 9 36 49 AM" src="https://github.com/user-attachments/assets/6a19238f-bbba-42fe-a483-1964bb5e0077" />

--------------------------------------------------

## 3. What Does Auditd Monitor?

Auditd tracks:

- Process execution
- File access and modification
- Network activity
- Privilege usage
- System changes

It works using system calls.

--------------------------------------------------

## 4. System Calls (Core Concept)

A System Call is a request made
by a program to the Linux kernel.

Examples:

Run program → execve  
Open file → open / openat  
Network access → connect  

Every action in Linux requires
a system call.

Attackers cannot bypass system calls easily.

--------------------------------------------------

## 5. How Auditd Works

Step 1:
SOC engineers define monitoring rules.

Step 2:
Auditd watches selected system calls.

Step 3:
Events are recorded when activity occurs.

Step 4:
Logs are stored for investigation.

--------------------------------------------------

## 6. Auditd Rules

Rules define WHAT should be monitored.

Rule location:

/etc/audit/rules.d/

Rules may monitor:

- Specific programs
- Important files
- Network actions
- System behavior

Example idea:
Log whenever wget runs.

--------------------------------------------------

## 7. Auditd Log Location

Audit logs are stored at:

/var/log/audit/audit.log

These logs are root protected.

Root privileges are required to read them.

--------------------------------------------------

## 8. Reading Audit Logs

Raw audit logs are difficult to read.

Tool used:
ausearch

ausearch formats logs
and allows filtering.

Example usage idea:

Search events using rule key.

--------------------------------------------------

## 9. Important Audit Event Sections

Auditd splits one activity
into multiple records.

--------------------------------------------------

### PROCTITLE

Shows full command executed.

Example:
wget file.zip

--------------------------------------------------

### CWD

Current working directory
where command was executed.

--------------------------------------------------

### EXECVE

Shows executed program arguments.

Helps understand exact command usage.

--------------------------------------------------

### SYSCALL

Contains core runtime information.

Most important investigation data.

--------------------------------------------------

## 10. Important Investigation Fields

pid
→ Process ID

ppid
→ Parent Process ID
(used to build process tree)

auid
→ Original logged-in user

uid
→ Current executing user

Difference example:
auid = ubuntu
uid = root

Meaning:
User escalated privileges.

tty
→ Session identifier

exe
→ Absolute path of executed program

key
→ Custom rule tag for filtering events

--------------------------------------------------

## 11. File Monitoring Example

Auditd can monitor critical files.

Example:
/etc/ssh/sshd_config

If edited:

nano /etc/ssh/sshd_config

Auditd records the event.

SOC teams monitor:

- SSH configs
- Cron jobs
- System settings
- Security files

--------------------------------------------------

## 12. Process Execution Monitoring

Auditd detects program execution.

Example:

wget malware.zip
naabu scan execution

Useful for detecting:

- Malware download
- Recon tools
- Unauthorized binaries

--------------------------------------------------

## 13. Investigation Workflow (SOC Method)

Step 1:
Identify suspicious activity.

Step 2:
Search audit logs.

Step 3:
Find executed process.

Step 4:
Check command arguments.

Step 5:
Trace parent process.

Step 6:
Build attack timeline.

--------------------------------------------------

## 14. Auditd Limitations

Auditd logs are:

✔ Very detailed  
✘ Hard to read  
✘ Large in size  

Because of this,
SOC teams sometimes use alternatives.

--------------------------------------------------

## 15. Auditd Alternatives

Sysmon for Linux  
Falco  
Osquery  
EDR solutions  

All tools work by monitoring
system calls.

--------------------------------------------------

## 16. Key SOC Concept

Logs show history.

Auditd shows behavior.

Runtime monitoring provides
deep visibility into attacks.

--------------------------------------------------
## Lab Questions
### Q.1 When was the secret.thm file opened for the first time? (MM/DD/YY HH:MM:SS) Note: Access to this file is logged with the "file_thmsecret" key.
- using command : ausearch -i -k file_thmsecret
### Q.2 What is the original file name downloaded from GitHub via wget? Note: Wget process creation is logged with the "proc_wget" key.
- using command : ausearch -i -k proc_wget
### Q.3 Which network range was scanned using the downloaded tool? Note: There is no dedicated key for this event, but it's still in auditd logs.

Meaning:

1. A tool was downloaded.
2. Tool was executed.
3. Tool performed network scanning.
4. Scan target (IP range) exists in audit logs.

--------------------------------------------------

## Investigation Logic

Download → Execution → Scan Target

Never search randomly.
Follow activity timeline.

--------------------------------------------------

## Step 1: Find Downloaded Tool

Search download activity.

Look for tools downloaded using:
wget or curl

Example idea:
wget tool.zip

Identify tool name.

Example:
naabu

--------------------------------------------------

## Step 2: Pivot Investigation

Downloading does NOT perform scanning.

Now search execution of downloaded tool.

Search audit logs using tool name.

Example idea:
search "naabu" inside audit logs.

--------------------------------------------------

## Step 3: Check Execution Logs

Focus on:

PROCTITLE  
EXECVE  

These fields show full executed command.

--------------------------------------------------

## Step 4: Identify Network Range

Look for CIDR format:

x.x.x.x/x

Examples:

10.10.0.0/24  
192.168.1.0/24  
172.16.0.0/16  

This network range is the answer.

--------------------------------------------------

## Step 5: Useful Hunting Trick

Network scans usually contain:

/24  
/16  
/8  

Searching "/" helps locate scan commands faster.

--------------------------------------------------

## Common Mistake

Wrong:
Searching words like scan or network.

Correct:
Search executed command arguments.

Auditd logs actions, not meanings.

--------------------------------------------------

## SOC Analyst Rule

Always pivot investigation:

Find download  
→ Find execution  
→ Extract command arguments  
→ Identify target

--------------------------------------------------

<img width="1470" height="956" alt="Screenshot 2026-02-22 at 9 35 07 AM" src="https://github.com/user-attachments/assets/b2031446-1294-469f-9286-b93dd17281fa" />
<img width="1470" height="956" alt="Screenshot 2026-02-22 at 9 35 18 AM" src="https://github.com/user-attachments/assets/f9a6e17f-1e4e-43b3-a0af-048beca4177e" />
<img width="1470" height="326" alt="Screenshot 2026-02-22 at 9 35 31 AM" src="https://github.com/user-attachments/assets/4409a416-c1e4-4812-9fbc-7b19364f4e8d" />


## Quick Revision

Auditd = Runtime monitoring tool  
Monitors system calls  
Rules stored in /etc/audit/rules.d  
Logs stored in /var/log/audit/audit.log  
ausearch used for investigation  
Tracks processes, files, and network actions  

--------------------------------------------------
