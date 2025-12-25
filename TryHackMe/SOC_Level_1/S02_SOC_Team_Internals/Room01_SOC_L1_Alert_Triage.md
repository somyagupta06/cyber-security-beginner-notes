# SOC Alerts

## Imagine This First 👀
You are sitting next to your SOC mentor.
On the screen, **many alerts are coming and going**.
Some look normal, some look scary.

These alerts are **warnings** that tell us:
👉 “Something happened. Please check.”

---

## What is an Alert? 🚨
An **alert** is a **message created by a security tool** when it sees something **suspicious**.

Example alerts:
- Email Marked as Phishing  
- Unusual Gmail Login Location  
- Unapproved Mimikatz Usage (very dangerous)

Alerts help SOC analysts so they **don’t have to read millions of logs**.

---

## From Event to Alert  🔄

Think like this:

1. **Event happens**
   - User logs in
   - File is downloaded
   - Program is opened

2. **System creates logs**
   - Computer, firewall, email, cloud → all write logs

3. **Logs go to security tools**
   - SIEM or EDR collect logs

4. **Rule checks logs**
   - If something looks strange → ALERT

👉 Without alerts, SOC people would go crazy reading logs all day.

---

## Where Do We See Alerts? (Platforms) 🖥️

| Platform Type | Examples | Simple Meaning |
|--------------|---------|----------------|
| SIEM | :contentReference[oaicite:0]{index=0}, :contentReference[oaicite:1]{index=1} | Main place where alerts are managed |
| EDR / NDR | :contentReference[oaicite:2]{index=2}, :contentReference[oaicite:3]{index=3} | Detect attacks on devices/network |
| SOAR | Splunk SOAR | Helps automate alert handling |
| ITSM | :contentReference[oaicite:4]{index=4}, :contentReference[oaicite:5]{index=5} | Used like tickets |
| Simple Boards | :contentReference[oaicite:6]{index=6} | Can be used for alert tracking |

---

## SOC L1 Role 🧠

SOC L1 = **First checker**

What L1 does:
- Looks at alerts
- Decides: **Real attack or not**
- Sends serious alerts to L2

Other roles:
- **L2** → Deep investigation + fix
- **SOC Engineer** → Makes alert rules
- **SOC Manager** → Checks speed & quality

---

## Alert Status (What is happening with alert?) 📌

| Status | Meaning |
|------|--------|
| New | No one has checked it yet |
| In Progress | Someone is working on it |
| Closed | Work done |

---

## Alert Verdict (Final Decision) ✅❌

| Verdict | Meaning |
|-------|--------|
| True Positive | Real attack |
| False Positive | No danger |

---

## Alert Properties (Must Remember for Exams & Interviews) ⭐

### 1. Alert Time
When alert was created  
(Event happened a little earlier)

Example:
- Event Time: 15:32  
- Alert Time: 15:35  

---

### 2. Alert Name
Short summary of what happened

Examples:
- Unusual Login Location  
- Email Marked as Phishing  

---

### 3. Alert Severity
How dangerous it is

| Color | Meaning |
|-----|--------|
| Green | Low |
| Yellow | Medium |
| Orange | High |
| Red | Critical |

---

### 4. Alert Status
Current stage of alert  
(New, In Progress, Closed)

---

### 5. Alert Verdict
Final answer:
- Attack or Not?

---

### 6. Alert Assignee
Who is responsible for this alert  
(Analyst name)

---

### 7. Alert Description
Explains:
- Why alert was created
- Why it may be dangerous
- How to check it (sometimes)

---

### 8. Alert Fields
Extra details like:
- Computer name
- User name
- Command used

These help analysts understand the alert faster.

---

## One-Line Revision Summary 📝
- **Event happens**
- **Logs are created**
- **Rules check logs**
- **Alert is generated**
- **SOC L1 checks alert**
- **Decide: Real attack or not**

---

## Final Simple Line ❤️
Alerts are **helpers**, not enemies.  
They tell SOC analysts **where to look**,  
so real attacks are **not missed**.

---

# Alert Prioritisation & Triage

## What Happens After Reading an Alert?
You already **read and understood** an alert.
Now the big question is:

👉 **Which alert should I work on first?**

This decision is called **Alert Prioritisation**.

---

## What is Alert Prioritisation? 🚦
Alert prioritisation means **choosing the most important alert first**  
so real attacks are caught **on time**.

SOC teams usually automate this in SIEM tools like :contentReference[oaicite:1]{index=1},  
but L1 analysts must still **think carefully**.

---

## How to Pick the Right Alert 

### 1️⃣ Filter the Alerts
First remove alerts that:
- Are already checked
- Are being worked on by someone else
- Are already closed

👉 Only take **New + Unassigned** alerts.

---

### 2️⃣ Sort by Severity
Always follow this order:

1. 🔴 Critical
2. 🟠 High
3. 🟡 Medium
4. 🟢 Low

Why?
- Critical alerts are **more dangerous**
- They are **more likely real attacks**

---

### 3️⃣ Sort by Time
Check **older alerts first**.

Reason:
- Old attack = attacker already inside
- New attack = attacker just started

👉 Old alert = **more urgent**

---

## Alert Triage = Alert Review 🔍
Alert triage means **checking an alert properly**.

Other names you may hear:
- Alert handling
- Alert investigation
- Alert analysis

(All mean the same thing)

---

## Alert Triage Steps ⭐

### Step 1: Initial Actions
Before deep checking:

- Assign alert to yourself
- Change status to **In Progress**
- Read:
  - Alert name
  - Alert description
  - Important fields (user, host, IP)

This shows **you own the alert**.

---

### Step 2: Investigation (Main Work)
Here you decide: **Attack or Not?**

Check these things:
- Who is affected?  
  (user, computer, server, cloud)
- What happened?  
  (login, malware, phishing)
- What happened before & after?  
  (nearby events in logs)
- Does threat intel say it is bad?

Some teams give **workbooks/playbooks**  
→ step-by-step investigation guides.

---

### Step 3: Final Actions ✅❌
Now make the final decision:

- **True Positive** → real attack
- **False Positive** → no threat

Then:
- Write a clear comment (what you checked + why)
- Change status to **Closed**

---

## SOC Dashboard Notes 🖥️
- If you don’t get a flag → values are wrong
- You can reset dashboard using **Restart** option
- Always double-check severity and time filters

---

## One-Page Revision Summary 📝
- Choose **new alerts only**
- Pick **critical first**
- Check **oldest first**
- Assign → Investigate → Decide → Comment → Close

---

## Final Line ❤️
Good alert prioritisation means:
**Real attacks caught early,  
less damage,  
and a strong SOC team.**


