# 📘 EDR (Endpoint Detection and Response) 

## 🧠 What is EDR?

EDR is a security tool that watches what is happening on computers (endpoints like laptops, servers).

It helps to:
- Detect attacks
- Understand what happened
- Stop the attack

👉 Simple idea:
EDR = "Security guard + CCTV + Investigator" for each computer

---

## ⚙️ Main Functions of EDR

### 1. Monitoring
EDR continuously watches:
- Processes (programs running)
- Files (created, deleted, modified)
- Network connections
- User actions

Example:
winword.exe starts powershell.exe → suspicious

---

### 2. Analysis
EDR checks:
- Is this normal behavior?
- Is it similar to malware?
- Does it match threat intelligence?

---

### 3. Response
EDR can take action:
- Kill process
- Delete file
- Isolate system from network

---

### 4. Investigation (For SOC Analyst)
SOC analyst uses EDR to:
- Find how attack started
- Track attacker actions
- Build timeline

Example:
Email → Word file → Macro → PowerShell → Malware

---

## 📊 Two Main Parts of EDR

### 1. Data Collection (Telemetry)
EDR collects:
- Process logs
- File activity
- Commands
- Network activity

---

### 2. Investigation & Response
SOC analyst:
- Searches for IOCs (IP, hash, domain)
- Finds root cause
- Hunts for similar attacks

---

## 🧩 Components of EDR

### Detection
Finds suspicious or malicious activity

### Response
Stops or contains the attack

### Integration
Works with other tools (SIEM, Firewall)

### Insights
Uses AI/ML to understand behavior

### Forensics
Looks at past events and builds attack timeline

---

## 🪟 Event Tracing for Windows (ETW)

ETW is a logging system in Windows.

It helps collect detailed system activity.

### Parts of ETW:
- Controller → starts logging
- Provider → generates logs
- Consumer → reads logs

👉 EDR uses ETW for deep visibility

---

## 📂 Windows Event Viewer

Tool to see logs in Windows

### Main Log Types:

#### Application Logs
- App-related events
- Malware often leaves traces here

#### System Logs
- OS and driver events

#### Security Logs (Very Important)
- Login success/failure
- Access attempts

---

## 📊 Event Levels

- Information → Normal activity
- Warning → Might cause problem later
- Error → Something failed
- Success Audit → Login success
- Failure Audit → Login failed (possible attack)

---

## 🧠 SOC Analyst Workflow Example

Alert:
powershell.exe running suspicious command

Steps:
1. Open EDR tool
2. Check process tree
   winword.exe → powershell.exe
3. Check logs in Event Viewer
4. Search IOCs (IP, hash)
5. Check if other systems affected
6. Take action:
   - isolate system
   - remove malware

---

## 📝 Final Revision Points

- EDR monitors endpoint activity
- Detects suspicious behavior
- Responds automatically or manually
- Helps SOC analysts investigate attacks
- Uses logs (ETW, Event Viewer) for detection

👉 Key idea:
EDR gives full visibility + control over endpoint security

----
# 📘 Aurora (EDR Tool)

## 🧠 What is Aurora?

Aurora is a Windows endpoint security tool.

It:
- Uses ETW logs (Windows activity data)
- Uses Sigma rules + IOCs
- Detects suspicious behavior
- Can take response actions

👉 Simple idea:
Aurora = "Smart detection + automatic response tool"

---

## ⚙️ How Aurora Works

1. Collects data from ETW (Windows logs)
2. Matches activity with Sigma rules
3. If match found → alert generated
4. Can trigger response (kill, suspend, etc.)

👉 Important:
Aurora only shows alerts when rules match  
(It does NOT log everything)

---

## 🔍 Key Concepts

### Sigma Rules
- Detection rules
- Define suspicious behavior

Example:
If powershell runs encoded command → alert

---

### IOC (Indicators of Compromise)
- Signs of attack

Examples:
- Malicious IP
- File hash
- Domain

---

## 📊 Aurora vs Sysmon

| Feature | Aurora | Sysmon |
|--------|--------|--------|
| Log Volume | Low | High |
| Detection | Yes | No |
| Response | Yes | No |
| Noise | Low | High |

👉 SOC Use:
- Sysmon → raw logs
- Aurora → useful alerts

---

## 🧩 Aurora Versions

### Aurora (Enterprise)
- Nextron rules + IOC set
- Extra modules
- Unlimited responses
- ASGARD management

### Aurora Lite
- Only open-source rules
- No advanced features

---

## 🧪 Aurora Presets (Configurations)

### Minimal
- High severity only
- Low CPU usage

### Reduced
- Less events than standard

### Standard
- Balanced monitoring

### Intense
- Low threshold (more alerts)
- High CPU usage

👉 Simple:
Minimal = fewer alerts  
Intense = more alerts  

---

## ▶️ Running Aurora

Start Aurora:
aurora-agent.exe -c config.yml

Run as service:
aurora-agent.exe --install -c config.yml

---

## 🛠️ Important Commands

Check status:
aurora-agent.exe --status

Trace events:
aurora-agent.exe --trace

Output in JSON:
aurora-agent.exe --json

---

## 📤 Output Locations

### 1. Windows Event Viewer (Most Important)
- Alerts visible here
- Check "Application Logs"

---

### 2. Log File
aurora-agent.exe --logfile aurora.log

---

### 3. Network (SIEM)
--udp-target host:port
--tcp-target host:port

---

## ⚡ Response Actions

### Predefined Responses
- kill → terminate process
- suspend → pause process
- dump → create memory dump

---

### Custom Responses
- Run custom command

Example:
copy suspicious file to backup folder

---

## 🧠 Response Flags (Simple)

- simulate → test only (no real action)
- recursive → affect child processes
- lowprivonly → only low privilege processes
- ancestor → affect parent process

---

## 🆔 Important Event IDs

| Event ID | Meaning |
|----------|--------|
| 1 | Process creation detected |
| 3 | Network connection detected |
| 11 | File creation detected |
| 22 | DNS query detected |
| 6000 | Response executed |
| 6001 | Response simulated |

---

## 🧠 SOC Workflow Example

Alert:
Event ID 1 → suspicious process

Steps:
1. Open Event Viewer
2. Check process chain
   winword.exe → powershell.exe
3. Identify suspicious behavior
4. Search IOC (IP, hash)
5. Check other systems
6. Take action:
   - kill process
   - isolate system

---

## 📝 Final Revision Points

- Aurora uses ETW + Sigma rules
- Detects only suspicious activity (low noise)
- Supports automatic response
- Logs alerts in Event Viewer
- Helps SOC analysts investigate quickly

👉 Key Idea:
Aurora = Detection + Response + Low Noise

---

# 📘 Aurora Function Tests & Detection Gaps 

## 🧪 Function Tests (Testing Aurora Detection)

These tests check if Aurora is working correctly.

---

### 🔹 Test 1: User Privilege Check

Command:
whoami /priv

Aurora Detection:
- Sigma rule triggered
- Alert level: WARNING
- Type: Process Creation

👉 Why suspicious?
Attackers use this to check privileges.

👉 SOC Action:
- Check who ran the command
- Verify if it is expected or not

---

### 🔹 Test 2: Suspicious Network (DNS Beaconing)

Example:
Cobalt Strike DNS beacon

Aurora Detection:
- Critical alert triggered

👉 Why suspicious?
- Repeated DNS requests = possible malware communication

👉 SOC Action:
- Check process making request
- Analyse domain
- Block if malicious
- Isolate system if needed

---

## ⚠️ Detection Gaps (Limitations of Aurora)

Aurora uses ETW, so some activities are not fully visible.

---

### ❌ 1. Named Pipes

What it is:
- Communication between processes

Problem:
- ETW cannot directly track named pipes

Risk:
- Attackers use it for hidden communication

Solution:
- Use Aurora in Intense mode
- Use Sysmon for better visibility

---

### ❌ 2. Registry Events

What it is:
- Windows configuration changes

Problem:
- ETW logs are complex (handle-based)
- Hard to track directly

Risk:
- Malware persistence (auto start)

Solution:
- Use Intense mode
- Use Sysmon for detailed logs

---

### ❌ 3. ETW Disabled (Critical Risk)

What happens:
- Attacker disables ETW logging

Impact:
- Aurora cannot see events
- System becomes blind

---

### ✅ Solutions

1. ETW Canary (Aurora feature)
   - Detects ETW tampering

2. --report-stats flag
   - Shows:
     - observed events
     - processed events
     - dropped events

👉 Mismatch in stats = suspicious activity

---

## 🧠 SOC Scenario Examples

### Case 1:
whoami /priv detected

→ Check user and process  
→ If unknown → investigate

---

### Case 2:
Repeated DNS requests

→ Possible beaconing  
→ Check domain + process  
→ Block + isolate

---

### Case 3:
No logs suddenly

→ Possible ETW disabled  
→ Check stats and system integrity

---

## 📝 Final Revision Points

- Aurora detects suspicious commands using Sigma rules
- DNS beaconing = strong sign of malware
- Named Pipes not visible via ETW
- Registry events are hard to analyse
- ETW disabled = biggest detection risk

👉 Key Idea:
Aurora is powerful but has blind spots → use with Sysmon


---

## Lab Questions
### Q.1 What is the title of the first event rule?
<img width="1470" height="956" alt="Screenshot 2026-03-28 at 12 22 20 PM" src="https://github.com/user-attachments/assets/d742aa6f-617a-483c-962d-bd1cf7dbb33a" />

### Q.2 What is the Rule ID of the matched Sigma rule?
### Q.3 What is the Sigma rule level of the event?
<img width="1470" height="956" alt="Screenshot 2026-03-28 at 12 22 51 PM" src="https://github.com/user-attachments/assets/480b1a7e-fc3b-4303-afe8-c01058b8c4d3" />




### Q.4 What is the Rule Title of the second Event?


### Q.5 Based on the Rule's characteristics, what malicious activity is associated with the event?


<img width="1220" height="817" alt="Screenshot 2026-03-28 at 12 25 26 PM" src="https://github.com/user-attachments/assets/71deb6eb-f4b6-40a8-a785-54f9ee8b73aa" />


























