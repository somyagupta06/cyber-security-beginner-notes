# Intrusion Detection System (IDS)

## Short idea (one-line)
An IDS watches the network from inside the building and tells the security team if something looks wrong after a connection has already passed the firewall.



## Building example 
- **Firewall** = the gate at the building entrance. It checks who can come in or go out.
- **IDS** = CCTV cameras inside the building. If someone sneaks in or starts doing bad things inside, the cameras notice and raise an alarm for the guards.
- IDS does **not** stop the attacker; it only **detects** and **alerts**.



## Two main kinds of IDS 
1. **Network-based IDS (NIDS)**  
   - Sits on the network (like in a corner watching traffic).  
   - Monitors packets on the network links.  
   - Good to see traffic patterns between machines.

2. **Host-based IDS (HIDS)**  
   - Runs on individual computers/servers.  
   - Monitors files, logs, system calls, and local activity.  
   - Good to detect changes on a host (like modified files).



## How IDS detects attacks (two common ways)
1. **Signature-based detection**
   - It uses a list of known bad patterns (signatures).  
   - If traffic matches a known signature -> alert.  
   - Pros: Accurate for known attacks.  
   - Cons: Cannot detect new / unknown attacks (zero-days).

2. **Anomaly-based detection**
   - Learns "normal" behaviour (baseline). If something is different -> alert.  
   - Pros: Can find new or unknown attacks.  
   - Cons: More false alarms until it learns well.

3. **Hybrid** = Uses both signature + anomaly methods for better coverage.



## IDS vs Firewall (comparison)
| Feature | Firewall | IDS |
|---|---:|---|
| Main job | Allow or block connections | Detect suspicious activity and alert |
| Action | Enforces (blocks) | Passive (alerts) |
| Placement | On network boundary/gateway | Inside network (perimeter or internal) |
| Detects | Policy violations, ports, IPs | Known attacks, anomalies, malicious internal actions |
| Example | Block port 23 (Telnet) | Alert when internal host suddenly sends many emails |



## IDS vs IPS 
- **IDS (Intrusion Detection System)**: *Detects & alerts* (passive).  
- **IPS (Intrusion Prevention System)**: *Detects & blocks* automatically (active).  
Think: IDS = CCTV, IPS = an automatic lock that closes the door when it sees a thief.


## Where to put IDS (placement tips)
- At important network segments (server farm, DMZ, critical subnets).  
- Mirror port or network tap to see copy of traffic (NIDS).  
- On critical hosts (HIDS) like domain controllers, DB servers.



## Example scenario (step-by-step)
1. Attacker sends a phishing email → user clicks link → establishes an outgoing connection.
2. Firewall allowed the connection (looks normal).  
3. Attacker downloads a small tool and starts scanning internal hosts.  
4. **NIDS** sees many internal scan packets → anomaly/signature match → generates alert.  
5. Security team receives alert, investigates logs and isolates the infected host.


## What an IDS alert might look like (plain text)
Time: 2025-09-13 10:24:05  
Source IP: 10.0.1.45  
Destination IP: 10.0.2.10  
Signature: SCAN-TCP-SYN-FLOOD  
Severity: HIGH  
Description: Multiple SYN packets to many ports in short time (possible internal port scan)

*(Write commands or follow-up steps as normal lines in the document; do not label them with shell language.)*

Suggested follow-up actions:
- Check the host 10.0.1.45 for new processes and suspicious files
- Block the host on switch port or apply ACLs
- Collect host logs and isolate for forensic analysis


## Pros and Cons (simple)
**Pros**
- Detects attacks missed by firewall
- Helps find insider threats and compromised hosts
- Provides logs and alerts for investigation

**Cons**
- Can produce false positives (especially anomaly-based)
- Needs tuning and maintenance (rules, thresholds)
- Does not stop attack by itself (unless IPS)



## Quick checklist for admins (practical)
- Deploy NIDS at key network points (DMZ, core, inter-VLAN links)  
- Install HIDS on critical servers (DB, domain controllers)  
- Keep signature database updated frequently  
- Tune anomaly system to reduce false alerts (learn baseline)  
- Integrate IDS alerts into a central system (SIEM) for easier triage  
- Define standard playbooks for common alerts (isolate, block, investigate)



## Small comparison table: Signature vs Anomaly
| Aspect | Signature-based | Anomaly-based |
|---|---:|---|
| Detects known attacks | Yes | Maybe (only if different) |
| Detects unknown attacks | No | Yes (possible) |
| False positives | Low (if signatures good) | Higher (until tuned) |
| Maintenance | Update signatures | Train baseline, tune model |



## TL;DR 
- Firewall = gatekeeper. IDS = internal CCTV.  
- IDS detects suspicious activity that passed the firewall and alerts admins.  
- Two types: NIDS (network) and HIDS (host).  
- Two detection methods: signature and anomaly.  
- IDS helps find attackers inside the network but does not block them automatically.

---
# IDS Categorization (Deployment & Detection Modes)

## Main idea
IDS can be categorized based on:
1. **Deployment modes** (where IDS is placed)  
2. **Detection modes** (how IDS detects attacks)  



## Deployment Modes

### 1. Host Intrusion Detection System (HIDS)
- Installed **on individual hosts** (servers, PCs).  
- Monitors files, logs, system calls, local activities of that host.  
- Gives **detailed visibility** of the host.  
- **Challenge**: hard to manage in large networks (needs resources and per-host management).

**Example:**  
If a hacker changes system files on a server, HIDS can detect it.  



### 2. Network Intrusion Detection System (NIDS)
- Installed at **network level** (like a sensor on traffic flow).  
- Monitors **all network traffic** between hosts.  
- Gives a **centralized view** of detections across the network.  

**Example:**  
If an attacker scans multiple machines on the network, NIDS will see suspicious traffic.



### Difference between HIDS and NIDS
| Feature | HIDS (Host IDS) | NIDS (Network IDS) |
|---|---:|---|
| Location | On individual hosts | On network segment/tap |
| Visibility | Local host activities | Network traffic across many hosts |
| Scope | One machine | Entire network |
| Resource use | High (on host) | Lower (centralized) |
| Example use | Detect file changes on server | Detect port scans across subnet |



## Detection Modes

### 1. Signature-Based IDS
- Uses **database of known attack patterns (signatures)**.  
- Matches traffic against stored signatures.  
- **Strong point**: Accurate for **known attacks**.  
- **Weak point**: Cannot detect **zero-day attacks** (new, unknown).  
- **Example tool**: Snort (open-source signature-based IDS).  



### 2. Anomaly-Based IDS
- Learns **normal baseline behavior** of network/system.  
- If activity **deviates from normal**, it raises an alert.  
- **Strong point**: Can detect **zero-day attacks**.  
- **Weak point**: May cause **false positives** (normal activities flagged as malicious).  
- **Fix**: Fine-tune baseline to reduce false alerts.  



### 3. Hybrid IDS
- **Combination** of signature-based + anomaly-based.  
- Uses signature method for known attacks.  
- Uses anomaly method for new/unknown threats.  
- **Best of both worlds**:  
  - Quick detection of known threats.  
  - Ability to detect zero-day attacks.  
- **Downside**: May have **higher processing overhead**.  



## Quick Comparison of Detection Modes

| Feature | Signature-based | Anomaly-based | Hybrid |
|---|---:|---:|---|
| Detects known attacks | ✔ | ✖ | ✔ |
| Detects unknown/zero-day | ✖ | ✔ | ✔ |
| Speed | Fast | Slower (needs analysis) | Medium |
| False positives | Low | High | Medium |
| Maintenance | Update signatures | Train + tune baseline | Both |
| Example | Snort | AI/ML IDS | Suricata (supports hybrid features) |



## Practical Takeaway
- **Signature-based IDS** → Good if your threats are mostly known and you want fast, accurate alerts.  
- **Anomaly-based IDS** → Good for detecting new, unknown attacks but needs fine-tuning.  
- **Hybrid IDS** → Best option for modern environments with both known and unknown threats.  



## TL;DR
- **Deployment**:  
  - HIDS = watches individual host.  
  - NIDS = watches whole network.  
- **Detection**:  
  - Signature-based = known patterns.  
  - Anomaly-based = abnormal behavior.  
  - Hybrid = both methods combined.  
---
# Snort IDS 

## What is Snort?
- **Snort** = Open-source IDS (Intrusion Detection System) created in **1998**.  
- Supports **signature-based** + **anomaly-based** detection.  
- Comes with **built-in rule files** (pre-installed).  
- You can:
  - **Use default rules** → detect known threats.  
  - **Create custom rules** → detect specific traffic you care about.  
  - **Disable rules** → if they are not relevant.  

So, Snort = flexible IDS where you can **tune rules** as per your network.



## Modes of Snort

Snort can run in 3 main modes:

### 1. Packet Sniffer Mode
- **What it does:** Reads & displays packets **without analysis**.  
- **Purpose:** Used for monitoring & troubleshooting (not IDS detection).  
- **Use case:** Network team needs to check traffic flow during a performance issue.  
- **Example:** Show live traffic on the console, or save it to a file.



### 2. Packet Logging Mode
- **What it does:** Captures packets and **saves them to PCAP files**.  
- **Purpose:** Logs traffic for later analysis (forensics).  
- **Use case:** Security team wants to investigate a past attack → they review Snort’s PCAP logs.  
- **Extra note:** Logs contain both raw traffic + detections.



### 3. NIDS Mode (Network Intrusion Detection System)
- **What it does:** Real-time **IDS functionality**.  
- Monitors network traffic and applies **rule files (signatures + anomalies)**.  
- Generates alerts if traffic matches malicious patterns.  
- **Use case:** Continuous proactive monitoring for threats in the network.  
- **Main mode** used when deploying Snort as an IDS.



## Quick Comparison of Snort Modes

| Mode | Description | Primary Use Case |
|---|---:|---|
| Packet Sniffer | Reads packets, shows them (no detection) | Troubleshooting network issues |
| Packet Logging | Captures packets into PCAP files | Forensics & root cause analysis |
| NIDS | Monitors + detects malicious traffic using rules | Real-time IDS security |



## TL;DR
- **Snort** = free, open-source IDS.  
- Works with **rules** (built-in + custom).  
- **Modes:**  
  - Sniffer = just look.  
  - Logging = save for later.  
  - NIDS = detect & alert (main IDS mode).  
---

# Snort — Installation, Rules, and Testing 

## 1. Snort Installation Setup
- When you install Snort, you must tell it:
  - **Network interface** (e.g., `eth0`, `wlan0`, or `lo` for loopback).  
  - **Network range** (e.g., `192.168.1.0/24`).  
- By default, Snort only captures traffic for its own host.  
- To see **all traffic in the network**, you must enable **promiscuous mode** on the interface.



## 2. Snort Directory
Snort stores its important files in `/etc/snort`.

List contents:
```
ubuntu@tryhackme:~$ ls /etc/snort
classification.config reference.config snort.debian.conf
community-sid-msg.map rules threshold.conf
gen-msg.map snort.conf unicode.map
```
- **snort.conf** → main config file (enables rules, sets HOME_NET, etc.)  
- **rules/** → folder where built-in and custom rules are kept.  
- **local.rules** → your custom rule file.



## 3. Rule Format (Example: Detect ICMP/Ping)
A sample rule to detect ping (ICMP) traffic:
```
alert icmp any any -> $HOME_NET any (msg:"Ping Detected"; sid:10001; rev:1;)
```
### Rule Components
- **Action**: `alert` → raise alert when matched.  
- **Protocol**: `icmp` → applies to ICMP traffic (like ping).  
- **Source IP/Port**: `any any` → traffic can come from any host/port.  
- **Destination IP/Port**: `$HOME_NET any` → traffic going to your home network.  
- **Metadata (inside parentheses)**:  
  - `msg` → message when triggered.  
  - `sid` → signature ID (unique rule number).  
  - `rev` → revision number (increase when modified).  



## 4. Creating a Custom Rule
Open the `local.rules` file:
```
ubuntu@tryhackme:~$ sudo nano /etc/snort/rules/local.rules
```
Add rule:
```
alert icmp any any -> 127.0.0.1 any (msg:"Loopback Ping Detected"; sid:10003; rev:1;)
```
Save file (`Ctrl+X`, press `y`, then Enter).



## 5. Running Snort in Detection Mode
Run Snort with the config file:
```
ubuntu@tryhackme:~$ sudo snort -q -l /var/log/snort -i lo -A console -c /etc/snort/snort.conf
```
- `-q` → quiet mode.  
- `-l` → log directory.  
- `-i lo` → network interface (`lo` = loopback).  
- `-A console` → show alerts on console.  
- `-c` → path to config file.  



## 6. Testing the Rule
Ping loopback:
```
ubuntu@tryhackme:~$ ping 127.0.0.1
```
Snort output (sample alert):
```
07/24-10:46:52.401504 [] [1:1000001:1] Loopback Ping Detected [] [Priority: 0] {ICMP} 127.0.0.1 -> 127.0.0.1
```
This means Snort successfully detected ICMP traffic based on our rule.



## 7. Running Snort on PCAP Files
Snort can also analyze **historical traffic logs (PCAP files)**.

Command:
```
ubuntu@tryhackme:~$ sudo snort -q -l /var/log/snort -r Task.pcap -A console -c /etc/snort/snort.conf
```
- `-r Task.pcap` → read packets from `Task.pcap`.  
- Useful for **forensics** and **root cause analysis** of past attacks.



## 8. Quick Recap
- Snort needs **interface + network range** for setup.  
- Rules are stored in `/etc/snort/rules/`.  
- **Custom rules** go in `local.rules`.  
- You can run Snort in **real-time detection** or on **PCAP logs**.  
- Example rule detects **ICMP ping** traffic to loopback.  
- Output shows alerts on console or logs.  



## TL;DR
- Snort = rule-based IDS with flexible configuration.  
- Write custom rules in `local.rules`.  
- Run Snort with `snort.conf`.  
- Test with ping → get alert → confirm IDS working.  
- Can analyze both **live traffic** and **saved PCAP files**.  
