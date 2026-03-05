# File and Hash Intelligence
## SOC L1 Analyst Notes (Simple Revision)

Security Operations Centre (SOC) analysts spend most of their time **triaging alerts**.  
Every alert investigation usually follows three main steps.

Verify → Enrich → Decide

| Step | Meaning |
|---|---|
Verify | Confirm if the alert is real or a false positive |
Enrich | Collect additional context about the indicator |
Decide | Take action such as escalate, block, or close |

File and hash intelligence is mainly used in the **Enrich phase** of alert investigation.

Example alert

Suspicious file detected: Setup.exe

The analyst must gather more information about this file before deciding the response.

---

# File Investigation in SOC

When a suspicious file appears in logs or alerts, analysts investigate:

- file path
- filename
- file hash
- behaviour

These indicators help determine whether the file is **benign or malicious**.

---

# Filepath Analysis

File paths can reveal attacker behaviour.  
Analysts treat them like **clues at a crime scene**.

Attackers often store malware in locations that:

- avoid monitoring
- allow access across users
- blend with normal activity

---

## Common Suspicious Locations

### System Drive

C:\

Attackers sometimes drop files directly in the system drive for persistence.

Example

C:\update.exe

---

### Public User Directory

C:\Users\Public

Reason

All users on the system can access this folder.

Example

C:\Users\Public\tool.exe

---

### Public Downloads Folder

C:\Users\Public\Public Downloads

This folder often contains many files, so malware may hide among legitimate downloads.

---

### Temporary Directory

C:\Windows\Temp

Attackers frequently use temporary folders for short-lived payloads.

Example

C:\Windows\Temp\payload.exe

Temporary files may receive less monitoring.

---

### ProgramData Directory

C:\ProgramData

This directory is writable and often used for stealth persistence.

Example

C:\ProgramData\service.exe

---

# Filename Heuristic Indicators

Attackers often manipulate filenames to bypass detection.

SOC analysts use **heuristic analysis** to identify suspicious patterns.

Heuristics do not prove malicious activity but provide strong indicators.

---

## Double Extension

Example

invoice.pdf.exe

Users may see only "invoice.pdf" if Windows hides extensions.

Actual file type

Executable malware.

This technique is commonly used in phishing campaigns.

---

## System Binary Impersonation

Attackers mimic legitimate system processes.

Example

scvhost.exe

The legitimate system process is

svchost.exe

Difference

One character is changed.

SOC analysts should verify both the **filename and file location**.

Example of legitimate location

C:\Windows\System32\svchost.exe

If the same filename appears in a different location, it may be malicious.

Example

C:\Users\Public\svchost.exe

---

## High Entropy Filenames

Example

jh8F21.exe

Random-looking names often indicate:

- automatically generated malware
- packed executables
- polymorphic malware samples

These names are frequently seen in phishing campaigns.

---

## Masquerading

Attackers may use filenames that appear normal or harmless.

Example

backup-2300.exe

The file name looks legitimate but may actually be malware.

---

### Character Substitution

Example

exp1orer.exe

The number "1" replaces the letter "l".

Legitimate file

explorer.exe

This trick allows malware to appear visually similar to trusted processes.

---

# SOC Investigation Example

Alert

File detected

C:\Windows\Temp\invoice.pdf.exe

SOC L1 investigation steps

Step 1

Path analysis

Temp directory used.

Step 2

Filename analysis

Double extension detected.

Step 3

Heuristic assessment

Possible phishing malware.

Step 4

File enrichment

Check file hash using intelligence platforms.

Example tools

VirusTotal  
MalwareBazaar

Step 5

Decision

Quarantine the file or escalate to Level 2.

---

# Key SOC L1 Skills for File Analysis

SOC analysts should be able to:

Interpret suspicious file paths  
Recognise suspicious filenames  
Generate and validate file hashes  
Use threat intelligence platforms for enrichment  
Map observed behaviour to MITRE ATT&CK techniques

---
# Lab Questions
## Q.1 One file displays one of the indicators mentioned. Can you identify the file and the indicator? (Answer: file, property)

<img width="691" height="240" alt="Screenshot 2026-03-05 at 1 30 38 PM" src="https://github.com/user-attachments/assets/1f490385-d614-468c-a969-9cbc38727d0d" />


# Quick Revision Summary

Important indicators during file investigation

| Indicator | Purpose |
|---|---|
File Path | Identify suspicious storage locations |
Filename | Detect attacker evasion techniques |
File Hash | Verify file reputation |
Behaviour | Identify malicious activity |

SOC analysts use these indicators to determine whether a file is **malicious, suspicious, or benign** during alert triage.

---
# File Hash Intelligence for SOC Analysts

File hashes provide a unique fingerprint for a file.  
Even if attackers rename malware files, the hash remains the same unless the file is modified.

SOC analysts use file hashes to identify malicious binaries and correlate them across systems.

---

# What is a File Hash

A file hash is a cryptographic value calculated from the contents of a file.

Common hash algorithms

MD5  
SHA1  
SHA256

Example SHA256 hash

4a7d1ed414474e4033ac29ccb8653d9b

Hashes uniquely identify files regardless of filename changes.

---

# Why Hashes are Important

Attackers frequently rename malware files to evade detection.

Example

setup.exe  
invoice.exe  
backup.exe

Even if the filename changes, the hash remains the same.

Hashes allow SOC analysts to track malware across different systems.

---

# Generating File Hashes

Windows Command Prompt

certutil -hashfile blogger.exe SHA256

PowerShell

Get-FileHash -Algorithm SHA256 blogger.exe

Linux

sha256sum blogger.exe

---

# Hash Investigation Best Practices

Store hashes in lowercase to avoid formatting differences.

Hash both compressed archives and extracted files if malware is delivered inside a zip file.

Always document the context of the hash.

Example

SHA256 hash of suspicious file from finance server alert.

Any modification in the file changes the hash value.

---

# VirusTotal Analysis

VirusTotal allows analysts to investigate files using their hash.

Key elements to analyze

---

## Detection Score

Shows how many antivirus engines detect the file.

Example

45/70

Higher detection ratios indicate a higher probability of malicious activity.

---

## Threat Labels

Vendor classifications describing the malware type.

Examples

Trojan  
Backdoor  
Ransomware

---

## Detection Rules

Technical signatures used by antivirus engines.

Examples

YARA rules  
Heuristic detection  
Behavioral triggers

---

## File Properties

Metadata about the file.

Includes

file type  
file size  
compile timestamp

Unusual timestamps or high entropy values may indicate packed malware.

---

## Network Infrastructure

VirusTotal may list domains or IP addresses used by the malware.

These indicators can be added to blocklists.

---

## Dropped Files

Malware may create additional files during execution.

These artifacts should also be investigated.

---

# MalwareBazaar

MalwareBazaar is a malware intelligence platform containing malware samples and related threat data.

It is commonly used to cross-reference VirusTotal findings.

---

# MalwareBazaar Capabilities

Malware family tagging

Examples

#IcedID  
#Emotet  
#TrickBot

YARA rule integration

Rules that detect related malware samples.

Campaign attribution

Tags identifying threat actors.

Example

#TA551

Malware sample downloads

Samples can be downloaded for sandbox analysis.

---

# Searching MalwareBazaar

To search using a SHA256 hash

sha256:<file_hash>

Example

sha256:4a7d1ed414474e4033ac29ccb8653d9b

---
# Lab Questions
## Q.1 What is the SHA256 hash of the file bl0gger?
- certutil -hashfile bl0gger.exe SHA256

## Q.2 On VirusTotal, what is the threat label used to identify the malicious file?
<img width="1470" height="956" alt="Screenshot 2026-03-05 at 1 32 36 PM" src="https://github.com/user-attachments/assets/0bf1fde5-600f-41dd-9cb7-d6345e56bfd8" />

## Q.3 When was the file first submitted for analysis? (Answer format: YYYY-MM-DD HH:MM:SS)
<img width="1470" height="956" alt="Screenshot 2026-03-05 at 1 33 22 PM" src="https://github.com/user-attachments/assets/091ebbde-e453-4125-85d0-457274a9a587" />

## Q.4 According to MalwareBazaar, which vendor classified the Morse-Code-Analyzer file as non-malicious?
- get the file hash of the file Morse-Code-Analyzer.exe
- search in MalwareBazaar
- sha256:1f8806869616c18cbae9ffcf581c0428915d32fb70119df16d08078d92d1a5e3
<img width="1470" height="956" alt="Screenshot 2026-03-05 at 1 37 10 PM" src="https://github.com/user-attachments/assets/4569c86c-cdad-4e31-b107-cc77aca57cd4" />
## Q.5 On VirusTotal, what MITRE technique has been flagged for persistence and privilege escalation for the Morse-Code-Analyzer file?
<img width="1470" height="956" alt="Screenshot 2026-03-05 at 1 39 17 PM" src="https://github.com/user-attachments/assets/e5191877-6608-4d4d-bbdf-95499639dac2" />


# SOC Investigation Workflow

Alert triggered for suspicious file.

Generate file hash.

Check hash reputation in VirusTotal.

Cross-reference results in MalwareBazaar.

Identify malware family and related infrastructure.

Block malicious indicators and escalate the incident.
---
# Dynamic Analysis for SOC Analysts

Dynamic analysis is the process of executing a suspicious file in a controlled environment to observe its behaviour.

Unlike static analysis, which examines file properties without execution, dynamic analysis reveals how malware behaves during runtime.

Security analysts perform dynamic analysis using sandbox environments.

---

# What is a Sandbox

A sandbox is an isolated virtual machine used to safely execute malware.

The sandbox records system activity during execution, including:

process creation  
registry modifications  
network connections  
file creation or deletion

This allows analysts to understand the real impact of malicious software.

---

# Objectives of Sandbox Analysis

SOC analysts use sandboxes for three main purposes.

---

## Confirm Execution

Analysts verify whether the suspicious file actually executes malicious behaviour.

If no activity occurs, the file may be a decoy or inactive sample.

---

## Extract Runtime Indicators of Compromise (IOCs)

Sandbox execution reveals indicators that can be used for threat detection.

Examples include:

malicious domains  
command and control IP addresses  
mutex names  
dropped files  
registry persistence keys

These indicators can be used to improve SIEM or EDR detections.

---

## Map Behaviour to MITRE ATT&CK

Many sandbox tools automatically map observed behaviour to MITRE ATT&CK techniques.

Examples include:

T1059 Command execution  
T1547 Persistence  
T1071 Command and Control

This mapping helps analysts quickly understand the attack techniques used.

---

# Popular Malware Sandboxes

Two commonly used sandbox platforms are Hybrid Analysis and Joe Sandbox.

---

## Hybrid Analysis

Hybrid Analysis focuses on behaviour summaries and MITRE ATT&CK visualizations.

Key features include:

behaviour trees  
threat score  
MITRE ATT&CK heatmaps

It is commonly used for quick threat assessments.

---

## Joe Sandbox

Joe Sandbox provides deeper technical analysis.

Capabilities include:

system call analysis  
memory dumps  
detailed behaviour logs

It is frequently used by malware analysts and detection engineers.

---

# Key Information from Sandbox Reports

When reviewing sandbox results, analysts should examine:

File details such as type, size, and compile timestamp.

Process activity to identify suspicious program execution.

Network connections to detect command and control infrastructure.

Dropped files created during malware execution.

Registry modifications indicating persistence techniques.

---

# Sandbox Limitations

Sandbox analysis has several limitations.

---

## Sandbox Evasion

Some malware can detect virtual environments and hide malicious activity.

Common techniques include:

virtual machine detection  
anti-debugging checks  
hardware fingerprint validation

---

## Limited Execution Time

Most sandbox environments run malware for only a few minutes.

Malware designed to activate after longer delays may evade detection.

---

## Encrypted Network Traffic

Many sandbox environments cannot decrypt encrypted traffic.

Malware may hide command and control communication using HTTPS or DNS tunnelling.

---

## Fileless Malware

Some attacks do not rely on files stored on disk.

Examples include:

PowerShell-based attacks  
WMI persistence techniques

These threats may bypass traditional sandbox detection.

---

# SOC Investigation Workflow

Alert triggered for suspicious file.

Perform file path and filename analysis.

Generate file hash.

Check reputation in VirusTotal.

Cross-reference intelligence in MalwareBazaar.

Execute the sample in a sandbox environment.

Extract runtime indicators and map behaviour to MITRE ATT&CK techniques.

Update security detections and escalate if required.

# Lab Questions
## Q.1 What tags are used to identify the bl0gger.exe malicious file on Hybrid Analysis? (Answer: Tag1, Tag2, Tag3)
<img width="1470" height="956" alt="Screenshot 2026-03-05 at 1 51 47 PM" src="https://github.com/user-attachments/assets/d73d191a-8166-45fa-a54d-ef205bba1f0f" />

## Q.2 What was the stealth command line executed from the file?
<img width="1470" height="956" alt="Screenshot 2026-03-05 at 1 53 46 PM" src="https://github.com/user-attachments/assets/7c57e95f-506a-4b51-a8ec-68d57049c734" />
## Q.3 Which other process was spawned according to the process tree?
<img width="1470" height="956" alt="Screenshot 2026-03-05 at 2 02 37 PM" src="https://github.com/user-attachments/assets/caa0b36d-7755-43d8-aeb7-9fd5269362da" />

## Q.4 The payroll.pdf application seems to be masquerading as which known Windows file?
<img width="1470" height="956" alt="Screenshot 2026-03-05 at 2 03 12 PM" src="https://github.com/user-attachments/assets/80048f51-a56e-433f-bbdb-dbe3ade1ce07" />

## Q.5 What associated URL is linked to the file?
<img width="1470" height="956" alt="Screenshot 2026-03-05 at 2 04 34 PM" src="https://github.com/user-attachments/assets/f3535e38-80d2-46a1-b9a3-53a8c922018c" />
## Q.6 How many extracted strings were identified from the sandbox analysis of the file?
<img width="1470" height="956" alt="Screenshot 2026-03-05 at 2 05 29 PM" src="https://github.com/user-attachments/assets/204473e4-8787-40f2-83de-746cde2e444e" />

---
# Practise Questions

## Q.1 What is the SHA256 hash of the file?
<img width="683" height="373" alt="Screenshot 2026-03-05 at 2 09 10 PM" src="https://github.com/user-attachments/assets/6e507f65-922a-4e7b-98a2-21541544fa84" />
## Q.2 What family labels are assigned to the file on VirusTotal?
<img width="1470" height="956" alt="Screenshot 2026-03-05 at 2 09 40 PM" src="https://github.com/user-attachments/assets/2370926e-13a6-4f07-9987-429fc716347a" />


## Q.3 When was the first time the file was recorded in the wild? (Answer Format: YYYY-MM-DD HH:MM:SS UTC)

<img width="1470" height="956" alt="Screenshot 2026-03-05 at 2 10 28 PM" src="https://github.com/user-attachments/assets/54172acf-2f66-4d72-9f3f-26eda00145dd" />


## Q.4 Name the text file dropped during the execution of the malicious file.
<img width="1470" height="956" alt="Screenshot 2026-03-05 at 2 13 52 PM" src="https://github.com/user-attachments/assets/3414883e-6266-46a2-974e-af7c1949b88c" />

## Q.5 What PowerShell command is observed to be executed?
<img width="1470" height="956" alt="Screenshot 2026-03-05 at 2 25 09 PM" src="https://github.com/user-attachments/assets/4d02d621-6e83-4021-8d05-e3f13646f6d6" />

## Q.6 What MITRE ATT&CK ID is associated with the actions of the command?
<img width="1470" height="956" alt="Screenshot 2026-03-05 at 2 22 37 PM" src="https://github.com/user-attachments/assets/85473c2b-1114-4efd-924d-8d532f3d7321" />


