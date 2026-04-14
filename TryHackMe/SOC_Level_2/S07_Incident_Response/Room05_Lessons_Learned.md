# 🛡️ Incident Response (IR) 

## 📌 What is this phase?
This is the **last phase** of Incident Response.

👉 Here we:
- Sit and think about the incident
- Understand what happened
- Learn from mistakes
- Improve security for future

---

# 🧠 Important Idea
> If you don’t learn from an incident, it will happen again.

---

# 🔁 Full IR Process (SOC View)

1. Preparation  
2. Identification & Scoping  
3. Containment  
4. Eradication & Recovery  
5. Lessons Learned (THIS PHASE)

---

# 🟢 1. Preparation (Before attack)

SOC must be ready.

### ✔️ What SOC needs:
- Trained team (analysts)
- Proper rules & playbooks
- Tools (SIEM, EDR)
- Asset list (know all systems)

👉 Simple:
SOC should be ready before attack happens.

---

# 🔵 2. Identification & Scoping (Detect + Investigate)

SOC detects alerts and investigates.

### 🔄 Important:
This is a loop:
- Alert comes
- Investigate
- Find more info
- Investigate again

### 📍 In this case:
- Phishing email sent
- One user reported ✔️
- One user ignored ❌

👉 Lesson:
User reporting is very important.

---

# 🟡 3. Containment + Threat Intelligence

### 🛑 Containment:
Stop the attack quickly:
- Block IP
- Disable accounts

### 🧠 Threat Intelligence:
Collect useful data:
- Malicious IPs
- Domains
- Malware files

👉 SOC uses this to:
- Create detection rules
- Stop future attacks faster

---

# 🔴 4. Eradication + Recovery

### 💣 Eradication:
- Remove malware
- Remove attacker access

### 🔧 Remediation:
Fix the root problem

### 📍 Problems found:
- No email security (SPF, DKIM, DMARC)
- Password reuse
- Weak monitoring

---

# 💥 Attack Flow (Simple)

1. Phishing email  
2. Password stolen  
3. Password reused  
4. Jenkins server hacked  
5. Fake backup script used  
6. Data stolen  

---

# 🧠 5. Lessons Learned (MOST IMPORTANT)

SOC must answer:

### ❓ What went wrong?
- Weak email security
- Users not reporting
- Poor monitoring

### ❓ How to fix?

---

# 🔧 Improvements (SOC Actions)

### 🚨 Detection
- Better phishing alerts
- Suspicious login alerts

### 🔐 Security
- Add SPF, DKIM, DMARC
- Enable MFA

### 👨‍🏫 Awareness
- Train users to report emails

### 📡 Monitoring
- Monitor critical systems (like Jenkins)

---

# 📊 Summary (Easy)

| Step | SOC Role |
|------|--------|
| Preparation | Be ready |
| Identification | Detect attack |
| Containment | Stop damage |
| Eradication | Remove attacker |
| Lessons Learned | Improve system |

---

# 💡 Final Line

> Good SOC = Not just fixing attacks, but learning from them.
---
# 🛡️  Technical & Executive Summary + Sigma

---

# 📌 1. Technical Summary (SOC View)

## 👉 What is it?
A **short technical report** of the incident from start to end.

## 🎯 Goal:
Explain **how the attack happened and how it was handled**

## 🧩 Include:
- Entry point (phishing, malware, etc.)
- Attack flow (how it spread)
- Affected systems
- Tools used (IP, domain, malware)
- Actions taken (containment, recovery)

## ⚠️ Important:
- Not too long ❌
- Not too short ❌
- Balanced ✅

---

# 📌 2. Executive Summary (Management View)

## 👉 What is it?
A **simple report for non-technical people**

## 🎯 Goal:
Explain **business impact**

## 🧩 Include:
- 💰 Money loss?
- 📂 Data stolen?
- 📉 System downtime?
- 😨 Reputation damage?
- 🤔 How attack happened (simple)
- 🔧 What actions taken

---

# ⚖️ Technical vs Executive

| Feature | Technical Summary | Executive Summary |
|--------|-----------------|-----------------|
| Audience | SOC / Engineers | Managers / CEO |
| Language | Technical | Simple |
| Focus | Attack details | Business impact |

---

# 📌 3. SoD (Source of Detection)

## 👉 What is it?
List of **malicious things found during investigation**

## 🧩 Examples:
- IP addresses
- Domains
- Malware files

👉 These are called:
**IOC (Indicators of Compromise)**

---

# 📌 4. SOC Cycle (VERY IMPORTANT)

1. Attack happens  
2. SOC investigates  
3. Finds IOCs  
4. Adds them to detection system  
5. Future attack → instantly detected  

👉 This is a **cycle 🔁**

---

# 📌 5. Sigma (Detection Rule Language)

## 👉 What is Sigma?
A **rule format** to detect threats in logs

## 🎯 Goal:
Create reusable detection rules

---

# 📌 6. Example Rule (Easy Understanding)

## 🧠 Logic:

IF:
- Download from malicious IP  
AND  
- File is .exe  

THEN:
🚨 Alert

---

## 🧩 Key Parts:

### 🔹 Malicious IPs
- 188.40.75.132  
- 3.250.38.141  

### 🔹 Suspicious Action
- Download of `.exe` file  

---

# 📌 7. Why This is Important (SOC)

- Detect same attacker again  
- Faster response  
- Better security  
- Less damage in future  

---

# 💡 Final Line

> SOC is not just about fixing attacks,  
> it is about learning and detecting them faster next time.


---
# Report 
## Executive Summary

On the 13th of July, 2023, Michael Ascot received an email from alex.swift@swiftspend[.]finance with a URL that leads to a seemingly innocuous O365 page that requires the user to “re-authenticate”. Upon supplying his credentials, the page didn’t show anything and so Michael immediately became suspicious and sent a reply to the email asking for clarifications for which he received the outlook error that lead to the user raising Ticket#2023012398704232 to SOC.

SOC immediately asked Michael to change his password. Upon further investigation, SOC found out that alex.swift@swiftspend[.]finance is a fake email, maliciously spoofed to look strikingly similar to the legitimate email alexander.swift@swiftspend[.]finance.

SOC also found out about the swiftspend_admin credential leak through a misconfiguration where the plaintext password is hard coded into an application’s config file. The threat actor’s discovery of password reuse led to the Jenkins server compromise where SOC recovered an exfiltration script and uncovered and cleared a number of backdoor accesses.

Fortunately, since the server has only been newly setup, essentially nothing of note has been stolen.

It is also confirmed that email controls weren’t able to detect the initial phishing campaign because of a previously identified unpatched vulnerability in the form of missing email security (i.e., SPF, DKIM, DMARC) that is yet to be remediated. 

On top of email security remediations, endpoint protection definitions and overall patch management are being targeted for the short to mid term implementation of the recovery plan.



## Technical Summary

Upon receipt of the incident Ticket#2023012398704232, SOC immediately requested for a message trace involving both Michael Scot and Alex Swift which consequently shows the targeted nature of the phishing campaign. Web Proxy logs were also identified as an essential resource wherein SOC confirmed the compromise of Michael’s credentials through a POST request sent to a phishing domain containing his plaintext credentials.

A review of the original phishing email allowed SOC to confirm that it is indeed a spoofed email, with the return domain being emkei[.]cz. Immediately after, SOC recommended the remediation of this vulnerability by focusing on the adaption of mail security controls namely SPF, DKIM, and DMARC.

A review of the packet capture retrieved during the time of the event also allowed for the discovery of numerous artefacts and pivot points, mainly consisting of malicious IPs that hosts the threat actor’s malware infrastructure, as well as malware samples.

Finally, SOC was able to finally trace the threat actor activity to the company’s Jenkins server where it was found out that the threat actor has already compromised the server, with the platform being manipulated to host a scheduled exfiltration task, disguised as a backup implementation. It has also been found out that the Jenkins service account has been turned into a backdoor, and that a swiftspend domain has been hijacked to host the threat actor’s exfiltration IP.

Full details of the artefact details are in the attached SoD.




<img width="1470" height="956" alt="Screenshot 2026-04-14 at 9 54 23 AM" src="https://github.com/user-attachments/assets/529227ba-5f42-428c-bd82-fcbc80cce732" />
