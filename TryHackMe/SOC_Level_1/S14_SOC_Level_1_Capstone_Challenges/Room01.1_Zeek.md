# Zeek (Bro) – Simple Notes (SOC Point of View)

## 1. What is Zeek?

Zeek is a **network traffic analyzer**.

Very simple idea:

Imagine a **CCTV camera for network traffic**.

When data moves in a network (computers talking to each other),  
Zeek **watches that traffic and writes detailed logs**.

Security analysts in a **SOC (Security Operations Center)** read those logs to find suspicious activity.

Example:

If a computer suddenly connects to a strange IP or downloads a suspicious file,  
Zeek records that activity in logs.

So later analysts can investigate it.

---

## 2. Why SOC teams use Zeek

SOC analysts use Zeek mainly for **investigation and threat hunting**.

Zeek helps to:

• see who connected to whom  
• see which files were downloaded  
• check DNS requests  
• detect suspicious traffic patterns  
• investigate security incidents  

Important point:

Zeek usually **does not block attacks**.

It mainly **observes and records everything**.

---

## 3. Network Monitoring vs Network Security Monitoring

These two sound similar but they are different.

| Feature | Network Monitoring | Network Security Monitoring |
|-------|-------|-------|
| Main Goal | Check network health | Detect attacks |
| Focus | uptime, performance | suspicious activity |
| Used by | IT / Network team | SOC / Security team |
| Example | network slow | malware communication |

Example:

Network Monitoring  
A server is down → IT team fixes it.

Network Security Monitoring  
A computer talks to a hacker server → SOC investigates it.

Zeek is mainly used for **Network Security Monitoring**.

---

## 4. Zeek vs Snort

Both tools are used in network security but they work differently.

| Feature | Zeek | Snort |
|-------|-------|-------|
| Focus | traffic analysis | attack detection |
| Detection method | events | signatures |
| Output | detailed logs | alerts |
| Best use | investigation | stopping known attacks |

Simple example:

Snort = security alarm  
Zeek = security camera recording everything

SOC teams often use **both tools together**.

---

## 5. Zeek Architecture (How Zeek Works)

Zeek has two main parts.

### 1. Event Engine

This part **reads network packets**.

It extracts basic information like:

• source IP  
• destination IP  
• protocol  
• session information  
• file transfers  

Example:

A packet says:

Computer A → Google DNS

The Event Engine understands this connection.

---

### 2. Policy Script Interpreter

This part **analyzes events using scripts**.

It decides if something is suspicious.

Example:

If a computer sends **thousands of DNS queries**,  
Zeek may flag it as suspicious.

So:

Event Engine → collects information  
Policy Script → analyzes the behavior

---

## 6. Zeek Frameworks

Frameworks are **extra systems inside Zeek** that add more features.

Important frameworks SOC analysts use:

| Framework | Purpose |
|------|------|
Logging | creates logs |
Notice | generates alerts |
Intelligence | uses threat intelligence |
File Analysis | analyzes transferred files |
GeoLocation | finds IP location |

Example:

If a file is downloaded from the network,  
the **File Analysis framework** can record information about it.

---

## 7. Zeek Logs

One of the biggest strengths of Zeek is **detailed logs**.

Zeek can generate **50+ different log types**.

Important logs SOC analysts use:

| Log File | What it shows |
|------|------|
conn.log | network connections |
dns.log | DNS requests |
http.log | web traffic |
ssl.log | encrypted traffic |
files.log | transferred files |

Example:

conn.log might show:

Computer A connected to IP 8.8.8.8 on port 53.

This helps analysts understand network behavior.

---

## 8. Where Zeek Saves Logs

When Zeek runs normally, logs are stored in:

/opt/zeek/logs/

If Zeek analyzes a **pcap file**, logs appear in the **current working folder**.

SOC analysts read these logs to investigate incidents.

---

## 9. Two Ways to Use Zeek

Zeek can work in two main modes.

### 1. Live Network Monitoring

Zeek watches **real-time network traffic**.

Commands used:

zeekctl start  
zeekctl status  
zeekctl stop  

SOC teams use this to monitor networks continuously.

---

### 2. PCAP Analysis

Zeek can analyze a **packet capture file (pcap)**.

Command:

zeek -C -r sample.pcap

After running this command, Zeek automatically generates logs like:

conn.log  
dns.log  
http.log  

This is very common in **incident investigation**.

---

## 10. Tools Used to Investigate Zeek Logs

SOC analysts use command line tools to analyze logs.

Common tools:

cat  
grep  
cut  
sort  
uniq  
zeek-cut  

Example idea:

Search for a suspicious IP in logs.

These tools help analysts quickly find important information.

---

# Quick Revision

Zeek is a **network traffic analysis tool** used in SOC environments.

It passively monitors network traffic and generates detailed logs.

SOC analysts use these logs for:

• threat hunting  
• incident investigation  
• traffic analysis  

Zeek works using two main layers:

Event Engine → processes packets  
Policy Script Interpreter → analyzes events

Zeek generates many logs such as:

conn.log  
dns.log  
http.log  
ssl.log  

These logs help analysts understand network activity and detect suspicious behavior.
---
# Lab Questions


## Q.1 What is the installed Zeek instance version number?
## Q.2 What is the version of the ZeekControl module?
<img width="1470" height="956" alt="Screenshot 2026-03-11 at 12 46 21 PM" src="https://github.com/user-attachments/assets/66dacb1e-ae33-47e6-b9c2-b874551d1281" />
## Q.3 Investigate the "sample.pcap" file. What is the number of generated alert files?
<img width="1470" height="956" alt="Screenshot 2026-03-11 at 12 50 32 PM" src="https://github.com/user-attachments/assets/528640bf-0687-4d99-8002-d22d9ee51aa4" />
---
# Zeek Logs – Simple Notes (SOC Point of View)

## 1. What are Zeek Logs?

When **Zeek monitors network traffic**, it automatically creates **log files**.

A log file is simply a **record of what happened in the network**.

Example idea:

Network activity → Zeek observes it → Zeek writes it in logs.

SOC analysts later read these logs to **investigate suspicious activity**.

Example situations logs can show:

• which computer connected to another computer  
• which website was accessed  
• which file was downloaded  
• if suspicious behavior occurred  

---

## 2. Structure of Zeek Logs

Zeek logs are **tab-separated text files**.

This means the information is stored in **columns**.

Example structure:

time | uid | source IP | source port | destination IP | destination port | protocol

Example entry:

1488571051  CTMFXm1AcIsSnq2Ric  192.168.121.2  51153  192.168.120.22  53  udp

Each column contains **different network information**.

---

## 3. UID (Very Important)

Each network session in Zeek is given a **UID (Unique Identifier)**.

This UID helps analysts **connect information across multiple logs**.

Example:

conn.log → connection information  
dns.log → DNS request information  

If both logs have the **same UID**, they belong to the **same network session**.

This process is called **log correlation**.

It helps SOC analysts understand the full story of an event.

---

## 4. Zeek Log Categories

Zeek generates **50+ logs**, grouped into **7 categories**.

These categories help analysts find the right log quickly.

| Category | Purpose |
|--------|--------|
| Network | Logs for network protocols |
| Files | Information about transferred files |
| NetControl | Network control actions |
| Detection | Security alerts and indicators |
| Network Observations | Network summary information |
| Miscellaneous | Unusual or rare events |
| Zeek Diagnostic | Zeek system and performance logs |

---

## 5. Important Network Protocol Logs

These logs show activity related to specific network protocols.

| Log File | Purpose |
|--------|--------|
conn.log | records network connections |
dns.log | DNS queries |
http.log | web browsing traffic |
ftp.log | FTP file transfers |
ssh.log | SSH login activity |
ssl.log | encrypted connections |
smtp.log | email traffic |

Example:

dns.log may show:

A computer requested the domain **example.com**.

---

## 6. File Analysis Logs

These logs track **files transferred in the network**.

| Log File | Purpose |
|--------|--------|
files.log | information about transferred files |
pe.log | details about executable files |
x509.log | SSL certificate information |

Example:

If a user downloads a file, **files.log** may record it.

This helps analysts detect **malware downloads**.

---

## 7. Detection Logs (Very Important for SOC)

These logs indicate **suspicious or malicious activity**.

| Log File | Purpose |
|--------|--------|
notice.log | unusual behavior detected |
intel.log | traffic matching threat intelligence |
signatures.log | triggered detection signatures |
traceroute.log | traceroute activity |

Example:

intel.log may show:

A connection was made to a **known malicious IP**.

---

## 8. Observation Logs

These logs summarize **network environment information**.

| Log File | Purpose |
|--------|--------|
known_hosts.log | list of active hosts |
known_services.log | services used in the network |
software.log | software detected in the network |

Example:

software.log might show:

A device running **Windows 10 and Chrome browser**.

---

## 9. Miscellaneous Logs

These logs capture **unexpected or strange events**.

| Log File | Purpose |
|--------|--------|
weird.log | unusual network behavior |
unknown_protocols.log | unidentified protocols |

Example:

If traffic does not match a known protocol, it may appear in **unknown_protocols.log**.

---

## 10. Diagnostic Logs

These logs show **internal Zeek information**.

| Log File | Purpose |
|--------|--------|
stats.log | performance statistics |
reporter.log | system messages |
capture_loss.log | packet capture loss information |

These logs help in **troubleshooting Zeek itself**.

---

## 11. Log Update Frequency

Some logs update **daily**, others update **per session**.

### Daily Logs

These logs store general network information.

| Log | Purpose |
|----|----|
known_hosts.log | list of network hosts |
known_services.log | services used |
known_certs.log | SSL certificates |
software.log | detected software |

---

### Session Logs

These logs update when events occur.

| Log | Purpose |
|----|----|
notice.log | suspicious events |
intel.log | malicious indicators |
signatures.log | triggered signatures |

These logs are **important for security investigations**.

---

## 12. Investigation Workflow Using Logs

SOC analysts usually follow this order during investigations.

### Step 1 – Overall Information

Check general activity first.

Important logs:

conn.log  
files.log  
intel.log  
loaded_scripts.log  

Goal:

Understand overall network activity.

---

### Step 2 – Protocol Investigation

Focus on specific protocols if something looks suspicious.

Important logs:

dns.log  
http.log  
ftp.log  
ssh.log  

Example:

If a suspicious domain appears → check **dns.log**.

---

### Step 3 – Detection Evidence

Check detection logs for alerts.

Important logs:

notice.log  
signatures.log  
intel.log  

These logs confirm possible **attacks or threats**.

---

### Step 4 – Observation

Review network summary logs.

Important logs:

known_hosts.log  
known_services.log  
software.log  
weird.log  

Goal:

Find hidden anomalies or confirm conclusions.

---

## 13. Tools Used to Read Zeek Logs

Because logs contain large amounts of data, analysts use tools to filter them.

Common tools:

cat  
grep  
cut  
sort  
uniq  

These tools help find **specific information quickly**.

---

## 14. Zeek-Cut Tool

Zeek logs contain many columns.  
Sometimes analysts only need **specific fields**.

The **zeek-cut** tool extracts selected columns.

Example idea:

Instead of reading the entire log, analysts extract only:

UID  
protocol  
source IP  
source port  
destination IP  
destination port  

This makes investigation **faster and easier**.

---

# Quick Revision

Zeek generates detailed **network activity logs**.

Key points:

• Zeek logs are **tab-separated text files**  
• Each session has a **UID (Unique Identifier)**  
• Logs are grouped into **7 categories**  
• Analysts correlate logs using **UID**  
• Important logs include:

conn.log  
dns.log  
http.log  
files.log  
notice.log  
intel.log  

SOC analysts read and correlate these logs to **detect attacks and investigate incidents**.
---
# Lab Questions
## Q.1 Investigate the sample.pcap file. Investigate the dhcp.log file. What is the available hostname?
<img width="1470" height="956" alt="Screenshot 2026-03-11 at 1 15 31 PM" src="https://github.com/user-attachments/assets/85aeccc0-8c06-4c3c-809e-fefec2186b44" />
## Q.2 Investigate the dns.log file. What is the number of unique DNS queries?
<img width="1470" height="956" alt="Screenshot 2026-03-11 at 1 24 00 PM" src="https://github.com/user-attachments/assets/08a466e5-226c-4e64-b68a-dc3e9b8dc62c" />

## Q.3 Investigate the conn.log file. What is the longest connection duration?
<img width="1470" height="956" alt="Screenshot 2026-03-11 at 1 24 27 PM" src="https://github.com/user-attachments/assets/a64b4901-8748-4315-8e1d-a216886facf0" />


---
# Zeek CLI Kung-Fu & Signatures – Simple SOC Notes

## 1. Why CLI is Important for SOC Analysts

When analysts investigate network logs, the data size can be **very large**.

Graphical tools (GUI) are useful, but they can become:

• slow  
• unstable  
• hard to filter large datasets  

So SOC analysts often use **CLI (Command Line Interface)** tools.

CLI helps analysts to:

• search logs quickly  
• filter specific events  
• extract important fields  
• automate investigations  

This is sometimes called **"CLI Kung-Fu"**, meaning strong command-line skills.

---

# 2. Basic CLI Commands for Log Analysis

## View Command History

Shows previously used commands.

history

Run the 10th command from history:

!10

Run the previous command again:

!!

---

# 3. Reading Files

## View entire file

cat sample.txt

Shows all file content.

---

## View first lines

head sample.txt

Shows the **first 10 lines** of a file.

Useful when logs are very large.

---

## View last lines

tail sample.txt

Shows the **last 10 lines**.

SOC analysts use this to see **latest events** in logs.

---

# 4. Finding and Filtering Data

## grep – Search for keywords

grep "keyword" file.txt

Example:

grep password http.log

Finds lines containing **password**.

Very useful to search:

• suspicious domains  
• malware patterns  
• attacker IPs  

---

## cut – Extract specific columns

cut -f 1 file.txt

Extracts the **first column**.

Logs usually contain many columns, so analysts extract only useful ones.

---

## sort – Sort data

sort file.txt

Sorts data alphabetically.

Sort numbers:

sort -n file.txt

Useful when ranking events or IP activity.

---

## uniq – Remove duplicates

sort file.txt | uniq

Shows only **unique values**.

Example use:

Find unique IP addresses in logs.

---

## uniq -c – Count occurrences

sort file.txt | uniq -c

Example output:

20 192.168.1.5  
5 192.168.1.10  

Meaning:

192.168.1.5 appeared **20 times**.

This helps detect **top communicating hosts**.

---

## wc -l – Count lines

wc -l file.txt

Counts total number of lines.

Useful to measure:

• number of connections  
• number of alerts  

---

# 5. Advanced Log Processing

## sed – Print specific lines

Print line 11:

sed -n '11p' file.txt

Print lines 10 to 15:

sed -n '10,15p' file.txt

---

## awk – Advanced filtering

Print line 11:

awk 'NR == 11' file.txt

Print lines below 11:

awk 'NR < 11' file.txt

awk is widely used for **complex log analysis**.

---

# 6. Zeek-Cut Tool

Zeek logs contain **many columns**.

Example fields in `conn.log`:

• timestamp  
• uid  
• source IP  
• source port  
• destination IP  
• destination port  
• protocol  

Reading the whole log is difficult.

So analysts use **zeek-cut**.

Example:

cat signatures.log | zeek-cut uid src_addr dst_addr

Output example:

UID       SRC_IP        DST_IP  
ABC123    10.0.0.5      8.8.8.8  

This extracts only **important fields**.

---

# 7. What are Zeek Signatures?

Zeek signatures are **rules used to detect patterns in network traffic**.

They help identify suspicious activity.

Example detection:

• password in HTTP traffic  
• brute-force login attempts  
• malicious payloads  

When a signature matches, Zeek creates alerts in logs such as:

• signatures.log  
• notice.log  

---

# 8. Structure of a Zeek Signature

A signature contains **three main parts**.

## 1. Signature ID

Unique name for the rule.

Example:

http-password

---

## 2. Conditions

Defines **when the rule should trigger**.

Two main types:

### Header Filters

Check packet headers.

Examples:

• source IP  
• destination IP  
• port number  
• protocol  

Example condition:

src-ip = 192.168.1.10

---

### Content Filters

Check packet payload for patterns.

Examples:

• HTTP requests  
• FTP commands  
• payload strings  

Example:

payload contains "password"

---

## 3. Action

Defines what happens when a rule matches.

Default action:

Create entry in **signatures.log**

Additional action:

Trigger Zeek scripts or alerts.

---

# 9. Running Zeek with a Signature File

Zeek signature files use the **.sig extension**.

Example command:

zeek -C -r sample.pcap -s sample.sig

Explanation:

-C → ignore checksum errors  
-r → read pcap file  
-s → load signature file  

---

# 10. Example Detection – Cleartext Password

A rule can detect **passwords sent in HTTP traffic**.

If packet contains the word:

password

Zeek triggers an alert.

Logs generated:

notice.log  
signatures.log  

Example alert:

Cleartext Password Found

This means a password was sent **without encryption**.

---

# 11. Example Detection – FTP Admin Login

A rule can detect FTP login attempts using the admin account.

Example detection:

USER admin

Log output example:

FTP Admin Login Attempt

This may indicate:

• account compromise  
• brute force attempts  

---

# 12. Detecting FTP Brute Force

Instead of only detecting admin accounts, analysts can detect **login failures**.

FTP response code:

530 Login incorrect

If many login failures occur:

It may indicate a **brute-force attack**.

Example alerts:

FTP Username Input Found  
FTP Brute-force Attempt  

SOC analysts correlate these alerts to confirm attacks.

---

# 13. Multiple Signatures in One File

A single `.sig` file can contain **multiple detection rules**.

Example file:

ftp-admin.sig

This file might contain rules for:

• admin login detection  
• brute force detection  

This helps detect **multiple threats at once**.

---

# 14. Snort Rules and Zeek

Earlier, a tool called **snort2bro** converted Snort rules to Zeek signatures.

However, this tool is **no longer supported**.

Today, Zeek signatures are usually written manually.

---

# Quick SOC Revision

Important CLI tools for log analysis:

cat  
grep  
cut  
sort  
uniq  
awk  
sed  
zeek-cut  

Zeek signatures are rules that detect patterns in network traffic.

When a signature matches:

• signatures.log is created  
• notice.log may contain alerts  

SOC analysts use CLI tools and Zeek signatures together to **detect attacks and investigate incidents**.
---
# Lab Questions
## Q.1 Investigate the http.pcap file. Create the  HTTP signature shown in the task and investigate the pcap. What is the source IP of the first event?
## Q.2 What is the source port of the second event?
<img width="1470" height="956" alt="Screenshot 2026-03-11 at 5 38 58 PM" src="https://github.com/user-attachments/assets/1126cfca-8123-4732-b3de-f97f7e33c39d" />
## Q.3 Investigate the conn.log. What is the total number of the sent and received packets from source port 38706?
<img width="1470" height="956" alt="Screenshot 2026-03-11 at 5 43 23 PM" src="https://github.com/user-attachments/assets/20205525-b389-43e0-a5ec-e4b16ea87e8e" />
## Q.4 Create the global rule shown in the task and investigate the ftp.pcap file. Investigate the notice.log. What is the number of unique events?
<img width="1470" height="956" alt="Screenshot 2026-03-11 at 5 59 11 PM" src="https://github.com/user-attachments/assets/41dd2252-20ed-4549-9e69-f685dc0e5e0f" />
## Q.5 What is the number of ftp-brute signature matches?
<img width="1470" height="956" alt="Screenshot 2026-03-11 at 6 02 31 PM" src="https://github.com/user-attachments/assets/584346a4-3324-4c7b-a4ec-1a1920126319" />

---
# Zeek Scripts – Simple SOC Notes

## 1. What are Zeek Scripts?

Zeek has its own **scripting language** that allows analysts to automate network analysis.

These scripts help to:

- detect suspicious activity
- extract information from traffic
- automate investigations
- correlate multiple network events

Instead of manually searching logs, Zeek scripts can **automatically detect patterns and print results**.

Example idea:

If a DHCP request appears in network traffic → extract the hostname automatically.

This automation makes Zeek very powerful for **SOC investigations**.

---

# 2. Zeek is Event-Driven

Very important concept.

Zeek is **event-driven**, not packet-driven.

This means Zeek does not process traffic only as raw packets.

Instead, it focuses on **events happening in the network**.

Examples of events:

- HTTP request
- DNS query
- FTP login
- DHCP request
- SSL handshake

When an event occurs, a **Zeek script can run and perform an action**.

Example logic:

Event → HTTP request  
Action → log suspicious domain

---

# 3. Zeek Script File Extension

Zeek scripts use the extension:

.zeek

Example script name:

dhcp-hostname.zeek

---

# 4. Important Script Locations

Zeek has different folders for different types of scripts.

## Base Scripts

Location:

/opt/zeek/share/zeek/base

Purpose:

Default scripts provided by Zeek.

Important rule:

Do NOT modify these scripts.

They are part of the Zeek system.

---

## Site Scripts (Custom Scripts)

Location:

/opt/zeek/share/zeek/site

Purpose:

Store **custom scripts created by users**.

SOC analysts usually place their scripts here.

Example:

Custom detection scripts  
Custom data extraction scripts

---

## Policy Scripts

Location:

/opt/zeek/share/zeek/policy

Purpose:

Scripts that apply **security policies**.

Example:

- detecting suspicious traffic
- defining monitoring rules
- custom network monitoring policies

---

# 5. Zeek Configuration File

Zeek must know which scripts to load during monitoring.

This is done using the configuration file:

/opt/zeek/share/zeek/site/local.zeek

In this file you can load scripts.

Example:

load @script-name

or

load @/path/to/script

When added here, the script runs automatically during **live network monitoring**.

---

# 6. Running Zeek with a Script (PCAP Analysis)

A script can also be run with a **pcap file**.

Example command:

zeek -C -r sample.pcap script.zeek

Explanation:

-C → ignore checksum errors  
-r → read the pcap file  
script.zeek → load and run the script

This method is often used during **incident investigations**.

---

# 7. Example Use Case – Extract DHCP Hostnames

Suppose an analyst wants to extract **DHCP hostnames** from a packet capture.

There are multiple ways to do this.

### Using Wireshark

Possible but manual and slow.

### Using tcpdump or tshark

Possible but requires complex command pipelines.

Example idea:

tcpdump | grep | awk | sort | uniq

This approach works but is harder to manage.

---

### Using Zeek Script

A simple Zeek script can automatically extract hostnames.

Example result:

student01-PC  
vinlap01  

This shows how Zeek scripts simplify **data extraction and analysis**.

---

# 8. Why Zeek Scripts are Powerful

Zeek scripting allows analysts to:

- automate repetitive tasks
- extract specific data from traffic
- correlate multiple network events
- detect custom threats

Instead of writing long command pipelines, a **small script can perform complex analysis**.

---

# 9. Built-In Functions (BIF)

Zeek includes many **built-in functions** called BIF.

These functions help scripts interact with network protocols.

They allow scripts to access:

- connection information
- HTTP data
- DNS queries
- FTP commands
- DHCP options

These built-in functions make script development easier.

---

# 10. Protocol Support

Zeek supports many network protocols.

Examples include:

- HTTP
- DNS
- FTP
- SMTP
- DHCP
- SSL
- SSH

Scripts can monitor events related to these protocols.

Example:

HTTP event → detect suspicious request  
DNS event → detect malicious domain  

---

# 11. Locations for Built-In Functions and Protocols

Built-in functions are located at:

/opt/zeek/share/zeek/base/bif

Plugins for built-in functions:

/opt/zeek/share/zeek/base/bif/plugins

Protocol scripts:

/opt/zeek/share/zeek/base/protocols

These directories contain the **core logic used by Zeek**.

---

# 12. SOC Use Cases of Zeek Scripts

SOC analysts use Zeek scripts for many tasks:

- detecting suspicious DNS queries
- identifying brute-force login attempts
- extracting files from network traffic
- detecting malware communication
- automating threat hunting

Scripts allow analysts to build **custom detection logic** for their environment.

---

# Quick Revision

Zeek scripts are used to **automate network analysis and detection**.

Important points:

- Zeek uses an **event-driven scripting language**
- Script extension is **.zeek**
- Default scripts are stored in the **base directory**
- Custom scripts are stored in the **site directory**
- Scripts can be loaded through **local.zeek**
- Scripts can also run with **pcap analysis**

Zeek scripting allows SOC analysts to create **custom detection and automation for network security monitoring**.


---

# Lab Questions
## Q.1 Investigate the smallFlows.pcap file. Investigate the dhcp.log file. What is the domain value of the "vinlap01" host?
<img width="1470" height="956" alt="Screenshot 2026-03-11 at 6 16 43 PM" src="https://github.com/user-attachments/assets/10283959-614e-4909-b380-1fd46d875f6b" />
## Q.2 Investigate the bigFlows.pcap file. Investigate the dhcp.log file. What is the number of identified unique hostnames?
<img width="1470" height="956" alt="Screenshot 2026-03-11 at 6 28 32 PM" src="https://github.com/user-attachments/assets/59b3efd4-5bea-4d9e-b316-3baeee1882b9" />
## Q.3 Investigate the dhcp.log file. What is the identified domain value?
<img width="1470" height="956" alt="Screenshot 2026-03-11 at 6 30 17 PM" src="https://github.com/user-attachments/assets/e7633427-837b-40be-a56a-79d3565e4fcd" />

---
# Zeek Scripts (101–202) – Simple SOC Notes

## 1. Introduction to Basic Zeek Scripts

Zeek scripts allow analysts to **monitor events and automate analysis**.

Scripts contain elements such as:

- operators
- types
- attributes
- declarations
- statements
- directives

However, the most important concept for beginners is **events**.

Zeek scripts react to **network events**.

---

# 2. Important Basic Events

## zeek_init

This event runs when **Zeek starts**.

Example use:

- print a message
- initialize variables
- start monitoring tasks

Example idea:

When Zeek starts → display message.

Output example:

Started Zeek!

---

## zeek_done

This event runs when **Zeek stops**.

Example use:

- print summary information
- finish logging tasks

Output example:

Stopped Zeek!

---

# 3. Running a Zeek Script

A script can be executed together with a packet capture file.

Example command:

zeek -C -r sample.pcap 101.zeek

Explanation:

-C → ignore checksum errors  
-r → read pcap file  
101.zeek → load the script

When executed, the script prints messages to the terminal while Zeek processes traffic.

Logs are still created separately.

---

# 4. new_connection Event

Zeek automatically generates an event called:

new_connection

This event triggers whenever a **new network connection is detected**.

Example:

Computer A connects to Server B.

The event provides a **connection object (c)** containing details about the connection.

Example connection information:

- source IP address
- source port
- destination IP
- destination port
- connection ID (UID)

Example data fields:

orig_h → source host  
orig_p → source port  
resp_h → destination host  
resp_p → destination port  
uid → unique connection identifier

---

# 5. Raw Connection Output (Script 102)

If a script prints the entire connection object, the output will contain **large amounts of data**.

Example fields included in connection data:

- source IP
- destination IP
- protocol
- connection state
- packet counts
- bytes transferred
- service information

This raw output is useful for learning Zeek’s data structure but is **not practical for real investigations**.

SOC analysts usually filter the information.

---

# 6. Filtering Important Fields (Script 103)

Instead of printing everything, scripts can extract only useful fields.

Example extracted information:

- source IP
- source port
- destination IP
- destination port

Example output:

New Connection Found!  
Source Host: 192.168.121.2 #58304  
Destination Host: 192.168.120.22 #53  

This approach makes investigation easier by showing **relevant connection details only**.

---

# 7. Combining Scripts and Signatures (Script 201)

Zeek scripts can work together with **signatures**.

Signatures detect patterns in traffic, while scripts process the results.

Example scenario:

A signature detects an **FTP admin login attempt**.

When the signature triggers, Zeek generates the event:

signature_match

A script can listen for this event.

Example script logic:

If signature ID = ftp-admin  
→ print alert message.

Example output:

Signature hit! → FTP-Admin

If the signature triggers multiple times, multiple alerts appear.

This technique helps analysts **correlate detection events**.

---

# 8. Running Zeek with Script and Signature

Example command:

zeek -C -r ftp.pcap -s ftp-admin.sig 201.zeek

Explanation:

-C → ignore checksum errors  
-r → read pcap file  
-s → load signature file  
201.zeek → run script

In this case:

Signature detects suspicious traffic  
Script prints alerts in the terminal.

---

# 9. Loading Local Scripts (Script 202)

Zeek includes many built-in scripts.

These scripts can be loaded using the **local configuration system**.

Example command:

zeek -C -r ftp.pcap local

This command loads scripts defined in:

/opt/zeek/share/zeek/site/local.zeek

---

# 10. Logs Generated When Loading Scripts

When many scripts are loaded, Zeek may generate additional logs.

Example logs:

capture_loss.log  
conn.log  
loaded_scripts.log  
stats.log  
weird.log  

These logs provide:

- performance information
- monitoring statistics
- unusual network behavior

---

# 11. Loading a Specific Script

Instead of loading all scripts, analysts can load **a single script**.

Example command:

zeek -C -r ftp.pcap /opt/zeek/share/zeek/policy/protocols/ftp/detect-bruteforcing.zeek

This script detects **FTP brute-force attacks**.

---

# 12. Example Output of FTP Brute Force Detection

Example result:

FTP::Bruteforcing  
10.234.125.254 had 20 failed logins on 1 FTP server in 1 minute

This indicates that a host attempted **multiple failed FTP logins**, which suggests a brute-force attack.

---

# 13. SOC Use of Zeek Scripts

SOC analysts use Zeek scripts to:

- automate traffic analysis
- detect suspicious connections
- correlate security events
- generate custom alerts
- detect brute-force attacks
- monitor protocol behavior

Scripts allow analysts to build **custom detection logic** for their environment.

---

# Quick Revision

Important Zeek script events:

zeek_init → runs when Zeek starts  
zeek_done → runs when Zeek stops  
new_connection → triggers when a new connection is detected  
signature_match → triggers when a signature rule matches

Scripts can:

- extract connection information
- respond to signature alerts
- generate custom outputs
- automate investigations

Zeek scripts combined with signatures create a **powerful network security monitoring system** for SOC environments.
---
# Lab Questions
## Q.1 Go to folder TASK-7/101. Investigate the sample.pcap file with 103.zeek script. Investigate the terminal output. What is the number of the detected new connections?
using command -  zeek -C -r sample.pcap 103.zeek | grep -c 'New Connection Found!'

## Q.2 Investigate the ftp.pcap file with ftp-admin.sig signature and  201.zeek script. Investigate the signatures.log file. What is the number of signature hits?
<img width="1470" height="956" alt="Screenshot 2026-03-11 at 6 50 23 PM" src="https://github.com/user-attachments/assets/3fdc7bde-86fd-4636-90a2-c0ec1e0b4452" />
## Q.3 Investigate the signatures.log file. What is the total number of "administrator" username detections?
<img width="1462" height="86" alt="Screenshot 2026-03-11 at 6 51 23 PM" src="https://github.com/user-attachments/assets/d11f3240-4d12-40de-bcb3-01885ac1f159" />
## Q.4 Investigate the ftp.pcap file with all local scripts, and investigate the loaded_scripts.log file. What is the total number of loaded scripts?
<img width="934" height="81" alt="Screenshot 2026-03-11 at 6 55 14 PM" src="https://github.com/user-attachments/assets/213768ea-928f-4281-a659-0a938f5a6546" />
## Q.5 Investigate the ftp-brute.pcap file with "/opt/zeek/share/zeek/policy/protocols/ftp/detect-bruteforcing.zeek" script. Investigate the notice.log file. What is the total number of brute-force detections?
<img width="1470" height="956" alt="Screenshot 2026-03-11 at 7 01 12 PM" src="https://github.com/user-attachments/assets/72518232-e298-4d17-b3d5-b4460de3f241" />
---

---
# Lab Questions
## Q.1 Investigate the case1.pcap file with intelligence-demo.zeek script. Investigate the intel.log file. Look at the second finding, where was the intel info found? 
<img width="1470" height="413" alt="Screenshot 2026-03-11 at 7 16 02 PM" src="https://github.com/user-attachments/assets/0577757b-ac77-4907-9123-72030e3071d5" />
## Q.2 Investigate the http.log file. What is the name of the downloaded .exe file?
<img width="1465" height="500" alt="Screenshot 2026-03-11 at 7 16 22 PM" src="https://github.com/user-attachments/assets/ed19a642-dc33-4ceb-b98c-1559863c1fb4" />
## Q.3 Investigate the case1.pcap file with hash-demo.zeek script. Investigate the files.log file. What is the MD5 hash of the downloaded .exe file?
<img width="1470" height="956" alt="Screenshot 2026-03-11 at 7 17 46 PM" src="https://github.com/user-attachments/assets/a0628edc-7106-4bbd-aa21-e8dbb47a6dca" />
## Q.4 Investigate the case1.pcap file with file-extract-demo.zeek script. Investigate the "extract_files" folder. Review the contents of the text file. What is written in the file?
<img width="1446" height="204" alt="Screenshot 2026-03-11 at 7 22 01 PM" src="https://github.com/user-attachments/assets/5df54906-c332-4037-99e7-f47f5e2c0af0" />


---
# Zeek Package Manager (zkg) – Simple SOC Notes

## 1. What are Zeek Packages?

Zeek packages are **third-party scripts or plugins** that extend Zeek’s capabilities.

They provide additional features without writing custom scripts.

Packages can help with:

- password detection
- geolocation analysis
- malware detection
- protocol monitoring
- threat intelligence integration

Packages are managed using the **Zeek Package Manager (zkg)**.

---

# 2. Zeek Package Manager (zkg)

Zeek includes a tool called **zkg**.

It allows analysts to:

- install packages
- remove packages
- update packages
- list installed packages
- manage Zeek extensions

Important note:

zkg commands usually require **root privileges**.

---

# 3. Basic zkg Commands

## Install a Package

Install from the Zeek package repository.

zkg install zeek/cybera/zeek-sniffpass

---

## Install from GitHub

zkg install https://github.com/corelight/ztest

---

## List Installed Packages

zkg list

Displays all installed packages.

---

## Remove a Package

zkg remove package_name

Removes an installed package.

---

## Check for Updates

zkg refresh

Checks if installed packages have new versions.

---

## Upgrade Packages

zkg upgrade

Updates all installed packages to the latest version.

---

# 4. Ways to Use Installed Packages

After installation, packages can be used in **three different ways**.

---

## Method 1 – Load Package in a Script

Example script:

@load /opt/zeek/share/zeek/site/zeek-sniffpass

Run Zeek with the script:

zeek -Cr http.pcap sniff-demo.zeek

---

## Method 2 – Load Package by Path

zeek -Cr http.pcap /opt/zeek/share/zeek/site/zeek-sniffpass

---

## Method 3 – Load Package by Name

zeek -Cr http.pcap zeek-sniffpass

This is the **simplest method**.

---

# 5. Example Package – zeek-sniffpass

Package:

zeek/cybera/zeek-sniffpass

Purpose:

Detect **cleartext passwords in HTTP POST requests**.

This package scans HTTP traffic and creates alerts when passwords appear in plaintext.

---

## Example Output (notice.log)

Fields:

id.orig_h → source IP  
id.resp_h → destination IP  
proto → protocol  
note → detection type  
msg → alert message  

Example:

10.10.57.178  44.228.249.3  tcp  SNIFFPASS::HTTP_POST_Password_Seen  
Password found for user BroZeek

Meaning:

A user submitted a password through **unencrypted HTTP traffic**.

This is a security risk because the password can be intercepted.

---

# 6. Why Packages are Useful

Earlier in Zeek analysis, password detection required:

- creating a custom signature
- writing detection rules

With packages, analysts can install **ready-made detection logic**.

Benefits:

- faster setup
- easier threat detection
- less scripting required

---

# 7. Example Package – geoip-conn

Package:

geoip-conn

Purpose:

Adds **geolocation information** to connection logs.

This package identifies the geographic location of IP addresses.

It uses the **GeoLite2-City database**.

---

# 8. Running the GeoIP Package

Example command:

zeek -Cr case1.pcap geoip-conn

After execution, additional fields appear in **conn.log**.

---

# 9. Example GeoIP Output

Example fields added to conn.log:

geo.orig.country_code  
geo.orig.region  
geo.orig.city  
geo.orig.latitude  
geo.orig.longitude  

Example entry:

UID  
Cbk46G2zXi2i73FOU6  

Source IP  
10.6.27.102  

Destination IP  
23.63.254.163  

Country  
US  

City  
Los Angeles  

This indicates the connection was made to a server located in **Los Angeles, United States**.

---

# 10. SOC Use Cases of Packages

SOC analysts use Zeek packages to:

- detect cleartext credentials
- identify suspicious geographic connections
- analyze network traffic faster
- extend Zeek detection capabilities
- automate threat detection

Packages provide **ready-to-use detection logic**, making Zeek easier to deploy in real environments.

---

# Quick Revision

Zeek packages are **third-party extensions** used to extend Zeek functionality.

Managed using:

zkg (Zeek Package Manager)

Important commands:

zkg install → install package  
zkg list → view installed packages  
zkg remove → uninstall package  
zkg refresh → check updates  
zkg upgrade → update packages  

Example packages:

zeek-sniffpass → detects cleartext HTTP passwords  
geoip-conn → adds geolocation data to connection logs  

Zeek packages allow SOC analysts to **quickly add detection features without writing custom scripts**.
----

# Lab Questions
## Q.1 Investigate the http.pcap file with the zeek-sniffpass module. Investigate the notice.log file. Which username has more module hits?
<img width="1470" height="956" alt="Screenshot 2026-03-11 at 7 54 56 PM" src="https://github.com/user-attachments/assets/712421e4-e856-4727-935d-577ef56728b3" />
## Q.2 Investigate the case2.pcap file with geoip-conn module. Investigate the conn.log file. What is the name of the identified City?
## Q.3 Which IP address is associated with the identified City?
<img width="1470" height="956" alt="Screenshot 2026-03-11 at 7 57 09 PM" src="https://github.com/user-attachments/assets/b31a6033-cc7e-4935-853e-6f473c261c67" />
## Q.4 Investigate the case2.pcap file with sumstats-counttable.zeek script. How many types of status codes are there in the given traffic capture?
<img width="1470" height="956" alt="Screenshot 2026-03-11 at 7 59 19 PM" src="https://github.com/user-attachments/assets/fa4ce1d8-d4e6-44cf-9b8a-c0cda308671b" />














