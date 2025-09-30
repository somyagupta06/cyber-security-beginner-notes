# 💎 Diamond Model of Intrusion Analysis 

---

## What is the Diamond Model? (Very Simple)
- Made in 2013 by **Sergio Caltagirone, Andrew Pendergast, Christopher Betz**.  
- It has **four main parts**: **Adversary**, **Infrastructure**, **Capability**, **Victim**.  
- These four parts connect to form a **diamond shape** — that’s why it’s called the Diamond Model.  
- Use: helps understand and explain a cyber attack step-by-step.

---

## The Four Core Features (Easy)

1. **Adversary**  
   - The person or group doing the attack (hacker, attacker, threat actor).  
   - Hard to know at first — often empty until we collect evidence.  
   - Two types to think about:
     - **Adversary Operator** = the actual person or team doing the attack.
     - **Adversary Customer** = who benefits from the attack (may be same as operator or different group).
   - Example: Operator = coder who writes malware. Customer = company paying for the malware.

2. **Infrastructure**  
   - Tools and systems used by the attacker to run the attack.  
   - Examples: malicious servers, botnets, phishing websites, command & control servers (C2).  
   - Think: where the attacker hides and controls their tools.

3. **Capability**  
   - The skills, malware, tools, or techniques the attacker uses.  
   - Examples: ransomware, exploit code, phishing templates, custom scripts.  
   - Shows *what the attacker can do*.

4. **Victim**  
   - The target of the attack (person, company, server, device).  
   - Example: a bank, a hospital, one user’s PC, or a cloud server.

---

## Extra Axes (Context) — Simple
- The model also uses **context** to expand understanding:
  - **Social**: human parts — employees, social engineering, insider help.  
  - **Political**: motives like nation-state goals, activism, or economic gain.  
  - **Technology**: software, hardware, protocols used.

These axes help explain *why* or *how* the attack happened beyond the diamond edges.

---

## How the Diamond Model Helps (Very Simple)
- Breaks an intrusion into clear parts so we can study it.  
- Link evidence: e.g., a malicious IP (infrastructure) + malware sample (capability) + target company (victim) → possible attacker (adversary).  
- Good for explaining to non-technical people: show the four boxes and how they connect.  
- Helps defenders: automate detection, group related incidents, predict attacker actions.

---

## Quick Example (Simple Case)
- **Adversary**: Unknown hacker group (operator), paid by a fraud group (customer).  
- **Infrastructure**: Phishing site + C2 server at IP 1.2.3.4.  
- **Capability**: Phishing email + credential-stealing script.  
- **Victim**: Employees of Company X (finance team).

You can draw edges: Adversary → uses → Infrastructure → launches → Capability → affects → Victim.

---

## Short Exercise (Do this to practice)
1. Pick a news story about a breach.  
2. Write four short lines: Adversary / Infrastructure / Capability / Victim.  
3. Add 1 sentence of context (Social, Political or Technology).  
4. Try to fill gaps with possible evidence (IP, malware name, motive).

---

## One-Line Analogy
- The Diamond Model is like **crime scene cards**: who did it (adversary), what tools they used (capability), where they worked from (infrastructure), and who got hurt (victim).

---

## Small Cheat Sheet (Very Quick)
- Adversary = who  
- Infrastructure = where (servers, URLs, IPs)  
- Capability = how (malware, tools)  
- Victim = whom

---
# 💎 Diamond Model — Deep Dive (Victim, Capability, Infrastructure)

---

## 1️⃣ Victim
- **Definition**: Target of the adversary. Could be:
  - Organization, person, email, IP, domain, system, network, etc.
- **Why important**: Every cyberattack has a victim — the adversary wants to exploit them or their assets.
- **Two parts**:
  1. **Victim Personae**: Who is being targeted.
     - Examples: Organization names, specific employees, industries, job roles, interests.
  2. **Victim Assets**: What is being attacked.
     - Examples: Systems, networks, email addresses, hosts, IPs, social media accounts.
- **Example**:
  - Spear-phishing email sent to a finance employee → employee clicks → **victim persona** = employee, **victim asset** = email account/system accessed.

---

## 2️⃣ Capability
- **Definition**: Skills, tools, and techniques the adversary uses to perform an attack.  
- **Purpose**: Shows *how* the adversary attacks the victim.
- **Includes**:
  - Manual methods: password guessing
  - Advanced methods: malware development, ransomware, phishing campaigns
- **Key terms**:
  1. **Capability Capacity**: Vulnerabilities or opportunities the capability can exploit.
  2. **Adversary Arsenal**: Complete set of capabilities an adversary has.  
     - Example: An adversary with phishing + malware + exploit scripts has a bigger arsenal.
- **Notes**:
  - Adversary may create capabilities themselves (skills) or acquire them (buy malware as a service).

---

## 3️⃣ Infrastructure
- **Definition**: Physical or logical systems the adversary uses to deliver capabilities or maintain control.
- **Examples**:
  - Command & Control (C2) servers
  - IP addresses, domains, email accounts
  - Malicious USBs or other hardware
- **Types**:
  1. **Type 1 Infrastructure**: Controlled or owned by the adversary.
     - Example: attacker’s own server or lab environment.
  2. **Type 2 Infrastructure**: Controlled by intermediaries, may be unaware.
     - Purpose: hide the real source (obfuscation)
     - Example: compromised servers, malware staging servers, spoofed domains
- **Service Providers**:
  - Organizations supporting Type 1 or Type 2 infrastructure indirectly.
  - Example: ISPs, domain registrars, webmail providers

---

## 🔑 Summary / Cheat Sheet

| Feature       | What it means                     | Key Notes / Example |
|---------------|----------------------------------|-------------------|
| **Victim**    | Who / What is attacked           | Persona = employee/org, Assets = systems/IP/email |
| **Capability**| Tools/skills used to attack      | Manual or advanced (malware, phishing), part of adversary arsenal |
| **Infrastructure** | Systems/tools to deliver capability | Type 1 = attacker-owned, Type 2 = intermediary-controlled, includes C2, domains, IPs |

---

## 💡 Quick Analogy
- Victim = target  
- Capability = weapon/tool  
- Infrastructure = how the weapon/tool is delivered or controlled  
- Adversary = the person or group using them

---

## 📝 Practical Tip
- When mapping a cyberattack:
  1. Identify **victim persona** and **assets**.  
  2. List adversary **capabilities** used.  
  3. Identify **infrastructure** (Type 1 & 2) supporting the attack.  
  4. Combine in a **Diamond Model diagram** to see full attack picture.
---
# 💎 Diamond Model — Meta‑features

Meta‑features are *optional* details you can add to a Diamond Model event.  
They give more context and help analysts connect events, find patterns, and explain what happened.

---

## 1️⃣ Timestamp (When)
- **What**: Date & time of the event (example: `2021-09-12 02:10:12.136`).  
- **Why**: Helps group events, find patterns, and guess attacker time zone or working hours.  
- **Tip**: Record start and end times if possible (e.g., `start: 2021-09-12 02:10`, `end: 2021-09-12 02:15`).

**Example**: `2024-07-03 03:05:22 UTC` — many events at 03:00 UTC could mean attacker works daytime in another timezone.

---

## 2️⃣ Phase (Where in the attack lifecycle)
- **What**: The stage of the intrusion (attacks are a sequence of phases).  
- **Common Phases** (like Cyber Kill Chain):  
  1. Reconnaissance  
  2. Weaponization  
  3. Delivery  
  4. Exploitation  
  5. Installation  
  6. Command & Control (C2)  
  7. Actions on Objective

**Example**: A phishing email = `Delivery`; clicking the link & running payload = `Exploitation` → `Installation`.

---

## 3️⃣ Result (Outcome)
- **What**: The outcome or post-condition of the event.  
- **Typical labels**: `Success`, `Failure`, `Unknown`.  
- **Also map to CIA**: e.g., `Confidentiality Compromised`, `Integrity Compromised`, `Availability Compromised`.  
- **Why**: Shows whether attacker achieved goals and what impact happened.

**Example**: `Result: Success — Confidentiality Compromised (credentials exfiltrated)`

---

## 4️⃣ Direction (Flow of activity)
- **What**: Which way the activity moved between diamond nodes.  
- **Seven possible values**:
  - `Victim-to-Infrastructure`  
  - `Infrastructure-to-Victim`  
  - `Infrastructure-to-Infrastructure`  
  - `Adversary-to-Infrastructure`  
  - `Infrastructure-to-Adversary`  
  - `Bidirectional`  
  - `Unknown`

**Example**: Victim uploading data to attacker C2 = `Victim-to-Infrastructure`. C2 sending commands to victim = `Infrastructure-to-Victim`.

---

## 5️⃣ Methodology (Type/class of attack)
- **What**: Short classification of attack type or general technique.  
- **Examples**: `Phishing`, `DDoS`, `Breach`, `Port scan`, `Brute force`, `Supply chain`, `SQL injection`, `Privilege escalation`.  
- **Why**: Quickly groups events by how they were done.

**Example**: `Methodology: Phishing` or `Methodology: SQL injection`.

---

## 6️⃣ Resources (What the attacker needs)
- **What**: External items required for the intrusion to work.  
- **Types of resources**:
  - **Software** — OS, exploitation frameworks (Metasploit), malware.  
  - **Knowledge** — skills to use tools or craft exploits.  
  - **Information** — credentials, insider info, DB schema.  
  - **Hardware** — servers, USB sticks, routers.  
  - **Funds** — money to buy domains, services, or malware.  
  - **Facilities** — power, hosting, physical location.  
  - **Access** — network path, VPN, compromised accounts, ISP link.  

**Example**: `Resources: Stolen credentials + rented VPS + Metasploit module + $50 for domain registration`

---

## ✅ Quick Cheat Sheet

- **Timestamp** = When the event happened.  
- **Phase** = Which step in the attack lifecycle.  
- **Result** = Success / Failure / Unknown (and CIA impact).  
- **Direction** = Which way data/commands moved (7 possible values).  
- **Methodology** = Attack type (phishing, DDoS, etc.).  
- **Resources** = Tools, money, access, or knowledge attacker used.

---

## 🧠 Small Example Event (Putting it together)
- **Adversary**: Unknown group  
- **Victim**: finance@company.com (victim persona)  
- **Capability**: Credential‑harvesting page (phishing)  
- **Infrastructure**: `phish-example[.]com` -> C2 on `1.2.3.4` (Type 1)  
- **Timestamp**: `2025-09-25 09:20:05 UTC`  
- **Phase**: `Delivery` → `Exploitation`  
- **Result**: `Success — Confidentiality Compromised (passwords stolen)`  
- **Direction**: `Victim-to-Infrastructure` (victim submitted credentials to site)  
- **Methodology**: `Phishing`  
- **Resources**: `Registered domain ($12), VPS rental ($5/month), phishing template, stolen email list`

---

# 💎 Diamond Model — Additional Meta-Features (Social-Political & Technology)

---

## 1️⃣ Social-Political Component
- **What**: Explains *why* the adversary is acting — their motives, goals, and intent.  
- **Purpose**: Helps analysts understand the reasoning behind attacks and predict future behavior.  
- **Common motives**:
  - Financial gain 💰
  - Reputation or acceptance in hacker communities 👥
  - Hacktivism (activism via hacking) ✊
  - Espionage (spying for organizations or countries) 🕵️‍♂️

**Example Scenario**:
- Victim: Computers used as zombies in a botnet
- Adversary goal: Crypto-mining profits  
- Interpretation: Adversary exploits victim resources for financial gain

---

## 2️⃣ Technology Component
- **What**: Highlights *how* the adversary operates — the relationship between capability (tools/skills) and infrastructure (systems used).  
- **Purpose**: Helps describe the technical mechanics of the attack.  

**Example Scenario — Watering-Hole Attack**:
- Method: Adversary compromises a legitimate website frequently visited by their target  
- Capability: Malware or exploit used to compromise site  
- Infrastructure: Compromised website + C2 server  
- Victim: Users who visit the site unknowingly get infected

---

## 🧠 Quick Summary
| Meta-Feature | What it describes | Example |
|--------------|-----------------|---------|
| Social-Political | Why the adversary acts, motives | Financial gain via botnet crypto-mining |
| Technology | How the adversary executes attack | Watering-hole attack using compromised website & malware |

---

### 💡 Tip:
When building a Diamond Model for an intrusion:
- **Social-Political** = motive / intent  
- **Technology** = method / technical relationship between capability & infrastructure  
- Combine with the core features (Adversary, Victim, Capability, Infrastructure) and other meta-features (Timestamp, Phase, Result, Direction, Methodology, Resources) for a complete picture.
