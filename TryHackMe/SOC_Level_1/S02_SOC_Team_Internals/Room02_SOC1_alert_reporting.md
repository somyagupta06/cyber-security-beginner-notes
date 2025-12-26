# SOC Alert Flow 

## Big Picture 
Think of a **security team** like a **school discipline system**.

- **L1 analyst** = Class monitor  
- **L2 analyst** = Class teacher  
- **DFIR team** = Principal + special investigation team  

Many small issues come every day, but only **very serious ones go to the top**.

---

## Alert Journey 

Out of **100 alerts**:
- **90 alerts** → False alarm → Closed by **L1**
- **10 alerts** → Real problem → Sent to **L2**
- **1 alert** → Very dangerous → Goes to **DFIR**

So, **triage helps reduce noise and save time**.
<img width="1025" height="463" alt="Screenshot 2025-12-26 at 12 42 02 PM" src="https://github.com/user-attachments/assets/5c2a6185-3291-47b0-aacd-d8c5c91d3275" />

---

## Step 1: Alert Reporting (Writing What You Found)

### What is Alert Reporting?
Before closing or sending an alert, **L1 writes what they checked and what they found**.

It is like:
> “Teacher, this student did this at this time for this reason.”

### Why Reporting is Important?

| Reason | Simple Meaning |
|------|---------------|
| Give context | L2 understands fast |
| Save evidence | Logs may delete, report stays |
| Improve skills | Writing = better thinking |

If you can explain clearly → you really understand the alert.

---

## Step 2: Alert Escalation (Sending to L2)

### What is Escalation?
If the alert is **True Positive and serious**, L1 sends it to **L2**.

L2 does **deep investigation** and **fixes the issue**.

### Why Report Helps Here?
Because L2 does NOT start from zero.
They read your report and continue.

---

## Step 3: Communication (Talking to Other Teams)

Sometimes SOC needs help from others:

- **IT Team** → “Did you give admin access to this user?”
- **HR Team** → “Is this employee new?”
- **Manager** → “This looks risky, please confirm.”

Security is **teamwork**, not solo work.

---

## Why L1 Reports Are VERY Important

| Purpose | Easy Explanation |
|------|------------------|
| Save L2 time | Less repeat work |
| Keep records | Old alerts stay forever |
| Skill growth | Makes you a better analyst |

Never think:
> “I just mark True or False.”

Your report is **your brain on paper**.

---

## Best Report Format – The 5 Ws (Golden Rule)

Always answer these **5 questions** 👇

### WHO
Which user did it?

Example:  
User name: john.doe

---

### WHAT
What exactly happened?

Example:  
Downloaded a suspicious file  
Ran a dangerous command

---

### WHEN
Time details

Example:  
Started at 10:32 AM  
Ended at 10:40 AM

---

### WHERE
From where?

Example:  
Laptop name  
IP address  
Website name

---

### WHY (Most Important)
Why you decided this is **True Positive or False Positive**

Example:  
Because the user is new and never used admin tools before.

---

## One-Line Revision Summary

- **L1** checks alerts and writes reports  
- **False alerts** are closed  
- **True alerts** go to L2  
- **Good reports save time and stop attacks**  
- **5 Ws = Perfect report**
<img width="698" height="325" alt="Screenshot 2025-12-26 at 12 42 30 PM" src="https://github.com/user-attachments/assets/9d49b86e-b5bf-429c-8374-4025b3e98694" />

---

### Final Tip (Exam Style)
If you remember only ONE thing:

> **A SOC alert report is written so that another person can understand the alert without asking you anything.**

Done. Clean. Simple. Revision-ready.
---
# Alert Escalation 

## Simple Idea First
After **L1** finishes checking an alert and writes the report, one big question comes:

❓ **Should I send this alert to L2 or not?**

This decision is called **Escalation**.

---

## When SHOULD You Escalate an Alert to L2?

You should escalate if **ANY ONE** of these is true:

### 1. Big Cyberattack Suspected
If the alert looks like:
- Ransomware
- Data theft
- Account takeover
- Serious malware

👉 L2 or DFIR team is needed.

---

### 2. Fixing Action Is Required
If the alert needs actions like:
- Malware removal  
- Isolating a computer  
- Resetting passwords  

👉 L1 usually cannot do this alone → escalate.

---

### 3. External Communication Needed
If someone outside SOC must be informed:
- Customers
- Company management
- Legal team
- Police or law enforcement

👉 Escalate immediately.

---

### 4. You Are Confused
If you **do not fully understand the alert**:

👉 NEVER guess  
👉 Ask L2 for help  

This is **normal**, especially for beginners.

---

## How to Escalate (Basic Steps)

### Normal Case
1. Write alert report
2. Mark alert as **In Progress**
3. Assign alert to **L2 on shift**
4. Message or inform L2

That’s it.

---

### Some Teams (Strict Process)
Some companies ask for:
- Formal escalation form
- Many mandatory fields

Follow **your SOC rules**.

---

## What Happens After Escalation?

Once L2 gets the alert:
- Reads your report
- Validates if it is really True Positive
- Investigates deeper
- Talks to IT, HR, Legal if needed
- Starts **Incident Response** for serious cases

Your report saves L2 a LOT of time.
<img width="1394" height="403" alt="Screenshot 2025-12-26 at 12 43 17 PM" src="https://github.com/user-attachments/assets/5a8ef064-f4b0-45e5-8d96-990176f6d064" />

---

## Requesting Help from L2 (Very Important)

It is **100% OK** to ask for help.

Especially in early months:
- Asking questions = good analyst
- Closing alerts blindly = dangerous analyst

### Typical Flow
- L1 asks L2 for help
- L2 supports and explains
- Knowledge sharing happens
<img width="1366" height="343" alt="Screenshot 2025-12-26 at 12 43 41 PM" src="https://github.com/user-attachments/assets/c7dce545-32c3-4291-90b5-a97e0ebbc965" />

---

## SOC Dashboard Escalation (Easy Flow)

1. Write report
2. Give verdict
3. Change status to **In Progress**
4. Assign to L2
5. L2 starts from your report

---

## Critical Communication Scenarios (Exam + Real Life)

### Case 1: Urgent Alert, L2 Not Responding
- Waited 30 minutes, no reply

✔ Call L2  
✔ If no answer → Call L3  
✔ If still no answer → Call Manager  

Never stay silent.
<img width="1393" height="401" alt="Screenshot 2025-12-26 at 12 44 10 PM" src="https://github.com/user-attachments/assets/d7ee8966-1e38-4152-a0ff-f477be9fc0c5" />

---

### Case 2: Chat Account Compromised (Slack / Teams)
Alert says user’s chat account is hacked.

❌ Do NOT message user on that chat  
✔ Call the user by phone or other safe method

---

### Case 3: Too Many Alerts at Once
You receive many alerts, some are critical.

✔ Follow prioritisation rules  
✔ Inform L2 that alert volume is high  

Communication matters.

---

### Case 4: You Realise You Made a Mistake
Days later, you think:
> “I may have closed a real attack.”

✔ Inform L2 immediately  
✔ Attacks can stay silent for weeks  

Late is better than never.

---

### Case 5: SIEM Logs Are Broken
Logs are missing or not searchable.

❌ Do NOT ignore alert  
✔ Investigate what you can  
✔ Inform L2 or SOC engineer  

---

## L2 Communication Example

If alert is very serious (example: data leak):
- L2 starts DFIR
- Contacts Legal team
- Contacts PR team
- Handles external communication

L1 should NOT do this alone.

---

## One-Line Revision Summary

- Escalate when alert is serious, unclear, or needs action  
- Asking L2 for help is correct behavior  
- Communication failures can cause big damage  
- Never ignore or hide mistakes  

---

### Final Golden Rule
> **If an alert makes you uncomfortable or confused, escalate it.**
