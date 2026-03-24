# Follina (CVE-2022-30190) 

## 1. What is this vulnerability?

Follina is a **Remote Code Execution (RCE)** vulnerability.

It happens when:

* Microsoft Word calls **MSDT (Microsoft Support Diagnostic Tool)**
* Using a special link: `ms-msdt:/`

### Simple meaning:

A Word file can make your system run commands **without your permission**.

---

## 2. What is MSDT?

MSDT is a Windows tool.

Purpose:

* Collect system information
* Help Microsoft fix issues

### Example:

Like a mechanic tool that checks your car and creates a report.

---

## 3. What went wrong?

Attackers misused MSDT.

Instead of fixing problems:

* They made MSDT run **malicious commands**

---

## 4. Why is this attack dangerous?

| Feature           | Explanation                                  |
| ----------------- | -------------------------------------------- |
| No macros needed  | User does not need to click "Enable Content" |
| Looks normal      | File appears safe (.docx)                    |
| Uses trusted tool | MSDT is a legitimate Windows tool            |
| Hard to detect    | EDR initially failed                         |

---

## 5. Simple Attack Flow

1. User opens a Word file
2. Word loads external content (hidden link)
3. Link uses `ms-msdt:/`
4. MSDT is launched
5. MSDT executes attacker commands

### Final Result:

Attacker can:

* Install programs
* Steal or delete data
* Create accounts

---

## 6. Timeline (Very Simple)

| Date       | What happened                                |
| ---------- | -------------------------------------------- |
| 2020       | Research showed MSDT can be abused           |
| 2021       | Microsoft partially fixed issue (Teams only) |
| April 2022 | Real attacks seen in the wild                |
| May 2022   | Publicly identified as zero-day              |
| June 2022  | Microsoft released official patch            |

---

## 7. What is a Zero-Day?

A zero-day means:

* Vulnerability is being exploited
* But no fix is available yet

---

## 8. SOC Analyst Perspective

### What should you focus on?

#### 1. Process behavior

Look for:

* WINWORD.EXE → MSDT.EXE
* MSDT.EXE → PowerShell

#### 2. Command line activity

Check:

* Suspicious parameters
* Encoded commands

#### 3. PowerShell logs

Look for:

* Script execution
* Obfuscated commands

---

## 9. Key Red Flags

* Word spawning MSDT
* MSDT spawning PowerShell
* Base64 encoded strings
* External network connections

---

## 10. Detection Strategy

SOC should use:

* Process creation logs (Event ID 4688)
* Command-line logging
* PowerShell script block logging

---

## 11. Mitigation (Defense)

### 1. Apply Patch

Install June 2022 Windows updates

### 2. Disable MSDT Protocol

Remove `ms-msdt` from registry

### 3. Use ASR Rules

Block:
Office applications → creating child processes

---

## 12. Final SOC Summary

* This attack does NOT need macros
* It abuses a trusted Windows tool (MSDT)
* Detection depends on behavior, not file

### One-line understanding:

A normal-looking Word file tricks Windows into running malicious code using MSDT.

---
# Follina (MSDT Exploit) 

## 1. What is MSDT (again, very simple)?

MSDT is a Windows tool.

Purpose:

* Collect system information
* Send it to Microsoft Support

### Simple example:

It is like a tool that checks your computer and creates a report for fixing problems.

---

## 2. Important SOC Concept: Baseline

Before detecting attacks, you must know what is **normal**.

### Baseline means:

* Normal processes running on system
* No attack activity

### Why important?

If you don’t know normal:
→ Everything will look suspicious
→ You will waste time

---

## 3. What to do in lab?

Open Process Explorer and observe:

Normal processes like:

* explorer.exe
* svchost.exe
* services.exe

👉 This is your **baseline**

---

## 4. Core Idea of Exploit

Two things are used:

### 1. OLE Object (inside Word file)

* Word file can load external content (HTML)

### 2. MSDT Tool

* Can execute commands

---

## 5. How attack works (simple flow)

1. Word file contains hidden link (OLE object)
2. Link points to attacker HTML file
3. Word loads that HTML automatically
4. HTML triggers `ms-msdt:/`
5. MSDT runs attacker command

---

## 6. Key Technical Trick (simple understanding)

Inside Word file:

* There is a reference to external URL

Example idea:

```
Word → load http://attacker/payload.html
```

Inside HTML:

```
Redirect to ms-msdt
```

---

## 7. What does attacker gain?

Attacker can:

* Run PowerShell
* Execute commands
* Download malware

Example:

```
calc.exe (demo)
```

Real case:

* Remote shell
* Data theft

---

## 8. Important SOC Detection Point

### Process Chain

Look for this:

```
WINWORD.EXE
   ↓
msdt.exe
   ↓
powershell.exe
```

👉 This is NOT normal

---

## 9. Why baseline helps?

Without baseline:

* You see many processes → confusion

With baseline:

* You quickly detect abnormal chain

Example:

```
Normal: explorer → chrome  
Abnormal: word → msdt → powershell
```

---

## 10. "Zero Click" Attack (VERY IMPORTANT)

### What normally happens?

User opens file → attack runs

---

### What happens here?

User does NOT open file

Just clicks once (preview)

---

### Why?

Because:

* File Explorer preview loads document
* RTF allows preview rendering
* Word engine runs in background

---

### Result:

```
No open
No macro
Still attack runs
```

---

## 11. SOC Key Learning

* Attack does not need user interaction
* Trusted tools are abused (MSDT)
* Detection = behavior, not file

---

## 12. Final Summary

* Word file loads external HTML
* HTML triggers MSDT
* MSDT runs malicious code
* Attack can happen without opening file

### One-line:

A Word file tricks the system into executing code using MSDT, even without user action.

---
# Follina (MSDT) — Threat Hunting & Defense Notes 

---

## 1. Why Logging is Important

To detect attacks, we need logs.

### Enable these logs:

* Process Creation Logging
* Command Line Logging
* PowerShell Script Block Logging

### Why?

They help us:

* See what process started
* See what command was used
* See what PowerShell executed

---

## 2. Key Log to Use

### Event ID: 4688

This log shows:

* New process created
* Parent process
* Command line

---

## 3. Hunting Steps (Simple)

### Step 1: Filter logs

Search:

```
Event ID = 4688
```

---

### Step 2: Find Word process

Search:

```
WINWORD.EXE
```

👉 This shows when Word file was opened

---

### Step 3: Look for suspicious child process

Check if:

```
WINWORD.EXE → msdt.exe
```

🚨 This is NOT normal

---

### Step 4: Check command line

Look for:

* Long strange commands
* PowerShell keywords
* `../../` (directory traversal)
* Encoded strings

Example:

```
Y2FsYw== → calc
```

---

## 4. Pivot to PowerShell Logs

Since PowerShell is involved:

Filter logs by:

```
Provider = PowerShell
```

---

### What to look for?

* Script execution
* Encoded commands
* Suspicious functions (IEX)

---

## 5. Attack Chain (Log View)

```
WINWORD.EXE
   ↓
msdt.exe
   ↓
powershell.exe
   ↓
payload executed
```

---

## 6. Key Detection Points

* Word spawning MSDT
* MSDT spawning PowerShell
* Encoded payloads
* External connections

---

## 7. Sigma Rule (Detection Automation)

Sigma = detection rule

Used for:

* Real-time alerts
* Hunting past attacks

---

## 8. Mitigation (Defense)

### 1. Apply Patch

Install June 2022 Windows update

---

### 2. Disable MSDT Protocol

Remove:

```
ms-msdt
```

Result:

* Word cannot call MSDT
* Attack stops

---

### 3. ASR Rule (Very Important)

Enable:

```
Block Office applications from creating child processes
```

---

### Why?

Because attack uses:

```
Word → msdt → PowerShell
```

ASR blocks this chain

---

## 9. SOC Strategy

### Detect:

* Use logs (4688 + PowerShell)

### Hunt:

* Search for msdt usage
* Check parent-child processes

### Prevent:

* Patch system
* Disable protocol
* Use ASR rules

---

## 10. Final Summary

* Attack uses Word + MSDT
* No macros required
* Detection is based on behavior

### One-line:

Monitor process chains and block abnormal behavior to detect and stop Follina attacks.

---
