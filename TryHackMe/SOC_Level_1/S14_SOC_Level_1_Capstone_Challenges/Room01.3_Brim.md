# Brim Notes (SOC Point of View – Very Easy English)

## 1. What is Brim?

Brim is an **open-source desktop tool** used to **analyze network traffic**.

It helps security analysts investigate:

- PCAP files (network traffic recordings)
- Zeek logs (network activity logs)

In simple words:

Brim = A tool that helps security analysts **search and investigate network activity easily**.

Instead of looking at millions of packets manually, Brim **converts traffic into readable logs and lets us search them quickly**.

This makes investigations much faster in a **Security Operations Center (SOC)**.

---

## 2. Why SOC Analysts Use Brim

In a SOC, analysts often receive:

- suspicious traffic alerts
- captured network traffic
- malware traffic samples

Sometimes the PCAP files can be very large:

- 1 GB
- 5 GB
- 10 GB

Tools like Wireshark may become **slow for very large PCAPs**.

Brim solves this problem by:

1. Processing the PCAP
2. Creating Zeek logs automatically
3. Allowing fast searching and filtering

So analysts can quickly find **malicious activity**.

---

## 3. Types of Data Brim Can Analyze

Brim mainly works with two types of data.

### 1. PCAP Files

PCAP means **Packet Capture**.

These files contain recorded network traffic.

Example tools that create PCAP files:

- Wireshark
- tcpdump
- tshark

Example file:

incident.pcap

Brim reads the PCAP and **converts the packets into Zeek logs**.

---

### 2. Log Files

Brim can also analyze structured logs such as:

- Zeek logs
- Suricata logs

These logs already contain **organized network information**, which is easier to search.

---

## 4. Technologies Used in Brim

Brim is built using several open-source technologies.

### Zeek

Zeek is a **network security monitoring tool**.

It analyzes PCAP files and creates detailed logs.

Example logs created by Zeek:

conn.log  
dns.log  
http.log  
ssl.log  
files.log  

These logs show different types of network activity.

SOC analysts use these logs to **detect suspicious behavior**.

---

### Zed Language

Zed is the **query language used in Brim**.

It allows analysts to search and filter logs.

Example queries:

_path=="dns"

Shows all DNS traffic.

_path=="http"

Shows all HTTP requests.

_path=="dns" | count() by query

Shows the most requested domains.

---

### ZNG Data Format

ZNG is the **data format used by Brim to store logs**.

It is designed for:

- fast searching
- efficient storage
- handling large datasets

This helps Brim process **large PCAP files quickly**.

---

### Electron and React

Brim uses Electron and React to create a **cross-platform graphical interface**.

This means Brim can run on:

- Windows
- Linux
- macOS

---

## 5. Brim vs Wireshark vs Zeek

All three tools are used in network analysis but for different purposes.

| Tool | Main Purpose | GUI | Packet Level Analysis | Log Analysis |
|-----|-----|-----|-----|-----|
| Brim | Log investigation and searching | Yes | No | Yes |
| Wireshark | Packet analysis | Yes | Yes | No |
| Zeek | Network monitoring and log creation | No | Yes | Yes |

### Typical SOC Workflow

Wireshark → inspect packets deeply  
Zeek → generate network logs  
Brim → search and investigate logs quickly

Each tool supports a different part of the investigation.

---

## 6. Brim Interface Overview

When Brim starts, the main page shows three sections:

Pools  
Queries  
History  

These sections help analysts organize and investigate data.

---

## 7. Pools (Data Sources)

Pools represent **imported data**.

When a PCAP file is loaded into Brim:

1. Brim processes the PCAP
2. Zeek logs are generated automatically
3. Logs are organized in a timeline

Example logs generated:

conn.log  
dns.log  
http.log  
ssl.log  
files.log  

This allows analysts to quickly explore network activity.

---

## 8. Timeline

Brim shows a **timeline of the captured traffic**.

The timeline helps analysts see:

- when the traffic started
- when it ended
- how activity changed over time

This helps identify **the moment when suspicious activity occurred**.

---

## 9. Log Details

Each log entry contains useful fields.

Example fields from conn.log:

ts → timestamp  
uid → unique connection ID  
id.orig_h → source IP  
id.resp_h → destination IP  
duration → connection duration  
proto → protocol used  

SOC analysts use these fields to understand:

- who connected
- where the traffic went
- how long the connection lasted

---

## 10. Correlation

One powerful feature of Brim is **event correlation**.

Correlation means linking different events together.

Example investigation:

Step 1: A computer requests a domain (DNS log)  
Step 2: The computer connects to a server (HTTP log)  
Step 3: A file is downloaded (files.log)

This sequence might indicate **malware download activity**.

Brim helps analysts follow this chain of events.

---

## 11. Useful Actions in Brim

Brim allows several actions when analyzing logs.

Filter values  
Count fields  
Sort data  
View details  
WHOIS lookup for IP addresses  
Open packets in Wireshark

Example:

Right-click an IP address and perform a **WHOIS lookup** to identify the owner of the IP.

This helps determine if the IP might belong to a **malicious server**.

---

## 12. Queries in Brim

Queries are used to **search and analyze logs**.

Example queries.

Show DNS logs:

_path=="dns"

Show HTTP traffic:

_path=="http"

Find most contacted IP addresses:

count() by id.resp_h

Find most requested domains:

_path=="dns" | count() by query

Queries help analysts quickly identify unusual patterns.

---

## 13. Query Library

Brim includes **prebuilt queries**.

These queries help analysts quickly perform common investigations.

Examples include:

Top DNS queries  
Long connections  
HTTP requests  
SSL traffic  

Analysts can run them by simply **double-clicking the query**.

---

## 14. Query History

The History tab stores **previously executed queries**.

This allows analysts to:

- repeat investigations
- track their analysis steps
- reuse useful queries

---

## 15. Simple SOC Investigation Example

Example scenario:

A SOC analyst receives a suspicious network capture.

Step 1  
Load the PCAP file into Brim.

Step 2  
Check DNS activity.

_path=="dns"

Step 3  
Look for unusual domains.

Step 4  
Check HTTP connections.

_path=="http"

Step 5  
Check if files were downloaded.

Step 6  
Correlate DNS → HTTP → file activity.

If a suspicious file download is found, it may indicate **malware infection**.

---
# Lab Questions
## Q.1 Process the "sample.pcap" file and look at the details of the first DNS log that appear on the dashboard. What is the "qclass_name"?
<img width="1470" height="956" alt="Screenshot 2026-03-12 at 9 22 08 AM" src="https://github.com/user-attachments/assets/f3a132c4-54c7-49be-99c2-306c3a2f3bf2" />
## Q.2 Look at the details of the first NTP log that appear on the dashboard. What is the "duration" value?
<img width="1470" height="956" alt="Screenshot 2026-03-12 at 9 25 17 AM" src="https://github.com/user-attachments/assets/0e5acd7d-42a6-4b4f-bdd2-1a3183e1fa5f" />

## Q.3 Look at the details of the STATS packet log that is visible on the dashboard. What is the "reassem_tcp_size"?
<img width="1470" height="956" alt="Screenshot 2026-03-12 at 9 26 10 AM" src="https://github.com/user-attachments/assets/68438569-b364-4cbb-9a9f-ef43f0a8031b" />


---
# Brim Default Queries (SOC Analyst Notes – Easy English)

These are **prebuilt queries available in Brim**.  
SOC analysts use them to **quickly investigate PCAP files** without writing queries manually.

They help answer important investigation questions like:

- What activity exists in the network traffic?
- Are there suspicious domains or IPs?
- Was any file downloaded?
- Are there security alerts?

---

# 1. Reviewing Overall Activity

Purpose:  
This query gives a **general overview of the PCAP file**.

It shows:

- how many logs were generated
- what types of network activities exist
- what data is available for investigation

Example logs that may appear:

conn.log  
dns.log  
http.log  
ssl.log  
files.log  

Why SOC analysts use it:

Before starting an investigation, analysts must know **what logs exist**.  
Without knowing the available logs, it is difficult to create **advanced queries**.

Simple idea:

This query answers the question:

"What type of network activity exists in this PCAP?"

---

# 2. Windows Specific Networking Activity

Purpose:  
This query shows **Windows related network activity**.

It focuses on protocols used by Windows systems such as:

SMB  
Named Pipes  
RPC Endpoints  

These protocols are used for:

- file sharing
- remote services
- authentication
- system communication

Why SOC analysts use it:

Attackers often abuse Windows protocols for attacks such as:

- SMB enumeration
- lateral movement
- service exploitation
- credential attacks

This query helps detect **suspicious Windows behavior in the network**.

Simple idea:

It answers the question:

"What are Windows machines doing in the network?"

---

# 3. Unique Network Connections

Purpose:  
This query shows **unique connections between devices**.

In normal traffic, the same connection may appear many times.

Example:

Computer → Google (100 times)

Instead of showing 100 entries, this query shows only:

Computer → Google

Why SOC analysts use it:

It helps analysts quickly see:

- which IPs were contacted
- unusual or unknown connections
- potential malicious servers

Simple idea:

It answers the question:

"Which servers did the system connect to?"

---

# 4. Transferred Data

Purpose:  
This query shows **how much data was transferred in each connection**.

It summarizes the data exchanged between systems.

Example:

Computer → normal website → 50 KB  
Computer → suspicious server → 2 GB

Large data transfers may indicate:

- data exfiltration
- malware communication
- suspicious downloads

Why SOC analysts use it:

It helps identify **unusual data transfer patterns**.

Simple idea:

It answers the question:

"How much data was sent or received?"

---

# 5. DNS Queries

Purpose:  
This query shows **all DNS requests made in the network traffic**.

DNS converts domain names into IP addresses.

Example domains:

google.com  
microsoft.com  
example.com  

Why SOC analysts use it:

Attackers often use **malicious domains**.

Example suspicious domain:

random-malware-domain.xyz

By reviewing DNS logs, analysts can detect:

- command and control servers
- phishing domains
- malware infrastructure

Simple idea:

It answers the question:

"Which domains did the computer try to reach?"

---

# 6. HTTP Methods

Purpose:  
This query shows **HTTP requests made by the system**.

Common HTTP methods include:

GET  
POST  

GET → request data from a website  
POST → send data to a server

Why SOC analysts use it:

POST requests can sometimes indicate:

- stolen data being sent
- malware communication
- form submissions

Analysts often modify the query to check:

HTTP POST requests  
HTTP GET requests

Simple idea:

It answers the question:

"What kind of web requests were made?"

---

# 7. File Activity

Purpose:  
This query shows **files transferred in the network traffic**.

Information provided includes:

file name  
file type (MIME type)  
hash values  

Common hash values:

MD5  
SHA1  

Why SOC analysts use it:

Files downloaded from suspicious servers may contain malware.

Analysts can use the hash values to check malware databases such as VirusTotal.

Example suspicious files:

malware.exe  
payload.zip  

Simple idea:

It answers the question:

"Were any files downloaded or transferred?"

---

# 8. IP Subnet Statistics

Purpose:  
This query shows **the IP subnet ranges present in the traffic**.

Example internal subnet:

192.168.x.x  

Example external IP:

8.8.8.8

Why SOC analysts use it:

It helps detect:

- communication with external networks
- unknown IP ranges
- unusual network behavior

Example suspicious case:

A system communicates with an unknown foreign IP range.

Simple idea:

It answers the question:

"Which networks did the system communicate with?"

---

# 9. Suricata Alerts

Purpose:  
This query shows **alerts generated by Suricata rules**.

Suricata is a **network intrusion detection system (IDS)**.

It detects suspicious traffic using predefined rules.

Example detections:

malware traffic  
network scanning  
exploit attempts  
command and control activity  

Suricata alerts can be viewed in different formats:

category based  
source and destination based  
subnet based  

Why SOC analysts use it:

It helps quickly identify **potential security threats in the network traffic**.

Simple idea:

It answers the question:

"Did any security detection rule trigger?"

---

# Simple SOC Investigation Flow Using Default Queries

A SOC analyst may investigate a PCAP using this process:

Step 1  
Run **Reviewing Overall Activity**

Step 2  
Check **DNS Queries**

Step 3  
Check **HTTP Methods**

Step 4  
Analyze **Unique Network Connections**

Step 5  
Check **File Activity**

Step 6  
Review **Suricata Alerts**

Finally, the analyst correlates the events to detect possible attacks.

Example attack chain:

DNS request → malicious domain  
HTTP request → attacker server  
File download → malware infection
---
# Lab Questions
## Q.1 Investigate the files. What is the name of the detected GIF file?
<img width="1470" height="956" alt="Screenshot 2026-03-12 at 9 37 23 AM" src="https://github.com/user-attachments/assets/a5a45383-2c7e-4c67-b1ad-95223360840b" />


## Q.2 Investigate the conn logfile. What is the number of the identified city names?
<img width="1470" height="956" alt="Screenshot 2026-03-12 at 9 36 42 AM" src="https://github.com/user-attachments/assets/a1a16a6a-ba72-43e0-9d0f-2c2164759cdd" />
## Q.3 Investigate the Suricata alerts. What is the Signature id of the alert category "Potential Corporate Privacy Violation"?

<img width="1470" height="956" alt="Screenshot 2026-03-12 at 9 38 55 AM" src="https://github.com/user-attachments/assets/873544b8-d331-4b45-807d-ee7f307d3373" />

---
# Brim Custom Queries & Malware C2 Detection (SOC Notes – Easy English)

These notes explain **how SOC analysts use custom queries in Brim** to investigate network traffic and detect **malware Command-and-Control (C2) activity**.

---

# 1. Why Custom Queries Are Important

Default queries help with **basic investigation**, but real SOC investigations often require **custom queries**.

Custom queries help analysts:

- search specific values
- filter suspicious traffic
- detect anomalies
- investigate malware communication

A SOC analyst uses queries to answer questions like:

- Which IPs communicated the most?
- Which domains were requested frequently?
- Did a system download a malicious file?
- Is there Command-and-Control traffic?

---

# 2. Basic Brim Query Syntax

## 2.1 Basic Search

Purpose:  
Search for any value such as an IP address, domain, or keyword.

Example:

10.0.0.1

This will show **all logs containing that IP address**.

---

## 2.2 Logical Operators

Operators used to combine search conditions.

Common operators:

AND  
OR  
NOT  

Example:

192 and NTP

This shows logs that contain both **192 and NTP**.

---

## 2.3 Filter Values

Purpose:  
Filter logs using a **specific field and value**.

Syntax:

field == value

Example:

id.orig_h==192.168.121.40

This filters logs where **source IP equals 192.168.121.40**.

---

## 2.4 List Specific Log File

Purpose:  
Display logs from a specific Zeek log file.

Syntax:

_path=="logname"

Example:

_path=="conn"

This displays **connection logs**.

---

## 2.5 Count Field Values

Purpose:  
Count occurrences of a specific field.

Syntax:

count() by field

Example:

count() by _path

This counts **how many entries exist in each log type**.

---

## 2.6 Sorting Results

Purpose:  
Sort results in ascending or descending order.

Example:

count() by _path | sort -r

This sorts results in **reverse order (highest first)**.

---

## 2.7 Extract Specific Fields (Cut)

Purpose:  
Display only specific fields from logs.

Example:

_path=="conn" | cut id.orig_h, id.resp_p, id.resp_h

This extracts:

- source IP
- destination port
- destination IP

This makes logs **simpler and easier to analyze**.

---

## 2.8 Unique Values

Purpose:  
Show unique values instead of repeating entries.

Example:

_path=="conn" | cut id.orig_h, id.resp_p, id.resp_h | sort | uniq

This shows **unique network connections only**.

---

# 3. Best Practice for Brim Queries

Instead of random keyword searches, analysts should use:

- field filtering
- structured queries
- log specific searches

Reason:

Structured queries are **faster and more accurate**.

---

# 4. Threat Hunting with Brim (Malware C2 Detection)

SOC analysts often hunt for **Command-and-Control (C2) traffic**.

C2 traffic occurs when **infected machines communicate with attacker servers**.

Example scenario:

1. Employee clicks a malicious link
2. A file is downloaded
3. Malware connects to attacker server
4. System begins abnormal network communication

Brim helps detect these activities.

---

# 5. Investigation Step 1 – Identify Available Logs

First step is always checking **what logs exist**.

Brim may generate logs like:

conn.log  
dns.log  
http.log  
ssl.log  
files.log  

These logs provide evidence for investigation.

---

# 6. Investigation Step 2 – Identify Frequent Connections

Query used:

cut id.orig_h, id.resp_p, id.resp_h | sort | uniq -c | sort -r count

Purpose:

- Identify frequently communicating hosts
- Detect suspicious external servers

Example observation:

Suspicious IPs detected:

10.22.x.x  
104.168.x.x

These IPs become **investigation targets**.

---

# 7. Investigation Step 3 – Check Ports and Services

Query:

_path=="conn" | cut id.resp_p, service | sort | uniq -c | sort -r count

Purpose:

- Identify most used ports
- detect unusual services

Example results may show:

80 (HTTP)  
443 (HTTPS)  
53 (DNS)

Large DNS activity may indicate **domain-based malware communication**.

---

# 8. Investigation Step 4 – Analyze DNS Queries

Query:

_path=="dns" | count() by query | sort -r

Purpose:

- find most requested domains
- detect suspicious domains

Example suspicious domain:

maliciousdomain.xyz

SOC analysts often verify domains using **VirusTotal**.

VirusTotal helps identify:

- malicious domains
- attacker infrastructure
- malware campaigns

---

# 9. Investigation Step 5 – Analyze HTTP Requests

Query:

_path=="http" | cut id.orig_h, id.resp_h, id.resp_p, method, host, uri | uniq -c | sort value.uri

Purpose:

- analyze web traffic
- detect file downloads
- identify suspicious web servers

Example finding:

HTTP request downloads a file from a suspicious IP.

This may indicate **malware payload delivery**.

---

# 10. Investigation Step 6 – Validate with Threat Intelligence

Suspicious IPs are checked with **VirusTotal**.

Example discovered malicious IPs:

45.147.x.x  
68.138.x.x  
185.70.x.x  

Threat intelligence confirms:

These IPs are associated with **CobaltStrike infrastructure**.

---

# 11. What is CobaltStrike?

CobaltStrike is a **penetration testing tool**, but attackers frequently use it.

It allows attackers to:

- control infected systems
- move inside networks
- communicate with compromised machines

This communication is called **C2 traffic**.

---

# 12. Investigation Step 7 – Review Suricata Alerts

Query:

event_type=="alert" | count() by alert.severity, alert.category | sort count

Purpose:

- view IDS detection alerts
- identify types of attacks detected

Example categories:

malware activity  
suspicious traffic  
exploit attempts  

Suricata alerts help analysts confirm **malicious activity**.

---

# 13. Important SOC Lesson

Attackers rarely use **only one C2 channel**.

They may use multiple:

- domains
- IP addresses
- communication methods

Therefore SOC analysts must:

- continue investigation
- search for additional suspicious connections
- correlate multiple logs

---

# 14. Typical SOC Threat Hunting Workflow in Brim

Step 1  
Check available logs.

Step 2  
Find frequently communicating hosts.

Step 3  
Analyze ports and services.

Step 4  
Investigate DNS queries.

Step 5  
Analyze HTTP requests.

Step 6  
Check file downloads.

Step 7  
Validate suspicious indicators using VirusTotal.

Step 8  
Review Suricata alerts.

Step 9  
Correlate findings to detect malware C2 communication.

---

# 15. Key Concept

Brim allows SOC analysts to:

- investigate PCAP files quickly
- search large logs efficiently
- detect malicious communication
- perform threat hunting using structured queries
___
# Lab Questions
## Q.1 What is the name of the file downloaded from the CobaltStrike C2 connection?

<img width="1470" height="956" alt="Screenshot 2026-03-12 at 10 07 29 AM" src="https://github.com/user-attachments/assets/36a7ce84-a4e2-49d4-a9d2-c174134b8d60" />

## Q.2 What is the number of CobaltStrike connections using port 443?
<img width="1470" height="956" alt="Screenshot 2026-03-12 at 10 14 40 AM" src="https://github.com/user-attachments/assets/ba6e2c37-12cb-42f4-85dc-a4a10f271a8c" />


## Q.3 There is an additional C2 channel in used the given case. What is the name of the secondary C2 channel?


---
# Threat Hunting with Brim – Crypto Mining Detection (SOC Notes)

This section explains how SOC analysts use **Brim to detect cryptocurrency mining activity (cryptojacking)** in network traffic.

---

# 1. What is Crypto Mining in Security Context?

Cryptocurrency mining is the process of **using computer resources to generate digital currency**.

Attackers sometimes install **mining software on compromised systems**.  
This attack is called **cryptojacking**.

Instead of stealing data, attackers use the victim’s:

- CPU power
- network bandwidth
- electricity
- corporate infrastructure

to mine cryptocurrency.

---

# 2. Why Crypto Mining is a Security Problem

Even though mining may not always involve malware, it is still a **security threat** because it:

- consumes system resources
- slows down corporate systems
- increases electricity costs
- reduces network performance
- may open backdoors through third-party mining tools

Because of these reasons, **crypto mining detection is an important task for SOC threat hunters**.

---

# 3. Investigation Goal

In this scenario, the SOC analyst must determine:

- whether crypto mining activity exists
- which system is responsible
- which mining server is being contacted

The investigation is performed using **Brim queries on PCAP traffic**.

---

# 4. Step 1 – Check Available Logs

First, analysts review **what logs are available**.

Example logs generated from the PCAP:

conn.log  
dns.log  
http.log  

In this case, there are **fewer logs available**, so analysts must rely mainly on **connection logs**.

---

# 5. Step 2 – Identify Frequently Communicating Hosts

Query used:

cut id.orig_h, id.resp_p, id.resp_h | sort | uniq -c | sort -r

Purpose:

- find hosts that communicate frequently
- detect abnormal communication patterns

Observation:

A specific internal IP address such as:

192.168.x.x

appears very frequently.

This indicates that this system may be **communicating heavily with external servers**.

This IP becomes the **primary investigation target**.

---

# 6. Step 3 – Analyze Ports and Services

Query used:

_path=="conn" | cut id.resp_p, service | sort | uniq -c | sort -r count

Purpose:

- identify ports used in network connections
- detect unusual services

Observation:

Multiple unusual ports are used.

Normal network traffic usually involves ports such as:

80 (HTTP)  
443 (HTTPS)  
53 (DNS)

However, mining tools often communicate on **uncommon ports**, which indicates suspicious activity.

---

# 7. Step 4 – Analyze Data Transfer Volume

Query used:

_path=="conn" | put total_bytes := orig_bytes + resp_bytes | sort -r total_bytes | cut uid, id, orig_bytes, resp_bytes, total_bytes

Purpose:

- calculate total data transferred in each connection
- identify systems generating large traffic

Explanation:

The query calculates:

total_bytes = orig_bytes + resp_bytes

This shows **how much data was exchanged during each connection**.

Observation:

A very large amount of traffic is generated from the suspicious IP address.

This strongly suggests **continuous communication with a mining server**.

---

# 8. Step 5 – Use Suricata Alerts

Since there are limited logs available, analysts check **Suricata detection alerts**.

Query used:

event_type=="alert" | count() by alert.severity, alert.category | sort count

Purpose:

- identify security alerts generated by IDS rules
- confirm suspicious activity

Observation:

Suricata alerts indicate:

Crypto Currency Mining activity.

This confirms the hypothesis that **crypto mining traffic is present**.

---

# 9. Step 6 – Identify the Mining Server

Next step is identifying the **external mining pool server**.

Query used:

_path=="conn" | 192.168.1.100

Purpose:

- filter connection logs related to the suspicious internal system
- identify the destination servers it communicates with

The destination IP address discovered in this step likely belongs to a **cryptocurrency mining pool**.

Analysts may confirm this by checking the IP using **VirusTotal** or threat intelligence databases.

---

# 10. Step 7 – Identify MITRE ATT&CK Techniques

Suricata alerts can also show mapped **MITRE ATT&CK techniques**.

Query used:

event_type=="alert" | cut alert.category, alert.metadata.mitre_technique_name, alert.metadata.mitre_technique_id, alert.metadata.mitre_tactic_name | sort | uniq -c

Purpose:

- identify attack techniques used in the activity
- understand attacker behavior

Example results:

Category: Crypto Currency Mining  
Technique Name: Resource Hijacking  
Technique ID: T1496  
Tactic Name: Impact

---

# 11. MITRE ATT&CK Mapping

| Category | MITRE Technique | Technique ID | Tactic |
|--------|--------|--------|--------|
| Crypto Currency Mining | Resource Hijacking | T1496 | Impact |

Explanation:

Resource Hijacking means attackers **abuse victim system resources** for their own purposes.

In this case, the attacker is using the system to **mine cryptocurrency**.

---

# 12. SOC Investigation Summary

The investigation followed these steps:

Step 1  
Check available logs.

Step 2  
Identify frequently communicating hosts.

Step 3  
Analyze ports and services.

Step 4  
Examine data transfer volume.

Step 5  
Check Suricata alerts.

Step 6  
Identify suspicious destination IP.

Step 7  
Confirm mining activity using threat intelligence.

Step 8  
Map activity to MITRE ATT&CK techniques.

---

# 13. Key SOC Indicators of Crypto Mining

Common indicators of crypto mining activity include:

- continuous outbound connections
- communication with mining pool servers
- unusual port usage
- high network traffic volume
- Suricata alerts for mining activity

---

# 14. Key Concept

Brim helps SOC analysts detect crypto mining by:

- analyzing connection logs
- identifying unusual traffic patterns
- correlating IDS alerts
- linking activity with threat intelligence

This makes it possible to **identify resource hijacking attacks inside corporate networks**.
---
# LAb Questions
## Q.1 How many connections used port 19999?

<img width="1470" height="956" alt="Screenshot 2026-03-12 at 10 25 42 AM" src="https://github.com/user-attachments/assets/77ff28bf-dafa-4dc6-a40c-8c0e0ac7602a" />
## Q.2 What is the name of the service used by port 6666?
<img width="1470" height="956" alt="Screenshot 2026-03-12 at 10 27 14 AM" src="https://github.com/user-attachments/assets/1a3e9091-5969-48f9-a560-0967f3dd0a53" />
## Q.3 What is the amount of transferred total bytes to "101.201.172.235:8888"?
<img width="1470" height="956" alt="Screenshot 2026-03-12 at 11 21 39 AM" src="https://github.com/user-attachments/assets/37238c89-3fc7-4fa1-8fc5-ad11acc2d61d" />

## Q.4 What is the detected MITRE tactic id?
<img width="1470" height="956" alt="Screenshot 2026-03-12 at 11 23 41 AM" src="https://github.com/user-attachments/assets/68315cfa-a9f8-4b3e-9cb2-746ac8306e07" />







