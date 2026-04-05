# 🛡️ SOC Notes: Threat Actor Activity 

## 1. What is Initial Payload?

Initial payload = First malicious code that runs on a victim system.

### Common Ways Attackers Execute Payload:

* Phishing email (user opens file)
* Malicious website download
* Software vulnerability exploit
* Using built-in tools (PowerShell, cmd)

### Example:

PowerShell downloads and runs a file
PowerShell -EncodedCommand <malicious code>

---

## 2. What Happens After Connection (Persistence)?

Once attacker gets access, they stay connected and start exploring.

---

## 3. Common Activities by Attackers

### 🔍 1. System Information (Discovery)

Purpose: Know the system

Commands:
whoami
hostname
ipconfig
net user

---

### 📂 2. File Search

Purpose: Find useful data

Commands:
dir
find password.txt

---

### 🔐 3. Credential Dumping

Purpose: Steal passwords

Example:
procdump.exe -ma lsass.exe lsass.dmp

---

### 🌐 4. Move to Other Systems (Lateral Movement)

Purpose: Spread in network

Example:
psexec \target_ip cmd.exe

---

### 📡 5. Persistence

Purpose: Stay in system even after reboot

Examples:
Create scheduled task
Add registry run key

---

### 📤 6. Data Theft (Exfiltration)

Purpose: Send data outside

Example:
Upload file to attacker server

---

### 🔥 7. Hide Activity (Defense Evasion)

Purpose: Avoid detection

Example:
Stop antivirus
Use encoded commands

---

## 4. How It Looks in SOC Environment

SOC analysts see logs like:

### 🚨 Suspicious Process

Word → PowerShell → Malware

---

### 🚨 Encoded Commands

PowerShell with long encoded string

---

### 🚨 LSASS Access

Process trying to read lsass.exe

---

### 🚨 Unknown Network Traffic

Internal system connecting to unknown IP

---

### 🚨 Persistence Changes

New scheduled task created
Registry modified

---

## 5. What is Atomic Red Team?

Atomic Red Team = Tool to simulate real attacks safely.

### Why SOC Uses It:

* Test detection rules
* Practice threat hunting
* Check security gaps

---

## 6. Atomic Test Example

Technique: Credential Dumping

Command used:
procdump.exe -ma lsass.exe

SOC should detect:

* Access to lsass.exe
* Dump file creation

---

## 7. Why Cleanup is Important?

* Keeps system safe
* Removes attack tools
* Allows repeat testing
* Prevents misuse

---

## 8. Simple Attack Flow

1. Initial access
2. Payload execution
3. Persistence
4. Information gathering
5. Credential stealing
6. Move to other systems
7. Data theft

---

## 9. SOC Job (Very Important)

SOC must:

* Detect suspicious activity
* Investigate logs
* Respond to threats
* Improve detection rules

---

## ⭐ Quick Revision

* Initial payload = first attack code
* Persistence = attacker stays in system
* LSASS dump = password stealing
* Atomic Red Team = attack simulation tool
* SOC = detect + investigate + protect

---
# 🛡️ Atomic Red Team 

## 1. What is Atomic Red Team?

Atomic Red Team is an open-source tool used to simulate real cyber attacks safely.

* Created by Red Canary
* Used for security testing
* Works like a “fake attacker”

👉 It helps SOC teams check if they can detect attacks.

---

## 2. Purpose (Why SOC Uses It)

SOC uses Atomic Red Team to:

* Test detection rules
* Check alerts in SIEM
* Practice real attack scenarios
* Find security gaps

---

## 3. Supported Platforms

Atomic Red Team works on many environments:

* Windows, Linux, macOS
* AWS, Azure, GCP
* Office 365, Google Workspace, Azure AD
* Kubernetes

👉 Meaning: It can test attacks almost everywhere.

---

## 4. What is Emulation?

Emulation = Copying attacker behavior

👉 Atomic Red Team runs commands similar to real attackers.

---

## 5. What are Executors?

Executors = Tools used to run attack commands

### Types:

* cmd.exe → Windows commands
* PowerShell → Script-based attacks
* sh/bash → Linux/macOS commands
* Manual → Step-by-step human actions

---

## 6. What are Atomics?

Atomics = Small individual attack tests

* Each atomic = one attack technique
* Based on MITRE ATT&CK

Example:

* Credential dumping
* File discovery
* System information gathering

---

## 7. Files Inside an Atomic

Each atomic has 2 main files:

### 1. Markdown File (.md)

* Explains the attack
* Shows usage and details

---

### 2. YAML File (.yaml)

* Contains instructions to run the attack
* Used by tools like Invoke-Atomic

---

## 8. YAML File Breakdown (Very Important)

### Basic Fields:

* attack_technique → Technique ID (e.g., T1003)
* display_name → Technique name
* atomic_tests → List of tests

---

### Inside Each Atomic Test:

* name → Short description
* description → Detailed explanation
* supported_platforms → OS used
* input_arguments → Required inputs

---

## 9. Important Sections in YAML

### Dependencies

* prereq_command → Check if requirement is met
* get_prereq_command → Install/download if missing

---

### Executor

* name → cmd / PowerShell / etc.
* command → Actual attack command

Example:
procdump.exe -ma lsass.exe

---

### Cleanup

* cleanup_command → Removes files or changes

---

### Elevation

* elevation_required → Admin access needed or not

---

## 10. SOC Perspective (Most Important)

When an atomic test runs, SOC should monitor:

* Process creation
* Suspicious commands
* File creation (like dump files)
* Network activity

---

## 11. Example (Credential Dumping)

Atomic runs:
procdump.exe -ma lsass.exe

SOC should detect:

* Access to lsass.exe
* Dump file creation
* Suspicious process behavior

---

## 12. Why Cleanup is Important?

* Keeps system safe
* Removes attack tools
* Prevents misuse
* Allows repeated testing

---

## ⭐ Quick Revision

* Atomic Red Team = attack simulation tool
* Atomics = small attack tests
* YAML = instructions to run attack
* Executors = run commands
* Cleanup = restore system

---

## 🧠 One-Line Summary

Atomic Red Team helps SOC teams simulate real attacks safely to test detection and improve security.

---
# 🛡️ Invoke-AtomicRedTeam 

## 1. What is Invoke-AtomicRedTeam?

Invoke-AtomicRedTeam is a PowerShell module used to run Atomic Red Team tests.

👉 Main command used:
Invoke-AtomicTest

👉 Purpose:

* Execute attack simulations
* Help SOC test detection

---

## 2. Basic Setup Steps

### Step 1: Open PowerShell (Bypass Security)

powershell -ExecutionPolicy bypass

---

### Step 2: Import Module

Import-Module "C:\Tools\invoke-atomicredteam\Invoke-AtomicRedTeam.psd1" -Force

---

### Step 3: Set Atomics Path

$PSDefaultParameterValues = @{"Invoke-AtomicTest:PathToAtomicsFolder"="C:\Tools\AtomicRedTeam\atomics"}

---

### Step 4: Verify Module

help Invoke-AtomicTest

---

## 3. Main Command Syntax

Invoke-AtomicTest <TechniqueID> [Options]

Example:
Invoke-AtomicTest T1127

👉 Runs all tests for technique T1127

---

## 4. Viewing Atomic Details

### ShowDetailsBrief (Short Info)

Invoke-AtomicTest T1127 -ShowDetailsBrief

👉 Shows:

* List of tests
* Test numbers

---

### ShowDetails (Full Info)

Invoke-AtomicTest T1127 -ShowDetails

👉 Shows:

* Commands executed
* Files created
* Cleanup steps

👉 Important for SOC to understand impact

---

## 5. Dependencies (Prerequisites)

### Check if Ready

Invoke-AtomicTest T1127 -CheckPrereqs

👉 Checks if required files/tools exist

---

### Get Missing Requirements

Invoke-AtomicTest T1127 -GetPrereqs

👉 Downloads missing files

⚠️ May fail if no internet

---

## 6. Executing Atomic Tests

### Run All Tests

Invoke-AtomicTest T1127

---

### Run Specific Tests (by number)

Invoke-AtomicTest T1127 -TestNumbers 1,2

---

### Run by Test Name

Invoke-AtomicTest T1127 -TestNames "Test Name"

---

### Run by GUID

Invoke-AtomicTest T1127 -TestGuids <GUID>

---

### Run Single Test Directly

Invoke-AtomicTest T1127-2

---

## 7. Example (Scheduled Task Attack)

Technique: T1053.005

Command:
Invoke-AtomicTest T1053.005 -TestNumbers 1,2

👉 Result:

* Scheduled task is created
* Simulates persistence attack

---

## 8. Cleanup (Very Important)

### Remove Changes After Test

Invoke-AtomicTest T1053.005 -TestNumbers 1,2 -Cleanup

👉 Removes:

* Created files
* Scheduled tasks
* System changes

---

## 9. SOC Perspective (Important)

SOC should monitor during execution:

* Process creation
* Command execution
* File creation
* Scheduled tasks
* Network activity

---

## 10. Detection Example

If Scheduled Task test is run:

SOC should detect:

* New task created
* Suspicious command execution
* Persistence behavior

---

## ⭐ Quick Revision

* Invoke-AtomicTest = run attack
* ShowDetails = understand attack
* CheckPrereqs = check readiness
* GetPrereqs = download requirements
* TestNumbers = run specific tests
* Cleanup = remove attack traces

---

## 🧠 One-Line Summary

Invoke-AtomicRedTeam helps SOC teams run and test attack simulations to verify detection and improve security.

---
# 🛡️ Atomic Red Team + MITRE ATT&CK Navigator 

## 1. Key Idea

Atomic Red Team is closely connected with MITRE ATT&CK.

* Each Atomic = One MITRE Technique
* Atomic folders are named using Technique ID (e.g., T1059)

👉 Meaning:
MITRE gives theory, Atomic gives practical testing

---

## 2. Problem

MITRE ATT&CK has many techniques.

👉 SOC cannot test everything randomly

---

## 3. Solution → ATT&CK Navigator

ATT&CK Navigator helps to:

* Select a specific threat group
* View techniques used by that attacker
* Focus on important attacks only

---

## 4. Workflow (Step-by-Step)

### Step 1: Open ATT&CK Navigator

* Select "Enterprise" layer

---

### Step 2: Select Threat Group

Example: admin@338

👉 This loads techniques used by that attacker

---

### Step 3: Highlight Techniques

* Assign score = 1
* Makes techniques visible

---

### Step 4: Sort & Expand

* Sort by score (descending)
* Expand sub-techniques

👉 Now all relevant techniques are visible

---

## 5. Extract Techniques

Example techniques:

| Tactic         | Technique ID | Technique           |
| -------------- | ------------ | ------------------- |
| Initial Access | T1566.001    | Phishing            |
| Execution      | T1059.003    | Command Shell       |
| Discovery      | T1083        | File Discovery      |
| Discovery      | T1082        | System Info         |
| Discovery      | T1016        | Network Config      |
| Discovery      | T1049        | Network Connections |
| Discovery      | T1007        | Service Discovery   |
| Discovery      | T1087.001    | Account Discovery   |

---

## 6. Check Atomic Availability

Command checks if Atomics exist:

👉 Result:

* 8 techniques available
* 1 technique missing

---

## 7. List Atomic Tests

Use:
Invoke-AtomicTest <TechniqueID> -ShowDetailsBrief

👉 Shows:

* Available tests
* Test numbers

---

## 8. Check Prerequisites

Use:
Invoke-AtomicTest <TechniqueID> -CheckPrereqs

👉 Result:

* Prerequisites met → test can run
* Not met → cannot run

Example issues:

* File missing
* Tool not installed

---

## 9. Select Tests (Smart Approach)

👉 Do NOT run all tests

✔ Select one test per technique
✔ Choose tests with prerequisites met

---

## 10. Selected Tests Example

* T1059.003-3 → Command execution
* T1083-1 → File discovery
* T1082-6 → Hostname discovery
* T1016-1 → Network configuration
* T1049-1 → Network connections
* T1007-2 → Service discovery
* T1087.001-9 → User enumeration
* T1566.001-1 → Phishing simulation

---

## 11. Execute Tests

Run one test at a time:

Invoke-AtomicTest T1059.003-3

👉 This runs a single attack simulation

---

## 12. SOC Perspective (Very Important)

During execution, SOC should monitor:

* Process creation
* Command execution
* File access
* Network activity
* User activity

---

## 13. Detection Goal

SOC should detect:

* Suspicious commands
* Discovery activities
* Phishing behavior
* System enumeration

---

## ⭐ Quick Revision

* MITRE = attack knowledge base
* Atomic = attack simulation
* Navigator = select attacker techniques
* Technique ID = link between MITRE & Atomic
* CheckPrereqs = readiness check
* Run one test at a time

---

## 🧠 One-Line Summary

Use ATT&CK Navigator to select real attacker techniques and Atomic Red Team to simulate them for testing SOC detection.

---
# 🛡️ Detection Engineering using Atomic Red Team

## 1. Key Idea

Now we move from Red Team to Blue Team.

👉 Goal:

* Observe attack behavior
* Detect it using logs and tools

---

## 2. What is Telemetry?

Telemetry = System logs/events

👉 It shows:

* What actions happened
* Which process ran
* What changed in system

---

## 3. Technique Used (Example)

Technique: T1547.001
Boot or Logon Autostart Execution

👉 Purpose:

* Run malware automatically at system startup

---

## 4. Attack Behavior

Attacker modifies:

* Registry Run Keys
* Startup Folder

👉 So malware runs on login

---

## 5. Using Sysmon (Log Monitoring)

Sysmon = Tool to collect detailed logs

### Steps:

1. Open Event Viewer
2. Go to Sysmon logs
3. Clear old logs (only in lab)
4. Run Atomic test

---

## 6. Execute Atomic Test

Invoke-AtomicTest T1547.001 -TestNumbers 1

---

## 7. Observing Logs

Total logs generated: 5

### Ignore:

* whoami.exe
* hostname.exe

👉 These are not part of attack

---

### Important Logs:

#### 🔹 Process Create (Event ID 1)

* Shows program execution
* Example:

  * reg.exe executed
  * via cmd.exe

---

#### 🔹 Registry Value Set (Event ID 13)

* Shows registry modification

---

## 8. Attack Summary (From Logs)

* reg.exe modifies registry

* Registry path:
  \SOFTWARE\Microsoft\Windows\CurrentVersion\Run

* Value points to:
  C:\Path\AtomicRedTeam.exe

👉 Meaning:
Malware will run automatically at login

---

## 9. SOC Detection Points

SOC should detect:

* reg.exe execution
* cmd.exe spawning reg.exe
* Registry Run key modification
* Suspicious file path

---

## 10. Aurora EDR (Detection Tool)

Aurora = Endpoint Detection Tool

👉 Uses:

* Sigma rules
* IOC matching

---

## 11. Running Aurora

1. Navigate to Aurora folder
2. Start aurora-agent

👉 Wait until agent starts

---

## 12. Execute Next Atomic Test

Invoke-AtomicTest T1547.001 -TestNumbers 2

---

## 13. Detection Output

Aurora shows:

* Alert name
* Description
* Command used
* Process details
* Hash values

---

## 14. Learning Outcome

Using Atomic Red Team, we can:

* Simulate attack
* Observe logs
* Test detection rules

---

## 15. SOC Workflow

1. Run attack (Atomic)
2. Collect logs (Sysmon)
3. Detect alerts (EDR/SIEM)
4. Improve rules

---

## ⚠️ Important Note

* Do not clear logs in real environment
* Understand expected logs before testing

---

## ⭐ Quick Revision

* Telemetry = logs
* Sysmon = log collection
* Aurora = detection tool
* Registry Run Key = persistence method
* Event ID 1 = process create
* Event ID 13 = registry change

---

## 🧠 One-Line Summary

Atomic Red Team helps SOC teams simulate attacks, observe logs, and test detection tools to improve security monitoring.

---
# 🛡️ Custom Atomic Tests & Input Arguments 

## 1. Key Idea

Sometimes Atomic Red Team is not enough.

👉 Reasons:

* Technique has no Atomic
* Organization has custom setup
* Need more realistic testing

---

## 2. Solution

We can:

* Use custom input arguments
* Create our own Atomic tests

---

## 3. What are Input Arguments?

Input arguments = Values used inside an Atomic test

👉 Example:

* username
* password

---

## 4. Example Technique

Technique: T1136.001 (Create Local Account)

### Default Values:

* username = T1136.001_CMD
* password = T1136.001_CMD!

---

## 5. Problem

Default password may fail due to policy.

👉 Result:
User not created

---

## 6. Solution → Custom Inputs

### Option 1: PromptForInputArgs

Invoke-AtomicTest T1136.001 -TestNumbers 3 -PromptForInputArgs

👉 User enters values manually

---

### Option 2: InputArgs (Recommended)

$customArgs = @{ "username" = "THM_Atomic"; "password" = "p@ssw0rd" }
Invoke-AtomicTest T1136.001 -TestNumbers 3 -InputArgs $customArgs

👉 Pass custom values directly

---

## 7. Result

* New user created successfully
* Can verify using:
  net user

---

## 8. Cleanup with Custom Inputs

Invoke-AtomicTest T1136.001 -TestNumbers 3 -PromptForInputArgs -Cleanup

👉 Removes created user

⚠️ Same input values must be used for cleanup

---

## 9. Limitation

* Not all Atomics support input arguments
* Only those with input_arguments field can be customized

---

## 10. Creating Custom Atomic Tests

If Atomic does not exist, create your own.

---

## 11. Tool: Atomic GUI

Start tool:
Start-AtomicGUI

Open in browser:
http://localhost:8487/home

---

## 12. What to Fill in Atomic GUI

* Test name and description
* Supported platform
* Attack command
* Executor (cmd / PowerShell)
* Cleanup command
* Input arguments
* Prerequisites

---

## 13. Output

* Generates YAML code
* Paste into Atomic technique file

---

## 14. Important Note

* YAML formatting must be correct
* Proper spacing is required

---

## 15. SOC Perspective (Important)

Custom Atomics help SOC to:

* Test organization-specific attacks
* Improve detection rules
* Simulate real-world scenarios

---

## ⭐ Quick Revision

* InputArgs = customize attack values
* PromptForInputArgs = manual input
* Default values may fail
* Cleanup needs same inputs
* Atomic GUI = create new tests

---

## 🧠 One-Line Summary

Custom inputs and custom Atomics allow SOC teams to simulate realistic and organization-specific attacks for better detection testing.

---
