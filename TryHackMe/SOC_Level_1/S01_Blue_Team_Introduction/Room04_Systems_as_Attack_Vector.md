# System Security 
## 1. Castle Example

Imagine a castle.

- The **gatekeeper** is trained (knows phishing and deepfake).
- But the **lock on the gate is cheap and weak**.

What happens?
The enemy does NOT talk to the gatekeeper.
They **break the lock quietly** and enter.

### Cyber Meaning
- Gatekeeper = User (human)
- Lock = System security
- Enemy = Hacker

👉 If the **system is weak**, user skills do not matter.

---

## 2. What is a System?

A **system** is where data is stored and processed.

Examples:
- Physical server
- Virtual machine
- Cloud platform like **:contentReference[oaicite:1]{index=1}**

### Why systems are important?
- If one email is hacked → 1 person suffers
- If the mail server is hacked → **thousands of emails are hacked**

---

## 3. Value of Different Systems

| Breached System | What Hacker Gets |
|-----------------|------------------|
| Student laptop | Game account + PC added to botnet |
| Bank IT admin laptop | Access to banking systems |
| Law firm mail server | All emails stolen, blackmail |
| Industrial server | Whole network locked (ransomware) |
| Government website panel | Website defacement |

👉 Bigger system = Bigger damage

---

## 4. What Hackers Want

First goal: **Get access to the system**

After that, they may:
- Steal data
- Lock files (ransomware)
- Delete data forever

⚠ Almost every attack starts in a **similar way**.

---

## 5. Human-Led Attacks (People Cause Attacks)

Humans often start attacks by mistake.

Examples:
- Using weak passwords
- Reusing same password everywhere
- Plugging unknown USB
- Downloading pirated software

📌 Fact:
81% of breaches happen because of **stolen or weak passwords**

---

## 6. Vulnerabilities (Software Weakness)

- Every software has bugs
- Hackers use bugs to enter systems

Facts:
- 40,000+ vulnerabilities found in 2024
- 300+ used in real attacks

Extra risk:
- Old systems
- Weak admin passwords
- Open access to internet

Example:
Old Windows servers still exposed online = easy target

---

## 7. Supply Chain Attack (Very Important)

Your computer has:
- Many apps
- Each app uses many libraries

If **one library is hacked**, all users of that app are affected.

This is called **Supply Chain Attack**

### Famous Examples
- **:contentReference[oaicite:2]{index=2}**
- 3CX

Even **:contentReference[oaicite:3]{index=3}** was affected once because of a library issue.

👉 You may be secure, but your software update can attack you.

---

## 8. Why Supply Chain Attacks Are Dangerous

- You do NOT control all software
- Trusted updates can be malicious
- Many companies get hacked together

---

## 9. SOC Analyst Lesson

As a SOC Analyst, you must:
- Expect system breaches
- Detect unusual behavior
- Respond fast
- Assume trusted software can fail

---

## 10. One-Line Revision Summary

- Weak system security = easy attack
- Systems are more valuable than users
- Humans make mistakes
- Software has vulnerabilities
- Supply chain attacks affect many users at once
---

# Software Vulnerabilities & Misconfigurations

## 1. Software Vulnerabilities (Easy Meaning)

Every software has mistakes inside it.
Some mistakes are found quickly.
Some mistakes stay hidden for many years.

Example:
- **:contentReference[oaicite:1]{index=1}**
- It existed since 1992
- Found only in 2014

👉 Hackers may find the mistake before defenders.

---

## 2. Zero-Day Vulnerability

Zero-day means:
- Vulnerability is **not known to the public**
- No patch is available
- Hackers already know it

This is the **most dangerous situation**.

Only SOC monitoring can help:
- Logs
- Alerts
- Unusual behavior detection

---

## 3. CVE (Common Vulnerabilities and Exposures)

When a vulnerability becomes public:
- It gets a **CVE number**
- Everyone can see it

Now a race starts:

| Attackers | Defenders |
|----------|-----------|
| Build exploits | Apply patches |
| Attack fast | Update systems |
| Look for old systems | Secure systems |

---

## 4. Windows Vulnerability Timeline 

Every year, a big Windows vulnerability appears:

- 2017 → EternalBlue  
- 2021 → PrintNightmare  
- 2022 → Follina  
- After that → New critical CVE every year  

👉 Old systems are always in danger.

---

## 5. Responding to Vulnerabilities

Best solution = **Patch (Update)**

If patch is not ready yet (zero-day), do temporary protection:

- Allow access only from trusted IPs
- Follow temporary vendor instructions
- Block attack patterns using firewall or WAF
- Monitor logs very closely

Goal: **Survive until patch is released**

---

## 6. Misconfiguration 

Misconfiguration is NOT a software bug.

It is a **human setup mistake**.

Examples:
- Weak password like "123456"
- Database open to the internet
- Cloud storage set to public

These are done to make work easier, but they create danger.
<img width="1105" height="389" alt="Screenshot 2025-12-23 at 12 42 28 PM" src="https://github.com/user-attachments/assets/dc327854-ec86-4838-92fe-403c61285826" />

---

## 7. Real-World Misconfiguration Examples

- Weak password exposed millions of job applications
- Cloud mistake leaked data of bank customers
- Smart devices used in botnet attacks

Common pattern:
1. Weak password
2. System exposed to internet
3. Hacker finds it
4. Data breached

---

## 8. Responding to Misconfigurations

No patch needed.
Just **fix the setup**.

SOC-related actions:

### Proactive Security
- Penetration Testing (ethical hackers test systems)
- Vulnerability Scans (find weak passwords, old software)
- Configuration Audits (check best practices)

These actions reduce future attacks.

---

## 9. Attackers’ Mindset (Fortress Logic)

Attackers choose:
- The easiest door
- The weakest wall
- The simplest mistake

They do NOT care if it is:
- Human mistake
- System mistake

Both are equal for them.

---

## 10. Mitigation vs Detection

You must do both.

| Threat | Protection |
|------|------------|
| Known vulnerability | Patch |
| Malware | Antivirus |
| Unknown attack | SOC detection |

You cannot train a system.
But you can train IT teams.

---

## 11. Common System Protection Methods

| Mitigation | Meaning |
|----------|--------|
| Patch Management | Regular updates |
| IT Training | Avoid bad setup |
| Network Protection | Limit access |
| Antivirus | Detect & block attacks |

<img width="1272" height="185" alt="Screenshot 2025-12-23 at 12 43 07 PM" src="https://github.com/user-attachments/assets/c1ccc529-f651-420c-8a2b-4edde198573a" />

---

## 13. One-Line Revision Summary

- Vulnerabilities are software bugs
- Zero-days are most dangerous
- CVEs start attacker vs defender race
- Misconfigurations are human mistakes
- Patch + Proper setup = Strong security
