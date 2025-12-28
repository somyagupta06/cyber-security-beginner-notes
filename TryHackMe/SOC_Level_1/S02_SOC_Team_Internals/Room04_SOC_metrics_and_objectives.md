# SOC Metrics 

## 1. What is a SOC?
A **SOC (Security Operations Center)** protects the company’s:
- **Confidentiality** – data should be secret  
- **Integrity** – data should not be changed wrongly  
- **Availability** – systems should always work  

SOC team does this by **watching alerts** and **stopping attacks**.

---

## 2. Role of an L1 Analyst (Your Job)
- You are the **first person** who sees alerts
- Your main job:
  - Ignore **fake alerts** (False Positives)
  - **Escalate real threats** to L2 analysts

---

## 3. Important SOC Metrics 

### (A) Alerts Count (AC)
**Meaning:**  
How many alerts you get in a day.

**Why it matters:**
- Too many alerts → confusing → real attack can be missed  
- Too few alerts → tools may be broken

**Good number:**  
**5 to 30 alerts per day per L1 analyst**

---

### (B) False Positive Rate (FPR)
**Formula:**  
False Positives / Total Alerts

**Meaning:**  
How many alerts are actually useless.

**Example:**  
- 80 alerts  
- 75 are fake  
- FPR = very high (bad)

**Why bad?**
- Too much noise
- Analysts stop taking alerts seriously

**Good value:**  
- 0% is impossible  
- **80% or more = very bad**

---

### (C) Alert Escalation Rate (AER)
**Formula:**  
Escalated Alerts / Total Alerts

**Meaning:**  
How often L1 sends alerts to L2.

**Why it matters:**
- Too high → L1 lacks confidence
- Too low → L1 may miss real attacks

**Good value:**  
- Below **50%**
- Best is **below 20%**

---

### (D) Threat Detection Rate (TDR)
**Formula:**  
Detected Threats / Total Threats

**Meaning:**  
How many real attacks the SOC actually caught.

**Example:**  
- 6 attacks happened  
- Only 4 detected  
- TDR = 67% (very bad)

**Ideal value:**  
- **100%**
- Even one missed attack is dangerous

---

## 4. Time-Based Metrics (SLA)

### (A) MTTD – Mean Time to Detect
- Time taken to **detect an attack**
- SLA: **5 minutes**

### (B) MTTA – Mean Time to Acknowledge
- Time for L1 to **start working on alert**
- SLA: **10 minutes**

### (C) MTTR – Mean Time to Respond
- Time to **stop the attack**
- SLA: **60 minutes**
<img width="1102" height="371" alt="Screenshot 2025-12-28 at 2 26 32 PM" src="https://github.com/user-attachments/assets/c9a2426d-16d6-434b-b0fb-02d80abfbdfa" />

---

## 5. Why Metrics Matter to You
- They show **how good you are**
- Used for:
  - Performance review
  - Promotion to **L2 analyst**
  - Salary growth

Good metrics = good career 📈

---

## 6. How to Improve Bad Metrics (Simple Table)

| Problem | What to Do |
|------|-----------|
| False Positive Rate > 80% | Remove trusted system activities from alerts |
|  | Automate common alerts |
| MTTD > 30 min | Make detection rules faster |
|  | Ensure logs come in real time |
| MTTA > 30 min | Get instant alert notifications |
|  | Share alerts equally among analysts |
| MTTR > 4 hours | Escalate quickly to L2 |
|  | Follow documented response steps |

---

## 7. One-Line Revision
- Less noise
- Faster detection
- Faster response
- Correct escalation  
= **Strong SOC + Strong Career**

--- 
END OF NOTES
