# Cyber Threat Intelligence (CTI) for SOC L1 Analysts 📝

Imagine you’re the guard at the entrance of a huge colony. Many people come and go every minute. Some are guests, some are delivery boys, and some may be thieves.  
Your job: figure out quickly who’s safe and who’s dangerous. That’s what an L1 SOC Analyst does, but with computer alerts.

---

## 1. What is Threat Intelligence? (CTI)
It’s like having a notebook with info about known thieves, their car numbers, their behaviour, and what they usually do.  
CTI = Knowledge + Context to decide which alert is really dangerous.

**Why it matters:**  
Without CTI = blind guesses.  
With CTI = smart, fast decisions.

---

## 2. Threat Intelligence Lifecycle  
Think of making tea:

- **Data:** Water + tea leaves (raw stuff)  
- **Information:** Tea brewing (you know it’s tea now)  
- **Intelligence:** Sweet hot tea in a cup (ready to drink)  

In SOC terms:

| Layer       | Meaning                          | Example in SOC            | What L1 does |
|-------------|---------------------------------|---------------------------|--------------|
| Data        | Raw observable                  | IP 45.155.205.3 :443       | Capture artefact |
| Information | Data + extra info               | IP from Hetzner hosting   | Record attributes |
| Intelligence| Analysed info (so what?)        | IP = BumbleBee C2 server  | Block/Escalate |

---

## 3. Key Words You’ll See  
- **IOC (Indicator of Compromise):** Evidence of bad stuff already happened. (C2 address found in logs)
- **IOA (Indicator of Attack):** A bad action happening right now. (PowerShell starting weird service)
- **TTP (Tactics, Techniques, Procedures):** The style/steps of attackers (like MITRE ATT&CK IDs).

---

## 4. Indicator Types & Where to Check  
When you see an alert, first identify the *type* of indicator.  
Then use the right tools/sites to “enrich” (add info) quickly.

| Indicator | Example                        | First Look-up Sites/Tools                        | IOA / TTP Example |
|-----------|-------------------------------|-------------------------------------------------|------------------|
| IPv4/IPv6 | 45.155.205.3                  | WHOIS, VirusTotal Relations, Shodan banner scan | IOA: Many SSH login failures (TTP: Password Guessing) |
| Domain    | malicious-updates[.]net       | WHOIS age, RiskIQ/SecurityTrails passive-DNS, urlscan.io | IOA: Many DNS queries to a brand-new domain |
| URL       | hxxp://malicious-updates[.]net/login | URLhaus reputation, urlscan.io graph, Any.Run dynamic run | IOA: Browser posting payload to /gateway.php |
| File Hash | e99a18c428cb38d5…             | VirusTotal static/dynamic, Hybrid-Analysis, MalShare | TTP: Process injection into regsvr32.exe |
| Email     | billing@evil-corp.com          | MXToolbox header analysis, Have I Been Pwned    | IOA: SPF fail + fresh domain registration |
| Local Artefact | HKCU\Software\Run\updater.exe | Sigma rules, EDR query, Vendor knowledge base   | TTP: Registry Run Keys Persistence |

**Tip:**  
Make a browser bookmark folder or SIEM shortcut panel with all your lookup sites.  
Click once → all tabs open → paste the indicator → save time.

---

## 5. Feeds vs Platforms  
- **Feed:** A continuous stream of indicators (like a daily list of suspect car numbers). Format: CSV, JSON, STIX/TAXII.  
  - If you take too many feeds blindly → false alarms everywhere.
- **Platform:** A big, organised database (like a police record system). Examples: MISP, OpenCTI.  
  - Stores indicators, tracks history, maps relationships, controls who can share.

Best practice:  
Add feeds slowly, check if they match your threat model. Only put into platform after testing usefulness.

---

## 6. Sources of CTI  
Where do we get all this info?

| Source            | What it is                                 | Reliability / Notes |
|-------------------|-------------------------------------------|---------------------|
| Internal telemetry| Your own logs: SIEM, EDR, phishing inbox   | Most relevant |
| Commercial services| Paid vendor feeds, sandboxes, closed analytics | High fidelity but sharing limited |
| OSINT             | Free sites like AbuseIPDB, URLhaus, blogs  | Cross-check before use |
| Communities/ISACs | Sector-specific info-sharing groups (FS-ISAC) | Rich context |

Always note where an indicator came from; helps for credibility and legal checks.

---

## 7. Threat Intelligence Classifications  

Think of four levels:

| Class        | Meaning / Focus                                    | Example |
|--------------|---------------------------------------------------|---------|
| Strategic    | Big-picture trends (helps managers/decisions)      | Annual ransomware trends report |
| Tactical     | TTPs, behaviours of attackers                      | Advisory note on new Visual Basic abuse |
| Operational  | Campaign-specific motives/targets                  | Knowing attackers want your payroll system |
| Technical    | The atomic details (IPs, hashes)                   | IP list of a malware campaign |

As an L1 analyst:
- You’ll escalate **Technical** IOCs,
- Observe/document **Tactical** IOAs,
- Identify patterns feeding **Operational** reports.

---

## 8. Quick Recap  
- **CTI = context** to know which alert matters.  
- Move data → info → intelligence.  
- Know IOCs, IOAs, TTPs.  
- Recognise indicator type → right lookup tool.  
- Don’t drown in feeds; use platforms.  
- Always note source.  
- Understand Strategic, Tactical, Operational, Technical intel levels.

---

### Mental Model:
L1 analyst = security guard  
CTI = your visitor logbook + CCTV + neighbourhood watch  
You: enrich raw data until you can say “safe” or “danger” confidently.
---
# CTI Lifecycle 
Imagine Alex is a security guard for TryHatMe’s **big vault** (the PostgreSQL database).  
Alex uses **Cyber Threat Intelligence (CTI)** to spot and stop attackers **before** they break in.  
CTI has a **6-step cycle** – like a loop – that Alex repeats again and again.  
Let’s walk through it like a story.
<img width="637" height="632" alt="Screenshot 2025-10-03 at 7 15 22 PM" src="https://github.com/user-attachments/assets/53ae4f8f-0ad3-4289-9361-5133c86b181a" />

---

## 🎨 Traffic Light Protocol (TLP) – Colour Rules for Sharing Intel  

Think of TLP like stickers on school notes:

| TLP Label | Who can see it? | What Alex does |
|-----------|-----------------|----------------|
| **TLP: CLEAR** (White board) | Anyone | Post openly on wiki/platform |
| **TLP: GREEN** (Green board) | Friends/partners only | Upload to MISP/Slack with partner SOCs |
| **TLP: AMBER** (Yellow board) | Only inside the company (need-to-know clients) | Keep in CTI platform, reference but don’t copy in tickets |
| **TLP: RED** (Red board) | Only specific named people | Store encrypted; don’t post in ticket system without clearance |

Rule: **Always keep the TLP label with the IOC**.  
Breaking it = breaking trust/contracts.

---

## 📦 Intel Formats  

Threat intel often comes in different “boxes” (formats).  
One famous box = **STIX** (JSON style) – machine-readable, describes indicators, relationships, context.

---

## 🌀 The 6 Steps of CTI Lifecycle  

### 1️⃣ Direction – Setting the Goal  

Alex meets the CTI lead and DB admin:  
- **Asset:** PostgreSQL production database.  
- **Risk:** GDPR fines, lost customer trust.  
- **Controls:** Firewall (block IP/domain), EDR (block file hashes).

Alex sets 2 clear questions:  
- **Q1:** Which IPs/domains attack or steal data from PostgreSQL?  
- **Q2:** Which malware targets PostgreSQL drivers/credentials this week, and what are their file hashes?

---

### 2️⃣ Collection – Gathering Raw Data  

Alex pulls info from 4 sources:

| Source | Why? | Example Artefacts |
|--------|------|-------------------|
| NGFW vendor feed | Same firewall model, high-fidelity | 37 IPs flagged as database exfil C2 |
| AbuseIPDB (tag PostgreSQL brute-force) | Fast community updates | 15 IPs, 4 domains |
| Internal MISP | History of incidents | 2 SHA-256 hashes of credential stealers |
| Vendor threat report | Strategic insight → technical IOCs | 1 new malware hash, 3 C2 domains |

Alex exports all as STIX/CSV and stores dated copies in “raw-intel” S3 bucket for reproducibility.

---

### 3️⃣ Processing – Cleaning & Combining  

Feeds = messy. Different styles.  
Alex runs Python scripts to:

- Normalise (lowercase domains, compress IPv6, strip subnet masks).
- Correlate & deduplicate against platform.
- Tag each with source, date, TLP.
- Produce two action files:
  - **firewall_blocklist.csv** → for firewall
  - **edr_hash_rules.yar** → for EDR YARA rules

Example: One IP = TLP: AMBER in NGFW feed but TLP: CLEAR in AbuseIPDB.  
Rule: take stricter label → TLP: AMBER. Prevents accidental oversharing.

---

### 4️⃣ Analysis – Judging What’s Really Bad  

Don’t block everything blindly. Alex checks relevance:

- **Firewall indicators (Q1):**  
  Splunk shows one NGFW IP tried TCP 5432 on the DB subnet.  
  → Yes, relevant.

- **Hash indicators (Q2):**  
  OpenCTI links new hash to PgSteal malware; Any.Run shows credential dump behaviour.  
  → High priority.

Alex grades indicators:

| Confidence | Criteria | Action |
|------------|----------|--------|
| **High** | Same IOC in ≥2 sources + local hit | Immediate block |
| **Medium** | Single trusted source + no local hits | Alert-only |
| **Low** | OSINT only, no context | Monitor 14 days |

7 IPs + 1 hash = High priority.  
Rest = Medium, monitor.

---

### 5️⃣ Dissemination – Sharing to the Right People  

Intel not shared = wasted. Alex tailors output:

| Stakeholder | Format | Why |
|-------------|--------|-----|
| Firewall team | CSV upload + change-ticket | They apply block rules; ticket shows TLP |
| Endpoint team | YARA rule set in EDR console | They load hash rules into policy |
| CTI platform | Full indicator objects with tags | Keeps history, supports future correlation, honours TLP |
| Management | 200-word weekly memo | Show ROI and process status |

Each gets only what they need.

---

### 6️⃣ Feedback – Measure & Improve  

2 weeks later, metrics show:

| KPI | Before | After Cycle |
|-----|--------|-------------|
| Median dwell time of brute-force IPs | 48h | 0h (blocked pre-emptively) |
| False-positive rate | — | 0% |

Success → Management approves next sprint (add DNS tunnelling IOCs).  
Alex updates direction doc, closes loop, starts cycle again.

---

## 📝 Recap Table  

| Step | Name | What Alex Did |
|------|------|---------------|
| 1 | Direction | Set clear questions for DB protection |
| 2 | Collection | Gathered feeds (IPs, hashes, domains) |
| 3 | Processing | Normalised, correlated, tagged, made action files |
| 4 | Analysis | Checked local relevance, graded indicators |
| 5 | Dissemination | Shared tailored outputs to teams/management |
| 6 | Feedback | Measured KPIs, refined cycle |

---

### Mental Picture:
CTI Lifecycle = Washing & Cooking Veggies 🍅  
- Collect veggies (feeds)  
- Clean & cut (processing)  
- Cook & taste (analysis)  
- Serve each guest the right dish (dissemination)  
- Get feedback → improve recipe next time (feedback)

This loop turns raw intel into **actionable security** for the SOC.
--- 
# 🛡️ Standards & Frameworks 

Think of these like **maps** and **rules** in a big game.  
They help everyone talk the same language about threats and how to stop them.  

---

## 🔹 MITRE ATT&CK — “Attack Dictionary”  

- **What it is:**  
  A big list of all the ways hackers attack (called “techniques”).  
  Each technique has a special ID like T1059.001 (PowerShell) or T1048.003 (DNS tunnel).  

- **Why it matters:**  
  Everyone (analyst, vendor, auditor) uses the same ID so there’s no confusion.  

- **How you use it:**  
  1. See the behaviour in your alert (like PowerShell running suspiciously).  
  2. Search in ATT&CK Matrix to find the matching technique.  
  3. Write the ID in your note:  
     “Observed T1071.001 (web-based C2) on FINANCE-TRYHATME-00.”  
  4. Pass the note to L2 or IR. They instantly know what you’re talking about.  

---

## 🔹 MITRE D3FEND — “Defence Dictionary”  

- **What it is:**  
  Like ATT&CK but for **defenders**. It lists how to block or catch each attack.  

- **Why it matters:**  
  You don’t just say “here’s the problem” — you can also suggest a fix.  

- **Example:**  
  - Your proxy flags a DNS tunnel (T1048.003).  
  - You search in D3FEND for DNS defence.  
  - It shows: “Block huge TXT records, alert on weird DNS queries.”  
  - Add that to “Next Actions” in your ticket.  

---

## 🔹 Cyber Kill Chain — “Attack Storyboard”  

This is like a **timeline of an attack**. 7 steps show where the hacker is.  

| Step (Phase) | Meaning (Simple) | Example |
|--------------|-----------------|---------|
| **Recon** | Hacker gathers info about victim | Collecting emails, scanning network |
| **Weaponisation** | Build the malicious file / exploit | Malicious Office doc |
| **Delivery** | Send it to the victim | Email, link, USB |
| **Exploitation** | Break into system / run code | EternalBlue, ZeroLogon |
| **Installation** | Put malware / backdoor inside | RATs, password dumpers |
| **C2 (Command & Control)** | Hacker controls the machine remotely | Cobalt Strike, Empire |
| **Actions on Objectives** | Final goal like stealing data or ransom | Data exfiltration, ransomware |

- **Why it matters:**  
  Helps you know “which stage” the hacker is in so IR can respond faster.  

---

## 🔹 CVEs / CVSS / NVD — “Vulnerability IDs”  

- **CVE:** A simple number for a known vulnerability (like a roll number).  
  Example: CVE-2023-4863.  
- **CVSS:** The score (0–10) showing how bad it is. Higher = more dangerous.  
- **NVD:** Official database linking CVE + CVSS + affected products + exploits.  

- **Why it matters:**  
  Your SOC queue has many vulnerability alerts. These numbers help you sort and escalate the worst ones first.  

---

## 🔹 STIX & TAXII — “Threat Intel Sharing”  

- **STIX:** A format (JSON style) to describe threat info (IOCs, bad IPs, bad domains).  
- **TAXII:** The pipeline (APIs) to send/receive this info automatically.  

  - **Collection model:** One producer hosts intel, you pull it.  
  - **Channel model:** Central server publishes to many at once.  

- **Why it matters:**  
  Real-time feeds mean you can block threats faster.  
  Contributing intel builds trust with other organisations.  

- **But careful:**  
  Don’t share private data, NDA info, or indicators too early — it can warn the hacker you caught them.  

---

## 📝 Quick Workflow for You (SOC L1)  

1. Alert comes in.  
2. Map it to ATT&CK ID.  
3. Note which Kill Chain phase.  
4. If possible, check D3FEND for mitigations.  
5. For vulnerability alerts, check CVE + CVSS in NVD.  
6. Pull/push IOCs via STIX/TAXII feeds if allowed.  

This way, you’re speaking the same language as senior analysts **and** you’re giving them a head start on mitigation.  
