# UKC (Unified Kill Chain) — Simple Easy English  

## What is a Kill Chain?
- **Kill Chain** = the attacker’s step-by-step plan.  
- Like a thief: first watching a house, then finding the key, then entering and stealing.  
- In cyber world, it’s the same idea.

---

## Why UKC is Useful
- With UKC we can **understand what an attacker will do**.  
- When we know these steps, we can build defence before the attack happens.  
- UKC also works together with other frameworks like MITRE ATT&CK.

---

## UKC Phases (Easy Version)
1. **Reconnaissance (Checking target)**  
   - Attacker collects info about the target.  
   - Example: website details, employee emails, open ports.

2. **Weaponization (Making the attack tool)**  
   - Attacker creates an attack tool or exploit (malicious file, exploit script).

3. **Delivery (Sending attack)**  
   - Attacker delivers attack: phishing email, malicious link, infected USB.

4. **Exploitation (Using vulnerability)**  
   - Attacker exploits a weakness to run code or gain access.

5. **Installation (Setting up)**  
   - Attacker installs malware or backdoor for future access.

6. **Command & Control (C2) (Remote control)**  
   - Attacker controls the machine remotely.

7. **Actions on Objectives (Final goal)**  
   - Stealing data, destroying systems, asking ransom, etc.

---

## Example Story
- **Step 1 (Recon):** Attacker looks on LinkedIn for company emails.  
- **Step 2 (Weaponize):** Creates fake PDF with malware.  
- **Step 3 (Deliver):** Sends email “Invoice attached”.  
- **Step 4 (Exploit):** Employee opens PDF, malware runs.  
- **Step 5 (Install):** Backdoor installed.  
- **Step 6 (C2):** Attacker connects and sends commands.  
- **Step 7 (Objective):** Attacker copies customer data.

---

## Threat Modelling — Super Simple
Threat modelling = looking at your system and planning:
1. **What is important? (Assets)**  
   - Example: customer database, payment system, internal emails.
2. **What are the weak points? (Vulnerabilities)**  
   - Example: old software, open ports, weak passwords.
3. **Who can attack? (Threats)**  
   - Example: script kiddie, insider, APT group.
4. **Make a plan to fix (Mitigation)**  
   - Example: patching, 2FA, backups, employee training.
5. **Add policies**  
   - Example: SDLC, regular scans, phishing awareness.

---

## STRIDE, DREAD, CVSS — Quick Intro
- **STRIDE** — categories of threats (good for design):
  - S = Spoofing (fake identity)
  - T = Tampering (change data)
  - R = Repudiation (deny actions)
  - I = Information disclosure (data leak)
  - D = Denial of Service (stop service)
  - E = Elevation of privilege (higher access)

- **DREAD** — old way to give risk a score:
  - D = Damage  
  - R = Reproducibility  
  - E = Exploitability  
  - A = Affected users  
  - D = Discoverability  

- **CVSS** — standard vulnerability score (0–10):
  - Higher = more critical → fix first.

---

## Comparison — When to Use
| Feature | UKC | MITRE ATT&CK |
|---|---:|---|
| Focus | Whole attack flow | Specific attacker techniques |
| Use | High-level strategy, threat modelling | Mapping alerts, building detections |
| Best for | Understanding attacker journey | Detailed detection work |

---

## Quick Practical Tips
- Keep software updated (patch regularly).  
- Make backups.  
- Train staff about phishing.  
- Least privilege (give only needed access).  
- Monitor logs for weird activity.

---

## Example Commands (just plain text)
nmap -sV target.com  
whois target.com  
curl http://target.com/robots.txt  
ssh user@target.com  

---

## Quick Threat Modelling Checklist (5 minutes)
1. List top 5 assets.  
2. For each, list 3 weak points.  
3. For each weak point, write 1 fix.  
4. Rank fixes (high/medium/low).  
5. Schedule patching/training for high risk.

---

## Final Note (very simple)
UKC is just a map of how an attacker works.  
When we know the map, we can set defence before attacks.  
---
# Unified Kill Chain (UKC) — Simple Easy English (continued)

> Source: Paul Pols, *The Unified Kill Chain* (white paper). :contentReference[oaicite:0]{index=0}

---

## Quick intro (one line)
UKC says an attacker’s full attack can be split into **18 clear phases** — from first snooping (recon) to final goals (objectives). This makes defending easier because you can spot and stop stages. :contentReference[oaicite:1]{index=1}

---

## The 18 UKC phases — very short easy meaning (one-liners)

1. **Reconnaissance** — Attacker studies target (who, what, where). :contentReference[oaicite:2]{index=2}  
2. **Resource Development** — Attacker prepares tools/infrastructure (servers, malware). :contentReference[oaicite:3]{index=3}  
3. **Delivery** — Sending the weapon (phishing email, infected file, supply-chain). :contentReference[oaicite:4]{index=4}  
4. **Social Engineering** — Tricking people to do something unsafe (click link, open file). :contentReference[oaicite:5]{index=5}  
5. **Exploitation** — Using a bug or mistake to run attacker code. :contentReference[oaicite:6]{index=6}  
6. **Persistence** — Making sure attacker can come back (backdoor, scheduled task). :contentReference[oaicite:7]{index=7}  
7. **Defense Evasion** — Hiding from detection (disable logs, obfuscate malware). :contentReference[oaicite:8]{index=8}  
8. **Command & Control (C2)** — Attacker talks to the compromised system remotely. :contentReference[oaicite:9]{index=9}  
9. **Pivoting** — Use one hacked machine to reach others (tunnel/forward traffic). :contentReference[oaicite:10]{index=10}  
10. **Discovery** — Attacker learns the network and finds valuable systems. :contentReference[oaicite:11]{index=11}  
11. **Privilege Escalation** — Gaining higher rights (admin/root). :contentReference[oaicite:12]{index=12}  
12. **Execution** — Running attacker controlled code on systems. :contentReference[oaicite:13]{index=13}  
13. **Credential Access** — Stealing passwords, tokens, keys. :contentReference[oaicite:14]{index=14}  
14. **Lateral Movement** — Moving sideways to other machines/accounts. :contentReference[oaicite:15]{index=15}  
15. **Collection** — Gathering the data the attacker wants (files, DBs). :contentReference[oaicite:16]{index=16}  
16. **Exfiltration** — Taking data out of the network (uploading out). :contentReference[oaicite:17]{index=17}  
17. **Impact** — Damage to systems or data (encrypt, delete, corrupt). :contentReference[oaicite:18]{index=18}  
18. **Objectives** — The attacker’s overall goal (money, espionage, disruption). :contentReference[oaicite:19]{index=19}

---

## Why UKC is better / useful (short)
- **More detail:** 18 phases cover more attacker actions than older models. :contentReference[oaicite:20]{index=20}  
- **Whole attack view:** It covers activities outside and inside the network (not just initial break-in). :contentReference[oaicite:21]{index=21}  
- **Iterative reality:** Attackers don’t always go straight; they repeat phases (exploit → recon → pivot, etc.). UKC models that loop. :contentReference[oaicite:22]{index=22}  
- **Maps to defenses:** You can pick which phases happen most and build layered defence (assume-breach + defense-in-depth). :contentReference[oaicite:23]{index=23}

---

## How UKC compares to Lockheed Martin / MITRE (simple table)

| Thing | UKC | Lockheed Martin CKC / MITRE ATT&CK |
|---|---:|---|
| Number of main phases | 18 (detailed). :contentReference[oaicite:24]{index=24} | CKC: ~8 phases (linear); ATT&CK: tactics & techniques (time-agnostic). :contentReference[oaicite:25]{index=25} |
| Scope | Full end-to-end, in and out of network. :contentReference[oaicite:26]{index=26} | CKC more perimeter/malware focused; ATT&CK lists techniques without strict order. :contentReference[oaicite:27]{index=27} |
| Use case | Threat modelling, layered defence, mapping attacker journey. :contentReference[oaicite:28]{index=28} | CKC/ATT&CK good for detection rules, telemetry and technique mapping. :contentReference[oaicite:29]{index=29} |

---

## Simple example to show looping (easy)
1. Attacker **exploits** one server → installs backdoor (**persistence**). :contentReference[oaicite:30]{index=30}  
2. From there attacker does **discovery** to find other machines. :contentReference[oaicite:31]{index=31}  
3. They **pivot** to another host, find a new vulnerability, **exploit** again — so phases repeat. :contentReference[oaicite:32]{index=32}

This loop is why UKC models real attacks better than a strict left-to-right chain. :contentReference[oaicite:33]{index=33}

---

## Practical quick tips using UKC (one-liners)
- Focus detection on **high-frequency phases** (discovery, lateral movement, credential access). :contentReference[oaicite:34]{index=34}  
- Use UKC to **prioritize controls**: e.g., strong logging + EDR for discovery/execution, MFA + password hygiene for credential access. :contentReference[oaicite:35]{index=35}  
- Map UKC phases to MITRE ATT&CK techniques when building detections — combine both for coverage. :contentReference[oaicite:36]{index=36}

---

## One-minute study plan (if you want to remember)
1. Learn the 3 groups: **In (get in)**, **Through (move inside)**, **Out (take data / damage)**. :contentReference[oaicite:37]{index=37}  
2. Memorize a few key UKC phases: Recon, Delivery, Exploit, Persistence, Discovery, Lateral, Exfil, Objectives. :contentReference[oaicite:38]{index=38}  
3. Practice by mapping a real attack story (phishing → malware → DB steal) to the phases.

---
# UKC — Easy English (continued): Getting a Foothold (simple)

**Main idea:**  
This group of phases is all about **how the attacker gets into a system** and stays there. They use many tricks — looking for weaknesses, tricking people, running code, making backdoors, hiding from defenses, and then using the infected machine to reach others.

---
<img width="700" height="463" alt="Screenshot 2025-09-29 at 2 10 03 PM" src="https://github.com/user-attachments/assets/69289f09-8475-4f25-82b0-7f3248cd286d" />

## Short plain summaries + examples

### Reconnaissance (MITRE TA0043)  
**What:** Attacker collects info about the target.  
**Why:** To find useful weak spots for attack later.  
**Examples:**  
- Scan which services run on a server.  
- Find employee emails or public phone numbers.  
- Find network layout to see which machines can be reached later.  

**One-line:** Look first, plan later.

---

### Weaponization (MITRE TA0001)  
**What:** Attacker prepares tools and infrastructure.  
**Why:** To be ready to launch and manage the attack.  
**Examples:**  
- Make a malicious file or exploit script.  
- Setup a server to receive reverse shells or store stolen data.  

**One-line:** Prepare the weapon and the control room.

---

### Social Engineering (MITRE TA0001)  
**What:** Trick people to do something that helps the attack.  
**Examples:**  
- Send a fake email with a malicious attachment.  
- Make a fake login page to steal credentials.  
- Call and pretend to be a technician to get a password reset.  

**One-line:** Attack people, not just machines.

---

### Exploitation (MITRE TA0002)  
**What:** Use a vulnerability to run attacker code or get access.  
**Examples:**  
- Upload a reverse shell to a web app.  
- Trigger a bug in an application to run commands.  

**One-line:** Use the weak spot to get in.

---

### Persistence (MITRE TA0003)  
**What:** Make sure attacker can come back later.  
**Examples:**  
- Add a startup service that runs malware.  
- Create a user account with backdoor access.  
- Configure scheduled tasks to reopen a shell.  

**One-line:** Lock in access so you don’t lose it.

---

### Defence Evasion (MITRE TA0005)  
**What:** Hide from security tools and admins.  
**Examples:**  
- Disable logging or change timestamps.  
- Obfuscate malware so antivirus misses it.  
- Use valid admin tools so activity looks normal.  

**One-line:** Stay invisible.

---

### Command & Control (C2) (MITRE TA0011)  
**What:** Attacker talks to the compromised machine from outside.  
**Why:** To run commands, move data, and control the attack.  
**Examples:**  
- Connect back to attacker’s server to receive instructions.  
- Use an encrypted channel to exfiltrate files.  

**One-line:** Remote control channel between attacker and target.

---

### Pivoting (MITRE TA0008)  
**What:** Use one compromised host to reach others inside the network.  
**Why:** Many valuable systems are not directly reachable from the internet.  
**Examples:**  
- From a public web server, scan and connect to internal database servers.  
- Use stolen credentials to access internal file servers.  

**One-line:** Use the first victim as a bridge to more valuable targets.

---

## Quick table — Phase → Simple defence ideas

| Phase | Simple Defence Actions |
|---|---|
| Reconnaissance | Limit public info, monitor scans, hide unnecessary services |
| Weaponization | Block known-malware domains, filter attachments |
| Social Engineering | Phishing training, email filtering, 2FA |
| Exploitation | Keep software patched, WAF, secure coding |
| Persistence | Monitor new services/accounts, integrity checks |
| Defence Evasion | EDR with behavior rules, immutable logs |
| Command & Control | Block suspicious outbound traffic, DNS monitoring |
| Pivoting | Network segmentation, least-privilege, strong internal auth |

---

## Small example flow (one short story)
1. Attacker finds a public web app (Recon).  
2. Builds a malicious file and C2 server (Weaponization).  
3. Emails an employee a fake invoice (Social Engineering).  
4. Employee opens and the web app executes a reverse shell (Exploitation).  
5. Attacker installs a backdoor service (Persistence).  
6. Attacker hides activity and deletes logs (Defence Evasion).  
7. Attacker connects to C2 and starts stealing files (C2 + Exfiltration).  
8. From that server, attacker accesses internal database server (Pivoting).

---

## Small checklist for defenders (fast)
- Lock public info and reduce exposed services.  
- Patch fast and use WAF for web apps.  
- Train users for phishing + enforce 2FA.  
- Use endpoint detection (EDR) and keep logs immutable.  
- Segment the network and use least privilege for accounts.  
- Monitor outbound traffic and DNS for C2 patterns.

---

# UKC — Inside the Network (very easy English)

**Main idea:**  
After the attacker gets a foothold, they try to **do more inside** — find important systems, get higher access, steal credentials, and move to other machines. This section is all about using one hacked machine as a base to attack the rest of the network.
<img width="689" height="508" alt="Screenshot 2025-09-29 at 2 11 21 PM" src="https://github.com/user-attachments/assets/b9bc34f8-5062-4c1e-a357-488ac13ae937" />

---

## Pivoting (MITRE TA0008) — simple
**What:** Use the first compromised machine as a bridge/tunnel to reach other internal systems.  
**Why:** Many valuable systems are not open to the internet. The pivot lets attacker reach them.  
**Examples:**  
- Set up SSH/tunnel on the hacked web server and connect to internal DB server.  
- Use the compromised machine to host malware that targets other hosts.  

**One-line:** Turn one hacked machine into a jumping point.

---

## Discovery (MITRE TA0007) — simple
**What:** Learn everything about the network and system from inside.  
**What they look for:** users, permissions, running apps, open shares, installed software, system configs.  
**Examples:**  
- Run `netstat`, `arp`, or `ipconfig` to see network layout.  
- List users, check admin groups, find shared folders.  
- Search for interesting files or credentials.  

**One-line:** Inside info-gathering to plan the next moves.

---

## Privilege Escalation (MITRE TA0004) — simple
**What:** Move from low-rights to higher-rights (admin, root, SYSTEM).  
**How:** Use bugs, misconfigured services, weak permissions, stolen tokens.  
**Examples:**  
- Exploit unpatched local service to get SYSTEM.  
- Abuse a writable service config to run code as admin.  

**One-line:** Get more power on the machine so you can do bigger damage.

---

## Execution (MITRE TA0002) — simple
**What:** Run attacker-controlled code on systems using the pivot host.  
**Examples:**  
- Run remote trojans or scripts.  
- Create scheduled tasks or services that execute payloads.  
- Launch scripts that scan and attack other hosts.  

**One-line:** Run the bad programs from the pivot.

---

## Credential Access (MITRE TA0006) — simple
**What:** Steal passwords, tokens, keys so attacker can log in like a legit user.  
**Methods:** keyloggers, credential dumping, stealing password files, phishing internal users.  
**Why:** Using valid credentials helps avoid detection (activity looks normal).  

**One-line:** Steal keys to open more doors.

---

## Lateral Movement (MITRE TA0008) — simple
**What:** Move from one host to another using stolen creds or exploits.  
**Goal:** Reach high-value systems (DB, file servers, domain controllers).  
**Examples:**  
- Use RDP/SMB with stolen admin creds to access a file server.  
- Use SMB relay or pass-the-hash to hop to other machines.  

**One-line:** Jump around the network quietly to reach the real prize.

---

## Small story flow (short)
1. Attacker uses web server as pivot (Pivoting).  
2. Runs discovery to find internal DB (Discovery).  
3. Finds a service with weak config and escalates to root (Privilege Escalation).  
4. Installs a scheduled task that runs a data-stealing script (Execution).  
5. Dumps password hashes from memory (Credential Access).  
6. Uses those hashes to RDP into the DB server (Lateral Movement).

---

## Simple defender actions — keep it small & practical

| Phase | Easy Defences |
|---|---|
| Pivoting | Network segmentation, limit direct host-to-host traffic |
| Discovery | Monitor suspicious internal scans, detect unusual commands |
| Privilege Escalation | Patch quickly, remove local admin rights, harden services |
| Execution | Restrict script execution, application allowlisting, EDR |
| Credential Access | MFA, limit credential storage, rotate admin passwords |
| Lateral Movement | Disable unnecessary protocols (SMB/RDP), jump hosts, strong logging |

---

## Quick plain commands examples (GitHub-friendly single lines)
# (examples — need permission in real life)
netstat -ano  
ipconfig /all  
whoami /groups  
schtasks /query  
mimikatz # (tool used for credential dumping — only in labs)  
rdesktop internal-db:3389  

---

## Fast checklist for defenders (5 items)
1. Segment network and use jump-hosts for admin access.  
2. Enforce MFA and rotate admin creds often.  
3. Patch OS and apps; remove local admin rights where not needed.  
4. Use EDR + log monitoring to catch unusual discovery/execution.  
5. Block or monitor lateral protocols (RDP, SMB) and alert on abnormal use.

---
# UKC — Endgame: Collection, Exfiltration, Impact, Objectives (Very Easy English)

## Main idea (one line)
This phase is the **finish line** — attacker already inside, finds the valuable stuff, takes or breaks it, and completes their goal (money, damage, or fame).

---

## Collection (MITRE TA0009) — simple
**What:** Attacker gathers the important data they want.  
**Where they look:** Drives, databases, browser data, email, audio/video, shared folders.  
**Why:** To prepare the stuff they will steal or use later.  

**One-line:** Collect all the good stuff quietly.

---

## Exfiltration (MITRE TA0010) — simple
**What:** Attacker sends the collected data out of the network.  
**How:** They often compress and encrypt the files so security tools don’t notice, and use the C2 tunnel set up earlier.  
**One-line:** Pack it, hide it, and send it out the back door.

---

## Impact (MITRE TA0040) — simple
**What:** Attacker breaks or changes systems/data to cause damage.  
**Examples:**  
- Encrypt files (ransomware).  
- Wipe disks (delete data).  
- Deface websites.  
- Deny service (DoS) to stop business operations.  

**One-line:** Make things stop working or lose trust.

---

## Objectives — simple
**What:** The attacker’s big reason for the attack.  
**Types of goals:**  
- Financial (ransom, fraud).  
- Espionage (steal secrets).  
- Disruption (hurt business operations).  
- Reputation damage (leak private data).  

**One-line:** Finish the mission — get money, secrets, or chaos.

---

## Short example story (easy)
1. Attacker finds a database with customer records (Collection).  
2. Compresses and encrypts the data and uploads it to their server via the C2 (Exfiltration).  
3. Then runs ransomware to encrypt the rest of the files (Impact).  
4. Finally demands money and/or publishes some data to hurt the company’s image (Objectives).

---

## Quick defender ideas (small and practical)

| Phase | Easy Defences |
|---|---|
| Collection | Monitor access to sensitive files, alert on large read/collection activity |
| Exfiltration | Monitor unusual outbound traffic, block suspicious destinations, use DLP |
| Impact | Use immutable backups, offline backups, tested restore plan |
| Objectives | Have incident response + legal + PR plan ready |

---

## Fast checklist for defenders (5 actions)
1. **Backups:** Keep offline immutable backups and test restores.  
2. **Data Loss Prevention (DLP):** Use DLP rules to detect large or suspicious exports.  
3. **Network Monitoring:** Watch for encrypted or unusual outbound traffic, strange ports, or volume spikes.  
4. **Limit Access:** Only give access to data on a need-to-know basis and log all access.  
5. **Incident Plan:** Have a tested incident response plan (include legal & PR) and rehearsal drills.

---

## Small commands / checks (plain lines — GitHub friendly)
# check recent big file transfers in webserver logs
grep -i "POST" /var/log/apache2/access.log | awk '{print $1, $7, $9, $10}'  
# list large files in shared folders
find /shared -type f -size +100M -ls  
# check outbound connections
netstat -tupan | grep ESTABLISHED

---

## Final tiny note (very easy)
This last phase is where attackers get their reward or cause real damage. Focus on **detecting big reads, stopping secret data leaving, and having good backups** — that reduces the hit a lot.

