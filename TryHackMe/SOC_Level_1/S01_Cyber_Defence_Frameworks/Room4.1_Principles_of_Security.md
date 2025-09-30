# 🛡️ Defence in Depth & CIA Triad 

---

## 1️⃣ Defence in Depth (Layered Security)

- **Meaning**: Use many different layers of security to protect data & systems.  
- **Goal**: If one layer fails, other layers still protect the data. (Like wearing helmet + seatbelt + airbag for safety.)  
- **Example**:  
  - Password + Two-Factor Authentication (2FA) + Firewall + Antivirus  

---

## 2️⃣ CIA Triad (Confidentiality, Integrity, Availability)

- **Purpose**: Model to check if data is truly secure.  
- **Idea**: If even one element is missing, security is weak.  
- **Analogy**: Like a fire triangle (fuel, heat, oxygen) – if one is missing, fire stops.  
- **Applies to**: Both digital (cybersecurity) & non-digital (paper files, records).  

---

| Element           | Meaning (Easy) | Example in Real Life | How to Achieve It |
|-------------------|----------------|----------------------|------------------|
| **Confidentiality** 🔒 | Keep data secret from people who should not see it. | HR files only seen by HR staff; government data marked “Top Secret.” | Passwords, Access Control, Encryption, Classification levels |
| **Integrity** ✅ | Data stays correct & unchanged (unless allowed). | Bank records must not be changed by hackers. | Hash checks, Digital signatures, Strict permissions, Authentication |
| **Availability** 📂 | Data must be reachable when needed. | Website with 99.99% uptime; ATM works 24/7. | Backup servers, Redundant systems, DDoS protection, Reliable hardware |

---

### 🔒 Confidentiality (Data Secret)

- Protect sensitive data from people who shouldn’t access it.  
- Example: Employee records (HR only), Accounting data (less sensitive).  
- Government example: Classified, Secret, Unclassified labels.  

**Techniques**:  
- Access control (only correct people get in).  
- Vetting staff (background checks).  
- Encryption of sensitive files.  

---

### ✅ Integrity (Data Correct)

- Keep data accurate & consistent.  
- Unauthorized changes or errors break integrity.  
- Must stay unchanged during storage, transmission, and use.

**Techniques**:  
- Strong authentication (only correct people can change).  
- Hash verification (detect if file changed).  
- Digital signatures (prove authenticity).  

---

### 📂 Availability (Data Accessible)

- Data must be available for authorized users at the right time.  
- If system is down → reputation loss + money loss.  
- Service Level Agreements (SLA) often state % uptime like 99.99%.  

**Techniques**:  
- Use reliable & tested hardware.  
- Have backups / redundant systems.  
- Security protocols to block attacks (like DDoS).  

---

## 🔄 Summary Table

| Concept | Simple Meaning | Quick Analogy |
|---------|---------------|---------------|
| **Defence in Depth** | Many layers of security | Helmet + Seatbelt + Airbag |
| **Confidentiality** | Keep data secret | Lock on your diary |
| **Integrity** | Keep data correct | Seal on medicine bottle (shows tampering) |
| **Availability** | Keep data accessible | ATM open 24/7 |

---
# 🔑 Access Control & Privilege Management  

---

## 1️⃣ Why Access Levels Matter  
- People in an organisation need **different levels of access** to systems.  
- **Decided by two factors**:  
  1. **Their role/function** in the organisation (HR, IT, Finance etc.)  
  2. **Sensitivity of the data** they are trying to access (Top Secret vs Normal data).  

---

## 2️⃣ Two Key Concepts  

| Concept | Meaning (Easy) | Purpose | Example |
|---------|---------------|---------|---------|
| **PIM (Privileged Identity Management)** | Turns a person’s job role into a system role. (Maps *who they are* to *what they can access*.) | Assign access roles based on job. | HR employee gets HR system role; IT admin gets admin system role. |
| **PAM (Privileged Access Management)** | Controls and manages what the system role can do and how. (Enforces security rules.) | Secure and monitor privileged access. | Set password policies, log actions, limit dangerous admin powers. |

---

### 🔹 How PIM & PAM Work Together  

- **PIM** = “Who gets what role?” (role assignment)  
- **PAM** = “What can that role do?” + security checks (role management)  

Think of it like this:  
- **PIM** is the HR department giving you a job title badge.  
- **PAM** is the security office deciding which doors your badge actually opens, logging your entry, and forcing you to change your PIN.  

---

## 3️⃣ Principle of Least Privilege (PoLP)

- **Meaning**: Give users **only the access they need** to do their job, nothing more.  
- **Why**: Limits damage if account is hacked & reduces mistakes.  
- **Example**:  
  - A cashier can view sales but not change product prices.  
  - An intern can read documents but not delete them.  

---

## 4️⃣ What PAM Also Covers  

Beyond assigning access, **PAM** also:  
- Enforces **password management** (strong passwords, rotations).  
- Creates **audit logs** (who accessed what and when).  
- Reduces **attack surface** (limit risky admin accounts).  

---

## 🔄 Quick Table  

| Term | Simple Meaning | Analogy |
|------|---------------|---------|
| **PIM** | Map job role → system role | HR says “You’re a teacher” |
| **PAM** | Control powers of system role + enforce policies | School says “You can open classroom but not principal’s office” |
| **Least Privilege** | Minimum permissions necessary | Give a house key only for the rooms someone needs |

---
# 📝 Security Models  

We already know the **CIA Triad**:
- **C**onfidentiality (keep data secret)
- **I**ntegrity (keep data correct)
- **A**vailability (keep data usable)

Now let’s see **formal models** that help achieve these goals.  
In these models:
- **Subject = User/Person**  
- **Object = Data/File/Resource**  

---

## 🔒 1. Bell–LaPadula Model (for Confidentiality)

- **Goal**: Keep data secret (Confidentiality).  
- **Assumption**: Hierarchy is clear (like government or military).  

### 📜 Rule:  
- **No Read Up** = A user cannot read data above their level.  
- **No Write Down** = A user cannot write data to a lower level.  

Think like this:
- **User Level = Confidential**  
- **Data Level = Secret**  
- User cannot read Secret data (no read up).  
- User cannot write their Confidential info into a Public file (no write down).

---

### ✅ Advantages:
- Matches real-life hierarchies (like military ranks).  
- Simple to understand and use.  

### ❌ Disadvantages:
- User may know something exists even if they can’t access it (not fully secret).  
- Relies on trusting people already vetted.  

### 🏛 Example:
- Government / Military – People are vetted, then given data access based on rank.
<img width="471" height="368" alt="Screenshot 2025-09-30 at 5 52 19 PM" src="https://github.com/user-attachments/assets/80389bd0-7f6e-4dac-bcb0-98afd21d56a1" />

---

## ✅ 2. Biba Model (for Integrity)

- **Goal**: Keep data accurate (Integrity).  
- Works opposite to Bell-LaPadula.

### 📜 Rule:
- **No Write Up** = A user cannot write to a higher level.  
- **No Read Down** = A user cannot read data from a lower level.  

Think like this:
- **Doctor** (high level) cannot write notes into a higher security file they don’t control.  
- **Doctor** also cannot read “lower” notes if rules forbid it.  

---

### ✅ Advantages:
- Simple to implement.  
- Fixes Bell-LaPadula’s limitation by focusing on integrity.  

### ❌ Disadvantages:
- Many levels & objects → easy to make mistakes.  
- Can slow down work (doctor can’t read nurse’s notes).  

### 🏢 Example:
- Software Development: Developers can only work on the code they need (integrity first).  
- Financial data systems where data accuracy is more important than secrecy.
<img width="458" height="377" alt="Screenshot 2025-09-30 at 5 52 33 PM" src="https://github.com/user-attachments/assets/98e71526-f480-47b3-9dd6-25e56ba23523" />

---

## 🔄 Quick Comparison Table  

| Model | CIA Focus | Rule | When Used | Example |
|-------|-----------|------|-----------|---------|
| **Bell-LaPadula** | Confidentiality | No Read Up, No Write Down | Military, Government | Classified Documents |
| **Biba** | Integrity | No Write Up, No Read Down | Software Dev, Finance | Protecting Code/Data Accuracy |

---

## 🧠 Easy Analogy  

| Model | Analogy |
|-------|---------|
| **Bell-LaPadula** | Like a library with restricted books: You can’t read books above your clearance, and you can’t put your notes into lower-class sections. |
| **Biba** | Like a clean kitchen: High-level chefs can’t take food from the trash (no read down) and can’t put unapproved ingredients into premium dishes (no write up). |

---
# 🛡️ Threat Modelling & Incident Response – Easy Notes  

---

## 1️⃣ What is Threat Modelling?

- **Meaning**: Checking, improving, and testing security in an organisation’s IT systems.  
- **Goal**: Find likely threats + find weaknesses + plan defences.  
- **Like**: Workplace risk assessment but for computer systems.  

### 🔄 4 Basic Principles of Threat Modelling:

| Step | Simple Meaning |
|------|----------------|
| **Preparation** | Get ready. Have tools, team, and process set up. |
| **Identification** | Find threats & vulnerabilities. |
| **Mitigations** | Plan defences to reduce risk. |
| **Review** | Check & update regularly. |

---

## 2️⃣ What Makes a Good Threat Model?

- **Threat Intelligence** (info about real-world attacks)  
- **Asset Identification** (know what you’re protecting)  
- **Mitigation Capabilities** (how to reduce risk)  
- **Risk Assessment** (measure likelihood & impact)  

---

## 3️⃣ STRIDE Framework (by Microsoft, 1999)

Helps identify different **threat types**.  
Each letter = one kind of threat:

| Principle | Simple Meaning | Example / Fix |
|-----------|---------------|---------------|
| **S – Spoofing** | Fake identity (pretending to be someone else). | Use authentication (passwords, API keys, encryption signatures). |
| **T – Tampering** | Changing data without permission. | Use anti-tampering measures, integrity checks (like seals on food). |
| **R – Repudiation** | Denying actions you actually did. | Use logs/audit trails so actions can be tracked. |
| **I – Information Disclosure** | Showing info to people who shouldn’t see it. | Configure permissions correctly; show only owner’s info. |
| **D – Denial of Service (DoS)** | Overloading system so it crashes/unavailable. | Rate limits, redundancy, DDoS protection. |
| **E – Elevation of Privilege** | User gets more power than allowed (like admin rights). | Strong access controls, patch vulnerabilities quickly. |

---

## 4️⃣ Security Incidents

- A **breach of security** = **incident**.  
- Even with strong models, incidents still happen.  
- **Incident Response (IR)** = Steps taken to solve and fix the threat.  

### 🔹 Classification:
- **Urgency**: Type of attack (how quickly must we act?).  
- **Impact**: Which system is affected and how badly does it hurt the business?  

---

## 5️⃣ CSIRT – The Incident Response Team

- **Computer Security Incident Response Team**  
- Pre-arranged group of employees with technical knowledge.  
- They handle incidents when they happen.

---

## 6️⃣ Six Phases of Incident Response  

| Action | Simple Meaning |
|--------|----------------|
| **Preparation** | Be ready with tools, policies, and team. |
| **Identification** | Detect and confirm the threat & attacker. |
| **Containment** | Stop the threat from spreading to other systems. |
| **Eradication** | Remove the threat completely. |
| **Recovery** | Restore systems to normal operation. |
| **Lessons Learned** | Analyse what happened and improve (e.g. train staff after phishing attack). |

---

## 🧠 Easy Analogy  

- **Threat Modelling** = Fire drill + checking building safety.  
- **STRIDE** = Types of fire hazards (match, gas leak, etc.).  
- **Incident Response** = Firefighters responding, putting out fire, and teaching staff how to prevent future fires.

---
