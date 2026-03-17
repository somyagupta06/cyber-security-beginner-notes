# Volatility (Memory Forensics) – Simple SOC Notes

## 1. What is Volatility?

Volatility is a tool used to analyze **RAM (memory dumps)**.

Simple meaning:

* It helps you see **what was happening inside a system**
* Even if system is turned OFF now

SOC Use:

* detect malware running in memory
* find hidden processes
* recover attack activity

---

# 2. Why Memory Analysis is Important?

Some attacks:

* run only in memory
* leave no files on disk

So normal tools miss them.

Volatility helps you:

* see running processes
* find injected malware
* detect hidden activity

---

# 3. Key Idea of Volatility

Volatility works using:

* plugins (small tools inside it)

Each plugin does a job like:

* list processes
* check network connections
* find malware

---

# 4. Volatility Versions

| Version      | Info           |
| ------------ | -------------- |
| Volatility 2 | old (Python 2) |
| Volatility 3 | new (Python 3) |

Use:
Volatility3 (recommended)

Important change:

* No profiles needed in Volatility3
* Tool auto-detects OS

---

# 5. Installation (Simple Idea)

Steps:

1. Install Python 3
2. Download Volatility
3. Run help command

Command:

python3 vol.py -h

SOC Tip:

* In TryHackMe AttackBox → already installed

---

# 6. Memory Dump Files

Memory dump = snapshot of RAM

Common formats:

| Source       | File Type |
| ------------ | --------- |
| Normal tools | .raw      |
| VMware       | .vmem     |
| Hyper-V      | .bin      |
| Parallels    | .mem      |
| VirtualBox   | .sav      |

SOC Use:

* this file is your main evidence

---

# 7. Tools to Capture Memory

Common tools:

* FTK Imager
* Redline
* DumpIt
* Memoryze
* FastDump

SOC Tip:

* memory capture takes time
* do it carefully (can affect system)

---

# 8. Plugin Structure (Very Important)

In Volatility3, plugins are like:

windows.<plugin>
linux.<plugin>
mac.<plugin>

Example:

windows.info

Meaning:

* OS = Windows
* plugin = info

---

# 9. First Step in Investigation

Always start with:

python3 vol.py -f file.raw windows.info

This gives:

* OS details
* system info
* build version

SOC Use:

* confirm target system
* understand environment

---

# 10. Important Concept (Old vs New)

Volatility2:

* needed "profile" (hard to guess)

Volatility3:

* auto-detects OS
* no profile needed

So easier for beginners

---

# 11. What Can Volatility Do?

With plugins, you can:

* list processes
* find malware
* check network connections
* extract files
* detect injected code

---

# 12. Basic Workflow (SOC)

Step 1 → Load memory file

python3 vol.py -f file.raw windows.info

Step 2 → Find processes

(use process plugins later)

Step 3 → find suspicious activity

Step 4 → extract evidence

---

# 13. Key SOC Idea

Disk analysis → what was saved
Memory analysis → what was running

Both are needed for full investigation

---

# Final Simple Summary

Volatility = RAM analysis tool

Plugins = small tools inside Volatility

Memory dump = main evidence

Volatility3 = easier (no profiles)

First command always:

python3 vol.py -f file.raw windows.info
---
# Volatility Plugins (SOC Notes – Easy Version)

---

# 1. Process Analysis Plugins (Very Important)

## pslist (Basic Process List)

Command:

python3 vol.py -f file.raw windows.pslist

Purpose:

* shows running + terminated processes
* similar to Task Manager

Limitation:

* malware can hide by removing itself (unlinking)

SOC Use:

* basic process overview

---

## psscan (Hidden Process Detection)

Command:

python3 vol.py -f file.raw windows.psscan

Purpose:

* scans memory for process structures (_EPROCESS)
* finds hidden/unlinked processes

Advantage:

* detects hidden malware

Limitation:

* may show false positives

SOC Use:

* find rootkits or hidden processes

---

## pstree (Process Tree)

Command:

python3 vol.py -f file.raw windows.pstree

Purpose:

* shows parent-child process relation

Example:

* explorer.exe → cmd.exe → malware.exe

SOC Use:

* understand attack chain
* detect suspicious parent-child behavior

---

# 2. Network Analysis

## netstat

Command:

python3 vol.py -f file.raw windows.netstat

Purpose:

* shows network connections

Limitation:

* unstable sometimes (Volatility3 issue)

Alternative:

* use bulk_extractor for PCAP

SOC Use:

* find suspicious IP connections
* detect C2 communication

---

# 3. DLL Analysis

## dlllist

Command:

python3 vol.py -f file.raw windows.dlllist

Purpose:

* shows DLLs loaded by processes

SOC Use:

* detect malicious DLL injection
* identify malware indicators

---

# 4. Malware Detection Plugins

## malfind (VERY IMPORTANT)

Command:

python3 vol.py -f file.raw windows.malfind

Purpose:

* finds injected code inside processes

How it works:

* looks for:

  * RWE or RX memory (executable)
  * no file on disk (fileless malware)

Indicators:

* MZ header → executable file
* shellcode → suspicious code

SOC Use:

* detect process injection
* detect fileless malware

---

## yarascan

Command:

python3 vol.py -f file.raw windows.yarascan

Purpose:

* scan memory using YARA rules

SOC Use:

* detect known malware patterns
* hunt based on signatures

---

# 5. Advanced Malware Hunting

## ssdt (Hook Detection)

Command:

python3 vol.py -f file.raw windows.ssdt

Purpose:

* detects SSDT hooking

Simple idea:

* malware modifies system function pointers

SOC Use:

* detect rootkits
* detect kernel-level manipulation

---

# 6. Driver Analysis

## modules

Command:

python3 vol.py -f file.raw windows.modules

Purpose:

* list loaded drivers/modules

Limitation:

* may miss hidden drivers

SOC Use:

* detect active malicious drivers

---

## driverscan

Command:

python3 vol.py -f file.raw windows.driverscan

Purpose:

* scans memory for drivers

Advantage:

* finds hidden drivers

Limitation:

* may return nothing

SOC Use:

* detect stealth drivers

---

# 7. Important Advanced Plugins (Know Names)

* modscan
* driverirp
* callbacks
* idt
* apihooks
* moddump
* handles

Use:

* advanced malware analysis
* rootkit detection

---

# 8. Key Evasion Techniques (Must Know)

Malware tries to hide using:

* unlink processes → hide from pslist
* code injection → detected by malfind
* hooking → detected by ssdt
* hidden drivers → detected by driverscan

---

# 9. Simple SOC Workflow

Step 1 → System info
python3 vol.py -f file.raw windows.info

Step 2 → Processes
python3 vol.py -f file.raw windows.pslist
python3 vol.py -f file.raw windows.psscan

Step 3 → Process tree
python3 vol.py -f file.raw windows.pstree

Step 4 → Network
python3 vol.py -f file.raw windows.netstat

Step 5 → Malware detection
python3 vol.py -f file.raw windows.malfind
python3 vol.py -f file.raw windows.yarascan

Step 6 → Drivers
python3 vol.py -f file.raw windows.modules
python3 vol.py -f file.raw windows.driverscan

---

# Final Key Idea

pslist → normal processes
psscan → hidden processes
pstree → process chain

malfind → injected malware
yarascan → pattern detection

ssdt → hooking detection
driverscan → hidden drivers

= Full memory investigation
