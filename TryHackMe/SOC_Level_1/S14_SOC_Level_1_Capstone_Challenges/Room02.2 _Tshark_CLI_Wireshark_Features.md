# TShark Statistics & Wireshark CLI Features (SOC Notes)

## 1. What are Statistics in TShark?

Statistics help SOC analysts to:

* get quick overview of traffic
* detect anomalies fast
* decide where to investigate

Main command:

tshark -z <option> -q

Important:

* -z → statistics
* -q → show only stats (hide packets)

---

# 2. Color Output

Command:

tshark --color

Purpose:

* shows colored packets (like Wireshark GUI)
* helps spot suspicious traffic quickly

SOC Use:

* faster visual analysis

---

# 3. Protocol Hierarchy

Command:

tshark -r file.pcap -z io,phs -q

Purpose:

* shows all protocols in tree format
* shows packet count + bytes

Example insight:

* TCP = 90% → main traffic
* HTTP present → web activity
* DNS present → domain queries

SOC Use:

* identify dominant protocol
* detect unusual protocol usage

Filter specific protocol:

tshark -r file.pcap -z io,phs,udp -q

---

# 4. Packet Length Statistics

Command:

tshark -r file.pcap -z plen,tree -q

Purpose:

* shows packet sizes distribution

SOC Use:

* very large packets → possible data exfiltration
* very small packets → scanning or beaconing

---

# 5. Endpoints (Who is talking?)

Command:

tshark -r file.pcap -z endpoints,ip -q

Purpose:

* shows all IP addresses
* shows packet + byte count per IP

SOC Use:

* find most active IP
* detect suspicious host

---

# 6. Conversations (Who talks to whom?)

Command:

tshark -r file.pcap -z conv,ip -q

Purpose:

* shows communication between two IPs

SOC Use:

* track attacker ↔ victim
* analyze data flow

---

# 7. Expert Info (Automatic Warnings)

Command:

tshark -r file.pcap -z expert -q

Purpose:

* shows warnings like:

  * retransmissions
  * duplicate ACK
  * connection issues

SOC Use:

* detect network issues
* detect suspicious behavior

---

# 8. Protocol Type Statistics

Command:

tshark -r file.pcap -z ptype,tree -q

Purpose:

* shows protocol distribution

Example:

* TCP → 95%
* UDP → 5%

SOC Use:

* detect abnormal protocol usage

---

# 9. All Hosts (Quick Overview)

Command:

tshark -r file.pcap -z ip_hosts,tree -q

Purpose:

* shows all IPs in one view

SOC Use:

* quickly find unknown/suspicious IP

---

# 10. Source & Destination Analysis

Command:

tshark -r file.pcap -z ip_srcdst,tree -q

Purpose:

* shows source vs destination traffic

SOC Use:

* identify attacker (source)
* identify victim (destination)

---

# 11. Destination & Ports

Command:

tshark -r file.pcap -z dests,tree -q

Purpose:

* shows destination IP + ports

SOC Use:

* detect open services
* find suspicious ports (e.g., 4444, 1337)

---

# 12. DNS Statistics

Command:

tshark -r file.pcap -z dns,tree -q

Purpose:

* shows DNS activity summary

SOC Use:

* detect DNS tunneling
* find suspicious domains

---

# 13. HTTP Statistics

Commands:

tshark -r file.pcap -z http,tree -q
tshark -r file.pcap -z http_req,tree -q
tshark -r file.pcap -z http_seq,tree -q

Purpose:

* show HTTP requests and responses
* show status codes (200, 404, etc)

SOC Use:

* detect malicious downloads
* analyze web traffic behavior

---

# 14. Key SOC Workflow Using Statistics

Step 1 → Overview protocols

tshark -r file.pcap -z io,phs -q

Step 2 → Find active IPs

tshark -r file.pcap -z endpoints,ip -q

Step 3 → Check communication

tshark -r file.pcap -z conv,ip -q

Step 4 → Check anomalies

tshark -r file.pcap -z expert -q

Step 5 → Analyze DNS/HTTP

tshark -r file.pcap -z dns,tree -q
tshark -r file.pcap -z http,tree -q

---

# Final Key Idea

Wireshark GUI → manual analysis
TShark Stats → fast overview + automation

SOC analysts use statistics first → then deep investigation
---
# TShark Advanced Features (Streams, Objects, Credentials)

## 1. Follow Stream (Very Important)

Purpose:

* View full communication between two systems
* Same as "Follow TCP Stream" in Wireshark

Command format:

tshark -r file.pcap -z follow,tcp,ascii,0 -q

Examples:

tshark -r file.pcap -z follow,tcp,ascii,0 -q
tshark -r file.pcap -z follow,udp,ascii,0 -q
tshark -r file.pcap -z follow,http,ascii,0 -q

Important:

* Stream number starts from 0

SOC Use:

* see full attacker communication
* detect credentials, commands, payloads
* understand attack flow

---

# 2. Export Objects (Extract Files)

Purpose:

* extract files from traffic

Supported protocols:

* HTTP
* SMB
* FTP (IMF)
* TFTP

Command:

tshark -r file.pcap --export-objects http,/path/folder -q

Example:

tshark -r file.pcap --export-objects http,/home/user/extracted -q

SOC Use:

* extract malware files
* download suspicious documents
* collect evidence

---

# 3. Credentials Extraction

Purpose:

* find usernames/passwords in cleartext

Command:

tshark -r file.pcap -z credentials -q

Supported protocols:

* FTP
* HTTP
* SMTP
* POP
* IMAP

SOC Use:

* detect leaked credentials
* identify compromised accounts

---

# 4. Advanced Filters (Important for Exams)

## Contains Filter

Purpose:

* search specific word inside packets
* case-sensitive

Example:

tshark -r file.pcap -Y 'http.server contains "Apache"'

SOC Use:

* find specific software
* detect vulnerable servers

---

## Matches Filter (Regex)

Purpose:

* search patterns using regex
* case-insensitive

Example:

tshark -r file.pcap -Y 'http.request.method matches "(GET|POST)"'

SOC Use:

* detect multiple patterns
* find attack behavior

---

# 5. Extract Fields (Very Important)

Purpose:

* extract specific data (like IPs, domains, user agents)

Command format:

tshark -r file.pcap -T fields -e field1 -e field2 -E header=y

Example:

tshark -r file.pcap -T fields -e ip.src -e ip.dst -E header=y

SOC Use:

* create clean datasets
* analyze logs easily
* automate investigation

---

# 6. Real SOC Use Cases

## Extract Hostnames

tshark -r file.pcap -T fields -e dhcp.option.hostname

Better version (clean output):

tshark -r file.pcap -T fields -e dhcp.option.hostname | awk NF | sort -r | uniq -c | sort -r

SOC Use:

* identify devices in network

---

## Extract DNS Queries

tshark -r file.pcap -T fields -e dns.qry.name | awk NF | sort -r | uniq -c | sort -r

SOC Use:

* detect suspicious domains
* detect malware communication

---

## Extract User Agents

tshark -r file.pcap -T fields -e http.user_agent | awk NF | sort -r | uniq -c | sort -r

SOC Use:

* detect tools like:

  * sqlmap
  * nmap
  * fuzzers

---

# 7. Quick SOC Workflow (Advanced)

Step 1 → Check streams
tshark -r file.pcap -z follow,tcp,ascii,0 -q

Step 2 → Extract files
tshark -r file.pcap --export-objects http,/folder -q

Step 3 → Find credentials
tshark -r file.pcap -z credentials -q

Step 4 → Search keywords
tshark -r file.pcap -Y 'http contains "password"'

Step 5 → Extract useful fields
tshark -r file.pcap -T fields -e ip.src -e dns.qry.name

---

# Final Key Idea

Streams → see full communication
Objects → extract files
Credentials → find leaked data
Filters → find patterns
Fields → organize data

All together = powerful SOC investigation

