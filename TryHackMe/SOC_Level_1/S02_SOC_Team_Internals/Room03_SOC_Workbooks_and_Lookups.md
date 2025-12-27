# SOC Notes – Identity, Asset & Network 

## 1. What is this alert story about?
You are working night shift in SOC.  
You see an alert:

- User **G.Baker** logged into a server **HQ-FINFS-02**
- A file **“Financial Report US 2024.xlsx”** was downloaded
- The file was shared with **R.Lund**

Your job is **NOT** to panic.  
Your job is to **check context** and decide:
👉 *Is this normal work or something suspicious?*

---

## 2. Identity Inventory (Who is who?)

### Meaning (Simple):
**Identity Inventory** = a list of all people and accounts in the company.

It tells you:
- Who the user is
- What job they do
- Where they work
- What systems they are allowed to use

### Why SOC needs it?
Because a username alone means nothing.  
You must know **WHO** that user is.

### Example (Easy Table):

| Name | Username | Role | Location | Access |
|----|----|----|----|----|
| Gregory Baker | G.Baker | CFO | UK | Finance, HQ |
| Raymond Lund | R.Lund | US Financial Adviser | USA | Finance |
| svc-veeam-06 | svc-veeam-06 | Backup account | N/A | Backup systems |

### How it helps in our alert:
- **G.Baker is CFO** → Finance files are normal for him
- **R.Lund is US Financial Adviser** → He may need US financial data

So sharing finance files **can be expected**.

---

## 3. Asset Inventory (What is HQ-FINFS-02?)

### Meaning (Simple):
**Asset Inventory** = a list of all company computers and servers.

It tells you:
- What the system is
- Where it is
- Who owns it
- Why it exists

### Example:

| Hostname | Location | OS | Purpose |
|----|----|----|----|
| HQ-FINFS-02 | UK Datacenter | Windows Server | Finance file server |
| HQ-ADDC-01 | UK Datacenter | Windows Server | Active Directory |

### How it helps:
- **HQ-FINFS-02 = Finance File Server**
- CFO accessing it = **normal**
- Random intern accessing it = **danger**

---

## 4. Why context matters (Very Important)

Without inventory:
> “Someone downloaded a finance file 😱”

With inventory:
> “CFO accessed finance server during work need 🙂”

SOC work = **understanding context**, not just alerts.

---

## 5. Network Alert Example (Easy Story)

### Logs say:
1. External IP connects to port **10443**
2. Firewall gives internal IP **10.10.0.53**
3. That IP scans company networks

### Question:
Is this normal or an attack?

---

## 6. Network Diagram (Think like a map 🗺️)
<img width="1096" height="450" alt="Screenshot 2025-12-27 at 11 31 59 AM" src="https://github.com/user-attachments/assets/1fb82bdf-a01e-4b09-b77c-a97fe320d683" />

### Meaning:
A **network diagram** shows:
- Which networks exist
- How they are connected
- What firewall allows

### Example Networks:
- VPN Network → 10.10.0.0/16
- Database Network → 172.16.15.0/24
- Office Network → 172.16.23.0/24

### What happened (Step-by-step):
1. Attacker tried VPN login on port 10443
2. VPN login worked
3. Attacker got VPN IP (10.10.0.53)
4. Attacker scanned Database network
5. Firewall blocked it
6. Attacker scanned Office network next

👉 This looks **malicious**, not normal work.

---

## 7. Final Revision Points (Exam Friendly)

- **Identity Inventory** = Who the user is
- **Asset Inventory** = What the system is
- **Network Diagram** = How networks connect
- SOC does **context checking**, not guessing
- Right user + right system + right time = OK
- Wrong user + sensitive system = Alert 🚨

---

## 8. One-line SOC Rule (Remember this 💡)

> **An alert without context is useless.**
---

# SOC Workbooks (Playbooks) 
## 1. Problem in SOC (Simple)
You see alerts every day.  
Some alerts are easy.  
Some alerts are confusing and long.

If you **skip steps**, you can:
- Miss an attack ❌
- Close a real threat ❌
- Panic on normal activity ❌

So the question is:
👉 **How do we make sure every alert is checked properly, step by step?**

Answer: **SOC Workbooks**

---

## 2. What is a SOC Workbook?
A **SOC Workbook** (also called playbook / runbook / workflow) is:

👉 A **step-by-step guide** that tells analysts:
- What to check
- In which order
- When to close the alert
- When to escalate

Think of it like:
> **A recipe book for SOC analysts**

If you follow the recipe, the result is correct.

---

## 3. Why SOC Workbooks are important
Especially for **L1 analysts (junior)**:

- They cannot remember every attack type
- They may miss important evidence
- They may make decisions too early

So **senior analysts** create workbooks to help them.

Workbooks ensure:
- Same steps every time
- No guessing
- No shortcuts
- Consistent decisions

---

## 4. Example: Unusual Login Location Alert

### Alert says:
> User logged in from a strange country

Is it attack or travel?

You **must not decide immediately**.

---

## 5. Workbook Structure (Very Important)

Most SOC workbooks have **3 main parts**:

---

## 6. Step 1: Enrichment
### Meaning (Easy):
**Add more information to the alert**

### What you check:
- User details (job, location, working hours)
- IP reputation (malicious or clean?)
- Previous login history

### Tools used:
- Identity inventory
- Threat intelligence
- HR systems

👉 Goal: **Understand who and what is involved**

---

## 7. Step 2: Investigation
### Meaning (Easy):
**Check logs and user behaviour**

### What you check:
- Login time (office hours or night?)
- Login location (normal or new?)
- Actions after login (download, scan, access?)

### Tools used:
- SIEM logs
- VPN logs
- Authentication logs

👉 Goal: **Decide if activity is expected or suspicious**

---

## 8. Step 3: Escalation
### Meaning (Easy):
**Decide final action**

### Possible actions:
- Close alert → if everything looks normal ✅
- Ask user → “Was this you?”
- Escalate to L2 → if attack signs are found 🚨

👉 Goal: **Right decision with proof**
<img width="1320" height="683" alt="Screenshot 2025-12-27 at 11 31 20 AM" src="https://github.com/user-attachments/assets/72eb062b-bff7-414c-a420-4239e309221f" />

---

## 9. Why Order Matters
If you:
- Investigate before enrichment ❌
- Escalate without evidence ❌
- Close without logs ❌

You create **bad SOC work**.

Workbook forces:
> **Enrichment → Investigation → Decision**

---

## 10. One-Line Revision Points

- SOC Workbook = step-by-step investigation guide
- Created by senior analysts
- Mainly used by L1 analysts
- Prevents mistakes and missed evidence
- Helps decide: Close or Escalate

---

## 11. Golden SOC Rule (Remember This)
> **Never trust your feeling. Trust the workbook.**


