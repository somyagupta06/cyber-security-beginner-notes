# Cold System Forensics (SOC Notes)

## What is Cold System Forensics?

Cold System Forensics means investigating a computer that is powered OFF.

The analyst examines:
- Hard disks
- Files
- Logs
- Registry
- Deleted files
- System artifacts

The goal is to find evidence without changing the original data.

---

## Simple Example

Imagine a crime happened in a room.

### Live Forensics
The room is still active and people are moving inside.

You can see:
- What people are doing now
- Current conversations
- Current activities

### Cold Forensics
The room is empty.

You cannot see current activities, but you can see:
- Footprints
- Documents
- Fingerprints
- Things left behind

Cold Forensics works in the same way.

---

# Why is Cold Forensics Important?

As a SOC analyst or DFIR investigator, you may need evidence for:

- Incident investigations
- Malware analysis
- Data theft investigations
- Legal cases
- Internal company investigations

Cold Forensics helps preserve evidence safely.

---

# Cold Forensics vs Live Forensics

| Cold Forensics | Live Forensics |
|---------------|---------------|
| System is OFF | System is ON |
| Safer for evidence | Evidence may change |
| Can analyze disk completely | Can see running activity |
| Cannot access RAM | Can access RAM |
| Good for legal cases | Good for active attacks |

### Easy Trick

Live Forensics = What is happening now?

Cold Forensics = What happened before?

---

# What Data is Lost After Shutdown?

When a computer is turned off:

- Running processes disappear
- Network connections disappear
- RAM contents disappear
- Temporary active information disappears

This is why investigators sometimes collect live data first.

---

# Order of Volatility

Order of Volatility means:

"What should be collected first before it disappears?"

Most volatile data disappears fastest.

## From Most Volatile to Least Volatile

1. CPU Registers and Cache
2. RAM
3. Process Information
4. Network Connections
5. Temporary Files
6. Hard Disk
7. Logs
8. Archived Media

### Important SOC Rule

Collect data that disappears fastest first.

---

# Disk Imaging

## What is Disk Imaging?

Disk Imaging means creating an exact copy of a disk.

The copy contains:

- Existing files
- Deleted files
- Hidden data
- File system information
- Unallocated space

Everything is copied.

---

## Why Not Analyze the Original Disk?

Because opening the original disk may change:

- Timestamps
- Metadata
- File information

This can damage evidence.

### Correct Process

Original Disk
↓
Create Image
↓
Analyze Image

Never analyze the original evidence directly.

---

# Write Blocker

## What is a Write Blocker?

A Write Blocker prevents any changes to evidence.

It allows:

- Reading data
- Copying data

It blocks:

- Editing data
- Deleting data
- Modifying data

### Why is it Important?

Without a write blocker:

The investigator may accidentally modify evidence.

---

# Physical Acquisition

Physical Acquisition means collecting data directly from hardware.

Used when:

- Device is damaged
- Device is locked
- Normal access is impossible

---

## Chip-Off Forensics

Storage chip is physically removed from the device.

The chip is then read using special equipment.

Commonly used on:

- Mobile devices
- Damaged devices

---

## JTAG Forensics

Uses hardware debugging interfaces to access data.

Commonly used on:

- IoT devices
- Embedded systems
- Routers

---

# Secure Storage of Evidence

After collecting evidence, it must be protected.

Best practices:

- Use encryption
- Limit access
- Store in secure locations
- Perform regular audits

Only authorized investigators should access evidence.

---

# Chain of Custody

## What is Chain of Custody?

Chain of Custody is a record showing:

- Who collected the evidence
- Who accessed it
- When it was accessed
- Why it was accessed

---

## Why is it Important?

Courts must trust that evidence was not modified.

Without Chain of Custody:

Evidence may become unreliable.

---

## Simple Example

10:00 AM - Analyst A collected laptop

12:00 PM - Analyst B received laptop

2:00 PM - Disk image created

4:00 PM - Evidence stored securely

Every action must be documented.

---

# Hashing

## What is Hashing?

Hashing creates a unique fingerprint of a file.

Example:

File → Hash Value

If the file changes:

The hash changes.

---

## Why is Hashing Important?

Investigators calculate hashes:

- Before acquisition
- After acquisition

If both hashes match:

Evidence was not modified.

If hashes are different:

Evidence may have been altered.

---

# Common Tools Used in Cold Forensics

## Disk Imaging Tools

### dd

Basic disk imaging tool.

Used to create exact disk copies.

---

### dc3dd

Forensic version of dd.

Additional features:

- Hashing
- Logging
- Better error handling

---

### FTK Imager

Popular forensic imaging tool.

Can:

- Create images
- Preview files
- Mount images

Very common in investigations.

---

### Guymager

Linux-based imaging tool.

Features:

- Fast imaging
- Hash verification
- Logging

---

# Disk Analysis Tools

## The Sleuth Kit (TSK)

Command-line forensic toolkit.

Can:

- List files
- Recover deleted files
- Analyze file systems

---

## Autopsy

GUI version of TSK.

Can analyze:

- Browser history
- Downloads
- User activity
- Timelines
- Deleted files

Very beginner friendly.

---

## EnCase

Professional forensic suite.

Commonly used by:

- Law enforcement
- Government agencies
- Corporate investigators

---

## FTK

Strong search and indexing capabilities.

Useful for:

- Large investigations
- Email analysis
- Keyword searching

---

## Magnet AXIOM

Advanced investigation tool.

Can analyze:

- Computers
- Mobile devices
- Cloud evidence

Provides timeline-based investigations.

---

# Basic Disk Analysis Workflow

Step 1:
Acquire disk image

Step 2:
Verify hash values

Step 3:
Load image into forensic tool

Step 4:
Analyze artifacts

Step 5:
Recover deleted files

Step 6:
Build timeline

Step 7:
Generate report

---

# Common Artifacts Investigators Look For

- Browser history
- Downloads
- Event logs
- Registry entries
- User documents
- Installed programs
- Email archives
- Deleted files
- USB activity

These artifacts help reconstruct attacker activity.

---

# Risks During Analysis

## Data Integrity Risk

Always:

- Use write blockers
- Verify hashes
- Analyze copies only

---

## Misinterpretation Risk

Never trust one artifact alone.

Always correlate:

- Logs
- Timestamps
- Files
- Registry
- User activity

---

## Documentation Risk

Document every action.

Good documentation improves:

- Investigation quality
- Evidence reliability
- Legal admissibility

---

# SOC Exam Revision

Cold Forensics:
Investigation of powered-off systems.

Disk Imaging:
Bit-by-bit copy of a storage device.

Write Blocker:
Prevents modification of evidence.

Hashing:
Verifies evidence integrity.

Chain of Custody:
Documents who handled evidence.

Most Volatile Data:
CPU Registers and RAM.

Best GUI Analysis Tool:
Autopsy.

Best Known Imaging Tool:
FTK Imager.

Main Goal:
Preserve and analyze evidence without altering it.
