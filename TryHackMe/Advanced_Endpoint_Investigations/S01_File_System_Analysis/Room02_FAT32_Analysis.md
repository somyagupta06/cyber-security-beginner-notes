# FAT32 Filesystem 

## What is a Filesystem?

A filesystem is a system that helps the Operating System:

- Store files
- Find files
- Delete files
- Track file information

Think of it like a library catalog.

Without a filesystem, the OS would not know where files are stored on the disk.

---

# Why Should a SOC Analyst Learn FAT32?

Many attacks use:

- USB drives
- SD cards
- Embedded devices
- IoT devices

Most of these still use FAT32.

Attackers can:

- Hide files
- Delete evidence
- Deliver malware through USBs

A SOC analyst must know where evidence is stored.

---

# Why Attackers Like FAT32

FAT32 does not support file permissions.

Example:

NTFS:
- User permissions
- Admin permissions

FAT32:
- No permissions

Result:

Anyone can access files on the USB.

This makes FAT32 useful for malware delivery.

---

# Real World Example

Stuxnet used a USB drive to infect air-gapped systems.

Air-gapped = No internet connection.

The malware was delivered through a USB because the target systems were isolated.

---

# FAT32 Structure

FAT32 has 3 main parts:

1. Reserved Area
2. FAT Area
3. Data Area

Simple memory trick:

Reserved Area = Information about the filesystem

FAT Area = Map of file locations

Data Area = Actual file data

---

# 1. Reserved Area

This area contains important filesystem information.

Main components:

- Boot Sector
- FSInfo Sector
- Backup Boot Sector
- Reserved Sectors

---

# Boot Sector

The Boot Sector is the first sector of the FAT32 partition.

It tells the OS:

- Sector size
- Cluster size
- FAT location
- Root Directory location
- Data Area location

Think of it as a map of the filesystem.

---

# SOC Importance

If an attacker modifies the Boot Sector:

Possible results:

- Hidden malware
- Bootkits
- Corrupted filesystem
- Persistence mechanisms

Always check Boot Sector integrity during forensic analysis.

---

# Important Boot Sector Fields

## Bytes Per Sector

Usually:

512 bytes

Meaning:

One sector contains 512 bytes.

---

## Sectors Per Cluster

A cluster is a group of sectors.

Example:

1 cluster = 8 sectors

Files are stored in clusters.

Not directly in sectors.

---

## Number of FATs

Usually:

2

Reason:

- FAT1 = Main table
- FAT2 = Backup table

If FAT1 becomes corrupted, FAT2 can help recover data.

---

# 2. FAT Area

FAT = File Allocation Table

This is the most important part of FAT32.

The FAT tracks where file data is stored.

Think of it as a navigation system.

---

# Cluster Chains

Large files may need multiple clusters.

Example:

report.pdf

Stored in:

Cluster 7
Cluster 8
Cluster 9

The FAT records:

7 → 8

8 → 9

9 → EOF

EOF = End Of File

This chain tells the OS how to read the complete file.

---

# Common FAT Values

Free Cluster

00 00 00 00

Meaning:

Cluster is empty.

---

Bad Cluster

0FFFFFF7

Meaning:

Damaged cluster.

Do not store data here.

---

End Of File (EOF)

0FFFFFFF

Meaning:

Last cluster of the file.

---

# SOC Importance

Investigators use FAT entries to:

- Rebuild deleted files
- Detect hidden files
- Understand file fragmentation
- Recover evidence

---

# 3. Data Area

This area stores actual data.

Examples:

- Documents
- Images
- Videos
- Malware files

Everything is stored here.

---

# Root Directory

The Root Directory contains file metadata.

Example:

notes.pdf

Metadata may include:

- File name
- File size
- Timestamps
- Starting cluster

The FAT uses the starting cluster to locate the file data.

---

# File Deletion in FAT32

Important forensic concept.

Deleting a file does NOT immediately erase data.

What happens?

Step 1:
File entry is marked as deleted.

Step 2:
Clusters are marked as available.

Step 3:
Actual data often remains on disk.

This is why deleted files can be recovered.

---

# SOC Investigation Tip

If malware was deleted:

Do not assume it is gone.

Check:

- Deleted entries
- FAT records
- Unallocated space

Evidence may still exist.

---

# FAT32 and MITRE ATT&CK

## T1070.004 - File Deletion

Attacker deletes files to hide evidence.

Example:

malware.exe deleted after execution.

---

## T1070.006 - Timestomping

Attacker changes timestamps.

Purpose:

Hide the real execution time.

---

## T1564.001 - Hidden Files

Attacker hides files from normal users.

---

## T1564.005 - Hidden File System

Attacker hides data in filesystem structures.

---

## T1006 - Direct Volume Access

Malware accesses the disk directly.

Bypasses normal file operations.

---

# USB Threats

Common attacker actions:

- Deliver malware
- Execute scripts
- Steal data
- Hide payloads
- Spread worms

Many malicious USBs use FAT32 because it works on almost every system.

---

# SOC Analyst Checklist

When analyzing a suspicious FAT32 USB:

1. Examine Boot Sector

2. Check FAT entries

3. Look for deleted files

4. Review timestamps

5. Search for hidden files

6. Inspect Root Directory

7. Recover deleted artifacts

8. Check for suspicious executables

---

# Quick Revision

Filesystem = Organizes files

Boot Sector = Filesystem map

FAT = Tracks file clusters

Data Area = Stores actual data

Cluster Chain = Links file data blocks

Deleted File ≠ Destroyed File

FAT32 = Common in USB attacks

SOC Goal = Find hidden, deleted, or malicious files

---

# FAT32 Data Area & Forensic Analysis Notes

## Data Area

The Data Area is the place where actual files and folders are stored.

It contains:

1. Root Directory
2. Data Region

Simple idea:

Root Directory = Index of files

Data Region = Actual file content

---

# Root Directory

The Root Directory stores metadata about files.

It does NOT store the actual file content.

It stores information such as:

- File name
- File size
- File timestamps
- Starting cluster
- File attributes

Think of it like a book index.

The index tells you where a chapter starts, but the chapter itself is stored elsewhere.

---

# FAT32 Root Directory

Older FAT versions had a fixed-size Root Directory.

FAT32 is different.

The Root Directory can grow when more files are added.

This makes FAT32 more flexible.

---

# Two Types of Directory Entries

Every file normally has:

1. LFN Entry (Long File Name)
2. SFN Entry (Short File Name)

---

# LFN (Long File Name)

Purpose:

Stores the real file name.

Example:

Project_Report_Final.docx

Without LFN, long names would not be possible.

---

# SFN (Short File Name)

Purpose:

Stores important file metadata.

Uses the old 8.3 naming format.

Example:

PROJECT~1DOC

---

# Why Should a SOC Analyst Care?

Attackers may:

- Hide files
- Rename malware
- Use misleading filenames
- Manipulate metadata

Both LFN and SFN should be checked during investigations.

---

# SFN Entry Fields

## File Name

Stores the short filename.

Example:

ABOUT_~1TXT

---

## Attributes

Describes file properties.

Common attributes:

READ_ONLY

HIDDEN

SYSTEM

DIRECTORY

ARCHIVE

---

## Hidden Attribute

Important for investigations.

Attackers may hide malware by setting:

HIDDEN

The file becomes invisible to normal users but can still be detected by forensic tools.

---

## Creation Time

Shows when the file was created.

Useful for timeline analysis.

---

## Last Access Time

Shows when the file was last opened.

Useful for identifying user activity.

---

## Last Modified Time

Shows when the file was last changed.

Useful for tracking attacker actions.

---

## Starting Cluster

Shows where the file begins in the Data Region.

The FAT uses this cluster number to locate the file data.

Example:

Starting Cluster = 7

FAT:

7 → 8 → 9 → EOF

---

## File Size

Stores the size of the file in bytes.

Maximum FAT32 file size:

4 GB - 1 byte

---

# Example Investigation

File:

malware.exe

Metadata:

Starting Cluster = 150

File Size = 2 MB

Investigator actions:

1. Locate Cluster 150

2. Follow FAT chain

150 → 151 → 155 → EOF

3. Recover file content

4. Analyze malware

---

# FAT32 Forensic Analysis Techniques

These techniques help investigators find evidence.

---

# 1. Cluster Chain Analysis

Focus:

Analyze how clusters are linked together.

Uses:

- FAT
- Starting Cluster

Purpose:

- Reconstruct files
- Detect corruption
- Detect hidden data
- Understand fragmentation

---

# 2. Directory Structure & Filename Analysis

Focus:

Analyze:

- File names
- Folder names
- File attributes

Purpose:

- Detect hidden files
- Detect suspicious files
- Detect unusual folder structures

Example:

USB contains:

Documents

Pictures

Videos

svchost.exe

The executable may be suspicious.

---

# 3. Boot Sector Analysis

Focus:

Analyze filesystem metadata.

Check:

- Sector size
- Cluster size
- FAT location
- Filesystem information

Purpose:

Detect:

- Tampering
- Bootkits
- Corruption

---

# 4. FAT Manipulation Analysis

Focus:

Compare:

FAT1

vs

FAT2

Purpose:

Detect:

- Tampering
- Corruption
- Hidden modifications

If FAT1 and FAT2 differ unexpectedly, investigate further.

---

# 5. Corruption & Tampering Detection

Focus:

Check filesystem consistency.

Look for:

- Broken cluster chains
- Invalid entries
- Missing metadata

Purpose:

Detect attacker attempts to hide evidence.

---

# Data Recovery & Content Analysis

Goal:

Recover evidence from the filesystem.

---

# 1. Deleted File Recovery

Important Concept:

Delete ≠ Destroy

When a file is deleted:

- Metadata is marked deleted
- Clusters become available

Actual data may still exist.

Purpose:

Recover attacker tools, malware, or documents.

---

# 2. File Carving

Used when metadata is missing.

The investigator searches for file signatures.

Examples:

JPEG = FFD8

PDF = 25504446

ZIP = 504B0304

Purpose:

Recover files without directory entries.

---

# 3. Slack Space Analysis

Slack Space = Unused space inside an allocated cluster.

Example:

Cluster Size = 4096 bytes

File Size = 3000 bytes

Unused Space = 1096 bytes

Purpose:

Find:

- Hidden data
- Leftover evidence
- Remnants of deleted files

Attackers sometimes hide information here.

---

# 4. Keyword & Pattern Search

Search for:

password

admin

powershell

cmd.exe

bitcoin

Purpose:

Quickly identify evidence across the filesystem.

Can be performed on:

- Allocated space
- Unallocated space

---

# 5. Timeline Analysis

Uses:

- Creation Time
- Access Time
- Modified Time

Purpose:

Reconstruct attacker activity.

Example:

08:00 USB inserted

08:01 malware copied

08:03 malware executed

08:05 persistence created

08:10 logs deleted

This helps build the attack timeline.

---

# SOC Analyst Checklist

When investigating a FAT32 USB:

✓ Check Boot Sector

✓ Check FAT1 and FAT2

✓ Review Root Directory

✓ Look for hidden files

✓ Recover deleted files

✓ Analyze cluster chains

✓ Examine Slack Space

✓ Search for suspicious keywords

✓ Build a timeline

---

# Quick Revision

Root Directory = File metadata

LFN = Real filename

SFN = Metadata entry

Starting Cluster = Beginning of file data

FAT = Tracks cluster chains

Data Region = Actual file content

Deleted File ≠ Gone

File Carving = Recover files using signatures

Slack Space = Hidden leftover space

Timeline Analysis = Reconstruct attacker activity

---

# FAT32 Forensics - Hidden Files, Timestomping & Deleted Files

## Why Attackers Like FAT32

FAT32 is commonly used in:

- USB drives
- SD cards
- IoT devices
- Embedded systems

FAT32 has no permission system.

This makes it easy for attackers to:

- Hide files
- Modify timestamps
- Delete evidence

---

# MITRE ATT&CK Techniques Covered

T1564.001 - Hidden Files and Directories

T1070.006 - Timestomping

T1070.004 - File Deletion

T1070.009 - Clear Persistence

---

# Hidden Files & Directories

## Attacker Goal

Hide malware or tools from normal users.

Example:

malware.exe

Attacker sets:

Hidden Attribute = ON

Result:

Normal users cannot see the file.

---

# How Hidden Files Work in FAT32

Hidden status is stored inside the SFN Entry.

Important field:

Offset 0x0B

Field Name:

Attributes

---

# Common FAT32 Attributes

0x01 = Read Only

0x02 = Hidden

0x04 = System

0x10 = Directory

0x20 = Archive

---

# SOC Investigation

When checking an SFN Entry:

Attribute = 0x02

Meaning:

Hidden File

---

Attribute = 0x13

Meaning:

Read Only + Hidden + Directory

---

# Hidden Directory Example

Directory:

M@LL0V3

Attributes:

0x13

Meaning:

- Read Only
- Hidden
- Directory

Investigation Result:

Hidden folder found.

---

# Hidden File Example

File:

BeMyValent1ne.txt

Short Name:

BEMYVA~1.TXT

Attributes:

0x02

Meaning:

Hidden file.

Investigation Result:

Hidden file discovered.

---

# Analyst Workflow for Hidden Files

Step 1

Open image in HxD

---

Step 2

Navigate to Root Directory

Example:

00400000

---

Step 3

Read SFN entries

---

Step 4

Check Attribute byte

Offset:

0x0B

---

Step 5

Identify hidden files and folders

---

# Autopsy Investigation

Autopsy automatically shows:

- Hidden files
- Hidden folders
- File metadata
- Timestamps
- File content

Useful tabs:

- Hex
- Text
- File Metadata

---

# Timestomping

## What Is Timestomping?

Changing timestamps to hide activity.

Attackers modify:

- Creation Time
- Last Access Time
- Last Modified Time

---

# Why Attackers Do It

Example:

Attacker creates malware today.

Real timestamp:

2025-01-07

Attacker changes it to:

2021-01-10

Now the file looks old and harmless.

---

# Common Timestomping Signs

## Sign 1

Creation Time > Modified Time

Suspicious

---

## Sign 2

Access Time older than Modification Time

Suspicious

---

## Sign 3

One file has dates very different from all other files

Suspicious

---

# Example From Investigation

File:

install.txt

Problem:

Last Access Date was older than Modified Date.

This is abnormal.

Possible timestamp manipulation.

---

# SOC Analyst Workflow

Collect:

- Creation Time
- Last Access Time
- Last Modified Time

Compare them.

Look for inconsistencies.

---

# Timeline Analysis

Purpose:

Reconstruct attacker activity.

Example:

10:00 USB inserted

10:02 malware copied

10:03 malware executed

10:05 persistence created

10:10 logs deleted

---

# Autopsy Timeline

Autopsy Timeline helps:

- Visualize file events
- Detect timestamp anomalies
- Detect timestomping

Useful for large investigations.

---

# File Deletion

## Attacker Goal

Remove evidence.

Examples:

- Delete malware
- Delete scripts
- Delete logs
- Delete persistence files

---

# Important Forensic Concept

Delete ≠ Destroy

Deleting a file usually does NOT remove data immediately.

The metadata is marked deleted.

Actual content may still remain on disk.

---

# Detecting Deleted Files

Look for deleted SFN entries.

Deleted entries often begin with:

E5

Example:

E5 53 54 52 49 4B 7E 31

Meaning:

Deleted file entry.

---

# Deleted File Example

Recovered File:

CstrikePers.ps1

Short Name:

STRIKE~1.PS1

Status:

Deleted

---

# Important Metadata

Starting Cluster:

28

File Size:

492 bytes

---

# Recovery Process

Step 1

Find deleted SFN entry.

---

Step 2

Read Starting Cluster.

Example:

Cluster 28

---

Step 3

Navigate to cluster.

---

Step 4

Recover content.

---

Step 5

Analyze recovered file.

---

# Why Deleted Files Matter

Attackers often delete:

- Malware
- Scripts
- Payloads
- Persistence mechanisms

Deleted files may contain valuable evidence.

---

# Recycle Bin Analysis

Always inspect:

$RECYCLE.BIN

Reason:

Deleted files may still exist there.

Many investigators forget this location.

---

# SOC Investigation Checklist

When analyzing a FAT32 image:

✓ Check Root Directory

✓ Check SFN entries

✓ Check hidden attributes

✓ Identify suspicious filenames

✓ Review timestamps

✓ Build timeline

✓ Check deleted entries

✓ Inspect $RECYCLE.BIN

✓ Recover deleted files

✓ Analyze recovered content

---

# Quick Revision

Hidden File

Attribute = 0x02

---

Hidden Directory

Attribute = 0x13

(Read Only + Hidden + Directory)

---

Timestomping

Manipulating timestamps to hide activity.

---

Suspicious Timestamp

Access Date older than Modified Date.

---

Deleted File

May still be recoverable.

---

Deleted Entry

Often starts with E5.

---

Recovery Flow

Deleted Entry

↓

Starting Cluster

↓

Cluster Data

↓

Recovered File

---

SOC Goal

Find:

- Hidden Files
- Hidden Folders
- Timestamp Manipulation
- Deleted Evidence
- Persistence Artifacts
