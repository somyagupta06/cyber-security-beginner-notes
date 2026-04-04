# 🛡️ SOC Level 2 (L2) Communication Notes 

---

## 🧠 What is L2 Role in SOC?

L1 finds alerts.  
L2 understands the attack and explains it clearly.

👉 L2 main job:
- Investigate deeply
- Write clear reports
- Communicate with others

---

# 🧑‍💼 1. Reporting to Company Leadership (CEO / CTO / CISO)

These people are **non-technical**.

## 🎯 Goal:
Explain the incident in a **business way**

---

## ✅ Rules to Follow

### 1. Focus on Business
Do not talk only about technical things.

✔️ Say:
- Data risk
- Money loss
- Business impact

❌ Do NOT say:
- Logs, hashes, commands

---

### 2. Use Formal Tone
✔️ Example:
"A security issue was detected and handled."

❌ Avoid:
Casual or friendly language

---

### 3. Keep it Simple
Use very easy English.

✔️ Example:
"A system tried to connect to an unknown server."

---

### 4. Talk with Proof
Always support your points:
- Logs
- Screenshots
- Timeline

---

### 5. Stay Calm
✔️ Say:
"The situation is under control."

❌ Do NOT panic

---

## 🧾 Simple Example (C-Level)

"A suspicious activity was detected on one system. It could have caused data loss, but the system was quickly isolated. No confirmed damage."

---

# 📧 2. Reporting to MSSP Customers

You will send emails to customers about security incidents.

👉 Always use:
- Email
- CC team members

---

# 🚨 3. Initial Report (Very Important)

## 🎯 Goal:
Tell customer quickly:
👉 "Take action now"

---

## 🧱 Structure

### 1. Urgency
"This is a critical incident"

---

### 2. What Happened
Short explanation

---

### 3. What Customer Should Do
Actions to stop damage

---

### 4. What SOC Will Do
Next steps

---

## 🧠 Example

- Malware found on laptop
- AWS keys stolen

### Actions:
- Disable AWS keys
- Inform user

---

👉 Important:
Initial report is **short + urgent**

---

# 📊 4. Final Report (Full Explanation)

## 🎯 Goal:
Explain everything clearly and close the case

---

## 🧱 Structure

### 1. Incident Summary
- When it happened
- What happened
- Who was affected

---

### 2. Root Cause
Why it happened

Example:
- User clicked phishing link
- Installed unsafe software

---

### 3. Impact
- What data was affected
- Risk level

---

### 4. Actions Taken
- Malware removed
- System isolated
- Keys changed

---

### 5. Recommendations
How to prevent in future

Example:
- User training
- Strong security rules

---

# ❓ Questions Every Final Report Must Answer

- How did the attack start?
- Who was affected?
- Is the attack still active?
- What damage happened?
- Why SOC did not stop it earlier?
- What will improve now?
- How to stop this in future?

---

# ⚡ Quick Comparison

| Type            | Goal              | Style                |
|-----------------|------------------|---------------------|
| Initial Report  | Quick action     | Short + urgent      |
| Final Report    | Full explanation | Detailed + clear    |
| C-Level Report  | Business view    | Simple + non-tech   |

---

# 🎯 Final Summary

- L2 = Investigator + Communicator
- Always write clear and simple reports
- Change your language based on audience
- Focus on action and impact

👉 If report is clear → incident can be stopped fast
👉 If report is confusing → big risk

---
