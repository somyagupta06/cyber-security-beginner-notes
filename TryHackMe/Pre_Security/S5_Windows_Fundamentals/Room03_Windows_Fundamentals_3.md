# 🌟 Windows Update & Windows Security 

---

# 1) 🚀 What is Windows Update?
Windows Update is like your phone’s software update.

Microsoft sends:
- Security fixes  
- New features  
- Bug patches  
- Defender antivirus updates  

🗓️ Updates come mostly on **Patch Tuesday**  
(2nd Tuesday of every month)

But if something is important or dangerous, Microsoft sends the update immediately.

---

## 🔍 Where do you find Windows Update?
Open:
Settings → Update & Security → Windows Update
<img width="728" height="540" alt="Screenshot 2025-12-07 at 7 36 18 PM" src="https://github.com/user-attachments/assets/82901f2c-15f1-4da8-a404-bb0d85356b37" />

OR use Run box:
control /name Microsoft.WindowsUpdate
<img width="790" height="639" alt="Screenshot 2025-12-07 at 7 36 32 PM" src="https://github.com/user-attachments/assets/c2181a00-ac47-49c8-ae37-4d43f9d72b25" />

---

## ⚠️ In the TryHackMe VM
- Updates are **managed** (because it’s a server)
- No new updates (VM has no internet)

---

## 💡 Why Windows forces updates now?
Earlier people used to ignore updates → got viruses → system broke.  
So Windows 10+ does not allow you to ignore updates forever.  
You can delay them, but update will install eventually.

---

## 🔄 Restart Required Screen
Windows shows options like:
- Restart now  
- Pick a time  
- Remind me later  
<img width="679" height="634" alt="Screenshot 2025-12-07 at 7 36 52 PM" src="https://github.com/user-attachments/assets/62c44c0a-cce9-4f81-a9cf-0b48a934da0b" />



---

# 2) 🛡️ Windows Security (Defender Security Center)

Windows Security = Home for all security tools.

You can open it from:
Settings → Update & Security → Windows Security
<img width="779" height="574" alt="Screenshot 2025-12-07 at 7 37 08 PM" src="https://github.com/user-attachments/assets/35076bd4-31c5-41a8-b8e6-99ca24805dda" />

Status icons meaning:
- 🟢 Green = Safe  
- 🟡 Yellow = Something needs checking  
- 🔴 Red = Serious problem  

---

# ▶️ Virus & Threat Protection

This section is divided into:

## 1) Current Threats  
Shows:
- Quick scan  
- Full scan  
- Custom scan  
- Scan history  
- Quarantined items  
- Allowed items  

⭐ Quick scan  
Checks common virus locations  
Fast, usually 1–2 mins  

⭐ Full scan  
Checks entire computer  
Slow (can be 1 hour or more)

⭐ Custom scan  
You choose which folder to scan

⚠️ Allowed threats  
Only allow if 100% sure — risky.
<img width="794" height="590" alt="Screenshot 2025-12-07 at 7 38 08 PM" src="https://github.com/user-attachments/assets/fee80dba-c5fc-4c5c-860f-5b75c2227c10" />
<img width="790" height="590" alt="Screenshot 2025-12-07 at 7 38 18 PM" src="https://github.com/user-attachments/assets/b401a734-a4be-4d91-bcbe-0f57c53c6640" />

<img width="478" height="594" alt="Screenshot 2025-12-07 at 7 38 32 PM" src="https://github.com/user-attachments/assets/6ad2253d-702a-46c6-90ce-7797aebc4288" />
---

## 2) Virus & Threat Protection Settings

### Important options:
- **Real-time protection** → Stops malware automatically  
- **Cloud protection** → Uses Microsoft online database for latest threats  
- **Automatic sample submission** → Sends suspicious files to Microsoft  
- **Controlled Folder Access** → Protects your personal files from ransomware  
- **Exclusions** → Files/folders Defender should NOT scan  

⚠️ Warning:  
Excluded files can hide malware. Only add exclusions if you know what you're doing.

---

## 3) Ransomware Protection
To use this, “Controlled Folder Access” must be ON.

---

### Right-click Scan Option
You can right-click any file → Scan with Microsoft Defender.

<img width="456" height="421" alt="Screenshot 2025-12-07 at 7 38 51 PM" src="https://github.com/user-attachments/assets/7dba8ed7-cdf1-49c6-b239-e9b0032b718b" />
<img width="421" height="453" alt="Screenshot 2025-12-07 at 7 39 15 PM" src="https://github.com/user-attachments/assets/daeef52e-1f94-4d1e-a52f-5aa548ddbb89" />


---

# 3) 🔥 Firewall & Network Protection

## 🔥 What is a firewall?
Think of a firewall like a **security guard at a door**.  
It checks what is allowed in/out through your network ports.
<img width="508" height="572" alt="Screenshot 2025-12-07 at 7 39 31 PM" src="https://github.com/user-attachments/assets/c1741628-66de-4776-83e7-864c2e252330" />

---

## 🧱 Types of Windows Firewall Profiles

| Profile | Simple Meaning |
|--------|----------------|
| **Domain** | Work/office network with domain controller |
| **Private** | Home or trusted network |
| **Public** | Cafe, airport, hotel Wi-Fi (least safe) |

---

## ✔️ Inside a Firewall Profile
You will see:
- Turn firewall ON/OFF  
- Block all incoming connections  

⚠️ Do NOT disable firewall unless you fully understand what you’re doing.

---

## ✔️ Allow an App Through Firewall
Here you can choose which apps can use:
- Private network  
- Public network  

Example apps:  
Chrome, File Sharing, Remote Desktop, etc.

---

## ✔️ Advanced Firewall Settings
This is for **advanced users only** —  
allows creating custom rules, port rules, program rules, etc.

Open Firewall Advanced Settings:
WF.msc

---

# ⭐ Summary Table

| Feature | What It Does |
|--------|---------------|
| Windows Update | Installs security fixes & new features |
| Virus & Threat Protection | Protects against malware |
| Real-time protection | Stops threats instantly |
| Controlled Folder Access | Protects files from ransomware |
| Firewall | Blocks unwanted network traffic |
| WF.msc | Opens advanced firewall panel |

---

# 🎯 Super Simple Memory Trick
- **Windows Update** → “Phone update for your PC”  
- **Defender** → “PC’s built-in bodyguard”  
- **Firewall** → “Security guard at network door”  
- **Quarantine** → “Virus is locked up”  
- **Exclusions** → “Defender will ignore these files”  

---
# 🌟 Windows Security (SmartScreen, Core Isolation, TPM, BitLocker, VSS)  
### Simple Notes — Explained Like You're in 9th Class 😄

---

# 1) 🛡️ Microsoft Defender SmartScreen

### 🌍 What is SmartScreen?
SmartScreen is like a **bodyguard for your browsing and downloading**.

It protects you from:
- Fake websites (phishing)
- Malware websites
- Suspicious apps
- Dangerous downloads

### 🔧 Three Modes:
- **Warn** → Show warning before opening risky app/website  
- **Block** → Completely stop you from opening it  
- **Off** → No protection (not recommended)

### ✔️ Important Setting:
**Check apps and files**  
→ Windows checks apps/files you download from the internet.  
If the app looks suspicious → SmartScreen gives a warning.

---

# 2) 💣 Exploit Protection

Exploit = A hacker trick to break into the system by abusing software weaknesses.

Exploit Protection =  
Windows has built-in shields to stop these attacks automatically.

You normally **don’t need to change anything** here.  
Default settings are safest.

---

# 3) 🧱 Core Isolation

Core Isolation = Extra security walls inside Windows.

## ⭐ Memory Integrity
This setting:
- Stops malware from injecting code into important system processes.
- Protects your system at a deeper level.

⚠️ Recommendation: Keep it ON unless you know exactly what you're doing.

---

# 4) 🔐 Security Processor (TPM)

### What is TPM?
TPM = A tiny security chip inside your computer.  
Think of it like a **locker that stores important keys safely**.

TPM helps with:
- Encryption  
- Secure login  
- Protecting passwords  
- Preventing tampering

### Why is TPM secure?
- Malware cannot modify it  
- It handles encryption inside hardware (not software)

This makes your computer MUCH safer.

---

# 5) 🔒 BitLocker (Drive Encryption)

### What is BitLocker?
BitLocker = A feature that **locks your entire hard drive** with encryption.

Even if someone:
- Steals your laptop  
- Removes your hard disk  
- Or tries to bypass Windows  

They **cannot read your data** without the key.

### Works best when TPM is present
TPM + BitLocker = Strongest protection  
Your drive unlocks only after TPM confirms system integrity.

### Note:
BitLocker is **not available** in your TryHackMe VM.

---

# 6) 🪞 Volume Shadow Copy Service (VSS)

### What is VSS?
VSS = Windows feature that creates **Restore Points** (snapshots).

Restore Points help you return your PC to an earlier time.

### VSS allows:
- Create restore point  
- Restore system to old state  
- Configure restore settings  
- Delete old restore points  

Snapshots are stored in:
System Volume Information (hidden system folder)

---

# ⚠️ Important Security Warning
Ransomware attackers often:
- Delete all VSS restore points  
→ So victims cannot recover their files.

This is why **off-site / offline backup** is important.

---

# 7) 🛠️ Using VSS (System Protection)
If enabled, you can:
- Turn protection ON/OFF  
- Make manual restore points  
- Restore previous system state  

This is helpful for fixing problems without reinstalling Windows.

---

# ⭐ Super Simple Summary Table

| Feature | Simple Meaning | Why Useful |
|--------|----------------|-----------|
| SmartScreen | Warns/blocks unsafe apps/websites | Stops scams & malware |
| Exploit Protection | Stops advanced hacker tricks | Protects system processes |
| Core Isolation | Extra security wall | Protects memory from attacks |
| TPM | Security chip | Stores encryption keys safely |
| BitLocker | Encrypts full drive | Protects data if laptop is stolen |
| VSS | System snapshots | Restore system to earlier time |

---

# 🎯 Memory Trick
- **SmartScreen** → "Alert! This file looks sus."  
- **Core Isolation** → "Keep malware away from system brain."  
- **TPM** → "Secret chip that guards your keys."  
- **BitLocker** → "Whole PC locked with a password."  
- **VSS** → "Time machine for your Windows."

---

