# Linux Hardening and Firewall Notes (SOC Perspective)

## What is Linux Hardening?

Linux Hardening means making a Linux system more secure and harder for attackers to compromise.

Security should use multiple layers. This is called **Defense in Depth**.

Example:

Physical Security
→ Disk Encryption
→ Firewall
→ Monitoring

If one layer fails, another layer can still protect the system.

---

# 1. Physical Security

Physical security is the first security layer.

If an attacker gets physical access to a computer:

* They can steal the hard disk.
* They can boot the system and reset the root password.
* They can access sensitive data.

Important idea:

Boot Access = Root Access

This means a person with physical access may gain administrator access.

### SOC Analyst Note

If a device is stolen, treat it as a security incident because the attacker may access company data.

---

# 2. GRUB Password

GRUB is the Linux bootloader.

A GRUB password prevents unauthorized users from:

* Changing boot settings
* Entering recovery mode
* Resetting the root password

Without a GRUB password:

Attacker → Reboot System → Reset Root Password

With a GRUB password:

Attacker → Reboot System → Password Required

### SOC Analyst Note

GRUB password adds another security layer but does not protect data if the disk is stolen.

---

# 3. LUKS Disk Encryption

LUKS stands for Linux Unified Key Setup.

It encrypts the entire disk.

Without encryption:

Attacker steals disk
→ Reads files directly

With LUKS:

Attacker steals disk
→ Data appears as unreadable encrypted data

### Main Goal

Protect data when a device or disk is stolen.

---

# 4. How LUKS Works

User enters a password.

The password is converted into a strong encryption key using:

PBKDF2 or Argon2

The encryption key is used to protect the data.

Important terms:

* Salt = Random value added to password
* Iterations = Repeated calculations to slow down brute-force attacks

### SOC Analyst Note

When investigating a system, encryption information can be viewed using:

cryptsetup luksDump /dev/sdb1

---

# 5. Linux Firewall

A firewall controls network traffic.

It decides:

* What can enter the system
* What can leave the system

Without a firewall:

Any service may be reachable from the network.

With a firewall:

Only approved traffic is allowed.

### SOC Analyst Note

Firewalls reduce the attack surface of a system.

---

# 6. Stateful Firewall

Modern Linux firewalls are stateful.

They remember active connections.

Example:

User opens a website
→ Firewall remembers the connection
→ Website replies are allowed

Random packets from attackers are blocked.

### Benefit

Better protection against spoofed or suspicious traffic.

---

# 7. Important Firewall Information

Firewalls mainly filter traffic using:

* Source IP Address
* Destination IP Address
* Source Port
* Destination Port

Common ports:

22 = SSH
80 = HTTP
443 = HTTPS
53 = DNS

### SOC Analyst Note

When reviewing firewall logs, always check:

* Source IP
* Destination IP
* Port number
* Allowed or blocked action

---

# 8. Netfilter

Netfilter is the core firewall framework inside Linux.

Most firewall tools use Netfilter in the background.

Examples:

* iptables
* nftables
* UFW

---

# 9. iptables

iptables is a firewall management tool.

Important chains:

INPUT
= Incoming traffic

OUTPUT
= Outgoing traffic

FORWARD
= Traffic passing through the system

### Example Rule

Allow SSH traffic on port 22.

iptables -A INPUT -p tcp --dport 22 -j ACCEPT

### SOC Analyst Note

Many legacy Linux systems still use iptables.

---

# 10. nftables

nftables is the modern replacement for iptables.

Advantages:

* Better performance
* Easier management
* More scalable

### SOC Analyst Note

Many newer Linux distributions prefer nftables.

---

# 11. UFW

UFW = Uncomplicated Firewall

It provides a simpler way to manage firewall rules.

Example:

ufw allow 22/tcp

This allows SSH traffic.

Check firewall status:

ufw status

### SOC Analyst Note

UFW is common on Ubuntu systems.

---

# 12. Firewall Policies

Two common approaches:

### Allow Everything, Block Some

Default = Allow

Advantages:

* Easy to use

Disadvantages:

* Less secure

---

### Block Everything, Allow Some

Default = Deny

Advantages:

* More secure
* Smaller attack surface

Disadvantages:

* Requires more configuration

### SOC Best Practice

Default Deny Policy

Only allow required services such as:

* DNS (53)
* HTTP (80)
* HTTPS (443)
* SSH (22 if needed)

---

# SOC Exam Quick Revision

Physical Security
→ Protect hardware from attackers

GRUB Password
→ Prevent boot-time attacks

LUKS
→ Protect data if disk is stolen

Firewall
→ Control network traffic

Stateful Firewall
→ Tracks active connections

Netfilter
→ Linux firewall engine

iptables
→ Traditional firewall tool

nftables
→ Modern firewall tool

UFW
→ Easy firewall management

Best Firewall Policy
→ Block Everything, Allow Only Required Traffic


---

# Linux Hardening - Remote Access Security Notes (SOC Perspective)

## Why Remote Access is Risky?

Remote access is useful because admins can manage systems from anywhere.

But attackers also see these services and try to attack them.

Common attacks:

* Password Sniffing
* Password Guessing
* Brute Force Attacks
* Service Exploitation

---

# 1. Protect Against Password Sniffing

## Problem

Some old protocols send usernames and passwords in plain text.

Example:

Telnet

An attacker capturing network traffic can read the password directly.

---

## Solution

Use encrypted protocols.

Preferred protocol:

SSH (Secure Shell)

SSH encrypts communication between client and server.

Even if attackers capture packets, they cannot read the password.

---

## SOC Analyst Note

Watch for:

* Telnet traffic
* Legacy remote access protocols
* Unencrypted authentication attempts

---

# 2. Protect Against Password Guessing

## Problem

Attackers constantly scan the internet for SSH servers.

Common targets:

* root
* admin
* test
* common usernames

They try thousands of passwords automatically.

This is called:

* Password Guessing
* Brute Force Attack

---

## Solution 1: Disable Root Login

Never allow direct root login through SSH.

Configuration:

PermitRootLogin no

---

## Why?

Without this setting:

Attacker → Guess Root Password → Full System Access

With this setting:

Attacker → Cannot Login Directly as Root

---

## SOC Analyst Note

Look for:

* Multiple failed SSH logins
* Repeated root login attempts
* Login attempts from unknown IP addresses

These may indicate brute-force activity.

---

# 3. Use SSH Key Authentication

## Problem

Passwords can be:

* Guessed
* Reused
* Stolen

---

## Better Solution

Use Public Key Authentication.

SSH uses:

Private Key
+
Public Key

Only the correct private key can authenticate.

---

## Generate SSH Keys

ssh-keygen -t rsa

---

## Copy Public Key to Server

ssh-copy-id username@server

---

## SSH Configuration

Enable key authentication:

PubkeyAuthentication yes

Disable password authentication:

PasswordAuthentication no

---

## Benefit

Even if attackers try millions of passwords, authentication will fail because passwords are not accepted.

---

## SOC Analyst Note

Best practice:

* Use SSH keys
* Disable password authentication whenever possible

---

# 4. Use sudo Instead of Root

## Why?

Root has complete control over the system.

A small mistake can:

* Delete files
* Break services
* Damage the operating system

---

## Better Approach

Use a normal account.

Use sudo only when administrator privileges are required.

---

## Meaning of sudo

Super User Do

---

## Add User to sudo Group

Debian / Ubuntu:

usermod -aG sudo username

RedHat / Fedora:

usermod -aG wheel username

---

## SOC Analyst Note

Follow the Principle of Least Privilege.

Users should only receive the permissions they actually need.

---

# 5. Disable the Root Account

After creating an administrative account with sudo privileges, direct root access can be disabled.

---

## Method

Change root shell from:

/bin/bash

to:

/sbin/nologin

---

## Benefit

Prevents interactive login using the root account.

---

## SOC Analyst Note

Disabling root reduces the risk of direct account compromise.

---

# 6. Enforce Strong Password Policies

## Why?

Weak passwords are easy to guess.

Examples of weak passwords:

* password123
* qwerty1234
* admin123

---

## Password Policy Settings

Common options:

difok
minlen
minclass
retry

---

## Example Configuration

difok=5

minlen=10

minclass=3

retry=2

---

## Meaning

difok=5

New password must differ from old password.

---

minlen=10

Minimum password length is 10 characters.

---

minclass=3

Must contain at least 3 character types.

Examples:

* Uppercase letters
* Lowercase letters
* Numbers
* Special characters

---

retry=2

User gets 2 attempts before failure.

---

## SOC Analyst Note

Strong password policies reduce the success rate of brute-force attacks.

---

# 7. Disable Unused Accounts

## Why?

Old accounts often become security risks.

Examples:

* Former employees
* Temporary accounts
* Forgotten accounts

---

## Method

Change shell to:

/sbin/nologin

---

## Example

Enabled Account:

michael:x:1000:1000:Michael:/home/michael:/usr/bin/fish

Disabled Account:

michael:x:1000:1000:Michael:/home/michael:/sbin/nologin

---

## SOC Analyst Note

Regularly review:

* User accounts
* Privileged accounts
* Inactive accounts

Disable accounts that are no longer required.

---

# 8. Disable Interactive Login for Service Accounts

Examples:

* www-data
* nginx
* mongo

These accounts are meant to run services, not for human login.

---

## Why?

If a service is compromised, attackers may try to log in using the service account.

Setting:

/sbin/nologin

reduces this risk.

---

## SOC Analyst Note

Service accounts should not have interactive shells unless absolutely necessary.

---

# 9. Disable Unnecessary Services

## Important Rule

More software = More vulnerabilities

Every installed package increases attack surface.

---

## Example

If no web server is required:

* Remove Apache
* Remove Nginx

If no FTP is required:

* Disable FTP services

---

## SOC Analyst Note

Only run services that are required for business operations.

---

# 10. Block Unnecessary Network Ports

After disabling services, firewall rules should also be updated.

---

## Example

If no web server exists:

Block:

80/TCP

443/TCP

---

## Why?

Even if a service accidentally starts later, the firewall can still block access.

---

## SOC Analyst Note

Open only required ports.

Everything else should remain blocked.

---

# 11. Avoid Legacy Protocols

## Avoid

* Telnet
* TFTP

These protocols are outdated and less secure.

---

## Use Instead

SSH instead of Telnet

SFTP instead of TFTP

---

## SOC Analyst Note

Legacy protocols should be removed whenever secure alternatives exist.

---

# 12. Remove Identification Strings

Many services reveal:

* Software name
* Version number
* Operating system information

---

## Risk

Attackers can use this information to find matching vulnerabilities.

---

## Example

Instead of seeing:

Apache/2.4.49

Attackers should see as little information as possible.

---

## SOC Analyst Note

Reduce information disclosure wherever possible.

---

# Quick Revision

SSH
→ Secure encrypted remote access

PermitRootLogin no
→ Disable direct root login

SSH Keys
→ Better than passwords

sudo
→ Administrative access without using root

Strong Password Policy
→ Prevent password guessing

Disable Unused Accounts
→ Reduce attack surface

Disable Service Logins
→ Protect service accounts

Remove Unnecessary Services
→ Fewer vulnerabilities

Block Unused Ports
→ Smaller attack surface

Avoid Telnet and TFTP
→ Use secure protocols

Remove Version Information
→ Reduce attacker reconnaissance

---

# One-Line Exam Revision

Secure Remote Access = SSH + No Root Login + SSH Keys + Strong Passwords + sudo + Disabled Unused Accounts + Minimal Services + Restricted Firewall Rules

---

# Linux Hardening - Updates and Logging Notes (SOC Perspective)

## Why Updates Matter

Security updates fix:

* Vulnerabilities
* Bugs
* Security weaknesses

If systems are not updated, attackers can exploit known vulnerabilities.

---

# System Updates

## Debian-Based Systems

Update package information:

apt update

Install available updates:

apt upgrade

---

## Red Hat / Fedora Systems

Newer versions:

dnf update

Older versions:

yum update

---

## SOC Analyst Note

Regular updates reduce the risk of system compromise.

Always patch:

* Operating System
* Applications
* Services
* Kernel

---

# Ubuntu LTS

LTS = Long Term Support

Examples:

* Ubuntu 18.04
* Ubuntu 20.04
* Ubuntu 22.04

---

## Benefits

* Stable releases
* Long security support
* Suitable for production environments

---

## Support Period

Ubuntu LTS provides:

* 5 years free security updates
* Additional support available through ESM

---

## SOC Analyst Note

Avoid running End-of-Life (EOL) operating systems because they no longer receive security patches.

---

# Red Hat Support Lifecycle

Support phases:

1. Full Support
2. Maintenance Support
3. Extended Life Phase

---

## Benefit

Long-term support makes Red Hat suitable for enterprise environments.

---

# Kernel Updates

The Linux kernel is the core component of the operating system.

Kernel vulnerabilities can lead to complete system compromise.

---

## Dirty COW

Dirty COW was a famous Linux kernel vulnerability.

Impact:

Normal User
→ Root Access

This is called:

Privilege Escalation

---

## SOC Analyst Note

Always keep the kernel updated.

Kernel vulnerabilities are often high severity because they can provide root access.

---

# Automatic Updates

Automatic updates help ensure security patches are installed quickly.

---

## Advantages

* Faster patch deployment
* Less manual work
* Reduced exposure time

---

## SOC Analyst Note

Automatic updates are useful for maintaining security, especially on stable production systems.

---

# Security Monitoring

SOC teams should monitor:

* Security advisories
* Vendor notifications
* Newly disclosed vulnerabilities
* Critical CVEs

---

## Goal

Identify whether company systems are affected by newly discovered vulnerabilities.

---

# Linux Logs

Logs record activities and events occurring on a system.

They help analysts:

* Detect attacks
* Investigate incidents
* Identify suspicious activity
* Troubleshoot problems

---

# Important Log Files

## /var/log/messages

General system log.

Contains:

* System events
* Service messages
* Warnings

---

## /var/log/auth.log

Used on Debian-based systems.

Contains:

* Login attempts
* SSH activity
* Authentication events
* sudo activity

---

## SOC Use Cases

Detect:

* Failed logins
* Brute-force attacks
* Successful SSH logins
* Privilege escalation activity

---

## /var/log/secure

Used on Red Hat and Fedora systems.

Purpose is similar to auth.log.

Contains:

* Authentication events
* Login attempts
* SSH activity

---

## /var/log/utmp

Shows users currently logged into the system.

Useful for checking active sessions.

---

## /var/log/wtmp

Stores login and logout history.

Useful for:

* User activity tracking
* Historical investigations

---

## /var/log/kern.log

Contains kernel messages.

Useful for:

* Driver issues
* Kernel events
* System-level problems

---

## /var/log/boot.log

Contains startup and boot information.

Useful for:

* Startup troubleshooting
* Boot failures
* Service startup issues

---

# Useful Log Analysis Commands

## tail

Displays the last lines of a log file.

Example:

tail -n 12 boot.log

Purpose:

View recent events quickly.

---

## grep

Searches for specific keywords.

Example:

grep FAILED boot.log

Purpose:

Find specific events inside large log files.

---

## Common Search Keywords

FAILED

ERROR

DENIED

ROOT

PASSWORD

SSH

SUDO

---

# SOC Investigation Example

Alert:

Multiple SSH Login Failures

---

Check:

/var/log/auth.log

or

/var/log/secure

---

Possible Finding:

Failed password for root

Failed password for root

Failed password for root

---

Conclusion:

Possible SSH Brute Force Attack

---

# What SOC Analysts Monitor

Authentication Logs

* Failed logins
* Successful logins
* Root login attempts

---

System Logs

* Service failures
* Errors
* Warnings

---

Kernel Logs

* Kernel crashes
* Security events
* Driver issues

---

Boot Logs

* Reboots
* Startup problems
* Unexpected restarts

---

# Quick Revision

apt update
→ Download package information

apt upgrade
→ Install available updates

dnf update
→ Update newer Red Hat systems

yum update
→ Update older Red Hat systems

Ubuntu LTS
→ Long-term supported release

Dirty COW
→ Privilege Escalation Vulnerability

Automatic Updates
→ Faster patch deployment

auth.log
→ Authentication events

secure
→ Authentication log on Red Hat

utmp
→ Current logged-in users

wtmp
→ Login and logout history

kern.log
→ Kernel messages

boot.log
→ Boot information

tail
→ View latest log entries

grep
→ Search log contents

---

# One-Line Exam Revision

Patch systems regularly, keep the kernel updated, monitor security advisories, and analyze Linux logs to detect authentication attacks, privilege escalation attempts, and suspicious activity.

