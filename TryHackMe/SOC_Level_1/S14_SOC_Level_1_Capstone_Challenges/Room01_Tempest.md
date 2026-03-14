# SOC Investigation Basics 

Imagine you are a **security detective** in a company.  
Your job is to **check computer activities and find anything suspicious**.  

To do this, SOC analysts mainly use three ideas:

1. Log Analysis
2. Event Correlation
3. Hash Comparison

---

# 1. Log Analysis

## What is a Log?

A **log** is like a **diary of a computer**.

Every time something happens on a computer, it writes it down in a log.

Example events recorded in logs:

| Event | Example |
|-----|-----|
| Login attempt | Someone tried to login |
| File created | A new file was created |
| Program executed | Chrome.exe started |
| Network connection | Computer connected to an IP |

Each log entry also has a **timestamp**.

Timestamp = exact time when the event happened.

Example:

| Time | Event |
|----|----|
| 10:01 | User login |
| 10:05 | File downloaded |
| 10:07 | Suspicious program executed |

This helps SOC analysts **reconstruct what happened step by step**.

### Why Log Analysis is Important

SOC analysts read logs to detect:

- Security attacks
- Malware activity
- Suspicious login attempts
- System problems
- Performance issues

Example:

If a user logs in from **India at 10:00** and from **Russia at 10:05**, something is suspicious.

---

# 2. Event Correlation

Sometimes **one log alone is not enough**.

So analysts **connect information from multiple logs**.

This process is called **Event Correlation**.

Think of it like **connecting puzzle pieces**.

Example scenario:

A computer connects to a suspicious IP.

Now we check **different logs**.

### Firewall Log

| Field | Example |
|----|----|
| Source IP | 192.168.1.10 |
| Destination IP | 45.77.23.11 |
| Port | 443 |
| Protocol | TCP |
| Action | Allowed |

Firewall tells us **network connection details**.

---

### Sysmon Log

| Field | Example |
|----|----|
| Process Name | powershell.exe |
| User | john |
| Machine | PC-01 |

Sysmon tells us **which program created the connection**.

---

### Now We Combine the Information

| Information | Source |
|----|----|
| Source IP | Firewall |
| Destination IP | Firewall |
| Port | Firewall |
| Protocol | Firewall |
| Process name | Sysmon |
| User account | Sysmon |
| Machine name | Sysmon |

Now the SOC analyst understands:
User john ran powershell.exe
powershell.exe created a connection
Connection went to suspicious IP 45.77.23.11

This is **event correlation**.

We combine multiple logs to **see the full story**.

---

# 3. Compare Artefacts by Hash

Before starting investigation, analysts **verify files using hashes**.

## What is a Hash?

A **hash** is like a **digital fingerprint of a file**.

If even **one tiny thing changes in the file**, the hash will change.

Example:

File: malware.exe

SHA256 Hash:

CB3A1E6ACFB246F256FBFEFDB6F494941AA30A5A7C3F5258C3E63CFA27A23DC6

If the file changes → hash will also change.

### Why SOC Analysts Compare Hashes

To check:

- Is the file modified?
- Is the file malware?
- Is the file original?

### PowerShell Command to Generate Hash

Write this command in PowerShell:

Get-FileHash -Algorithm SHA256 .\capture.pcapng

Example Output:

| Algorithm | Hash |
|----|----|
| SHA256 | CB3A1E6ACFB246F256FBFEFDB6F494941AA30A5A7C3F5258C3E63CFA27A23DC6 |

---

# Files Used in Investigation

During investigation we usually get files like these:

| File | Purpose |
|----|----|
| capture.pcapng | Network traffic |
| sysmon.evtx | Sysmon logs |
| windows.evtx | Windows event logs |

---

# Tools Used in This Investigation

SOC analysts use different tools to analyse logs and network traffic.

## Endpoint Log Analysis Tools

| Tool | Purpose |
|----|----|
| EvtxEcmd | Convert EVTX logs into readable format |
| Timeline Explorer | Filter and analyse parsed logs |
| SysmonView | Visualise Sysmon activity |
| Event Viewer | Default Windows log viewer |

---

## Network Analysis Tools

| Tool | Purpose |
|----|----|
| Wireshark | Analyse network packets |
| Brim | Fast network log investigation |

---

# EvtxEcmd

EvtxEcmd is a **command line forensic tool**.

It converts **Windows Event Logs (.evtx)** into formats like:

- CSV
- JSON
- XML

CSV is useful because we can easily analyse it.

Example command:

.\EvtxECmd.exe -f C:\Users\user\Desktop\Incident Files\sysmon.evtx --csv C:\Users\user\Desktop\Incident Files --csvf sysmon.csv

What this command does:

1. Takes sysmon.evtx log file
2. Converts it into CSV
3. Saves it as sysmon.csv

---

# Timeline Explorer

Timeline Explorer is a **GUI tool for analysing CSV logs**.

Steps to use:

1. Open Timeline Explorer
2. Click **File → Open**
3. Select **sysmon.csv**

Now you can:

- Filter logs
- Search events
- Sort columns
- Find suspicious activities

Example search:
powershell.exe

This shows all events related to PowerShell.

---

# SysmonView

SysmonView helps analysts **visualise Sysmon logs**.

But first we must export logs as XML.

## Export XML from Event Viewer

Steps:

1. Open Event Viewer
2. Export logs
3. Save as XML

Then open SysmonView.

Steps:

1. File → Import Sysmon Event Logs
2. Select XML file

Now SysmonView shows:

- Process relationships
- Network connections
- Process tree

Example:
explorer.exe
└ powershell.exe
└ malware.exe

This helps analysts understand **process behaviour quickly**.

---

# Simple SOC Investigation Workflow

A typical SOC investigation may look like this:

Step 1  
Verify files using hashes.

Step 2  
Parse Windows logs using EvtxEcmd.

Step 3  
Analyse logs using Timeline Explorer.

Step 4  
Visualise process activity using SysmonView.

Step 5  
Analyse network traffic using Wireshark or Brim.

Step 6  
Correlate events from different sources.

Step 7  
Reconstruct the full attack timeline.

---

# Key Idea (Very Important for SOC)

SOC analysts **never trust one log only**.

They always:

- Check multiple logs
- Correlate events
- Build the timeline
- Understand the full attack story

---
# Lab Questions
## Q.1 The user of this machine was compromised by a malicious document. What is the file name of the document?
## Q.2 What is the name of the compromised user and machine?














