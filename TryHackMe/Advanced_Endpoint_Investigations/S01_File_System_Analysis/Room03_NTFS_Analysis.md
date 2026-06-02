# NTFS Overview 

## What is NTFS?

NTFS (New Technology File System) is the default file system used by Windows.

A file system is simply a method used by the operating system to store, organize, and manage files on a disk.

Think of NTFS like a library management system:

- Files = Books
- Disk = Library
- NTFS = Librarian who knows where every book is stored

---

# Why is NTFS Important for SOC Analysts?

NTFS stores a lot of metadata about files.

Metadata = Information about a file, not the file content itself.

Examples:

- File name
- Creation time
- Modification time
- Access time
- Permissions
- Location on disk

This information helps investigators understand:

- What happened?
- When did it happen?
- Which file was involved?
- Was a file deleted or modified?

---

# Key Features of NTFS

## 1. Journaling

NTFS keeps a record of file system changes before applying them.

Why useful?

If the system crashes, NTFS can recover.

SOC Value:

- Shows traces of file activity
- Helps during investigations

---

## 2. Advanced Metadata

NTFS stores detailed information about every file.

SOC Value:

Can be used to build an attack timeline.

Example:

A malware file was:

- Created at 10:00 PM
- Modified at 10:05 PM
- Executed at 10:06 PM

---

## 3. File Permissions

NTFS supports access control.

SOC Value:

Helps identify:

- Who could access a file
- Unauthorized access attempts

---

## 4. Encryption (EFS)

Files can be encrypted using Windows Encrypting File System.

SOC Value:

Investigators may find encrypted files used to hide data.

---

## 5. Compression

Files can be compressed to save disk space.

SOC Value:

Attackers may compress stolen data before exfiltration.

---

# Important NTFS Components

## Partition Boot Sector (PBS)

The first sector of the NTFS partition.

Contains:

- NTFS signature
- Disk layout information
- MFT location

SOC Value:

Can reveal:

- Boot tampering
- Rootkits
- Disk modifications

---

# Master File Table (MFT)

The most important NTFS artifact.

Every file and folder gets an MFT record.

Think of MFT as:

"Database of all files on the disk"

Contains:

- File name
- Timestamps
- Permissions
- File size
- Data location

SOC Value:

Even deleted files may still leave records inside the MFT.

---

# MFT Mirror ($MFTMirr)

Backup copy of important MFT records.

SOC Value:

Useful when the main MFT becomes corrupted.

Can help recover evidence.

---

# System Files

NTFS contains special hidden files.

Important examples:

### $MFT

Stores all file records.

### $LogFile

Stores file system transaction logs.

### $Bitmap

Tracks used and free disk space.

### $BadClus

Tracks bad sectors.

### $Boot

Stores boot information.

SOC Value:

These files contain valuable forensic evidence.

---

# File Data Area

This is where actual file content is stored.

SOC Value:

Main source for:

- Data recovery
- Deleted file recovery
- Slack space analysis

---

# Alternate Data Streams (ADS)

ADS allows extra hidden data to be attached to a file.

Example:

report.txt

may secretly contain:

report.txt:hidden.exe

The user only sees:

report.txt

SOC Value:

Attackers may hide:

- Malware
- Payloads
- Scripts

inside ADS.

---

# MACB Timestamps

Very important for investigations.

NTFS stores four important timestamps.

## M = Modified

When file content changed.

Example:

User edited a document.

---

## A = Accessed

When file was opened or read.

Example:

User viewed a file.

---

## C = Changed

When metadata changed.

Examples:

- File renamed
- Permissions changed
- Attributes changed

---

## B = Birth (Created)

When the file was first created.

Example:

Malware downloaded to disk.

---

# Why MACB is Important

MACB helps create an attack timeline.

Example:

10:00 PM -> Malware created

10:05 PM -> Malware modified

10:06 PM -> Malware executed

10:10 PM -> Malware deleted

This helps investigators understand attacker activity.

---

# Timestomping

Attackers sometimes modify timestamps to hide their actions.

Example:

Real creation date:

2025

Attacker changes it to:

2021

Now the file looks old and harmless.

SOC Value:

Look for timestamp inconsistencies between:

- MFT
- Event Logs
- USN Journal
- LogFile

Mismatch = Possible timestomping.

---

# Important NTFS Artifacts for SOC

| Artifact | Why It Matters |
|-----------|---------------|
| $MFT | Information about every file |
| $LogFile | File system activity history |
| $UsnJrnl | File create, modify, delete activity |
| $Bitmap | Used and free clusters |
| ADS | Hidden attacker data |
| MFTMirr | Backup of MFT |

---

# Quick Revision

- NTFS = Default Windows file system
- MFT = Database of all files
- Journaling = Records file system changes
- ADS = Hidden data storage
- MACB = File timeline timestamps
- Timestomping = Timestamp manipulation
- $LogFile = File activity logs
- $UsnJrnl = File change history
- MFT records may remain even after file deletion

# SOC Analyst Goal

Use NTFS artifacts to answer:

- What file was involved?
- When was it created?
- When was it modified?
- Was it deleted?
- Did the attacker try to hide evidence?

---

# NTFS Journaling and $I30 (SOC Notes)

## What is NTFS Journaling?

NTFS Journaling is a feature that records file system changes before they are written to disk.

Purpose:

- Maintain file system integrity
- Help recover after crashes
- Reduce corruption risks
- Keep records of file activity

Think of it as a security camera for file system changes.

---

# Why is Journaling Important for SOC Analysts?

Journaling helps answer questions such as:

- When was a file created?
- When was a file deleted?
- Was a file renamed?
- Was data modified?
- Did an attacker try to hide activity?

It helps build an investigation timeline.

---

# Two Important NTFS Journals

## 1. $LogFile

### What is it?

$LogFile is the NTFS transaction journal.

It records metadata changes before they are committed to disk.

Examples:

- File creation
- File deletion
- File rename
- Metadata updates

---

### SOC Value

Useful for:

- Understanding file system activity
- Investigating suspicious file operations
- Recovering information after crashes

---

### Easy Memory Trick

$LogFile = NTFS's internal diary

---

# 2. USN Journal ($UsnJrnl)

### What is it?

USN Journal keeps a history of file and directory changes.

It records activities happening on the volume.

Examples:

- File created
- File modified
- File renamed
- File deleted

---

### SOC Value

Useful for:

- Timeline creation
- Tracking attacker activity
- Investigating deleted files

---

### Easy Memory Trick

USN Journal = File activity history

---

# Components of USN Journal

## $Max

Stores:

- Maximum journal size
- Allocation settings

Purpose:

Controls how large the journal can grow.

---

## $J

Stores:

- Actual file change records

This is the main file investigators analyze.

---

### Easy Memory Trick

$Max = Rules

$J = Evidence

---

# Common Update Reasons in USN Journal

## USN_REASON_FILE_CREATE

Meaning:

A new file or directory was created.

Example:

malware.exe downloaded to disk

---

## USN_REASON_FILE_DELETE

Meaning:

A file or directory was deleted.

Example:

Attacker deletes malware after execution.

---

## USN_REASON_DATA_OVERWRITE

Meaning:

Existing file content was replaced.

Example:

File content edited.

---

## USN_REASON_DATA_EXTEND

Meaning:

File size increased.

Example:

More data added to a file.

---

## USN_REASON_DATA_TRUNCATION

Meaning:

File size decreased.

Example:

Content removed from a file.

---

## USN_REASON_RENAME_OLD_NAME

Meaning:

A file was renamed.

Example:

malware.exe → svchost.exe

---

## USN_REASON_CLOSE

Meaning:

File was closed after changes.

---

## USN_REASON_NAMED_DATA_OVERWRITE

Meaning:

An Alternate Data Stream (ADS) was modified.

---

## USN_REASON_NAMED_DATA_EXTEND

Meaning:

An Alternate Data Stream (ADS) increased in size.

---

# Why SOC Analysts Like USN Journal

USN Journal helps reveal:

- File creation
- File modification
- File rename activity
- File deletion
- Suspicious file behavior

Even when files are no longer present.

---

# Example Investigation

Timeline:

10:00 AM → payload.exe created

10:02 AM → payload.exe modified

10:03 AM → payload.exe renamed

10:05 AM → payload.exe deleted

USN Journal can show this sequence.

---

# What is $I30?

$I30 is an NTFS directory index attribute.

It stores information about files and folders inside a directory.

Think of it as:

Directory inventory list

---

# Information Stored in $I30

- File names
- Folder names
- File attributes
- File size
- MACB timestamps
- Parent directory information

---

# Why is $I30 Important?

Even if a file is deleted, traces of its metadata may still remain.

This makes $I30 valuable during investigations.

---

# Slack Space in $I30

## What is Slack Space?

When files are:

- Deleted
- Renamed
- Moved

Their old index entries may remain in unused space.

This unused area is called Slack Space.

---

## SOC Value

Slack Space may reveal:

- Deleted files
- Old file names
- Previous directory contents

---

# Forensic Value of $I30

## Deleted Files

May reveal files that existed previously.

---

## Renamed Files

Can help identify file name changes.

---

## Moved Files

Can provide clues about file movement.

---

## Hidden Files

File attributes may reveal hidden content.

---

# Important File Attributes

Common attributes include:

- Read Only
- Hidden
- System
- Directory
- Archive
- Compressed
- Encrypted

---

# Difference Between $LogFile and USN Journal

| Feature | $LogFile | USN Journal |
|----------|----------|-------------|
| Purpose | NTFS transaction tracking | File activity tracking |
| Focus | Metadata changes | File operations |
| Useful For | Recovery and metadata analysis | Timeline analysis |
| Tracks | Internal NTFS actions | User and file activity |

---

# Difference Between USN Journal and $I30

| Feature | USN Journal | $I30 |
|----------|-------------|------|
| Scope | Entire volume | Single directory |
| Tracks | File activity | Directory entries |
| Useful For | Timeline creation | Folder investigations |
| Shows | Create, modify, delete, rename | Existing and deleted directory records |

---

# Quick Revision

- NTFS Journaling records file system changes.
- $LogFile stores NTFS transaction records.
- USN Journal stores file activity history.
- $J contains actual USN records.
- $Max controls journal size.
- USN Journal helps build attack timelines.
- $I30 stores directory index information.
- Slack Space may contain deleted file traces.
- $I30 can reveal deleted, moved, or renamed files.
- SOC analysts use these artifacts to reconstruct attacker activity.

# Exam Summary

$LogFile = NTFS transaction journal

USN Journal = File activity history

$J = Actual USN records

$I30 = Directory index

Slack Space = Leftover deleted entries

Main Goal = Reconstruct attacker actions and build a timeline
