# Network Threat Intelligence (SOC L1 Notes)
# Very Simple English | SOC Point of View

Imagine you are a security guard of a company network.
Your job is to check alerts and decide if something is dangerous or normal.

In SOC, many alerts contain:
- IP addresses
- Domains (websites)

Your job is to investigate them.

--------------------------------------------------

# 1. SOC Investigation Process

SOC analysts usually follow a simple process.

Verify → Enrich → Decide

Step 1: Verify
Check if the alert is real.

Things to check:
- Which user accessed it
- Which computer used it
- Time of activity
- How many times it happened

Example

User: HR-PC
Domain: advanced-ip-sccanner.com
Time: 10:12 AM
Action: Allowed

If it looks suspicious → move to enrichment.

--------------------------------------------------

# 2. Enrichment (Most Important Step)

Enrichment means collecting more information about the IP or domain.

Why?

Because a single IP or domain tells us nothing.
We must gather context.

Things SOC analysts check:

- Geolocation
- ASN (network owner)
- DNS records
- Domain age (WHOIS)
- Reputation
- Open services

--------------------------------------------------

# 3. Why DNS Is Important

DNS is like the phonebook of the internet.

Humans use names.
Computers use IP addresses.

DNS converts domain names into IP addresses.

Example

google.com → 142.250.183.100

Every time a user opens a website:
DNS resolves the domain.

Attackers also use DNS for:

- phishing websites
- malware command servers
- fake domains

Because of this, DNS alerts are very valuable for SOC analysts.

--------------------------------------------------

# 4. Important DNS Records (SOC Investigation)

When a SOC analyst investigates a domain,
these records are important.

--------------------------------------------------

A Record

Purpose:
Maps a domain to an IPv4 address.

Example

advanced-ip-sccanner.com → 166.1.160.118

SOC action:
Investigate the IP address.

--------------------------------------------------

AAAA Record

Purpose:
Maps a domain to an IPv6 address.

Example

example.com → 2001:db8::1

--------------------------------------------------

NS Record (Nameserver)

Purpose:
Shows which servers manage the domain.

Example

ns1.namecheap.com
ns2.namecheap.com

SOC observation:

Normal domains
→ known DNS providers

Suspicious domains
→ unknown or newly created DNS servers

--------------------------------------------------

MX Record (Mail Server)

Purpose:
Defines the email server of a domain.

Example

MX → mail.domain.com

SOC use:

Important in phishing investigations.

If a suspicious domain also has mail servers,
it may send phishing emails.

--------------------------------------------------

TXT Record

Purpose:
Stores extra information.

Usually contains:

- SPF
- DKIM
- verification records

Example

v=spf1 include:_spf.google.com ~all

SOC observation:

Poor email configuration can increase phishing risk.

--------------------------------------------------

SOA Record (Start of Authority)

Purpose:
Shows the main authority of the domain.

Contains:

- primary nameserver
- domain admin information
- serial number

SOC use:

Helps understand domain ownership structure.

--------------------------------------------------

TTL (Time To Live)

Purpose:
How long DNS information stays cached.

Example

TTL = 300 seconds

SOC interpretation:

Normal TTL
→ several minutes or hours

Very low TTL
→ domain may change IPs frequently

This may indicate malicious infrastructure.

--------------------------------------------------

# 5. Common DNS Attack Techniques

SOC analysts must recognize these patterns.

--------------------------------------------------

Fast Flux Hosting

Attackers change IP addresses very quickly.

Example

10:00 → 185.22.10.3
10:05 → 91.203.11.20
10:10 → 46.221.9.11

This helps attackers avoid detection.

SOC action:
Escalate investigation.

--------------------------------------------------

CDN Infrastructure

Some legitimate companies rotate IPs too.

Examples:

Cloudflare
Akamai
Fastly

In this case, IP changes are normal.

SOC action:
Check ASN and provider.

--------------------------------------------------

Typosquatting

Attackers create domains similar to famous brands.

Example

Real domain
paypal.com

Fake domain
paypa1.com

Users may not notice the difference.

SOC action:
Treat as high-risk phishing domain.

--------------------------------------------------

IDN (Unicode Domain Attack)

Attackers use special characters that look identical.

Example

xn--ppaypal-3ya.com

It visually looks like paypal.com.

SOC action:
Decode the domain and verify.

--------------------------------------------------

# 6. Tools Used by SOC Analysts

SOC analysts use many tools to investigate IPs and domains.

Common tools

DNS lookup
nslookup.io
dnschecker.org

Reputation tools
VirusTotal
AbuseIPDB
Cisco Talos

Infrastructure analysis
Shodan
Censys

Domain ownership
WHOIS lookup

--------------------------------------------------

# 7. Example SOC Investigation

Alert

Suspicious domain detected

advanced-ip-sccanner.com

SOC investigation steps

Step 1
Check DNS records.

Step 2
Find the IP address.

advanced-ip-sccanner.com → 166.1.160.118

Step 3
Check reputation.

VirusTotal shows multiple detections.

Step 4
Check domain age using WHOIS.

Domain created: 3 days ago

Step 5
Check infrastructure using Shodan.

Open ports:
80
443
8080

--------------------------------------------------

# 8. SOC Decision

After investigation the analyst decides the action.

If domain is malicious:

- Block domain in proxy
- Block related IP addresses
- Alert security team
- Escalate to L2 analysts

If domain is suspicious:

- Monitor activity

If domain is safe:

- Close the alert

--------------------------------------------------
# Lab Questions 

## Q.1 From the downloadable report, what are the IP addresses for the A Record associated with our flagged domain, advanced-ip-sccanner[.]com? Answer: IP-1, IP-2.
<img width="1470" height="956" alt="Screenshot 2026-03-06 at 10 19 29 AM" src="https://github.com/user-attachments/assets/629544c3-34b4-488f-bb0a-6db7ef8d1510" />
## Q.2 What nameserver addresses are associated with the IP address? Defang the addresses.

<img width="1470" height="956" alt="Screenshot 2026-03-06 at 10 19 57 AM" src="https://github.com/user-attachments/assets/395d341c-9a3a-4330-9c7c-c035e1c53238" />

---

# Quick Revision Summary

SOC analysts investigate domains and IPs using threat intelligence.

Main steps:

Verify → Enrich → Decide

Important checks:

- DNS records
- IP geolocation
- ASN owner
- Domain age
- Reputation
- Infrastructure analysis

Goal:

Identify malicious infrastructure
and protect the organization network.

--------------------------------------------------

# IP Enrichment in SOC
SOC Level 1 Analyst Notes (Very Simple English)

In SOC alerts, IP addresses appear very often.

Example alert

IP: 64.31.63.194  
User: HR-PC  
Event: Suspicious connection

An IP address alone does not tell us if it is dangerous.

It could be:
- a normal website server
- a cloud server used by many companies
- a home user device
- a hacker command server

Because of this, SOC analysts perform **IP enrichment**.

IP enrichment means **collecting extra information about the IP** before making a decision.

---

# 1. What Information SOC Analysts Collect

During IP enrichment, analysts collect the following information.

| Information | Why It Is Important |
|---|---|
Owner | Who owns the IP |
ASN | Which network the IP belongs to |
Geolocation | Country of the IP |
Services | What services are running |
History | Has the IP appeared before |

This helps the analyst understand the **role of the IP**.

---

# 2. RDAP (IP Ownership Database)

RDAP stands for **Registration Data Access Protocol**.

It is the official database that shows **who owns an IP address**.

The data comes from internet registries.

Examples of registries

- ARIN
- RIPE NCC
- APNIC

These organizations assign IP address ranges to companies.

SOC analysts use RDAP to confirm the **real owner of the IP**.

Example RDAP result

IP: 64.31.63.194

NetRange: 64.31.56.0 - 64.31.63.255  
Organisation: Limestone Networks  
Abuse Contact: abuse@limestonenetworks.com  

---

# 3. Important RDAP Fields

SOC analysts mainly look at four fields.

## NetRange

Shows the full IP range owned by the organization.

Example

64.31.56.0 - 64.31.63.255

Meaning

The IP is part of a larger block owned by the same company.

---

## Organisation

Shows the registered owner.

Example

Organisation: Amazon AWS

Meaning

The IP belongs to Amazon cloud infrastructure.

---

## Remarks

Sometimes RDAP includes notes about how the IP range is used.

Example

remarks: residential broadband customers

Meaning

The IP belongs to home internet users.

---

## Abuse Contact

Shows the email address used to report malicious activity.

Example

abuse@company.com

SOC teams may send abuse reports to this email.

---

# 4. ASN (Autonomous System Number)

The internet is made of thousands of networks.

Each network has a unique number called an **ASN**.

Example

AS16509 → Amazon AWS  
AS32934 → Facebook  
AS15169 → Google

An ASN represents an organization that controls a network.

SOC analysts use ASN information to understand **what type of infrastructure the IP belongs to**.

---

# 5. Types of ASN Infrastructure

## Hosting Providers

These companies rent servers to customers.

Examples

- DigitalOcean
- OVH
- Hetzner

Attackers often use hosting providers to host malware servers.

SOC observation

Hosting networks may contain malicious servers.

---

## Residential ISPs

These networks belong to home internet providers.

Examples

- Airtel
- Jio
- Vodafone

If suspicious traffic comes from these networks,
it may mean a **compromised home device**.

---

## Cloud Providers

Cloud infrastructure hosts services for many companies.

Examples

- Amazon AWS
- Microsoft Azure
- Google Cloud

Attackers sometimes create temporary servers in cloud platforms.

SOC rule

Never block the entire cloud network.

Instead, block specific IPs or domains.

---

# 6. Geolocation

Geolocation shows the country where the IP is located.

Tools used

- ipinfo.io
- iplocation.net

Example

IP: 64.31.63.194  
Country: United States

---

## Limitations of Geolocation

Geolocation is **not always accurate**.

Reasons

- cloud servers may be registered in one country
- CDN servers operate globally
- city-level information is unreliable

SOC best practice

Check the country using **two different tools**.

Example

ipinfo.io → United States  
iplocation.net → United States

Record the country but treat it as a hint.

---

# 7. Reverse DNS (rDNS)

Reverse DNS converts an IP address into a hostname.

Example

IP: 82.132.231.12

Hostname

host-82-132-231-12.btcentralplus.com

From the hostname we can sometimes identify the provider.

Example

btcentralplus.com → UK broadband provider

Meaning

The IP likely belongs to a residential customer.

SOC rule

Reverse DNS is helpful but **should not be the only evidence**.

---

# 8. Checking Internal Logs

SOC analysts also check internal logs.

This helps understand how often the IP appears.

Example search in SIEM

index=proxy_logs  
ip=64.31.63.194  
last 30 days

Questions analysts ask

- Has this IP appeared before?
- Which users connected to it?
- How many times did it appear?

Example

Seen 1 time → suspicious

Seen many times across many users → likely normal traffic

---

# 9. Classifying the IP Role

After collecting information, SOC analysts classify the IP.

| Category | Meaning |
|---|---|
Hosting | rented servers |
Residential | home user internet |
Cloud | cloud infrastructure |
CDN | content delivery networks |

Example classification

IP: 64.31.63.194  
ASN: Limestone Networks  
Type: Hosting Infrastructure

---

# 10. SOC Decision

After investigation the analyst decides the next action.

Possible actions

Close Alert  
Monitor Activity  
Block IP Address  
Escalate to Level 2

Example

Findings

- IP hosted in a hosting provider
- suspicious connection
- domain linked to phishing

Decision

Block IP in firewall  
Investigate related domains  
Escalate case to L2 analysts

---
# Lab Questions
## Q.1 Open client.rdap.org and identify when the 64[.]31[.]63[.]194 IP was logged for registration.

## Q.2 What roles are assigned to the entity Entity NOC2791-ARIN associated with the IP address?
<img width="1470" height="956" alt="Screenshot 2026-03-06 at 10 42 55 AM" src="https://github.com/user-attachments/assets/e25d88e5-f19f-43b4-93d2-7d5c144fd91b" />
## Q.3 What is the country's name for the same IP address (64[.]31[.]63[.]194)? 
<img width="1470" height="956" alt="Screenshot 2026-03-06 at 10 44 55 AM" src="https://github.com/user-attachments/assets/f686781a-da10-4b9d-8cbf-126b14aadbb7" />
## Q.4 Can you identify the Autonomous System linked with the same IP address?
<img width="1470" height="956" alt="Screenshot 2026-03-06 at 10 45 38 AM" src="https://github.com/user-attachments/assets/12d2ed80-e2d1-420f-9a70-77854c6713e3" />


# Quick Revision Summary

IP enrichment helps SOC analysts understand the role of an IP address.

Main investigation steps

1. Check RDAP ownership  
2. Identify ASN and provider  
3. Verify geolocation  
4. Review reverse DNS  
5. Check internal logs  
6. Classify infrastructure type  
7. Decide the response

Goal

Make evidence-based security decisions and avoid blocking legitimate infrastructure.

---
# Services and Certificates (SOC Investigation Notes)
SOC Level 1 Analyst | Simple English | Quick Revision

When a SOC alert contains an IP address or domain, analysts must understand
what the system is actually doing.

This is done by checking:

- exposed services
- open ports
- server banners
- TLS certificates

These details help analysts understand the **purpose of the system** and
how dangerous it could be.

---

# 1. Why Services Are Important

Every server runs services on specific ports.

Common ports

| Port | Service | Purpose |
|-----|------|------|
80 | HTTP | website |
443 | HTTPS | secure website |
22 | SSH | remote server login |
3389 | RDP | remote desktop |

SOC analysts check open ports to understand **what the server is used for**.

Example

IP: 69.197.185.26

Open ports

80  
873  

Meaning

| Port | Service |
|----|----|
80 | web server |
873 | rsync file transfer |

Some services may increase security risk.

Example

Port 3389 → RDP exposed to internet

This could allow attackers to perform:

- password brute force attacks
- unauthorized remote access

SOC analysts must record exposed services.

---

# 2. Shodan (Internet Device Search Engine)

Shodan is a search engine that scans internet-connected systems.

It helps SOC analysts discover:

- open ports
- running services
- software versions
- device information

Example investigation

Search in Shodan

69.197.185.26

Possible results

Open Ports

80  
873  

Service banner

nginx web server

This tells the analyst that the IP hosts a **web server using Nginx**.

---

# 3. Service Banners

A service banner is information sent by a server when someone connects to it.

Example banner

Server: nginx/1.18.0

This tells analysts:

- software type
- software version

Old or vulnerable versions may indicate security weaknesses.

Example

Apache 2.2

This version is outdated and may contain known vulnerabilities.

SOC analysts record banner information during investigations.

---

# 4. TLS Certificates

Websites using HTTPS must have a TLS certificate.

Example

https://example.com

TLS certificates help browsers verify that the website is legitimate.

SOC analysts investigate TLS certificates to find connections
between different domains.

Tool commonly used

crt.sh

This tool searches **certificate transparency logs**.

---

# 5. Certificate Transparency

Certificate Transparency logs record all publicly trusted certificates.

SOC analysts use these logs to find:

- related domains
- phishing infrastructure
- attacker networks

Example discovery

A certificate may contain multiple domains.

This can reveal **hidden attacker infrastructure**.

---

# 6. Important TLS Certificate Fields

SOC analysts focus on three main certificate fields.

---

## Issuer

Issuer shows which authority signed the certificate.

Example

Issuer: Let's Encrypt

This is a common certificate authority.

Another example

Issuer: Self-signed

Meaning

The server created its own certificate.

This may indicate a temporary or suspicious setup.

---

## Validity Period

Shows how long the certificate is valid.

Example

Validity: 90 days

This is normal for many modern certificates.

However, analysts should watch for:

- repeated certificate generation
- frequent certificate changes

This may indicate phishing infrastructure.

---

## Subject Alternative Names (SAN)

SAN lists all domains covered by the certificate.

Example

example.com  
www.example.com  
api.example.com  

This is normal.

Suspicious example

paypal-login.com  
paypal-update.net  
paypal-secure.org  

These domains imitate a brand and may be used for phishing.

SOC analysts should investigate such patterns.

---

# 7. Censys (Alternative to Shodan)

Censys is another platform used to analyze internet-facing systems.

It can detect:

- open ports
- exposed services
- security misconfigurations

Sometimes Censys detects services that Shodan misses.

Example

Shodan shows

Ports

80  
443  

Censys shows

Ports

80  
443  
56003 (SSH)

This means the system may have additional exposed services.

---

# 8. Pivoting (Advanced SOC Investigation)

Pivoting means using one clue to find more related infrastructure.

Example

A TLS certificate contains several domains.

The analyst searches those domains.

This may reveal:

- additional malicious servers
- phishing websites
- attacker infrastructure

Pivoting helps analysts map an entire attack environment.

---

# 9. Blast Radius Assessment

Blast radius means **how much damage could occur if the system is abused**.

Example scenarios

Case 1

RDP open on residential ISP

Meaning

Likely a compromised home device.

Damage impact

Limited.

---

Case 2

Many domains on a CDN certificate

Meaning

Shared infrastructure.

Blocking the IP could break many legitimate services.

SOC action

Avoid blocking the entire IP.

---

Case 3

Self-signed certificate on a small hosting server

Meaning

Possible attacker control panel or proxy server.

SOC action

Investigate further or block.

---

# 10. SOC Investigation Workflow

SOC analysts follow these steps during IP/domain investigation.

Step 1  
Check exposed services using Shodan or Censys.

Step 2  
Review open ports and service banners.

Step 3  
Investigate TLS certificates using crt.sh.

Step 4  
Identify unusual domains or certificate patterns.

Step 5  
Pivot to related infrastructure.

Step 6  
Assess potential blast radius.

Step 7  
Decide action.

Possible actions

- block IP or domain
- monitor activity
- escalate to higher analysts

---
## Lab Questions
## Q.1 Using shodan.io, what is the first exposed service name of the 85[.]188[.]1[.]133 IP?
<img width="1470" height="956" alt="Screenshot 2026-03-06 at 11 05 40 AM" src="https://github.com/user-attachments/assets/82716ce8-4606-412f-b1f1-09e9830a2757" />
## Q.2 According to crt.sh, what is the Subject's commonName of the identified TLS certificate?
<img width="1470" height="956" alt="Screenshot 2026-03-06 at 11 10 36 AM" src="https://github.com/user-attachments/assets/9d555e18-3306-4a72-9a2b-a087fa680b57" />



# Quick Revision Summary

During investigations SOC analysts analyze services and certificates
to understand system behavior.

Key tasks

- identify open ports
- analyze service banners
- review TLS certificates
- investigate related domains
- evaluate risk level

Goal

Determine whether the system is legitimate infrastructure
or part of malicious activity.

---
# Reputation and History Analysis (SOC Notes)
SOC Level 1 Analyst | Simple English | Quick Revision

During IP or domain investigation, knowing the **owner and services** is not enough.

SOC analysts must also understand **the past behavior of the infrastructure**.

This is because IPs and domains change frequently.

A domain used for phishing today may become inactive next week.

An IP used by attackers today may later be reassigned to a legitimate company.

Because of this, analysts must check **reputation and historical activity**.

---

# 1. Why Reputation and History Are Important

IPs and domains are **dynamic assets**.

This means their usage can change over time.

Example

Today

login-secure-update.com → phishing website

Next week

login-secure-update.com → inactive domain

Because of this, SOC analysts must check **historical intelligence**.

Important questions analysts ask

- Has this indicator been malicious before?
- When was it first seen?
- When was it last active?
- Has its behavior changed recently?

---

# 2. Reputation Services

Reputation services help analysts understand whether an IP or domain
has previously been associated with malicious activity.

Common reputation tools used in SOC

| Tool | Purpose |
|---|---|
VirusTotal | malware and indicator reputation |
Cisco Talos | IP, domain, and email reputation |

---

# 3. VirusTotal

VirusTotal is a threat intelligence platform that analyzes files,
IPs, and domains using multiple security vendors.

SOC analysts check the following information.

Detection ratio

Example

12 / 90 vendors flagged the domain

Meaning

Several security tools detect the indicator as malicious.

Other useful information from VirusTotal

First Seen  
Last Seen  
Related domains and IPs  
Community comments

Example

First Seen: 3 days ago  
Last Seen: today  

Interpretation

The domain is very new and may be part of a phishing campaign.

---

# 4. Cisco Talos Intelligence

Cisco Talos provides reputation scores and category labels
for domains and IP addresses.

Example result

Domain: example.com

Reputation: Neutral  
Category: Technology

Suspicious example

Domain: fake-login.com

Reputation: Poor  
Category: Phishing

SOC analysts record the reputation score and category.

---

# 5. Vulnerability Intelligence

Cisco Talos also provides vulnerability information.

These vulnerabilities are identified using CVE numbers.

Example

CVE-2023-1234  
CVSS Score: 9.8

Meaning

A critical vulnerability exists in the system.

SOC teams may use this information to improve detection rules.

---

# 6. IP2Proxy

IP2Proxy identifies IP addresses that belong to:

- VPN services
- Proxy servers
- Tor exit nodes

Example result

IP: 45.120.34.12  
Type: VPN

Meaning

The real source of traffic may be hidden.

SOC interpretation

Attribution becomes more difficult because the user location is masked.

---

# 7. Passive DNS

Passive DNS records historical DNS data.

It shows how a domain resolved to different IP addresses over time.

Example

Domain: suspicious-domain.com

DNS history

Jan 10 → 185.22.10.3  
Jan 12 → 91.203.11.20  
Jan 14 → 46.221.9.11  

SOC interpretation

Frequent IP changes may indicate malicious infrastructure.

---

# 8. Important Passive DNS Signals

SOC analysts review several signals from passive DNS data.

---

## First Seen

Indicates when the domain first appeared.

Example

First Seen: 2 days ago

Interpretation

Very new domains are often used in phishing campaigns.

---

## Last Seen

Indicates the most recent activity.

Example

Last Seen: today

Interpretation

The domain is currently active.

---

## Number of IPs

Shows how many IP addresses the domain used recently.

Example

10 IPs used in 7 days

Interpretation

High IP rotation may indicate fast-flux hosting.

---

## ASN Spread

Shows how many different networks host the domain.

Example

IP1 → ASN 1234  
IP2 → ASN 5678  
IP3 → ASN 9101  

Interpretation

Multiple unrelated ASNs may indicate suspicious infrastructure.

Example of normal behavior

All IPs belong to Cloudflare ASN.

This likely indicates CDN infrastructure.

---

# 9. Certificate Transparency Logs

Certificate Transparency logs record every publicly trusted TLS certificate.

SOC analysts use CT logs to detect:

- phishing infrastructure
- suspicious domain registrations
- related domains

Example

Certificate contains these domains

paypal-login-secure.com  
paypal-update.net  
paypal-verification.org  

Interpretation

Possible phishing campaign targeting PayPal users.

---

# 10. Wayback Machine

The Wayback Machine shows historical versions of websites.

SOC analysts use it to detect content changes.

Example

example.com

2019 → personal blog  
2025 → phishing login page  

Interpretation

The domain may have been compromised or repurposed.

---

# 11. SOC Investigation Workflow

SOC analysts follow a structured process when analyzing reputation and history.

Step 1  
Check VirusTotal for detection ratio and history.

Step 2  
Check Cisco Talos for reputation score and category.

Step 3  
Use IP2Proxy to identify VPN, proxy, or Tor nodes.

Step 4  
Review Passive DNS records.

Step 5  
Check Certificate Transparency logs.

Step 6  
Use Wayback Machine to analyze historical website content.

---

# 12. SOC Decision

After reviewing reputation and historical intelligence,
analysts must decide the appropriate response.

Possible actions

Close Alert  
Monitor Activity  
Block IP or Domain  
Escalate to higher-level analysts

Decisions should always be based on **evidence and historical context**.

---
# lab Questions
## Q.1 What file has been linked to the IP 166[.]1.160[.]118?
<img width="1470" height="956" alt="Screenshot 2026-03-06 at 11 47 50 AM" src="https://github.com/user-attachments/assets/b218cb2d-96c0-4456-8797-3eeea17b22a4" />
## Q.2 What organisation is identified on historical WHOIS lookups?
<img width="1470" height="956" alt="Screenshot 2026-03-06 at 11 48 16 AM" src="https://github.com/user-attachments/assets/9d399906-8a57-43a5-b2ca-71af562f4e19" />


# Quick Revision Summary

Reputation and historical analysis help SOC analysts understand
how infrastructure has behaved over time.

Important investigation sources

VirusTotal  
Cisco Talos  
IP2Proxy  
Passive DNS  
Certificate Transparency logs  
Wayback Machine

Goal

Use historical intelligence to determine whether
an IP or domain is part of malicious activity.

---
# Turning Intelligence into Action (SOC Analyst Notes)


In SOC work, finding intelligence about an IP or domain is only half the job.

The real task is **turning that intelligence into a safe security action**.

If analysts block too aggressively, they may break legitimate business systems.

If they ignore a threat, an attacker may remain in the network.

Because of this, SOC analysts must apply **precise and controlled actions**.

---

# 1. Safe Integration Patterns

SOC teams should apply blocking rules carefully.

The goal is to **stop the threat without affecting legitimate services**.

---

## Prefer Hostnames Instead of IP Addresses

Many services use dynamic IP addresses.

Examples include:

- CDNs
- cloud infrastructure
- load balancers

Example

example-service.com may resolve to different IPs every day.

If we block the IP address directly,
we might accidentally block other legitimate services.

Better approach

Block the **hostname or domain**.

Example controls

DNS response policy zones (RPZ)  
Proxy filtering rules  
SNI filtering in TLS traffic

This allows the SOC team to block the malicious domain
without affecting other systems using the same IP.

---

## Use Narrow IP Blocks for Dedicated Servers

Some malicious servers use a **single VPS (Virtual Private Server)**.

Example

Attacker command server

IP: 185.120.10.25

In this case, a very precise block can be used.

Example

Block

185.120.10.25 /32

A /32 block means **only one IP address is blocked**.

This keeps the blast radius small.

---

## Set Expiry on Blocks

Internet infrastructure changes frequently.

An IP used by attackers today may belong to a legitimate service next week.

Because of this, SOC teams should add **expiry dates to blocks**.

Example

Block duration

7 days  
14 days  

If malicious activity appears again,
the block can be extended automatically.

This prevents long-term disruption of legitimate services.

---

## Document Evidence in SOAR

All investigation results must be recorded.

SOC teams store evidence in SOAR platforms.

Examples of evidence

Screenshots of investigation tools  
RDAP ownership records  
Certificate information  
Service banner details  
Investigation reasoning

Proper documentation allows other analysts to review the case later.

---

# 2. Geofencing Cautions

Geofencing means blocking traffic from specific countries.

Example

Blocking all traffic from Country X.

This may sound effective,
but it often causes operational problems.

Reasons

Employees may travel internationally.

Cloud services may operate from foreign regions.

Many companies host services globally.

Example

A company in India may use servers in Singapore or the United States.

Blocking those regions may disrupt business systems.

Best practice

Use geolocation as **context information**, not a primary blocking rule.

Geolocation may increase investigation priority,
but should not automatically trigger blocks.

---

# 3. Cloud and CDN Provider Pitfalls

Large cloud providers host millions of services.

Examples

Amazon AWS  
Microsoft Azure  
Google Cloud  
Cloudflare  

These providers reuse IP addresses across many customers.

Example

One AWS IP address may host hundreds of websites.

Blocking a large cloud IP range can cause serious damage.

Example mistake

Blocking the entire Amazon IP range.

Possible consequences

Business applications stop working  
Internal systems fail  
Customer services break

Correct approach

Block the **specific domain or hostname**, not the entire cloud network.

Example

Malicious domain

malicious-app.example

Action

Block the domain instead of blocking the AWS IP.

SOC teams may also contact the provider’s abuse team.

---

# 4. Legal and Provider Considerations

Knowing the infrastructure provider helps SOC teams decide
how to escalate an incident.

Information collected during enrichment includes

- RIR ownership
- hosting provider
- abuse contact email

Example RDAP information

Organisation: Example Hosting Ltd  
Abuse Contact: abuse@examplehosting.com

SOC teams may send abuse reports to request:

- investigation
- takedown of malicious infrastructure

Some providers respond quickly.

Others may respond slowly due to legal or regional limitations.

SOC analysts must record this information
so that escalation paths are clear.

---

# 5. From Data to Decision (SOC Playbook)

SOC analysts follow a structured decision process.

---

## Step 1 — Verify

Confirm the indicator appears in internal logs.

Example

Proxy logs  
Firewall logs  
Email gateway alerts

Ensure the indicator is actually relevant to the environment.

---

## Step 2 — Enrich

Collect intelligence about the indicator.

Examples

Geolocation  
ASN ownership  
Open services  
TLS certificates  
Reputation scores  
Historical activity

---

## Step 3 — Score

Evaluate how suspicious the indicator is.

Factors considered

Reputation results  
Infrastructure type  
Historical behavior  
Known attack patterns

Record the reasoning behind the score.

---

## Step 4 — Decide

Choose the most appropriate action.

Possible actions

Block indicator  
Monitor activity  
Allow traffic

Use precise controls whenever possible.

Add expiry dates to temporary blocks.

---

## Step 5 — Hunt and Notify

Search for related indicators.

Example

Related domains  
Related IP addresses  
Similar certificates

Inform relevant stakeholders.

Examples

Security team  
Network administrators  
Incident response team

Create follow-up investigation tasks if needed.

---

# Quick Revision Summary

SOC analysts must carefully convert threat intelligence into action.

Key principles

Use precise controls  
Avoid blocking large infrastructure ranges  
Add expiry to temporary blocks  
Document all evidence  
Consider business impact

Decision process

Verify → Enrich → Score → Decide → Hunt and Notify

Goal

Stop malicious activity while protecting legitimate business operations.

---
# Challenge

## Q.1 What is the RIR associated with 170[.]130[.]202[.]134?
<img width="1470" height="956" alt="Screenshot 2026-03-06 at 11 54 41 AM" src="https://github.com/user-attachments/assets/8d8841cf-9d71-4fec-969d-074b5015dc88" />
## Q.2 What ASN is the IP connected with?
<img width="1470" height="956" alt="Screenshot 2026-03-06 at 11 55 16 AM" src="https://github.com/user-attachments/assets/d21b3f82-b235-4b47-b453-8c310f855f82" />



















