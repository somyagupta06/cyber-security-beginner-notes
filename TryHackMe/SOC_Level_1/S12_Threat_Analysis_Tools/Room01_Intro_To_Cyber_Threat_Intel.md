# Cyber Threat Intelligence (CTI) for SOC L1 Analysts



Imagine you are a **new SOC analyst** who checks security alerts and decides if something is dangerous or not.

---

# 1. What is Cyber Threat Intelligence?

Cyber Threat Intelligence (CTI) means:

Understanding attackers and their activities using data and analysis so that we can **detect and stop attacks faster**.

In simple words:

CTI helps a SOC analyst answer these questions:

1. Who is behind this alert?
2. Is this activity dangerous?
3. What action should we take?

Example:

Alert shows this IP address:

45.155.205.3

If we check threat intelligence tools and discover:

This IP is used by malware Command and Control (C2).

Now the analyst knows:

This alert is **real and dangerous**.

Action:

Block the IP and escalate the incident.

So CTI gives **context** to alerts.

---

# 2. Why CTI is Important in a SOC

A SOC receives many alerts every day.

Example:

| Total Alerts | Real Attacks | Normal Activity |
|---|---|---|
| 200 | 10 | 190 |

Without CTI:

The analyst may waste time on harmless alerts.

With CTI:

The analyst can quickly identify **real threats**.

CTI helps with:

- Alert investigation
- Threat detection
- Faster incident response

---

# 3. Data vs Information vs Intelligence

Security analysis usually moves through three stages.

| Layer | Meaning | Example |
|---|---|---|
| Data | Raw observation | 45.155.205.3 |
| Information | Data with details | IP belongs to Hetzner hosting |
| Intelligence | Final conclusion | IP used by BumbleBee malware C2 |

Example workflow:

Data

45.155.205.3

Information

IP hosted in Germany, provider Hetzner.

Intelligence

This IP is connected to malware infrastructure.

Action

Block the IP and escalate.

SOC L1 analysts help transform **data into intelligence**.

---

# 4. Indicators Used in SOC Investigation

Indicators are clues that help analysts investigate alerts.

The most common indicators are:

| Indicator Type | Example | What Analyst Checks |
|---|---|---|
| IP Address | 45.155.205.3 | Reputation and history |
| Domain | malicious-updates.net | Domain age and DNS records |
| URL | http://malicious-updates.net/login | Website behavior |
| File Hash | e99a18c428cb38d5 | Malware reputation |
| Email Address | billing@evil-corp.com | Email headers and domain |
| System Artefact | HKCU\Software\Run\updater.exe | Persistence behavior |

Each indicator must be **enriched with intelligence tools**.

---

# 5. IOC (Indicator of Compromise)

IOC means:

Evidence that a system **has already been compromised**.

Examples of IOC:

- Malware file hash
- Command and Control IP
- Malicious domain
- Suspicious registry key

Example:

Malware connects to this IP:

45.155.205.3

This IP becomes an **IOC**.

---

# 6. IOA (Indicator of Attack)

IOA means:

Evidence that **an attack is currently happening**.

Examples:

- PowerShell executing suspicious commands
- Multiple login failures
- A process downloading malware

Example:

PowerShell runs this command:

PowerShell downloading a file from the internet.

This shows an **active attack**.

---

# 7. TTP (Tactics, Techniques, Procedures)

TTP describes **how attackers perform attacks**.

Security teams map these behaviors using the **MITRE ATT&CK framework**.

Examples:

| Tactic | Technique | Example |
|---|---|---|
| Initial Access | Phishing | Malicious email |
| Credential Access | Password Guessing | Brute force login |
| Execution | PowerShell | Malicious script execution |
| Persistence | Registry Run Key | Malware starts on boot |

TTPs help analysts understand **attacker behavior patterns**.

---

# 8. Threat Intelligence Feeds

A threat intelligence feed is:

A continuous stream of threat indicators provided by external sources.

Feeds may contain:

- Malicious IP lists
- Malware hashes
- Phishing domains

Example formats:

CSV  
JSON  
STIX  
TAXII

SOC tools can automatically import these feeds.

Important note:

Too many feeds can create **false positives**, so feeds must be carefully selected.

---

# 9. Threat Intelligence Platforms

A Threat Intelligence Platform stores and organizes threat data.

These platforms help analysts:

- store indicators
- track relationships between threats
- share intelligence with other teams

Popular platforms:

MISP  
OpenCTI

These platforms become the **central database for threat intelligence**.

---

# 10. Sources of Threat Intelligence

Threat intelligence comes from different sources.

| Source | Description |
|---|---|
| Internal Data | SIEM logs, EDR alerts, phishing reports |
| Commercial Intelligence | Paid threat intelligence services |
| Open Source Intelligence (OSINT) | Public sources like AbuseIPDB and research blogs |
| Community Sharing | Industry sharing groups like ISAC |

Internal telemetry is usually the **most relevant** for the organization.

---

# 11. Types of Threat Intelligence

Threat intelligence can be divided into four categories.

| Type | Purpose | Example |
|---|---|---|
| Strategic | High level trends | Annual ransomware trends |
| Tactical | Attacker behavior | PowerShell abuse in phishing |
| Operational | Details of specific campaigns | Ransomware targeting banks |
| Technical | Atomic indicators | IPs, hashes, domains |

SOC L1 analysts mainly work with **Technical Intelligence**.

---

# 12. SOC L1 Analyst Workflow Using CTI

Typical SOC alert investigation process:

Alert received

Example alert:

Outbound connection to IP 45.155.205.3

Step 1  
Identify the indicator (IP address)

Step 2  
Check threat intelligence tools

VirusTotal  
AbuseIPDB  
Threat feeds

Step 3  
Analyze the result

Example result:

IP linked to malware Command and Control server

Step 4  
Take action

Block the IP  
Isolate the host  
Escalate to L2 or Incident Response team

---

# Quick Revision Summary

Cyber Threat Intelligence helps SOC analysts understand threats and investigate alerts faster.

Important concepts:

CTI provides context for alerts.

Data → raw observation  
Information → data with context  
Intelligence → analyzed conclusion

Important indicators:

IP  
Domain  
URL  
File hash  
Email  
System artefacts

Key terms:

IOC = evidence of compromise  
IOA = evidence of ongoing attack  
TTP = attacker techniques and behavior

SOC L1 analysts mainly work with **technical indicators and alert triage**.

CTI helps them decide:

Is the alert dangerous?  
Should it be escalated?  
What action should be taken?

---

# Cyber Threat Intelligence Lifecycle (SOC L1 Notes)

Cyber Threat Intelligence (CTI) follows a **six phase lifecycle** that transforms raw threat data into actionable intelligence used by SOC teams.

These steps help analysts detect and respond to threats efficiently.

---

# Traffic Light Protocol (TLP)

Traffic Light Protocol controls how intelligence can be shared.

| TLP | Meaning | SOC Usage |
|---|---|---|
TLP:CLEAR | No restriction | Can be shared publicly |
TLP:GREEN | Community sharing allowed | Share with trusted partners |
TLP:AMBER | Organization only | Keep inside company |
TLP:RED | Named recipients only | Highly restricted sharing |

SOC analysts must always **respect the TLP label** when sharing indicators.

---

# Intelligence Format

Threat intelligence is often shared in structured formats.

One common format is **STIX (Structured Threat Information Expression)**.

STIX is a **JSON based format** used to describe:

- threat indicators
- malware
- attack techniques
- relationships between threats

This allows security tools to automatically process threat intelligence.

---

# CTI Lifecycle Phases

The Cyber Threat Intelligence lifecycle consists of six phases.

---

## 1 Direction

This phase defines the intelligence goals.

Questions are created to guide investigation.

Example:

Which IP addresses are attacking the PostgreSQL server?

Which malware is targeting database credentials?

---

## 2 Collection

Threat data is gathered from different sources.

Common sources include:

Internal logs  
Threat intelligence feeds  
Security research reports  
Community intelligence platforms

Examples of collected artefacts:

Malicious IP addresses  
Domains  
File hashes

---

## 3 Processing

Collected data must be cleaned and standardized.

Processing tasks include:

Normalize indicator formats  
Remove duplicates  
Add metadata such as source and date

Processed data is converted into usable security rules.

Example outputs:

firewall_blocklist.csv  
edr_hash_rules.yar

---

## 4 Analysis

Analysts evaluate the relevance of indicators.

Indicators are checked against internal logs and intelligence platforms.

Indicators may be classified as:

High confidence  
Medium confidence  
Low confidence

High confidence indicators are usually blocked immediately.

---

## 5 Dissemination

Intelligence must be shared with the correct teams.

Examples:

Firewall team receives IP block lists.

Endpoint security team receives malware detection rules.

Management receives a summary of threats.

Proper dissemination ensures that intelligence leads to action.

---

## 6 Feedback

Security teams evaluate the effectiveness of the intelligence process.

Metrics may include:

attack detection time  
false positive rate  
blocked threats

Based on the results, the CTI process is improved for the next cycle.

---

# Key Idea

The CTI lifecycle is a **continuous loop**.

Direction → Collection → Processing → Analysis → Dissemination → Feedback → Repeat

Each cycle improves the organization's ability to detect and respond to cyber threats.

---

# SOC L1 Perspective

SOC Level 1 analysts mainly interact with:

Threat indicators such as IP addresses, domains, and hashes.

They perform:

alert triage  
indicator enrichment  
threat validation

CTI helps them determine whether an alert represents a **real threat or normal activity**.
---

# Threat Intelligence Standards & Frameworks
## SOC L1 Analyst Notes (Simple Revision)

These notes explain the **important standards and frameworks used in Cyber Threat Intelligence (CTI)** from a **SOC Level 1 analyst perspective**.

Standards and frameworks help security teams:

- use common terminology
- communicate clearly with other analysts
- investigate threats in a structured way

---

# 1 MITRE ATT&CK Framework

MITRE ATT&CK is a knowledge base that describes **how attackers perform attacks**.

Each attacker technique is given a **unique ID**.

SOC analysts use ATT&CK to **identify attacker behaviour during investigations**.

Example techniques:

| Technique ID | Description |
|---|---|
T1059.001 | PowerShell Execution |
T1048.003 | DNS Data Exfiltration |
T1071.001 | Web-Based Command and Control |

---

## SOC L1 Usage

During an alert investigation:

1 Identify suspicious behaviour.

Example alert

PowerShell executing encoded command

2 Search the technique in the MITRE ATT&CK database.

Result

T1059.001 PowerShell Execution

3 Add the technique ID to the investigation note.

Example triage note

Observed T1059.001 PowerShell execution on host FINANCE-SERVER-01.

This allows Level 2 analysts and Incident Response teams to quickly understand the attack behaviour.

---

# 2 MITRE D3FEND

MITRE D3FEND is a defensive framework.

If ATT&CK explains **how attackers operate**, D3FEND explains **how defenders respond**.

D3FEND maps defensive controls to attacker techniques.

---

## SOC Example

Alert

DNS tunnelling detected.

ATT&CK mapping

T1048.003 DNS exfiltration.

Using D3FEND

Suggested defensive actions

- analyze DNS request patterns
- detect abnormal TXT record usage
- monitor unusual DNS query entropy

SOC analysts can add mitigation recommendations in their investigation notes.

---

# 3 Cyber Kill Chain

The Cyber Kill Chain is a model developed by Lockheed Martin.

It explains the **stages of a cyber attack**.

Understanding these stages helps SOC analysts identify **where an attack currently is**.

---

## Cyber Kill Chain Stages

| Stage | Purpose | Example |
|---|---|---|
Reconnaissance | Attacker collects information about the victim | Email harvesting, network scanning |
Weaponization | Malware or exploit is prepared | Malicious document |
Delivery | Malware is delivered to the victim | Phishing email |
Exploitation | Vulnerability is exploited | EternalBlue exploit |
Installation | Malware is installed | Backdoor or RAT |
Command and Control | Attacker remotely controls the system | Cobalt Strike |
Actions on Objectives | Attacker completes their goal | Data theft or ransomware |

---

## SOC L1 Usage

Example alert

Suspicious outbound connection detected.

Kill Chain stage

Command and Control

Conclusion

The system may already be compromised.

Action

Escalate to Incident Response and isolate the host.

---

# 4 Vulnerability Identification Frameworks

SOC analysts also handle vulnerability alerts.

Three important systems are used to identify and evaluate vulnerabilities.

---

## CVE (Common Vulnerabilities and Exposures)

CVE provides a **unique identifier for vulnerabilities**.

Example

CVE-2023-4863

Each CVE represents a specific security flaw in software.

---

## CVSS (Common Vulnerability Scoring System)

CVSS measures the **severity of vulnerabilities**.

Score range

0 to 10

| Score Range | Severity |
|---|---|
0–3 | Low |
4–6 | Medium |
7–8 | High |
9–10 | Critical |

Example

CVSS Score: 9.8

This indicates a critical vulnerability that requires urgent attention.

---

## NVD (National Vulnerability Database)

The NVD is a public database that provides detailed vulnerability information.

It links:

- CVE identifiers
- CVSS scores
- affected products
- exploit information

SOC analysts use NVD to understand the impact of vulnerabilities.

---

# 5 Threat Intelligence Sharing Standards

Threat intelligence is often shared between organizations.

Two important standards are used for this purpose.

---

## STIX (Structured Threat Information Expression)

STIX is a structured data format used to describe threat intelligence.

It is usually written in JSON format.

STIX can describe:

- indicators (IPs, domains, hashes)
- malware
- attack techniques
- relationships between threats

STIX allows security tools to process threat intelligence automatically.

---

## TAXII (Trusted Automated Exchange of Indicator Information)

TAXII is a protocol used to **exchange threat intelligence between systems**.

Key idea

STIX defines the data structure.  
TAXII provides the transport method.

Using TAXII, organizations can receive threat intelligence updates automatically.

---

# Threat Intelligence Sharing Models

TAXII supports two sharing models.

Collection

Threat intelligence is stored and collected by a central provider.

Channel

Threat intelligence is broadcast to subscribers in near real-time.

---

# Benefits of Threat Intelligence Sharing

Threat intelligence sharing helps organizations:

- detect attacks earlier
- learn from incidents in other organizations
- improve threat detection capabilities

Example

If one organization shares ransomware indicators, other organizations can block the attack before it spreads.

---

# Limitations of Sharing

Not all threat intelligence can be shared.

Restrictions may exist due to:

- privacy regulations
- customer confidentiality agreements
- internal security policies

In some cases, sharing indicators too early may alert attackers that their campaign has been detected.

---

# Quick Revision Summary

Important frameworks for SOC analysts:

| Framework | Purpose |
|---|---|
MITRE ATT&CK | Identify attacker techniques |
MITRE D3FEND | Identify defensive countermeasures |
Cyber Kill Chain | Identify attack stage |
CVE | Vulnerability identifier |
CVSS | Vulnerability severity score |
NVD | Vulnerability information database |
STIX | Threat intelligence data format |
TAXII | Threat intelligence sharing protocol |

SOC L1 analysts mainly use these frameworks to:

- investigate alerts
- understand attacker behaviour
- recommend defensive actions
- communicate clearly with security teams
