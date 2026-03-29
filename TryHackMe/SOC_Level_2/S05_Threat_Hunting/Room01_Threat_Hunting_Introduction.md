# SOC Basics: Threat Hunting 

----------------------------------------

## 1. What is SOC?

SOC = Security Operations Center

It is a team that:
- Monitors systems 24/7
- Detects attacks
- Responds to incidents

Simple:
SOC = "Security team that protects systems"

----------------------------------------

## 2. What is Incident Response (IR)?

Incident Response means:
Handling a detected attack

Steps:
1. Alert comes
2. Analyst checks it
3. Confirms attack
4. Takes action (block, isolate, fix)

Simple:
IR = "Fix the problem after it is found"

----------------------------------------

## 3. What is Threat Hunting?

Threat Hunting means:
Searching for hidden threats without any alert

- No alert needed
- Analyst searches manually

Simple:
Threat Hunting = "Find hidden attackers early"

----------------------------------------

## 4. Difference: IR vs Threat Hunting

| Feature        | Incident Response (IR) | Threat Hunting        |
|---------------|----------------------|----------------------|
| Type          | Reactive             | Proactive            |
| Trigger       | Alert needed         | No alert needed      |
| Goal          | Fix attack           | Find hidden attack   |

Easy way:
- IR = After problem
- Hunting = Before problem

----------------------------------------

## 5. Why Threat Hunting is Needed?

Problems in SOC:
- Not all attacks create alerts
- Some attackers stay hidden

So we hunt to:
- Detect silent attacks
- Improve security
- Reduce damage

----------------------------------------

## 6. What is the Base of Threat Hunting?

We do NOT search randomly ❌

We always start with:
👉 Leads (Clues)

Examples:
- Known malware
- Threat Intelligence
- Past incidents

Simple:
No clue = No hunting

----------------------------------------

## 7. What is Threat Intelligence?

Threat Intelligence = Information about attackers

It helps us understand:
- How attackers work
- What tools they use
- What they target

Example:
If intel says attackers use PowerShell,
→ We check PowerShell logs

----------------------------------------

## 8. Types of Threat Intelligence

### A. Internal Intelligence (Best)

From our own organisation

Examples:
- Past attacks
- Known bad IPs
- Malware seen before

Benefits:
- Very accurate
- Fast detection

----------------------------------------

### B. External Intelligence (Feeds)

From outside sources

Examples:
- MISP
- Recorded Future
- Digital Shadows

Used when:
- We don’t have enough data

----------------------------------------

## 9. What are IOCs?

IOC = Indicator of Compromise

These are signs of attack

Examples:
- IP address
- File hash
- Domain
- Suspicious process

Simple:
IOC = "Proof attacker was here"

----------------------------------------

## 10. Threat Hunting Process (SOC)

Step 1: Get threat intelligence  
Step 2: Create hypothesis  
Step 3: Search logs  
Step 4: Find suspicious activity  
Step 5: Send to IR team  
Step 6: Create detection rule  

----------------------------------------

## 11. Simple Example

Intel says:
Attackers use "wmic.exe"

So we:
- Search logs for wmic
- Find unusual activity

If found:
→ Send to IR

----------------------------------------

## 12. Important Points

- Never hunt randomly
- Always use intelligence
- Internal intel is best
- IOCs are important
- Hunting improves detection

----------------------------------------

## Final Line 

Threat Hunting = Smart searching using clues  
IR = Fixing the problem after detection




--------

# Threat Hunting: What to Hunt & How to Hunt 

----------------------------------------

## 1. What do we hunt for?

This is the most important question.

It decides:
- Direction of hunt
- What data to check
- What logs to analyze

Simple:
We must know what we are looking for

----------------------------------------

## 2. Main Hunting Targets

### A. Known Malware

Meaning:
Malware that is already known

We use:
- Threat Intelligence
- Public reports

What to check:
- File hash
- Process name
- Suspicious files

Example:
If malware "Emotet" is known,
→ Check if it exists in our systems

----------------------------------------

### B. Attack Residues (Signs of Attack)

Meaning:
Small traces left after an attack

Examples:
- Unusual login
- New admin user
- Strange processes
- Weird network activity

Problem:
Looks like normal activity sometimes

Important:
Know your environment well

----------------------------------------

### C. Known Vulnerabilities

Meaning:
Weakness in software or system

Examples:
- Old software version
- Unpatched systems
- CVEs

What to check:
1. Is system vulnerable?
2. Is someone exploiting it?

Benefit:
Can detect attack + fix vulnerability

----------------------------------------

## 3. Important Idea

Attacker needs to be perfect always  
Defender needs to catch only once

So even small clue is important

----------------------------------------

## 4. How do we hunt?

After deciding target,
we convert it into actionable data

----------------------------------------

### A. IOCs (Indicators of Compromise)

Meaning:
Proof of attack

Examples:
- IP address
- File hash
- Domain
- Process name

Use:
Search logs for these values

Example:
IOC = 1.2.3.4  
→ Check if any system connected to it

----------------------------------------

### B. Attack Signatures

Meaning:
Known pattern of attack

Used to:
- Detect specific attacks
- Match known behavior

----------------------------------------

### C. Logical Queries

Meaning:
Smart search in logs

Example:
Find systems with old software

Query idea:
- version < latest version

Result:
List of vulnerable systems

----------------------------------------

### D. Patterns of Activity

Meaning:
Attacker behavior sequence

Example:
1. Login
2. Privilege escalation
3. Data exfiltration

We try to detect this pattern

----------------------------------------

## 5. Important Resource

MITRE ATT&CK

Meaning:
Database of attacker techniques

Use:
- Understand attacker behavior
- Track attack steps

----------------------------------------

## 6. Simple Hunting Flow

Step 1: Decide target (malware / residue / vulnerability)  
Step 2: Convert into IOCs  
Step 3: Run queries  
Step 4: Check patterns  
Step 5: Find suspicious activity  
Step 6: Send to IR  

----------------------------------------

## 7. Key Points (Revision)

- Always start with a target
- Do not hunt randomly
- IOCs are very important
- Queries make hunting easy
- Patterns help detect advanced attacks

----------------------------------------

## Final Line

Threat Hunting =  
Know what to find → convert into clues → search smartly

----
# MITRE ATT&CK Navigator + Hunting Purpose 

----------------------------------------

## 1. What is MITRE ATT&CK Navigator?

It is a tool to:
- Visualize attacker techniques
- Understand attack flow
- Plan threat hunting

Simple:
It shows how attackers behave

----------------------------------------

## 2. What is a Layer?

Layer = Data of one threat (malware/attacker)

Examples:
- WannaCry = 1 layer
- Stuxnet = 1 layer
- Conficker = 1 layer

Each layer shows:
- Techniques used by that threat

----------------------------------------

## 3. Why Combine Layers?

We combine multiple layers to:
- Find common techniques
- Identify high-risk behaviors

----------------------------------------

## 4. Score System

Each threat is given a score

Example:
- WannaCry = 1
- Stuxnet = 2
- Conficker = 4

When combined:
Scores add up

Meaning:
Higher score = more common technique

----------------------------------------

## 5. Color Meaning

- Dark Red → Used by all threats (High Risk)
- Light Color → Used by some threats

SOC Use:
Focus on high-risk (dark red) techniques

----------------------------------------

## 6. What are TTPs?

TTPs = Tactics, Techniques, Procedures

Meaning:
Attacker behavior pattern

Example:
1. Initial access
2. Privilege escalation
3. Data theft

----------------------------------------

## 7. Why use MITRE in Hunting?

- Understand attacker behavior
- Detect patterns
- Make hunting easier

Simple:
Use attacker behavior to find attacker

----------------------------------------

## 8. When to Stop Hunting?

Important:
You may NOT always find anything

Difference:

| CTF               | Threat Hunting       |
|------------------|---------------------|
| Always has answer| May have no result  |

Conclusion:
No result is also normal

----------------------------------------

## 9. Why do we do Threat Hunting?

### A. Find threats early
Catch attacker before damage

### B. Find hidden attacks
Detect what tools missed

### C. Reduce dwell time
Dwell time = time attacker stays inside

Less time = less damage

### D. Improve detection
Convert findings into rules

----------------------------------------

## 10. SOC Feedback Loop

Step 1: Threat Hunting finds suspicious activity  
Step 2: IR investigates  
Step 3: Confirm attack  
Step 4: Create detection rule  
Step 5: Improve monitoring  

----------------------------------------

## 11. Key Points (Revision)

- MITRE shows attacker techniques
- Layers represent threats
- Combine layers to find common patterns
- High score = high importance
- Hunting may not always find results
- Goal is to reduce attacker time

----------------------------------------

## Final Line

MITRE Navigator = Map of attacker behavior  
Threat Hunting = Using that map to find attackers
