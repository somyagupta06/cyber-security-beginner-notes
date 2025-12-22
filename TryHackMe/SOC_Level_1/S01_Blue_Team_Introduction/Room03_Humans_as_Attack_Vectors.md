# Social Engineering 

## 1. Why Hackers Target Humans (Not Computers)

Hackers have **two ways** to break into a company:

1. Hard way  
   Break firewall, find software bugs, exploit systems  
   (takes days, skills, and luck)

2. Easy way  
   **Trick a human** to give access  
   (takes one email or one call)

Humans are the **weakest link** in cyber security.

Think like this:
A big fort has strong walls,
but if the guard opens the gate himself,
the enemy walks in easily.

Humans are those guards.

---

## 2. What Hackers Want From Humans

Humans already have access to important things like:
- Email
- Databases
- VPN
- Company systems

So hackers target people instead of machines.

### Simple Examples

| Who is attacked | What hacker gets |
|-----------------|------------------|
| HR Manager | Employee data |
| Rich person | Bank access |
| IT Admin | Full company network |
| Government worker | Secret information |

---

## 3. What Is Social Engineering?

Social Engineering means:

**Tricking people into helping hackers**

The victim helps the hacker:
- knowingly (by sharing info)
- unknowingly (by clicking links, opening files)

No technical hacking.
Only **mind game**.

---

## 4. How Social Engineering Works

For an attack to succeed, it must be:

### Trustworthy  
The attacker looks real  
(example: bank email, IT support call)

### Emotional  
The attacker creates:
- fear ("your account is hacked")
- urgency ("act now")
- curiosity ("see who viewed your profile")

People stop thinking logically.

---

## 5. Phishing (Most Common Attack)

Phishing = fake message to steal login details

Example:
You get an email:
"Your account is compromised. Click here."

You click →
Fake login page →
You enter username & password →
Hacker gets it.

Fact:
Billions of phishing emails are sent **every day**.

Types:
- Fake login links
- Email with malicious attachments (ZIP, PDF)

---

## 6. Malware Downloads

Sometimes users **install malware themselves**.

How hackers trick users:
- Fake software websites
- Fake CAPTCHA pages
- QR codes
- Google search results showing fake links

User thinks:
"I am installing a normal app"

But actually:
Malware gets installed.

---

## 7. Deepfakes (New Dangerous Trick)

Hackers use AI to create:
- Fake video calls
- Fake voice calls

Example:
Employee sees a video call from "boss"
Boss asks for urgent money transfer
Employee sends money

Later:
It was a deepfake  
Money is gone

---

## 8. Impersonation (Without AI)

Hackers pretend to be:
- IT support
- Bank staff
- Company employee

Example:
Attacker calls:
"I am from IT, system repair needed"

Victim trusts →
Shares password →
Company gets hacked

Many ransomware attacks start like this.

---

## 9. Other Social Engineering Attacks

Some more tricks hackers use:
- USB drives left in office
- Fake job offers
- Insider threats (employees helping attackers)
- Physical access tricks

All depend on **human mistakes**.

---

## 10. Key Point for Revision

Computers fail because of bugs  
Humans fail because of emotions  

Hackers love humans more than machines.

As a SOC analyst:
Always think:
"Was this caused by human mistake?"

---

## One Line Summary

Social Engineering =  
**Hacking the human mind instead of hacking the computer**
---

# Defending Against Threats 

## 1. Two Main Defense Tasks

Defending a company has **two important tasks**:

### 1. Mitigation  
Mitigation means:
Preventing attacks  
or  
Reducing their damage

Example:
- Training employees
- Blocking phishing emails

Mitigation tries to stop attacks **before** they succeed.

### 2. Detection  
Detection means:
Finding attacks that **bypass mitigation**

No security is 100% perfect.
Some attacks will always slip through.

That is where **SOC analysts** are needed.

---

## 2. Simple Example

Three attacks try to enter a company:

1. First attack  
Blocked by anti-phishing tool

2. Second attack  
Stopped because employee was trained

3. Third attack  
Gets inside  
SOC analysts detect it and stop it

Lesson:
Even good protection fails sometimes.

---

## 3. Role of a SOC Analyst

As a SOC analyst, your main job is:
- Detect attacks
- Investigate alerts
- Stop advanced threats

You will learn this in SOC Level 1.

But problem:
SOC teams get **too many alerts every day**  
This becomes tiring.

So smart idea:
**Prevent common attacks automatically**

---

## 4. Why Mitigation Is Important for SOC

Good mitigation:
- Reduces number of alerts
- Makes SOC work easier
- Improves overall security

If SOC gives good mitigation ideas
and management approves them,
everyone benefits.

---

## 5. Defending Humans (Employees)

Employees are the main target.
So protecting humans is very important.

### Common Mitigation Measures

| Mitigation | What it does |
|----------|-------------|
| Anti-phishing solution | Blocks phishing emails before users see them |
| Antivirus / EDR | Stops malware from running on computers |
| Trust but verify rule | Employees verify requests from CEO or IT |
| Security awareness training | Teaches employees how to spot phishing |

---

## 6. Trust But Verify 

Do not blindly trust messages.

If someone says:
- "I am the CEO"
- "I am from IT"

Employee should:
- Double-check
- Verify using another method

This helps against:
- Deepfakes
- Impersonation attacks

---

## 7. Final Revision Points

- Mitigation tries to stop attacks early
- Detection catches attacks that bypass defenses
- Humans are the weakest link
- Training + tools reduce SOC workload
- SOC analysts protect the company when prevention fails

---

## One Line Summary

Mitigation reduces attacks  
Detection catches the ones that survive  
SOC analysts save the day
