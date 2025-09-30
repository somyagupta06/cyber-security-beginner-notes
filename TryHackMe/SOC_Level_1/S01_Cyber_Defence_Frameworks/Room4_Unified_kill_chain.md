# UKC (Unified Kill Chain)  

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

## UKC Phases 
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

## Threat Modelling 
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

## STRIDE, DREAD, CVSS 
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

## Example Commands
nmap -sV target.com  
whois target.com  
curl http://target.com/robots.txt  
ssh user@target.com  

---

## Quick Threat Modelling Checklist 
1. List top 5 assets.  
2. For each, list 3 weak points.  
3. For each weak point, write 1 fix.  
4. Rank fixes (high/medium/low).  
5. Schedule patching/training for high risk.

## Final Note 

UKC is just a map of how an attacker works.  
When we know the map, we can set defence before attacks.  

# Unified Kill Chain (UKC) 

> Source: Paul Pols, *The Unified Kill Chain* (white paper). :contentReference[oaicite:0]{index=0}

---

## Quick intro 
UKC says an attacker’s full attack can be split into **18 clear phases** — from first snooping (recon) to final goals (objectives). This makes defending easier because you can spot and stop stages. :contentReference[oaicite:1]{index=1}

---

## The 18 UKC phases

1. **Reconnaissance** — Attacker studies target (who, what, where). 
2. **Resource Development** — Attacker prepares tools/infrastructure (servers, malware). 
3. **Delivery** — Sending the weapon (phishing email, infected file, supply-chain).  
4. **Social Engineering** — Tricking people to do something unsafe (click link, open file). 
5. **Exploitation** — Using a bug or mistake to run attacker code.
6. **Persistence** — Making sure attacker can come back (backdoor, scheduled task).
7. **Defense Evasion** — Hiding from detection (disable logs, obfuscate malware).
8. **Command & Control (C2)** — Attacker talks to the compromised system remotely.
9. **Pivoting** — Use one hacked machine to reach others (tunnel/forward traffic). 
10. **Discovery** — Attacker learns the network and finds valuable systems. 
11. **Privilege Escalation** — Gaining higher rights (admin/root). 
12. **Execution** — Running attacker controlled code on systems.  
13. **Credential Access** — Stealing passwords, tokens, keys.
14. **Lateral Movement** — Moving sideways to other machines/accounts.  
15. **Collection** — Gathering the data the attacker wants (files, DBs). 
16. **Exfiltration** — Taking data out of the network (uploading out). 
17. **Impact** — Damage to systems or data (encrypt, delete, corrupt). 
18. **Objectives** — The attacker’s overall goal (money, espionage, disruption).

---

## Why UKC is better / useful 
- **More detail:** 18 phases cover more attacker actions than older models.   
- **Whole attack view:** It covers activities outside and inside the network (not just initial break-in). 
- **Iterative reality:** Attackers don’t always go straight; they repeat phases (exploit → recon → pivot, etc.). UKC models that loop.  
- **Maps to defenses:** You can pick which phases happen most and build layered defence (assume-breach + defense-in-depth). 
---

## How UKC compares to Lockheed Martin / MITRE 

| Thing | UKC | Lockheed Martin CKC / MITRE ATT&CK |
|---|---:|---|
| Number of main phases | 18 (detailed). | CKC: ~8 phases (linear); ATT&CK: tactics & techniques (time-agnostic). |
| Scope | Full end-to-end, in and out of network. | CKC more perimeter/malware focused; ATT&CK lists techniques without strict order.  |
| Use case | Threat modelling, layered defence, mapping attacker journey.  | CKC/ATT&CK good for detection rules, telemetry and technique mapping. |

---

## Simple example to show looping 
1. Attacker **exploits** one server → installs backdoor (**persistence**). 
2. From there attacker does **discovery** to find other machines.  
3. They **pivot** to another host, find a new vulnerability, **exploit** again — so phases repeat. 

This loop is why UKC models real attacks better than a strict left-to-right chain. 

---

## Practical quick tips using UKC 
- Focus detection on **high-frequency phases** (discovery, lateral movement, credential access).
- Use UKC to **prioritize controls**: e.g., strong logging + EDR for discovery/execution, MFA + password hygiene for credential access. 
- Map UKC phases to MITRE ATT&CK techniques when building detections — combine both for coverage.

---

## One-minute study plan (if you want to remember)
1. Learn the 3 groups: **In (get in)**, **Through (move inside)**, **Out (take data / damage)**. 
2. Memorize a few key UKC phases: Recon, Delivery, Exploit, Persistence, Discovery, Lateral, Exfil, Objectives. 
3. Practice by mapping a real attack story (phishing → malware → DB steal) to the phases.

---
# UKC — Getting a Foothold 

**Main idea:**  
This group of phases is all about **how the attacker gets into a system** and stays there. They use many tricks — looking for weaknesses, tricking people, running code, making backdoors, hiding from defenses, and then using the infected machine to reach others.

---
<img width="700" height="463" alt="Screenshot 2025-09-29 at 2 10 03 PM" src="https://github.com/user-attachments/assets/69289f09-8475-4f25-82b0-7f3248cd286d" />

## summaries + examples

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

## Small example flow 
1. Attacker finds a public web app (Recon).  
2. Builds a malicious file and C2 server (Weaponization).  
3. Emails an employee a fake invoice (Social Engineering).  
4. Employee opens and the web app executes a reverse shell (Exploitation).  
5. Attacker installs a backdoor service (Persistence).  
6. Attacker hides activity and deletes logs (Defence Evasion).  
7. Attacker connects to C2 and starts stealing files (C2 + Exfiltration).  
8. From that server, attacker accesses internal database server (Pivoting).

---

## Small checklist for defenders
- Lock public info and reduce exposed services.  
- Patch fast and use WAF for web apps.  
- Train users for phishing + enforce 2FA.  
- Use endpoint detection (EDR) and keep logs immutable.  
- Segment the network and use least privilege for accounts.  
- Monitor outbound traffic and DNS for C2 patterns.

---

# UKC — Inside the Network 

**Main idea:**  
After the attacker gets a foothold, they try to **do more inside** — find important systems, get higher access, steal credentials, and move to other machines. This section is all about using one hacked machine as a base to attack the rest of the network.
<img width="689" height="508" alt="Screenshot 2025-09-29 at 2 11 21 PM" src="https://github.com/user-attachments/assets/b9bc34f8-5062-4c1e-a357-488ac13ae937" />

---

## Pivoting (MITRE TA0008) 
**What:** Use the first compromised machine as a bridge/tunnel to reach other internal systems.  
**Why:** Many valuable systems are not open to the internet. The pivot lets attacker reach them.  
**Examples:**  
- Set up SSH/tunnel on the hacked web server and connect to internal DB server.  
- Use the compromised machine to host malware that targets other hosts.  

**One-line:** Turn one hacked machine into a jumping point.

---

## Discovery (MITRE TA0007) 
**What:** Learn everything about the network and system from inside.  
**What they look for:** users, permissions, running apps, open shares, installed software, system configs.  
**Examples:**  
- Run `netstat`, `arp`, or `ipconfig` to see network layout.  
- List users, check admin groups, find shared folders.  
- Search for interesting files or credentials.  

**One-line:** Inside info-gathering to plan the next moves.

---

## Privilege Escalation (MITRE TA0004) 
**What:** Move from low-rights to higher-rights (admin, root, SYSTEM).  
**How:** Use bugs, misconfigured services, weak permissions, stolen tokens.  
**Examples:**  
- Exploit unpatched local service to get SYSTEM.  
- Abuse a writable service config to run code as admin.  

**One-line:** Get more power on the machine so you can do bigger damage.

---

## Execution (MITRE TA0002) e
**What:** Run attacker-controlled code on systems using the pivot host.  
**Examples:**  
- Run remote trojans or scripts.  
- Create scheduled tasks or services that execute payloads.  
- Launch scripts that scan and attack other hosts.  

**One-line:** Run the bad programs from the pivot.

---

## Credential Access (MITRE TA0006) 
**What:** Steal passwords, tokens, keys so attacker can log in like a legit user.  
**Methods:** keyloggers, credential dumping, stealing password files, phishing internal users.  
**Why:** Using valid credentials helps avoid detection (activity looks normal).  

**One-line:** Steal keys to open more doors.

---

## Lateral Movement (MITRE TA0008) 
**What:** Move from one host to another using stolen creds or exploits.  
**Goal:** Reach high-value systems (DB, file servers, domain controllers).  
**Examples:**  
- Use RDP/SMB with stolen admin creds to access a file server.  
- Use SMB relay or pass-the-hash to hop to other machines.  

**One-line:** Jump around the network quietly to reach the real prize.

---

## Small story flow 
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



## Quick plain commands examples 
 
netstat -ano  
ipconfig /all  
whoami /groups  
schtasks /query  
mimikatz # (tool used for credential dumping — only in labs)  
rdesktop internal-db:3389  



## Fast checklist for defenders 
1. Segment network and use jump-hosts for admin access.  
2. Enforce MFA and rotate admin creds often.  
3. Patch OS and apps; remove local admin rights where not needed.  
4. Use EDR + log monitoring to catch unusual discovery/execution.  
5. Block or monitor lateral protocols (RDP, SMB) and alert on abnormal use.

---
# UKC — Endgame: Collection, Exfiltration, Impact, Objectives 

## Main idea 
This phase is the **finish line** — attacker already inside, finds the valuable stuff, takes or breaks it, and completes their goal (money, damage, or fame).

---

## Collection (MITRE TA0009) 
**What:** Attacker gathers the important data they want.  
**Where they look:** Drives, databases, browser data, email, audio/video, shared folders.  
**Why:** To prepare the stuff they will steal or use later.  

**One-line:** Collect all the good stuff quietly.

---

## Exfiltration (MITRE TA0010) 
**What:** Attacker sends the collected data out of the network.  
**How:** They often compress and encrypt the files so security tools don’t notice, and use the C2 tunnel set up earlier.  
**One-line:** Pack it, hide it, and send it out the back door.

---

## Impact (MITRE TA0040) 
**What:** Attacker breaks or changes systems/data to cause damage.  
**Examples:**  
- Encrypt files (ransomware).  
- Wipe disks (delete data).  
- Deface websites.  
- Deny service (DoS) to stop business operations.  

**One-line:** Make things stop working or lose trust.

---

## Objectives
**What:** The attacker’s big reason for the attack.  
**Types of goals:**  
- Financial (ransom, fraud).  
- Espionage (steal secrets).  
- Disruption (hurt business operations).  
- Reputation damage (leak private data).  

**One-line:** Finish the mission — get money, secrets, or chaos.

---

## Short example story 
1. Attacker finds a database with customer records (Collection).  
2. Compresses and encrypts the data and uploads it to their server via the C2 (Exfiltration).  
3. Then runs ransomware to encrypt the rest of the files (Impact).  
4. Finally demands money and/or publishes some data to hurt the company’s image (Objectives).

---

## Quick defender ideas 

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
- check recent big file transfers in webserver logs
```
grep -i "POST" /var/log/apache2/access.log | awk '{print $1, $7, $9, $10}'
```
- list large files in shared folders
```
find /shared -type f -size +100M -ls
```
- check outbound connections
```
netstat -tupan | grep ESTABLISHED
```
---

## Final tiny note 
This last phase is where attackers get their reward or cause real damage. Focus on **detecting big reads, stopping secret data leaving, and having good backups** — that reduces the hit a lot.

