# TShark Command-Line Packet Analysis (Simple Notes)

## 1. What is TShark?

TShark is the **command-line version of Wireshark**.

It is used to:

* Capture network traffic
* Read PCAP files
* Analyze packets using terminal
* Automate investigations with scripts

SOC analysts often use TShark because it works well with **Linux tools and automation**.

---

# 2. Useful Linux Tools in Packet Analysis

These tools help SOC analysts process packet data faster.

| Tool     | Purpose                                        |
| -------- | ---------------------------------------------- |
| capinfos | Shows summary info of a PCAP file              |
| grep     | Search text patterns                           |
| cut      | Extract specific parts of data                 |
| uniq     | Remove duplicate lines                         |
| nl       | Show line numbers                              |
| sed      | Edit and filter text streams                   |
| awk      | Advanced pattern searching and data processing |

Example investigation workflow:

capinfos demo.pcapng → see summary
tshark → analyze packets
grep → search suspicious strings

---

# 3. Checking PCAP File Information

Before starting analysis, SOC analysts first check **capture file details**.

Command:

capinfos demo.pcapng

This shows:

* number of packets
* capture duration
* packet size
* timestamps
* hashes

Why SOC analysts do this:

* understand dataset size
* check if file is corrupted
* know investigation timeframe

---

# 4. Important TShark Parameters

| Parameter | Purpose                       |
| --------- | ----------------------------- |
| -h        | show help menu                |
| -v        | show tshark version           |
| -D        | list network interfaces       |
| -i        | choose interface for sniffing |

Examples

tshark -h
tshark -v
tshark -D
tshark -i 1

SOC Use:

* find correct network interface before capturing traffic.

---

# 5. Packet Sniffing

Sniffing means **capturing live network traffic**.

If no interface is given, TShark uses **interface 1 by default**.

Example

tshark

Choose specific interface

tshark -i 2

SOC Use:

* monitor suspicious network activity
* capture malware communication
* detect lateral movement

---

# 6. Reading PCAP Files

SOC analysts often investigate already captured traffic.

Command:

tshark -r demo.pcapng

Show only first few packets:

tshark -r demo.pcapng -c 5

SOC Use:

* quickly preview packets
* reduce large output

---

# 7. Writing Packets to a New File

You can save selected packets into another file.

Example

tshark -r demo.pcapng -c 1 -w suspicious.pcap

SOC Use:

* isolate malicious traffic
* share evidence with incident response team

---

# 8. Packet Byte View (Hex + ASCII)

TShark can show **raw packet bytes**.

Command

tshark -r demo.pcapng -x

This shows:

* hexadecimal data
* ASCII representation

SOC Use:

* detect encoded payloads
* identify malware signatures
* analyze suspicious data exfiltration

---

# 9. Verbose Packet Details

Verbose mode shows **very detailed packet information**.

Command

tshark -r demo.pcapng -c 1 -V

Shows:

* Ethernet header
* IP header
* TCP/UDP details
* flags
* packet metadata

SOC Use:

* deep packet investigation
* protocol analysis
* malware traffic analysis

Important tip:
Always filter packets first before using verbose mode.

---

# 10. Capture Control (Autostop)

Sometimes SOC analysts need capture to stop automatically.

Parameter: -a

Examples

tshark -w capture.pcap -a duration:5
Stop after 5 seconds

tshark -w capture.pcap -a filesize:10
Stop when file reaches 10 KB

tshark -w capture.pcap -a files:3
Stop after creating 3 files

SOC Use:

* control capture storage
* avoid disk overflow
* scheduled monitoring

---

# 11. Ring Buffer Capture

Ring buffer creates multiple files continuously.

Parameter: -b

Example

tshark -w capture.pcap -b filesize:10 -b files:3

Meaning:

* each file = 10 KB
* keep only 3 files
* overwrite oldest file

SOC Use:

* continuous monitoring
* rotating packet capture logs
* network troubleshooting

---

# 12. Simple SOC Investigation Workflow

Step 1 — Check file info

capinfos traffic.pcap

Step 2 — Preview packets

tshark -r traffic.pcap -c 10

Step 3 — Search suspicious traffic

tshark -r traffic.pcap

Step 4 — Investigate suspicious packets

tshark -r traffic.pcap -V

Step 5 — Extract suspicious packets

tshark -r traffic.pcap -w suspicious.pcap

---

# Key Idea to Remember

Wireshark = GUI analysis
TShark = CLI automation

SOC analysts prefer TShark when:

* analyzing large PCAP files
* automating investigations
* working on Linux servers
---
# TShark Packet Filtering (SOC Notes)

## 1. Why Packet Filtering is Important

In SOC investigations, packet captures can contain **thousands or millions of packets**.

Filtering helps analysts:

* reduce noise
* focus on suspicious traffic
* investigate faster

TShark has **two types of filtering**.

---

# 2. Two Types of Filters

| Filter Type    | When Used                | Purpose                         |
| -------------- | ------------------------ | ------------------------------- |
| Capture Filter | Before capturing traffic | Capture only specific traffic   |
| Display Filter | After capture            | Investigate packets inside PCAP |

Simple idea:

Capture Filter → control **what gets saved**

Display Filter → control **what you see**

---

# 3. Capture Filters (-f)

Capture filters work **during live packet capture**.

They use **BPF syntax** (same as Wireshark capture filters).

Example

tshark -f "host 10.10.10.10"

This captures **only traffic related to that host**.

SOC use:

* capture attacker traffic only
* reduce capture file size
* monitor suspicious IPs

---

# 4. Capture Filter Structure

Capture filters have **three main parts**.

| Component | Meaning                          |
| --------- | -------------------------------- |
| Type      | what to filter (host, net, port) |
| Direction | source or destination            |
| Protocol  | tcp, udp, icmp etc               |

---

# 5. Host Filtering

Capture traffic from a specific host.

Example

tshark -f "host 10.10.10.10"

SOC Use:

* monitor attacker IP
* track communication with C2 server

---

# 6. Network Filtering

Capture traffic from a network range.

Example

tshark -f "net 10.10.10.0/24"

SOC Use:

* monitor entire subnet
* investigate internal network activity

---

# 7. Port Filtering

Capture traffic on a specific port.

Example

tshark -f "port 80"

SOC Use:

* monitor web traffic
* detect malicious services

Port range example

tshark -f "portrange 80-100"

---

# 8. Direction Filtering

Filter based on **traffic direction**.

Source filtering

tshark -f "src host 10.10.10.10"

Destination filtering

tshark -f "dst host 10.10.10.10"

SOC Use:

* identify attacker source
* identify victim systems

---

# 9. Protocol Filtering

Capture only specific protocol traffic.

Example

tshark -f "tcp"

Example

tshark -f "udp"

Example

tshark -f "icmp"

SOC Use:

* monitor malware communication
* detect suspicious protocols

---

# 10. MAC Address Filtering

Capture traffic from specific device.

Example

tshark -f "ether host F8:DB:C5:A2:5D:81"

SOC Use:

* track compromised device in network

---

# 11. Display Filters (-Y)

Display filters are used **after packets are captured**.

They do **not modify the PCAP file**.

They only **filter what you see during investigation**.

Example

tshark -r demo.pcapng -Y 'http'

SOC Use:

* investigate suspicious protocols
* analyze malware traffic

---

# 12. IP Display Filters

Filter packets by IP address.

Example

tshark -Y 'ip.addr == 10.10.10.10'

Source IP

tshark -Y 'ip.src == 10.10.10.10'

Destination IP

tshark -Y 'ip.dst == 10.10.10.10'

SOC Use:

* track attacker IP
* investigate compromised host

---

# 13. TCP Display Filters

Filter TCP ports.

Example

tshark -Y 'tcp.port == 80'

Source port

tshark -Y 'tcp.srcport == 80'

SOC Use:

* investigate suspicious connections
* analyze malware communication

---

# 14. HTTP Display Filters

Filter HTTP traffic.

Example

tshark -Y 'http'

Filter HTTP response code.

Example

tshark -Y "http.response.code == 200"

SOC Use:

* detect malicious downloads
* analyze web-based attacks

---

# 15. DNS Display Filters

Filter DNS traffic.

Example

tshark -Y 'dns'

Filter DNS A records.

Example

tshark -Y 'dns.qry.type == 1'

SOC Use:

* detect DNS tunneling
* find suspicious domain queries

---

# 16. Counting Filtered Packets

TShark keeps **original packet numbers**, not filtered count.

Example

tshark -r demo.pcapng -Y 'http'

Output might show packets like:

4
18
27
38

These are **original packet numbers**.

To count filtered packets easily, use:

tshark -r demo.pcapng -Y 'http' | nl

This adds numbering like:

1
2
3
4

SOC Use:

* count suspicious packets quickly

---

# SOC Investigation Example

Step 1 – Open PCAP

tshark -r traffic.pcap

Step 2 – Filter HTTP traffic

tshark -r traffic.pcap -Y 'http'

Step 3 – Investigate suspicious IP

tshark -r traffic.pcap -Y 'ip.addr == 192.168.1.10'

Step 4 – Analyze DNS traffic

tshark -r traffic.pcap -Y 'dns'

Step 5 – Count suspicious packets

tshark -r traffic.pcap -Y 'http' | nl

---

# Key Difference to Remember

Capture Filter (-f)

Used before capture
Reduces captured traffic

Display Filter (-Y)

Used after capture
Helps investigate packets
