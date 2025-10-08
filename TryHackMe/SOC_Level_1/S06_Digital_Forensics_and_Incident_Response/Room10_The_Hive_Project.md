# 🐝 TheHive Project

## 🌟 What is TheHive?

TheHive is an open-source (free) Security Incident Response Platform.  
That means it helps people who work in cybersecurity (like in SOCs, CSIRTs, CERTs) to **find, track, and solve** security problems fast and together.

It’s like a big teamwork tool where many security analysts can see and work on the same problem at the same time.

---

## 💬 Why is it Useful?

Imagine a school has many computers. If a virus or hacker attack happens, the security team needs to check what’s wrong and fix it fast.  
TheHive helps them:
- **Collaborate** – work together live
- **Elaborate** – break down the problem into smaller tasks
- **Act** – take quick action to stop or fix the problem

---

## ⚙️ Three Main Functions of TheHive

### 🧑‍🤝‍🧑 1. Collaborate
Many analysts can work together on the same case.  
Everyone can see live updates (like a group project board that updates in real time).

### 📝 2. Elaborate
Each investigation = 1 case.  
A case can have many **tasks**, and you can make them from scratch or templates.  
You can:
- Write notes or progress updates  
- Add evidence files or screenshots  
- Assign tasks to teammates

### 🚀 3. Act
Analysts can:
- Add “observables” (things they found, like IP addresses, files, emails)
- Mark some as **IOCs (Indicators of Compromise)** – basically signs that a hacker is involved  
- Tag and organize everything neatly

---

## 🧩 Important Features of TheHive

### 📁 Case & Task Management
Each investigation = a **case**.  
Inside a case, you can add smaller **tasks** to do step by step.  
You can also attach files, add notes, and make templates for next time.

### 🚨 Alert Triage
You can import alerts from other tools (like SIEM systems or emails).  
Then decide if it should become a case (real problem) or not.

### 🔍 Observable Enrichment (with Cortex)
Cortex is another tool that works with TheHive.  
It helps to analyze indicators (like checking an IP or file hash) and find related patterns.  
So you can understand more about the attack.

### ⚡ Active Response
Analysts can send responses or take direct actions to stop or control a threat.

### 📊 Custom Dashboards
You can make dashboards to see stats like:
- How many cases are open?
- How serious are they?
- How fast are analysts solving them?

### 🧠 MISP Integration
MISP is a system for sharing threat info (IOCs).  
TheHive can connect with MISP to:
- Create new cases from MISP events  
- Import or export indicators  
So everyone shares knowledge and stays safer.

### 🌐 Other Integrations
It also works with:
- **DigitalShadows2TH** and **ZeroFox2TH**  
These help pull alerts from other platforms into TheHive easily.

---

## 👨‍💼 Admin and Users

Admins can create organizations and users with different roles.

### 🧾 User Roles
| Role | What they can do |
|------|------------------|
| admin | Full control of system settings (but not cases) |
| org-admin | Manage users and cases |
| analyst | Create and edit cases and tasks |
| read-only | Can only view cases and tasks |

---

## 🔑 Permissions List

Each role has some powers called *permissions*.  
Here’s what they mean in simple words:

| Permission | What it allows |
|-------------|----------------|
| manageOrganisation | Create or update an organisation |
| manageConfig | Change settings |
| manageProfile | Create or edit user profiles |
| manageTag | Add or remove tags |
| manageCustomField | Make custom fields for cases |
| manageCase | Create, edit, or delete cases |
| manageObservable | Handle observables (like IPs, files, etc.) |
| manageAlert | Manage and import alerts |
| manageUser | Add or remove users |
| manageCaseTemplate | Make templates for future cases |
| manageTask | Create or edit tasks |
| manageShare | Share info with other orgs |
| manageAnalyse | Run analysis tools |
| manageAction | Run active response actions |
| manageAnalyserTemplate | Make analysis templates |

---

## 🧰 Other Things Admin Can Do
- Add **custom fields** (extra info sections)
- Create **custom observable types**
- Import **MITRE ATT&CK TTPs** (tactics and techniques hackers use)

---

## 🖥️ Analyst Dashboard

When an analyst logs in, they see:
- A menu on top to make new cases or see alerts
- A list of all active cases in the center

### ➕ New Case
When creating a case, you fill in:
- **Severity:** How serious is the problem (Low, Medium, High, Critical)
- **TLP (Traffic Light Protocol):** Who can see the info (White = public, Red = secret)
- **PAP (Permissible Actions Protocol):** What actions are allowed with the info

You can also add tasks and choose TTPs (from MITRE ATT&CK) related to the case.  
Example: Exfiltration (stealing data) → Technique T1048.003.

---

## 🧾 Observables

When you add observables (the clues you found), you fill:

| Field | Description | Example |
|--------|--------------|----------|
| Type | What kind of data | IP, Hash, Domain |
| Value | The actual data | 8.8.8.8 |
| TLP | Who can see it | Green, Amber, Red |
| Is IOC | Is it an indicator of compromise? | Yes/No |
| Has been sighted | Has it been seen in your system? | Yes/No |
| Tags | Helpful keywords | Malware IP, MITRE Tactic |
| Description | Small explanation | IP used by Emotet malware |

---

## 🧠 In Simple Words
TheHive is like a **team notebook + task manager + detective board** for cybersecurity experts.  
It helps them **find problems**, **work together**, **take action**, and **share info safely** — all in one place.

---
