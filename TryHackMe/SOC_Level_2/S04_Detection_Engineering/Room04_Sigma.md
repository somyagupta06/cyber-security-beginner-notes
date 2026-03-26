# Sigma Notes 

## 1. What is the problem in SOC?

In SOC, analysts:

* collect logs
* check logs
* try to find attacks

Problem:

* Every SIEM tool is different (Splunk, ELK, QRadar)
* Each tool uses a different query language
* Hard to share detection rules with others

So, same detection must be written again and again.

---

## 2. What is Sigma?

Sigma is:

* an open-source detection rule language
* used to describe suspicious log activity
* written in a simple format (YAML)

Simple meaning:
Sigma = one common way to write detection rules for logs

---

## 3. Why do we use Sigma?

Sigma helps SOC analysts to:

* write detection rules once
* use them in different SIEM tools
* share rules with other analysts
* standardize detections

---

## 4. Easy Example

Goal: Detect suspicious command execution

Without Sigma:

* Splunk → write one query
* ELK → write another query
* QRadar → write another query

With Sigma:

* Write one Sigma rule
* Convert it for all SIEMs

---

## 5. How Sigma Works (Step-by-Step)

Step 1: Logs are collected
Example:

* Windows logs
* Firewall logs
* Endpoint logs

Step 2: Analyst finds suspicious activity
Example:

* strange PowerShell usage
* unknown login

Step 3: Analyst writes Sigma rule

* define what to detect
* define conditions

Step 4: Sigma Converter

* converts Sigma rule into SIEM queries

Step 5: SIEM runs the query

* if match found → alert generated

---

## 6. Sigma Rule Format (Basic Idea)

Sigma rules are written in YAML.

They include:

* title → name of detection
* description → what it detects
* logsource → where logs come from
* detection → condition to match
* level → severity

---

## 7. Simple Comparison

| Tool  | Purpose                        |
| ----- | ------------------------------ |
| Snort | Detect network traffic attacks |
| Yara  | Detect malicious files         |
| Sigma | Detect suspicious log events   |

---

## 8. SOC Use Cases of Sigma

* Detect attacker behavior from logs
* Share detection rules with team
* Improve threat detection
* Use same rule across tools

---

## 9. Key Benefits

* No vendor dependency
* Easy to share
* Standard format
* Saves time

---

## 10. One Line Summary

Sigma is a common rule language that helps SOC analysts detect threats from logs and use the same rule across different SIEM tools.

---
# Sigma Syntax Notes 

---

## 1. YAML Basics (Used in Sigma)

* YAML is human-readable
* File extension → `.yml`
* Case-sensitive
* Use **spaces**, not tabs
* `#` → comment
* `:` → key and value
* `-` → list

### Example

```
name: test
value: 10
list:
  - one
  - two
```

---

## 2. Sigma Rule = What?

A Sigma rule tells:

→ what suspicious activity to detect in logs

---

## 3. Important Fields (Must Know)

---

### 3.1 Title

```
title: Suspicious Activity
```

* Name of the rule
* Should be short and clear

---

### 3.2 ID

```
id: unique-id
```

* Unique identifier
* Used to track rules

---

### 3.3 Status

```
status: test
```

Types:

* stable → ready to use
* test → testing stage
* experimental → may give false alerts
* deprecated → old rule
* unsupported → not usable

---

### 3.4 Description

```
description: Detects suspicious behavior
```

* Explains what rule does
* Helps during investigation

---

### 3.5 Logsource (VERY IMPORTANT)

```
logsource:
  product: windows
  category: process_creation
```

* Defines where logs come from

Fields:

* product → system (windows, linux)
* category → type of logs
* service → specific logs

---

### 3.6 Detection (MOST IMPORTANT)

```
detection:
  selection:
    EventID:
      - 19
      - 20
      - 21
  condition: selection
```

* Defines what to search
* Defines when alert should trigger

---

## 4. Lists vs Maps

---

### List → OR logic

```
EventID:
  - 19
  - 20
```

Meaning:
→ EventID = 19 OR 20

---

### Map → AND logic

```
Image: cmd.exe
User: admin
```

Meaning:
→ Image = cmd.exe AND User = admin

---

## 5. Value Modifiers

Used with `|`

---

### contains

```
CommandLine|contains: powershell
```

→ value can appear anywhere

---

### endswith

```
Image|endswith: cmd.exe
```

→ value must be at end

---

### startswith

```
CommandLine|startswith: power
```

→ value must be at start

---

### all

* Changes OR → AND in lists

---

## 6. Condition (Logic of Rule)

---

### Simple

```
condition: selection
```

→ match selection

---

### AND condition

```
condition: tools and filter
```

→ both must match

---

### NOT condition

```
condition: selection and not 1 of filter_*
```

→ match selection
→ ignore filters

---

## 7. False Positives

```
falsepositives:
  - admin activity
```

* Legit activity that may trigger alert
* Helps reduce noise

---

## 8. Level (Severity)

```
level: medium
```

Levels:

* informational
* low
* medium
* high
* critical

---

## 9. Tags

```
tags:
  - attack.persistence
  - attack.t1546.003
```

* Used for classification
* Based on MITRE ATT&CK

---

## 10. Important Concept (VERY IMPORTANT)

Detection section has 2 parts:

1. Selection → what to search
2. Condition → how to match

---

## 11. Easy Flow (SOC Perspective)

1. Choose logs (logsource)
2. Define suspicious activity (detection)
3. Apply logic (condition)
4. Set severity (level)
5. Handle noise (falsepositives)

---

## 12. One Line Revision

Sigma is a rule format that helps SOC analysts detect threats in logs using a standard and shareable method.

---
# Sigma Rule Writing Notes 

---

## 1. What is this topic?

This is about:
→ how a SOC analyst writes a Sigma rule using threat intelligence

---

## 2. SOC Thinking Process (VERY IMPORTANT)

When you get threat intel:

1. Understand attacker behavior
2. Pick important indicators
3. Map them to logs
4. Write detection rule

---

## 3. Scenario Summary (AnyDesk Attack)

Attacker:

* downloads AnyDesk
* installs it silently
* gets remote access

Goal:
→ Detect AnyDesk installation in logs

---

## 4. Step 1: Intel Analysis

Pick important values from intel:

### Commands used

```
--install
--start-with-win
--silent
```

### File location

```
C:\ProgramData\AnyDesk.exe
```

### Other behavior

* creates user (persistence)
* modifies registry

---

## 5. Step 2: Rule Identification

```
title: AnyDesk Installation
status: experimental
description: Detects AnyDesk installation
```

* Title → what rule detects
* Status → testing stage
* Description → explanation

---

## 6. Step 3: Logsource

```
logsource:
  product: windows
  category: process_creation
```

* We use process_creation logs
* Because installation = process execution

---

## 7. Step 4: Detection (MOST IMPORTANT)

```
detection:
  selection:
    CommandLine|contains|all:
      - '--install'
      - '--start-with-win'
    CurrentDirectory|contains:
      - 'C:\ProgramData\AnyDesk.exe'
  condition: selection
```

### Meaning:

* CommandLine must contain:

  * --install
  * --start-with-win

* AND directory must contain:

  * AnyDesk.exe path

→ If all match → alert

---

## 8. Step 5: Metadata

### False Positives

```
falsepositives:
  - Legitimate AnyDesk installation
```

→ Sometimes normal usage can trigger alert

---

### Level

```
level: high
```

→ High severity (remote access risk)

---

### Tags

```
tags:
  - attack.command_and_control
  - attack.t1219
```

→ MITRE ATT&CK mapping

---

### References

```
references:
  - threat intel source link
```

→ Source of information

---

## 9. Final Rule Flow

1. Analyze intel
2. Extract important patterns
3. Choose logsource
4. Write detection logic
5. Add metadata

---

## 10. Rule Conversion (IMPORTANT)

Sigma rule must be converted before use

---

### Tool 1: Sigmac

```
python3.9 sigmac -t splunk -c splunk-windows file.yml
```

→ Converts Sigma to Splunk query

---

### Tool 2: Uncoder.io

Steps:

1. Paste Sigma rule
2. Select SIEM
3. Get query

---

## 11. Real SOC Issue

Converted queries may need fixes:

* remove extra *
* fix slashes ()
* adjust fields

---

## 12. Testing in SIEM (Kibana)

* Paste converted query
* Search logs
* Check if AnyDesk activity exists

---

## 13. One Line Revision

Sigma rule writing = converting attacker behavior into log detection logic.

---
## Lab Questions
### Q.1 At what time was the AnyDesk installation event created? [MMM DD, YYYY @ HH:MM:SS]
<img width="1470" height="956" alt="Screenshot 2026-03-26 at 12 10 38 PM" src="https://github.com/user-attachments/assets/c71923e7-b5b5-4088-a58d-0a82d3a2aaef" />

### Q.2 What version of AnyDesk was installed?
<img width="1470" height="956" alt="Screenshot 2026-03-26 at 12 13 25 PM" src="https://github.com/user-attachments/assets/a259b74e-af21-45f2-9bde-0990d5d42031" />
# Sigma Rules + Elastic Queries (SOC Use) – Very Easy English

---

# 🔴 1. Scheduled Task Detection Rule

## ✅ Sigma Rule

```yaml id="schtask-rule"
title: Suspicious Scheduled Task Creation
status: experimental
description: Detects suspicious scheduled task creation using schtasks.exe

logsource:
  product: windows
  category: process_creation

detection:
  selection:
    Image|endswith: schtasks.exe
    CommandLine|contains:
      - '/create'
  filter:
    User: SYSTEM
  condition: selection and not filter

falsepositives:
  - Legitimate admin activity

level: medium

tags:
  - attack.persistence
  - attack.t1053
```

---

## ✅ Elastic Query (Use in Kibana)

```id="schtask-query"
process.name:schtasks.exe AND process.command_line:*create* AND NOT user.name:SYSTEM
```

---

# 🔴 2. Ransomware Activity Detection Rule

## ✅ Sigma Rule

```yaml id="ransom-rule"
title: Suspicious Ransomware File Creation
status: experimental
description: Detects possible ransomware activity via cmd creating .txt files

logsource:
  product: windows
  category: process_creation

detection:
  selection:
    Image|endswith: cmd.exe
    CommandLine|contains:
      - '.txt'
  condition: selection

falsepositives:
  - Legitimate script usage

level: high

tags:
  - attack.impact
  - attack.t1486
```

---

## ✅ Elastic Query (Use in Kibana)

```id="ransom-query"
process.name:cmd.exe AND process.command_line:*.txt*
```

---

# 🔥 Important Notes (VERY IMPORTANT)

* Always set time range → **Last 1 year**
* Field names may vary:

  * process.name OR Image
  * process.command_line OR CommandLine
* If query not working → try small changes

---

# 🎯 One Line Revision

* Scheduled Task → detect schtasks.exe
* Ransomware → detect cmd.exe creating .txt files

---
# Sigma Practical Investigation Notes 

---

## 1. Key Concept

* Same Sigma rule → different queries from tools
* Tools:

  * Sigmac
  * Uncoder.io

→ Output may differ because of field names and configs

---

## 2. Important SOC Lesson

* Do NOT trust tools blindly
* Always adjust query based on your SIEM

Example:

```id="ex1"
Image.keyword vs process.executable
CommandLine vs process.command_line
```

→ Same meaning, different fields

---

## 3. Scenario Summary (Aurora Case)

Suspicious activities:

* Scheduled tasks created
* Ransomware activity detected

Goal:

→ Detect both using Sigma + Kibana

---

## 4. Investigation Approach (SOC Flow)

1. Understand attack
2. Identify log type
3. Write Sigma rule
4. Convert to Elastic query
5. Run in Kibana
6. Analyze results

---

## 5. Rule 1: Scheduled Task Detection

---

### Log Type

* process_creation logs

---

### Key Indicators

* schtasks.exe
* command contains "create"
* exclude SYSTEM user

---

### Sigma Detection Idea

```id="schtask"
Image → schtasks.exe  
CommandLine → create  
User ≠ SYSTEM  
```

---

### Elastic Query

```id="schtask-q"
process.name:schtasks.exe AND process.command_line:*create* AND NOT user.name:SYSTEM
```

---

## 6. Rule 2: Ransomware Detection

---

### Log Type

* process_creation logs

---

### Key Indicators

* cmd.exe used
* .txt file creation

---

### Sigma Detection Idea

```id="ransom"
Image → cmd.exe  
CommandLine → .txt  
```

---

### Elastic Query

```id="ransom-q"
process.name:cmd.exe AND process.command_line:*.txt*
```

---

## 7. Important Kibana Step

* Change time range → **Last 1 year**

Reason:

* Attack may be old (e.g., 2022)

---

## 8. Common Issues (Real SOC)

---

### Problem 1: No results

* Fix time range
* Check field names

---

### Problem 2: Query not working

* Remove extra `*`
* Fix slashes `\`

---

### Problem 3: Different outputs

* Choose correct fields for your SIEM

---

## 9. Field Name Mapping (Helpful)

| Sigma Field | Elastic Field        |
| ----------- | -------------------- |
| Image       | process.name         |
| CommandLine | process.command_line |

---

## 10. Final Understanding

* Sigma = detection logic
* Conversion = SIEM query
* Kibana = investigation tool

---

## 11. One Line Revision

Sigma helps convert attacker behavior into searchable queries in logs.

---

## Lab Questions

### Q.2 What was the name of the scheduled task created?
### Q.3 What time is this task meant to run?
<img width="1470" height="956" alt="Screenshot 2026-03-26 at 12 55 51 PM" src="https://github.com/user-attachments/assets/bef4c053-deba-4a7c-a02a-2db9e4f8e3a7" />

### Q.5 What is the name of the created file?
<img width="1470" height="956" alt="Screenshot 2026-03-26 at 12 49 39 PM" src="https://github.com/user-attachments/assets/758268c6-1e4b-4dcf-9942-d1daf26e9ab7" />

### Q.6 What were the contents of the created ransomware file?
<img width="1470" height="956" alt="Screenshot 2026-03-26 at 12 54 11 PM" src="https://github.com/user-attachments/assets/11762fc0-fa65-4e10-9748-766665bfa506" />



