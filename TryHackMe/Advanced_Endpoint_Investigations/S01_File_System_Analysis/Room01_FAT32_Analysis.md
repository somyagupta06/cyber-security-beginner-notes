# FAT32 Filesystem - Easy SOC Notes

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
