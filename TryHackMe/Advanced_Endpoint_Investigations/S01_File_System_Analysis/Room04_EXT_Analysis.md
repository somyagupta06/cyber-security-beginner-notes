# EXT4 File System Analysis

## Why Do We Analyze a File System?

In digital forensics, we analyze a file system to understand:

- What files existed
- Who created them
- When they were modified
- When they were accessed
- Whether they were deleted
- Whether an attacker tried to hide evidence

EXT4 is the default file system used in many Linux systems.

---

# Evolution of EXT File Systems

| File System | Important Feature |
|------------|------------------|
| EXT | First Linux file system |
| EXT2 | Improved version of EXT |
| EXT3 | Added Journaling |
| EXT4 | Better performance, larger size support, journaling improvements |

### Quick Comparison

| Feature | EXT2 | EXT3 | EXT4 |
|----------|-------|-------|-------|
| Journaling | No | Yes | Yes |
| Max File Size | 2 TiB | 2 TiB | 16 TiB |
| Max Volume Size | 32 TiB | 32 TiB | 1 EiB |

---

# Main Components of EXT4

Think of EXT4 as a library.

### Superblock = Library Information Desk

Stores information about the entire file system.

Examples:

- Total blocks
- Total inodes
- Block size
- Free space
- File system status

Without the superblock, understanding the file system becomes difficult.

---

### Inode = Student Record Card

Every file gets an inode.

An inode stores:

- Owner
- Permissions
- File size
- Timestamps
- Data block locations

Important:

File name is NOT stored inside the inode.

---

### Data Blocks = Actual Book Pages

These blocks store the real file content.

Example:

A text file containing notes is stored inside data blocks.

---

### Directory = Name List

A directory connects:

File Name → Inode Number

Example:

test.txt → Inode 11

---

### Bitmap = Attendance Sheet

Tracks which resources are used and free.

Examples:

- Used blocks
- Free blocks
- Used inodes
- Free inodes

---

# How a File is Created

When a file is created:

1. A free inode is allocated.
2. Data blocks are allocated.
3. File content is written into blocks.
4. File name is linked to the inode.
5. Bitmaps are updated.
6. Superblock information is updated.

---

# Block Groups

The disk is divided into groups called Block Groups.

Purpose:

- Better organization
- Faster access
- Better performance

Each block group contains:

- Inodes
- Data blocks
- Bitmaps

---

# Superblock Investigation

The Superblock contains important forensic information:

- Block size
- Number of inodes
- Number of blocks
- File system creation time
- Free space information

Investigators often check the superblock first to understand the file system structure.

---

# Block Size

EXT4 stores data in blocks.

Common block sizes:

- 1024 bytes
- 2048 bytes
- 4096 bytes

In this lab:

Block Size = 4096 bytes

Knowing the block size helps during file recovery.

---

# Understanding Inodes

Every file has a unique inode number.

Example:

test_file.txt

may have:

Inode = 11

The inode contains:

- Permissions
- Owner
- Timestamps
- File size
- Block locations

Forensic investigators often track files using inode numbers because file names can change.

---

# Useful Timestamps

### atime (Access Time)

Last time the file was read.

Example:

Opening a file updates atime.

---

### mtime (Modification Time)

Last time file content changed.

Example:

Adding text to a file updates mtime.

---

### ctime (Change Time)

Last time file metadata changed.

Examples:

- Permission changes
- Ownership changes

---

### crtime (Creation Time)

Original file creation time.

Available in EXT4.

Very useful during investigations.

---

# Why Timestamps Matter

Timestamps help investigators build timelines.

Questions answered:

- When was the file created?
- When was it modified?
- When was it accessed?
- Did an attacker modify it?

---

# Example Investigation

A suspicious file shows:

Birth Time = 2025

Modify Time = 2016

This is impossible because a file cannot be modified before it exists.

This indicates timestamp manipulation.

---

# Timestomping

Timestomping is an anti-forensic technique.

Goal:

Make a malicious file look old and legitimate.

Attackers change:

- Access time
- Modification time

to hide their activity.

---

# How Investigators Detect Timestomping

Investigators compare:

- atime
- mtime
- ctime
- crtime

If timestamps do not make sense, the file becomes suspicious.

Example:

Birth Time = 2025

Modify Time = 2016

This strongly suggests timestamp tampering.

---

# Why EXT4 is Useful for DFIR

EXT4 provides valuable evidence:

- File metadata
- Timestamps
- Permissions
- Ownership
- Deleted file traces
- Journaling information

This helps investigators reconstruct attacker activity.

---

# SOC Analyst Key Points

- Superblock describes the entire file system.
- Inodes store file metadata.
- File names are linked to inodes through directories.
- Data blocks store actual content.
- Bitmaps track free and used resources.
- EXT4 supports journaling.
- Timestamps are critical for timeline analysis.
- Compare atime, mtime, ctime, and crtime during investigations.
- Timestomping is a common anti-forensic technique.
- Inode numbers help track files even if names change.

---

# Quick Revision

Superblock → Information about the whole file system

Inode → Metadata of a file

Directory → File name to inode mapping

Data Block → Actual file content

Bitmap → Tracks used and free resources

atime → Last access

mtime → Last content modification

ctime → Last metadata change

crtime → File creation time

Timestomping → Fake timestamps used to hide attacker activity


---

# EXT4 Forensic Artifacts and File Recovery

## Why Are Inodes Important?

Every file in EXT4 has an inode.

Think of an inode as a file's identity card.

An inode stores:

* Owner
* Permissions
* File size
* Timestamps
* Data block locations

Important:

The file name is NOT stored inside the inode.

The directory only connects:

File Name → Inode Number

---

# Example

File:

test_file2.txt

Information:

* Inode = 11
* Owner = root
* Size = 9 bytes

Even if the file name changes, the inode remains the same.

This helps investigators track files during an investigation.

---

# Using stat

The stat command provides important forensic information.

Example findings:

* File size
* Inode number
* Access time
* Modification time
* Change time
* Creation time

Example:

Inode = 11

This means the file is internally identified as inode 11 by the file system.

---

# Understanding File Timestamps

## atime (Access Time)

Shows when the file was last read.

Example:

Opening a file updates atime.

---

## mtime (Modification Time)

Shows when file content was modified.

Example:

Adding text to a file updates mtime.

---

## ctime (Change Time)

Shows when metadata changed.

Examples:

* Permission changes
* Ownership changes

Changing permissions updates ctime.

---

## crtime (Creation Time)

Shows when the file was originally created.

Available in EXT4.

Very useful for timeline analysis.

---

# Investigating Inodes with debugfs

Investigators can directly inspect inode information from disk.

This provides:

* Permissions
* Owner
* Timestamps
* Block locations
* Checksums

This information comes directly from the file system structures.

---

# Permission Change Example

Original permission:

0777

After modification:

0755

Observation:

* ctime changed
* mtime stayed the same

Reason:

Only metadata changed.

The file content did not change.

---

# Why ctime Is Important

Attackers often modify permissions.

Examples:

* Making malware executable
* Hiding files
* Changing ownership

These actions update ctime.

Forensic analysts should always review ctime.

---

# EXTENTS

An extent points to the location where file data is stored.

Example:

EXTENTS:
(0):24577

Meaning:

The file content is stored in block 24577.

The inode knows where the actual data is located.

---

# Deleted File Recovery

Deleting a file does not immediately erase data.

Usually:

1. The file name reference is removed.
2. The inode becomes available.
3. Data blocks remain until overwritten.

Because of this, deleted files can often be recovered.

---

# Recovery Process

Suppose investigators know that a deleted file contains:

AAAAAAAA

First, search the disk for that content.

The search returns:

Offset = 100671488

This tells us where the content exists on disk.

---

# Converting Offset to Block Number

Block Size = 4096 bytes

Calculation:

100671488 ÷ 4096

Result:

24578

This means the data exists in block 24578.

---

# Extracting the Block

Investigators copy block 24578 from the disk.

The recovered block contains:

AAAAAAAA
BBBBBBBB
ZZZZZZZZ

The deleted file is successfully recovered.

---

# Why Recovery Works

Deleting a file usually removes references.

The actual data often remains on disk.

Until new data overwrites those blocks, recovery is possible.

---

# SOC Analyst Perspective

During investigations:

Look for:

* Suspicious inode metadata
* Recent permission changes
* Unusual ownership changes
* Timeline inconsistencies
* Deleted files
* Recoverable data blocks

---

# Quick Revision

Inode = File metadata

Directory = File name to inode mapping

atime = Last read

mtime = Last content modification

ctime = Last metadata change

crtime = File creation time

Extent = Location of file data blocks

Deleted file ≠ Destroyed file

Deleted files can often be recovered if data blocks are not overwritten.
:::

Ye version DFIR practicals aur viva dono ke liye enough hai. Isme unnecessary kernel structure hata diya aur sirf investigation wali cheezein rakhi hain jo SOC analyst ko yaad honi chahiye.

---

# EXT4 Timestamp Analysis and Autopsy

# Why Are Timestamps Important?

Timestamps help investigators understand:

* When a file was created
* When it was modified
* When it was accessed
* When it was deleted

They help build a timeline of events during an investigation.

---

# Important EXT4 Timestamps

## atime (Access Time)

Shows the last time a file was read.

Example:

Opening a file updates atime.

---

## mtime (Modification Time)

Shows the last time file content changed.

Example:

Adding new text updates mtime.

---

## ctime (Change Time)

Shows the last time metadata changed.

Examples:

* Permission changes
* Ownership changes

---

## dtime (Deletion Time)

Shows when a file was deleted.

Useful during deleted file investigations.

---

## crtime (Creation Time)

Shows when a file was originally created.

Available in EXT4.

Very useful for forensic timeline analysis.

---

# Normal File Example

normal_file.txt

Results:

* atime = 2025
* mtime = 2025
* ctime = 2025
* crtime = 2025

Observation:

Everything looks normal.

The timestamps match each other.

---

# What Is Timestomping?

Timestomping is an anti-forensic technique.

Attackers modify timestamps to make a malicious file appear old and legitimate.

Goal:

Hide malicious activity from investigators.

---

# Suspicious File Example

timestomped_file.txt

Results:

* atime = 2016
* mtime = 2016
* ctime = 2025
* crtime = 2025

Observation:

The file claims it was modified in 2016.

However, it was actually created in 2025.

This is impossible.

This indicates timestamp manipulation.

---

# How Investigators Detect Timestomping

Investigators compare:

* atime
* mtime
* ctime
* crtime

Questions:

* Do the timestamps make sense?
* Does creation time match modification time?
* Is there a large gap between timestamps?

If not, the file becomes suspicious.

---

# Important Investigation Rule

Never trust only the output of:

ls -l

Reason:

It mainly shows modification time.

Attackers can manipulate this value.

Always verify using:

stat

because it shows all important timestamps.

---

# Finding Suspicious Files

Investigators can search using ctime.

Reason:

Attackers often change mtime and atime.

However, changing metadata usually updates ctime.

Therefore, ctime often reveals recent activity.

---

# Why Timestomping Fails

Attackers can modify some timestamps.

However:

* ctime often exposes changes
* crtime reveals original creation time

This creates inconsistencies that investigators can detect.

---

# Autopsy

Autopsy is a digital forensic analysis platform.

It provides a graphical interface for investigations.

Instead of manually checking files, investigators can use Autopsy to analyze evidence faster.

---

# Why Use Autopsy?

Autopsy helps investigators:

* View files
* View metadata
* Recover deleted files
* Analyze timestamps
* Create timelines
* Search artifacts

---

# Creating a Case

Basic workflow:

1. Create a new case
2. Select a case directory
3. Create a host
4. Add a disk image
5. Start analysis

---

# Data Source

In this lab:

Data Source = ext4_case.img

Autopsy loads the image and begins processing forensic artifacts.

---

# Examining Files

Autopsy allows investigators to browse files directly.

Example:

* normal_file.txt
* timestomped_file.txt

The timestamps can be viewed from the interface.

This makes anomaly detection easier.

---

# Deleted File Detection

One useful Autopsy feature is deleted file recovery.

Autopsy can identify:

* Deleted files
* Deleted directories
* Recoverable artifacts

This helps investigators find evidence attackers attempted to remove.

---

# Why Autopsy Is Useful

Manual tools are powerful:

* debugfs
* stat
* extundelete

However, Autopsy provides:

* Easier navigation
* Faster analysis
* Better visualization
* Timeline generation
* Centralized investigation

---

# SOC Analyst Key Points

* Timestamps help build attack timelines.
* atime = Last read.
* mtime = Last content modification.
* ctime = Last metadata change.
* dtime = Deletion time.
* crtime = Creation time.
* Timestomping is a common anti-forensic technique.
* Compare all timestamps when investigating.
* Never rely only on ls output.
* Use stat for detailed timestamp analysis.
* Autopsy helps analyze metadata and recover deleted files.
* Autopsy can detect deleted files and suspicious timestamps.

---

# Quick Revision

atime → Last access

mtime → Last content modification

ctime → Last metadata change

dtime → Deletion time

crtime → Creation time

Timestomping → Fake timestamps used to hide attacker activity

stat → Shows complete timestamp information

Autopsy → GUI forensic tool for file system analysis and deleted file recovery
:::

Ye wala practical 3 ke notes jaisa hai — short, revision-friendly, SOC-focused aur exam se pehle 5 minute me revise kiya ja sakta hai.
